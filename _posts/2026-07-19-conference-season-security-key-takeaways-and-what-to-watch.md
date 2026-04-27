---
layout: single
title: "Conference Season Security: Key Takeaways and What to Watch"
toc: true
excerpt: "Summer conference season — DEF CON, Black Hat, BSides, and dozens of regional events — surfaces emerging threats and techniques months before they appear in mainstream threat intelligence. This post covers how to get maximum value from conference season whether you attend in person or follow remotely, and highlights the themes worth watching in 2026."
header:
  overlay_image: /assets/images/post-bsideslv-2025.jpg
  teaser: /assets/images/post-bsideslv-2025.jpg
  overlay_filter: 0.5
---

Every August, the security industry makes a pilgrimage to Las Vegas for what's collectively known as Hacker Summer Camp — Black Hat, DEF CON, and BSides Las Vegas, plus an expanding ecosystem of vendor events and side meetings. The pace is exhausting, the cost is real, and yet the value of the week tends to outpace nearly any other industry event. The reasons are practical: the research presented is often months ahead of public disclosure, the hallway conversations surface threats and techniques you won't read about anywhere, and the collision of practitioners from every corner of the industry produces context you can't get sitting in your own SOC.

But the value of conference season isn't reserved for people who attend in person, and it isn't limited to the headline events. The regional BSides circuit, sector-specific conferences like S4 and SANS events, vendor user conferences, and the long tail of recorded talks released after the main events all contribute to the same picture. Whether you're walking the floor at Mandalay Bay or watching streams from your couch, getting full value from conference season requires more intention than most security teams bring to it.

## What Conferences Actually Surface That Other Channels Miss

Threat intelligence vendors do a reasonable job of summarizing known threats. Vendor research blogs surface specific findings tied to those vendors' products. Industry ISACs share information within their sectors. None of these channels reliably surface what conferences surface: the emerging research, novel techniques, and field observations from practitioners that haven't yet made it into productized intelligence.

A typical Black Hat or DEF CON cycle includes original research disclosing new attack classes — not just specific vulnerabilities, but new ways of thinking about classes of weakness. The cloud security research presented in 2023 reshaped how organizations thought about IAM misconfiguration months before any vendor product caught up. The hardware research from years past has driven firmware update programs that are still rolling out. The detection engineering talks from defenders consistently outpace what shows up in vendor SIEM content for six to twelve months.

The same pattern repeats at smaller events. BSides events feature researchers who can't or won't speak at the larger conferences but are doing meaningful work, often in specialized areas. We covered some of the threads worth following from a recent event in our [BSides LV 2025 debrief](/blog/bsideslv-2025-debrief-hacker-summer-camp-insights/), and the broader pattern holds: the research published at BSides today often becomes the threat intelligence headline next year.

## Themes Worth Watching in 2026

Several themes are dominating the 2026 conference circuit and warrant attention from security teams regardless of whether you attend the events.

AI security has matured past the initial "prompt injection demos" phase and into substantive research on adversarial use of generative AI, attacks against AI infrastructure, and the security implications of agentic systems with real autonomy. The talks worth watching aren't the ones speculating about future risks but the ones documenting attacks already happening — credential abuse against AI service providers, model theft and extraction, and the specific vulnerabilities introduced by retrieval-augmented generation deployments.

Identity continues to dominate as the primary attack surface, with research increasingly focused on cross-tenant attacks, federation exploitation, and the security implications of consolidated identity providers. The shift from on-premise Active Directory to cloud-native identity has been ongoing for years, but the research community is now publishing concrete techniques against the assumed-secure cloud configurations that organizations have been adopting.

Supply chain security research has moved beyond high-profile incidents into systematic examination of dependencies, build pipelines, and the trust relationships that modern software depends on. Talks on package ecosystem attacks, build infrastructure compromise, and SBOM forensics are producing practical guidance for defenders that goes well beyond "patch your dependencies."

OT and ICS research continues to mature, with growing focus on the IT/OT boundary, the realities of legacy control system environments, and incident response in operational contexts. The research presented at S4, DEF CON's ICS Village, and various sector-specific events is increasingly relevant to organizations far beyond traditional industrial verticals — water, healthcare, transportation, and logistics all share variations of the same problems.

## How to Get Value Without Attending

Plenty of security teams can't justify the cost of sending people to Las Vegas, and the perception that you have to be there to benefit is wrong. The talks from major conferences are recorded and posted, often within weeks. The slide decks circulate. The written research that backs the talks gets published. With some discipline about how you consume it, you can capture much of the technical value remotely.

A practical approach is to designate someone on the team to monitor the major conferences in real time — following session feeds, reading research releases, and noting which talks are generating discussion. After the events conclude, the team reviews the curated list and assigns specific talks for deeper review based on relevance to the organization's environment. The reviewed material then gets summarized for the broader team and turned into specific actions: detection rules to write, configurations to verify, threat models to update.

What you can't replicate remotely is the hallway conversation and the relationship-building. For organizations that want some of that value without sending a delegation, the regional conference circuit is often a better investment. BSides events in your region, SecureWV, ShmooCon, and similar events bring much of the same community at a fraction of the cost and with conversations that are often easier to have than at the chaotic main events.

## How to Get Value If You Do Attend

If your organization is sending people, send them with a plan. The biggest mistake we see is teams that arrive without specific objectives and try to absorb everything, then leave exhausted with notes nobody reads. A better approach is to align attendance with current priorities — if your organization is rolling out cloud identity, focus your sessions on identity research; if you're standing up an AI security program, focus on AI security tracks.

Build in unstructured time. Some of the most valuable conversations happen in hotel lobbies, vendor parties, and meals with people you didn't plan to meet. Schedules that are packed wall-to-wall with sessions miss this entirely. We coach clients to schedule explicit blocks for unstructured engagement and to treat them as protected as the sessions themselves.

Capture aggressively but selectively. The talks you attend matter less than what you do with them afterward. Brief notes on each session — three takeaways, one action — produce vastly more organizational value than verbatim transcripts that nobody reviews. Build the post-conference debrief into your team's calendar before you leave for the event, so the synthesis happens while the material is fresh.

## Translating Conference Insights into Program Changes

The hardest part of conference season isn't finding insights — it's converting them into changes in your security program. Most organizations come back with notebooks full of interesting ideas and execute none of them, because there's no mechanism to convert insight into work.

The mechanism doesn't have to be elaborate. A standing post-conference review where the team presents what they learned, identifies the three to five items most relevant to current priorities, and assigns owners is enough for most organizations. The items go into the team's regular planning cadence rather than living in a separate "things to do someday" list. Items that don't make the cut still get documented, because next year's conference will surface related work and the connection back to last year's notes can accelerate decisions.

Conference season produces signal that the rest of the year doesn't. The teams that compound the most from it are the ones that build conference attendance and follow-up into their actual program rhythm rather than treating the trip as a perk and the insights as ephemera.
