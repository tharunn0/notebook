# Go (Golang) — Question Bank (Fundamentals → Senior → Staff Backend)

Part A covers fundamentals and senior-level questions (~70%). Part B covers senior+ and staff-level questions (~30%).

---

# Part A — Fundamentals & Senior

## 1. Language Basics & Types

1. What are Go's basic types, and what are their zero values?
2. What is the difference between `var x int`, `x := 0`, and `var x = 0`, and where can each be used?
3. What does variable shadowing mean, and how does `:=` in an inner scope cause subtle bugs?
4. What are untyped constants, and why can `const x = 1 << 100` compile?
5. How does `iota` work, and how do you build enums with it?
6. What is the difference between a type definition (`type A B`) and a type alias (`type A = B`)?
7. Why does Go have no implicit numeric conversions?
8. What happens on integer overflow in Go?
9. What is the difference between `byte`, `rune`, and `string`?
10. How are strings represented internally, and why are they immutable?
11. Why does `len("héllo")` not return 5, and how do you iterate over characters correctly?
12. What happens when you convert between `string` and `[]byte`, and when does it allocate?
13. Why is `strings.Builder` faster than `+=` concatenation in a loop?
14. What are the rules for exported vs unexported identifiers?
15. What does the blank identifier `_` do in imports, assignments, and interface checks?
16. What does `init()` do, in what order do `init` functions run, and why should you avoid heavy logic in them?

## 2. Arrays, Slices & Maps

17. What is the difference between an array and a slice?
18. What is a slice header (pointer, length, capacity)?
19. How does `append` grow a slice, and when does it allocate a new backing array?
20. Why can two slices unexpectedly share data after `append`, and how do you prevent it?
21. What does the full slice expression `s[low:high:max]` do and when do you need it?
22. What is the difference between a `nil` slice and an empty slice, and does it matter for JSON encoding?
23. How can sub-slicing a large slice cause a memory leak?
24. How do you remove an element from a slice, and what are the `slices` package helpers?
25. How are maps implemented internally (buckets, hashing, growth; Swiss tables in Go 1.24)?
26. Why is map iteration order random?
27. Why can't you take the address of a map element (`&m[k]`)?
28. What happens when you write to a `nil` map vs read from one?
29. Are maps safe for concurrent use? What happens when two goroutines write at once?
30. When would you use `sync.Map` instead of a map with a mutex?
31. Which types can be map keys, and why can't slices be keys?
32. Does deleting from a map shrink its memory?

## 3. Functions, Methods & Structs

33. Is Go pass-by-value or pass-by-reference? Explain with slices, maps, and pointers.
34. What are variadic functions, and how do you pass a slice to one?
35. What are closures, and how do they capture variables?
36. How did the loop variable semantics change in Go 1.22, and what bug did it fix?
37. What is the difference between value receivers and pointer receivers?
38. When should you choose a pointer receiver?
39. What is a method set, and why can't a value type satisfy an interface whose methods have pointer receivers?
40. How does struct embedding work, and how is it different from inheritance?
41. What happens when two embedded types have methods with the same name?
42. What are struct tags, and how does `encoding/json` use them?
43. How does struct field ordering affect memory layout and padding?
44. When are structs comparable, and how does `==` work on them?
45. What are function types, and how are they used for middleware and options?
46. What is the functional options pattern, and when is it better than a config struct?

## 4. Interfaces

47. How are interfaces satisfied implicitly, and what are the benefits?
48. What is the internal representation of an interface value (`iface`/`eface`, type and data pointers)?
49. Why is an interface holding a nil pointer not equal to `nil`? Show the classic `error` bug.
50. What is `any` / `interface{}`, and what is the cost of using it?
51. What are type assertions and type switches, and what happens when an assertion fails?
52. Why does "accept interfaces, return structs" exist as a guideline?
53. Why should interfaces usually be defined by the consumer, not the producer?
54. Why are small interfaces (`io.Reader`, `io.Writer`) so powerful in Go?
55. How do you check at compile time that a type implements an interface (`var _ I = (*T)(nil)`)?
56. What is the cost of a method call through an interface vs a direct call?
57. How do interface embedding and composition work (`io.ReadWriteCloser`)?

## 5. Error Handling

58. Why does Go use error values instead of exceptions?
59. What is the difference between `errors.New`, `fmt.Errorf`, and custom error types?
60. How does error wrapping with `%w` work, and how do `errors.Is` and `errors.As` traverse the chain?
61. What are sentinel errors, and what are their drawbacks?
62. When should you wrap an error, and when should you not expose the underlying error?
63. How do you wrap multiple errors (`errors.Join`, multiple `%w`)?
64. When should you use `panic`, and when is it wrong?
65. How does `recover` work, and why does it only work inside a deferred function?
66. Does a panic in one goroutine crash the whole program? How do you protect worker goroutines?
67. How do you design error types for an API layer (mapping domain errors to HTTP or gRPC codes)?

## 6. Defer

68. In what order do deferred calls run?
69. When are deferred function arguments evaluated?
70. How can a deferred function modify named return values?
71. Why is `defer` inside a loop a problem?
72. Why is `defer f.Close()` on a file opened for writing potentially wrong, and how do you handle the close error?
73. What was the performance cost of `defer` historically, and what changed with open-coded defers?

## 7. Goroutines & Concurrency Basics

74. What is a goroutine, and how does it differ from an OS thread?
75. How much memory does a goroutine start with, and how does its stack grow?
76. What is the difference between concurrency and parallelism?
77. What does `GOMAXPROCS` control, and how does it behave in containers (Go 1.25 cgroup awareness)?
78. What is a goroutine leak, give three common causes, and how do you detect one?
79. How does `sync.WaitGroup` work, and what are common misuses (calling `Add` inside the goroutine)?
80. What does `WaitGroup.Go` (Go 1.25) simplify?
81. What is a data race, and how does `go test -race` detect it?
82. Why is a data race in Go undefined behavior and not just a "stale value"?

## 8. Channels & Select

83. What is the difference between unbuffered and buffered channels?
84. What happens when you send to or receive from a `nil` channel?
85. What happens when you send to, receive from, or close a closed channel?
86. Who should close a channel, and why should receivers never close?
87. How does `for range` over a channel work, and when does it terminate?
88. How does `select` choose when multiple cases are ready?
89. How do you implement a timeout with `select` and `time.After`, and what is the leak risk in loops?
90. How does a `default` case make a non-blocking send or receive?
91. What are directional channel types (`chan<-`, `<-chan`), and why use them in APIs?
92. How do you implement a worker pool with channels?
93. How do you implement fan-out and fan-in?
94. How do you implement a pipeline with cancellation?
95. How do you implement a semaphore with a buffered channel?
96. When should you use channels vs mutexes?
97. How is a channel implemented internally (`hchan`, lock, send/recv queues, ring buffer)?

## 9. The `sync` Package & Atomics

98. What is the difference between `sync.Mutex` and `sync.RWMutex`, and when is RWMutex slower?
99. Why must you never copy a `sync.Mutex`, and how does `go vet` catch it?
100. Is Go's mutex reentrant? What happens if you lock it twice in the same goroutine?
101. How does `sync.Once` work, and what do `sync.OnceFunc` and `OnceValue` add?
102. What is `sync.Cond`, and when would you use it instead of channels?
103. How does `sync.Pool` work, when is it emptied, and what should you put in it?
104. What do the `sync/atomic` typed values (`atomic.Int64`, `atomic.Pointer[T]`) offer over the old functions?
105. When is a lock-free approach with atomics appropriate, and when is it over-engineering?
106. What does `errgroup.Group` do, and how does `SetLimit` help?
107. What does `singleflight` do, and when would you use it in front of a cache?

## 10. Context

108. What is `context.Context` for, and what are its four main capabilities?
109. What is the difference between `WithCancel`, `WithTimeout`, `WithDeadline`, and `WithValue`?
110. Why must you always call the `cancel` function, even on success?
111. Why should context be the first parameter and never stored in a struct?
112. What should and should not go into `context.WithValue`, and why use unexported key types?
113. How does cancellation propagate from parent to child contexts?
114. What do `context.WithoutCancel`, `AfterFunc`, and `WithCancelCause` (Go 1.20/1.21) solve?
115. How do you make a long-running loop or blocking call respect context cancellation?

## 11. Generics

116. How do type parameters and constraints work in Go?
117. What is the difference between `any`, `comparable`, and custom constraint interfaces?
118. What does the `~` tilde mean in a constraint (`~int | ~string`)?
119. When should you use generics, and when are interfaces the better choice?
120. Why can't methods have their own type parameters?
121. How are generics implemented (GC shape stenciling with dictionaries), and what does that mean for performance?
122. What do the `slices`, `maps`, and `cmp` standard packages provide?
123. What are range-over-func iterators (`iter.Seq`, Go 1.23), and how do they work?

## 12. Packages, Modules & Tooling

124. What is the difference between a package and a module?
125. How does `go.mod` work, and what do `require`, `replace`, `exclude`, and `toolchain` do?
126. What is Minimal Version Selection, and how does it differ from npm or Cargo resolution?
127. What is `go.sum`, and what does it protect against?
128. How does semantic import versioning work for `v2+` modules?
129. What is the `internal/` directory convention, and how does the compiler enforce it?
130. How do you avoid import cycles, and what does an import cycle usually say about the design?
131. What do `go vet`, `staticcheck`, and `golangci-lint` catch?
132. What are build tags and build constraints, and how are they used?
133. What are Go workspaces (`go.work`) for?
134. How do you manage tool dependencies (`tool` directive in Go 1.24)?

## 13. Testing

135. How do table-driven tests work, and why are they idiomatic?
136. What do `t.Run`, `t.Parallel`, `t.Cleanup`, and `t.Helper` do?
137. What is the loop-variable pitfall with `t.Parallel` in subtests before Go 1.22?
138. How do you write benchmarks, and what do `b.ReportAllocs` and `b.Loop` (Go 1.24) do?
139. How does fuzz testing work in Go?
140. How do you mock dependencies in Go without a mocking framework?
141. How do you test HTTP handlers and clients with `httptest`?
142. How do you write integration tests against a real database (testcontainers, build tags)?
143. What does `testing/synctest` (Go 1.24/1.25) help with for concurrent code and time?
144. How do you measure and interpret test coverage?

---

# Part B — Senior+ & Staff

## 14. Runtime & Scheduler

145. Explain the GMP model (goroutines, machines/threads, processors).
146. How does work stealing work in the Go scheduler?
147. What happens when a goroutine makes a blocking syscall (handoff of the P)?
148. How does the network poller (netpoller with epoll/kqueue) let goroutines block on I/O cheaply?
149. What is asynchronous preemption (Go 1.14), and what problem did it solve?
150. How are goroutine stacks grown and shrunk (contiguous stack copying)?
151. What does `runtime.LockOSThread` do, and when is it necessary?
152. How do cgo calls interact with the scheduler, and why are they expensive?
153. What does a "goroutine dump" (`SIGQUIT`, `pprof/goroutine`) tell you, and how do you read it?

## 15. Memory Management & Garbage Collection

154. How does escape analysis decide between stack and heap allocation, and how do you inspect it (`-gcflags=-m`)?
155. Which common patterns cause values to escape to the heap?
156. How does Go's concurrent tri-color mark-and-sweep GC work?
157. What is a write barrier, and why does the GC need one?
158. What do `GOGC` and `GOMEMLIMIT` control, and how do you tune them for a container with a memory limit?
159. What is GC assist, and why can allocation-heavy goroutines slow down?
160. Why is Go's GC non-generational and non-compacting, and what are the trade-offs?
161. How do you reduce allocations in a hot path?
162. What is the Go memory model, and what does "happens-before" mean?
163. Why is double-checked locking without atomics broken in Go?
164. How do finalizers and `runtime.AddCleanup` (Go 1.24) work, and why should you avoid relying on them?
165. What are `weak` pointers (Go 1.24), and when would you use them?
166. What does `unsafe` allow, what rules govern `unsafe.Pointer`, and when is it justified?

## 16. Performance & Profiling

167. How do you profile CPU, heap, allocations, blocking, and mutex contention with `pprof`?
168. What is the difference between `inuse_space` and `alloc_space` in a heap profile?
169. How do you use `go tool trace` to diagnose latency and scheduler issues?
170. What is profile-guided optimization (PGO) in Go, and how much does it help?
171. When does the compiler inline functions, and how do you check?
172. How does bounds-check elimination work, and how can you help the compiler?
173. What is false sharing, and how can it hurt concurrent Go code?
174. How would you find why p99 latency spiked while p50 stayed flat in a Go service?
175. How do you avoid lock contention on a hot shared map (sharding, copy-on-write, atomics)?

## 17. Networking, HTTP & gRPC

176. How does `net/http` handle each request (goroutine per connection), and what limits throughput?
177. Why must you always close `resp.Body` and drain it to reuse connections?
178. Why is `http.DefaultClient` dangerous in production, and which timeouts must you set?
179. What do `ReadTimeout`, `ReadHeaderTimeout`, `WriteTimeout`, and `IdleTimeout` protect against on a server?
180. How does `http.Transport` connection pooling work (`MaxIdleConnsPerHost`)?
181. How does the enhanced `ServeMux` routing (Go 1.22) work with methods and path wildcards?
182. How do you implement graceful shutdown for HTTP and gRPC servers with signal handling?
183. How do you write HTTP middleware for logging, recovery, auth, and tracing?
184. How do gRPC interceptors, deadlines, metadata, and streaming work in Go?
185. How do you stream large uploads without buffering them fully in memory (`io.Pipe`, `io.Copy`)?
186. How do you implement retries with exponential backoff and jitter correctly?
187. How do you implement rate limiting (`golang.org/x/time/rate`) and load shedding in a Go service?

## 18. Design, Architecture & Idioms at Scale

188. How do you structure a large Go service (layout, package boundaries, avoiding a `utils` package)?
189. How do you do dependency injection in Go without a framework, and when are Wire or Fx worth it?
190. How do you design a public Go library API for backward compatibility?
191. What makes a Go API hard to misuse (zero-value usefulness, constructors, unexported fields)?
192. How do you use `log/slog` for structured logging, and how do you attach request-scoped fields?
193. How do you add OpenTelemetry tracing and metrics to a Go service?
194. How would you design a concurrent cache with TTL, size limits, and stampede protection?
195. How would you design an in-process job scheduler that survives restarts and doesn't double-run jobs?
196. How would you build a high-throughput event consumer (Kafka) with ordering, batching, and backpressure?
197. How do you handle configuration, secrets, and feature flags cleanly in Go services?
198. When is Go the wrong language for a system, and what would you choose instead?
199. How do you run safe database access in Go (`database/sql` pool settings, `pgx`, transactions, `sqlc` vs ORM)?
200. How do you reason about and test a concurrent system for correctness beyond `-race`?

## 19. Debugging Production Scenarios

201. A Go service's memory grows steadily until it is OOM-killed — how do you investigate?
202. Goroutine count climbs from 1k to 500k over a day — what do you check first?
203. A service deadlocks with "all goroutines are asleep" in production — how do you find the cause?
204. CPU is high but throughput is low — what profiles do you take and what do you look for?
205. A service gets throttled under its Kubernetes CPU limit — how do `GOMAXPROCS` and CFS quotas interact?
206. Latency spikes every few minutes — how do you tell whether GC is to blame?
207. A downstream dependency slows down and your service collapses — what design changes prevent cascading failure?
208. How do you debug a running Go process safely (Delve, pprof endpoints, core dumps)?
