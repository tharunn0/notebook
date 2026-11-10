# Docker & Kubernetes — Question Bank (Fundamentals → Senior → Staff Backend)

Part A covers containers and Docker. Part B covers Kubernetes fundamentals. Part C covers senior-level Kubernetes in production. Part D covers senior+ and staff-level platform topics.

---

# Part A — Containers & Docker

## 1. Container Fundamentals

1. What is a container, and how is it different from a virtual machine?
2. Which Linux kernel features make containers possible (namespaces, cgroups, union filesystems)?
3. Which namespaces exist (pid, net, mnt, uts, ipc, user, cgroup), and what does each isolate?
4. What do cgroups control, and what changed between cgroups v1 and v2?
5. Why is a container "just a process" on the host, and what can you see from the host with `ps`?
6. What are the OCI image and runtime specifications, and why do they matter?
7. What is the relationship between Docker, containerd, runc, and CRI-O?
8. What is a container runtime vs a container engine?
9. Why are containers less isolated than VMs, and what do gVisor, Kata Containers, and Firecracker add?

## 2. Images & Dockerfiles

10. What is an image layer, and how does the union filesystem (overlayfs) combine layers?
11. What is the difference between an image and a container?
12. How does Docker's build cache work, and how does instruction order affect it?
13. Why should you copy dependency manifests (`go.mod`, `package.json`) before copying source code?
14. What is the difference between `CMD` and `ENTRYPOINT`, and how do they combine?
15. What is the difference between the shell form and exec form of `CMD`/`ENTRYPOINT`, and why does it matter for signals?
16. What is the difference between `COPY` and `ADD`?
17. What is the difference between `ARG` and `ENV`, and why shouldn't secrets go in either?
18. How do multi-stage builds work, and why do they shrink images?
19. What are the trade-offs of `scratch`, `distroless`, `alpine`, and `debian-slim` base images?
20. Why can Alpine's musl libc cause problems for some applications?
21. What does `.dockerignore` do, and what goes wrong without it?
22. Why should you pin base images by digest rather than by tag?
23. Why is the `latest` tag dangerous in production?
24. What is BuildKit, and what do cache mounts and secret mounts (`--mount=type=secret`) provide?
25. How do you build multi-architecture images (amd64/arm64) with `docker buildx`?
26. How do you reduce image size, and why does image size matter?
27. What are reproducible builds, and how close can Docker builds get to them?

## 3. Running Containers

28. Why should a container run one main process?
29. What is the PID 1 problem, and why do containers sometimes ignore `SIGTERM`?
30. What do `tini` and `docker run --init` solve (signal forwarding, zombie reaping)?
31. How does `docker stop` work (SIGTERM, grace period, SIGKILL)?
32. Why should containers run as a non-root user, and how do you set one up?
33. What are Linux capabilities, and why drop them (`--cap-drop=ALL`)?
34. What does `--privileged` do, and why is it dangerous?
35. What do read-only root filesystems provide?
36. How do memory and CPU limits work in Docker, and what happens when a container exceeds its memory limit?
37. What does the `HEALTHCHECK` instruction do, and how does it differ from Kubernetes probes?
38. What are restart policies, and when would you use each?
39. How should applications log inside containers, and why log to stdout/stderr?

## 4. Docker Storage & Networking

40. What is the difference between volumes, bind mounts, and tmpfs mounts?
41. Why is data written to the container's writable layer lost when the container is removed?
42. What are Docker's network drivers (bridge, host, none, overlay, macvlan)?
43. How does container-to-container DNS work on a user-defined bridge network?
44. How does port publishing (`-p 8080:80`) work under the hood (iptables, NAT, docker-proxy)?
45. What is the difference between `EXPOSE` and publishing a port?
46. How does Docker Compose manage networks, volumes, and service dependencies?
47. Why doesn't `depends_on` guarantee a service is ready, and how do you handle readiness in Compose?

## 5. Image Security & Supply Chain

48. How do you scan images for vulnerabilities (Trivy, Grype), and how do you act on the results?
49. What is an SBOM, and why does it matter?
50. How do image signing and verification work (Cosign, Sigstore, Notary)?
51. How do you keep secrets out of image layers and build history?
52. What is a container registry, and how do you secure a private one?
53. What is SLSA, and what does build provenance protect against?

---

# Part B — Kubernetes Fundamentals

## 6. Architecture

54. What problems does Kubernetes solve that plain Docker does not?
55. What are the control plane components (kube-apiserver, etcd, scheduler, controller-manager, cloud-controller-manager)?
56. What runs on each worker node (kubelet, kube-proxy, container runtime)?
57. Why is etcd the most critical component, and what happens if it loses quorum?
58. What is the declarative model, and what is a reconciliation loop?
59. What happens, step by step, when you run `kubectl apply -f deployment.yaml`?
60. What is the Container Runtime Interface (CRI), and why was dockershim removed?
61. What are the CNI and CSI interfaces?

## 7. Workloads

62. What is a Pod, and why is it the smallest deployable unit instead of a container?
63. Why do containers in the same Pod share a network namespace, and what does that allow?
64. What are init containers, and when do you use them?
65. What are sidecar containers, and how do native sidecars (`restartPolicy: Always` on init containers) change their lifecycle?
66. What is the difference between a ReplicaSet and a Deployment?
67. How does a rolling update work, and what do `maxSurge` and `maxUnavailable` control?
68. How do you roll back a Deployment, and what does revision history store?
69. When do you use a StatefulSet, and what guarantees does it give (stable identity, ordered rollout, stable storage)?
70. What is a DaemonSet, and what are typical use cases?
71. What is the difference between a Job and a CronJob, and what do `backoffLimit`, `completions`, and `parallelism` do?
72. What happens to CronJobs when the controller misses a schedule (`concurrencyPolicy`, `startingDeadlineSeconds`)?
73. What are labels, selectors, and annotations, and how do they differ?
74. What are namespaces, and what do they isolate and not isolate?

## 8. Pod Lifecycle & Health

75. What are the Pod phases, and what is the difference between phase and container state?
76. What is the difference between liveness, readiness, and startup probes?
77. Why can a badly designed liveness probe cause cascading restarts?
78. Should a readiness probe check downstream dependencies like the database? Why or why not?
79. What happens during Pod termination (endpoint removal, `preStop` hook, SIGTERM, grace period, SIGKILL)?
80. Why can a Pod still receive traffic after it gets SIGTERM, and how do you avoid dropped requests?
81. What do `CrashLoopBackOff`, `ImagePullBackOff`, `OOMKilled`, `Pending`, and `Evicted` mean, and how do you debug each?
82. What do restart policies (`Always`, `OnFailure`, `Never`) mean for different workloads?

## 9. Configuration & Secrets

83. What is the difference between a ConfigMap and a Secret?
84. Are Kubernetes Secrets encrypted? What does base64 encoding not provide?
85. How do you enable encryption at rest for Secrets in etcd?
86. Environment variables vs mounted files for config: what are the trade-offs, including updates?
87. How do you integrate external secret stores (Vault, AWS Secrets Manager, External Secrets Operator, CSI Secrets Store)?
88. How do you trigger a rollout when a ConfigMap changes?

## 10. Networking & Services

89. What is the Kubernetes networking model (every Pod gets an IP, no NAT between Pods)?
90. What are the Service types (ClusterIP, NodePort, LoadBalancer, ExternalName), and when do you use each?
91. What is a headless Service, and why do StatefulSets use one?
92. How does kube-proxy implement Services (iptables vs IPVS vs eBPF with Cilium)?
93. What are EndpointSlices, and why did they replace Endpoints?
94. How does DNS work in Kubernetes (CoreDNS, `svc.cluster.local`, `ndots`)?
95. Why can the default `ndots:5` setting cause DNS latency, and how do you fix it?
96. What is an Ingress, and what does an Ingress controller do?
97. What is the Gateway API, and how does it improve on Ingress?
98. What are NetworkPolicies, and why does a cluster allow all traffic by default?
99. How do you write a default-deny NetworkPolicy and then allow specific traffic?
100. Why does gRPC load balancing break with a standard ClusterIP Service, and how do you fix it?

## 11. Storage

101. What are PersistentVolumes, PersistentVolumeClaims, and StorageClasses?
102. What is dynamic provisioning?
103. What are the access modes (RWO, ROX, RWX, RWOP), and which do block storage volumes support?
104. What do reclaim policies (`Retain`, `Delete`) do, and how can the wrong one cause data loss?
105. What does `volumeBindingMode: WaitForFirstConsumer` solve in multi-zone clusters?
106. How do `volumeClaimTemplates` work in StatefulSets?
107. What are `emptyDir` volumes, and when are they cleaned up?
108. How do volume snapshots and volume expansion work?

## 12. kubectl & Everyday Operations

109. How do you debug a Pod that won't start (`describe`, `logs --previous`, events)?
110. What does `kubectl debug` with ephemeral containers let you do?
111. How do `port-forward`, `exec`, and `cp` work, and what are their security implications?
112. What is the difference between `kubectl apply`, `create`, `replace`, and `patch`?
113. What is server-side apply, and how does field ownership work?
114. How do you safely drain a node for maintenance?

---

# Part C — Kubernetes in Production (Senior)

## 13. Scheduling

115. How does the scheduler pick a node (filtering and scoring)?
116. What are node selectors, node affinity, and pod affinity/anti-affinity?
117. What are taints and tolerations, and how do they differ from affinity?
118. How do topology spread constraints distribute Pods across zones and nodes?
119. What are priority classes and preemption?
120. Why might a Pod stay `Pending` forever, and how do you investigate?

## 14. Resources & QoS

121. What is the difference between resource requests and limits?
122. How do requests affect scheduling, and how do limits affect runtime behavior?
123. What are the QoS classes (Guaranteed, Burstable, BestEffort), and how do they affect eviction?
124. Why does CPU throttling happen even when node CPU is idle (CFS quotas)?
125. Should you set CPU limits at all? What are the arguments on each side?
126. What happens when a container exceeds its memory limit vs when a node runs low on memory?
127. How do JVM, Go, and Node.js runtimes need to be configured to respect container limits?
128. What are LimitRanges and ResourceQuotas?
129. What does in-place Pod resource resizing allow?

## 15. Autoscaling

130. How does the Horizontal Pod Autoscaler work, and what metrics can it use?
131. Why can HPA scale on CPU badly for I/O-bound services, and what are the alternatives (custom metrics, KEDA)?
132. What does the Vertical Pod Autoscaler do, and why shouldn't it run alongside HPA on the same metric?
133. How do the Cluster Autoscaler and Karpenter differ?
134. How do you handle scaling lag during sudden traffic spikes (overprovisioning, pause Pods)?
135. How do HPA stabilization windows and scaling policies prevent flapping?

## 16. Availability & Deployments

136. What is a PodDisruptionBudget, and what does it protect against?
137. What is the difference between voluntary and involuntary disruptions?
138. How do you run a highly available application across zones?
139. How do you implement blue-green and canary deployments (Argo Rollouts, Flagger, service mesh)?
140. How do you run database migrations safely in a Kubernetes deployment pipeline?
141. How do you achieve zero-downtime deployments end to end?

## 17. Security

142. How does RBAC work (Roles, ClusterRoles, RoleBindings, ClusterRoleBindings)?
143. What are ServiceAccounts, and what changed with bound, projected service account tokens?
144. How do workload identity mechanisms (IRSA, GKE Workload Identity) let Pods access cloud APIs without static keys?
145. What are Pod Security Standards (privileged, baseline, restricted) and Pod Security Admission?
146. What does a `securityContext` control (runAsNonRoot, readOnlyRootFilesystem, capabilities, seccomp)?
147. What are admission controllers, and what do validating and mutating webhooks do?
148. How do policy engines (OPA Gatekeeper, Kyverno, ValidatingAdmissionPolicy with CEL) enforce standards?
149. How would an attacker escalate from a compromised Pod to the whole cluster, and how do you prevent it?
150. How do you secure the API server and etcd?
151. What does runtime security tooling (Falco, Tetragon) detect?

## 18. Observability & Troubleshooting

152. How do you collect logs from a cluster (node agents, sidecars), and what are the trade-offs?
153. How does Prometheus discover and scrape targets in Kubernetes?
154. Which metrics matter most for cluster health (kube-state-metrics, node-exporter, cAdvisor)?
155. A Service returns intermittent 502s during deployments — how do you investigate?
156. Pods are being OOMKilled but memory metrics look normal — what could explain it?
157. DNS lookups are slow and timing out in the cluster — how do you diagnose it?
158. A node shows `NotReady` — what do you check?
159. Latency rose after moving a service to Kubernetes — what are the likely causes?

---

# Part D — Senior+ & Staff (Platform)

## 19. Extending Kubernetes

160. What are Custom Resource Definitions, and when should you create one?
161. What is the operator pattern, and how does a controller reconcile desired vs actual state?
162. How do you write a controller that is idempotent and level-triggered rather than edge-triggered?
163. What are informers, work queues, and resync periods in client-go?
164. What are finalizers and owner references, and how do they control deletion and garbage collection?
165. Why can a stuck finalizer block namespace deletion, and how do you resolve it safely?
166. How do you run a stateful system (database, Raft cluster, Kafka) on Kubernetes, and when shouldn't you?

## 20. Packaging, GitOps & Delivery

167. Helm vs Kustomize: what are the trade-offs?
168. What are the common pitfalls of Helm charts (templating complexity, CRD upgrades, secrets)?
169. What is GitOps, and how do Argo CD and Flux implement it?
170. How do you handle configuration drift and manual changes in a GitOps model?
171. How do you promote changes across dev, staging, and production environments?
172. How do you manage secrets in a GitOps workflow (Sealed Secrets, SOPS, External Secrets)?

## 21. Cluster Architecture & Platform Engineering

173. One large cluster vs many small clusters: what are the trade-offs?
174. How do you implement multi-tenancy in Kubernetes (namespaces, quotas, policies, virtual clusters)?
175. How do you design a multi-region or multi-cluster setup, and how does traffic fail over between clusters?
176. Managed Kubernetes (EKS, GKE, AKS) vs self-managed: how do you decide?
177. How do you upgrade a production cluster's Kubernetes version safely?
178. How do you handle deprecated and removed APIs during upgrades?
179. How do you back up and restore a cluster (etcd snapshots, Velero), and what does a backup not cover?
180. What are the scaling limits of a Kubernetes cluster (nodes, Pods, Services, etcd size), and what breaks first?
181. How do you reduce Kubernetes cost (rightsizing, bin packing, spot instances, autoscaling)?
182. What is a service mesh, what does it cost in latency and complexity, and when is it worth it?
183. How does eBPF (Cilium) change Kubernetes networking, security, and observability?
184. How would you design an internal developer platform on top of Kubernetes?
185. What should application teams own, and what should the platform team own?
186. When is Kubernetes the wrong choice, and what would you use instead (serverless, ECS, Nomad, plain VMs)?
187. How do you set and enforce reliability standards (probes, PDBs, resource requests) across hundreds of services?
188. How do you run a cluster-wide incident review after a control plane outage?
