---
layout: single
title: "API Security: The Often-Overlooked Attack Surface"
toc: true
excerpt: "APIs have quietly become the dominant attack surface in modern applications, yet most organizations still assess them as an afterthought during web application testing. This post covers how to build a systematic API security program — from discovery and inventory through authentication hardening and runtime monitoring."
header:
  overlay_image: /assets/images/post-appsec-start.jpg
  teaser: /assets/images/post-appsec-start.jpg
  overlay_filter: 0.5
---

If you've read a breach disclosure in the last three years, there's a reasonable chance an API was involved. Not always as the headline vulnerability, but as the mechanism through which data left the building — an unauthenticated endpoint someone forgot about, a rate-limiting gap that let an attacker enumerate account records, a legacy mobile backend that never got the same hardening attention as the main web app. APIs have become the connective tissue of modern software, and that means they've also become the connective tissue of modern attacks.

The frustrating part is that most organizations know this. Security teams understand that APIs are important. The problem is operational: API security tends to fall between the cracks of the web application testing program (which focuses on browser-rendered interfaces), the network security program (which doesn't go deep into HTTP semantics), and the software development lifecycle (where security review often ends at the front-end and never touches the API layer systematically). Building a serious API security program means closing those gaps deliberately.

## Start With Discovery: You Can't Secure What You Don't Know About

The most common finding in API security assessments isn't a sophisticated vulnerability — it's undocumented, untracked endpoints that nobody claimed ownership of. Shadow APIs, legacy endpoints left running after feature deprecation, internal APIs accidentally exposed to the internet, third-party integrations that spun up their own API surface without telling anyone. Before you can assess security posture, you need an accurate inventory.

Discovery should work from multiple angles simultaneously. Start with what you know: gather OpenAPI/Swagger specifications, internal API catalogs, service mesh documentation, and developer portals. Then verify against reality by crawling traffic logs from your WAF, load balancers, and API gateways. What's documented and what's actually receiving traffic frequently diverge. Tools like Postman's API catalog, Kong's discovery features, or dedicated API security platforms can help correlate the two.

Don't forget mobile applications. Decompiling your own iOS and Android apps is a legitimate and frequently revealing exercise — mobile apps often reference API endpoints that never appeared in any internal documentation because they were built by a different team or a third-party vendor. Similarly, browser developer tools and intercepting proxies used against your own web applications will surface API calls that a traditional application inventory would miss.

The output of this phase should be a living API inventory: endpoint paths, HTTP methods, authentication requirements, data classifications for what each endpoint handles, and ownership. Ungoverned APIs are unacceptable — every endpoint needs an owner who is responsible for its security posture.

## Authentication and Authorization: Where Most API Vulnerabilities Live

Once you have an inventory, the most productive place to focus initial hardening effort is authentication and authorization. OWASP's API Security Top 10 leads with Broken Object Level Authorization (BOLA) and Broken Authentication for good reason — these categories account for an outsized share of real-world API breaches.

BOLA vulnerabilities occur when an API accepts a user's request to access a resource without verifying that the user actually owns or has rights to that specific resource. The classic pattern: `GET /api/orders/12345` returns an order, but the API only checks whether the caller is authenticated, not whether they own order 12345. An attacker who is authenticated as any user can enumerate order IDs and read anyone's records. Fixing this requires consistent server-side ownership validation on every object-level operation, not just checking for a valid token.

Authentication hardening covers several dimensions. Token hygiene — short expiration windows, rotation on privilege escalation, revocation mechanisms that actually work — matters more in API contexts than web contexts because API tokens tend to live longer and travel more places. OAuth 2.0 implementation quality varies enormously; pay particular attention to redirect URI validation, state parameter use, and whether your authorization server correctly enforces scope limitations. Machine-to-machine authentication using mutual TLS or signed request schemes provides stronger guarantees than shared secrets or long-lived API keys for high-value integrations.

Rate limiting and resource-level controls are authentication's overlooked sibling. An API that correctly authenticates every request but allows unlimited calls to a credential-checking endpoint is still providing free credential stuffing infrastructure. Implement rate limits at the API gateway layer, enforce them per-user rather than globally, and design them to degrade gracefully rather than hard-fail when limits are approached.

## Input Validation and Injection: Applying Secure Coding Principles at Scale

APIs don't escape the fundamentals of secure coding — they just apply them at a different layer. Injection vulnerabilities (SQL, command, LDAP, XML) remain relevant wherever API inputs flow into backend systems without proper sanitization. The API surface area is often wider than developers realize: path parameters, query strings, request headers, and JSON/XML body fields all represent injection opportunities.

Schema validation at the API gateway or application layer is a high-leverage control. If your OpenAPI specification defines a field as an integer between 1 and 100, enforce that constraint before the request reaches your application code. Strict schema validation blocks entire classes of injection attacks, mass assignment vulnerabilities (where attackers add unexpected fields to requests), and data type confusion exploits. It also produces better error messages for legitimate developers, which is a secondary benefit worth noting when making the business case to development teams.

Mass assignment deserves specific attention because it's consistently underestimated. When a REST API accepts a JSON object and maps it directly to a database model, an attacker who knows your data model can add fields to their request payload that your application will happily process — including things like `isAdmin: true` or `accountBalance: 99999`. Explicit allow-listing of accepted fields, rather than blocking-listing dangerous ones, is the correct mitigation pattern.

## Runtime Monitoring and Anomaly Detection

Static security testing — code review, DAST scanning, penetration testing — finds vulnerabilities before they're exploited. Runtime monitoring catches exploitation attempts against vulnerabilities you haven't found yet, detects abuse patterns that aren't vulnerabilities per se, and provides the visibility needed to respond when something goes wrong.

API-specific runtime monitoring looks different from traditional application monitoring. You're not just watching for errors and latency; you're watching for behavioral patterns: a user who normally calls 10 endpoints per day suddenly calling 10,000, requests for object IDs that follow sequential enumeration patterns, authentication attempts that fan across many usernames from a single IP, unusual response volumes that suggest bulk data extraction. These signals are often invisible in generic APM tools but are straightforward to detect in purpose-built API security monitoring platforms or with custom analytics on API gateway logs.

Log completeness is a prerequisite. API logs should capture full request URLs including query parameters, request body hashes or select fields (avoid logging sensitive data verbatim), response status codes and body sizes, authentication identifiers, and source IPs. Many organizations discover during incident response that their API logs didn't capture enough context to reconstruct what an attacker accessed — by then it's too late.

## Integrating API Security Into the SDLC

Runtime monitoring and periodic assessments will catch things, but they're inherently reactive. Sustainable API security requires integrating security requirements into the development lifecycle so APIs are built more securely from the start.

This means adding API security to your threat modeling process for new features — when a team designs a new API endpoint, asking explicitly: who can call this, what can they access, what happens if someone sends unexpected inputs? It means including API security test cases in your CI/CD pipeline, either through automated DAST tools that can authenticate to APIs and fuzz inputs, or through a library of security-focused integration tests that developers maintain alongside functional tests.

Documentation requirements also matter. Requiring OpenAPI specifications as a deliverable for any new API surface isn't bureaucracy — it's a forcing function that makes implicit security assumptions explicit and gives your security tooling something to validate against. APIs without specs are significantly harder to test, monitor, and assess.

If you're building out a broader application security program, the API security program fits naturally as a track within it. See our post on [starting your application security program](/blog/starting-your-application-security-program/) for the broader context on how API security integrates with code review, dependency management, and security testing infrastructure.

## Getting Started: Prioritizing the Work

If you're starting from zero or trying to move a nascent API security effort forward, the sequencing matters. Discovery and inventory first — you need the map before you can plan the route. Authentication and authorization hardening second — this is where the highest-impact vulnerabilities live and where hardening investment returns the most risk reduction. Schema validation and input controls third, because they're high-leverage and often achievable without significant development effort if you have an API gateway in place. Runtime monitoring in parallel with hardening work, because you want visibility into exploitation attempts even while you're remediating.

Avoid the trap of waiting for a comprehensive program before doing anything. A partial API inventory is better than none. Fixing authentication on your five highest-risk APIs delivers real risk reduction even if the other 95 are still unassessed. Progress matters more than completeness, and the biggest risk in API security programs is often organizational inertia rather than technical complexity.

APIs are not going away, and neither is the attack surface they represent. Building systematic security around them — inventory, authentication, validation, monitoring, and SDLC integration — converts an unmanaged risk into a managed one. That's the goal.
