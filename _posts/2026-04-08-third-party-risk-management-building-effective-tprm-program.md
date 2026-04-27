---
layout: single
title: "Third-Party Risk Management: Building an Effective TPRM Program"
toc: true
excerpt: "Your third-party vendors now represent one of the most significant and least-controlled risks in your environment — and most TPRM programs are still built around annual questionnaires that don't correlate with actual security outcomes. This post outlines a maturity-based approach to third-party risk that moves from compliance theater to genuine risk reduction."
header:
  overlay_image: /assets/images/post-ma-security.jpg
  teaser: /assets/images/post-ma-security.jpg
  overlay_filter: 0.5
---

The defining characteristic of major breaches over the last several years isn't sophisticated malware or zero-day exploits — it's supply chain and third-party access. Attackers have figured out that it's often easier to compromise one vendor with privileged access to a hundred customers than to attack those hundred customers directly. SolarWinds. MOVEit. Change Healthcare. In each case, the victim organizations' own security controls were largely irrelevant, because the attack entered through a trusted third party that had already been given access.

Most TPRM programs were not designed with this threat model in mind. They were designed to satisfy compliance requirements — specifically, to demonstrate that you asked vendors about their security practices and kept records of the answers. Annual questionnaires, SOC 2 report reviews, and contract security clauses create documentation that auditors can check. What they don't reliably do is reduce the probability that a vendor compromise leads to your compromise. Building an effective TPRM program means understanding the difference and closing the gap.

## Tiering Your Vendor Inventory: Not All Vendors Are Equal

The first mistake most organizations make in TPRM is treating all vendors the same. Running a comprehensive assessment process on your coffee machine vendor with the same rigor you apply to your identity provider wastes resources and produces a program too cumbersome to operate consistently. Effective TPRM starts with a tiering framework that allocates assessment depth proportionally to actual risk.

Risk tiering should consider two dimensions: the nature of the vendor's access to your environment or data, and the criticality of what they could affect. A vendor with read-only access to anonymized analytics data poses fundamentally different risk than a vendor whose software runs with elevated privileges on your production systems. A vendor who processes payment card data for you carries regulatory risk on top of operational risk. A vendor who provides your primary authentication service sits in a category where failure means your business stops functioning.

Typical tiering models use three to four tiers. Tier 1 (highest risk): vendors with privileged access to critical systems, vendors who process regulated data at scale, vendors whose software runs in your environment with significant permissions. Tier 2: vendors with limited access to your systems or data, SaaS vendors handling business data without regulated classification. Tier 3 (lowest risk): vendors with no meaningful access to your environment, pure commodity services. Assessment frequency, depth, and ongoing monitoring intensity should match the tier. Tier 1 vendors get annual comprehensive assessments plus continuous monitoring; Tier 3 gets a lightweight questionnaire at onboarding and a periodic review.

Building this inventory is often harder than building the assessment process. Organizations routinely discover that their official vendor list significantly undercounts actual vendors — particularly SaaS applications that business units adopted independently. Shadow IT discovery, finance system queries for recurring software subscriptions, and IT asset management data all feed into a complete vendor inventory. Don't let perfect be the enemy of good here: start with the vendors you know about and build completeness over time.

## Beyond Questionnaires: Assessment Methods That Reveal Actual Risk

The standard security questionnaire asks vendors to attest to their security practices. The problem is that attestation is cheap — a vendor can check "yes" next to "we have a vulnerability management program" without that program bearing any resemblance to a functional one. Attestation data is better than nothing, but it shouldn't be the primary signal in your assessment.

More signal-rich assessment methods include: reviewing the vendor's SOC 2 Type II report (not just its existence, but the exceptions and management responses), requesting evidence of specific controls rather than yes/no attestations, conducting technical assessments of vendor-provided access points, and using external attack surface intelligence to understand what the vendor's internet-facing infrastructure looks like.

SOC 2 report reviews reward closer reading. A vendor can hold a clean SOC 2 report with a report period that ended 18 months ago, exceptions noted in the auditor's findings that were never remediated, and a scope that excluded the exact system that connects to your environment. Read the report, not just the existence of the certificate. Pay attention to the scope section (what systems were included), the description of controls, any qualified opinions or exceptions, and the management response to any findings.

For Tier 1 vendors, consider requiring right-to-audit clauses in contracts and exercising them — or commissioning a vendor security assessment as part of the initial procurement process. External attack surface assessments using OSINT and passive reconnaissance techniques can surface internet-facing systems, certificate transparency data, known vulnerabilities in vendor-operated infrastructure, and data breach history. These inputs don't require vendor cooperation and often reveal things that questionnaire responses don't.

The vendor's incident history matters too. A vendor with a breach history isn't automatically disqualified, but how they handled previous incidents — detection time, notification speed, remediation quality, customer communication — is a meaningful predictor of how they'll handle a future one. This information is often publicly available for significant breaches.

## Contractual Security Requirements: Writing Controls That Matter

Security requirements in vendor contracts tend toward the generic: "vendor will maintain commercially reasonable security practices" covers essentially nothing. If your contracts don't specify what security practices you're actually requiring, you have no enforceable standard when something goes wrong and no leverage to require remediation of gaps discovered during assessment.

Meaningful contractual security requirements include: notification timelines for security incidents (24 or 48 hours to report incidents affecting your data is achievable and meaningful, versus the "without undue delay" language that can mean weeks), specific data handling requirements aligned with your data classification standards, subprocessor management (vendors who outsource work to sub-vendors should be required to flow down equivalent security requirements), right-to-audit clauses, requirements to maintain specific certifications, and breach liability provisions.

Incident notification clauses deserve particular emphasis given the current threat landscape. When a vendor is compromised, your incident response timeline starts from when you're notified. A vendor who takes three weeks to notify you of a breach that may have exposed your data — or who never notifies you directly and lets you find out from news coverage — fundamentally breaks your ability to respond effectively. Short, specific notification timelines with defined content requirements (what the notification must include) are worth fighting for in contract negotiations.

For organizations going through acquisitions or inheriting vendor relationships from acquired companies, the TPRM integration process is a specific and often challenging problem. See our post on [securing mergers and acquisitions](/blog/securing-mergers-and-acquisitions/) for how to approach inherited vendor risk as part of an M&A security integration.

## Continuous Monitoring: Moving From Point-in-Time to Ongoing Visibility

Annual assessments create a point-in-time snapshot that may be accurate for a few months and increasingly irrelevant as the year progresses. A vendor who passed your assessment in January may have experienced a breach in March, changed ownership in July, or deprecated the security controls you assessed in October. Point-in-time assessment combined with continuous monitoring is significantly more effective than either alone.

Continuous monitoring for vendor risk operates at several levels. At the most basic level, automated feeds can alert you when a vendor is mentioned in breach disclosures, dark web credential dumps, or cybersecurity news. Services like SecurityScorecard, BitSight, and RiskRecon provide ongoing assessment of vendors' external attack surfaces, tracking things like exposed services, certificate issues, patch cadence on internet-facing infrastructure, and indicators of compromise. These scores are imperfect but provide signal between formal assessment cycles.

Internally, monitoring vendor access patterns in your own environment is often more direct. If a vendor has privileged access to your systems, your SIEM and identity management tools should be tracking what that vendor's accounts are doing. Anomalous access patterns — activity at unusual hours, access to systems outside the vendor's normal scope, large data exports — should trigger investigation. Many vendor compromises are visible in the victim's own logs, in retrospect, if someone was looking.

## Governance and Operating the Program

A TPRM program that exists as a set of processes but has no clear ownership, no integration with procurement, and no escalation path for high-risk findings doesn't actually reduce risk. Governance is the difference between a program that runs and one that sits on a shelf.

Effective governance requires: a named program owner with authority to require assessment completion before vendor onboarding, integration with the procurement process so that security review is a gate rather than a retrospective activity, a defined escalation process for vendors who fail assessment or refuse to remediate findings, and executive visibility into the program's scope and high-risk findings.

The integration with procurement is particularly important. If business units can onboard vendors without security review, the program covers only the vendors who were conscientious enough to ask — which means it misses the highest-risk ones, since high-risk vendors are more likely to be adopted by business units moving quickly. Embedding security review as a required step in the procurement workflow, with appropriate tiering so it doesn't impose equivalent burden on all vendors, makes the program comprehensive rather than opt-in.

Third-party risk isn't a problem you can solve once and declare finished. The vendor landscape changes, your environment changes, and the threat landscape changes. Building the operational discipline to maintain your vendor inventory, run tiered assessments on a regular cadence, monitor continuously, and enforce contractual requirements when needed is what separates a mature TPRM program from a compliance exercise.
