# General Web Technologies — Question Bank (Fundamentals → Advanced)

Part A covers web fundamentals (~90%). Part B covers advanced topics (~10%).

---

# Part A — Fundamentals

## 1. How the Web Works

1. What happens, step by step, when you type a URL into the browser and press Enter?
2. What is the difference between the internet and the World Wide Web?
3. What is the client–server model?
4. What are the parts of a URL (scheme, host, port, path, query, fragment)?
5. Why is the URL fragment (`#section`) never sent to the server?
6. What is URL encoding (percent-encoding), and when is it needed?
7. What is the difference between a URI, a URL, and a URN?
8. What is the difference between a static website and a dynamic web application?
9. What is the difference between a web server (nginx, Apache) and an application server?

## 2. Networking Basics

10. What are the layers of the OSI model and the TCP/IP model, and where does HTTP sit?
11. What is an IP address, and what is the difference between IPv4 and IPv6?
12. What is the difference between public and private IP addresses, and what does NAT do?
13. What is a port, and what are well-known ports (80, 443, 22, 53, 5432)?
14. How does the TCP three-way handshake work?
15. How does TCP guarantee reliable, ordered delivery?
16. What is the difference between TCP and UDP, and when is UDP used on the web?
17. How is a TCP connection closed (FIN/ACK, four-way close)?
18. What is latency vs bandwidth, and what is round-trip time (RTT)?
19. What is a socket?

## 3. DNS

20. What is DNS, and why is it needed?
21. How does a DNS lookup work (browser cache, OS cache, recursive resolver, root, TLD, authoritative server)?
22. What are A, AAAA, CNAME, MX, TXT, NS, and SOA records?
23. Why can't you put a CNAME at the root (apex) of a domain, and what are ALIAS/ANAME records?
24. What is TTL in DNS, and what are the trade-offs of short vs long TTLs?
25. What is DNS propagation, and why do changes not appear instantly?
26. What are DNS over HTTPS (DoH) and DNS over TLS (DoT)?
27. What is DNS spoofing or cache poisoning, and what does DNSSEC do?

## 4. HTTP Fundamentals

28. What is HTTP, and why is it called a stateless protocol?
29. What does an HTTP request and response look like (start line, headers, body)?
30. What are the HTTP methods (GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS), and what does each mean?
31. Which HTTP methods are safe, and which are idempotent?
32. What is the difference between PUT and PATCH?
33. What is the difference between PUT and POST?
34. Can a GET request have a body? Should it?
35. What do the status code classes (1xx, 2xx, 3xx, 4xx, 5xx) mean?
36. What is the difference between 200, 201, 202, and 204?
37. What is the difference between 301, 302, 307, and 308 redirects?
38. What is the difference between 401 and 403?
39. What is the difference between 400, 404, 409, 422, and 429?
40. What is the difference between 500, 502, 503, and 504?
41. What are common request headers (`Host`, `User-Agent`, `Accept`, `Authorization`, `Content-Type`, `Cookie`)?
42. What are common response headers (`Content-Type`, `Content-Length`, `Cache-Control`, `Set-Cookie`, `Location`)?
43. Why is the `Host` header required in HTTP/1.1, and what does it enable (virtual hosting)?
44. What is content negotiation (`Accept`, `Accept-Language`, `Accept-Encoding`)?
45. What are MIME types, and why does a wrong `Content-Type` cause problems?
46. What is chunked transfer encoding?
47. What is HTTP compression (gzip, Brotli, zstd), and how is it negotiated?
48. What are range requests and `206 Partial Content` used for?
49. What is HTTP keep-alive (persistent connections)?

## 5. HTTP Versions

50. What did HTTP/1.1 add over HTTP/1.0?
51. What is head-of-line blocking in HTTP/1.1?
52. What does HTTP/2 add (binary framing, multiplexing, header compression with HPACK)?
53. Why did HTTP/2 server push get deprecated, and what replaced it (103 Early Hints)?
54. What is HTTP/3, and why does it use QUIC over UDP instead of TCP?
55. How does QUIC solve TCP-level head-of-line blocking and speed up connection setup?

## 6. HTTPS, TLS & Certificates

56. What is the difference between HTTP and HTTPS?
57. What does TLS provide (confidentiality, integrity, authentication)?
58. How does the TLS handshake work at a high level?
59. What does TLS 1.3 improve over TLS 1.2 (fewer round trips, removed weak ciphers, 0-RTT)?
60. What is the difference between symmetric and asymmetric encryption, and how does TLS use both?
61. What is a digital certificate, and what is a certificate authority (CA)?
62. How does the browser verify a certificate chain?
63. What is SNI, and why is it needed?
64. What is HSTS, and what attack does it prevent?
65. What are certificate revocation, OCSP, and OCSP stapling?
66. What is Let's Encrypt, and how does the ACME protocol automate certificates?
67. What is a man-in-the-middle attack, and how does HTTPS prevent it?

## 7. Browsers & Rendering

68. What are the main components of a browser (UI, networking, rendering engine, JavaScript engine, storage)?
69. What is the critical rendering path (HTML → DOM, CSS → CSSOM, render tree, layout, paint, composite)?
70. What is the difference between reflow (layout) and repaint?
71. Why do `<script>` tags block parsing, and what do `async` and `defer` do?
72. What is the DOM, and how is it different from the HTML source?
73. What is the JavaScript event loop (call stack, task queue, microtask queue)?
74. What is the difference between microtasks (Promises) and macrotasks (`setTimeout`)?
75. What are Web Workers and Service Workers, and how do they differ?
76. What is the difference between `localStorage`, `sessionStorage`, cookies, and IndexedDB?
77. How does browser caching work (memory cache, disk cache, back/forward cache)?

## 8. Frontend Fundamentals (for Backend Engineers)

78. What is semantic HTML, and why does it matter for accessibility and SEO?
79. What is the CSS box model?
80. What is the difference between client-side rendering (CSR), server-side rendering (SSR), static site generation (SSG), and incremental static regeneration (ISR)?
81. What is hydration, and why can it be slow?
82. What is a single-page application (SPA), and what are its trade-offs?
83. What is the difference between progressive enhancement and graceful degradation?
84. What is responsive design?
85. What does a JavaScript bundler do (Webpack, Vite, esbuild), and what are tree shaking and code splitting?
86. What is web accessibility (a11y), and what is ARIA for?

## 9. State, Cookies & Sessions

87. How do cookies work, and how does the browser decide which cookies to send?
88. What do the cookie attributes `Domain`, `Path`, `Expires`/`Max-Age`, `Secure`, `HttpOnly`, and `SameSite` do?
89. What is the difference between `SameSite=Strict`, `Lax`, and `None`?
90. What is the difference between first-party and third-party cookies, and why are browsers restricting third-party cookies?
91. How do server-side sessions work?
92. What is the difference between session-based and token-based authentication?
93. Where should you store tokens in a browser, and what are the risks of `localStorage`?

## 10. Authentication & Authorization

94. What is the difference between authentication and authorization?
95. How does HTTP Basic authentication work, and why is it only acceptable over HTTPS?
96. What is a JWT, what are its three parts, and how is it verified?
97. Why can't you easily revoke a JWT, and how do short-lived access tokens with refresh tokens help?
98. What is OAuth 2.0, and what problem does it solve?
99. What are the OAuth roles (resource owner, client, authorization server, resource server)?
100. How does the Authorization Code flow with PKCE work, and why is PKCE needed?
101. Why were the Implicit flow and Password grant deprecated?
102. What is OpenID Connect, and how does it differ from OAuth 2.0?
103. What is SSO, and how do SAML and OIDC compare?
104. What are API keys, and how do they differ from OAuth tokens?
105. What is multi-factor authentication, and what are passkeys (WebAuthn)?

## 11. Web Security

106. What is the Same-Origin Policy, and what defines an origin?
107. What is CORS, and why does it exist?
108. What is a CORS preflight request, and when does the browser send one?
109. Why can't you use `Access-Control-Allow-Origin: *` with credentials?
110. What is Cross-Site Scripting (XSS)? Explain stored, reflected, and DOM-based XSS.
111. How do you prevent XSS (output encoding, sanitization, CSP)?
112. What is Content Security Policy (CSP), and how do nonces and hashes work?
113. What is Cross-Site Request Forgery (CSRF), and how do CSRF tokens and `SameSite` cookies prevent it?
114. What is SQL injection, and how do parameterized queries prevent it?
115. What are command injection and path traversal attacks?
116. What is Server-Side Request Forgery (SSRF), and why is it dangerous in cloud environments?
117. What is clickjacking, and how do `X-Frame-Options` and `frame-ancestors` prevent it?
118. What are open redirects, and why are they a problem?
119. What does the OWASP Top 10 cover?
120. Which security headers should every site set (`Strict-Transport-Security`, `Content-Security-Policy`, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`)?
121. How should passwords be stored (bcrypt, scrypt, Argon2), and why not plain SHA-256?
122. What are brute-force and credential-stuffing attacks, and how do you defend against them?
123. What is rate limiting, and why is it a security control as well as a reliability control?

## 12. APIs

124. What is REST, and what are its constraints (stateless, uniform interface, cacheable, layered)?
125. What makes a good RESTful URL design (nouns, plural resources, nesting)?
126. How should you version a web API (URL path, header, media type)?
127. What is the difference between REST, GraphQL, and gRPC?
128. What problems does GraphQL solve, and what problems does it introduce (N+1, caching, query cost)?
129. What is JSON, and what are its limitations (no dates, number precision, no binary)?
130. What are JSON Schema and OpenAPI (Swagger) used for?
131. How do you implement pagination (offset vs cursor)?
132. How do you implement filtering, sorting, and field selection in an API?
133. What is idempotency, and how do idempotency keys make POST requests safe to retry?
134. How should API errors be structured (RFC 9457 Problem Details)?
135. What are webhooks, and how do you verify that a webhook came from the real sender?
136. How do you document and test APIs?

## 13. Real-Time Communication

137. What is the difference between short polling, long polling, Server-Sent Events, and WebSockets?
138. How does the WebSocket handshake work (HTTP Upgrade)?
139. When would you choose SSE over WebSockets?
140. What is WebRTC, and what are STUN and TURN servers for?
141. How do you authenticate and scale WebSocket connections?

## 14. Caching

142. How do `Cache-Control` directives work (`max-age`, `s-maxage`, `no-cache`, `no-store`, `private`, `public`, `immutable`)?
143. What is the difference between `no-cache` and `no-store`?
144. How do ETags and `If-None-Match` enable conditional requests and `304 Not Modified`?
145. How do `Last-Modified` and `If-Modified-Since` work, and why are ETags preferred?
146. What does `stale-while-revalidate` do?
147. What is the `Vary` header, and why can a wrong `Vary` break caching?
148. What is cache busting, and why do build tools add content hashes to file names?
149. What is a CDN, and how does it cache content at the edge?
150. What is the difference between a CDN cache, a reverse proxy cache, and an application cache?
151. How do you invalidate content on a CDN?

## 15. Web Servers, Proxies & Infrastructure

152. What is a reverse proxy, and why put nginx or Envoy in front of an application?
153. What is the difference between a forward proxy and a reverse proxy?
154. What is a load balancer, and what is the difference between L4 and L7 load balancing?
155. What is TLS termination, and where should it happen?
156. What do `X-Forwarded-For`, `X-Forwarded-Proto`, and the `Forwarded` header do, and why can't you trust them blindly?
157. What is an API gateway?
158. What is serverless computing, and what are cold starts?
159. What is edge computing (Cloudflare Workers, edge functions)?

## 16. Web Performance

160. What are Core Web Vitals (LCP, INP, CLS), and what does each measure?
161. What is Time to First Byte (TTFB), and what affects it?
162. How do you reduce page load time (compression, caching, CDNs, minification, lazy loading)?
163. What are resource hints (`preload`, `prefetch`, `preconnect`, `dns-prefetch`)?
164. How do image formats (JPEG, PNG, WebP, AVIF, SVG) compare, and what are responsive images?
165. Why does reducing the number of round trips matter more than reducing bytes on high-latency networks?
166. How do you measure web performance (Lighthouse, DevTools, real user monitoring)?

---

# Part B — Advanced

## 17. Advanced Topics

167. How does QUIC handle connection migration when a mobile client changes networks?
168. What are the security risks of TLS 1.3 0-RTT (replay attacks), and how do servers mitigate them?
169. What is HTTP request smuggling, and why does it happen between proxies and backends?
170. What are cache poisoning and web cache deception attacks?
171. What are the risks of JWT misuse (the `alg: none` attack, algorithm confusion, weak secrets)?
172. What are mTLS and certificate pinning, and what are the operational costs of each?
173. What is Certificate Transparency, and why does it matter?
174. How do browsers isolate sites (Site Isolation, COOP, COEP, CORP), and why did Spectre force these changes?
175. What is WebAssembly, and where does it fit on the client and the server?
176. How do you design a globally distributed web application with low latency for users everywhere?
177. How do you handle zero-downtime deployments for web applications, including long-lived WebSocket connections?
178. What is HTTP/2 rapid reset (CVE-2023-44487), and what does it teach about protocol-level DoS attacks?
