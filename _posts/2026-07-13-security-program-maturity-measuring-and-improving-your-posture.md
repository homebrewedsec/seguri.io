---
layout: single
title: "Security Program Maturity: Measuring and Improving Your Posture"
toc: true
excerpt: "Security maturity models give organizations a structured way to assess current capabilities and prioritize improvements — but maturity scores are only valuable if they're connected to actual risk reduction rather than paper compliance. This post covers how to use maturity frameworks practically, avoid common assessment pitfalls, and translate maturity gaps into a defensible roadmap."
header:
  overlay_image: /assets/images/post-meta-blog-series-guide.jpg
  teaser: /assets/images/post-meta-blog-series-guide.jpg
  overlay_filter: 0.5
---

Security maturity models are everywhere — NIST CSF tiers, CMMC levels, C2M2 dimensions, CIS Implementation Groups, vendor-specific frameworks, custom internal scorecards. Used well, they give organizations a structured language for assessing where they are and where they need to go. Used poorly, they become elaborate exercises in self-deception, where teams produce ever-improving maturity scores while their actual risk posture stays the same or gets worse.

We get pulled into a lot of maturity assessments — sometimes as the assessor, sometimes as advisors helping organizations interpret assessments others have produced. The pattern that separates useful assessments from theatrical ones isn't the framework chosen. It's whether the assessment is grounded in evidence, whether the gaps it identifies map to real risks the organization faces, and whether the resulting roadmap is something the organization will actually execute. Frameworks are tools, and like any tool they reward discipline and punish shortcuts.

## Pick a Framework That Matches Your Reality

There's no single best maturity framework, and shopping for one based on which produces the most flattering score is a sign the program isn't ready for an honest assessment. The right framework depends on your industry, your regulatory exposure, your size, and what you're trying to accomplish with the assessment.

NIST CSF works well as a general-purpose framework for organizations that need to communicate maturity broadly across functions and audiences. CMMC is the right choice for defense industrial base organizations regardless of preference. C2M2 is purpose-built for energy sector and adapts well to other operational technology environments. CIS Controls Implementation Groups work well for organizations that want a more prescriptive, controls-level view. Healthcare organizations often combine HHS CPGs with HITRUST. Financial services frequently use FFIEC CAT or NIST CSF mapped to their regulatory expectations.

Pick one and commit to it for at least a few assessment cycles. Switching frameworks frequently makes year-over-year comparison impossible and gives the appearance of progress that may not exist. If multiple frameworks are required for different stakeholders, build the assessment in the most rigorous one and crosswalk to the others rather than running parallel assessments.

## Evidence-Based Scoring Beats Self-Assessment

The single biggest factor in whether a maturity assessment produces useful output is whether scores are backed by evidence or by opinion. Self-assessments where the security team rates its own capabilities against a framework reliably produce optimistic scores — not because the team is dishonest, but because people score what they intend to do or believe they're doing rather than what evidence demonstrates.

Evidence-based scoring requires that each capability score be supported by artifacts: policies that have been reviewed and approved, configurations that have been observed in production, logs that show the control operating as designed, results from tests that exercise the control under realistic conditions. A score of "managed" for vulnerability management isn't supported by the existence of a scanning tool — it's supported by evidence that scans run on schedule, that results flow into a tracking system, that remediation SLAs are met for some defined percentage of findings, and that exceptions go through documented review. If the evidence isn't available, the score isn't supported.

This discipline produces lower scores in the first assessment cycle than organizations are used to seeing, which is uncomfortable but accurate. The lower starting point also makes year-over-year improvement more meaningful, because the improvements are real rather than artifacts of more generous scoring.

## Avoid the Common Assessment Pitfalls

Three pitfalls show up in almost every maturity program we review. The first is scoring against intent rather than execution. Policies that exist on paper but aren't followed in practice don't represent maturity at the level the policy describes — they represent maturity at the level of execution, which is often considerably lower. Assessment teams need to look past the policy library and into the operational reality.

The second is averaging away the gaps. Maturity scores rolled up to a single number for the organization conceal enormous variation between business units, geographies, and technology stacks. An organization with strong identity controls in its corporate environment and weak controls in a recently acquired subsidiary doesn't have a uniform maturity level — it has two very different environments that need different treatment plans. Roll-ups have their place for executive communication, but the working assessment needs to preserve the variation.

The third is conflating compliance with maturity. Compliance frameworks measure adherence to specific requirements, often using minimum-bar definitions. Maturity frameworks measure capability across a continuum. An organization can be fully compliant with a regulation while sitting at the lower end of the maturity continuum, and an organization can be highly mature while still having compliance gaps. Conflating the two leads to assessments that overstate maturity in compliance-heavy areas and understate it in areas the regulation doesn't address. We address this same dynamic in [security metrics that actually drive improvement](/blog/security-metrics-that-actually-drive-improvement/) — measurement that conflates activity with outcomes leads to programs that look healthy on paper while struggling in reality.

## Connect Maturity Gaps to Risk

A maturity gap is interesting only insofar as it represents a risk the organization should be reducing. Programs that present gap lists in framework order — every score below target, regardless of significance — produce roadmaps that diffuse limited resources across capabilities of varying importance. Programs that prioritize gaps based on the risks they leave unaddressed produce roadmaps that actually reduce risk.

The translation work happens after the assessment. For each material gap, the question is what risks the organization carries because the capability isn't more mature, and what specific scenarios become more likely or more impactful as a result. Gaps that affect critical business systems, sensitive data, or regulatory exposure rise to the top. Gaps in capabilities that don't connect to identified risks may not need to be closed at all — sometimes the right answer is accepting the maturity gap because the underlying risk doesn't justify the investment.

This is also where maturity frameworks intersect productively with threat modeling and adversary-informed planning. Maturity tells you where capabilities are weak. Threat modeling tells you where weakness matters. The combination produces a sharper roadmap than either approach alone.

## Build a Roadmap You'll Actually Execute

Maturity roadmaps fail more often than they succeed. The most common failure is a roadmap that promises too much across too many fronts — every capability moving up a level within twelve months, every gap closed by year-end, parallel workstreams that exceed the team's actual capacity. These roadmaps look ambitious in the executive summary and quietly collapse during execution.

Realistic roadmaps prioritize ruthlessly. A reasonable annual goal is moving three to five high-priority capabilities up one level, supported by specific projects, owners, and success criteria. Sequencing matters: capabilities that depend on other capabilities need to be ordered correctly, and foundational capabilities like asset inventory, identity, and logging need to mature before downstream capabilities can be evaluated meaningfully. Capacity needs to be honest — including operational work, incident response, and business support that will inevitably consume time.

The roadmap should also include capabilities that are intentionally not being advanced this cycle, with the rationale documented. This prevents future leadership from looking at the roadmap years later and assuming everything not on it was overlooked rather than deliberately deprioritized. It also forces the current cycle's planning to engage honestly with trade-offs rather than papering over them.

## Reassess on a Cadence That Matches Reality

Annual reassessment is the default cadence, and it works for most organizations. But annual is the maximum useful interval, not the minimum. Organizations going through significant change — major acquisitions, large technology migrations, regulatory changes, post-incident recovery — benefit from more frequent assessment, even if it's targeted to specific capabilities rather than the full framework.

The reassessment isn't just a score update — it's an opportunity to evaluate whether the assumptions behind the previous roadmap still hold. Threats evolve, business priorities shift, and the cost-benefit of specific maturity investments changes over time. A reassessment that mechanically rescores against the same framework without revisiting the underlying strategy misses much of its value. The most useful maturity programs treat each cycle as a planning opportunity, not just an audit.
