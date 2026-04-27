---
layout: single
title: "Container and Kubernetes Security: Practical Hardening Guide"
toc: true
excerpt: "Kubernetes has made it easy to deploy and scale applications but has also introduced a sprawling attack surface that most security teams weren't prepared to defend. This post covers the high-impact hardening controls for container environments — from image scanning and least-privilege pod security to runtime detection."
header:
  overlay_image: /assets/images/post-containers-kubernetes.jpg
  teaser: /assets/images/post-containers-kubernetes.jpg
  overlay_filter: 0.5
---

Kubernetes adoption has accelerated faster than most organizations' ability to secure it. The platform abstracts away a lot of infrastructure complexity, which is why development teams love it, but that same abstraction makes the security model genuinely difficult to reason about. When something goes wrong in a Kubernetes environment — a misconfigured pod, a compromised container, a lateral movement chain through a service mesh — the blast radius can be significant and the forensic trail can be difficult to reconstruct.

The good news is that the most impactful Kubernetes security controls aren't exotic. They're the same principles — least privilege, defense in depth, visibility, and patch discipline — applied to a new set of primitives. What's different is the implementation surface: instead of configuring a firewall rule, you're writing a NetworkPolicy; instead of hardening a server image, you're hardening a container image and the Kubernetes node running it. This guide covers the highest-priority hardening work across the stack, from image security through cluster configuration to runtime monitoring.

## Container Image Security: The Foundation

Every workload in your cluster starts with a container image, and the security of that image directly constrains the security posture of everything running on top of it. Image security has three dimensions: what's in the image, how the image is built, and how the image is managed over time.

What's in the image matters enormously. Images built from bloated base images (full Ubuntu or Debian installations) carry hundreds of packages that your application doesn't need, each of which is a potential vulnerability surface. Distroless images, Alpine-based images, or custom minimal base images dramatically reduce this attack surface. For compiled languages like Go or Rust, multi-stage builds let you compile in a full build environment and copy only the final binary into a minimal runtime image. The result is an image that contains exactly what's needed to run the application and nothing else — no shell, no package manager, no debugging tools that an attacker could misuse.

Image scanning should be integrated at build time, not treated as a periodic compliance exercise. Tools like Trivy, Grype, or commercial alternatives integrated into your CI/CD pipeline can catch known CVEs before images reach your registry. Set policies that fail builds on critical or high vulnerabilities with available fixes — the "available fix" qualifier is important because blocking on all vulnerabilities including those with no patch produces alert fatigue and trains developers to ignore the scanner. Your registry should also scan on push and on a schedule, because new CVEs are disclosed against images that were clean when they were built.

Image signing with tools like Cosign, combined with admission controller policies that reject unsigned or untrusted images, closes the gap between a clean CI/CD pipeline and actual cluster workloads. Without this, a developer or attacker who can push to your registry can bypass your pipeline controls entirely.

## Pod Security: Least Privilege at the Workload Level

The Kubernetes pod security model gives you granular control over what containers are allowed to do at runtime — and most clusters have far too permissive defaults. The most dangerous misconfigurations involve privilege escalation paths: containers running as root, pods with `allowPrivilegeEscalation: true`, and workloads with unnecessary Linux capabilities.

Start with the basics. Set `runAsNonRoot: true` and specify a non-zero `runAsUser` in your pod security context. Containers running as UID 0 (root) can cause significantly more damage if compromised — they can read files owned by root on the host filesystem via volume mounts, and in some configurations can escape the container namespace entirely. Set `allowPrivilegeEscalation: false` to prevent processes inside the container from gaining more privileges than their parent process. Drop all Linux capabilities by default and add back only what's explicitly required — most application workloads need zero capabilities.

Read-only root filesystems (`readOnlyRootFilesystem: true`) are worth implementing wherever your application supports it. A container that can't write to its filesystem is significantly harder for an attacker to use as a staging ground — they can't write tools, modify binaries, or persist changes. Applications that need writeable paths can use tmpfs volumes for ephemeral scratch space.

Kubernetes Pod Security Admission (which replaced the deprecated PodSecurityPolicy) enforces these controls at the namespace level using three built-in profiles: privileged, baseline, and restricted. Moving your production namespaces to the `restricted` profile is a meaningful hardening milestone. Start by auditing which workloads would fail under restricted, fix the ones that can be fixed easily, and make an explicit risk decision about the ones that genuinely require elevated permissions.

## RBAC and Secrets Management: Access Controls That Actually Work

Role-Based Access Control in Kubernetes is powerful and frequently misconfigured. The most common patterns we see in assessments: overly broad ClusterRoles granted where namespace-scoped Roles would suffice, service accounts with excessive permissions that no workload actually needs, and default service accounts left with their auto-mounted tokens enabled across all pods.

Audit your RBAC configuration with the perspective of an attacker who has compromised a pod. What can that pod's service account token do? Can it list secrets cluster-wide? Can it create new pods? Can it modify RBAC policies? Tools like `kubectl-who-can` and `rbac-tool` make this audit tractable. For each workload, the service account should have exactly the permissions that workload needs — nothing more. If your application pods don't need to call the Kubernetes API at all, disable service account token auto-mounting (`automountServiceAccountToken: false`).

Secrets management deserves particular attention because Kubernetes native secrets are only base64-encoded, not encrypted, and are stored in etcd without encryption at rest unless you've explicitly configured that. At minimum, enable etcd encryption for secrets. Better: use an external secrets management solution — HashiCorp Vault, AWS Secrets Manager, Azure Key Vault — with a Kubernetes integration that injects secrets as environment variables or files at pod startup without storing the plaintext secret in the cluster itself. This prevents secrets from being readable by anyone with etcd access or sufficient RBAC permissions, and gives you a single authoritative secrets store with audit logging.

## Network Policy: Microsegmentation Inside the Cluster

By default, Kubernetes allows all pods in a cluster to communicate with all other pods. For most threat models, this is far too permissive — if an attacker compromises one workload, unrestricted east-west communication makes lateral movement trivial. NetworkPolicies implement microsegmentation at the pod level, defining exactly which pods can communicate with which other pods on which ports.

The first NetworkPolicy to implement in any namespace is a default-deny-all ingress and egress policy. This immediately restricts communication to only what's explicitly allowed. Then add allow policies for the specific communication paths your application requires: the web tier can receive traffic from the ingress controller, can reach the API tier on port 8080, but cannot directly reach the database. The API tier can reach the database on port 5432 but cannot initiate connections to arbitrary internet destinations. Build these policies incrementally, using network policy audit tools or your CNI's logging features to identify traffic that would be blocked before enforcing.

Note that NetworkPolicy requires a CNI plugin that supports it — Calico, Cilium, Weave Net, and others do; the default CNI in many managed Kubernetes offerings may not. Verify your CNI supports NetworkPolicy before assuming your policies are being enforced.

## Runtime Security and Threat Detection

Static hardening reduces your attack surface but doesn't eliminate it. Runtime security tools monitor what's actually happening inside your cluster and alert on behaviors that indicate compromise or policy violation. The category leader here is Falco, an open-source runtime security tool that uses kernel-level system call monitoring to detect behaviors like: a shell spawned inside a container, a binary writing to an unexpected path, a process making network connections to unusual destinations, privilege escalation attempts.

Falco's default ruleset covers many high-signal attack patterns. Customize it for your environment — suppress rules that generate noise from legitimate workload behavior and add rules specific to your threat model. A production cluster should have alerts for container escapes, unexpected privileged operations, anomalous network connections from sensitive namespaces, and access to credential files.

Complement Falco with Kubernetes audit logging. Enable audit logging on your API server and configure it to capture at minimum all requests to sensitive resource types (secrets, configmaps, RBAC resources) and all requests from service accounts. Ship these logs to your SIEM. Many attacks on Kubernetes clusters involve API server operations — creating new pods, modifying RBAC, reading secrets — that would be clearly visible in audit logs but invisible to network monitoring or endpoint detection.

## Keeping Current: Patch and Upgrade Discipline

Kubernetes releases security patches on a regular cadence and drops support for older minor versions aggressively — supported versions typically receive patches for about 14 months. Organizations running end-of-life cluster versions are exposed to known, public CVEs with no available patch path short of upgrading. Node operating systems, container runtimes (containerd, CRI-O), and CNI plugins need the same patch attention as the Kubernetes control plane itself.

Establish a regular upgrade cadence before falling behind forces an emergency upgrade under pressure. Managed Kubernetes services (EKS, AKE, GKE) make this more tractable by automating much of the control plane upgrade process — take advantage of those capabilities rather than treating the cluster as infrastructure to be manually maintained. Test upgrades in non-production environments first, but don't let the testing process become a reason to defer production upgrades indefinitely. A cluster that's two minor versions behind is already a security liability.

Container image patching is a separate concern from cluster patching. Even if your cluster is current, workloads built on stale base images accumulate CVEs over time. Automated image rebuilds triggered by base image updates, combined with the scanning pipeline described earlier, keep workload images reasonably current without requiring manual tracking of every image in your environment.

Building Kubernetes security doesn't happen in a single sprint — it's an ongoing program of configuration hardening, access control discipline, monitoring, and patch management. The prioritization is clear: fix the high-blast-radius issues first (root containers, wildcard RBAC grants, missing NetworkPolicies), instrument for visibility so you can detect what you haven't prevented, and build the operational processes that keep the environment from drifting back toward an insecure baseline.
