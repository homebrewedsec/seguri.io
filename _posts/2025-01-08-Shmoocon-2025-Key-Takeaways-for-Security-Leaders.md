---
layout: single
title: "Shmoocon 2025: The End of an Era and What Comes Next"
toc: true
excerpt: "After 20 years, Shmoocon feels different. The scrappy hacker conference has grown up, and so has the security industry. Here are the key insights from what might be the last Shmoocon as we knew it."
header:
  overlay_image: /assets/images/post-shmoocon-2025.jpg
  teaser: /assets/images/post-shmoocon-2025.jpg
  overlay_filter: 0.5
---

Walking into Shmoocon 2025 felt different. Maybe it was the corporate sponsors that now dominate the vendor area, or the fact that half the attendees were wearing company polo shirts instead of hacker t-shirts. After two decades, the scrappy conference that started in a hotel ballroom has evolved into something unrecognizable from its origins.

This might be the end of Shmoocon as we knew it – and maybe that's not entirely a bad thing. The security industry has matured, and so have the challenges we face. Here's what this year's conference revealed about where we've been and where we're heading.

## The AI Security Reality Check

### It's Not About AI Attacks (Yet)

Despite the conference buzz around AI-powered attacks, the real AI security challenge for most organizations is much more mundane: **employees using AI tools without oversight**. 

**What we're seeing:**
- Developers copying sensitive code into ChatGPT for debugging
- Sales teams uploading customer data to AI writing assistants
- Finance teams using AI tools to process confidential documents
- HR teams using AI for resume screening without bias auditing

**Immediate actions:**
1. **Inventory AI tool usage** across your organization (spoiler: it's higher than you think)
2. **Create AI usage guidelines** that focus on data classification, not tool prohibition
3. **Implement DLP controls** that catch sensitive data heading to AI services
4. **Train teams** on AI-safe practices rather than blanket bans they'll ignore

### The Supply Chain AI Challenge

Multiple talks highlighted a growing concern: AI in your software supply chain. We're not talking about malicious AI attacks, but the reality that AI-generated code is now everywhere in your dependencies.

**Key risks:**
- AI-generated code with subtle security vulnerabilities
- Dependencies created by AI without proper security review
- Open source projects increasingly using AI for contributions
- Code review processes that aren't designed to catch AI-generated issues

**Practical response:** Update your software composition analysis to flag dependencies with high rates of AI-generated contributions and prioritize security testing for these components.

## The Authentication Evolution

### Passkeys Are Ready (Finally)

This was the year Shmoocon really embraced passkeys as a practical solution, not just a future promise. The implementation stories were compelling, and the user adoption data was better than expected.

**Why now:**
- Major platforms (Apple, Google, Microsoft) have stable implementations
- Enterprise identity providers are offering seamless integration
- User experience is actually better than password+MFA for most use cases
- Phishing resistance is proving valuable against sophisticated attacks

**Implementation strategy:**
1. **Start with high-risk users** (executives, IT admins, finance team)
2. **Pilot with tech-savvy departments** to work out deployment kinks
3. **Maintain password fallbacks** during transition period
4. **Focus on mobile-first deployment** where passkey experience is best

### MFA Fatigue is Breaking Security

Several presentations highlighted what we're seeing in client environments: MFA fatigue is leading to worse security outcomes, not better ones.

**The problem:**
- Users approve MFA prompts without reading them
- Repeated prompts lead to automatic approval behavior
- MFA bombing attacks are increasingly successful
- Help desk is disabling MFA for "productivity reasons"

**Better approaches:**
- **Risk-based authentication** that reduces prompts for trusted contexts
- **FIDO2 tokens** for high-risk users who need frequent authentication
- **Conditional access policies** that eliminate redundant prompts
- **User education** focused on when NOT to approve MFA requests

## Network Security Realities

### Zero Trust Isn't What You Think It Is

The most honest zero trust discussion we've heard focused on what zero trust actually looks like in practice versus the marketing vision.

**Reality check:** Zero trust isn't a product you buy – it's an architecture principle that requires fundamental changes to how you design and operate networks.

**Practical zero trust starts with:**
1. **Network segmentation** that actually limits lateral movement
2. **Identity-based access controls** rather than network-based permissions
3. **Continuous monitoring** of user and device behavior
4. **Assumption that perimeter security has already failed**

**Don't start with:** Expensive zero trust platforms that promise to solve everything. Start with segmentation and identity – the platforms can come later.

### The Insider Threat You're Missing

The most sobering talk was about insider threats that aren't malicious – they're just careless. The data was striking: most insider-caused breaches involve employees who thought they were following policy.

**Key insight:** Your insider threat program probably focuses too much on detecting malicious insiders and not enough on preventing accidental ones.

**Practical improvements:**
- **Just-in-time access** that reduces standing privileges
- **Data classification** that makes sensitive data obvious
- **Behavior analytics** focused on unusual data access patterns
- **Regular access reviews** that remove unused permissions

## Supply Chain Security Gets Real

### Software Bills of Materials (SBOMs) Are Ready

Multiple vendors demonstrated practical SBOM implementations that go beyond compliance checkboxes. The tooling has matured to the point where SBOMs can actually improve security decision-making.

**What's working:**
- Automated SBOM generation integrated into CI/CD pipelines
- Vulnerability correlation across the entire software supply chain
- License compliance automation that reduces legal risk
- Dependency analysis that identifies high-risk components

**Implementation priority:** Start with your most critical applications and work backward. Focus on actionable insights, not comprehensive coverage.

### The Third-Party Risk Management Evolution

The old approach of annual vendor questionnaires and compliance checklists is dead. Organizations are moving toward continuous monitoring and risk-based vendor management.

**New approaches:**
- **Real-time security monitoring** of critical vendors
- **Incident response integration** that includes vendor notification
- **Risk scoring** based on actual security posture, not questionnaires
- **Contractual requirements** for security incident disclosure

## Cloud Security Maturation

### Multi-Cloud is the Default, Not the Exception

The conversation has shifted from "should we do multi-cloud?" to "how do we secure multi-cloud effectively?" Most organizations are running workloads across multiple cloud providers, whether intentionally or through acquisitions and shadow IT.

**Security implications:**
- **Consistent security policies** across different cloud platforms
- **Cross-cloud identity and access management**
- **Unified monitoring and incident response**
- **Data governance** across multiple cloud environments

**Practical approach:** Focus on standards and automation rather than trying to make every cloud look the same.

### Container Security Beyond Vulnerability Scanning

Container security discussions have evolved beyond image scanning to focus on runtime security, supply chain integrity, and operational security practices.

**Key areas:**
- **Runtime threat detection** for containerized applications
- **Supply chain security** for container images and base layers
- **Secrets management** for containerized environments
- **Network security** for container-to-container communication

## The Threat Landscape Reality

### Nation-State Attacks Are Becoming Commodity

Multiple presentations highlighted how nation-state attack techniques are rapidly becoming available to lower-tier attackers. The time between nation-state innovation and commodity availability is shrinking.

**Defense implications:**
- **Assume advanced persistent threat capabilities** in your threat model
- **Focus on fundamental security controls** that work against sophisticated attacks
- **Improve detection and response capabilities** rather than just prevention
- **Plan for long-term compromise scenarios**

### Ransomware Evolution Continues

Ransomware continues to evolve, with attackers focusing more on data theft and business disruption rather than just encryption.

**New patterns:**
- **Data theft before encryption** to increase pressure for payment
- **Supply chain targeting** to maximize impact
- **Cloud environment attacks** using legitimate administration tools
- **Social engineering** to gain initial access rather than technical exploits

## What This Transition Means for Security Programs

The corporate takeover of Shmoocon reflects broader changes in cybersecurity. Here's what organizations should focus on as the industry matures:

### Quarter 1 Priorities
1. **AI usage governance** - Policy and technical controls for AI tool usage
2. **Passkey pilot program** - Start with high-risk users
3. **Network segmentation assessment** - Understand your current lateral movement risk

### Quarter 2 Priorities
1. **SBOM implementation** - Focus on critical applications
2. **Multi-cloud security standards** - Consistent policies across platforms
3. **Insider threat program refresh** - Focus on accidental threats

### Quarter 3-4 Priorities
1. **Zero trust architecture planning** - Start with identity and access
2. **Container security maturation** - Beyond vulnerability scanning
3. **Third-party risk management automation** - Move beyond questionnaires

## The Bottom Line

Shmoocon 2025 reinforced a key theme: the most effective security programs focus on practical implementations of proven concepts rather than chasing the latest shiny objects. The organizations succeeding in 2025 are those that execute fundamentals well while gradually adopting emerging technologies.

The threat landscape is evolving rapidly, but the core security principles remain the same: know your assets, control access, monitor for anomalies, and respond quickly to incidents. The tools and techniques are getting better, but success still comes down to execution.

## What's Next?

Pick one or two items from the priority list above and focus on implementation over the next 90 days. The conference insights are only valuable if you act on them.

If you need help translating conference learnings into practical security program improvements, [let's talk](https://seguri.io/contact). We specialize in taking the latest security research and making it work in real organizational environments – with real budgets, real politics, and real operational constraints.

The security landscape is evolving fast, but you don't have to navigate it alone.