# AI Engineering — Question Bank (Fundamentals → Senior → Staff Backend)

Part A covers LLM fundamentals. Part B covers building LLM applications. Part C covers production AI systems at senior level. Part D covers senior+ and staff-level design, strategy, and leadership.

---

# Part A — LLM Fundamentals

## 1. How LLMs Work

1. What is a large language model, and what does "predicting the next token" actually mean?
2. What is a token, and why don't tokens map one-to-one to words?
3. How does tokenization (BPE, SentencePiece) work, and why does it affect cost, multilingual text, and arithmetic?
4. What is the transformer architecture at a high level?
5. What is self-attention, and why was it a breakthrough over RNNs and LSTMs?
6. What are queries, keys, and values in attention?
7. Why does attention cost grow quadratically with sequence length, and what techniques reduce it?
8. What are positional encodings, and how do approaches like RoPE help with long contexts?
9. What is the difference between encoder-only, decoder-only, and encoder–decoder models?
10. What are pre-training, supervised fine-tuning (SFT), and preference tuning (RLHF, DPO), and what does each stage add?
11. What is a base model vs an instruction-tuned or chat model?
12. What is a context window, and what happens when input exceeds it?
13. What is "lost in the middle," and how does it affect long-context prompts?
14. What are emergent abilities and scaling laws, and how much should engineers rely on them?
15. What is a mixture-of-experts (MoE) model, and how does it change inference cost?
16. What are reasoning (thinking) models, and how do they differ from standard chat models in cost, latency, and use cases?
17. What are multimodal models, and how are images or audio turned into tokens?

## 2. Generation & Sampling

18. What are logits, and how does softmax turn them into probabilities?
19. What do temperature, top-p (nucleus), and top-k sampling do?
20. When would you use temperature 0, and why is the output still not always deterministic?
21. What are stop sequences and max-token limits, and how do they affect truncation?
22. What is the difference between greedy decoding and beam search, and why do chat models rarely use beam search?
23. What are frequency and presence penalties?
24. What are log probabilities, and how can you use them for confidence scoring or classification?
25. Why do LLMs hallucinate, and what kinds of hallucination exist?

## 3. Model Selection & Providers

26. How do you choose between a frontier model, a small model, and an open-weights model for a task?
27. What are the trade-offs between hosted APIs and self-hosted open-weights models?
28. How do you compare models fairly for your use case instead of relying on public benchmarks?
29. Why are public benchmarks often contaminated or misleading?
30. What factors matter in model pricing (input vs output tokens, cached tokens, batch discounts)?
31. How do you plan for model deprecations and version changes from a provider?
32. What are data retention, privacy, and residency concerns when sending data to a model provider?

---

# Part B — Building LLM Applications

## 4. Prompt Engineering

33. What are the components of a good prompt (role, context, task, constraints, format, examples)?
34. What is the difference between a system prompt and a user message, and what belongs in each?
35. What are zero-shot, one-shot, and few-shot prompting, and when do examples help or hurt?
36. What is chain-of-thought prompting, and when is it worth the extra tokens?
37. How do you choose good few-shot examples, and how can they bias outputs?
38. Why do clear delimiters (XML tags, Markdown sections) improve prompt reliability?
39. What is prompt chaining, and when should you split one prompt into several steps?
40. How do you make prompts robust to unusual or adversarial user input?
41. How do you version, review, and test prompts like code?
42. How do you handle prompts that must work across several models?
43. Why does adding more instructions sometimes make results worse?

## 5. Structured Output & Tool Use

44. How do you get reliable JSON output from an LLM?
45. What is the difference between "JSON mode," schema-constrained decoding, and prompting for JSON?
46. How does constrained decoding (grammars, JSON Schema) guarantee valid output?
47. What is tool use (function calling), and how does the request–tool call–tool result loop work?
48. How should you design tool names, descriptions, and parameters so the model uses them correctly?
49. How many tools can you give a model before accuracy drops, and how do you handle large tool sets?
50. How do you validate and sanitize tool arguments produced by the model?
51. How do you handle tool errors so the model can recover instead of looping?
52. What is parallel tool calling, and when is it safe?
53. What is the Model Context Protocol (MCP), and what problem does it solve?
54. What are MCP's main concepts (hosts, clients, servers, tools, resources, prompts)?
55. What are the security risks of connecting MCP servers to an agent?

## 6. Embeddings & Vector Search

56. What is an embedding, and what does it mean for two embeddings to be "close"?
57. What are cosine similarity, dot product, and Euclidean distance, and when are they equivalent?
58. How do you choose an embedding model (dimensions, domain, language, cost)?
59. Why must queries and documents use the same embedding model, and what happens when you change models?
60. What is the difference between dense and sparse (BM25, SPLADE) retrieval?
61. What is hybrid search, and how do you combine dense and keyword scores (reciprocal rank fusion)?
62. What is approximate nearest neighbor (ANN) search, and why is exact search impractical at scale?
63. How do HNSW, IVF, and product quantization work, and what are their trade-offs?
64. How do recall, latency, memory, and index build time trade off in a vector index?
65. When should you use a dedicated vector database vs `pgvector` vs a search engine with vector support?
66. How do you handle metadata filtering together with vector search, and why can filtering hurt recall?
67. What are Matryoshka embeddings and embedding quantization, and how do they reduce cost?

## 7. Retrieval-Augmented Generation (RAG)

68. What is RAG, and what problems does it solve compared with fine-tuning?
69. What are the stages of a RAG pipeline (ingest, chunk, embed, index, retrieve, rerank, generate)?
70. How do you choose a chunking strategy (fixed size, recursive, semantic, document-structure aware)?
71. How do chunk size and overlap affect retrieval quality and cost?
72. Why should you store metadata (source, section, date, permissions) with chunks?
73. What is reranking with a cross-encoder, and why does it improve precision?
74. What are query rewriting, query expansion, HyDE, and multi-query retrieval?
75. What is contextual retrieval (adding document context to each chunk before embedding)?
76. How do you handle documents with tables, images, PDFs, and code?
77. How do you make the model cite sources and avoid answering beyond the retrieved context?
78. How do you handle "I don't know" when retrieval finds nothing relevant?
79. How do you keep a RAG index fresh when source documents change or are deleted?
80. How do you enforce document-level permissions so users only retrieve what they're allowed to see?
81. When is long-context stuffing better than RAG, and when is it worse?
82. What is GraphRAG, and when do knowledge graphs help retrieval?
83. What is agentic RAG, where the model decides when and what to retrieve?
84. What are the most common reasons a RAG system gives bad answers, and how do you diagnose which stage failed?

## 8. Agents

85. What is an AI agent, and how is it different from a chatbot or a fixed workflow?
86. What is the difference between a workflow (predefined steps) and an agent (model-directed steps), and when should you use each?
87. How does the basic agent loop work (observe, reason, act, repeat)?
88. What are common agent patterns (ReAct, plan-and-execute, reflection, router, orchestrator–workers, evaluator–optimizer)?
89. How do you decide when an agent should stop?
90. How do you prevent infinite loops, runaway costs, and repeated failing tool calls?
91. What is short-term vs long-term memory for agents, and how do you implement each?
92. How do you manage context in long-running agents (summarization, compaction, scratchpads, sub-agents)?
93. When do multi-agent systems help, and when do they just add cost and confusion?
94. How do you design human-in-the-loop approvals for risky actions?
95. How do you make agent actions idempotent and safe to retry?
96. How do you give an agent access to code execution or a browser safely (sandboxing)?
97. How do coding agents work, and what makes a codebase easier for an agent to work in?
98. Why do agents fail more often on long tasks, and how do you break tasks down to improve success rates?

## 9. Fine-Tuning & Adaptation

99. When should you fine-tune instead of using prompting or RAG?
100. What does fine-tuning teach well (format, style, task behavior), and what does it teach poorly (new facts)?
101. What are full fine-tuning, LoRA, and QLoRA, and what are their trade-offs?
102. How much training data do you need, and how do you build a high-quality dataset?
103. What is catastrophic forgetting, and how do you reduce it?
104. What is distillation, and how do you use a large model to train a smaller, cheaper one?
105. What is preference tuning (DPO, RLHF), and when would an application team use it?
106. How do you evaluate whether a fine-tuned model is actually better than the base model with a good prompt?
107. What are synthetic data generation risks (model collapse, bias amplification, low diversity)?

---

# Part C — Production AI Systems (Senior)

## 10. Evaluation

108. Why is evaluation the hardest and most important part of AI engineering?
109. What is the difference between offline evaluation and online evaluation?
110. How do you build an evaluation dataset (golden sets, real traffic samples, edge cases, adversarial cases)?
111. What are reference-based vs reference-free evaluation metrics?
112. What is LLM-as-a-judge, and what are its biases (position bias, verbosity bias, self-preference)?
113. How do you validate that an LLM judge agrees with human judgment?
114. How do you evaluate RAG separately for retrieval (recall@k, MRR, nDCG) and generation (faithfulness, relevance)?
115. How do you evaluate agents (task success, tool-call accuracy, steps, cost, trajectory)?
116. How do you run regression tests on prompts and models in CI?
117. How do you handle non-determinism when evaluating (multiple runs, pass@k, confidence intervals)?
118. How do you A/B test an LLM feature, and which product metrics matter?
119. How do you collect and use user feedback (thumbs up/down, edits, implicit signals)?
120. What does "vibe checking" miss, and how do you move a team past it?

## 11. Safety, Security & Guardrails

121. What is prompt injection, and what is the difference between direct and indirect prompt injection?
122. Why can't prompt injection be fully solved with better prompts?
123. How do you design an architecture that limits the damage of prompt injection (least privilege, separating trusted and untrusted content, confirmation steps)?
124. What is the "lethal trifecta" (private data, untrusted content, and an exfiltration channel), and why is that combination dangerous?
125. What are jailbreaks, and how do they differ from prompt injection?
126. How do you prevent data exfiltration through tool calls, links, or rendered images?
127. How do you prevent sensitive data (PII, secrets) from being sent to or leaked by a model?
128. What are input and output guardrails, and what are their latency and false-positive costs?
129. How do you moderate harmful content in user inputs and model outputs?
130. What does the OWASP Top 10 for LLM Applications cover?
131. How do you handle multi-tenant isolation so one customer's data never reaches another customer's context?
132. What are the legal and compliance concerns (copyright, GDPR, the EU AI Act, audit logs)?

## 12. Inference & Serving

133. What are the two phases of LLM inference (prefill and decode), and why do they have different bottlenecks?
134. What is the KV cache, and why does it dominate GPU memory during inference?
135. What are time to first token (TTFT), inter-token latency, and throughput, and how do they trade off?
136. What is continuous (in-flight) batching, and why does it improve throughput?
137. What is PagedAttention (vLLM), and what problem does it solve?
138. What is quantization (INT8, INT4, FP8), and what does it cost in quality?
139. What is speculative decoding, and how does it speed up generation?
140. What are tensor parallelism and pipeline parallelism, and when do you need them?
141. How do you estimate GPU memory needed to serve a model of a given size?
142. What serving stacks exist (vLLM, TGI, TensorRT-LLM, SGLang, Ollama, llama.cpp), and when would you use each?
143. How do you autoscale GPU inference, and why are cold starts a problem?
144. How do you stream responses to clients (SSE), and how do you handle cancellation mid-stream?

## 13. Cost, Latency & Reliability

145. How do you estimate the cost of an LLM feature before launch?
146. What is prompt caching, and how do you structure prompts to maximize cache hits?
147. What is semantic caching, and what are its risks?
148. What is model routing (sending easy requests to cheap models), and how do you decide the route?
149. When should you use batch APIs instead of real-time calls?
150. How do you reduce latency (streaming, smaller models, parallel calls, shorter outputs, speculative work)?
151. How do you handle provider rate limits, timeouts, and outages (retries, fallbacks, multiple providers)?
152. What does an LLM gateway do (routing, rate limiting, cost tracking, key management, logging)?
153. How do you set per-user or per-tenant token budgets and quotas?
154. How do you prevent a single user or bug from causing a huge bill?

## 14. LLMOps & Observability

155. What should you log for every LLM call (prompt version, model, tokens, latency, cost, tool calls, outcome)?
156. How do you trace multi-step chains and agents end to end?
157. How do you detect quality drift in production when there is no ground truth?
158. How do you roll out a new model or prompt safely (shadow traffic, canary, feature flags)?
159. How do you balance logging for debugging against privacy requirements?
160. What tools exist for LLM observability and evaluation, and what should you build vs buy?
161. How do you run an incident review when an AI feature produced a harmful or wrong output?

## 15. Data & Pipelines for AI

162. How do you build an ingestion pipeline for documents at scale (parsing, cleaning, deduplication, chunking, embedding)?
163. How do you re-embed millions of documents when you change embedding models, without downtime?
164. How do you handle data quality issues (duplicates, stale content, conflicting sources) that hurt answers?
165. How do you turn production logs into evaluation sets and training data responsibly?
166. How do you handle personal data deletion requests (right to be forgotten) across indexes, logs, and fine-tuned models?

## 16. AI System Design Problems

For each problem, cover requirements, architecture, retrieval or tools, evaluation, safety, cost, and failure modes.

167. Design a customer-support assistant that answers from a company knowledge base and escalates to humans.
168. Design an internal "chat with all company documents" system with permission-aware retrieval.
169. Design a coding assistant that can read a repository, make changes, and run tests.
170. Design a semantic search engine for a large e-commerce catalog.
171. Design a system that extracts structured data from millions of invoices and PDFs.
172. Design an AI agent that books travel and handles payments safely.
173. Design an LLM gateway used by every team in a large company.
174. Design a content moderation pipeline combining classifiers and LLMs.
175. Design an evaluation platform that lets teams test prompts and models before release.
176. Design a meeting summarization and action-item system.
177. Design a recommendation system that uses embeddings and LLM-generated explanations.
178. Design a voice assistant with low latency (speech-to-text, LLM, text-to-speech).
179. Design a multi-tenant AI platform where each customer brings their own data.

---

# Part D — Senior+ & Staff

## 17. Architecture & Trade-offs

180. How do you decide whether a problem needs an LLM at all, or whether rules or classical ML are better?
181. How do you design AI features that degrade gracefully when the model is wrong or unavailable?
182. How do you design for model portability so you aren't locked into one provider?
183. How do you combine deterministic code and LLM calls so that the deterministic parts carry the correctness guarantees?
184. How do you reason about reliability when each step of a pipeline is only 90% accurate?
185. How do you design an AI system whose behavior can be audited and explained after the fact?
186. When does it make sense to self-host models, and how do you build the cost model for that decision?
187. How do you design for rapidly improving models, so a better model next quarter is an easy upgrade rather than a rewrite?

## 18. Strategy & Leadership

188. How do you pick which AI use cases to build first in a company (value, feasibility, risk)?
189. How do you set realistic expectations with stakeholders about accuracy and failure modes?
190. How do you define success metrics for an AI feature before building it?
191. How do you set up AI engineering standards across teams (evals, logging, safety reviews, prompt management)?
192. How do you run a responsible AI review process without blocking every launch?
193. How do you integrate AI coding tools into an engineering team while keeping code quality and ownership?
194. How do you review AI-generated code effectively, and which risks need extra attention?
195. How do you keep engineers' skills sharp when much of the code is written with AI help?
196. How do you evaluate AI vendors and tools for a company?
197. How do you measure the ROI of an AI initiative after launch?
198. Tell me about an AI feature that failed or underperformed. What did you learn?
199. How do you stay current in a field that changes every few months without chasing every trend?
200. Which parts of software engineering do you think AI will change most over the next few years, and how are you preparing?
