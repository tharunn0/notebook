# Security Engineering & Practices — Question Bank (Fundamentals → Senior → Staff Backend)

Part A covers security fundamentals (~75%). Part B covers senior-level practices. Part C covers senior+ and staff-level security leadership.

---

# Part A — Fundamentals

## 1. Core Concepts

1. What is the CIA triad (confidentiality, integrity, availability), and give an example of an attack on each?
2. What are authenticity and non-repudiation, and how do they extend the CIA triad?
3. What is the difference between a vulnerability, a threat, a risk, and an exploit?
4. What is the attack surface of a system, and how do you reduce it?
5. What is the principle of least privilege?
6. What is defense in depth, and give an example in a web backend?
7. What do "secure by default" and "fail securely (fail closed)" mean?
8. What is separation of duties?
9. Why is "security through obscurity" weak on its own, and when does obscurity still help?
10. What is the difference between authentication, authorization, and accounting (AAA)?
11. What is a trust boundary, and where are the trust boundaries in a typical web application?
12. What is the difference between a security vulnerability and a security misconfiguration?
13. What do CVE, CWE, and CVSS mean?
14. What is a zero-day vulnerability?
15. What is the difference between a black-box, gray-box, and white-box security test?

## 2. Cryptography Fundamentals

16. What is the difference between encoding, encryption, and hashing?
17. What is the difference between symmetric and asymmetric encryption, and when do you use each?
18. What are AES, RSA, and elliptic-curve cryptography (ECC), and what is each used for?
19. What are block cipher modes (ECB, CBC, CTR, GCM), and why is ECB insecure?
20. What is authenticated encryption (AEAD), and why should you prefer AES-GCM or ChaCha20-Poly1305?
21. What is an initialization vector (IV) or nonce, and what happens if you reuse one with AES-GCM?
22. What are the properties of a cryptographic hash function (preimage resistance, collision resistance)?
23. Why are MD5 and SHA-1 considered broken, and what should you use instead?
24. What is an HMAC, and why isn't `hash(secret + message)` good enough (length-extension attacks)?
25. What is a digital signature, and how does it differ from an HMAC?
26. How does Diffie–Hellman key exchange work at a high level?
27. What is forward secrecy, and why does it matter?
28. What is a key derivation function (HKDF, PBKDF2), and when do you need one?
29. What is a cryptographically secure random number generator, and why can't you use `math/rand`?
30. What is a timing attack, and why should you use constant-time comparisons for secrets?
31. What is post-quantum cryptography, and why are systems starting to adopt hybrid key exchange?
32. Why is "don't roll your own crypto" such strong advice?

## 3. Public Key Infrastructure & TLS

33. What is a digital certificate, and what does it contain?
34. What is a certificate authority, and how does the chain of trust work?
35. How does the TLS handshake establish a secure connection?
36. What does TLS 1.3 improve over TLS 1.2?
37. What is mutual TLS (mTLS), and when would you use it?
38. What is certificate revocation, and why are CRLs and OCSP unreliable in practice?
39. What is Certificate Transparency?
40. What is HSTS, and what is HSTS preloading?
41. What is certificate pinning, and why has it fallen out of favor?
42. How do you automate certificate rotation, and what goes wrong when certificates expire?

## 4. Authentication

43. How should passwords be stored, and why use bcrypt, scrypt, or Argon2id?
44. What is a salt, and what is a pepper?
45. What are rainbow tables, and how does salting defeat them?
46. What makes a good password policy according to modern guidance (NIST 800-63B)?
47. What are brute-force, credential-stuffing, and password-spraying attacks, and how do you defend against each?
48. What is multi-factor authentication, and what are the weaknesses of SMS-based codes?
49. How do TOTP authenticator apps work?
50. What are passkeys and WebAuthn, and why are they phishing-resistant?
51. How should you design a secure password reset flow?
52. What is account enumeration, and how do you prevent it in login and reset flows?
53. How do you securely manage sessions (session IDs, expiry, rotation after login)?
54. What is session fixation, and how do you prevent it?
55. What are JWTs, and what are the common mistakes when using them?
56. What are the `alg: none` and algorithm confusion attacks on JWTs?
57. How do you revoke access when using stateless tokens?
58. How do OAuth 2.0 and OpenID Connect work, and what is PKCE for?
59. What is single sign-on (SSO), and how do SAML and OIDC compare?

## 5. Authorization & Access Control

60. What are RBAC, ABAC, and ReBAC (relationship-based access control, Zanzibar-style)?
61. What is Insecure Direct Object Reference (IDOR), and how do you prevent it?
62. What is Broken Object Level Authorization (BOLA), and why is it the top API security risk?
63. What is the difference between horizontal and vertical privilege escalation?
64. Why must authorization be enforced on the server, never only in the UI?
65. Where should authorization checks live in a backend (middleware, service layer, database row-level security)?
66. How do you design authorization for a multi-tenant system so tenants can never see each other's data?
67. What is the confused deputy problem?
68. How do you test authorization logic systematically?

## 6. Web Application Security

69. What does the OWASP Top 10 cover?
70. What is SQL injection, and how do parameterized queries prevent it?
71. Why does escaping input manually fail to prevent injection reliably?
72. What are NoSQL injection, command injection, and LDAP injection?
73. What is Cross-Site Scripting (XSS)? Explain stored, reflected, and DOM-based XSS.
74. How do you prevent XSS (context-aware output encoding, sanitization, CSP, safe templating)?
75. What is Content Security Policy, and how do nonces and `strict-dynamic` work?
76. What is Cross-Site Request Forgery (CSRF), and how do `SameSite` cookies and CSRF tokens prevent it?
77. What is the Same-Origin Policy, and how does CORS relax it?
78. What CORS misconfigurations create vulnerabilities (reflecting any origin, allowing credentials with wildcards)?
79. What is Server-Side Request Forgery (SSRF), and why is it especially dangerous in cloud environments (metadata endpoints)?
80. How do you defend against SSRF (allowlists, blocking internal ranges, IMDSv2, egress controls)?
81. What is path traversal, and how do you prevent it?
82. What is XML External Entity (XXE) injection?
83. What is insecure deserialization, and why can it lead to remote code execution?
84. What is clickjacking, and how do `frame-ancestors` and `X-Frame-Options` prevent it?
85. What are open redirects, and how can they be abused in phishing and OAuth flows?
86. What are mass assignment vulnerabilities?
87. What is HTTP request smuggling?
88. What are file upload vulnerabilities, and how do you handle uploads safely?
89. Which security headers should every web app set, and what does each do?
90. What are cookie security attributes (`Secure`, `HttpOnly`, `SameSite`, `__Host-` prefix)?

## 7. API Security

91. What does the OWASP API Security Top 10 cover?
92. How do you authenticate service-to-service API calls?
93. How do you design rate limiting and quotas to prevent abuse?
94. What is excessive data exposure, and how do you avoid returning more fields than needed?
95. How do you validate input properly (allowlists, schemas, types, lengths)?
96. How do you sign and verify webhooks?
97. What are the security risks specific to GraphQL (introspection, deeply nested queries, batching attacks)?
98. How do you protect APIs against replay attacks?
99. Why should error messages avoid leaking internal details?

## 8. Secrets Management

100. Why should secrets never be committed to source control, and what should you do if one is leaked?
101. How do secret managers work (Vault, AWS Secrets Manager, GCP Secret Manager)?
102. What is secret rotation, and how do you rotate without downtime?
103. Environment variables vs mounted files vs secret manager APIs: what are the trade-offs?
104. What are short-lived, dynamic credentials, and why are they better than long-lived keys?
105. How do you detect secrets in code and history (gitleaks, trufflehog, push protection)?
106. How do you keep secrets out of logs, error messages, and crash dumps?

## 9. Network Security

107. What is the difference between a firewall, a security group, and a network ACL?
108. What is network segmentation, and why does it limit the blast radius of a breach?
109. What is zero-trust networking, and how does it differ from perimeter security?
110. What is a DMZ?
111. What is a VPN, and how do WireGuard and IPsec compare?
112. What is a DDoS attack, and how do volumetric, protocol, and application-layer attacks differ?
113. How do you mitigate DDoS attacks (CDNs, rate limiting, autoscaling, anycast)?
114. What is a man-in-the-middle attack, and how do TLS and HSTS prevent it?
115. What are DNS attacks (spoofing, hijacking, tunneling), and how are they mitigated?
116. What are bastion hosts, and what are modern alternatives (SSM Session Manager, identity-aware proxies)?
117. What do IDS and IPS systems do, and what is a WAF?

## 10. Data Protection & Privacy

118. What is the difference between encryption at rest and encryption in transit?
119. What is envelope encryption, and how does a KMS use data keys and key-encryption keys?
120. What is the difference between application-level, database-level, and disk-level encryption, and what threats does each address?
121. What are tokenization and data masking?
122. What is PII, and how should it be classified and handled?
123. What are the core principles of GDPR (data minimization, purpose limitation, right to erasure)?
124. How do you implement the right to be forgotten across databases, backups, logs, and analytics?
125. What are data retention policies, and why do they reduce risk?
126. How do you securely delete data, and what makes deletion hard on SSDs and in the cloud?

## 11. Secure Development Lifecycle

127. What is a secure software development lifecycle (SSDLC)?
128. What is "shift left" in security?
129. What is threat modeling, and how does the STRIDE framework work?
130. How do you run a lightweight threat model for a new feature?
131. What should a security-focused code review look for?
132. What is SAST, DAST, IAST, and SCA, and what does each find?
133. Why do security scanners produce so many false positives, and how do you handle them?
134. What is fuzzing, and why does it find security bugs other tests miss?
135. What is a penetration test, and how is it different from a vulnerability scan?
136. What is a bug bounty program, and when is a company ready for one?
137. How do you prioritize vulnerabilities for fixing (CVSS, exploitability, EPSS, reachability)?
138. What is responsible (coordinated) disclosure?

## 12. Software Supply Chain Security

139. What is a software supply chain attack? Give examples (SolarWinds, the xz backdoor, malicious npm packages).
140. What are dependency confusion and typosquatting attacks?
141. Why should you pin dependencies and verify lock files?
142. What is an SBOM, and how is it used during incident response?
143. What is SLSA, and what does build provenance protect against?
144. How do Sigstore and Cosign sign artifacts and container images?
145. How do you secure a CI/CD pipeline (least-privilege tokens, OIDC to cloud, protected branches, isolated runners)?
146. Why are third-party GitHub Actions a supply chain risk, and how do you pin them safely?

## 13. Logging, Monitoring & Incident Response

147. Which security events should every application log (logins, permission changes, admin actions, failures)?
148. What should never be logged?
149. What is an audit log, and how do you make it tamper-evident?
150. What is a SIEM, and how does it help detect attacks?
151. What are the phases of incident response (preparation, detection, containment, eradication, recovery, lessons learned)?
152. What should you do in the first hour after discovering a breach?
153. How do you preserve evidence during an incident?
154. What is a security runbook, and what should one contain?

---

# Part B — Senior-Level Practices

## 14. Cloud & Infrastructure Security

155. What is the shared responsibility model in the cloud?
156. How does cloud IAM work, and what are the most common IAM misconfigurations?
157. How do you avoid long-lived cloud access keys (workload identity, OIDC federation, instance roles)?
158. Why are publicly exposed storage buckets such a common breach cause, and how do you prevent them?
159. What is Cloud Security Posture Management (CSPM)?
160. What is the instance metadata service, and why was IMDSv2 introduced?
161. How do you secure infrastructure-as-code (policy as code, scanning Terraform, drift detection)?
162. How do you design guardrails across many cloud accounts (organizations, service control policies)?

## 15. Container & Kubernetes Security

163. Why should containers run as non-root with a read-only filesystem and dropped capabilities?
164. What is a container escape, and how do you reduce its likelihood?
165. How do you scan and harden container images?
166. How do Kubernetes RBAC, ServiceAccounts, and NetworkPolicies limit an attacker's movement?
167. What are Pod Security Standards, and how do admission policies enforce them?
168. How do you protect Kubernetes Secrets and etcd?
169. What does runtime security monitoring (Falco, Tetragon) detect?

## 16. Application Architecture Security

170. How do you design a secure authentication and session architecture for web and mobile clients?
171. How do you securely implement service-to-service authentication in a microservices system (mTLS, SPIFFE/SPIRE, signed tokens)?
172. How do you design secure multi-tenancy at the database, cache, queue, and storage layers?
173. How do you securely handle payments and comply with PCI DSS scope reduction?
174. How do you design a secure file-sharing or presigned URL system?
175. How do you protect admin panels and internal tools?
176. How do you design secure AI and LLM features (prompt injection, data exfiltration, tool permissions)?
177. How do you build security into an API gateway (authentication, rate limiting, schema validation)?

## 17. Compliance & Governance

178. What are SOC 2, ISO 27001, HIPAA, and PCI DSS, and what do they require from engineering teams?
179. What is the difference between being compliant and being secure?
180. How do you collect compliance evidence without slowing engineers down?
181. What is a vendor security review, and what should it check?
182. What are access reviews, and how do you automate them?

---

# Part C — Senior+ & Staff

## 18. Security Strategy & Leadership

183. How do you build a security program for a startup with no security team?
184. How do you prioritize security investments with a limited budget?
185. How do you measure the security posture of an organization?
186. How do you make secure defaults the easy path for developers (paved roads, secure libraries, templates)?
187. How do you run a security champions program across engineering teams?
188. How do you balance security requirements with delivery speed?
189. How do you handle a team that keeps ignoring security findings?
190. How do you communicate security risk to executives in business terms?
191. How do you decide whether to accept, mitigate, transfer, or avoid a risk?
192. How do you design for assume-breach, where you expect attackers to get in?
193. How do you plan and run a red team or tabletop exercise?
194. How do you lead the technical response to a major security incident and its public disclosure?
195. How do you evaluate the security impact of adopting AI coding tools across an organization?
196. Tell me about a security issue you found or fixed. What was the root cause, and what systemic change prevented recurrence?
