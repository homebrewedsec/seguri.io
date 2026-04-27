---
layout: single
title: "Cyber Insurance: What Coverage Actually Protects You"
toc: true
excerpt: "Cyber insurance policies have become increasingly technical in their requirements, with carriers demanding evidence of specific controls before they'll pay claims — and the gap between what policyholders think is covered and what actually is can be financially devastating. This post helps organizations understand policy terms, align security controls with coverage requirements, and use insurance as a genuine risk transfer tool."
header:
  overlay_image: /assets/images/post-cyber-insurance.jpg
  teaser: /assets/images/post-cyber-insurance.jpg
  overlay_filter: 0.5
---

The cyber insurance market has changed substantially over the past several years, and not in ways most policyholders fully appreciate. Premiums have climbed, coverage limits have tightened, and exclusions have multiplied. More importantly, carriers have moved from underwriting based on relatively simple questionnaires to demanding detailed evidence of specific security controls before binding coverage — and they have become much more willing to deny claims when the controls that were attested to during underwriting were not actually in place when the incident occurred.

For organizations treating cyber insurance as a financial backstop, the new market reality requires more attention than the old one did. The policy terms matter, the security control requirements matter, and the alignment between what your security program actually does and what your insurance program assumes it does has become a material risk consideration in its own right.

## How the Market Got Here

The early years of cyber insurance were characterized by relatively loose underwriting and broad coverage. Carriers had limited claims data, premiums were modest relative to potential payouts, and the market was competitive enough that policyholders could shop for favorable terms. Then came the ransomware era. Loss ratios across the cyber insurance industry exceeded 100 percent in some years as ransomware payouts, business interruption claims, and recovery costs accumulated faster than premium income.

Carriers responded by raising premiums sharply, lowering coverage limits, narrowing the scope of insured perils, and substantially increasing the rigor of underwriting. The pre-binding security questionnaire grew from a handful of yes/no questions to detailed assessments covering MFA deployment, EDR coverage, backup architecture, privileged access controls, vulnerability management, security awareness training, and incident response readiness. Carriers began requiring evidence rather than just attestation — screenshots of MFA configuration, samples of backup test results, EDR coverage reports.

The market has stabilized somewhat in recent years, but the rigorous underwriting environment has not relaxed. If anything, it has continued to tighten. The implication for security and risk leaders is straightforward: cyber insurance is now a control-based product where the quality and accuracy of your security program determines both what coverage you can obtain and whether claims will actually be paid when something happens.

## What Cyber Policies Actually Cover

Cyber insurance policies are heterogeneous, and the coverage terms vary substantially across carriers and product lines. A useful mental model groups coverages into a few major categories: first-party costs (your own losses from an incident), third-party liability (claims from others affected by your incident), and various specialty coverages that may or may not be included by default.

First-party coverage typically includes incident response costs (forensics, legal counsel, notification, credit monitoring), business interruption losses (lost revenue while systems are down), data restoration costs, and ransom payments. The specifics matter enormously here. Business interruption coverage may have a waiting period before coverage starts (often 8 to 12 hours, sometimes longer), specific definitions of when the interruption clock starts and stops, and limitations on what losses qualify. Ransom payment coverage may require specific carrier approval before payment is made and may exclude payments to sanctioned entities, which the carrier expects you to verify.

Third-party liability coverage addresses claims brought against you by parties affected by the incident — customers whose data was exposed, business partners affected by the breach, regulators imposing fines. The scope of this coverage and its limits often dwarf the first-party coverage, both in policy limits and in the scenarios where it matters most. A breach affecting a hundred thousand customers can produce regulatory fines and class action exposure that exceed almost any first-party loss scenario.

Specialty coverages — social engineering and funds transfer fraud, regulatory defense and fines, reputational harm, contingent business interruption from third-party provider incidents — are increasingly important and are not always included in standard policies. Social engineering and funds transfer fraud coverage in particular is worth examining carefully because business email compromise and invoice fraud incidents are common, and the coverage often has lower sublimits than the headline policy limit, with specific control requirements before the coverage applies.

## The Control Requirements That Determine Coverage

The pre-binding control requirements have become the most important thing for security teams to understand about their organization's cyber insurance. These requirements vary by carrier and policy, but a representative list of what carriers commonly require evidence of: MFA on email, remote access, and privileged accounts; endpoint detection and response coverage on a high percentage of endpoints; immutable or air-gapped backups with regular tested restores; privileged access controls including separation of administrative and ordinary use accounts; vulnerability management with defined SLAs for critical patches; security awareness training; incident response plans with regular exercises.

The critical word is "evidence." Carriers increasingly want documentation that demonstrates the control is actually in place — coverage reports showing MFA enrollment percentages, EDR coverage maps, backup test logs, incident response plan documents with exercise dates. Attestation alone is not sufficient, and carriers have demonstrated willingness to investigate and challenge attestations when claims are filed.

The risk that often goes underappreciated is the misalignment between what was attested during underwriting and what is actually in place. Security programs change continuously, and a control that was deployed when the policy was bound may have degraded or been disabled by the time an incident occurs. If the post-incident investigation reveals that the controls attested to in underwriting were not actually in place, claim denial becomes a real possibility. We have worked with organizations that discovered this gap in the most painful possible way: filing a multi-million-dollar claim that was denied because their MFA coverage had drifted below the policy threshold.

The implication is that cyber insurance underwriting attestations should be treated as an ongoing program commitment, not a one-time disclosure. The controls listed in the application are commitments to maintain those controls throughout the policy period, and tracking whether they remain in place is a security program responsibility.

## Where Coverage Gaps Hide

Even policies that look comprehensive often have gaps that become important during an actual incident. A few that we see consistently misunderstood:

Acts of war and state-sponsored attack exclusions have expanded since the Merck v. Ace American decision and the broader pattern of carrier pushback on nation-state-attributed incidents. The exact wording matters — some exclusions are narrowly drawn around clear acts of war, while others are broad enough to potentially exclude any incident attributed to a nation-state actor. Given that significant portions of major cyber incidents are attributable to state-affiliated groups, the breadth of this exclusion can substantially affect actual coverage.

Sublimit structures often surprise policyholders during claims. The headline policy limit may be $10 million, but specific coverages within the policy may have much lower sublimits. Social engineering fraud may have a $250,000 sublimit. Regulatory defense and fines may have a $1 million sublimit. Contingent business interruption may have a sublimit lower than direct business interruption. The sublimits, not the headline limit, are what apply to specific incident types, and misunderstanding this can produce material underinsurance.

Vendor and third-party scenarios have become an area of significant coverage variation. If your incident is caused by a third-party service provider's compromise — a managed service provider, a SaaS vendor, a supply chain partner — does your policy cover the resulting impact to your organization? Some policies cover this scenario robustly, others have specific exclusions for third-party-caused incidents, and others require specific endorsements.

Pre-existing conditions and known vulnerabilities have become a focus of carrier scrutiny. If a vulnerability that was identified in a vulnerability scan months before the incident was the entry point used by attackers, carriers may argue that the loss was foreseeable and excluded under known-event clauses. Organizations with substantial unremediated vulnerability backlogs that they were aware of through their own scanning programs face particular exposure to this category of denial.

## Aligning Security Programs with Insurance Requirements

The practical implication of all of this is that cyber insurance and the security program are connected in ways that did not exist a decade ago. Insurance underwriting drives security control investments because the controls are required to obtain coverage. Security program decisions affect what coverage is available and how claims will be handled. Treating insurance and security as separate programs that occasionally interact misses the integration that the current market requires.

A few practices we recommend for organizations that want to use cyber insurance as a genuine risk transfer tool:

Maintain documentation that demonstrates the controls attested to in underwriting are actually in place — not just at the time of binding, but throughout the policy period. The MFA coverage report you provided during underwriting should be regenerable today and should still show similar coverage. If it does not, the gap needs to be addressed before it produces a denied claim.

Map your insurance policy's coverage terms against the realistic incident scenarios that concern your organization. If your most concerning scenario is ransomware encrypting your production environment, walk through how the policy responds to that scenario specifically — what is covered, what is excluded, where the sublimits apply, what evidence will be required from your security program. Discovering coverage gaps during this exercise is far less painful than discovering them after an incident.

Build the relationship with your carrier and broker as a working partnership rather than a transactional one. Many carriers offer pre-incident services — tabletop exercises, security control assessments, breach coach relationships — that provide real value if you engage with them. The post-incident relationship is also smoother when the carrier understands your environment and security program through ongoing engagement rather than encountering it for the first time during a claim.

## Cyber Insurance as One Layer in Risk Management

Cyber insurance is a financial control, not a security control. It does not prevent incidents, and it does not substitute for the security investments that reduce incident likelihood and impact. Treated as a complement to a serious security program, it provides genuine value as a tail-risk hedge against incidents whose costs would otherwise be devastating. Treated as a substitute for security investment — "we have insurance, so we do not need to spend on this control" — it produces both worse security outcomes and likely uncollectible claims when incidents occur.

The good news is that the rigor the insurance market has introduced into security control verification has accelerated security improvements at many organizations. Controls that the security team had been advocating for years often get prioritized once the insurance carrier requires them. Used as a forcing function for control deployment, the insurance program can drive real security improvement even before its financial protection is needed.

The thing to avoid is the worst-of-both-worlds outcome: substantial premiums paid for coverage that will not actually pay out due to control gaps, alongside security investment that has been deferred because the insurance was supposed to cover it. Aligning the two programs intentionally — using insurance to drive control deployment, maintaining the controls to preserve coverage, and understanding the policy well enough to know what is and is not actually transferred — is what makes cyber insurance a useful risk management tool rather than an expensive false comfort.
