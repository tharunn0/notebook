# Operating Systems — Question Bank (Fundamentals → Senior → Staff Backend)

Part A covers OS fundamentals. Part B covers Linux internals at the depth a senior backend engineer needs. Part C covers senior+ and staff-level performance, debugging, and systems design topics.

---

# Part A — Fundamentals

## 1. OS Basics & Architecture

1. What does an operating system do, and what are its core responsibilities?
2. What is the difference between kernel mode and user mode, and why does the CPU enforce it?
3. What are protection rings, and which ones do Linux and Windows actually use?
4. What is the difference between monolithic kernels, microkernels, and hybrid kernels? Give examples.
5. What are loadable kernel modules, and what are their risks?
6. What happens, step by step, when a computer boots (firmware/UEFI, bootloader, kernel, init/systemd)?
7. What is an interrupt, and how do hardware interrupts, software interrupts, and exceptions differ?
8. What are top halves and bottom halves (softirqs, tasklets, workqueues) in interrupt handling?
9. What is a trap, and how does the CPU switch from user mode to kernel mode?

## 2. System Calls

10. What is a system call, and how does it work at the CPU level (`syscall` instruction, registers, return)?
11. Why are system calls expensive compared with ordinary function calls?
12. What is the difference between a libc wrapper function and the actual system call?
13. What is the vDSO, and why can `gettimeofday` and `clock_gettime` avoid a full syscall?
14. How did Spectre and Meltdown mitigations (KPTI) affect syscall cost?
15. How do you trace system calls made by a process (`strace`, `ltrace`, `perf trace`)?
16. What does `errno` represent, and why is `EINTR` important to handle?

## 3. Processes

17. What is a process, and what state does the kernel keep for it (PCB / `task_struct`)?
18. What are the process states (new, ready, running, waiting/blocked, terminated), and what causes each transition?
19. How does `fork()` work, and what does copy-on-write optimize?
20. What does `exec()` do, and why are `fork` and `exec` separate calls?
21. What are `vfork`, `posix_spawn`, and `clone`, and when are they preferred over `fork`?
22. What is a zombie process, and how is it cleaned up?
23. What is an orphan process, and what happens to it?
24. What is PID 1's special role, and why does it matter in containers?
25. What are process groups, sessions, and controlling terminals?
26. What is a daemon, and how is one traditionally created (double fork, `setsid`)?
27. What are exit codes, and what do codes like 137 and 143 mean?

## 4. Threads

28. What is the difference between a process and a thread?
29. What do threads in the same process share, and what does each thread own?
30. What is the difference between user-level threads, kernel threads, and the 1:1, N:1, and M:N threading models?
31. How does Linux implement threads (`clone` with shared flags; threads are tasks)?
32. What are green threads and coroutines, and how do Go goroutines and async/await relate to them?
33. What is thread-local storage, and when is it used?
34. How expensive is creating a thread vs creating a process?
35. Why does a thread-per-connection server stop scaling, and what replaced it?

## 5. CPU Scheduling

36. What is a context switch, what does it save and restore, and why is it costly?
37. What is the difference between a voluntary and an involuntary context switch?
38. What is the difference between preemptive and cooperative scheduling?
39. Explain FCFS, SJF, SRTF, round robin, priority, and multilevel feedback queue scheduling.
40. What are throughput, turnaround time, waiting time, and response time in scheduling?
41. What is starvation, and how does aging prevent it?
42. What is priority inversion, and how does priority inheritance solve it (the Mars Pathfinder bug)?
43. How does Linux's Completely Fair Scheduler (CFS) work (virtual runtime, red-black tree)?
44. What is EEVDF, and why did it replace CFS in Linux 6.6?
45. What are `nice` values, and how do they affect scheduling weight?
46. What are real-time scheduling policies (`SCHED_FIFO`, `SCHED_RR`, `SCHED_DEADLINE`)?
47. What is CPU affinity, and when would you pin a process or thread to a core?
48. What does load average actually measure on Linux, and why does it include uninterruptible (D state) tasks?

## 6. Synchronization & Concurrency

49. What is a race condition, and what is a critical section?
50. What properties must a correct critical-section solution have (mutual exclusion, progress, bounded waiting)?
51. What is the difference between a mutex, a semaphore, and a binary semaphore?
52. What is a spinlock, and when is spinning better than sleeping?
53. What is a futex, and how does it make uncontended locks cheap?
54. What are condition variables, and why must you wait in a `while` loop (spurious wakeups)?
55. What is a read-write lock, and when can it be slower than a mutex?
56. What are atomic operations and compare-and-swap (CAS)?
57. What is the ABA problem, and how is it solved?
58. What is lock-free vs wait-free programming?
59. What is RCU (read-copy-update), and why is it widely used in the Linux kernel?
60. What are memory barriers, and why do CPUs and compilers reorder memory operations?
61. Explain the classic problems: producer–consumer, readers–writers, and dining philosophers.

## 7. Deadlocks

62. What are the four Coffman conditions for deadlock?
63. What is the difference between deadlock prevention, avoidance, detection, and recovery?
64. How does the Banker's algorithm work, and why is it rarely used in practice?
65. What is a livelock, and how does it differ from deadlock?
66. How do you prevent deadlocks in application code (lock ordering, timeouts, try-lock)?
67. How do you detect a deadlock in a running process?

## 8. Memory Management

68. What is virtual memory, and what problems does it solve?
69. What is the difference between a virtual address and a physical address?
70. How does paging work, and what is a page table?
71. Why do modern CPUs use multi-level page tables (4-level and 5-level paging)?
72. What is the TLB, and what happens on a TLB miss?
73. What is a TLB shootdown, and why is it expensive on many-core machines?
74. What is a page fault, and what is the difference between minor and major faults?
75. What is demand paging?
76. Explain page replacement algorithms (FIFO, LRU, Optimal, Clock/second chance). What is Belady's anomaly?
77. What is thrashing, and how do you detect and fix it?
78. What is the working set of a process?
79. What is the difference between internal and external fragmentation?
80. What is segmentation, and how does it differ from paging?
81. What is the layout of a process's address space (text, data, BSS, heap, mmap region, stack)?
82. What is the difference between stack and heap allocation?
83. What causes a segmentation fault?
84. What is a stack overflow, and how do guard pages detect it?
85. What is ASLR, and what does it protect against?
86. What is memory-mapped I/O, and what does `mmap` do?
87. What is the difference between private and shared mappings, and file-backed vs anonymous mappings?

## 9. File Systems

88. What is an inode, and what does it store (and not store)?
89. What is the difference between a hard link and a symbolic link?
90. What is a file descriptor, and how do the per-process fd table, open file table, and inode table relate?
91. What happens when two processes open the same file, or when a process forks with open files?
92. What happens if you delete a file that a process still has open?
93. What is the VFS layer in Linux?
94. What is journaling, and how do journaling modes (`data=ordered`, `writeback`, `journal`) differ in ext4?
95. How do ext4, XFS, Btrfs, and ZFS differ (journaling vs copy-on-write, checksums, snapshots)?
96. What are file permissions, `umask`, setuid, setgid, and the sticky bit?
97. What are directory entries, and why do huge directories slow down some operations?
98. Why is `rename()` atomic, and how is it used for safe file updates?
99. What is the difference between `fsync`, `fdatasync`, `sync`, and `O_SYNC`/`O_DSYNC`?
100. Why must you `fsync` the parent directory after creating or renaming a file?
101. What was the "fsyncgate" issue with PostgreSQL, and what did it teach about error handling after a failed fsync?
102. What are sparse files?
103. What are `/proc` and `/sys`, and what can you learn from them?

## 10. I/O

104. What is the difference between blocking, non-blocking, synchronous, and asynchronous I/O?
105. What is the page cache, and how do reads and writes go through it?
106. What are dirty pages, and when does the kernel write them back (`dirty_ratio`, `dirty_background_ratio`)?
107. What is direct I/O (`O_DIRECT`), and why do databases sometimes use it?
108. What is buffered I/O in user space (stdio buffers) vs the kernel page cache?
109. What is DMA, and why does it matter for I/O performance?
110. How do HDDs and SSDs differ in latency, random vs sequential I/O, and failure modes?
111. What are SSD write amplification, TRIM, and wear leveling?
112. What are I/O schedulers (`none`, `mq-deadline`, `bfq`, `kyber`), and why is `none` common for NVMe?
113. What is readahead, and when does it help or hurt?

## 11. Inter-Process Communication & Signals

114. What IPC mechanisms exist (pipes, named pipes, Unix domain sockets, shared memory, message queues, semaphores)?
115. What is the fastest IPC mechanism, and why?
116. When would you use a Unix domain socket instead of TCP on localhost?
117. How can Unix domain sockets pass file descriptors between processes?
118. What is a signal, and how are signals delivered and handled?
119. Why can't `SIGKILL` and `SIGSTOP` be caught or ignored?
120. What is the difference between `SIGTERM`, `SIGINT`, `SIGHUP`, `SIGQUIT`, and `SIGKILL`?
121. What does it mean for a function to be async-signal-safe, and why is `printf` in a signal handler unsafe?
122. What is `SIGPIPE`, and why do servers usually ignore it?
123. What does `SIGCHLD` do, and how is it related to reaping zombies?

---

# Part B — Linux Internals for Backend Engineers (Senior)

## 12. Networking in the Kernel

124. What is a socket, and what happens in the kernel on `socket`, `bind`, `listen`, `accept`, and `connect`?
125. What are the SYN queue and the accept queue, and what do `somaxconn` and the `listen` backlog control?
126. What happens when the accept queue overflows?
127. What is the C10K problem, and how did event-driven I/O solve it?
128. How do `select`, `poll`, and `epoll` differ, and why does `epoll` scale better?
129. What is the difference between level-triggered and edge-triggered `epoll`?
130. What is the thundering herd problem on `accept`, and what do `EPOLLEXCLUSIVE` and `SO_REUSEPORT` do?
131. What is `io_uring`, how does it differ from `epoll` and Linux AIO, and why is it significant?
132. What are kernel socket send and receive buffers, and how do they relate to TCP windows?
133. What is the `TIME_WAIT` state, why does it exist, and when does it cause port exhaustion?
134. What do `SO_REUSEADDR` and `SO_REUSEPORT` do?
135. What is Nagle's algorithm, what is `TCP_NODELAY`, and how does Nagle interact with delayed ACKs?
136. What is ephemeral port exhaustion, and how do you diagnose and fix it?
137. What do `conntrack` table limits mean, and how can they drop connections on busy hosts?
138. What are zero-copy techniques (`sendfile`, `splice`, `MSG_ZEROCOPY`), and when do they help?

## 13. Resource Limits & Isolation

139. What are `ulimit` and `rlimit`, and which limits matter for servers (open files, processes, core size)?
140. Why do servers hit "too many open files," and how do you fix it properly?
141. What are cgroups, and how do CPU shares, CPU quotas, and memory limits work?
142. What is the OOM killer, how does it choose a victim, and what does `oom_score_adj` do?
143. What is the difference between the cgroup OOM killer and the global OOM killer?
144. What is memory overcommit, and what do the `vm.overcommit_memory` modes do?
145. What is swap, and should production servers or Kubernetes nodes use it?
146. What do `vm.swappiness` and memory pressure (PSI) tell you?
147. What are namespaces, and how do they combine with cgroups to make containers?
148. What are seccomp, AppArmor, and SELinux, and what does each restrict?

## 14. Memory in Practice

149. What is the difference between RSS, VSZ, PSS, and USS?
150. Why does `free` show little "free" memory on a healthy Linux server?
151. How do `malloc` implementations work (`brk` vs `mmap`, arenas), and why might freed memory not return to the OS?
152. What are jemalloc and tcmalloc, and why do some services switch to them?
153. What are huge pages and Transparent Huge Pages (THP), and why do databases like Redis and MongoDB often recommend disabling THP?
154. What is NUMA, and how does non-uniform memory access affect performance?
155. How do you detect and debug a memory leak in a native or managed process?

---

# Part C — Senior+ & Staff (Performance, Debugging & Systems Design)

## 15. CPU & Hardware Architecture

156. How do CPU caches (L1, L2, L3) work, and what are cache lines?
157. What is cache coherence, and how does the MESI protocol work?
158. What is false sharing, and how do you detect and fix it?
159. What is the difference between spatial and temporal locality, and how do data structures exploit them?
160. What are branch prediction and speculative execution, and how did they lead to Spectre?
161. What is SIMD, and when does it matter for backend workloads?
162. What is hyper-threading (SMT), and how can it hurt latency-sensitive workloads?
163. What is CPU frequency scaling, and how can power states (C-states) increase tail latency?
164. What does "mechanical sympathy" mean for writing high-performance software?

## 16. Performance Analysis & Observability

165. What is the USE method (utilization, saturation, errors), and how do you apply it to CPU, memory, disk, and network?
166. Which Linux tools would you use first on a slow server (`uptime`, `top`, `vmstat`, `iostat`, `mpstat`, `sar`, `ss`, `dmesg`)?
167. What do user, system, iowait, steal, and softirq CPU time mean, and what does high steal time indicate?
168. How do you use `perf` to profile CPU usage, and what are flame graphs?
169. What is off-CPU analysis, and when does it explain latency that CPU profiles miss?
170. What is eBPF, and how do tools like bcc and bpftrace enable safe kernel-level tracing in production?
171. How do kprobes, uprobes, and tracepoints differ?
172. How do you find which process is causing high disk I/O or high network usage?
173. What is a core dump, how do you enable one, and how do you analyze it?
174. How do you debug a process stuck in D (uninterruptible sleep) state?

## 17. Production Scenarios

175. A server's load average is 50 but CPU usage is low — what is happening?
176. A process is killed with no logs — how do you find out whether the OOM killer did it?
177. Latency spikes every 30 seconds on a database host — what OS-level causes would you investigate?
178. Disk shows as full but `du` can't find the files — what is the likely cause?
179. A service suddenly cannot open new outbound connections — what are the likely OS-level causes?
180. A Java or Go service is slow only on large multi-socket machines — what could NUMA or scheduling have to do with it?
181. A container is CPU-throttled even though the node is mostly idle — why?
182. After a kernel upgrade, network throughput dropped — how would you approach the investigation?

## 18. OS Concepts Behind Systems Design

183. How do databases use the OS (page cache vs their own buffer pool, `fsync`, `mmap`, `O_DIRECT`)?
184. Why do many database authors argue against using `mmap` for buffer management?
185. How does a write-ahead log rely on OS and disk guarantees for durability?
186. What guarantees do disks actually give for durability (volatile write caches, FUA, power-loss protection)?
187. What is a torn write, and how do databases protect against partial page writes?
188. How do event loops (Node.js, nginx, Redis) use the OS to handle many connections with few threads?
189. How do language runtimes (Go, JVM) build their own schedulers on top of OS threads?
190. What is kernel bypass (DPDK, RDMA, SPDK), and when is it worth the complexity?
191. How do virtualization and hypervisors work (Type 1 vs Type 2, hardware-assisted virtualization, VM exits)?
192. How do containers, microVMs, and VMs differ in isolation, startup time, and overhead?
193. How would you tune a Linux host for a high-throughput, low-latency network service?
194. What trade-offs come with tuning kernel parameters, and how do you roll such changes out safely across a fleet?
