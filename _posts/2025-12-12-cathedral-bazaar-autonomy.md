---
layout: post
title: "The Cathedral and the Bazaar: Lessons for Autonomous Systems"
date: 2025-12-12
excerpt: "Eric Raymond's seminal essay on open-source development offers a blueprint for building autonomous systems and human-machine teams—favoring the bazaar's distributed collaboration over the cathedral's closed design."
---

In 1997, Eric Raymond published "[The Cathedral and the Bazaar](http://www.catb.org/~esr/writings/cathedral-bazaar/)"—an essay that would reshape how we think about software development. His central argument: the "bazaar" style of development, with its frequent releases, continuous user feedback, and decentralized community of co-developers, produces superior software compared to the "cathedral" model of tightly controlled, isolated development.

Nearly three decades later, these principles have profound implications for autonomous systems and human-machine teaming. And they reveal an uncomfortable truth: the autonomy industry is stuck in cathedral mode while the rest of the software world has moved on.

## The Current State: Cathedrals Everywhere

Look at the drone and robotics landscape today. It's a collection of proprietary silos—closed hardware, closed software, closed ecosystems. Every vendor builds their own cathedral, and interoperability is largely a fiction we tell ourselves at trade shows.

The standards we do have are artifacts of a different era. [STANAG 4586](https://nso.nato.int/nso/nsdd/main/standards?search=4586), NATO's standard for UAV interoperability, dates to 2002. It was designed for a world of large, expensive platforms with human operators in ground control stations—not swarms of autonomous agents coordinating in real-time. [Cursor-on-Target](https://www.mitre.org/news-insights/publication/cursor-target-message-router-users-guide) (CoT), MITRE's protocol for sharing tactical information, is similarly long in the tooth. These aren't foundations for the future; they're legacy infrastructure we're forced to route around.

The result? Every integration is a custom engineering effort. Every new platform requires bespoke adapters. "Interoperability" means spending months building point-to-point bridges that break when either side updates. We've created a world where combining a drone from Vendor A with sensors from Vendor B and a command system from Vendor C requires a small army of integration engineers and a large budget for middleware.

This is cathedral thinking at its worst: each vendor optimizing for lock-in rather than capability, and the entire ecosystem paying the tax.

## The Platform Trap

The cathedral pattern isn't limited to legacy defense contractors. It's being replicated across every domain where autonomy is taking hold.

**Agriculture:** John Deere has built an impressive autonomous farming stack—and locked it down completely. Farmers who buy Deere equipment can't access their own data, can't repair their own machines, can't integrate third-party implements without Deere's blessing. The [right-to-repair movement](https://www.deere.com/en/our-company/right-to-repair/) exists largely because Deere turned tractors into cathedrals. Your farm, their platform.

**Autonomous vehicles:** Every major AV player—Waymo, Cruise, Tesla, Motional—has built a vertically integrated stack. Sensors, perception, planning, control: all proprietary, all closed. There's no "ONNX for autonomous driving" that lets you swap a better perception model into a competitor's vehicle. Each company is building its own cathedral, betting they'll be the one left standing.

**Warehouse robotics:** Amazon acquired Kiva Systems in 2012 and promptly pulled it from the market, keeping the technology for themselves. Competitors had to build from scratch. The warehouse autonomy space has since fragmented into dozens of incompatible systems, each vendor hoping to become the platform that locks in the next generation of logistics.

**Industrial automation:** Siemens, Rockwell, ABB—the incumbent automation giants—have spent decades building proprietary control systems with proprietary protocols. Integrating across vendors requires expensive middleware and specialized expertise. Sound familiar?

### The Defense Variant

In defense, this pattern takes its most acute form. Anduril's [Lattice](https://www.anduril.com/lattice/) represents the explicit strategy: become the operating system for defense autonomy, with every other vendor's hardware plugging into their mesh.

Give them credit—they recognized the interoperability problem and built technology to address it. But their solution is to *become* the platform. The pitch is seductive: one unified command-and-control layer, one interface, one vendor to call when things break. For acquisition officers exhausted by integration nightmares, it sounds like salvation.

But the end state is the same lock-in, just at a higher level of abstraction. Instead of being locked into a drone vendor, you're locked into a *platform* vendor. The middleware becomes the moat. When one company controls the coordination layer, they control the narrative. They decide which systems integrate smoothly and which face friction. Startups with better sensors, better algorithms, better hardware? They succeed or fail based on Lattice compatibility.

This is the cathedral model dressed in software-company clothing. Move fast, ship product, but maintain control. The interoperability offered is interoperability *on their terms*, through their APIs, mediated by their platform.

Whether it's tractors, taxis, warehouses, or weapons systems—the pattern is the same. And so is the outcome: lock-in, stagnation, and ecosystems that serve vendors rather than users.

## Markets and Procurement: Why Cathedrals Win

These platform traps don't exist in a vacuum. They're enabled by how we buy technology—whether through enterprise procurement, venture-backed growth, or government acquisition.

**The enterprise buyer** wants "one throat to choke." Integrated platforms reduce vendor management overhead, simplify support contracts, and provide someone to blame when things go wrong. Best-of-breed architectures require more sophistication to evaluate, purchase, and operate. Cathedrals are easier to buy.

**The venture model** rewards platform plays. Investors want moats. They want network effects and switching costs that compound over time. An open, interoperable component is a commodity; a proprietary platform is a defensible business. The incentives push toward lock-in.

**Government acquisition** takes these dynamics and amplifies them. The process rewards companies that can absorb compliance overhead, navigate byzantine procurement rules, and sustain years-long sales cycles. When a program office needs to show progress, buying a unified platform from a single vendor is the path of least resistance.

None of this fits neatly into a FAR-compliant contract vehicle. Open standards require coordination across stakeholders. Modular architectures require defining interfaces upfront and holding vendors to them. Bazaar-style development requires accepting that the best solution might come from a company that doesn't exist yet.

The result is a self-reinforcing cycle: vendors build cathedrals because that's what gets bought, and buyers keep buying cathedrals because that's what vendors build. Each iteration entrenches the pattern further.

Breaking the cycle requires changes at every level: procurement language that mandates open interfaces, contract structures that reward interoperability, enterprise architects willing to manage multi-vendor complexity, and investors who recognize that open ecosystems can win too.

The bazaar isn't just a development philosophy. It's a procurement philosophy. And until buying catches up with building, we'll keep erecting cathedrals by default.

## Two Models of Development

| Feature | Cathedral Model | Bazaar Model |
|---------|-----------------|--------------|
| **Development** | Centralized, top-down, small exclusive team | Decentralized, community-driven, widespread collaboration |
| **Releases** | Infrequent, with long periods between releases | "Release early, release often" |
| **Feedback** | Limited to the internal team | Continuous feedback from a large base of users/developers |
| **Key Principle** | Meticulous a priori design by experts | "Given enough eyeballs, all bugs are shallow" (Linus's Law) |

Raymond's "Linus's Law"—named for Linux creator Linus Torvalds—captures the bazaar's core insight: with enough contributors examining the code, problems that seem intractable to a small team become obvious to someone in the crowd.

## Why This Matters for Autonomy

The bazaar model offers distinct advantages for the complex and safety-critical field of autonomous systems:

**Improved Safety and Security.** A larger and more diverse group of contributors can identify and resolve potential safety flaws, biases, and security vulnerabilities much faster than a small, closed team. [Cisco's release of an open-source security framework](https://outshift.cisco.com/blog/agntcy-the-open-source-agentic-framework) for autonomous agent networks exemplifies this approach—inviting the community to stress-test security assumptions rather than hiding them behind closed doors.

**Faster Innovation.** The open model allows for rapid prototyping and an evolutionary approach to development. This accelerates the creation of better, more functional AI models and enables the emergence of new standards and protocols through collective experimentation rather than committee design.

**Enhanced Reliability.** Continuous testing and error correction by the community lead to more robust systems. When thousands of developers can probe edge cases, the system hardens faster than any internal QA process could achieve.

**Standardization and Interoperability.** Open-source foundations create neutral hubs for cross-industry standards. The [Open Source AI Definition](https://opensource.org/ai/open-source-ai-definition) from the Open Source Initiative and emerging agent interoperability efforts demonstrate how open collaboration enables different AI agents and systems to integrate seamlessly—critical for heterogeneous human-machine teams.

## A Model Worth Following: How AI Solved Interoperability

Here's the contrast that should embarrass the autonomy industry: the AI/ML community faced the same fragmentation problem—and solved it.

In 2017, Facebook and Microsoft launched [ONNX](https://onnx.ai/) (Open Neural Network Exchange). The problem was familiar: models trained in PyTorch couldn't run in TensorFlow. Every framework was a walled garden. Moving a model from research to production meant rewriting it, sometimes from scratch.

ONNX created an open, vendor-neutral interchange format. Train your model anywhere, deploy it anywhere. Today, ONNX is supported by virtually every major ML framework and hardware accelerator. A model built by a researcher using PyTorch can be optimized and deployed on edge hardware from NVIDIA, Intel, Qualcomm, or a dozen others—without modification.

The AI community chose the bazaar. They built an open standard through collaboration rather than committee, iterated rapidly based on real-world usage, and created an ecosystem where competition happens on quality and performance rather than lock-in.

Now imagine if drones and robots worked this way. A perception model trained on one platform, deployable on any. A planning algorithm developed by one team, integratable by all. Control interfaces that speak a common language. Hardware that competes on capability rather than captivity.

This isn't utopian thinking. It's what the software world already does. The autonomy industry just hasn't caught up.

## The ROS2 Exception—And Its Limits

To be fair, robotics has one genuine bazaar success story: [ROS2](https://docs.ros.org/en/rolling/index.html) (Robot Operating System 2).

ROS2 got a lot right. It's open-source, community-driven, and has become the de facto standard for robotics development. It provides a common framework for perception, planning, and control. It has a thriving ecosystem of packages. Companies that would never share proprietary code contribute to the commons because the rising tide lifts all boats.

For single-robot autonomy, ROS2 is the bazaar in action.

But ROS2 was designed for a different problem. It excels at making one robot work—coordinating sensors, actuators, and algorithms within a single system or a local cluster. What it doesn't address:

**Collaborative autonomy.** When dozens or hundreds of heterogeneous autonomous systems need to coordinate—sharing intent, negotiating tasks, maintaining coherent situational awareness—ROS2's architecture doesn't scale. It assumes a level of network reliability and latency that simply doesn't exist in contested, distributed operations.

**Global distribution.** ROS2's original communication model (DDS) works well in a lab or a local network. It wasn't built for systems scattered across continents, operating through intermittent satellite links, mesh networks, and degraded communications. The assumptions baked into DDS break down at scale. (Credit where due: the ROS2 community is actively addressing this. The move toward [Zenoh](https://zenoh.io/) as an alternative middleware—designed from the ground up for constrained networks, edge computing, and global distribution—is exactly the kind of bazaar-style course correction that makes open ecosystems work. Recognize the problem, build the solution, iterate.)

**Cross-domain teaming.** A future where ground robots, aerial drones, maritime vessels, and space assets collaborate seamlessly requires abstractions ROS2 doesn't provide. It's a robotics framework, not a coordination protocol for heterogeneous autonomous systems operating across domains.

ROS2 proved the bazaar model works for robotics. Now we need to apply the same philosophy to the harder problem: collaborative autonomy at global scale, across domains, under real-world constraints.

## Implications for Human-Machine Teaming

For human-machine teams, the bazaar approach emphasizes flexible and adaptable systems that can be easily modified and integrated with diverse human workflows and data sources.

**Flexible Integration.** Open-source AI models can be modified and optimized for specific operational needs, enabling better performance and superior decision-making within human-machine collaboration systems. Teams can adapt tools to their workflows rather than contorting workflows to fit tools.

**Focus on Human Oversight.** Successful AI-human collaboration requires maintaining human oversight at critical decision points. This is significantly easier when the underlying system's code and logic are transparent and open to inspection. The bazaar model makes "trust but verify" actually possible—you can verify.

**Collaborative Design.** The development process can mirror the desired operational outcome: a collaborative environment where both humans and machines (or different teams building them) work together effectively. Building in the open trains organizations for operating in the open.

## The Cathedral Still Has Its Place

This isn't to say closed development is always wrong. Some components—particularly those involving classified capabilities or zero-day vulnerabilities—may require cathedral-style protection. The art is knowing which parts of your system benefit from broad scrutiny and which require controlled access.

But for the core challenges of autonomous systems—reliability, safety, interoperability, and human-machine integration—the bazaar offers something the cathedral cannot: the collective intelligence of a community that has skin in the game.

Given enough eyeballs, all bugs are shallow. Given enough collaborators, even autonomous systems can be made trustworthy.

---

*Attribution: This post draws on Eric S. Raymond's "[The Cathedral and the Bazaar](http://www.catb.org/~esr/writings/cathedral-bazaar/)" (1997). Standards referenced include [STANAG 4586](https://nso.nato.int/nso/nsdd/main/standards?search=4586) (NATO UAV interoperability) and MITRE's [Cursor-on-Target](https://www.mitre.org/news-insights/publication/cursor-target-message-router-users-guide) protocol. Open-source projects discussed: [ONNX](https://onnx.ai/) (AI/ML interchange), [ROS2](https://docs.ros.org/en/rolling/index.html) (robotics framework), [Zenoh](https://zenoh.io/) (distributed middleware), [Cisco's Outshift](https://outshift.cisco.com/blog/agntcy-the-open-source-agentic-framework) (agentic security), and the [Open Source Initiative](https://opensource.org/ai/open-source-ai-definition) (AI definitions). Proprietary platform examples: [John Deere](https://www.deere.com/en/our-company/right-to-repair/) (agriculture), [Anduril Lattice](https://www.anduril.com/lattice/) (defense).*
