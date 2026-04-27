---
layout: single
title: "Building a Security Champions Program: Scaling Security Culture"
toc: true
excerpt: "Security teams are always outnumbered — a security champions program multiplies your reach by developing security-minded practitioners embedded in every development team, business unit, and operations group. This post covers how to design, launch, and sustain a security champions program that produces measurable changes in organizational security behavior."
header:
  overlay_image: /assets/images/post-awareness-metrics.jpg
  teaser: /assets/images/post-awareness-metrics.jpg
  overlay_filter: 0.5
---

Security teams are structurally outnumbered. No matter how well-staffed your security function is, the number of engineers, developers, operations staff, and business users making decisions that affect security posture every day will always exceed your team's capacity to directly supervise or advise them. The traditional response — security policies, annual awareness training, and review checkpoints in development pipelines — creates friction without creating security instincts. People learn to work around security processes they don't understand rather than integrating security thinking into their work.

A security champions program offers a different model. Instead of concentrating security expertise in a central team and distributing it through process gates, you identify and develop security-minded practitioners embedded throughout the organization. These champions become the first point of contact for security questions in their teams, advocates for security practices in their domains, and a two-way communication channel between the security team and the broader organization. Done well, a security champions program is one of the highest-leverage investments a security team can make. Done poorly, it creates a compliance theater layer that satisfies metrics without changing behavior.

The difference between a program that works and one that doesn't comes down to design choices made at the outset.

## Defining What a Champion Actually Does

Before launching a program, you need a clear answer to the question your first recruits will ask: what does this actually involve? The answer to that question determines who you attract and whether the program delivers value.

Security champions are not security police. Their job is not to enforce policy or report violations to the security team. Organizations that position champions that way quickly find that no credible person in their peer group will take the role, and the champions they do recruit become isolated figures that teammates route around. The role should be explicitly positioned as a resource and advocate — someone who helps their team make good security decisions, not someone who monitors for bad ones.

The practical activities of a security champion typically include: serving as a liaison between their team and the central security function, participating in security reviews for projects in their domain, staying current on relevant threats and security techniques, helping teammates understand security requirements and why they exist, and raising security concerns through appropriate channels when they identify risks. The time commitment matters — most effective programs expect two to four hours per week from champions, with occasional heavier involvement during specific project phases or security initiatives.

Be specific about the skills and knowledge you're developing in champions. A development team champion should understand common web application vulnerability classes and how to avoid them in code review, how to interpret SAST tool output, and what the secure development lifecycle looks like in practice. An operations champion should understand network segmentation concepts, patch management principles, and configuration baseline management. The security team's role is to develop those skills, not just designate champions and assume they'll figure it out.

## Recruiting the Right People

The success of a champions program depends heavily on recruiting people who are genuinely interested in security and credible within their peer group. Both qualities matter. A champion who is deeply interested in security but not respected by their team won't be consulted or listened to. A champion who is highly respected but fundamentally uninterested in security won't invest the effort to be useful.

The best champions typically self-identify. Look for people who already ask security questions in code reviews, who mention security considerations in architectural discussions, or who have sought out security training beyond what their organization requires. In many organizations, those people exist but have no structured channel for their security interest — a champions program gives them a community and a role.

Nomination from managers carries risk. Managers sometimes nominate their most available team member rather than their most security-curious one. When you receive manager nominations, validate interest directly — talk to nominees before onboarding them and assess whether they're genuinely interested or just complying with a manager's assignment.

The onboarding conversation should be honest about expectations. Tell candidates exactly how much time the program requires, what the development commitments look like, and what they will and won't be expected to do. Some people will opt out after an honest conversation — that is far better than recruiting people who participate minimally and create the appearance of a program without the substance.

Incentivize participation in ways that matter to the specific champions. For engineers, technical development opportunities, recognition at technical forums, and participation in security assessments are often more motivating than formal compensation. For others, career development recognition, leadership visibility, or explicit connection to performance review criteria may matter more. Understand what motivates the individuals in your program and make sure participation is visibly valued by their management chain.

## Building the Development Curriculum

What you teach champions determines what they're able to do. A common failure mode is treating champions training as a compressed version of security awareness training — a series of informational modules on phishing, password hygiene, and data handling. That's not enough to make someone useful as a security resource for their team.

Champions need domain-specific technical depth. A development team champion needs enough understanding of OWASP Top 10 vulnerability classes to catch issues in code review — not just to recognize that SQL injection is bad, but to identify it in actual code. An IT operations champion needs to understand attack paths through Active Directory and what configuration patterns create risk. An infrastructure champion needs to understand what network segmentation actually accomplishes and how to evaluate whether it's working.

The most effective champions development programs combine structured learning with hands-on practice. CTF (Capture the Flag) challenges, secure code review labs, and tabletop scenarios relevant to the champion's domain are more effective than slide-based training at developing judgment that transfers to real work. If your security team doesn't have the capacity to develop domain-specific technical training, there are high-quality external resources — Secure Code Warrior, HackTheBox, SANS course tracks — that can be curated for different champion roles.

Build a regular rhythm of champions engagement. Monthly or quarterly champions meetings that cover emerging threats, review recent incidents (anonymized), share techniques, and maintain community cohesion are essential. Champions who are recruited, given initial training, and then left without ongoing engagement drift back to their primary role and stop functioning as champions. The community component — the sense of being part of a group with a shared purpose — is often what sustains participation over time.

## Measuring Program Effectiveness

A security champions program without measurement is a program that can't demonstrate its value or identify what needs to improve. Define success metrics before the program launches, not after.

Outcome metrics are more valuable than activity metrics. Knowing that your champions completed twelve training hours this quarter tells you about participation but nothing about impact. Metrics that reflect actual behavior change include: security defect rates in code review for teams with champions versus teams without, time to triage security findings in different business units, participation rates in threat modeling sessions, and the number of security issues raised by champions before they reached the security team's radar. These metrics require baseline data, which is another reason to define your measurement approach at launch.

Track champion retention and engagement over time. Programs that start strong and decay over twelve to eighteen months are common. Understanding why champions disengage — role changes, management pressure, lack of clear purpose, inadequate support from the security team — lets you address those factors before they become program-threatening attrition patterns.

Survey champions and their team members periodically. Champions' perception of the program's usefulness and support level predicts retention. Their team members' perception of whether the champion is a useful resource predicts the program's actual impact on security behavior. Both perspectives are necessary and neither substitutes for the other.

## Sustaining the Program Over Time

Security champions programs that don't have explicit organizational support fade. Champions need visible backing from both the security leadership and their own management chains. If a champion's manager treats security champions activities as interference with primary job responsibilities, that champion will stop being a champion within a few months.

Getting management chain support requires educating managers on what the program does and why it serves their interests. Managers whose teams have a well-developed security champion typically find that security-related delays in their projects decrease — the champion helps the team avoid mistakes that create rework and security review bottlenecks. Making that value visible to managers is an ongoing communication responsibility for the security team, not a one-time explanation at program launch.

Create a career development path for champions who want to grow in the security direction. Some champions will discover through the program that they want to pursue security more seriously — either developing deeper technical specializations or eventually transitioning into security roles. Organizations that can offer that path retain motivated champions longer and build an internal pipeline for security team hiring. Champions who have spent two years embedded in development teams and developing their security skills often make excellent security engineers who retain the operational empathy their background provides.

For teams working on the broader cultural dimensions of security awareness, our post on [building a strong security awareness program](/blog/building-a-strong-security-awareness-program/) covers the foundational awareness and training practices that complement a champions program.

## Connecting Champions to Security Operations

One of the underused benefits of a security champions program is the intelligence channel it creates. Champions embedded in business units and development teams observe things the central security team doesn't see: shadow IT projects under development, third-party integrations being planned without security review, misunderstandings about security policies that are creating workarounds. When champions feel they have a clear and safe channel to surface those observations, the security team gains early warning on risks that would otherwise appear only after they've created problems.

Build explicit mechanisms for champions to report concerns without creating a surveillance dynamic. A clear escalation path — "if you see something that concerns you, here's who to contact and what to say" — combined with an explicit commitment that champions aren't expected to police their colleagues, establishes a functional early warning system. The security team's response to champion-sourced intelligence matters: if concerns are consistently acknowledged and acted on appropriately, champions will continue to surface them. If they're ignored or the champion receives pushback from their team as a result, the channel closes quickly.

Over time, a well-functioning champions network provides the security team with the ground-level awareness they need to make better prioritization decisions. The combination of security team expertise and champions-sourced organizational intelligence is more effective than either alone.
