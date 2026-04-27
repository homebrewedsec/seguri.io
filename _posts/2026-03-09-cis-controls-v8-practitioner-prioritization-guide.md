---
layout: single
title: "CIS Controls v8: A Practitioner's Prioritization Guide"
toc: true
excerpt: "CIS Controls v8 reorganized 153 safeguards into 18 control groups and introduced Implementation Groups to help organizations prioritize based on their risk profile — but many security teams still treat the controls as an undifferentiated checklist. This post explains how to actually use IG1, IG2, and IG3 to drive prioritized security investment."
header:
  overlay_image: /assets/images/post-mvsp.jpg
  teaser: /assets/images/post-mvsp.jpg
  overlay_filter: 0.5
---

The Center for Internet Security released v8 of the CIS Controls in May 2021, and the changes were more than cosmetic. The consolidation from 20 control families to 18, the reorganization of safeguards around "what you manage" rather than "what you do," and the refinement of the Implementation Group model all reflect lessons from organizations that had tried to implement earlier versions. The result is a framework that is significantly more usable — if you engage with it as a prioritization tool rather than a compliance checklist.

The Implementation Groups are the feature that most distinguishes v8 from its predecessors, and they're also the most underused. IG1 represents basic cyber hygiene — the 56 safeguards that every organization should implement regardless of size, sector, or resources. IG2 builds on IG1 with 74 additional safeguards for organizations with greater complexity or risk. IG3 adds the remaining 23 safeguards for organizations facing significant threat actor capability or operating in high-stakes sectors. The IGs are cumulative and sequential: you don't work on IG2 safeguards until IG1 is complete.

This sequencing principle is where many security programs go wrong. They select controls based on what seems interesting, what a recent audit flagged, or what a vendor is selling — rather than systematically building from the foundation up. The result is programs with sophisticated capabilities in some areas and embarrassing gaps in others.

## Understanding What IG1 Actually Covers

IG1 is described as "essential cyber hygiene" and that description undersells it. The 56 IG1 safeguards represent the CIS community's consensus on the minimum conditions for organizational security — not the minimum conditions for passing an audit, but the minimum conditions for not being trivially compromised by non-targeted attackers.

The IG1 safeguards span inventory and control of enterprise assets (Control 1), inventory and control of software assets (Control 2), data protection (Control 3), secure configuration of enterprise assets and software (Control 4), account management (Control 5), access control management (Control 6), continuous vulnerability management (Control 7), audit log management (Control 8), email and web browser protections (Control 9), and malware defenses (Control 10).

What's notable about this list is how foundational it is. Knowing what assets you have and ensuring they're running expected, configured software is prerequisite to almost every other security capability. Organizations that lack an accurate asset inventory can't patch systematically, can't enforce configurations, can't monitor effectively, and can't respond to incidents efficiently. Control 1 and Control 2 aren't the exciting parts of the framework, but neglecting them undermines everything built on top.

A rigorous IG1 assessment often reveals that organizations have implemented sophisticated later-stage capabilities — endpoint detection and response tools, SIEM platforms, advanced network monitoring — while still having significant gaps in basic inventory, configuration management, and account hygiene. These gaps create attack surface that the sophisticated tools can't compensate for.

See [Minimum Viable Security Product](/blog/minimum-viable-security-product/) for a complementary perspective on how to think about baseline security fundamentals — the concepts align closely with the IG1 philosophy of foundational hygiene before advanced capability.

## IG2: Managing Complexity and Sensitive Data

The 74 IG2 safeguards extend the baseline with capabilities appropriate for organizations that have significant digital operations, handle sensitive data, or have regulatory obligations that require more rigorous control environments. IG2 introduces more substantive requirements in areas like data governance, network infrastructure management, security testing, and security awareness.

Several IG2 areas deserve specific attention for organizations making the transition from IG1 maturity.

Data classification and protection at IG2 requires knowing not just what data you have, but what sensitivity level it carries and whether your technical controls are calibrated to that sensitivity. Many organizations have blanket DLP policies or encryption requirements but lack the data classification program that makes those controls purposeful rather than arbitrary.

Penetration testing appears at IG2 (Control 18) — specifically, Safeguard 18.5 requires annual penetration tests for organizations at IG2. This is a meaningful requirement because penetration testing validates whether your IG1 and IG2 safeguards are actually functioning as intended, rather than simply present in policy. Configuration drift, incomplete deployments, and compensating controls that don't actually compensate are all things that penetration testing surfaces that internal assessments often miss.

Network infrastructure management at IG2 introduces more rigorous requirements for network segmentation and secure network architecture. The IG1 baseline establishes basic network boundary controls; IG2 expects organizations to manage network infrastructure intentionally, with documented architecture, controlled change processes, and monitoring of network traffic for anomalies. This is where many organizations discover that their "flat" internal network architecture is a significant risk amplifier — an attacker who compromises any internal system can typically reach everything else.

## IG3: High-Stakes Environments

The 23 IG3 safeguards address organizations facing the most capable threat actors or operating in environments where security failures have severe consequences — financial services, healthcare, critical infrastructure, defense contractors, and similar sectors. IG3 capabilities include application layer filtering, advanced malware analysis, advanced penetration testing (including red team exercises), and more rigorous data recovery and incident response requirements.

IG3 organizations aren't just doing more of what IG2 does — they're operating security programs at a qualitatively different level. The threat actors targeting IG3 organizations include nation-state actors and sophisticated criminal groups that have the capability, time, and motivation to defeat controls that would stop lower-capability attackers. Detection and response capabilities need to be calibrated to that threat level: not just monitoring for known-bad indicators, but actively hunting for attacker tradecraft before alerts fire.

If your organization is genuinely in the IG3 risk profile, the 23 IG3 safeguards are not optional enhancements — they're the capabilities you need to have a realistic chance of detecting and responding to targeted attacks before significant impact occurs. The investment required to achieve IG3 maturity is substantial, but so is the cost of a successful breach against the assets and operations you're trying to protect.

## Using the Controls for Program Prioritization

The practical value of the Implementation Group model is that it provides a principled answer to the question security teams face constantly: "What should we work on next?" The answer is: complete IG1 before investing in IG2 capabilities, and complete IG2 before investing in IG3 capabilities. Within each IG, prioritize the safeguards with the highest coverage-to-effort ratios — the ones that close the most exposure with the least investment.

CIS provides the CIS Controls Self-Assessment Tool (CIS CSAT) as a free resource for organizations to assess their current coverage against the safeguards and track progress over time. The tool produces a coverage scorecard by control group and Implementation Group, which provides both a baseline assessment and a structured improvement roadmap. For organizations that need to communicate security program progress to leadership or boards, CIS CSAT scores provide a concrete, externally validated framework for that conversation.

Gap remediation should be sequenced by risk, not by control number. Within IG1, gaps in asset inventory (Controls 1 and 2) and access control (Controls 5 and 6) typically carry higher risk than gaps in email filtering (Control 9), because account compromise and unauthorized access enabled by inventory blindspots are more frequent attack pathways than malware delivered through email in environments with basic filtering. Assess the gaps you find against your specific environment's threat model, not just the control sequence.

## Mapping CIS Controls to Other Frameworks

Many organizations operate under multiple compliance frameworks — PCI DSS, HIPAA, NIST CSF, SOC 2, or sector-specific requirements. CIS v8 provides mappings to all major frameworks, which enables organizations to rationalize their control environment rather than maintaining parallel control sets.

The practical implication is that achieving IG1 maturity addresses a meaningful percentage of requirements across most major compliance frameworks, because they all share a common foundation of basic hygiene requirements. Organizations that focus on IG1 completion first, then extend to IG2, build a compliance-ready control environment more efficiently than those that chase individual framework requirements in isolation.

CIS also maintains specific implementation guidance for cloud environments (CIS Benchmarks), specific platforms (Windows, Linux, macOS, common enterprise applications), and sector-specific configurations that make the v8 safeguards more directly actionable than the framework's abstract language might suggest. Leveraging these resources turns the CIS Controls from a policy framework into an operational implementation guide.

The controls don't implement themselves, and achieving genuine IG1 maturity across a complex enterprise environment requires sustained effort. But the organizations that get there — that actually know what assets they have, ensure they're properly configured, manage accounts and access rigorously, and maintain continuous visibility — are dramatically more resilient to the attack patterns that account for the vast majority of successful breaches.
