---
layout: post
title: "Stop Moving Data, Start Moving Decisions"
date: 2025-12-26
excerpt: "The IoT-to-cloud model breaks in contested environments. The fix: move decision-making to where data lives, not the other way around."
---

# Stop Moving Data, Start Moving Decisions

The IoT revolution made a promise: connect everything, send it all to the cloud, and intelligence will emerge. Sensors on every machine, every vehicle, every soldier. Terabytes flowing to data centers where AI would make sense of it all.

That promise is breaking.

Not because the cloud isn't powerful—it is. Not because the AI isn't capable—it's extraordinary. The promise is breaking because the architecture assumes something that isn't true: that you can always move data to where the compute lives.

In contested environments, you can't.

## The Bandwidth Tax

Every byte you send costs something. On a fiber connection in a data center, that cost is negligible. On a tactical radio network, it's ruinous.

Consider a simple scenario: ten autonomous platforms sharing situational awareness. If each platform sends its full sensor state to every other platform, you have 90 data flows (n² - n). Add a few cameras, some LIDAR, maybe radar returns, and you're pushing gigabits across links measured in kilobits.

The math doesn't work. It never did. We just pretended it would because Moore's Law kept bailing us out on compute, and we assumed bandwidth would follow.

It didn't. Physics doesn't care about your roadmap.

Tactical networks—the mesh radios, satellite links, and contested spectrum that actual operations depend on—remain stubbornly constrained. And they degrade exactly when you need them most: when someone is actively trying to deny them.

## The Latency Problem

Even when bandwidth exists, latency kills.

Round-trip to a cloud data center: 50-200 milliseconds on a good day. Longer through a satellite hop. Much longer through a congested tactical network under electronic attack.

For a drone making obstacle avoidance decisions at 60 mph, 200 milliseconds is 17 feet of travel. For a coordinated maneuver between platforms, it's an eternity. For a human operator trying to maintain situational awareness, it's the difference between seeing what's happening and seeing what happened.

The IoT model assumes latency is a temporary engineering problem. For edge operations, it's a permanent physical constraint.

## Inverting the Model

Here's the shift: instead of moving data to where decisions happen, move decision-making to where data lives.

This isn't a fringe position anymore. The team at [Ditto](https://ditto.live) has been evangelizing "[cloud optional](https://interface.media/blog/2024/07/02/peer-to-peer-sync-making-the-cloud-optional-again/)" architecture for years—building peer-to-peer sync that lets devices share data directly without round-tripping through servers. They've proven the model works in retail, healthcare, and defense. The insight is spreading.

But "cloud optional" is just the first step. The deeper move isn't just about *where* data lives—it's about what you're moving in the first place.

This isn't a new idea. It's how biological systems work. Your hand doesn't send raw nerve signals to your brain and wait for motor commands. The spinal cord handles reflexes locally. The brain gets summaries—"hand touched something hot"—and makes higher-order decisions—"don't touch the stove again."

The nervous system is a hierarchical decision architecture. Raw data stays local. Abstractions flow up. Intent flows down.

Apply this to autonomous systems:

**Local decisions stay local.** Obstacle avoidance, sensor fusion, immediate responses—these happen on the platform, with the data, in microseconds. No round-trip required.

**Relevant abstractions propagate.** Instead of sending video frames, send "vehicle detected at grid reference." Instead of raw telemetry, send "capability degraded, estimate 30 minutes to repair." The information that matters for coordination is orders of magnitude smaller than the raw data that produced it.

**Intent distributes.** Higher echelons don't micromanage. They set objectives, constraints, boundaries. "Maintain surveillance of this area." "Avoid engagement unless threatened." "Coordinate with adjacent units." Platforms interpret intent locally, using local data, making local decisions.

This is the control plane / data plane separation that network engineers have understood for decades. The data plane handles packets at line rate, locally. The control plane handles routing decisions, tolerating latency because the decisions are less frequent and more abstract.

## What Changes

When you stop moving data and start moving decisions, several things shift:

**Bandwidth requirements collapse.** Sharing "I can provide ISR coverage for grid squares A1-A3 for the next 2 hours" costs bytes. Sharing the video feed that informed that assessment costs megabytes per second. The ratio can be 1000:1 or more.

**Resilience improves.** When the network degrades—and it will—local operations continue. Platforms have what they need to execute. They're not waiting for permission or data from somewhere else. When connectivity returns, they sync state and continue.

**Latency becomes tolerable.** Coordination decisions can handle hundreds of milliseconds of delay because they're not time-critical in the same way reflexive actions are. You've separated the fast path from the slow path.

**Humans stay in the loop.** When the system moves decisions rather than data, humans can actually engage at the decision level. They're not drowning in sensor feeds they can't possibly process. They're seeing actionable information at the right level of abstraction for their role.

## The Hard Part

This architecture isn't free. It requires something the IoT model avoided: defining what decisions matter at each level.

When you pump raw data to the cloud, you defer that question. "We'll figure out what's important later, with ML, in the cloud." It's intellectually lazy but operationally simple.

Moving decisions requires you to think hard about abstraction boundaries. What does a platform need to decide locally? What does a team leader need to know? What does a commander need to see? These aren't just technical questions—they're questions about doctrine, authority, and trust.

They're also the questions that matter. If you can't answer them, no amount of bandwidth will save you. If you can, you've built something that works when the network doesn't.

## The Point

The IoT-to-cloud model works beautifully in environments where connectivity is assumed. Smart homes. Fleet management. Industrial monitoring. Anywhere you have reliable, high-bandwidth, low-latency connections to capable infrastructure.

Contested environments aren't that. Tactical edge isn't that. Disaster response isn't that. Space operations aren't that.

For those domains, the architecture has to change. Stop assuming you can move data to compute. Start moving compute—and decisions—to data.

The cloud is a tool, not a destination. Use it when you can. Don't depend on it when you can't.

---

*Kit Plummer is the founder of (r)evolve, building coordination protocols for human-machine-AI teams. More at [revolveteam.com](https://revolveteam.com).*
