---
layout: single
title: "ICS/SCADA Security Assessments: A Practitioner's Approach"
toc: true
excerpt: "ICS/SCADA assessments require a fundamentally different methodology than IT security assessments — active scanning that's routine in enterprise environments can cause real process disruption or equipment damage in OT networks. This post covers the assessment techniques, tooling, and engagement practices that produce meaningful findings without endangering operations."
header:
  overlay_image: /assets/images/service-ot-security.jpg
  teaser: /assets/images/service-ot-security.jpg
  overlay_filter: 0.5
---

The penetration testing methodology that works well in an enterprise IT environment is a liability in an industrial control system network. Nmap scans, aggressive enumeration, and the assumption that systems can handle unexpected traffic are foundational to IT security assessment practice. In an ICS/SCADA environment, those same techniques can cause uncontrolled process shutdowns, equipment damage, or safety system activation. A testing approach designed for a web application or Active Directory environment has no place near a distributed control system managing a physical process.

This isn't a theoretical concern. There are documented cases of well-intentioned security activities — including legitimate assessments — causing operational disruptions in OT environments because the practitioners didn't fully understand the constraints. The cost of an unplanned process shutdown in a manufacturing facility, water treatment plant, or electric utility isn't measured in SLA violations. It can mean product loss, equipment damage, regulatory consequences, or in the worst cases, safety incidents.

Effective ICS/SCADA assessments require practitioners who understand both the security domain and the operational technology domain — and who have the discipline to apply methodologies appropriate to the environment even when they're less efficient than their IT counterparts.

## Understanding the OT Environment Before Assessment Begins

The pre-engagement phase of an OT assessment is longer and more consequential than in IT assessments. Before any assessment activity begins, practitioners need a thorough understanding of what's running, how it's connected, and what the operational impact of a disruption would be.

The first step is documentation review. Process and instrumentation diagrams (P&IDs), network diagrams, asset inventories, and vendor documentation for critical systems should all be collected and reviewed before any on-site or remote assessment activity. This isn't just efficiency — it's risk management. Understanding the control network topology before probing it means you don't accidentally send traffic to a safety instrumented system while trying to enumerate a historian.

Engage operations and engineering staff early and keep them engaged throughout. The security team may have authorized the assessment, but the people who understand what's critical and what's fragile are the process engineers, control system technicians, and operations supervisors. A good pre-assessment workshop with these stakeholders will reveal undocumented systems, unsupported legacy equipment that can't handle unexpected traffic, and operational windows when assessment activity is less risky because processes are in standby or maintenance mode.

Define a clear scope boundary between IT and OT networks. Many industrial environments have blurry IT/OT boundaries — historians that connect to both domains, DMZs with varying degrees of actual segregation, wireless networks that span both. Understanding exactly where the OT assessment scope begins and ends prevents scope creep into systems that weren't cleared for assessment activity.

Establish explicit emergency stop criteria and communication channels. Before the assessment starts, everyone involved should know the answer to: if something goes wrong, who gets called, what happens to assessment activity, and what's the process for determining whether assessment activity caused the issue? These aren't pessimistic preparations — they're professional practice.

## Passive Assessment Techniques: The Foundation of OT Security Testing

Passive assessment techniques — those that observe traffic and system behavior without generating new traffic — are the appropriate starting point for most OT assessment activity and often produce the most valuable findings.

Network traffic capture and analysis from a span port or tap on the process control network reveals the actual communication patterns of the environment. What protocols are in use? Which devices are talking to which other devices? Are there unexpected connections — a PLC communicating with an IP address outside the control network, or a workstation establishing outbound connections that don't match legitimate historian or HMI behavior? Protocols like Modbus, DNP3, EtherNet/IP, and Profinet have well-documented behavior patterns. Traffic analysis against those patterns identifies anomalies without generating a single additional packet.

Passive discovery from network traffic is more complete than many practitioners expect. You can identify assets, map communication relationships, and detect unencrypted credential transmission in process protocols — all without active scanning. Tools designed for OT environments, including Dragos Platform, Claroty, and Nozomi Networks, perform continuous passive discovery and can be deployed in assessment mode to provide a rapid baseline of the network's actual communication topology.

Configuration review of PLCs, HMIs, RTUs, and historians — conducted through vendor-approved management interfaces rather than generic scanning tools — provides findings about authentication configuration, remote access exposure, default credentials, and patch status without any risk to operational continuity. This requires physical or logical access to management interfaces, which is why pre-engagement engagement with operations staff matters so much: they can provide authorized access through appropriate channels without requiring you to enumerate your way there.

Wireless assessment is particularly valuable in industrial environments, where unauthorized wireless access points and poorly configured industrial wireless networks are common findings. Site survey tools can identify the wireless landscape passively before any active testing.

## Controlled Active Testing: What's Appropriate and How to Execute It

Some active testing is appropriate in OT environments, but the scope is narrower and the execution more cautious than in IT assessments.

Architecture and segmentation testing — verifying that firewall rules and network controls actually enforce the intended segmentation — is generally appropriate and important. The finding that OT network traffic can reach internet-routable addresses, or that an engineering workstation has unrestricted access to the safety system network, is a high-severity finding that architecture review alone may not surface. Controlled attempts to traverse segmentation boundaries, conducted during agreed maintenance windows and with operations staff on standby, can verify whether the segmentation is actually effective.

Authentication testing against management interfaces — HMI login pages, engineering software, remote access portals — using controlled techniques is appropriate when those interfaces are confirmed to be management-only and not directly controlling physical processes. Testing for default credentials, weak authentication policies, and account lockout behavior on management interfaces is meaningfully different from probing devices in the control path itself.

Where active vulnerability scanning is done at all, it should use OT-aware scanning tools configured for safe operation in industrial environments. Tenable OT (formerly Indegy) and Claroty provide scanning capabilities specifically designed to avoid traffic patterns that ICS devices handle poorly. Even with OT-aware tools, scanning should be conducted on a segmented test network or during confirmed maintenance windows, never against live process control systems during production operation.

Treat vendor guidance as a hard constraint, not a suggestion. If a PLC vendor's documentation says "do not use network scanners against this device," that guidance exists because the device firmware handles unexpected traffic poorly. Ignoring vendor constraints in pursuit of a more comprehensive assessment is how assessments cause incidents.

## Findings Prioritization and Reporting for OT Environments

OT assessment findings require different prioritization logic than IT findings. The standard CVSS scoring system wasn't designed for environments where a vulnerability's actual impact depends heavily on what physical process the affected system controls.

A remote code execution vulnerability in an HMI controlling a low-consequence monitoring function is a different priority than the same vulnerability in an HMI that an operator uses to manage chemical dosing or pressure control. Standard CVSS doesn't capture that distinction. OT-specific risk scoring frameworks, including those in NIST SP 800-82 and IEC 62443, provide better models for contextualizing vulnerability severity in terms of the safety, operational, and environmental consequences of exploitation.

For each significant finding, the report should include remediation guidance that accounts for OT constraints. Recommending "apply the vendor patch immediately" for a finding against a system whose vendor released the last patch three years ago, and whose support contract has lapsed, is not operationally useful guidance. Practical remediation for OT environments often involves compensating controls — network segmentation, access restrictions, monitoring — rather than immediate patching, because the patching process itself may require process shutdown and change management that can't happen on an IT-equivalent timeline.

The remediation discussion should also address the risk of leaving findings open during the period before remediation is feasible. If a critical finding requires a scheduled maintenance window six months away to address, the report should identify what interim risk mitigation looks like and whether that interim risk is acceptable given the process criticality.

## The Long-Term OT Security Program: Beyond Point-in-Time Assessment

A periodic assessment, even a well-executed one, provides a snapshot. OT environments change — new devices get connected, configurations get modified, remote access gets provisioned for vendor support — and the security posture that existed at assessment time may not reflect reality six months later.

Continuous passive monitoring is the complement to periodic assessment. An OT-aware network monitoring solution that watches process network traffic against a behavioral baseline will detect new assets, unexpected communication patterns, and anomalous protocol behavior in near-real-time. This doesn't replace assessment — assessment provides the depth that continuous monitoring lacks — but it provides ongoing visibility that periodic snapshots cannot.

Asset inventory management is foundational to everything else. OT environments notoriously have incomplete asset inventories, with devices added over decades of operation that nobody fully tracks. A thorough passive discovery during the assessment, combined with a process for maintaining inventory going forward, dramatically improves the security program's ability to manage the environment.

For organizations operating critical OT infrastructure, our post on [OT managed detection and response](/blog/ot-managed-detection-response-unique-requirements/) covers the ongoing operational security considerations that assessments inform but can't replace. The assessment finds where you stand today. The monitoring program tells you where you stand tomorrow.
