---
layout: post
title: "NATO's Unmanned Systems Standard: Right Problem, Wrong Model?"
date: 2026-01-05
excerpt: "STANAG 4817 aims to coordinate unmanned systems across domains. But its data-centric approach and closed development process work against the interoperability it's trying to achieve."
---

# NATO's Unmanned Systems Standard: Right Problem, Wrong Model?

*By Kit Plummer, (r)evolve*

---

NATO is building a standard for coordinating unmanned systems across domains. It's called [STANAG 4817](https://www.janes.com/osint-insights/defence-and-national-security-analysis/stanag4817-nato-maritime-unmanned-systems-jigsaw), and it's being positioned as the backbone of future multi-domain operations — enabling drones, UUVs, and USVs from different nations to share data and operate together.

On paper, it sounds essential. In practice, there are problems — both with the approach and with how it's being developed.

## What 4817 Is Trying to Do

STANAG 4817 builds on [STANAG 4586](https://en.wikipedia.org/wiki/STANAG_4586), the existing NATO standard for UAV control interfaces. Where 4586 defined how an operator controls a single unmanned aircraft, 4817 extends to multi-domain: air, surface, and underwater systems coordinated from unified control stations.

The standard incorporates the [Collaborative Autonomy Tasking Layer (CATL)](https://ieeexplore.ieee.org/document/10244669), a message format developed by [NATO's SCI-343 Research Task Group](https://www.sto.nato.int/) for sharing task and status information between autonomous systems. CATL has been tested at exercises like [REPMUS](https://www.yourdefensenews.com/repmus-2023-nato-tests-unmanned-maritime-systems-in-portugal/) and [Dynamic Messenger](https://www.act.nato.int/article/dynamic-messenger-22-an-opex-exercise-designed-for-the-future/), demonstrating interoperability between platforms from multiple nations.

The stated goals are reasonable:
- Common data formats for unmanned systems across domains
- Interoperability between different manufacturers and nations
- Unified situational awareness through data fusion

So what's the problem?

## The Transparency Problem

Try to find the STANAG 4817 specification. Or the CATL message schema. Or documentation on how the standard is being developed.

We did. We're a defense contractor. We couldn't find it either.

The [Custodian Support Team](https://fourdrobotics.com/_sys/june-3-6-nato-stanag-4817-custodial-support-team-meeting-lockheed-martin-canada-cdl-systems-calgary/) meets periodically — hosted by organizations like Lockheed Martin Canada and NATO Centers of Excellence. Participants include representatives from NATO and Partnership for Peace nations. The work builds on recommendations from NATO Industrial Advisory Group studies (NIAG SG-157 and SG-202).

We know CATL exists because researchers reference it in [published papers](https://ieeexplore.ieee.org/document/10244669). We know exercises like REPMUS test it. We know it defines "four message types for task administration and data synchronization." But the actual schema? The message definitions? The protocol specification? Not publicly available — and apparently not available through normal defense industry channels either.

Compare this to how other standards are developed:
- **IETF RFCs**: Public drafts, open mailing lists, anyone can comment
- **W3C**: Public working drafts, community feedback periods
- **IEEE**: Published specifications, academic participation
- Even **SAE/JAUS**: While the final documents cost money, there are [open-source implementations](https://openjaus.com/) and public working groups

If the goal is interoperability across nations and vendors, developing the standard behind closed doors is counterproductive. The companies that need to implement it can't see it. The researchers who could improve it can't review it. The broader defense industrial base is locked out until... when, exactly?

## The Bigger Problem: Data-Centric vs. Decision-Centric

Even setting aside transparency concerns, there's a more fundamental issue with the approach.

The 4817/CATL model is built on a **data-centric** mental model:
- Move data from sensors and platforms to control stations
- Fuse that data into a "comprehensive operational picture"
- Give commanders a "unified view" of the battlespace

This made sense when the constraint was information scarcity. Historically, commanders didn't have enough data. The solution was to collect more and aggregate it centrally.

But that's not the problem anymore. The problem now is **information overload**. Commanders are drowning in data. Adding more sensors, more platforms, and more data fusion doesn't help — it makes cognitive load worse.

AI-enhanced decision support requires the opposite approach — **decision-centric** rather than data-centric:

| Data-Centric (4817 model) | Decision-Centric (what's needed) |
|---------------------------|----------------------------------|
| Move data to where decisions happen | Move decisions to where data lives |
| Comprehensive operational picture | Filtered, contextual information |
| More data = better awareness | Right data = better decisions |
| Centralized fusion | Distributed processing |
| Human processes the picture | AI reduces cognitive load |

The goal shouldn't be showing commanders everything. It should be showing them **only what they need to decide**, with the supporting context, at the moment they need it.

This isn't just a UI problem. It's an architectural problem. A standard designed around "unified view" and "data fusion" bakes in assumptions that work against effective AI integration.

## What Would Actually Help

For human-machine-AI teams to operate effectively at scale, we need coordination frameworks that:

**Support hierarchical information flow.** Military organizations aggregate information as it flows up and decompose decisions as they flow down. Flat data-sharing models don't match how operations actually work.

**Enable distributed processing.** AI and autonomy should filter, prioritize, and pre-process at the edge — not just add more streams to a central fusion engine.

**Reduce cognitive load.** The measure of success isn't how much data reaches the commander. It's how little data reaches the commander while still enabling good decisions.

**Work in contested networks.** Tactical communications are bandwidth-limited, high-latency, and unreliable. Standards that assume enterprise connectivity will fail in the field.

**Are actually open.** If interoperability is the goal, the specifications need to be public. Implementations need to be testable. Participation needs to be accessible beyond the current defense insider club.

## Where We're Focused

At [(r)evolve](https://revolveteam.com), we're working on coordination infrastructure for human-machine-AI teams — building on concepts we first explored under [DIU's Common Operational Database (COD)](https://www.diu.mil/) program.

We think the solution requires a different starting point: decision-centric rather than data-centric, hierarchical rather than flat, designed for contested networks rather than enterprise connectivity. And we think it needs to be developed in the open.

More to share soon. If you're working on similar challenges — or frustrated by the same gaps — [we'd like to hear from you](mailto:kit@revolveteam.com).

---

*Kit Plummer is the founder of (r)evolve.*
