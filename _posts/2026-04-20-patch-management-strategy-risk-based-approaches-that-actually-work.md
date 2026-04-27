---
layout: single
title: "Patch Management Strategy: Risk-Based Approaches That Actually Work"
toc: true
excerpt: "Patching everything within 30 days sounds like good security hygiene until you have 10,000 assets and a six-person team — risk-based patch management uses exploit availability, asset criticality, and exposure context to focus effort where it genuinely reduces breach likelihood. This post covers how to build a defensible, sustainable patch program."
header:
  overlay_image: /assets/images/post-spm.jpg
  teaser: /assets/images/post-spm.jpg
  overlay_filter: 0.5
---

Every patch management policy we review starts with some version of the same aspiration: critical vulnerabilities patched within 7 days, high within 30, medium within 90. It reads well in an audit response. It also bears almost no relationship to what the team is actually doing in practice. By the time a calendar quarter ends, the backlog has grown faster than the patch cadence, the highest-CVSS items are stuck behind change windows that operations will not approve, and the team is patching whatever was easiest rather than whatever mattered most.

The disconnect is not a discipline problem. It is a math problem. Modern environments produce hundreds of new patchable vulnerabilities per month, the surface area of assets continues to expand, and CVSS-based prioritization treats most issues as roughly equivalent when in reality only a small fraction will ever be exploited. A risk-based patch program reframes the question from "how do we patch everything fast enough" to "how do we patch the things that actually reduce breach likelihood, on a cadence we can sustain."

## Why CVSS-Based Prioritization Has Failed

CVSS scores were never designed to drive patching priority — they were designed to describe technical severity in a vendor-neutral way. Treating them as a priority signal produces predictable distortions. Roughly half of all CVEs published carry a high or critical CVSS score, which means that "patch all criticals within 7 days" effectively means "patch about 30 percent of all new vulnerabilities within 7 days," forever. That target is not achievable in any environment we have worked in.

More importantly, CVSS does not predict exploitation. Research by Cyentia and others has consistently shown that only about 5 percent of disclosed CVEs are ever exploited in the wild. The set that gets exploited is not strongly correlated with CVSS score — many highly exploited vulnerabilities are CVSS 7s and 8s rather than 9.8s, and many CVSS 9.8s have never been observed in attacks. A program that patches by CVSS score is therefore expending most of its effort on vulnerabilities that will never be used against it, while potentially missing the lower-scored issues that are actively being exploited.

The Exploit Prediction Scoring System (EPSS), which estimates the probability that a CVE will be exploited within 30 days, and CISA's Known Exploited Vulnerabilities (KEV) catalog, which lists vulnerabilities with confirmed in-the-wild exploitation, have changed what defensible prioritization looks like. A CVE with high EPSS or KEV listing is empirically dangerous in a way that a high CVSS score by itself is not.

## The Inputs That Should Drive Prioritization

Risk-based prioritization combines three factors: the likelihood that a vulnerability will be exploited, the value or sensitivity of the asset where it lives, and the exposure of that asset to potential attackers. None of these factors alone is sufficient — the combination is what produces useful priority signal.

Exploitation likelihood draws on EPSS scores, KEV listing, threat intelligence about active campaigns, and the existence of public exploit code. A CVE on the KEV list is the strongest possible signal — CISA has confirmed that someone is using it against real organizations right now. EPSS scores above 0.5 represent meaningful exploitation probability and warrant accelerated treatment. Public exploit availability — Metasploit modules, proof-of-concept code on GitHub, ransomware operator tooling — substantially raises the likelihood of exploitation against organizations that have not patched.

Asset criticality reflects the business consequence if the asset is compromised. A vulnerability on a domain controller, an internet-facing application server, a database with customer data, or a manufacturing control system represents fundamentally different risk than the same vulnerability on a developer's test VM. Most organizations have asset inventory data that can support criticality scoring, but very few are using it to drive patch prioritization. The connection between asset management and vulnerability management is one of the highest-leverage improvements a patch program can make.

Exposure context is the third leg. A vulnerability on an internet-facing system, accessible from anywhere, is qualitatively different from the same vulnerability on a system that is only reachable from a small administrative network. Exposure data is increasingly available from external attack surface management platforms, internal network segmentation reviews, and cloud security posture tools, and integrating it into patch prioritization meaningfully sharpens the signal.

When you combine these factors, what emerges is typically a list where the top 5 to 10 percent of open vulnerabilities represent the overwhelming majority of practical risk. That is the list a six-person team can actually patch on a meaningful cadence.

## Service Level Objectives That Reflect Reality

Patch SLOs that the team cannot meet do more harm than good. They breed cynicism, encourage lying about closure dates, and damage credibility with audit and leadership when the gap between policy and practice becomes obvious. Sustainable SLOs are tiered based on the risk-based prioritization above, and they are calibrated to what your team can actually deliver.

A defensible tier structure for most organizations: critical risk vulnerabilities (KEV-listed or high EPSS combined with critical asset and external exposure) within 14 days, high risk within 30 to 45 days, medium risk within 90 days, low risk on quarterly maintenance cycles, and informational-only items addressed through routine patching without specific SLOs. The exact numbers should be tuned to your environment and team capacity, but the principle is that genuinely dangerous vulnerabilities get accelerated treatment while less-likely-to-be-exploited issues are addressed on a sustainable cadence.

Track and report SLO performance honestly. If your team is consistently missing the 14-day target on critical-risk items, the answer is either more resources, automation, scope reduction, or a frank conversation with leadership about why the target is not being met. None of those options are improved by manufacturing closure dates to look better in the dashboard.

## Operational Patterns That Make Patching Sustainable

Risk-based prioritization is necessary but not sufficient. Sustainable patch programs also depend on operational patterns that reduce the friction of getting patches deployed.

Standardized maintenance windows are foundational. Reserving recurring time blocks for patching — weekly for non-production, monthly for production — creates predictable opportunities to deploy patches without negotiating individual change windows for each one. The opposite pattern, where every patch requires its own change approval, generates so much administrative overhead that the team patches less than they otherwise could.

Automated patch deployment for standard tiers is a force multiplier. Workstations and standard server images can largely be patched without per-system attention through tools like Microsoft Intune, SCCM, Tanium, BigFix, or cloud-native patch management services. Reserve human attention for the patches that genuinely require it: kernel updates on critical systems, patches with known compatibility issues, or systems where downtime requires coordination with business stakeholders.

Compensating controls deserve more attention than they typically get in patch programs. When a critical vulnerability cannot be patched within the SLO window — because the patch is not yet available, the system cannot tolerate the downtime, or compatibility testing requires more time — explicit compensating controls reduce risk in the interim. Network segmentation that limits exposure, intrusion prevention signatures that detect exploitation attempts, application whitelisting that blocks the post-exploitation actions an attacker would take — these controls turn an unpatched vulnerability into a reduced-risk situation rather than an unmitigated one. Document the compensating controls in your tracking system so the risk story is clear.

## The OT and Legacy Systems Problem

Risk-based patching is straightforward in environments where patches can actually be deployed. Operational technology, embedded systems, and legacy applications often cannot be patched on any reasonable cadence because vendor support has ended, the patch breaks dependent systems, or operational constraints prevent the downtime required.

For these populations, patch management becomes risk management. The question shifts from "how fast can we patch" to "how do we keep this system safe given that we cannot patch it." The answer is layered compensating controls: aggressive network segmentation that limits the system's exposure, monitoring tuned to detect exploitation of the known unpatched vulnerabilities, and explicit ownership of the residual risk by the appropriate business stakeholder. Treat these populations as a distinct workstream with its own metrics and ownership rather than failing them against patch SLOs that were never realistic.

The longer-term work is reducing the legacy population through modernization. Every system that comes off the unpatchable list because it was replaced or upgraded is a permanent risk reduction, not a recurring patching burden.

## Connecting Patching to Posture Management

Patch management does not exist in isolation — it is one of several related disciplines that together determine your security posture. Misconfiguration management, identity hygiene, exposure management, and vulnerability management feed into the same overall risk picture, and treating them as separate programs with separate priorities produces inefficient effort allocation. Our [continuous security posture assessment](/blog/continuous-security-posture-assessment/) approach addresses this directly: prioritizing remediation effort based on the integrated risk picture rather than each discipline's separately scored backlog.

The unifying metric across these disciplines is exposure to realistic attack paths. A vulnerability that an attacker could exploit to reach your most sensitive data deserves more attention than one with the same CVSS score that an attacker could not reach without first compromising several other systems. Attack path analysis tools like BloodHound for Active Directory, or commercial platforms that build similar models across cloud environments, can sharpen patch prioritization by surfacing which vulnerabilities sit on practical attack paths to high-value assets.

## Measuring Program Health

The metrics that indicate a healthy patch program go beyond SLO compliance. Track mean time to patch for KEV-listed vulnerabilities specifically — these are the items where speed most clearly translates to risk reduction. Track the size of the open vulnerability backlog over time, broken down by risk tier, to understand whether the program is keeping up with new disclosures or falling behind. Track the percentage of internet-facing systems with no known critical vulnerabilities — this is one of the strongest indicators of resistance to opportunistic attack.

Report patch program metrics in business terms when possible. "We have reduced our exposure to ransomware-affiliated CVEs by 60 percent over the last quarter" lands differently with leadership than "We patched 1,400 vulnerabilities last month." The former connects effort to outcome; the latter measures activity.

A risk-based patch management program is more work to design than a CVSS-driven one, and it requires commitment to honest measurement and prioritization. The reward is a program that actually reduces breach likelihood, that the team can sustain without burning out, and that you can defend confidently in an audit, board meeting, or post-incident review. The alternative — patch policies that look good on paper while the actual risk picture continues to deteriorate — is a setup for the kind of incident no patch program survives professionally.
