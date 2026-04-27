---
layout: single
title: "Critical Infrastructure Protection: Lessons from Recent Incidents"
toc: true
excerpt: "Recent attacks on water utilities, energy infrastructure, and manufacturing have demonstrated that critical infrastructure operators face sophisticated, patient adversaries willing to establish long-term persistence before acting. This post draws lessons from publicly disclosed incidents and translates them into defensive priorities for industrial asset owners."
header:
  overlay_image: /assets/images/post-critical-infrastructure.jpg
  teaser: /assets/images/post-critical-infrastructure.jpg
  overlay_filter: 0.5
---

The pattern across major critical infrastructure incidents of the past several years is consistent enough to be instructive. Adversaries gain initial access through IT networks — often through internet-facing systems, VPN appliances, or vendor connections — establish persistence quietly, spend weeks or months learning the environment, and then either act at a moment of strategic timing or get discovered during an unrelated investigation. The dwell times measured in these incidents are not outliers. They are the standard.

What makes critical infrastructure targeting different from typical enterprise intrusions is the patience and the objective. Financially motivated actors want to encrypt files and collect ransom as quickly as possible. Nation-state actors targeting power grids, water systems, and manufacturing facilities often want something else entirely: pre-positioned access that can be activated during a geopolitical crisis, or detailed knowledge of how industrial processes work that can be used to cause physical effects rather than just data disruption. Both types of adversaries are active against critical infrastructure today, and the defensive implications differ in important ways.

This post draws from publicly disclosed incidents — Volt Typhoon's documented activity across US critical infrastructure, the Oldsmar water treatment intrusion, incidents affecting European energy providers following the Ukraine conflict, and manufacturing sector ransomware campaigns — to identify the defensive priorities that matter most for industrial asset owners.

## The IT/OT Boundary Remains the Primary Attack Path

Every major OT-affecting incident in the public record began with IT network compromise. Not a single one involved adversaries teleporting directly into industrial control systems from the internet. The path is consistent: compromise an IT system, establish persistence, conduct reconnaissance to identify OT network connections, pivot across the boundary, and then begin learning the OT environment.

This means that IT security quality is a foundational OT security requirement — but most industrial organizations don't operationalize that relationship. IT teams and OT teams frequently operate with limited coordination, separate tool stacks, different incident response procedures, and in some cases genuine organizational friction about ownership of boundary systems. Adversaries exploit exactly this seam.

The boundary itself is often softer than organizations believe. In multiple publicly disclosed incidents, investigators found unexpected network paths between IT and OT segments that weren't reflected in network diagrams. Historian servers, jump servers, engineering workstations, and vendor remote access solutions are common conduits. Organizations that mapped their IT/OT network connections in a tabletop exercise found the actual connection count to be two or three times what the documentation described.

The defensive priority here is straightforward: enumerate every path that connects IT and OT networks, validate that the paths are intentional and properly controlled, and establish monitoring at each crossing point. This is harder than it sounds but more tractable than most organizations assume.

## Volt Typhoon and Living-Off-The-Land Techniques

The Volt Typhoon advisory from CISA, NSA, and FBI published in 2023 and updated in 2024 deserves careful reading by every critical infrastructure security team. The activity described represents a shift in adversary tradecraft that has direct implications for detection strategy.

Living-off-the-land techniques — using native operating system tools and legitimate remote management capabilities rather than custom malware — are specifically designed to defeat signature-based detection. The Volt Typhoon operators used tools like wmic, ntdsutil, netsh, and built-in Windows remote management capabilities. They moved slowly, blended with normal administrative activity, and in multiple cases used credentials obtained from legitimate sources rather than deploying malicious tooling.

The detection implication is that organizations relying primarily on endpoint detection and response products with malware-detection tuning will miss this class of activity. Detecting living-off-the-land requires behavioral baselines — understanding what normal administrative activity looks like in your environment and alerting on deviations. This includes process lineage anomalies, unusual use of administrative tools from atypical parent processes, lateral movement patterns, and credential usage from unexpected systems or times.

For most critical infrastructure operators, building behavioral detection capabilities requires a logging investment first. OT environments often have limited logging, and IT environments in industrial sectors frequently have logging gaps on the workstations and servers that adversaries use as pivot points. Closing those logging gaps is a prerequisite to meaningful detection improvement.

## The Oldsmar Lessons Are Still Relevant

The 2021 intrusion at the Oldsmar, Florida water treatment plant — where an attacker briefly increased sodium hydroxide levels to potentially dangerous concentrations — generated significant attention and then faded from the security conversation relatively quickly. That's a mistake, because the technical findings from that incident are still directly applicable.

The attacker gained access through TeamViewer, a remote access tool that had been installed for legitimate operational purposes. The system was running an unsupported version of Windows 7. Multiple systems on the OT network shared the same TeamViewer credentials. There was no MFA on remote access. An operator noticed the anomalous control change and corrected it manually, providing a physical safeguard that prevented consequences.

None of these findings are unique to Oldsmar. Remote access tools deployed for operational convenience without security controls, legacy operating systems that can't be patched due to vendor support constraints, shared credentials across multiple systems, and absence of multi-factor authentication remain endemic in water and wastewater systems, small municipal utilities, and industrial facilities across every sector. The Oldsmar attacker didn't use sophisticated techniques because they didn't need to.

The defensive priorities this incident illustrates are accessible to organizations with limited security budgets: audit and reduce remote access tools, require MFA on everything that connects to OT systems from outside, eliminate shared credentials, and document the physical safeguards that exist in industrial processes so that incident response planning can account for them.

## Supply Chain and Vendor Access Exposure

The SolarWinds compromise demonstrated at scale what critical infrastructure security teams already knew at a smaller scale: vendor and third-party access is an attack surface that most organizations manage poorly. Automation vendors, system integrators, SCADA software providers, and remote monitoring services all require periodic access to industrial systems — and each represents a potential entry point if their own security posture is compromised or their credentials are misused.

Volt Typhoon specifically leveraged trusted vendor relationships and legitimate remote access tools in several of its documented intrusion paths. The adversary didn't need to break through perimeter security because they used access paths that were already open and trusted.

Managing this exposure requires a vendor access governance program that goes beyond paperwork. Vendor remote access should be time-limited, session-logged, require MFA, and be terminated when not in active use rather than left standing. Vendor accounts should be reviewed regularly against the current roster of active vendors and active projects. Jump server architectures that funnel vendor access through monitored chokepoints provide visibility and control that direct VPN access does not.

For organizations still managing vendor access through VPN credentials emailed to field technicians, this is a high-priority risk reduction opportunity that doesn't require significant capital investment — it requires process discipline and access management tooling that most organizations already own.

## Detection and Response in OT Environments

Detection in OT environments has historically been treated as optional or impossible. The arguments against OT monitoring are familiar: industrial protocols are proprietary, passive monitoring is technically complex, active scanning risks disrupting sensitive processes, and the OT team doesn't want IT security tools anywhere near production systems.

These objections are real but increasingly untenable. Passive OT network monitoring — which captures traffic without injecting packets into industrial networks — has become technically mature and operationally proven. Products from multiple vendors can decode industrial protocols, baseline normal communications patterns, and alert on anomalies without any active interaction with OT systems. The operational disruption concern, properly addressed, reduces to deploying network taps or SPAN ports correctly, which is a solved problem.

The detection priority for OT environments should start with anomalous communication patterns rather than signature-based malware detection. New connections between OT assets, communication to IP addresses outside normal operational scope, protocol anomalies, and unexpected engineering workstation activity are the signals that matter. These are detectable with network monitoring tools that OT teams can typically accept because they don't require agents on industrial controllers.

Response in OT environments requires explicit planning that accounts for process continuity requirements. The incident response procedures that work in IT environments — isolate the affected system, image it, restore from backup — don't apply directly to industrial processes that can't be safely interrupted. See [OT incident response planning](/blog/ot-incident-response-planning-beyond-traditional-playbooks/) for a detailed treatment of how to build playbooks that account for OT operational constraints.

## Building Defensible Industrial Architecture

The underlying lesson from the corpus of critical infrastructure incidents is that adversaries are exploiting architectural decisions made years ago under different threat assumptions. Legacy systems with unpatched vulnerabilities, flat network segments, standing remote access credentials, and absent monitoring reflect the security posture of organizations that didn't anticipate being targeted by nation-state actors or sophisticated ransomware groups. That's understandable historically but not a sustainable operating model.

Improving critical infrastructure security doesn't require replacing installed base. It requires layering compensating controls onto existing architecture while building toward a more defensible long-term design. That means network segmentation and monitoring at OT boundaries, access control enforcement on the paths adversaries actually use, logging and detection capability for the living-off-the-land techniques documented in recent advisories, and incident response planning that accounts for the physical consequences that distinguish industrial incidents from IT incidents.

CISA's Cross-Sector Cybersecurity Performance Goals provide a reasonable starting framework for organizations assessing where to invest first. They're not comprehensive, but they identify the highest-leverage controls across the incident corpus — and they're available at no cost to any organization that wants a structured starting point.

The critical infrastructure security problem is not primarily a technology problem. It's an organizational commitment problem. The organizations that have made measurable progress against this threat landscape are the ones that secured executive and board-level attention, built cross-functional relationships between IT and OT teams, and committed sustained resources to the problem — not the ones that deployed a new tool and moved on.
