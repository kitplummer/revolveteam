---
layout: post
title: "Building a CI Pipeline for Radicle with goa"
date: 2026-01-10
excerpt: "How to set up self-hosted continuous integration for Radicle's peer-to-peer code collaboration stack using goa, a lightweight GitOps agent."
---

# Building a CI Pipeline for Radicle with goa

*By Kit Plummer, (r)evolve*

---

[Radicle](https://radicle.xyz) is a peer-to-peer code collaboration stack built on Git. Unlike GitHub or GitLab, Radicle is decentralized—your code lives on your machine and is replicated across a network of nodes. But this raises a question: how do you run CI/CD for a decentralized repository?

Enter **goa** (GitOps Agent), a lightweight tool I created about 5 years ago to assist with some MLOps work I was doing that watches repositories for changes and executes commands when updates are detected. With goa's Radicle support, you can build a self-hosted CI pipeline for your Radicle projects.  Thanks to Claude Code for helping review and refresh the original goa codebase quickly.

## Why goa for Radicle CI?

- **No external services required** - runs entirely on your local machine or server
- **Watches both pushes and patches** - trigger CI on new commits or when contributors submit patches (Radicle's equivalent of pull requests)
- **Simple setup** - single binary, no complex configuration
- **Environment variables** - access commit info, patch details, and more in your CI scripts

## Prerequisites

- A running [Radicle node](https://radicle.xyz/guides/user) (`rad` CLI installed and initialized)
- Your repository seeded to your node
- Rust toolchain (for installing goa) or install one of the binaries at [https://github.com/kitplummer/goa/releases](https://github.com/kitplummer/goa/releases).

## Installation

```bash
cargo install gitops-agent
```

This installs the `goa` binary.

## Basic Setup

### 1. Find Your Repository ID

Every Radicle repository has a unique identifier (RID). Find yours with:

```bash
rad inspect
```

You'll see output like:
```
rad:z3fF7wV6LXz915ND1nbHTfeY3Qcq7
```

### 2. Identify Your Seed Node

Your local node exposes an HTTP API. By default, it's available at `http://localhost:8080`. You can also use public seed nodes like `https://seed.radicle.garden`.

### 3. Start Watching

```bash
goa radicle \
  --seed-url http://localhost:8080 \
  --rid rad:z3fF7wV6LXz915ND1nbHTfeY3Qcq7 \
  --command './ci.sh' \
  --watch-patches \
  --delay 60
```

This tells goa to:
- Connect to your local Radicle node's HTTP API
- Watch the specified repository
- Run `./ci.sh` when changes are detected
- Check for both push and patch updates
- Poll every 60 seconds

## Writing Your CI Script

goa passes context about the trigger via environment variables. Here's a complete CI script:

```bash
#!/bin/bash
set -e

echo "=== Radicle CI ==="
echo "Repository: $GOA_RADICLE_RID"
echo "Trigger: $GOA_TRIGGER_TYPE"
echo "Commit: $GOA_COMMIT_OID"

# Clone from local Radicle storage
WORK_DIR="/tmp/ci-$(date +%s)"
STORAGE_PATH="$HOME/.radicle/storage/${GOA_RADICLE_RID#rad:}"

git clone "$STORAGE_PATH" "$WORK_DIR"
cd "$WORK_DIR"

# Checkout the specific commit
git checkout "$GOA_COMMIT_OID"

# If this is a patch, show details
if [ "$GOA_TRIGGER_TYPE" = "patch" ]; then
  echo "Patch ID: $GOA_PATCH_ID"
  echo "Title: $GOA_PATCH_TITLE"
  echo "State: $GOA_PATCH_STATE"
  echo "Base: $GOA_BASE_COMMIT"
fi

echo "=== Running Tests ==="

# Your build/test commands here
cargo build
cargo test

echo "=== CI Complete ==="

# Cleanup
cd /
rm -rf "$WORK_DIR"
```

Save this as `ci.sh` and make it executable:

```bash
chmod +x ci.sh
```

## Environment Variables Reference

goa provides these environment variables when triggered:

| Variable | Description | Example |
|----------|-------------|---------|
| `GOA_RADICLE_RID` | Repository ID | `rad:z3fF7wV6LXz915ND1nbHTfeY3Qcq7` |
| `GOA_RADICLE_URL` | Seed node URL | `http://localhost:8080` |
| `GOA_TRIGGER_TYPE` | `push` or `patch` | `patch` |
| `GOA_COMMIT_OID` | Commit SHA to test | `a1b2c3d4...` |
| `GOA_PATCH_ID` | Patch ID (if patch) | `abc123...` |
| `GOA_PATCH_TITLE` | Patch title (if patch) | `Fix memory leak` |
| `GOA_PATCH_STATE` | Patch state (if patch) | `open` |
| `GOA_BASE_COMMIT` | Base commit (if patch) | `def456...` |

## Running as a System Service

For production use, run goa as a systemd service:

```ini
# /etc/systemd/system/radicle-ci.service
[Unit]
Description=Radicle CI Agent
After=network.target radicle-node.service

[Service]
Type=simple
User=ci
WorkingDirectory=/home/ci/myproject
ExecStart=/usr/local/bin/goa radicle \
  --seed-url http://localhost:8080 \
  --rid rad:z3fF7wV6LXz915ND1nbHTfeY3Qcq7 \
  --command './ci.sh' \
  --watch-patches \
  --delay 60 \
  --timeout 600 \
  --verbosity 1
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Enable and start:

```bash
sudo systemctl enable radicle-ci
sudo systemctl start radicle-ci
```

View logs:

```bash
journalctl -u radicle-ci -f
```

## Advanced: Testing Patches Before Merge

One powerful use case is automatically testing patches (PRs) before they're merged. Here's an enhanced CI script that posts results back:

```bash
#!/bin/bash
set -e

WORK_DIR="/tmp/ci-$(date +%s)"
STORAGE_PATH="$HOME/.radicle/storage/${GOA_RADICLE_RID#rad:}"
RESULT_FILE="/tmp/ci-result-$$"

# Clone and checkout
git clone "$STORAGE_PATH" "$WORK_DIR"
cd "$WORK_DIR"
git checkout "$GOA_COMMIT_OID"

# Run tests, capture result
if cargo test 2>&1 | tee "$RESULT_FILE"; then
  STATUS="passed"
else
  STATUS="failed"
fi

# If this was a patch, we could comment on it
# (Radicle CLI support for this is evolving)
if [ "$GOA_TRIGGER_TYPE" = "patch" ]; then
  echo "Patch $GOA_PATCH_ID: tests $STATUS"
  # Future: rad patch comment $GOA_PATCH_ID "CI $STATUS"
fi

# Cleanup
rm -rf "$WORK_DIR" "$RESULT_FILE"

# Exit with appropriate code
[ "$STATUS" = "passed" ]
```

## Tips and Best Practices

1. **Use `--timeout`** to prevent runaway builds:
   ```bash
   goa radicle ... --timeout 600  # 10 minute limit
   ```

2. **Adjust `--delay`** based on your needs:
   - Development: 30-60 seconds
   - Production: 120-300 seconds

3. **Use `--verbosity 2`** for debugging:
   ```bash
   goa radicle ... --verbosity 2
   ```

4. **Isolate CI environments** - use containers or VMs for untrusted code:
   ```bash
   # In ci.sh
   docker run --rm -v "$WORK_DIR:/src" my-ci-image /src/run-tests.sh
   ```

5. **Monitor multiple repositories** by running multiple goa instances:
   ```bash
   goa radicle --rid rad:z111... --command './ci-project1.sh' &
   goa radicle --rid rad:z222... --command './ci-project2.sh' &
   ```

## Conclusion

With goa, you can bring CI/CD to the decentralized world of Radicle without relying on centralized services. Your code stays sovereign, your CI runs locally, and you maintain full control over your development workflow.

The combination of Radicle's peer-to-peer collaboration and goa's lightweight automation creates a powerful, self-hosted development environment that respects your autonomy while providing the conveniences of modern DevOps practices.

---

**Resources:**
- [goa on crates.io](https://crates.io/crates/gitops-agent)
- [goa on GitHub](https://github.com/kitplummer/goa)
- [Radicle Documentation](https://radicle.xyz/guides)
