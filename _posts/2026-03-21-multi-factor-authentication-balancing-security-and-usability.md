---
layout: single
title: "Multi-Factor Authentication: Balancing Security and Usability"
toc: true
excerpt: "Not all MFA is created equal — SMS OTP offers basic protection while phishing-resistant FIDO2 passkeys represent a qualitatively stronger security guarantee. This post guides security teams through selecting, deploying, and sustaining MFA in ways that reduce risk without eroding user adoption."
header:
  overlay_image: /assets/images/post-iam-assessments.jpg
  teaser: /assets/images/post-iam-assessments.jpg
  overlay_filter: 0.5
---

Multi-factor authentication was supposed to solve the password problem. And for years, security teams cited MFA as the single most impactful control available — until attackers adapted. Today, MFA bypass is a standard technique in the adversary playbook: SIM swapping to intercept SMS codes, adversary-in-the-middle proxies that relay real-time OTPs, MFA fatigue attacks that bomb users with push notifications until they approve one to make it stop. The control is still valuable, but "we have MFA" is no longer a complete answer.

The more nuanced reality is that MFA exists on a spectrum from "marginally better than nothing" to "genuinely resistant to modern phishing and credential theft attacks." Where your MFA program falls on that spectrum depends on the authentication factors you deploy, how you handle exceptions and fallback options, and whether your users understand what they are being asked to authenticate. Getting this right requires both technical rigor and a serious commitment to usability.

## Understanding the MFA Spectrum

The weakest broadly-deployed MFA is SMS one-time passwords. The authentication channel (the phone number) is controlled by a carrier, not by your identity platform, which means SIM swapping — convincing the carrier to transfer a number to an attacker-controlled SIM — can compromise it without any involvement from your systems. SMS OTP also does not protect against real-time phishing: if an attacker can convince a user to enter their username and password on a fake site, they can relay those credentials to the real site and simultaneously prompt the user to enter the OTP they receive, completing authentication as the user.

Time-based one-time passwords (TOTP) via authenticator apps are a step up. They eliminate the carrier control problem, since the shared secret lives in the authenticator app rather than being delivered via SMS. But they share the real-time phishing vulnerability — an attacker-in-the-middle proxy can relay TOTP codes as quickly as SMS codes. They also introduce usability friction: users must manually enter a code that expires every 30 seconds, which leads to errors, frustration, and pressure to create exception processes.

Push authentication — where the user approves a notification on their phone rather than entering a code — removes the manual entry step and generally improves the user experience. It does not solve the phishing problem, and it introduces MFA fatigue: a class of attack where the attacker triggers repeated authentication prompts until the exhausted or confused user approves one. Microsoft's Authenticator app introduced number matching and additional context display (showing the application being accessed and the approximate login location) specifically to counter this attack pattern, and those features meaningfully reduce the risk.

FIDO2 and passkeys represent a qualitative security improvement over all OTP-based methods. The authentication is bound to the specific site being accessed at the cryptographic level — if you are on a phishing site, the passkey will not authenticate because the origin does not match the registered relying party. There is no shared secret to steal, no code to relay, and no user interaction that can be tricked. FIDO2 hardware security keys have been used by organizations like Google internally since 2017 with zero reported account takeovers via phishing since adoption. Passkeys, which extend FIDO2 to device authenticators like biometrics, bring the same security guarantee to a form factor most users are already comfortable with.

## Selecting the Right Factors for Your Environment

The right MFA strategy is not "deploy the strongest factor everywhere" — it is matching authentication requirements to risk levels while maintaining adoption. A field technician logging into a work order system has different risk and usability constraints than a finance executive accessing accounts payable systems or an administrator managing your identity platform.

Risk tiering should drive factor requirements. A reasonable framework: standard users accessing productivity applications can use push MFA with number matching; users accessing sensitive data, financial systems, or HR platforms require FIDO2-capable factors; privileged users and administrators require hardware security keys as the non-negotiable baseline. This graduated approach concentrates the cost and complexity of stronger factors where they provide the most protection.

Hardware security key deployment is often cited as expensive and operationally complex, but the economics have improved substantially. YubiKey and similar devices cost $25–$50 per unit, and the total cost of a key program for administrative users is typically manageable when weighed against the breach cost it prevents. The operational complexity — registration, backup key provisioning, lost key procedures — is real and needs planning, but organizations that have managed this process describe it as less disruptive than anticipated once the initial rollout is complete.

Passkeys are increasingly viable for enterprise environments and represent the best path to broad phishing-resistant adoption. Microsoft Entra ID, Okta, and Ping Identity all support passkey authentication. The device-bound nature of passkeys raises questions about multi-device workflows that are worth working through before broad deployment, but the trajectory is clearly toward passkeys as the enterprise standard for user-facing phishing-resistant MFA.

## Legacy Application Challenges and Risk-Based Authentication

The realistic picture in most enterprise environments includes applications that do not support modern authentication. Legacy on-premises applications may only support basic username and password authentication. Vendor-hosted systems may have MFA options that are two generations behind current standards. This is where the gap between policy and reality lives, and it deserves honest assessment rather than paper coverage.

For applications that cannot support direct MFA, proxy authentication through a capable identity provider is often the answer. Placing the application behind an identity-aware proxy that enforces MFA before allowing access preserves the protection even when the application itself does not support it. This approach works well for web-accessible applications; it is more complex for thick-client applications and may not be feasible for some legacy systems.

Risk-based authentication — dynamically adjusting authentication requirements based on contextual signals — is a middle path that can improve both security and usability. When a user authenticates from a recognized device, on a known network, at a normal time, from their usual location, the risk profile is low and strong MFA may be waived in favor of a simpler interaction. When signals are anomalous — unknown device, unusual location, new browser, unexpected time — stronger authentication is required. Platforms like Okta Adaptive MFA, Microsoft Entra Conditional Access, and Duo's Risk-Based Authentication implement this model, and the usability improvement is significant: users completing routine tasks from familiar contexts encounter less friction, while unusual access attempts face stronger scrutiny.

The risk with risk-based authentication is in the risk model itself. If the signals used to assess risk are too coarse, too easily manipulated, or poorly calibrated to your environment, the adaptive policy becomes an attack surface. VPN usage, managed device enrollment status, and IP reputation are reasonable signals. Geolocation alone is not — it is easily spoofed and generates false positives from legitimate travel.

## Handling MFA Exceptions Without Creating Back Doors

Every MFA exception is a potential back door, and exception management is where many otherwise-good MFA programs fail. The most common failure mode: an IT department maintains a list of "MFA-exempt" accounts for operational convenience, that list grows over time, and the accounts on it are exactly the ones attackers target because they have neither the friction of MFA nor the scrutiny of privileged account monitoring.

Service accounts are the most common source of MFA exemptions. These accounts often cannot interactively approve a push notification, so they get exempted from MFA policy. The right answer is not exemption — it is replacing password authentication for service accounts with certificate-based authentication or managed identity systems that do not require any interactive authentication factor at all. Azure Managed Identities, AWS IAM roles for EC2 instances, and Kubernetes service account tokens all eliminate the credential problem rather than working around it.

Break-glass procedures — emergency access processes for situations where the normal authentication flow fails — require particularly careful design. A break-glass account that bypasses MFA entirely defeats the purpose of having MFA. Better design: break-glass accounts require hardware key authentication with significantly stronger access controls and monitoring, and every use triggers immediate security team notification and mandatory post-use review.

Fallback options deserve scrutiny. If your primary MFA method is a FIDO2 key but users can fall back to SMS OTP when the key is unavailable, attackers who know this will target the fallback path. The fallback should be at least as strong as the risk associated with the system being protected — for high-value systems, there is no fallback; you authenticate with the required factor or you contact the helpdesk for a supervised recovery process.

## Deployment, Adoption, and Ongoing Management

The best-designed MFA program fails if users route around it, demand exemptions, or disable it through social engineering. Adoption is as important as architecture, and it requires deliberate attention throughout rollout and sustainment.

Phased rollout gives the organization time to address friction before it becomes resistance. Start with a pilot group of technically comfortable users who can provide structured feedback. Use their experience to identify integration gaps, usability issues, and edge cases in your deployment before rolling out to the broader population. Communicate clearly and repeatedly about why MFA is being implemented, what users will experience, and where to get help. The organizations that deploy MFA smoothly treat it as a change management exercise, not just a technical project.

Helpdesk training and tooling matter. If the helpdesk is not equipped to handle MFA-related support requests efficiently, or if account recovery requires bypassing MFA, you will face pressure to create exceptions that undermine the program. Invest in helpdesk enablement, clear escalation paths, and documented recovery procedures before launch, not after.

Our work on [IAM security assessments](/blog/identity-access-management-security-assessments/) consistently finds that MFA coverage looks complete on paper but has meaningful gaps in practice — specific applications excluded, exception lists that have grown out of control, or fallback paths that are more accessible than the primary authentication method. Regular MFA posture reviews should be part of your identity security program, not a one-time deployment validation.

## Measuring MFA Program Effectiveness

Deployment coverage — percentage of accounts with MFA enrolled — is a necessary metric but not a sufficient one. You also need to know how often MFA is being successfully challenged, how often it is being bypassed or exempted, what the breakdown of MFA methods in use is, and whether your phishing-resistant factor coverage extends to your highest-risk user populations.

Track MFA fatigue indicators: unusual spikes in push notifications, approval patterns at unusual hours, or patterns suggesting users are approving prompts without reading them. These signals can indicate active attacks or habituation that undermines the control. If your push MFA platform logs individual prompt-to-approval times, monitoring for very fast approvals can identify users who are rubber-stamping notifications rather than actually verifying them.

MFA is a significant investment and a genuine security control when implemented well. The key is being honest about which factors you have deployed, which attack techniques they resist, and where the gaps are. An organization with SMS OTP MFA deployed broadly should understand that they are protected against opportunistic credential stuffing but not against targeted phishing. An organization deploying FIDO2 universally for administrative access has a qualitatively different risk profile. Know where you are on the spectrum — and have a plan to move toward the stronger end of it.
