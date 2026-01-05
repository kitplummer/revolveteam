---
layout: post
title: "The 20-Platform Wall: Why Human-Machine Teams Need a New Coordination Layer"
date: 2026-01-05
excerpt: "Every program building autonomous systems hits the same wall around 20 platforms. This isn't a training problem — it's an architecture problem our current standards weren't designed to solve."
---

# The 20-Platform Wall: Why Human-Machine Teams Need a New Coordination Layer

*By Kit Plummer, (r)evolve*

---

Every program building autonomous systems hits the same wall. Somewhere around 20 platforms, things fall apart. Operators get overwhelmed. Networks get saturated. Coordination breaks down.

This isn't a training problem or a UI problem. It's an architecture problem — and our current standards weren't designed to solve it.

## The Math Nobody Talks About

In peer-to-peer architectures where every node might communicate with every other node, connection complexity grows with the square of node count. Ten nodes means 45 possible connections. Twenty nodes means 190. A hundred nodes means 4,950.

This isn't just networking overhead. It's an information management problem. If every platform needs awareness of every other platform's state, cognitive and computational load scales quadratically. The [Air Force Research Laboratory](https://www.afrl.af.mil/) and [NATO Human Factors panels](https://www.sto.nato.int/Pages/technical-team.aspx?k=(*)&s=Search%20HFM%20Activities) have validated this limit empirically — operators max out around 10-20 platforms with current systems.

## Hierarchy Isn't Bureaucracy — It's Architecture

Here's the thing: military organizations solved this problem centuries ago. A squad leader doesn't track every vehicle in the battalion. They know their squad and get relevant information from adjacent and higher echelons. Information flows up aggregated, decisions flow down decomposed.

This structure reduces O(n²) complexity to O(n log n). It's not bureaucratic overhead. It's communication optimization.

Yet our technical standards ignore this. [STANAG 4586](https://en.wikipedia.org/wiki/STANAG_4586) defines how to control individual UAVs. [JAUS](https://en.wikipedia.org/wiki/JAUS) standardizes component interfaces. [DDS](https://www.omg.org/spec/DDS/) moves messages between nodes. All assume flat topologies where every node is equivalent.

There's no mechanism for aggregation. No representation of echelon relationships. No way to express "this platoon has these combined capabilities" versus listing every platform individually.

## The Transport Problem Compounds It

[DDS](https://www.dds-foundation.org/) — the middleware underlying [ROS2](https://docs.ros.org/en/rolling/index.html) and referenced in DoD's [FACE standard](https://www.opengroup.org/face) — was designed for reliable, high-bandwidth networks. Its discovery protocol assumes multicast works. Its reliability model assumes retransmission is cheap.

On a 9600 baud tactical radio with 40% packet loss? DDS falls over. The ROS2 community [is actively decoupling from DDS](https://discourse.ros.org/t/ros-2-middleware-change-proposal/29313) precisely because it doesn't fit constrained environments.

Meanwhile, [STANAG 4817](https://www.janes.com/osint-insights/defence-and-national-security-analysis/stanag4817-nato-maritime-unmanned-systems-jigsaw) — NATO's emerging multi-domain control standard — reportedly addresses some coordination concerns. But the schemas aren't publicly available. There's no reference implementation. And nothing suggests it solves the fundamental scaling problem.

## What's Actually Missing

We have standards for platform control. We have middleware for moving bytes. We have frameworks for building robots. What we don't have is a **coordination layer** — common data structures for:

- **Team composition** — representing humans, machines, and AI agents as team members
- **Capability aggregation** — rolling up squad capabilities to platoon to company
- **Task allocation** — distributing work across heterogeneous teams
- **Shared state under stress** — synchronizing when networks partition

And critically, this layer needs to be designed for the real world: intermittent connectivity, bandwidth scarcity, and the hierarchical information flow that military operations actually require.

## The Gap

The [MITRE Human-Machine Teaming guide](https://www.mitre.org/sites/default/files/2021-11/prs-17-4208-human-machine-teaming-systems-engineering-guide.pdf) gives us design principles. NATO exercises like [REPMUS](https://www.yourdefensenews.com/repmus-2023-nato-tests-unmanned-maritime-systems-in-portugal/) test integration concepts. Research programs explore the art of the possible.

But nobody has published an **open, formally specified data framework** for human-machine-AI team coordination at scale. Every program builds its own. Coalition interoperability remains a dream. And we keep hitting the 20-platform wall.

---

At [(r)evolve](https://revolveteam.com), we're working on this problem — building on coordination concepts we first explored under [DIU's Common Operational Database (COD)](https://www.diu.mil/) program. We think the solution requires conflict-free data structures, native hierarchy support, and schemas designed for tactical networks.

More to share soon. If you're working on similar challenges, [we'd like to hear from you](mailto:kit@revolveteam.com).

---

*Kit Plummer is the founder of (r)evolve.*
