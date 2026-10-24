# Java / Spring Boot / Microservices — Complete Interview Question Bank

A **stack-wise consolidated compilation** of every question collected so far. All sources (the senior Java/Spring full question bank, the stack-based set, both loose question lists, and the screenshot coding questions) have been merged and regrouped by technology stack instead of by source. Question wording is kept as in the originals; only exact duplicates were collapsed. Questions only — **no full answers**. Each question is followed by a 🔑 *keyword line*: the minimum set of terms to hit so you can recall the structure of a good answer and explain it in your own words.

> **Format:** **bold = the question** · 🔑 *italic = recall keywords*

## Contents

1. [Core Java & OOP Fundamentals](#core-java--oop-fundamentals)
2. [JVM Internals, Memory Model & Garbage Collection](#jvm-internals-memory-model--garbage-collection)
3. [Collections & Data Structures](#collections--data-structures)
4. [Concurrency, Multithreading & Virtual Threads](#concurrency-multithreading--virtual-threads)
5. [Java 8+ — Functional, Streams, Records & Sealed Classes](#java-8--functional-streams-records--sealed-classes)
6. [Exceptions & Error Handling](#exceptions--error-handling)
7. [Spring Core & Spring Boot](#spring-core--spring-boot)
8. [JPA / Hibernate / Transactions](#jpa--hibernate--transactions)
9. [Databases — SQL, NoSQL & Query Optimization](#databases--sql-nosql--query-optimization)
10. [REST API Design & Web Layer](#rest-api-design--web-layer)
11. [Microservices Architecture & Resilience](#microservices-architecture--resilience)
12. [Kafka, Messaging & Event-Driven Architecture](#kafka-messaging--event-driven-architecture)
13. [Security — AuthN/AuthZ, OWASP, TLS & Secrets](#security--authnauthz-owasp-tls--secrets)
14. [Performance, Caching & Scalability](#performance-caching--scalability)
15. [Production Troubleshooting, Logging & Observability](#production-troubleshooting-logging--observability)
16. [Testing & Code Quality](#testing--code-quality)
17. [Cloud, DevOps, Docker, Kubernetes & CI/CD](#cloud-devops-docker-kubernetes--cicd)
18. [Design Patterns & SOLID](#design-patterns--solid)
19. [System Design & Design Scenarios](#system-design--design-scenarios)
20. [Kotlin vs Java](#kotlin-vs-java)
21. [Version Control, SDLC & Documentation](#version-control-sdlc--documentation)
22. [Behavioral / Managerial](#behavioral--managerial)
23. [Coding Problems](#coding-problems)

---

## Core Java & OOP Fundamentals

**1. How does String pooling work? Why is String immutable, and what's the performance implication of heavy + concatenation in loops?**
   🔑 *String pool in heap, interning, immutability → thread-safe + hashcode cache + security, O(n²) in loops, StringBuilder, intern()*

**2. Explain generics type erasure — why can't you do new T[] or instanceof T?**
   🔑 *Erasure at compile time, bytecode has Object/bound, no reified types, no new T[], unchecked cast, bridge methods, Class<T> token workaround*

**3. Difference between polymorphism (compile-time vs runtime) with code**
   🔑 *Compile-time = overloading/static binding, runtime = overriding/dynamic dispatch, vtable, reference type vs object type*

**4. Explain differences between abstract class and interface (Java 8+ default methods).**
   🔑 *Abstract = state + constructor + single inheritance; interface = contract + multiple inheritance + default/static/private methods; diamond resolution*

**5. Control statement in java**
   🔑 *if/else, switch, loops (for, while, do-while), break/continue/return, labeled break*

**6. Control flow statement in java**
   🔑 *Selection, iteration, jump; enhanced for; switch expressions (Java 14+)*

**7. What is polymorphism? Explain JVM's role in Dynamic Polymorphism.**
   🔑 *Same reference, many forms; JVM invokevirtual + vtable/itable lookup, late binding, method table per class*

**8. Which object-oriented concepts are supporting polymorphism and how?**
   🔑 *Inheritance + overriding + upcasting + interfaces + dynamic binding; overloading for compile-time*

**9. Instance block and other things on class, interface , on which**
   🔑 *Instance block runs before constructor body, per object; static block once at class load; interfaces allow only static/default methods, no instance block*

**10. Benefits of marker interface**
   🔑 *Empty interface = metadata tag (Serializable, Cloneable, RandomAccess), JVM/library checks via instanceof, now replaced by annotations*

**11. StringPoolTrick**
   🔑 *Literals interned in pool, new String() = heap object, == vs equals, intern(), compile-time constant folding*

**12. Constructor can be final ?**
   🔑 *No — final means no override; constructors aren't inherited/overridden, so it's a compile error*

**13. Assert property**
   🔑 *assert expr : msg; disabled by default, enable with -ea; for internal invariants not for validating user input*

**14. Reflection in Java**
   🔑 *Class.forName, getDeclaredMethods/Fields, setAccessible, newInstance; used by Spring/Jackson/JUnit; cost = slow + breaks encapsulation + no compile-time safety*

**15. Print to console without a main method**
   🔑 *Static block (pre-Java 7), or JVM runs static init at class load; modern JVMs require main — demo with static initializer + System.exit*

**16. Can we have a static block — and what is a static block?**
   🔑 *Yes — static{} runs once at class loading (initialization phase), in order, before main; used for constants/driver loading*

**17. How can we prevent singleton cloning?**
   🔑 *Override clone() and throw CloneNotSupportedException, or don't implement Cloneable; enum singleton is immune*

**18. Can we override the main method, or overload it?**
   🔑 *Overload yes (different params), override no — main is static → method hiding, JVM only calls String[] version*

**19. Does overloading only work for objects?**
   🔑 *Overloading is compile-time on reference/declared type — applies to primitives too (widening → autoboxing → varargs resolution order)*

**20. How does `new` work internally?**
   🔑 *Memory alloc in heap (TLAB), default init, super constructor chain, instance blocks, constructor body, reference assigned*

**21. Role of the constructor**
   🔑 *Initialize state, enforce invariants, no return type, chained via this()/super(), implicit no-arg*

**22. How is Java pass-by-value and not pass-by-reference?**
   🔑 *Everything is pass-by-value — for objects the copied value is the reference; reassigning the param doesn't affect caller, mutating the object does*

**23. Can a singleton instance be serialized? What is serialization, and how do you ensure it can be serialized?**
   🔑 *Implement Serializable, serialVersionUID; singleton breaks on deserialize → implement readResolve() returning the instance, or use enum*

**24. Constructor in an abstract class — why do we have one even though we don't create objects of abstract classes? Can we make it private — what compilation issues arise?**
   🔑 *Constructor runs during subclass instantiation (super() chain) to init common state; private constructor = no subclass can extend → compile error*

**25. Object class methods**
   🔑 *equals, hashCode, toString, getClass, clone, finalize, wait, notify, notifyAll*

**26. Immutable class — name any immutable class**
   🔑 *final class, private final fields, no setters, defensive copies, init in constructor; String, Integer/wrappers, LocalDate, BigDecimal*

**27. Custom immutable class**
   🔑 *final class + final fields + no setter + deep copy in getter/constructor + no 'this' escape in constructor*

**28. How to break a singleton**
   🔑 *Reflection on private constructor, serialization, cloning, multiple classloaders, multithreading in lazy init — fixes: enum, readResolve, throw in constructor, double-checked locking + volatile*

**29. Benefits of a marker interface**
   🔑 *Tag interface, no methods, instanceof check, JVM/API special handling, annotation alternative*

**30. How does the compiler force a class to implement all methods of an interface?**
   🔑 *Compiler checks abstract contract; class must be abstract or implement all — unimplemented method = compile error; interface methods are implicitly public abstract*

**31. How do overriding and overloading work internally? What is late (lazy) binding?**
   🔑 *Overloading = static/early binding via compile-time signature; overriding = dynamic/late binding via invokevirtual + vtable at runtime*

**32. Class and interface relationship — Object, class, interface, abstract class hierarchy**
   🔑 *Object is root of all classes; interfaces don't extend Object implicitly but can be assigned; abstract class = partial impl; class implements interface, extends one class*

**33. Why `@Override` annotation — is it mandatory?**
   🔑 *Not mandatory but recommended — compile-time safety against typos/signature mismatch, documents intent, catches wrong @Override on static/private*

**34. Constructor, init block — what happens when we put a return type in a constructor?**
   🔑 *Instance init block runs before constructor body; adding return type turns it into a normal method — silently never called*

**35. What is finalize in the Object class?**
   🔑 *Called once by GC before reclaim; deprecated (Java 9+), unpredictable, resurrection risk, delays collection → use try-with-resources/Cleaner*

**36. Parent-child relationship across classes, interfaces and abstract classes**
   🔑 *Single class inheritance, multiple interface implementation, IS-A, upcasting, covariant returns, super keyword, constructor chaining*

**37. Lower bound, upper bound, generics**
   🔑 *? extends T = upper bound (producer, read-only), ? super T = lower bound (consumer, write), PECS, unbounded ?, no instantiation of wildcard*

**38. Static keyword**
   🔑 *Class-level, loaded once, shared across instances; static method/field/block/nested class; no 'this', can't be overridden (hidden)*

**39. Access modifiers and their rules, hierarchy, parent-child rules**
   🔑 *private < default(package) < protected < public; overriding can widen but not narrow; class can only be public/package-private*

**40. Constructor rules — parent-child, instance block, static block**
   🔑 *Super constructor first, then instance blocks in declaration order, then constructor body; static blocks once at class load (parent first)*

**41. Switch statement internally**
   🔑 *tableswitch (dense, O(1)) vs lookupswitch (sparse, binary search); String switch = hashCode + equals; enum = ordinal array*

**42. Can we use abstract and final together on a class?**
   🔑 *No — abstract needs subclass, final forbids it; contradiction, compile error (abstract methods in final class also illegal)*

**43. Can a constructor be final?**
   🔑 *No — constructors are never inherited or overridden, so final is meaningless and a compile error*

**44. Why do we need a singleton, and what is not a singleton?**
   🔑 *One instance per JVM, shared state/costly resource (config, cache, connection pool); not singleton = prototype/new per request; Spring beans are singleton per container not per JVM*

**45. Static instance and constructor for a class**
   🔑 *Static instance = class-level shared, created at class load; constructor private to control instantiation; static init order*

**46. Instance block and other members — on a class vs on an interface, which is allowed where?**
   🔑 *Class: instance block, static block, constructor allowed; interface: only constants, default/static/private methods — no instance/static init block or constructor*

## JVM Internals, Memory Model & Garbage Collection

**47. Walk through JVM memory areas — heap, stack, metaspace, PC register, native method stack.**
   🔑 *Heap (young/old, shared), stack per thread (frames, locals), metaspace (class metadata, native mem), PC register, native method stack, code cache*

**48. How does garbage collection actually work? Compare G1, ZGC, and Parallel GC — when would you pick each?**
   🔑 *Mark-sweep-compact, reachability from GC roots, generational hypothesis; G1 = region-based balanced, ZGC/Shenandoah = sub-ms pauses large heaps, Parallel = throughput batch*

**49. What causes a memory leak in Java despite GC? Give a real example (e.g., static collections, unclosed listeners, ThreadLocal misuse).**
   🔑 *Static/long-lived collections, unclosed streams/listeners, ThreadLocal in pools, ClassLoader leaks, cache without eviction, inner-class holding outer; heap dump + dominator tree*

**50. Explain the difference between shallow and deep class loading issues — how have you debugged a ClassNotFoundException/NoSuchMethodError from a dependency conflict?**
   🔑 *Class loader delegation (bootstrap→platform→app), jar hell/duplicate versions, NoSuchMethodError = compile vs runtime mismatch; mvn dependency:tree, shading, -verbose:class*

**51. How does garbage collection work? Explain generational GC.**
   🔑 *Weak generational hypothesis, Eden→Survivor S0/S1→Old, minor vs major/full GC, aging threshold, promotion, stop-the-world*

**52. JVM memory area**
   🔑 *Heap, stack, metaspace, PC register, native method stack, code cache; per-thread vs shared*

**53. How leak memory in java application even JVM has GC , give real example**
   🔑 *Static map cache, ThreadLocal not removed, listener not deregistered, unbounded cache, substring/keys mutated in HashMap*

**54. Island of isolation**
   🔑 *Group of objects referencing each other but unreachable from GC roots — still collected (reference counting would leak, reachability doesn't)*

**55. Why override finalize? Do we have finalize in a record class? Do record classes have methods?**
   🔑 *Rarely — resource cleanup legacy; records have no finalize override need (finalize deprecated), records DO have methods (accessors, equals/hashCode/toString, compact constructor, static, custom)*

**56. How does cleanup work in GC?**
   🔑 *Mark reachable from roots → sweep unreachable → compact; finalization queue, reference queues, Cleaner/PhantomReference*

**57. Which thread performs GC? What is a daemon thread, and is it only for GC?**
   🔑 *GC threads are daemon JVM-internal threads; daemon = background thread that doesn't block JVM exit (also used by timers, Spring schedulers)*

**58. Marking, deletion and compaction**
   🔑 *Mark (reachability), sweep (free), compact (defragment, moves objects + updates refs) — avoids fragmentation, costs pause time*

**59. Generational GC strategy and how age is calculated**
   🔑 *Most objects die young; age counter incremented per survived minor GC in survivor space; MaxTenuringThreshold (default 15) → promotion; dynamic age based on survivor occupancy*

**60. "Die young" hypothesis**
   🔑 *Weak generational hypothesis — cheap Eden allocation (bump pointer) + bulk copy of few survivors = minor GC is cheap*

**61. Heap areas for GC — young generation, tenured, permanent generation**
   🔑 *Young (Eden + S0 + S1), Old/Tenured, PermGen replaced by Metaspace (native memory) in Java 8*

**62. Eden space, survivor space**
   🔑 *Eden = new allocations (TLAB), 2 survivor spaces for copying collector, always one empty, objects copied S0↔S1 each minor GC*

**63. What is promotion?**
   🔑 *Object moving from young to old gen after surviving N minor GCs or when survivor space is full / object too big (humongous → direct to old)*

**64. Minor GC vs major GC**
   🔑 *Minor = young gen only, frequent + fast, STW; major/full = old gen + metaspace, long pause; concurrent cycles in G1/CMS*

**65. Use of method area, native memory and PC registers**
   🔑 *Method area/metaspace = class metadata, constant pool, static vars; PC register = current instruction per thread; native memory = direct buffers, threads, JNI*

## Collections & Data Structures

**66. equals()/hashCode() contract — what breaks if you override one but not the other? What happens if you mutate a key used in a HashMap?**
   🔑 *Equal objects must have equal hashCodes; broken → lost entries in HashMap/HashSet, duplicate in set; mutating key changes bucket → entry unreachable (memory leak)*

**67. Difference between HashMap, ConcurrentHashMap, and Hashtable internals — how does ConcurrentHashMap achieve thread safety without locking the whole map (segment locking in Java 7 vs CAS/bins in Java 8+)?**
   🔑 *HashMap not thread-safe; Hashtable fully synchronized (legacy); CHM Java7 = segment lock, Java8 = CAS on empty bin + synchronized on head node, treeify, no null keys*

**68. Difference between List, Set, Map — when would you use LinkedHashMap vs TreeMap?**
   🔑 *List = ordered/duplicates, Set = unique, Map = key-value; LinkedHashMap keeps insertion/access order (LRU), TreeMap sorted by Comparable/Comparator O(log n)*

**69. Explain equals()/hashCode() contract and where it breaks things.**
   🔑 *Contract, consistency, hash bucket lookup, HashSet duplicates, Lombok @EqualsAndHashCode, mutable key pitfall*

**70. Why the size of the array is fixed? Think from the perspective of JVM. And state JVM's role in it. What needs to be changed to make the array dynamic?**
   🔑 *Array length stored in object header at allocation, contiguous fixed memory block, no realloc; dynamic = ArrayList copy-on-grow (Arrays.copyOf, 1.5x)*

**71. Collection class, Like I have custom user object and I have to add two same type of object in set like new USer("a", 12) and new User("a", 12), so set will allow me to add ?**
   🔑 *HashSet uses hashCode + equals — without overriding them, two logically equal Users are both added (identity hash)*

**72. How or what can we use to do this**
   🔑 *Override equals/hashCode (or Lombok @Data/@EqualsAndHashCode), use record, or TreeSet with Comparator*

**73. LiskList, tree, graph, and various algorithms and implementation**
   🔑 *LinkedList (node/pointers), trees (BST, AVL, red-black, trie), graphs (adjacency list/matrix, BFS/DFS, Dijkstra, topological sort)*

**74. We can access lisklist by index but why array is faster the linklist**
   🔑 *Array = contiguous memory + O(1) index + CPU cache locality; LinkedList index = O(n) traversal + pointer chasing + object header overhead*

**75. Hash table**
   🔑 *Legacy synchronized map, no null key/value, Enumeration, replaced by ConcurrentHashMap; initial capacity 11, rehash *2+1*

**76. Load factor of a HashMap**
   🔑 *Default 0.75 — trade-off between space and collision probability; resize (double + rehash) when size > capacity × loadFactor*

**77. Can a HashMap cause a memory leak?**
   🔑 *Yes — mutable key (hashCode change), strong refs in static map, no eviction; use WeakHashMap or bounded cache (Caffeine)*

**78. Comparator vs Comparable**
   🔑 *Comparable = natural order, compareTo, modifies the class; Comparator = external/multiple orders, compare, lambdas + thenComparing/reversed*

**79. What type of collection have you used in your project — give a real purpose for using it**
   🔑 *ArrayList (read-heavy list), HashMap (lookup/cache), HashSet (dedup), LinkedHashMap (LRU/ordered response), ConcurrentHashMap (shared counters), TreeMap (sorted reports), BlockingQueue (producer-consumer)*

**80. Can we set a class object as a key in a HashMap?**
   🔑 *Yes — must override hashCode/equals and ideally be immutable; mutating fields after insertion makes the entry unfindable*

**81. Why does Java HashMap use a red-black tree? Why convert to a red-black tree, what is the time complexity, and why change from linked list to red-black tree?**
   🔑 *Collisions in one bucket degrade to O(n) list → treeify at 8 nodes (and capacity ≥ 64) giving O(log n), untreeify at 6; protects against hash-collision DoS*

**82. Why are Strings considered good keys for a map?**
   🔑 *Immutable → hashCode can't change, cached hashCode, well-distributed hash, final equals/hashCode, interning*

**83. Hashmap vs LinkedHashMap vs Hashtable**
   🔑 *HashMap = unordered, not sync, 1 null key; LinkedHashMap = insertion/access order via doubly-linked list; Hashtable = legacy, fully synchronized, no nulls*

**84. Relation between hashCode() and equals()**
   🔑 *equals true ⇒ hashCode equal; hashCode equal ⇏ equals; used by hash-based collections for bucket + equality check*

**85. Why ConcurrentHashMap after Hashtable?**
   🔑 *Hashtable locks whole map (single lock, poor concurrency, legacy Enumeration); CHM gives bucket-level/CAS concurrency, weakly-consistent iterators, scalable*

**86. LinkedList, tree, graph — various algorithms and implementations**
   🔑 *Traversal (BFS/DFS), recursion vs iteration, balanced trees, heaps/PriorityQueue, adjacency list, cycle detection*

**87. We can access a LinkedList by index, so why is an array faster than a LinkedList?**
   🔑 *Random access O(1) vs sequential O(n), cache locality, memory overhead per node, insert/delete at middle is where LinkedList wins (with an iterator)*

## Concurrency, Multithreading & Virtual Threads

**88. Explain the Java Memory Model (JMM): happens-before, visibility, volatile vs synchronized.**
   🔑 *Happens-before edges (lock release→acquire, volatile write→read, thread start/join, final fields), visibility vs atomicity, CPU caches + reordering, volatile = visibility only, synchronized = visibility + mutual exclusion*

**89. Difference between CompletableFuture, Future, and reactive (Mono/Flux) — when do you reach for which?**
   🔑 *Future = blocking get, no chaining; CompletableFuture = non-blocking composition (thenApply/thenCompose/allOf), custom executor; Mono/Flux = lazy, backpressure, full reactive stack*

**90. Explain synchronized internals — monitor locks, biased/lightweight/heavyweight locking.**
   🔑 *Object header mark word, monitorenter/monitorexit, biased (removed in 15+) → thin/lightweight CAS → inflated heavyweight (OS mutex, park/unpark), lock elision/coarsening*

**91. Producer-consumer implementation — how would you build it with BlockingQueue vs wait/notify vs Semaphore?**
   🔑 *BlockingQueue (ArrayBlockingQueue/LinkedBlockingQueue) = simplest, handles signalling; wait/notify in while loop guarding spurious wakeups; Semaphore for permits; bounded queue = backpressure*

**92. What is a deadlock, and how have you actually detected/diagnosed one in production (thread dumps, jstack)?**
   🔑 *Circular wait on 2+ locks; 4 Coffman conditions; jstack/thread dump "Found one Java-level deadlock", VisualVM/JFR, ThreadMXBean.findDeadlockedThreads; fix = lock ordering, tryLock timeout*

**93. ExecutorService — how do you size a thread pool for a CPU-bound vs I/O-bound workload? What's the danger of Executors.newCachedThreadPool() in production?**
   🔑 *CPU-bound ≈ cores+1; I/O-bound = cores × (1 + wait/service time) or Little's law; cachedThreadPool = unbounded threads + SynchronousQueue → OOM under load; prefer bounded queue + rejection policy*

**94. ThreadLocal — what's the memory leak risk in a pooled-thread environment (e.g., app servers), and how do you avoid it?**
   🔑 *Pooled thread never dies → ThreadLocalMap entry (weak key, strong value) keeps value alive; classloader leak in app servers; always remove() in finally / use try-with-resources filter*

**95. Explain CountDownLatch vs CyclicBarrier vs Phaser with a real use case.**
   🔑 *CountDownLatch = one-shot wait for N events (startup/fan-in); CyclicBarrier = reusable rendezvous of N threads (batch rounds); Phaser = dynamic parties + phases*

**96. What is false sharing, and have you ever had to deal with it?**
   🔑 *Independent variables on the same 64-byte cache line → false invalidation across cores; fix with @Contended/padding, LongAdder; seen in high-throughput counters*

**97. Scenario: A service under load starts throwing intermittent TimeoutException only during peak traffic. Walk me through how you'd diagnose whether it's thread starvation, GC pause, connection pool exhaustion, or downstream latency.**
   🔑 *Check GC logs (pause/old gen), thread dump (BLOCKED/WAITING count), Hikari pool metrics (pending threads), downstream p99 latency, APM traces; correlate before blaming code*

**98. Explain multithreading basics: synchronized, volatile, ExecutorService.**
   🔑 *synchronized (mutual exclusion + happens-before), volatile (visibility, no atomicity), ExecutorService (pool, Future, shutdown), atomic classes, concurrent collections*

**99. Counter in synchronize method, multiple thread working on it but still producing wrong totals, what are the possible cause**
   🔑 *synchronized on different objects/instances, static vs instance lock, non-atomic read-modify-write outside the lock, field not volatile with other readers, multiple JVMs → use AtomicLong/LongAdder or DB*

**100. What is Java Thread Pool? How it works.**
   🔑 *Reusable worker threads + task queue; corePoolSize, maxPoolSize, keepAlive, BlockingQueue, ThreadFactory, RejectedExecutionHandler; avoids thread creation cost*

**101. Before java 8, how can we do parallel programming**
   🔑 *Raw Thread/Runnable, ExecutorService + Future (Java 5), Fork/Join + RecursiveTask (Java 7), CountDownLatch, manual partitioning*

**102. Why future is blocking**
   🔑 *Future only exposes blocking get()/isDone polling — no callback or composition API; CompletableFuture added completion callbacks*

**103. How to avoid dead lock**
   🔑 *Global lock ordering, tryLock with timeout, reduce lock scope, single lock/lock-free structures, avoid nested locks and calling foreign code while holding a lock, timeouts on DB rows*

**104. Dead lock example how to fix it**
   🔑 *Two threads locking A→B and B→A; reproduce, take jstack, fix via consistent ordering or tryLock + backoff*

**105. CompletableFuture**
   🔑 *supplyAsync/runAsync, thenApply/thenAccept/thenCompose/thenCombine, allOf/anyOf, exceptionally/handle, custom executor, join vs get, orTimeout*

**106. getNotify() and notifyAll()**
   🔑 *notify() wakes one arbitrary waiter, notifyAll() wakes all; must hold the monitor, use while loop (spurious wakeups), prefer notifyAll or Condition to avoid lost signals*

**107. When does volatile fail?**
   🔑 *Compound actions (count++), check-then-act, invariants across multiple fields, arrays/objects (only the reference is volatile), long chains — use atomics/locks instead*

**108. Why can't we use (volatile) on local variables and static fields?**
   🔑 *Local vars live on the thread's own stack → never shared, so visibility is irrelevant; static fields CAN be volatile (static volatile is legal) — what's illegal is volatile + final*

**109. How do you make a class thread-safe and a collection thread-safe?**
   🔑 *Immutability, confinement, synchronized/locks, atomic classes, concurrent collections, Collections.synchronizedXxx, copy-on-write, defensive copies, no 'this' escape*

**110. Normal thread creation vs Executor class**
   🔑 *new Thread = manual lifecycle, no reuse/queueing, no result; ExecutorService = pooling, Future/CompletableFuture, scheduling, graceful shutdown, rejection policy*

**111. `getNotify()` / `notify()` and `notifyAll()`**
   🔑 *notify vs notifyAll, must be inside synchronized, IllegalMonitorStateException, wait in while loop, Condition.await/signal*

**112. Lock striping and object pooling — how do they help us?**
   🔑 *Lock striping = split one lock into N per-bucket locks (ConcurrentHashMap, LongAdder cells) → less contention; object pooling reuses expensive objects (connections, threads, buffers) → less alloc/GC*

**113. Sync vs asynchronous**
   🔑 *Sync = caller blocks, simple, immediate consistency; async = non-blocking, callbacks/queues, better throughput, harder error handling and ordering, eventual consistency*

**114. Distributed locking**
   🔑 *Redis SET NX PX + unique token + Lua release (Redlock caveats), Zookeeper ephemeral node, DB row lock/advisory lock; TTL, fencing token, idempotency as backup*

**115. Real scenario where a deadlock happened in production for a banking system**
   🔑 *Two transfers A→B and B→A locking accounts in opposite order; batch job + online update hitting same rows; fix = order by account id, short transactions, retry on deadlock error code*

**116. How do you detect a deadlock in production?**
   🔑 *jstack/kill -3, ThreadMXBean.findDeadlockedThreads exposed via actuator, JFR, APM thread states, DB deadlock logs (MySQL SHOW ENGINE INNODB STATUS), alert on BLOCKED thread count*

**117. Virtual thread limitations — how do you control them? What happens if we create too many virtual threads? There is no thread pool for virtual threads, so does it affect resources, memory, or anything else?**
   🔑 *Not for CPU-bound work, pinning on synchronized/native frames, unbounded creation → memory + downstream overload, no pooling by design; control with Semaphore, structured concurrency, bounded queues*

**118. What happens if we create a separate virtual thread for each request?**
   🔑 *Fine — thread-per-request becomes cheap (few KB stack on heap); but downstream (DB pool, external API) becomes the bottleneck → must rate-limit*

**119. Can virtual threads cause connection pool overload?**
   🔑 *Yes — millions of virtual threads can all request connections; pool stays small → queueing/timeouts; use semaphore/bulkhead sized to the pool*

**120. What is Project Loom?**
   🔑 *JEP 444 virtual threads, continuations, carrier threads, structured concurrency, scoped values; goal = simple blocking code with async scalability*

**121. CPU → OS thread → platform thread → virtual thread relationship**
   🔑 *CPU core runs OS/kernel thread; platform thread = 1:1 wrapper over OS thread (~1MB stack); virtual thread = JVM-scheduled, mounted on a carrier (ForkJoinPool) and unmounted when it blocks*

**122. How do you handle ExecutorService and its parameters in a production system — how do you decide pool size?**
   🔑 *corePoolSize/max, bounded queue size, named ThreadFactory, rejection policy (CallerRuns for backpressure), timeouts, metrics (queue depth, active threads), separate pools per dependency (bulkhead)*

**123. What is a thread dump and a heap dump, and how do you monitor application health?**
   🔑 *Thread dump = thread states/stacks (jstack, kill -3) for deadlock/blocking; heap dump = objects/memory (jmap, HeapDumpOnOutOfMemoryError, MAT); plus actuator/Micrometer, Prometheus+Grafana, JFR*

**124. How do you find a blocked thread?**
   🔑 *Thread dump → BLOCKED state + "waiting to lock <0x..>" + owner thread, repeat dumps to spot stuck threads, ThreadMXBean, JFR thread-park events*

**125. How does CompletableFuture work internally?**
   🔑 *Backed by a completion stack of dependent actions; default ForkJoinPool.commonPool (or supplied executor), non-blocking callbacks fired on completion, AsyncSupply/UniApply nodes, exception propagated as CompletionException*

**126. Why is Future blocking?**
   🔑 *get() parks the caller until done, no callback hooks, no composition — compute pushed back to caller thread; CompletableFuture/reactive solves it*

**127. Before Java 8, how could we do parallel programming?**
   🔑 *Thread/Runnable, ExecutorService + invokeAll, Fork/Join work-stealing, manual chunking, CountDownLatch for joining*

**128. How do concurrent data structures work — ConcurrentHashMap, concurrent set, CopyOnWriteArrayList?**
   🔑 *CHM = CAS + per-bin synchronized + treeify, weakly-consistent iterators; CopyOnWriteArrayList/Set = snapshot on write, great read-heavy; ConcurrentSkipListMap = sorted lock-free; BlockingQueues*

**129. Collections internal working — map, set, list, ArrayList**
   🔑 *HashMap (array of bins + hash + treeify), ArrayList (Object[] + grow 1.5x), LinkedList (doubly-linked nodes), HashSet (backed by HashMap), TreeMap (red-black tree)*

**130. How does a map know a key is unique? Same for set**
   🔑 *hash(key) → bucket index → compare hashCode then equals in bin; Set delegates to a Map with a dummy value; TreeSet/TreeMap use compareTo/compare instead*

**131. How does ArrayList know it needs to increase its size — if we create an ArrayList with 2 elements vs 100 elements, at which point and how does it know to resize?**
   🔑 *Default capacity 10 (lazy on first add), grows when size == capacity via grow() → newCapacity = old + old/2 (1.5x) with Arrays.copyOf; new ArrayList<>(100) presizes and avoids copies*

**132. How does a thread pool work — what happens with no tasks and with too many tasks?**
   🔑 *Core threads first → queue → extra threads up to max → rejection handler; idle non-core threads die after keepAlive; too many tasks + unbounded queue = memory growth, bounded queue = rejection/backpressure*

**133. We don't have a thread pool for virtual threads, so how do we control virtual threads, and is there any option to reuse them?**
   🔑 *By design unlimited and not pooled (cheap, no reuse); control via Semaphore permits, bounded executors for downstream, structured concurrency scopes, rate limiting*

**134. How does CAS work for objects, how does volatile work for objects, and how do Atomic classes work?**
   🔑 *CAS = compare-and-swap on a memory address (Unsafe/VarHandle, lock-free, ABA risk → AtomicStampedReference); volatile gives visibility of the reference not the object's fields; AtomicReference/AtomicInteger use CAS loops, LongAdder uses striping*

**135. How does synchronization work in Java?**
   🔑 *Monitor per object, synchronized method = lock on this / class object for static, happens-before on release-acquire, reentrant, blocking; Lock/ReentrantLock gives tryLock, fairness, conditions*

**136. Can 2 threads execute on the same object simultaneously — if there is no locking? If yes, how?**
   🔑 *Yes — different methods or non-synchronized methods run concurrently on the same object; only threads contending for the same monitor are serialized; immutable/stateless objects are always safe*

**137. Counter in a synchronized method with multiple threads still producing wrong totals — what are the possible causes?**
   🔑 *Shared non-atomic increment, lock on different objects, static vs instance scope, volatile ≠ atomic, multiple instances/JVMs, unsynchronized read path*

**138. How to check thread dump and heap dump in production?**
   🔑 *jcmd <pid> Thread.print / GC.heap_dump, jstack, jmap, kill -3, -XX:+HeapDumpOnOutOfMemoryError, k8s ephemeral container + kubectl cp, analyze in MAT/VisualVM/fastThread*

**139. Deadlock example and how to fix it**
   🔑 *Classic lock-ordering deadlock; fix with ordered acquisition, tryLock(timeout), single coarse lock, or lock-free design*

## Java 8+ — Functional, Streams, Records & Sealed Classes

**140. What's new/relevant from Java 11 → 17 → 21 you've actually used: records, sealed classes, pattern matching for switch, virtual threads (Project Loom)? Have you used virtual threads in production — what changed for your thread-pool sizing assumptions?**
   🔑 *Records, sealed classes/interfaces, pattern matching for switch + instanceof, text blocks, var, Optional/Stream API additions, HttpClient, virtual threads (21), ZGC*

**141. What's new in Java 17/21 (records, sealed classes, virtual threads)?**
   🔑 *Java 17 LTS: records, sealed, switch pattern matching (preview), text blocks; Java 21 LTS: virtual threads, structured concurrency (preview), record patterns, sequenced collections*

**142. Functional interface, default method, optional class, Stream API, parallel Stream, advancetage of parallel stream, how parallel stream work on memory CPU and alls ,**
   🔑 *@FunctionalInterface + SAM, default/static methods, Optional (of/ofNullable/map/orElseThrow), Stream pipeline (lazy intermediate + terminal), parallelStream uses common ForkJoinPool — good for large CPU-bound splittable data, bad for I/O/small sets/shared state*

**143. Pick in stream**
   🔑 *peek() = intermediate, for debugging/logging only, skipped if the pipeline can be short-circuited/optimized, not for mutation*

**144. Parellalel stream vs sequesntial stream**
   🔑 *Sequential = one thread, ordered, predictable; parallel = ForkJoinPool.commonPool, splitting via Spliterator, needs stateless + associative ops, overhead of split/merge, blocks common pool on I/O*

**145. Main usages of flat map and map**
   🔑 *map = 1:1 transform; flatMap = 1:many + flattens nested streams/Optionals (List<List<T>> → List<T>, chaining Optional)*

**146. Why function interface don't have more than one abstract method**
   🔑 *A lambda must map to exactly one method signature — more than one abstract method makes the target ambiguous; default/static/Object methods (equals/toString) don't count*

**147. Can a functional interface extend another interface?**
   🔑 *Yes, but it stays functional only if the total abstract method count is 1 (e.g. extending an interface with no new abstract methods)*

**148. Streams internally**
   🔑 *Spliterator source → lazy intermediate ops build a pipeline of stages → terminal op triggers traversal, single pass, short-circuiting (findFirst/limit), ReferencePipeline + Sink chain*

**149. Why `@FunctionalInterface` annotation — is it mandatory?**
   🔑 *Not mandatory — it's a compile-time check that the interface has exactly one abstract method and documents intent; lambdas work on any SAM interface*

**150. Why can't a functional interface have more than one abstract method?**
   🔑 *Ambiguous lambda target typing; SAM contract; Object's public methods and default/static methods excluded*

**151. Pattern matching and method references — how do they work internally?**
   🔑 *instanceof pattern binds a typed variable (compiler inserts cast), switch patterns compile to invokedynamic + type switch bootstrap; method refs desugar to the same invokedynamic/LambdaMetafactory as lambdas*

**152. Record class — constructor, methods, static members; can it participate in inheritance?**
   🔑 *Implicit private final fields + canonical constructor + accessors + equals/hashCode/toString; compact constructor for validation; can have static members and custom methods; implicitly final → no inheritance, but can implement interfaces*

**153. Sealed class — with state, inheritance, static, singleton**
   🔑 *sealed + permits, subclasses must be final/sealed/non-sealed, same module/package; can hold state and static members; enables exhaustive switch; singleton via enum/sealed hierarchy*

**154. Second largest number in an array using Streams**
   🔑 *stream().distinct().sorted(Comparator.reverseOrder()).skip(1).findFirst() or boxed().collect with TreeSet; handle duplicates and empty/size<2 case*

**155. Peek in Stream**
   🔑 *Debugging aid, intermediate, lazily executed, may be elided by the JVM, avoid side effects*

**156. Parallel stream vs sequential stream**
   🔑 *Thread count, ForkJoinPool common pool, Spliterator split cost, ordered vs unordered, thread-safe collectors, measure before using*

**157. Main usages of map vs flatMap**
   🔑 *map for transformation, flatMap to flatten nested structures / merge streams / unwrap Optional, used heavily on List<Order> → List<Item>*

## Exceptions & Error Handling

**158. Checked vs unchecked exceptions — what's your team's actual policy, and do you agree with it?**
   🔑 *Checked = recoverable, compiler-enforced, pollutes signatures/lambdas; unchecked = programming errors; modern practice = unchecked + @ControllerAdvice + domain exceptions, never swallow, wrap with context*

**159. finally vs try-with-resources — what happens if both the try block and the close() throw?**
   🔑 *finally can swallow the original exception if it throws; try-with-resources closes in reverse order and attaches the close() failure as a suppressed exception (getSuppressed), preserving the primary one*

**160. What are checked vs unchecked exceptions? When to use each?**
   🔑 *Checked (IOException, SQLException) = caller must handle/declare; unchecked (RuntimeException, IllegalArgument, NPE) = bug or unrecoverable; Error = JVM level*

**161. Executor framework, Exception handling, Exception type, throw vs throws,**
   🔑 *ExecutorService swallows exceptions in submit() (stored in Future) but surfaces them in execute(); throw = statement, throws = declaration; UncaughtExceptionHandler, always get() the Future*

**162. `@ControllerAdvice` exception checking**
   🔑 *@RestControllerAdvice + @ExceptionHandler per exception type, ordered most-specific-first, ResponseEntity + ProblemDetail (RFC 7807), @ResponseStatus, handles only controller-layer exceptions (not filters)*

**163. Can we have multiple finally blocks? If yes, which one runs?**
   🔑 *Only one finally per try, but nested try blocks each have their own — innermost runs first, outer afterwards; finally always runs except System.exit/JVM crash*

**164. Exceptions and their rules — parent-child**
   🔑 *Catch subclass before superclass (unreachable catch otherwise), overriding method can't throw broader checked exceptions (can narrow or throw unchecked), multi-catch requires unrelated types*

**165. Checked and unchecked exceptions at compile stage — how does the compiler check and force handling of checked exceptions?**
   🔑 *Checked = compile-time "handle or declare" rule enforced by the compiler against the method's throws clause; unchecked (RuntimeException/Error subtree) is exempt; erasure/lambdas can't propagate checked exceptions*

## Spring Core & Spring Boot

**166. Explain the Spring bean lifecycle end-to-end (instantiation → dependency injection → BeanPostProcessor → init → destroy).**
   🔑 *Scan/definition → instantiate (constructor) → populate deps → *Aware callbacks → BeanPostProcessor.before → @PostConstruct/afterPropertiesSet/init-method → BPP.after (proxy created here) → in use → @PreDestroy/destroy*

**167. Constructor injection vs field injection — why is constructor injection preferred at scale, and what does it protect against (circular dependencies, immutability, testability)?**
   🔑 *Guarantees fully-initialized immutable final fields, fails fast on missing/circular deps at startup, testable without a container/reflection, no hidden deps; field injection needs reflection and hides a bloated constructor*

**168. How does Spring Boot auto-configuration actually work under the hood (@EnableAutoConfiguration, spring.factories / AutoConfiguration.imports, conditional annotations)?**
   🔑 *@EnableAutoConfiguration → AutoConfigurationImportSelector reads META-INF/spring/...AutoConfiguration.imports (spring.factories pre-2.7) → @Conditional evaluation (OnClass/OnMissingBean/OnProperty) → ordered, user beans win; debug with --debug conditions report*

**169. Explain bean scopes: singleton, prototype, request, session — what's a real bug you've seen from injecting a prototype bean into a singleton incorrectly?**
   🔑 *singleton (default, per container), prototype (new per lookup), request/session/application (web); prototype injected into singleton is created once → use ObjectProvider/@Lookup/scoped proxy; singleton holding request state = thread-safety bug*

**170. @Transactional — how does the proxy work, and why does a self-invocation call (calling an @Transactional method from within the same class) silently not get intercepted?**
   🔑 *Spring creates a JDK dynamic/CGLIB proxy; the TransactionInterceptor starts/commits/rolls back around the call; self-invocation bypasses the proxy (calls this.method directly) → no transaction; fix = self-inject, AopContext, or move to another bean*

**171. Explain propagation types (REQUIRED, REQUIRES_NEW, NESTED) with a real scenario for each.**
   🔑 *REQUIRED joins or creates (default); REQUIRES_NEW suspends the outer tx (independent audit/log commit); NESTED uses savepoints (partial rollback, JDBC only); also MANDATORY/SUPPORTS/NEVER/NOT_SUPPORTED*

**172. How do you handle configuration across environments (dev/stage/prod) — profiles, @ConfigurationProperties, Spring Cloud Config, Vault?**
   🔑 *application-{profile}.yml + spring.profiles.active, @ConfigurationProperties typed binding + validation, @Profile beans, env vars/K8s ConfigMap-Secret, Spring Cloud Config/Vault, never commit secrets*

**173. What does Spring Boot Actuator give you in production, and which custom health indicators/metrics have you built?**
   🔑 */health (+ readiness/liveness groups), /metrics via Micrometer → Prometheus, /info, /env, /loggers (runtime log level), /httpexchanges, /threaddump, /heapdump; custom HealthIndicator for DB/Kafka/downstream; secure and expose selectively*

**174. AOP in Spring — how have you used it for cross-cutting concerns (logging, auditing, retry)? What are the proxy limitations (final methods/classes, self-invocation)?**
   🔑 *@Aspect + pointcut + @Around/@Before/@AfterThrowing for logging, auditing, metrics, retry, security; proxy-based → no self-invocation, no final/private/static methods, only Spring beans; AspectJ weaving for the rest*

**175. Explain the difference between @Component, @Service, @Repository, @Controller beyond "just semantics" — what does @Repository actually add (exception translation)?**
   🔑 *All are @Component stereotypes; @Repository adds persistence exception translation to DataAccessException; @Controller/@RestController enable handler mapping; @Service is semantic + a hook for pointcuts*

**176. Difference between @RestController and @Controller.**
   🔑 *@RestController = @Controller + @ResponseBody → returns body serialized by HttpMessageConverters; @Controller returns view names resolved by a ViewResolver*

**177. What is @Qualifier used for, and when do you need it?**
   🔑 *Disambiguates multiple beans of the same type (with @Primary as the default winner); match by bean name or custom qualifier annotation; alternative = inject List/Map of beans*

**178. What does @SpringBootApplication actually bundle (@Configuration, @EnableAutoConfiguration, @ComponentScan)?**
   🔑 *@SpringBootConfiguration (@Configuration) + @EnableAutoConfiguration + @ComponentScan of the current package downward*

**179. What's the difference between @RequestParam, @PathVariable, and @RequestBody?**
   🔑 *@RequestParam = query/form param (required, defaultValue); @PathVariable = URI template segment; @RequestBody = deserialized JSON body via HttpMessageConverter + @Valid*

**180. What does @Async do, and what's the gotcha with calling an @Async method from the same class?**
   🔑 *Runs the method on a TaskExecutor returning void/Future/CompletableFuture; needs @EnableAsync; proxy-based so self-invocation runs synchronously; also private/static methods are skipped; define your own executor + exception handler*

**181. What's a WebFlux/reactive stack, and when would you not use it over traditional Spring MVC?**
   🔑 *Non-blocking Netty + Reactor (Mono/Flux), backpressure, few threads for high concurrency; avoid when the stack is blocking (JDBC), team is unfamiliar, debugging/stack traces matter, or load is modest — MVC + virtual threads is often simpler*

**182. Explain application.yml profile-specific overrides and property precedence order.**
   🔑 *application.yml → application-{profile}.yml override; precedence: devtools < properties/yml in jar < external config < profile-specific < env vars < system props < command line args < @TestPropertySource*

**183. How does Spring Boot auto-configuration work internally (@Conditional, spring.factories/AutoConfiguration.imports)**
   🔑 *@Conditional family, AutoConfiguration.imports, @AutoConfiguration ordering (before/after), @ConfigurationProperties defaults, @ConditionalOnMissingBean lets user config win*

**184. Bean scopes, bean lifecycle, @PostConstruct/@PreDestroy**
   🔑 *Scopes (singleton/prototype/request/session/websocket/application), lifecycle callbacks @PostConstruct/@PreDestroy vs InitializingBean/DisposableBean, prototype beans are not destroyed by the container*

**185. Spring Security filter chain — how does a request flow through it?**
   🔑 *SecurityFilterChain of ~15 filters: SecurityContextPersistence → CORS → CSRF → auth filters (UsernamePassword/JWT/BearerToken) → ExceptionTranslation → FilterSecurityInterceptor/AuthorizationFilter; AuthenticationManager → Provider → UserDetailsService → SecurityContextHolder (ThreadLocal)*

**186. How do you implement JWT-based authentication in Spring Security (filter, token validation, SecurityContext)?**
   🔑 *OncePerRequestFilter before UsernamePasswordAuthenticationFilter → parse Bearer header → validate signature/exp/issuer → build Authentication with authorities → SecurityContextHolder → stateless SessionCreationPolicy → entry point returns 401*

**187. @Transactional — propagation types (REQUIRED, REQUIRES_NEW), isolation levels, what happens with self-invocation (proxy issue)**
   🔑 *REQUIRED/REQUIRES_NEW/NESTED, isolation READ_COMMITTED default, rollbackFor checked exceptions, readOnly hint, timeout; self-invocation and private methods bypass the proxy*

**188. Coding: Implement a custom OncePerRequestFilter to validate JWT tokens**
   🔑 *extends OncePerRequestFilter, doFilterInternal, extract Authorization header, jjwt parse/validate, set SecurityContext, chain.doFilter, shouldNotFilter for public paths, handle expired/invalid → 401*

**189. What is actuator, how it is work.**
   🔑 *Production endpoints over HTTP/JMX, auto-configured, Micrometer metrics, HealthIndicator/HealthContributor aggregation, management.endpoints.web.exposure.include, secure behind auth/separate port*

**190. How auto configuration worked internally , beans on condition, auto configuration.import file**
   🔑 *@Conditional evaluation order, AutoConfiguration.imports file, @ConditionalOnMissingBean backs off when you define your own, conditions evaluation report with --debug*

**191. Any functional difference between, @controller, @service @repository**
   🔑 *Functionally they are all @Component (same scanning/bean creation); differences are semantics + @Repository exception translation + @Controller request mapping + AOP pointcut targeting*

**192. Spring vs springboot,**
   🔑 *Spring = modular framework, manual config, external server, XML/JavaConfig; Boot = opinionated auto-config, starters, embedded server, actuator, externalized config, fat jar*

**193. Webserver in springboot**
   🔑 *Embedded Tomcat by default (spring-boot-starter-web), swap to Jetty/Undertow by excluding and adding the starter, Netty for WebFlux; configure server.port/threads, or build a war for an external server*

**194. IOC and DI relationship**
   🔑 *IoC = the principle (container owns object creation/lifecycle); DI = the implementation mechanism (constructor/setter/field); result = loose coupling + testability*

**195. IOC and DI relation**
   🔑 *ApplicationContext as the IoC container, BeanDefinition registry, DI via constructor/setter/field, @Autowired resolution by type then qualifier/name*

**196. @ConfigurationProperties**
   🔑 *Type-safe binding of prefixed properties to a POJO, relaxed binding, @Validated/JSR-303, nested objects/lists, @EnableConfigurationProperties, constructor binding for immutability, refresh scope*

**197. @post construct**
   🔑 *Runs after dependency injection, before the bean is in use; one per class, no args, void; alternatives InitializingBean/@Bean(initMethod); use for warm-up/validation, not heavy blocking work*

**198. flow of springboot application, annotations, methods, architecture, diff modulers and micro service.**
   🔑 *main → SpringApplication.run → environment + profiles → create context → component scan/auto-config → bean creation → embedded server start → DispatcherServlet → filter/interceptor → controller → service → repository → response*

**199. What happens if we override configuration?**
   🔑 *Later sources override earlier ones by precedence (CLI > env > profile yml > application.yml); user-defined beans override auto-config (@ConditionalOnMissingBean); duplicate keys in one file = last wins*

**200. What is a profile?**
   🔑 *A named logical grouping of beans/config for an environment (dev/qa/prod); @Profile on beans/configs + application-{profile}.yml*

**201. How do you activate a profile?**
   🔑 *spring.profiles.active in yml, -Dspring.profiles.active, SPRING_PROFILES_ACTIVE env var, CLI arg, @ActiveProfiles in tests, spring.profiles.group for composites*

**202. AOP — what is it and where/why do we use it, with a use case**
   🔑 *Cross-cutting concerns without touching business code: logging, metrics, auditing, security, retry, caching, transactions; join point/pointcut/advice/aspect/weaving; proxy limitations*

**203. Bean factory vs application context**
   🔑 *BeanFactory = basic lazy DI container; ApplicationContext = BeanFactory + eager singletons + i18n + events + AOP + resource loading + environment — what Boot uses*

**204. Async in Spring**
   🔑 *@EnableAsync + @Async with a dedicated ThreadPoolTaskExecutor, returns void/CompletableFuture, AsyncUncaughtExceptionHandler, no transaction/security context propagation by default, proxy self-invocation gotcha*

**205. WebFlux**
   🔑 *Reactive non-blocking stack (Reactor, Netty), functional routes or annotated controllers, backpressure, R2DBC needed to stay non-blocking, WebClient*

**206. Why is field injection a bad injection style?**
   🔑 *Hidden dependencies, needs reflection/container for tests, no final fields, allows circular deps to slip through, harder to spot a bloated class — use constructor injection*

**207. How do you change the server in Spring (external server instead of Tomcat)?**
   🔑 *Exclude spring-boot-starter-tomcat and add Jetty/Undertow starter; or package as war + SpringBootServletInitializer + provided scope for an external Tomcat/WebLogic*

**208. What happens in the background when we start a Spring application?**
   🔑 *SpringApplication.run: deduce web app type, load ApplicationContextInitializers/Listeners, prepare Environment + profiles, print banner, create context, load bean definitions (scan + auto-config), refresh (BeanFactoryPostProcessors, BeanPostProcessors, singletons, AOP proxies), start embedded server, publish ApplicationReadyEvent, run ApplicationRunners*

**209. Role of `SpringApplication.run()`**
   🔑 *Bootstraps and returns the ApplicationContext — environment prep, context creation/refresh, auto-config, embedded server, lifecycle events, CommandLineRunner/ApplicationRunner execution*

**210. How does auto-configuration work internally, and how does it decide what to enable?**
   🔑 *Classpath-driven: @ConditionalOnClass/OnMissingBean/OnProperty/OnWebApplication evaluated against the environment and existing bean definitions; ordering via @AutoConfigureBefore/After; exclude = property or annotation attribute*

**211. Actuators and how to change the actuator port**
   🔑 *Actuator endpoints under /actuator; management.server.port for a separate port, management.endpoints.web.base-path, exposure.include/exclude, secure with Spring Security*

**212. How do you manage service/management endpoints — what if we want to add new ones?**
   🔑 *management.endpoints.web.exposure.include, custom @Endpoint/@WebEndpoint + @ReadOperation/@WriteOperation, custom HealthIndicator/InfoContributor, Micrometer custom metrics, document + secure them*

**213. `@Data` — when is it safe and when not? How can it create bugs silently?**
   🔑 *Lombok @Data = getters/setters/equals/hashCode/toString/RequiredArgsConstructor; dangerous on JPA entities (equals/hashCode on all fields + lazy loading + bidirectional toString → StackOverflow/N+1), mutable DTOs, accidental setters; prefer @Getter + explicit equals/hashCode on the id, or records*

**214. IoC storage and DI internal working**
   🔑 *Container keeps a ConcurrentHashMap of singletons + BeanDefinitionRegistry; DI resolves by type → qualifier → name via DefaultListableBeanFactory.resolveDependency, with 3-level cache for circular refs*

**215. Bean init and working, scanning, injection**
   🔑 *@ComponentScan basePackages, BeanDefinitionReader, instantiate → populate → aware → BPP → init; @Bean methods in @Configuration (CGLIB-proxied for singleton semantics), lazy vs eager*

**216. Spring Boot application flow**
   🔑 *Request → filter chain → DispatcherServlet → HandlerMapping → HandlerAdapter → interceptor → controller → service → repository → entity/DTO → message converter → response; startup flow as in SpringApplication.run*

**217. How can we remove a cyclic dependency between services?**
   🔑 *Redesign responsibilities (extract a shared service), use events/async messaging, interface + callback, @Lazy or setter injection as a stopgap; Boot 2.6+ fails on circular refs by default*

**218. Spring Batch — why do we need it, and what is a pipeline?**
   🔑 *Chunk-oriented batch: Job → Step → ItemReader/ItemProcessor/ItemWriter, chunk size + commit interval, JobRepository for restart/skip/retry, partitioning/multithreaded steps, scheduling*

**219. I want to read a sheet with 2M rows and store it in MongoDB — how do you build that with Spring Batch?**
   🔑 *FlatFileItemReader/PoiItemReader (or SAX streaming) → processor for validation/mapping → MongoItemWriter, chunk 1k–5k, bulk/ordered unsafe writes, partitioned or multi-threaded steps, skip policy + job restartability, indexes created after load, monitor memory*

**220. Are `@Controller`, `@Service`, `@Repository` singleton by default?**
   🔑 *Yes — default scope is singleton per ApplicationContext (one instance shared); keep them stateless or use request scope/method-local state*

**221. Can we use `@Component` instead of `@Service` and `@Repository`?**
   🔑 *Yes technically, but you lose @Repository exception translation, semantic clarity, and targeted AOP pointcuts; @Controller is required for request mapping*

**222. What if `@Service` is put on a repository class and `@Repository` on a service class?**
   🔑 *It still becomes a bean and usually works, but persistence exceptions are no longer translated to DataAccessException, layering/AOP conventions break, and readers are misled — a code-smell/code-review catch*

## JPA / Hibernate / Transactions

**223. N+1 query problem — how do you detect it and fix it (JOIN FETCH, @EntityGraph, batch fetching)?**
   🔑 *1 parent query + N child queries; detect via show-sql / hibernate.generate_statistics / datasource-proxy / p6spy; fix = JOIN FETCH, @EntityGraph, @BatchSize, default_batch_fetch_size, DTO projection; beware cartesian product with multiple bags*

**224. Explain first-level vs second-level cache in Hibernate — risks of enabling second-level cache in a distributed/multi-instance deployment.**
   🔑 *L1 = Session/EntityManager scope, mandatory, per-transaction identity map; L2 = SessionFactory scope, shared across sessions (Ehcache/Hazelcast/Redis), needs @Cacheable + cache mode; distributed risk = stale reads, invalidation storms, split-brain, needs clustered/replicated cache + TTL; never cache hot-mutating entities*

**225. Lazy vs eager loading — what's LazyInitializationException and how do you avoid it cleanly (not just OpenSessionInView, which has its own trade-offs)?**
   🔑 *LAZY = proxy (bytecode/Javassist) loaded on access; EAGER = join at load; LazyInitializationException when session closed; fix = JOIN FETCH / @EntityGraph / DTO projection / @Transactional boundary at service; OSIV defers connection release → pool exhaustion*

**226. Explain optimistic vs pessimistic locking — when have you used @Version to solve a real concurrency bug (e.g., double-processing a payment)?**
   🔑 *Optimistic = @Version column, no DB lock, OptimisticLockException on stale update, best for low contention; Pessimistic = PESSIMISTIC_READ/WRITE → SELECT ... FOR UPDATE, DB-level lock, risk of deadlock + timeout; payment double-processing → @Version or unique idempotency key*

**227. Scenario: Two threads/instances update the same row concurrently and you're seeing lost updates. How do you fix it — DB constraint, versioning, distributed lock, or redesign?**
   🔑 *Lost update = read-modify-write race; fixes = @Version optimistic lock + retry, SELECT FOR UPDATE pessimistic, atomic UPDATE ... SET x = x - ? WHERE, DB unique constraint, distributed lock (Redis/Zookeeper), or redesign to append-only event/ledger*

**228. N+1 query problem in JPA/Hibernate — how do you fix it? (JOIN FETCH, @EntityGraph)**
   🔑 *JOIN FETCH, @EntityGraph (named/dynamic), @BatchSize, hibernate.default_batch_fetch_size, DTO/interface projection, @Fetch(SUBSELECT)*

**229. Coding: Write a JPQL/native query to find N+1 problem and fix using @EntityGraph**
   🔑 *@EntityGraph(attributePaths = {"orders"}) on repository method; or @Query("select d from Dept d join fetch d.employees"); countQuery needed for pagination*

**230. First-level vs second-level cache**
   🔑 *L1 = mandatory, session-scoped, identity map; L2 = optional, SessionFactory-scoped, provider-backed; query cache is separate and needs L2*

**231. @OneToMany vs @ManyToMany mapping, fetch = LAZY vs EAGER**
   🔑 *@OneToMany = mappedBy on owning side, FK in child table; @ManyToMany = join table, @JoinTable, prefer two @OneToMany to an explicit join entity; LAZY default for collections, EAGER default for @ManyToOne/@OneToOne — always make them LAZY*

**232. Difference between save(), saveAndFlush(), persist(), merge()**
   🔑 *persist = JPA, makes transient→managed, no id return guarantee, throws if detached; save = Hibernate, returns id, can merge detached; merge = copies detached state into a managed copy and returns it; saveAndFlush = save + immediate flush to DB (still same transaction)*

**233. What is the Hibernate dirty checking mechanism?**
   🔑 *On flush, Hibernate compares entity's current state with the snapshot taken at load; generates UPDATE only for changed columns (or all, unless @DynamicUpdate); no explicit save() needed inside a transaction*

**234. We have parent method which is handling 2 child method, if second call fail then how can we revert first method ? Which type of annotations we can use, how can we use. Why we should use?**
   🔑 *Single @Transactional on the parent → one transaction, second failure rolls back both; REQUIRES_NEW on children makes them independent; self-invocation bypasses the proxy; use TransactionTemplate or self-injection; compensating transaction / Saga for cross-service*

**235. How to define annotations on all 3 methods, if what, I don't want to rollback but have to store state on failure.**
   🔑 *Parent @Transactional(REQUIRED); children REQUIRED to share rollback; to persist failure state use REQUIRES_NEW on the audit/state write (own transaction survives rollback) or TransactionSynchronization afterCompletion, or noRollbackFor = X.class*

**236. Pessimistic locks and optimistic locks with DB, how to impolement. If we have pessimistic lock, can other thread read that row at a time ? And what for optimistic lock, like how it behave when 2 thread trying to access it.**
   🔑 *Pessimistic write → SELECT FOR UPDATE blocks other writers; readers may still read (MVCC in Postgres/Oracle) unless PESSIMISTIC_READ/FOR SHARE; optimistic → both read version n, first commit wins, second gets OptimisticLockException → retry; set javax.persistence.lock.timeout*

**237. JPA vs hibernate, internal methods, naming, how to use**
   🔑 *JPA = specification/API (EntityManager, JPQL, annotations); Hibernate = implementation + extras (Criteria, @DynamicUpdate, natural id, filters); Spring Data JPA = repository abstraction with derived query method naming (findByXAndYOrderByZ)*

**238. How JPA works internally**
   🔑 *EntityManagerFactory → EntityManager → persistence context (identity map + dirty checking) → flush order → SQL via dialect → JDBC batch; proxies for LAZY, ActionQueue, FlushMode.AUTO before query/commit*

**239. Soft delete vs hard delete**
   🔑 *Hard delete = row removed, FK/audit loss, unrecoverable; soft delete = is_deleted/deleted_at flag + @SQLDelete + @Where (Hibernate) or @SQLRestriction; costs = index bloat, unique constraint conflicts (use partial index), every query must filter, archival job*

**240. DB locks, and too much DB operation in a single transaction — how do you handle that?**
   🔑 *Keep transactions short — never do remote calls/file IO inside; chunk large writes with batch size + flush/clear; lower isolation where safe; consistent lock ordering to avoid deadlocks; move non-critical work to async/outbox; watch lock_timeout and pool timeout*

**241. 2 services updating the same DB row — how do you handle it?**
   🔑 *Optimistic @Version + retry, pessimistic SELECT FOR UPDATE, atomic conditional UPDATE, DB unique constraint, or distributed lock; for cross-service prefer single writer/ownership and events*

**242. How do you handle simultaneous DB calls on a single row in the trip table?**
   🔑 *Row-level lock (SELECT FOR UPDATE) on trip row, conditional UPDATE ... WHERE status = 'REQUESTED' and check affected rows, @Version, or idempotent state machine; Redis distributed lock for pre-DB gating*

**243. Should we keep rider/driver id directly in the trip table or make joins from trip to rider and driver tables — why use this relationship?**
   🔑 *Store riderId/driverId as @ManyToOne associations (FK) — gives referential integrity + joins; denormalized snapshot columns (name at trip time) for historical accuracy and read speed; avoid bidirectional if not needed*

**244. Lazy fetching vs eager fetching — which is the better approach?**
   🔑 *LAZY by default everywhere, fetch explicitly per use case via JOIN FETCH/@EntityGraph/DTO projection; EAGER causes N+1 and over-fetching and can't be turned off at query time*

**245. What if `@Transactional` is not working — what could be the reasons?**
   🔑 *Self-invocation (proxy bypass), non-public method, class not a Spring bean, checked exception thrown, catching exception without rethrow, wrong PlatformTransactionManager, non-transactional DB engine (MyISAM), proxyTargetClass/final method, @Transactional on the wrong layer, calling from a different thread*

**246. `@Transactional` on a private method**
   🔑 *No effect with proxy-based AOP — proxy can only intercept public (CGLIB: non-private, non-final) methods; works only with AspectJ load-time/compile-time weaving; refactor to a separate bean*

**247. `@Transactional` internally — how does the proxy mechanism work?**
   🔑 *Spring creates a JDK dynamic proxy (interface) or CGLIB subclass; TransactionInterceptor around the call → begin/suspend, bind connection to ThreadLocal via TransactionSynchronizationManager, commit on success, rollback on unchecked exception*

**248. What happens with a checked exception in a transaction — does it continue (safe) or roll back?**
   🔑 *By default only RuntimeException and Error roll back — a checked exception commits the transaction; use @Transactional(rollbackFor = Exception.class) to roll back, or noRollbackFor to deliberately commit*

**249. What exactly is the proxy in a transaction?**
   🔑 *An auto-generated wrapper bean (JDK dynamic proxy / CGLIB subclass) injected in place of the real bean; it applies the transaction advice before/after the target method — self-calls go to `this`, not the proxy, so advice is skipped*

**250. How transactions work, how can you create one yourself, and how does it roll back?**
   🔑 *@Transactional declarative, or programmatic via TransactionTemplate / PlatformTransactionManager (getTransaction, commit, rollback); rollback via undo log/WAL in the DB; setRollbackOnly marks it unrecoverable; savepoints for NESTED*

**251. DB connection methods — save, commit, flush, etc.**
   🔑 *save/persist stage changes in the persistence context; flush pushes SQL to DB (still uncommitted); commit makes it durable; clear/detach evicts; refresh re-reads; flush happens automatically before queries and at commit*

**252. How do you handle transactions in production?**
   🔑 *Short transactions, explicit boundaries at service layer, read-only for queries, timeout + lock timeout, retry on optimistic failure/deadlock, no remote calls inside, idempotency keys, outbox for events, monitor long-running transactions and lock waits*

**253. JPA repository internals**
   🔑 *Spring Data creates a proxy over SimpleJpaRepository at startup; query lookup strategy = derived from method name, @Query, or named query; QueryLookupStrategy + PartTree parsing → Criteria/JPQL; transactions applied via @Transactional on SimpleJpaRepository*

**254. How does Hibernate work — all the terms**
   🔑 *SessionFactory, Session, persistence context, entity states (transient/persistent/detached/removed), dirty checking, flush, cascade types, fetch strategies, dialect, HQL/JPQL, Criteria, caches L1/L2/query, @Version*

## Databases — SQL, NoSQL & Query Optimization

**255. How do you handle schema migrations safely in a live system with zero downtime (Flyway/Liquibase, expand-contract pattern)?**
   🔑 *Expand-contract (parallel change): add nullable column → dual-write → backfill in batches → switch reads → stop writing old → drop; Flyway/Liquibase versioned + repeatable migrations, checksum, baseline; never rename/drop in the same release; avoid long table locks (Postgres: ADD COLUMN with default is safe 11+, CREATE INDEX CONCURRENTLY), lock_timeout + statement_timeout, backward-compatible for N-1 app version during rolling deploy*

**256. Explain isolation levels (Read Committed, Repeatable Read, Serializable) and a real phantom-read/dirty-read bug you've hit.**
   🔑 *READ UNCOMMITTED → dirty read; READ COMMITTED → non-repeatable read (Postgres default); REPEATABLE READ → phantom read (MySQL default, gap locks prevent it); SERIALIZABLE → full isolation via locking/SSI; MVCC snapshots; trade-off isolation vs throughput; classic bug = balance check then debit under READ COMMITTED → double spend*

**257. How do you paginate efficiently over a very large table without OFFSET performance collapse?**
   🔑 *Keyset/seek pagination: WHERE (created_at, id) < (:lastTs, :lastId) ORDER BY ... LIMIT n — uses index, O(1) per page; OFFSET must scan+discard N rows → O(N); also cursor/token-based APIs, covering index, avoid COUNT(*) for totals (estimate or 'has next')*

**258. ACID properties with examples**
   🔑 *Atomicity = all-or-nothing (transfer debit+credit); Consistency = constraints/invariants hold; Isolation = concurrent txns don't interfere; Durability = committed survives crash (WAL/redo log, fsync)*

**259. Indexing — B-tree index, when does an index NOT help (low cardinality columns)?**
   🔑 *B-tree = balanced, sorted, O(log n), supports =, range, ORDER BY, prefix of composite; doesn't help on low cardinality (gender/boolean — use partial/bitmap), leading wildcard LIKE '%x', function on column (unless functional index), small tables, high write churn (index maintenance cost), or when selectivity is poor so optimizer picks a seq scan*

**260. SQL vs NoSQL — when would you pick MongoDB over PostgreSQL?**
   🔑 *SQL for relational integrity, joins, ACID, fixed schema, reporting; MongoDB for flexible/evolving schema, document aggregates read together, horizontal sharding, high write throughput, denormalized read models; Mongo now has multi-doc transactions but joins ($lookup) are weak*

**261. Elasticsearch — inverted index concept, when to use it over SQL LIKE queries**
   🔑 *Inverted index = term → posting list of doc ids, built via analyzer (tokenize, lowercase, stem, stopwords); gives relevance scoring (BM25/TF-IDF), fuzzy, typo tolerance, facets, millisecond full-text; SQL LIKE '%x%' is a full scan with no ranking; ES is near-real-time, not a system of record*

**262. How to optimize query, DB projection, polling, how to find which query is slow, SQL vs NOSQL, disadvantage of both Postgres and mongodb**
   🔑 *EXPLAIN/EXPLAIN ANALYZE, pg_stat_statements, slow query log, APM; projections/DTO to avoid SELECT *, proper indexes, avoid N+1, batch instead of polling (or CDC/outbox), pagination; Postgres cons = vertical scaling limits, connection cost (needs PgBouncer), VACUUM/bloat; Mongo cons = weak joins, memory-hungry working set, eventual consistency on secondaries, schema drift*

**263. What are Cursor and Trigger? When to use trigger?**
   🔑 *Cursor = row-by-row pointer over a result set (explicit/implicit, FETCH), heavy — prefer set-based SQL; Trigger = procedural code fired on INSERT/UPDATE/DELETE (BEFORE/AFTER, row/statement); use for audit trails, enforcing invariants, CDC/outbox; avoid for business logic — hidden side effects, hard to debug and test, cascading triggers*

**264. How rows are stored in Databases. At which physical location it is stored? And how they're inter-relating with each other.**
   🔑 *Row stored in a fixed-size page/block (8KB in Postgres, 16KB in InnoDB) inside a heap/segment file; row located by ctid/RID (page no + slot) or by clustered PK (InnoDB stores row in the leaf of the PK B-tree); secondary indexes store the key + a pointer (ctid or PK); relations connected logically by FK values, not physical pointers; overflow/TOAST for large values*

**265. Difference between SQL and NoSQL. And how to decide when to use SQL or NoSQL,**
   🔑 *SQL = schema-on-write, ACID, joins, strong consistency, vertical scale; NoSQL = schema-on-read, BASE/eventual, horizontal scale, denormalized, access-pattern driven; decide by consistency needs, relationships, query shape, scale, write pattern — often polyglot persistence*

**266. We have an employee table with attributes: first name, Lastname, and embed. Two admin want to change employees with the same id at the same time.**
   🔑 *Lost update race; fix = optimistic @Version/ETag (second admin gets 409 conflict + merge UI), or SELECT FOR UPDATE, or field-level/conditional UPDATE ... WHERE last_modified = ?, plus audit log of who changed what*

**267. How to optimize SQL query**
   🔑 *EXPLAIN ANALYZE first; index the WHERE/JOIN/ORDER BY columns, keep predicates SARGable (no function on column), select only needed columns, avoid correlated subqueries and SELECT *, use EXISTS over IN for large sets, JOIN order/type, update statistics, partition/archive huge tables, batch writes, covering index*

**268. SQL explain join and indexing to optimize**
   🔑 *EXPLAIN shows plan, access path, join algorithm (nested loop / hash / merge), rows estimated vs actual, cost; nested loop good for small driving set + index on inner, hash join for large unsorted sets, merge join for pre-sorted; index join keys, watch for seq scan on big tables and misestimated rows*

**269. Having vs where in sql**
   🔑 *WHERE filters rows before grouping and can't use aggregates; HAVING filters groups after GROUP BY and can use aggregates; order = FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT; push conditions into WHERE for performance*

**270. Diff joins in sql**
   🔑 *INNER (matching only), LEFT/RIGHT OUTER (keep all from one side + NULLs), FULL OUTER, CROSS (cartesian), SELF join, anti-join (NOT EXISTS) and semi-join (EXISTS); watch row multiplication with one-to-many*

**271. Joins in sql, sql vs no sql db**
   🔑 *Joins as above; SQL = joins across normalized tables; NoSQL = embed/denormalize instead of join, or app-side join / $lookup; choose by access pattern*

**272. create sql query with optional filter, date range, ascending and DESC order, range of date, and order to service joins, and customer id, also pagination and batching**
   🔑 *Dynamic filters via JPA Criteria API / Specification (Spring Data) or QueryDSL BooleanBuilder; or @Query with ':x IS NULL OR col = :x'; date range BETWEEN with index; sort via Pageable Sort (whitelist columns to avoid injection); keyset pagination for deep pages; join trip→service→customer with fetch join; JDBC batch/chunking for bulk*

**273. Java specifications, how to decide DB indexing, composite index, order of indexing, at a time two index can be used or only one,**
   🔑 *Index columns used in WHERE/JOIN/ORDER BY with high selectivity; composite index order = equality columns first, then range, then sort (ESR rule); leftmost-prefix rule; covering/INCLUDE index to avoid table lookup; usually one index per table access, but the optimizer can combine via bitmap index scan (Postgres) or index merge (MySQL); cost = slower writes + storage*

**274. DB migration**
   🔑 *Versioned scripts in VCS (Flyway/Liquibase), run on startup or as a pipeline step/init container, one-way forward migrations, checksums, baseline for legacy, rollback via compensating script, idempotent and backward-compatible for rolling deploys, separate DDL from large data backfills*

**275. Column validation changes in DB**
   🔑 *Add constraint as NOT VALID then VALIDATE separately (Postgres) to avoid long locks; widen types safely (varchar length up, int→bigint carefully), never narrow in one step; add nullable then backfill then set NOT NULL; validate in app layer during the transition; CHECK constraints vs Bean Validation*

**276. Postgres and NoSQL DB**
   🔑 *Postgres = relational, ACID, MVCC, rich types (JSONB, arrays), window functions, strong indexing (GIN/GiST/BRIN/partial), extensions (PostGIS); NoSQL = document/key-value/column/graph families, horizontal scale, flexible schema, eventual consistency, access-pattern-first modelling*

**277. RDBMS**
   🔑 *Relational model = tables/rows/columns, keys (PK/FK/unique/composite), normalization, SQL, ACID transactions, constraints, referential integrity, schema-on-write; examples Postgres, MySQL, Oracle, SQL Server*

**278. External SQL framework**
   🔑 *JPA/Hibernate (ORM), Spring Data JPA, MyBatis (SQL mapper), jOOQ (typesafe DSL), Spring JdbcTemplate, QueryDSL; pick by control over SQL vs mapping convenience — jOOQ/MyBatis when SQL is complex or performance-critical*

**279. What is DB sharding?**
   🔑 *Horizontal partitioning across nodes by a shard key; strategies = range, hash, directory/lookup, geo; needs good key (even distribution, no hotspot, matches query pattern); costs = cross-shard joins/transactions, rebalancing/resharding, global unique IDs (Snowflake), fan-out queries; consistent hashing to limit movement*

**280. How do you handle multiple databases in a single service?**
   🔑 *Multiple DataSource beans + @Primary, separate EntityManagerFactory/TransactionManager per source, @EnableJpaRepositories(basePackages, entityManagerFactoryRef), separate packages for entities/repos; no single JTA transaction unless using XA — prefer per-DB transactions + Saga/outbox for consistency; AbstractRoutingDataSource for read/write split*

**281. Migrating an application to a new DB without downtime**
   🔑 *Dual-write or CDC (Debezium) replication, backfill historical data, shadow reads + comparison, feature flag to switch reads, then writes, keep rollback path, verify with reconciliation, finally decommission old DB; strangler-fig, zero-downtime cutover window*

**282. How do you develop schema with the new and old way (backward-compatible schema evolution)?**
   🔑 *Expand-contract: additive-only changes, keep old columns/fields readable, dual-write during transition, version the API/DTO not the table, default values for new fields, tolerant reader; never remove or rename until all consumers migrated*

**283. Count(*) vs count(1) vs count(column)**
   🔑 *COUNT(*) counts rows (optimizer picks cheapest index, fastest); COUNT(1) is identical in practice — no real difference in Postgres/MySQL/Oracle; COUNT(column) skips NULLs in that column; COUNT(DISTINCT col) is much more expensive*

**284. Normalisation**
   🔑 *1NF atomic values, 2NF no partial dependency on part of a composite key, 3NF no transitive dependency, BCNF stricter; benefits = no update/insert/delete anomalies, less redundancy; denormalize deliberately for read performance with a refresh/consistency strategy*

**285. Aggregation stages in MongoDB**
   🔑 *$match early (uses index), $project, $group with accumulators ($sum/$avg/$push), $sort (before $limit), $lookup (left join), $unwind, $facet, $addFields, $out/$merge; pipeline order matters — filter first, allowDiskUse for large sorts, index the $match/$sort fields*

**286. How to optimize a SQL query**
   🔑 *Same as above — EXPLAIN ANALYZE, SARGable predicates, right indexes, avoid SELECT */N+1, EXISTS vs IN, limit result set, statistics, partitioning, caching, batch*

**287. SQL EXPLAIN, joins and indexing to optimize**
   🔑 *Read the plan: scan type, join strategy, estimated vs actual rows, sort/hash spills to disk; add/adjust composite index on join + filter columns, add covering columns, rewrite correlated subquery as join, ANALYZE to refresh stats*

**288. Different joins in SQL**
   🔑 *INNER, LEFT, RIGHT, FULL OUTER, CROSS, SELF, semi (EXISTS), anti (NOT EXISTS); NULL behaviour in outer joins; join on indexed keys*

**289. How do you decide DB indexing, composite index, order of indexing — can two indexes be used at a time or only one?**
   🔑 *High-selectivity columns, composite order = equality → range → sort (ESR), leftmost prefix, covering index, partial index for filtered subsets; the optimizer generally picks one index per table but can combine with bitmap index scan / index merge; too many indexes slow writes and bloat storage*

**290. How do you optimize queries, DB projection, polling — how do you find which query is slow?**
   🔑 *pg_stat_statements / slow query log / EXPLAIN ANALYZE / APM traces; projections (select only needed columns, DTO/interface projection), fetch joins, batch size, pagination, caching; replace polling with event/CDC or long-poll/websocket; index and connection pool tuning*

## REST API Design & Web Layer

**291. How do you design idempotent APIs — why does this matter especially for POST-based payment/transaction endpoints (idempotency keys)?**
   🔑 *Client-generated Idempotency-Key header → unique index on (key, endpoint) in a dedupe table; store request fingerprint + response; same key + same payload replays the stored response, different payload → 409; TTL + cleanup; matters because network timeouts cause retries → double charge; GET/PUT/DELETE are naturally idempotent, POST is not*

**292. Versioning strategy for public APIs — URI versioning vs header vs content negotiation — trade-offs?**
   🔑 *URI (/v1/) = simple, visible, cacheable, but clutters URL and breaks REST purity; header (Accept-Version / custom) = clean URLs, harder to test/cache; content negotiation (Accept: application/vnd.x.v2+json) = purest, poor tooling support; always version only on breaking change, support N-1, publish deprecation window + Sunset header*

**293. How do you handle partial failures in a request that fans out to 3 downstream services?**
   🔑 *Decide per-call criticality: fail-fast vs degrade; aggregate with CompletableFuture.allOf / Mono.zip + per-call timeout + circuit breaker; return 207 Multi-Status or a partial payload with an errors block; compensating action or Saga for writes; retry only idempotent calls with backoff + jitter; never leave the client in an unknown state*

**294. Explain HATEOAS — have you actually used it, or is it mostly theoretical in your experience?**
   🔑 *Hypermedia As The Engine Of Application State — responses embed _links (self, next, cancel) so clients navigate by links not hardcoded URLs; Spring HATEOAS, HAL; Richardson Maturity Model level 3; in practice rarely used — clients are tightly coupled anyway, extra payload, limited tooling*

**295. Designing pagination, filtering, and sorting for a large collection endpoint — cursor-based vs offset-based, and why.**
   🔑 *Offset = simple, random page access, but O(N) scan and unstable under concurrent inserts; cursor/keyset = stable, index-friendly, O(1), opaque token encoding last sort key, but no jump-to-page; whitelist sortable/filterable fields (injection + index coverage), cap page size, return hasNext instead of exact totals*

**296. How do you version and evolve a contract without breaking existing clients (backward compatibility, consumer-driven contracts, Pact)?**
   🔑 *Additive-only changes, tolerant reader, never remove/rename/retype a field, new fields optional with defaults; consumer-driven contract tests (Pact/Spring Cloud Contract) in CI, schema registry for events, semantic versioning, deprecation headers + sunset policy, parallel run of old and new*

**297. Difference between PUT and PATCH.**
   🔑 *PUT = full replace, idempotent, missing fields reset/null; PATCH = partial update, not necessarily idempotent, JSON Merge Patch (RFC 7396) or JSON Patch (RFC 6902 op/path/value); PATCH needs careful null-vs-absent semantics (use Optional/JsonNullable)*

**298. Difference between SOAP and REST — have you had to maintain a legacy SOAP integration?**
   🔑 *SOAP = XML-only, WSDL contract, envelope, WS-Security/WS-AtomicTransaction, protocol-agnostic, heavy, built-in standards; REST = resources + HTTP verbs, JSON, stateless, lightweight, cacheable; legacy SOAP via wsimport/JAXB or Spring WS, CXF client*

**299. REST vs GraphQL vs gRPC — when would you choose each? (gRPC for internal high-perf service-to-service, GraphQL for flexible client queries, REST for simplicity/public APIs)**
   🔑 *REST = public APIs, simplicity, caching, ubiquitous tooling; GraphQL = client-driven queries, avoids over/under-fetching, one endpoint, schema + resolvers, but N+1 (DataLoader), caching and rate limiting are harder; gRPC = HTTP/2 + Protobuf, binary, bidirectional streaming, codegen, low latency for internal service-to-service, poor browser support (needs grpc-web)*

**300. SOAP vs REST — why would a system (like payments) still use SOAP?**
   🔑 *Legacy banking/payment rails (ISO 20022, SWIFT), formal WSDL contract and codegen, WS-Security (message-level signing/encryption, not just transport), WS-AtomicTransaction, non-repudiation, regulatory/partner mandate*

**301. How do you version a REST API?**
   🔑 *URI path, request header, Accept media type, or query param; version only on breaking change; keep controllers separate per version and share a service layer; map v1 DTO → common domain → v2 DTO; deprecate with Sunset header and metrics on old version usage*

**302. How do you handle API versioning?**
   🔑 *Same as above: URI/header/media-type; semantic versioning, backward-compatible additive changes, contract tests, N-1 support, documented deprecation window, gateway-level routing between versions*

**303. How do you design idempotent APIs?**
   🔑 *Idempotency key + unique constraint + stored response; or make the operation naturally idempotent (PUT with client-supplied id, UPSERT, state machine transition guarded by WHERE status = ?); dedupe table with TTL; return the same result on replay*

**304. What is HTTP protocol and why it is used for communication between server and client? Any other we have**
   🔑 *HTTP = stateless request/response application protocol over TCP — methods, status codes, headers, caching, content negotiation; text-based, firewall/proxy friendly, universal client support; HTTP/2 multiplexing, HTTP/3 over QUIC; alternatives = WebSocket (full-duplex), gRPC, SSE, MQTT/AMQP, raw TCP/UDP*

**305. What is gRPC, main difference in SOAP and REST other that xml and json ? Strong point**
   🔑 *gRPC = RPC over HTTP/2 with Protobuf IDL; vs SOAP/REST → binary not text (smaller/faster), strict schema + codegen in many languages, 4 streaming modes (unary, server, client, bidi), built-in deadlines, multiplexed connections; weak points = not human readable, limited browser support, needs proxy for web*

**306. How to notify frontend, like tell frontend like this task is complete…**
   🔑 *WebSocket (STOMP/SockJS), Server-Sent Events for one-way push, long polling as fallback, webhook callback, or push via a message broker to a notification service; for long jobs return 202 Accepted + job id and a status endpoint, then push completion*

**307. REST, GraphQL, stateless and stateful, SOAP, response codes**
   🔑 *REST = resources + verbs + stateless; GraphQL = single endpoint, client-specified query shape; SOAP = XML + WSDL + WS-*; stateless = no server session, token carries context, horizontally scalable; stateful = server session/sticky sessions; status codes 2xx success, 3xx redirect, 4xx client, 5xx server*

**308. CORS**
   🔑 *Browser same-origin policy; server responds with Access-Control-Allow-Origin/Methods/Headers/Credentials; preflight OPTIONS for non-simple requests; Spring @CrossOrigin or CorsConfigurationSource bean — must be registered before Spring Security filter chain; never use '*' with credentials; it's a browser-side control only*

**309. What is ResponseEntity?**
   🔑 *Generic wrapper giving full control over status code, headers and body: ResponseEntity.ok(body), .status(HttpStatus.CREATED).header(...).body(...), .noContent(), .build(); ResponseEntity<Void> for empty; alternative is @ResponseStatus + plain return type*

**310. Few RestTemplate methods used in a project — how does PATCH work with RestTemplate?**
   🔑 *getForObject/getForEntity, postForObject/postForEntity, exchange (full control, generic types via ParameterizedTypeReference), put, delete; PATCH fails with default SimpleClientHttpRequestFactory (HttpURLConnection doesn't support PATCH) → use HttpComponentsClientHttpRequestFactory (Apache HttpClient) or WebClient; RestTemplate is in maintenance mode → prefer WebClient/RestClient*

**311. Idempotent and safe operations**
   🔑 *Safe = no state change (GET, HEAD, OPTIONS); idempotent = same result on repeated calls (GET, PUT, DELETE, HEAD); POST and PATCH are neither by default; safe implies idempotent but not vice versa*

**312. Idempotency — what is it and how did you implement it in your project?**
   🔑 *Same operation repeated gives same outcome; implemented via Idempotency-Key header + unique index, dedupe/outbox table storing status+response, conditional UPDATE on state, Kafka consumer dedupe by event id or transactional outbox; critical for retries, at-least-once messaging and payment double-submit*

**313. 2 APIs, one sending a proper response object and another sending direct text — what is the difference between the two?**
   🔑 *Object → serialized by Jackson (HttpMessageConverter) with Content-Type application/json, typed/parsable, versionable, errors structured; raw String → text/plain, no structure, breaks client parsing and content negotiation; always return a DTO (plus a consistent error envelope)*

**314. HTTP status code 3xx — and can we add custom codes like 600–700?**
   🔑 *3xx = redirection: 301 permanent, 302 found, 303 see other, 304 not modified (caching/ETag), 307/308 preserve method; you cannot invent 6xx — HTTP status is a fixed registry (IANA) and clients/proxies treat unknown codes by class (x00); use a standard code + an application error code in the body (RFC 7807 problem+json)*

**315. APIs work fine in Postman but not from the site for the frontend developer — what would be the issue?**
   🔑 *CORS preflight failure, missing/incorrect auth header or cookie (SameSite, HttpOnly, credentials: include), different base URL/port/proxy, HTTPS mixed content, Content-Type mismatch, CSRF token missing, gateway/WAF rule, browser caching, or payload differences — check browser devtools network tab + server access logs*

**316. REST vs SOAP**
   🔑 *REST = lightweight, JSON, HTTP verbs, stateless, cacheable, easy to consume; SOAP = XML envelope, WSDL contract, WS-Security/transactions, protocol-independent, verbose; SOAP for strict contracts/regulated integrations, REST for everything else*

**317. What is gRPC?**
   🔑 *Google RPC framework — Protobuf IDL + codegen, HTTP/2 transport, binary, unary/streaming, deadlines, interceptors, built-in load balancing; ideal for internal microservice calls; needs grpc-web/gateway for browsers*

**318. How do we validate a request body across multiple URL versions?**
   🔑 *Version-specific DTOs each with their own Bean Validation annotations (@NotNull, @Valid, groups), shared base class for common fields, validation groups for version-specific rules, @Validated on controller, custom ConstraintValidator; map all versions to one domain model and validate invariants there too*

**319. Can we do overloading in a controller to support versioning and backward compatibility for a large user base?**
   🔑 *No — Java overloading can't dispatch on URL version, and Spring will fail with ambiguous mapping if path+method+params are identical; use distinct @RequestMapping paths (/v1, /v2), separate controllers or a version header with 'headers=' / 'produces=' attribute; keep business logic in a shared service*

**320. v2 makes one extra DB call but v1 does not — how do we handle this for backward compatibility?**
   🔑 *v2 controller does the extra call and enriches the response; v1 keeps the old DTO and skips it (or uses a cached/default value); shared service with an optional flag/strategy; new field is additive so old clients ignore it; feature flag + metrics on v1 usage until deprecation*

**321. How do you notify the frontend that a task is complete?**
   🔑 *202 Accepted + job id, client polls a status endpoint, or push via WebSocket/STOMP, SSE, or webhook; back it with a job status table and a Kafka completion event consumed by a notification service*

## Microservices Architecture & Resilience

**322. Monolith → microservices migration — what are the actual triggers that justify the split, and what's the first thing you decompose?**
   🔑 *Triggers = independent scaling needs, team autonomy/Conway's law, deploy coupling and slow release trains, divergent tech/runtime needs, fault isolation; NOT 'microservices are modern'; start with strangler-fig on a bounded context that is read-heavy, low-coupling and high-change (e.g. notifications, reporting, auth); split the data last, keep a shared DB temporarily behind an API, measure with DORA metrics*

**323. How do you handle distributed transactions across services — Saga pattern (choreography vs orchestration), compensating transactions?**
   🔑 *No 2PC/XA in practice (blocking, poor availability); Saga = sequence of local transactions + compensating actions; choreography (events, no central brain, loose coupling, hard to trace) vs orchestration (central orchestrator, explicit state machine, easier to debug/monitor, single point of coordination); compensations must be idempotent and commutative; semantic locks for in-flight state*

**324. Explain the Outbox pattern — why is it needed alongside message publishing to avoid dual-write inconsistency?**
   🔑 *Dual-write problem = DB commit succeeds but broker publish fails (or vice versa) → inconsistency; Outbox = write the event into an outbox table in the SAME local transaction, then a relay (polling publisher or CDC/Debezium) reads and publishes to Kafka, marks sent; gives at-least-once + ordering; consumer must dedupe; alternative is listen-to-yourself / transaction log tailing*

**325. Service discovery and load balancing — Eureka/Consul vs Kubernetes-native (DNS + kube-proxy) — which have you used and why?**
   🔑 *Client-side (Eureka + Ribbon/Spring Cloud LoadBalancer): registry, heartbeat/lease renewal, client caches registry and picks instance, self-preservation mode; Consul adds health checks + KV + DNS; Kubernetes-native: Service + Endpoints/EndpointSlice, cluster DNS, kube-proxy iptables/IPVS, readiness probes gate endpoints, headless service for direct pod IPs; on K8s you usually don't need Eureka*

**326. API Gateway responsibilities — routing, auth, rate limiting, request aggregation — what have you built with Spring Cloud Gateway/Kong/Zuul?**
   🔑 *Routing/path rewrite, authn/authz + token validation, rate limiting and quotas, request/response transformation, aggregation/BFF, TLS termination, canary/traffic splitting, caching, CORS, observability and correlation IDs; Spring Cloud Gateway = reactive, predicates + filters (Global/Gateway filters), non-blocking; avoid putting business logic in the gateway and avoid it becoming a SPOF*

**327. Circuit breaker pattern — Resilience4j vs Hystrix — how do you tune thresholds, and what's a real incident where a missing circuit breaker caused cascading failure?**
   🔑 *States CLOSED → OPEN → HALF_OPEN; Resilience4j = lightweight, functional, decorators, sliding window (count/time), modular (retry, bulkhead, rate limiter, time limiter); Hystrix = thread-pool isolation, in maintenance mode; tune failureRateThreshold (~50%), slowCallRateThreshold/duration, minimumNumberOfCalls (avoid tripping on tiny samples), waitDurationInOpenState, permittedCallsInHalfOpen; incident = downstream latency spike → all threads blocked → cascading failure/thread pool exhaustion across the chain*

**328. How do you do centralized configuration and secret management across dozens of services (Spring Cloud Config, Vault, AWS Secrets Manager)?**
   🔑 *Spring Cloud Config Server (Git-backed, @RefreshScope, /actuator/refresh, Spring Cloud Bus for broadcast), Consul/etcd KV, Kubernetes ConfigMap + Secret, Vault (dynamic DB credentials, leases, transit encryption), AWS Secrets Manager/Parameter Store with rotation + IRSA; never commit secrets, encrypt at rest, least-privilege IAM, audit access, config per profile/environment*

**329. Explain the CAP theorem in the context of a real design decision you made (choosing consistency vs availability).**
   🔑 *Under a network Partition you must pick Consistency or Availability (CA is not an option in a distributed system); PACELC extends it — else Latency vs Consistency; payments/ledger → CP (reject rather than double-spend); product catalog, feed, recommendations → AP with eventual consistency; quorum (R+W>N) to tune*

**330. What does an API Gateway do — routing, auth, rate limiting, aggregation**
   🔑 *Single entry point: routing, auth/token validation, rate limiting/throttling, aggregation, protocol translation, TLS termination, caching, request shaping, logging/tracing, canary routing*

**331. How do you handle service-to-service communication failures? (Circuit breaker — Resilience4j, retries, fallback)**
   🔑 *Timeouts (always first), retries with exponential backoff + jitter (only for idempotent ops), circuit breaker, bulkhead isolation, fallback/cached/default response, rate limiter, dead-letter + async retry, graceful degradation; Resilience4j decorator order = Retry(CircuitBreaker(TimeLimiter(Bulkhead(call))))*

**332. Design question: How would you design an inter-service communication pattern for an order + payment + inventory microservice setup? (sync REST vs async Kafka — trade-offs)**
   🔑 *Order service owns the saga: sync REST only for fast read/validation, async Kafka events for the workflow (OrderCreated → PaymentRequested → PaymentCompleted/Failed → InventoryReserved → OrderConfirmed); outbox for reliable publishing, idempotency keys, compensating events (refund/release stock), state machine on the order, DLQ + retry; sync couples availability and latency, async gives resilience but eventual consistency*

**333. How do microservices communicate (REST, gRPC, messaging)?**
   🔑 *Sync = REST/HTTP, gRPC, GraphQL (simple, immediate result, temporal coupling); async = Kafka/RabbitMQ/SQS events or commands (decoupled, buffered, replayable, eventual consistency); service mesh/sidecar for transport concerns; pick async for workflows, sync for queries*

**334. Explain event-driven architecture vs request-response.**
   🔑 *Request-response = caller waits, tight temporal coupling, easy to reason about, failure propagates; event-driven = publish facts, consumers react independently, loose coupling, buffering and replay, scales fan-out, but harder debugging, ordering, eventual consistency and duplicate handling; often hybrid*

**335. How do you handle distributed transactions (Saga pattern)?**
   🔑 *Saga = chain of local transactions with compensations; choreography via events vs orchestration via a central coordinator (state machine); idempotent compensations, saga log/state persistence, timeouts for stuck sagas, outbox for reliable publishing*

**336. What is service discovery? How does it work (Eureka, Consul)?**
   🔑 *Service registry keeps instance metadata; registration (self-register or third-party), heartbeat/lease + eviction, client-side lookup with local cache vs server-side via LB/DNS; Eureka = AP with self-preservation, Consul = health checks + DNS + KV (CP via Raft); K8s uses DNS + Endpoints instead*

**337. Explain circuit breaker pattern (Resilience4j/Hystrix) and why it's needed.**
   🔑 *Prevents cascading failure and resource exhaustion when a downstream is failing/slow; fail fast instead of piling up threads; CLOSED/OPEN/HALF_OPEN, sliding window of failure and slow-call rate, waitDurationInOpenState, probe calls in half-open, fallback; always combine with timeout + bulkhead*

**338. Explain API Gateway's role.**
   🔑 *Single entry point; routing, cross-cutting security, rate limiting, aggregation/BFF, protocol translation, TLS, observability, versioning, canary; keeps clients decoupled from internal topology*

**339. How do you ensure a service is highly available and fault-tolerant?**
   🔑 *Redundancy across AZs, stateless services + horizontal autoscaling, health probes, timeouts/retries/circuit breakers/bulkheads, graceful degradation and fallback, async buffering with queues, idempotency, DB replicas + failover, rate limiting and load shedding, chaos testing, DR plan with RTO/RPO, blue-green/canary deploys, SLOs + error budgets*

**340. Microservice architecture**
   🔑 *Independently deployable services around bounded contexts, own database per service, API/event contracts, decentralized governance, smart endpoints/dumb pipes, automation (CI/CD, IaC), observability, resilience patterns; requires DevOps maturity*

**341. Explain project architecture and work flow, kafka,**
   🔑 *Describe bounded contexts and ownership, request flow through gateway → services → DB, Kafka topics and event flow, sync vs async boundaries, data stores, caching, security, deployment topology (K8s), observability, and one concrete end-to-end use case you personally built*

**342. advantage and disadvantage of micro service, and monolithic architecture**
   🔑 *Microservices: independent deploy/scale, team autonomy, fault isolation, tech diversity — but distributed complexity, network latency, eventual consistency, testing/debugging difficulty, operational cost, data duplication; Monolith: simple, fast local calls, single transaction, easy testing — but scaling is all-or-nothing, slow releases, tight coupling, large blast radius; start modular monolith*

**343. How you implemented SAGA, like for choreography we have kafka , so what is for Orchestration**
   🔑 *Choreography = Kafka events, each service listens and reacts; Orchestration = a central orchestrator service with a state machine — Camunda/Zeebe, Temporal, AWS Step Functions, Netflix Conductor, or a hand-rolled saga coordinator persisting saga state and issuing commands via Kafka topics with reply topics*

**344. CQRS and it's drawback , how would you achieve consistency In this , specially in read and write**
   🔑 *Command Query Responsibility Segregation = separate write model (normalized, transactional) from read model (denormalized projections), often with event sourcing; drawbacks = eventual consistency lag, duplicate storage, projection rebuild complexity, more moving parts, debugging; consistency via read-your-own-writes (route to write model or version/sequence token), optimistic UI, versioned projections, idempotent handlers, monitoring projection lag*

**345. Leader follower pattern, why**
   🔑 *One leader handles writes/coordination, followers replicate and can serve reads; gives a single ordering point, avoids split-brain, simple failover via consensus (Raft/ZAB/Paxos) or leader election (Zookeeper, etcd, Kafka controller); used in Kafka partitions, databases, and for singleton scheduled jobs across instances*

**346. How you decide instance of service ?**
   🔑 *From throughput per instance (load test) vs peak RPS + headroom, p99 latency targets, CPU/memory profile, HPA on CPU/RPS/custom metrics, min replicas for HA (≥2-3 across AZs), connection pool and downstream limits, cost; always leave capacity for a rolling deploy and one AZ failure*

**347. Sync and asynchronous communication real time example**
   🔑 *Sync = checkout calling payment authorization, login, price lookup — need the answer now; async = order confirmation email, invoice generation, analytics, inventory sync, report export — fire and forget with eventual completion*

**348. Reverting operation or transaction on failure of other service**
   🔑 *No distributed rollback — use compensating transactions (refund, release reservation, cancel), Saga with persisted state, idempotent compensation handlers, outbox for reliable emission, DLQ + manual intervention queue for unrecoverable cases, reconciliation job*

**349. Circuit breaker and different patterns**
   🔑 *Circuit breaker plus the rest of the resilience family: retry with backoff+jitter, timeout/time limiter, bulkhead (thread pool or semaphore isolation), rate limiter, fallback/cache, load shedding, Saga/compensation, outbox, sidecar/service mesh, BFF, strangler fig*

**350. How do you implement a timeout in a circuit breaker?**
   🔑 *Resilience4j TimeLimiter wraps the call in a CompletableFuture with a timeout and cancels it, counting the timeout as a failure for the breaker; also set transport-level timeouts (connect/read) on WebClient/RestTemplate/Feign — a circuit breaker without a client timeout is useless because the call never returns to be counted*

**351. Custom settings for a circuit breaker — sliding window, threshold, timeout, wait duration, etc.**
   🔑 *slidingWindowType COUNT_BASED or TIME_BASED, slidingWindowSize, minimumNumberOfCalls, failureRateThreshold, slowCallRateThreshold + slowCallDurationThreshold, waitDurationInOpenState, permittedNumberOfCallsInHalfOpenState, automaticTransitionFromOpenToHalfOpen, recordExceptions/ignoreExceptions; configured in application.yml per instance name, plus Micrometer metrics on state changes*

**352. How do you implement a fallback mechanism in a circuit breaker, how does it work internally, and how does it trigger the fallback?**
   🔑 *Annotate with @CircuitBreaker(name, fallbackMethod) — fallback must have the same signature plus a Throwable param; internally the decorator catches the recorded exception or CallNotPermittedException when OPEN and invokes the fallback; fallback should be fast and local — cached value, default/empty response, queue for later, or a friendly degraded message; can chain fallbacks by exception type*

**353. What if one service is slow in the circuit — how can we solve it when we are passing the result to another service?**
   🔑 *Set an aggressive timeout so slowness becomes a fast failure, add slowCallRateThreshold so the breaker opens on latency not just errors, bulkhead to cap concurrent calls, serve cached/default data to the next service, make the call async (return 202 + callback/event), or degrade the response; never let a slow dependency hold your request threads*

**354. If one microservice crashes, how do other services detect it?**
   🔑 *Health checks and heartbeats — K8s liveness/readiness probes remove the pod from Service endpoints, Eureka lease expiry, Consul health check; at call time you see connection refused/timeout → circuit breaker opens; plus alerting on metrics, Kafka consumer group rebalance, and distributed tracing gaps*

**355. What is latency in microservices and how do you handle it?**
   🔑 *Latency = time for a request across the network hops; sources = extra hops, serialization, N+1 service calls, cold start, GC, DB; handle with parallel/async calls (CompletableFuture, WebClient), caching, batching/aggregation (BFF), gRPC/protobuf, connection pooling + keep-alive, co-location, timeouts/budgets per hop, and measure p95/p99 not averages with distributed tracing*

**356. A service uploads a large file — how do you handle it?**
   🔑 *Stream it — don't buffer in memory: multipart streaming, chunked transfer, StreamingResponseBody/InputStreamResource; better, use pre-signed S3 URLs so the client uploads directly and the service only gets a notification; chunked/resumable uploads, size and content-type validation, virus scan, async processing via event, backpressure with WebFlux, raise multipart max-file-size/max-request-size deliberately*

**357. ACL (anti-corruption layer) pattern**
   🔑 *Anti-Corruption Layer (DDD) — a translation layer between your bounded context and a legacy/external model so their model doesn't leak into yours; adapter/facade + DTO mapping, isolates change, prevents a bad upstream schema from corrupting the domain; used heavily in strangler-fig migrations*

**358. Sidecar pattern**
   🔑 *Deploy a helper container alongside the main container in the same pod sharing network/volume — handles cross-cutting concerns (proxy/mTLS via Envoy, log shipping, config sync, metrics); language-agnostic, independently upgradable; basis of service mesh (Istio/Linkerd); cost = extra resources and latency*

**359. Fault tolerance in microservices**
   🔑 *Timeouts, retries with backoff+jitter, circuit breakers, bulkheads, rate limiting/load shedding, fallback and graceful degradation, idempotency, async buffering with queues + DLQ, redundancy/multi-AZ, health probes, chaos engineering, failing fast rather than cascading*

**360. Feign client vs REST client**
   🔑 *Feign = declarative interface-based client, annotations, integrates with Eureka/LoadBalancer/Resilience4j, less boilerplate, but reflective and blocking; RestTemplate = imperative, blocking, maintenance mode; WebClient/RestClient = modern, non-blocking (WebClient) or fluent blocking (RestClient), better timeout/streaming control; pick Feign for many simple internal calls, WebClient for reactive/streaming/fine control*

**361. How do you make sure all instances share the load evenly?**
   🔑 *Load balancer algorithm (round robin, least connections, least request), client-side LB with instance list refresh, K8s Service/kube-proxy or mesh-aware LB, avoid sticky sessions (keep services stateless), connection draining and keep-alive rebalancing (long-lived HTTP/2 connections can pin traffic — use periodic reconnect or a mesh), Kafka partition assignment for consumers, and verify with per-instance RPS metrics*

**362. How do you decide the DB and strategy to use — separate DB for each microservice vs shared DB?**
   🔑 *Database-per-service is the default — independent schema evolution, scaling and failure isolation, no hidden coupling; shared DB only as a temporary migration step or for a tightly-coupled legacy pair (it creates deploy coupling and contention); choose engine per access pattern (Postgres for transactional, Mongo for documents, Redis for cache, ES for search); cross-service data via API, events/CDC, or a read-model projection*

**363. Distributed transactions and the SAGA pattern**
   🔑 *No XA — Saga (choreography/orchestration) with compensating transactions, outbox for atomic publish, idempotent handlers, saga state persistence with timeouts, reconciliation; alternatives = TCC (try-confirm-cancel), eventual consistency with reconciliation*

**364. How do you handle dependent microservices and their failure?**
   🔑 *Timeout + circuit breaker + fallback, bulkhead so one dependency can't starve the rest, cache last-known-good, make the dependency async where possible, degrade features rather than fail the request, define hard vs soft dependencies, retry only idempotent calls, and alert on dependency error budget*

**365. Why would we need to shift from microservices back to a monolith?**
   🔑 *Over-decomposition ('distributed monolith'), chatty sync calls and latency, operational/infra cost, small team can't own many services, debugging and data consistency pain, deploy coupling anyway; consolidating into a modular monolith or coarser services reduces cost and complexity — Amazon Prime Video case; migrate back by merging bounded contexts that always change together*

**366. Convert a monolith to microservices**
   🔑 *Strangler fig — put a facade/gateway in front, carve out one bounded context at a time (DDD, event storming), start with low-risk/high-change modules, extract code then data, dual-write or CDC during transition, feature flag the cutover, keep a rollback path, repeat until the monolith is hollow; invest in CI/CD, observability and contract tests first*

**367. How does an API gateway know which APIs map to which service, and what other features do we use in the gateway?**
   🔑 *Route definitions — static config (Spring Cloud Gateway predicates: Path, Host, Method, Header) or dynamic from a service registry/K8s Ingress/CRD; matching by path prefix, host, header or weight; other features = auth, rate limiting, retry, circuit breaker, request/response filters, canary weights, CORS, caching, tracing headers, request size limits*

**368. BFF (backend for frontend) communication**
   🔑 *Backend For Frontend — a dedicated API layer per client type (web, mobile, partner) that aggregates and shapes downstream responses, reduces round trips and over-fetching, owns client-specific logic and auth; avoids a bloated generic gateway; trade-off = more services and some duplication; GraphQL can play the same role*

**369. Eventual consistency, fault tolerance, and fault tolerance with Kafka**
   🔑 *Eventual consistency = replicas/read models converge after a lag; Kafka supports it with durable partitioned log, replication factor + min.insync.replicas + acks=all, consumer offsets and replay, idempotent producer and transactions, DLQ and retry topics, consumer group rebalance for failover; design consumers to be idempotent and tolerant of out-of-order/duplicate events*

**370. Difficulties in microservices**
   🔑 *Distributed debugging and tracing, data consistency and distributed transactions, network latency and partial failure, testing (integration/contract/e2e), versioning and contract evolution, deployment and infra complexity, observability cost, service sprawl and ownership, local dev environment, security surface, duplicate code vs shared libs*

**371. Microservice design patterns**
   🔑 *Decomposition (by business capability/subdomain, strangler fig), database per service, Saga, CQRS, Event Sourcing, Outbox/CDC, API Gateway, BFF, Aggregator, Service Discovery, Client/Server-side LB, Circuit Breaker, Bulkhead, Retry, Sidecar/Ambassador, Anti-Corruption Layer, Service Mesh, Log Aggregation, Distributed Tracing, Health Check API, Externalized Configuration, Blue-Green/Canary*

**372. How do you decide the number of instances of a service?**
   🔑 *Peak RPS ÷ sustainable RPS per instance, plus headroom for spikes and rolling deploys; minimum 2-3 for HA across AZs; HPA on CPU/memory/custom metrics (RPS, queue lag) with sensible min/max; bounded by DB connection pool, downstream rate limits and Kafka partition count; validate with load tests and cost review*

**373. CQRS and its drawbacks — how would you achieve consistency, specially between read and write?**
   🔑 *Separate write and read models; drawbacks = eventual consistency, extra infrastructure, projection lag and rebuild cost, duplicated data, harder debugging, more code; consistency via read-your-writes routing, version/sequence tokens returned to the client, optimistic UI, idempotent + ordered projection handlers, lag monitoring and alerting, replay to rebuild*

**374. How did you implement SAGA — for choreography we have Kafka, so what do we use for orchestration?**
   🔑 *Choreography with Kafka events; orchestration with a workflow engine — Temporal, Camunda/Zeebe, AWS Step Functions, Netflix Conductor — or a custom orchestrator persisting saga state and sending commands/awaiting reply events; orchestrator owns retries, timeouts and compensation order*

**375. Service protection from failure of another service — communication strategies**
   🔑 *Timeouts, circuit breaker, bulkhead/thread isolation, retry with backoff for idempotent calls, fallback and cached responses, async/queue decoupling, rate limiting and load shedding, graceful degradation, dependency health checks, and contract/version compatibility to avoid breaking changes*

**376. How do we handle reverting an operation or transaction on failure of another service?**
   🔑 *Compensating transactions via Saga, idempotent compensation handlers, persisted saga state with timeout detection, outbox to guarantee the compensation event is published, DLQ + manual remediation queue, and a reconciliation/audit job to catch anything that slipped through*

## Kafka, Messaging & Event-Driven Architecture

**377. Explain event-driven architecture with Kafka — partitioning strategy, consumer group rebalancing, exactly-once vs at-least-once semantics, and how you've handled duplicate message processing.**
   🔑 *Partition key = entity id (accountId/orderId) for ordering + even spread, avoid hot keys, partitions only grow (never shrink) and adding them breaks key→partition mapping; rebalancing = group coordinator reassigns on join/leave/timeout, stop-the-world vs cooperative-sticky, static membership (group.instance.id) to avoid churn, max.poll.interval.ms for slow processing; EOS = idempotent producer (enable.idempotence, PID+sequence) + transactions (transactional.id, read_committed) only within Kafka; across external systems use at-least-once + idempotent consumer*

**378. How do you ensure message ordering when it matters (e.g., processing events for the same account/entity) in a partitioned topic?**
   🔑 *Ordering is guaranteed only within a partition — key all events for the same entity to the same partition; keep max.in.flight.requests.per.connection ≤ 5 with idempotence (or 1 without) so retries don't reorder; single consumer per partition in a group; avoid parallel/async processing inside the consumer unless you key-shard the worker pool; ordering breaks if you add partitions or use retry topics*

**379. What's your strategy for schema evolution in Kafka messages (Avro/Protobuf + schema registry) without breaking consumers?**
   🔑 *Avro/Protobuf + Confluent Schema Registry; compatibility modes BACKWARD (new consumer reads old data — add optional fields with defaults), FORWARD, FULL, TRANSITIVE; never remove/rename a required field or change a type; use defaults and aliases; register schemas in CI and fail the build on incompatible change; consumers use a tolerant reader; roll out consumers before producers for BACKWARD*

**380. Dead-letter queues — how do you design retry + DLQ handling so failed messages don't silently vanish or infinite-loop?**
   🔑 *Bounded in-memory retries (DefaultErrorHandler + FixedBackOff/ExponentialBackOff), then non-blocking retry topics with increasing delay (@RetryableTopic), finally a DLQ topic with original headers (exception, stack, original topic/partition/offset, attempt count); classify retryable vs non-retryable (deserialization/validation → straight to DLT); alert and dashboard DLQ depth, provide a replay/reprocess tool, never swallow or auto-commit past a failure*

**381. Kafka core concepts: topic, partition, consumer group, offset, replication factor**
   🔑 *Topic = named log; partition = ordered immutable append-only sequence, unit of parallelism and ordering; offset = per-partition sequential position, committed per consumer group; consumer group = set of consumers sharing a subscription, each partition assigned to exactly one member; replication factor = copies across brokers, leader + ISR followers, min.insync.replicas + acks=all for durability; retention by time/size, log compaction by key*

**382. How does Kafka guarantee message ordering? (only within a partition)**
   🔑 *Only within a partition — producer appends in order, consumer reads sequentially by offset; same key → same partition via murmur2(key) % numPartitions; no global ordering across partitions; preserve it with idempotent producer, bounded in-flight requests, and single-threaded per-partition processing*

**383. At-least-once vs exactly-once vs at-most-once delivery — how do you achieve each?**
   🔑 *At-most-once = commit offset before processing (loss on crash); at-least-once = process then commit (duplicates — the common default, needs idempotent consumer); exactly-once = idempotent producer + transactions + isolation.level=read_committed, or read-process-write with offsets committed inside the producer transaction (sendOffsetsToTransaction); EOS is only within Kafka — external side effects still need idempotency/dedupe*

**384. Difference between Kafka and RabbitMQ — when to use which (Kafka: high throughput/event streaming/replay; RabbitMQ: complex routing, lower latency per message, task queues)**
   🔑 *Kafka = distributed commit log, pull-based, retention and replay, high throughput, partitioned ordering, consumer groups, stream processing, great for event sourcing/analytics/fan-out; RabbitMQ = broker with exchanges/queues/bindings, push-based, flexible routing (direct/topic/fanout/headers), per-message ack and priority, TTL, delayed messages, lower per-message latency, good for task queues and RPC; Kafka messages aren't deleted on consume, Rabbit's are*

**385. How do you handle duplicate message processing (idempotency)?**
   🔑 *Make the consumer idempotent — dedupe table/cache keyed by event id or business key with a unique constraint, conditional UPDATE/UPSERT, state-machine transitions guarded by current status, Redis SETNX with TTL for a fast pre-check; store processed offsets/ids transactionally with the business write; never rely on exactly-once alone*

**386. Coding/design: Design an event-driven flow for "Order Placed → Inventory Updated → Payment Processed → Notification Sent" using Kafka topics**
   🔑 *Topics per event with the order id as key: order-events → inventory-service reserves stock → inventory-events → payment-service charges → payment-events → notification-service; each step writes its DB and publishes via the outbox; compensations (release stock, refund) on failure; idempotent consumers, retry topics + DLQ, saga state on the order aggregate, schema registry for contracts*

**387. How you handled duplicate msg from produce in kafka,**
   🔑 *Enable idempotent producer (enable.idempotence=true, acks=all, retries>0) to stop broker-side duplicates from retries; for application-level duplicates attach a unique event/business id and dedupe on the consumer (unique index, Redis, or processed-events table); transactions if the duplicate comes from a read-process-write loop*

**388. How you troubleshoot kafka consumer that is unable to consume message**
   🔑 *Check group assignment and lag (kafka-consumer-groups --describe), is the consumer actually subscribed to the right topic/partition and bootstrap servers; auto.offset.reset (latest skips old messages), offsets already committed past the data, group.id changed, consumer stuck in rebalance, max.poll.interval.ms exceeded → kicked out of group, deserialization exception killing the container, ACL/auth failures, topic/partition leadership or broker connectivity, listener container not started, poison pill blocking the partition*

**389. How you handle kafka consumer that continues failing while processing message**
   🔑 *Classify the error: transient (retry with backoff) vs poison pill (skip to DLT immediately); configure DefaultErrorHandler with backoff and addNotRetryableExceptions, ErrorHandlingDeserializer to survive bad payloads, @RetryableTopic for non-blocking retries, DLQ with full context headers, circuit-break the consumer if the downstream is down (pause/resume the container) so you don't burn the retry budget, alert on DLQ growth and lag, and provide a replay path after the fix*

**390. How to get response bake in kafka, like payment -> invoice -> generate**
   🔑 *Kafka is fire-and-forget by design — use a reply topic with a correlation id (ReplyingKafkaTemplate request/reply), or model it as a chained event flow payment-completed → invoice-service → invoice-generated → next consumer; for the caller return 202 Accepted + job id and push completion via WebSocket/SSE/webhook; the orchestrator/saga tracks state*

**391. What are bare minimum configuration to write kafka consumer**
   🔑 *bootstrap.servers, group.id, key.deserializer, value.deserializer — plus practically auto.offset.reset (earliest/latest), enable.auto.commit=false with manual ack, max.poll.records, max.poll.interval.ms, session.timeout.ms/heartbeat.interval.ms; in Spring @KafkaListener + ConsumerFactory/ConcurrentKafkaListenerContainerFactory*

**392. Consumer rebalancing**
   🔑 *Triggered when a consumer joins/leaves, crashes, misses heartbeats, max.poll.interval.ms is exceeded, or topic metadata/partition count changes; group coordinator + leader run the assignor (range, round-robin, sticky, cooperative-sticky); eager rebalance = stop-the-world revoke-all, cooperative = incremental; reduce impact with static membership, longer max.poll.interval, smaller max.poll.records, cooperative-sticky assignor; commit offsets in onPartitionsRevoked*

**393. Kafka stream api**
   🔑 *Stream processing library (not a cluster) — KStream/KTable/GlobalKTable, map/filter/flatMap, groupByKey + aggregate/count/reduce, windowing (tumbling, hopping, sliding, session), joins (stream-stream, stream-table), state stores backed by RocksDB + changelog topic, exactly-once (processing.guarantee), interactive queries, repartition topics on key change; alternative to Flink/Spark for Kafka-native pipelines*

**394. How you publish application to perticular topic**
   🔑 *KafkaTemplate.send(topic, key, payload) or send(ProducerRecord with headers); topic from config/@Value, key for partitioning, serializer per value type; add callback/CompletableFuture for ack handling; for reliability publish from the outbox inside the transaction rather than directly in request flow*

**395. Republish msg to kafka topic**
   🔑 *Produce the record again to the same or a retry topic, preserving the original key (ordering) and copying headers (original topic/partition/offset, attempt count, exception); use a DLT consumer or an admin replay tool; beware infinite loops — cap attempts and track them in a header*

**396. Send() is sync or asynchronous**
   🔑 *send() is asynchronous — it buffers into the RecordAccumulator and returns a Future/CompletableFuture immediately; the I/O thread batches by linger.ms/batch.size and sends; make it effectively sync with send(...).get() (kills throughput) or handle the callback; acks and retries determine durability, not synchrony*

**397. Kafka partition and it's use, why we have partition**
   🔑 *Partition = unit of parallelism, ordering and storage distribution; more partitions → more consumer concurrency and throughput, spread across brokers; key determines partition so related events stay ordered; costs = more open file handles/memory, longer leader election and rebalance, more end-to-end latency; partition count can only increase*

**398. Patition consumer relation, same consumer with multiple partition ? what if more consumer then partition**
   🔑 *Within a group each partition goes to exactly one consumer, but one consumer can own many partitions; if consumers > partitions the extras sit idle (no work); so partition count caps consumer parallelism — size partitions for peak expected consumers*

**399. Kafka vs RabbitMQ**
   🔑 *Kafka = log-based, replay, retention, high throughput, partition ordering, consumer groups, stream processing; RabbitMQ = queue/exchange routing, push, per-message ack/priority/TTL/delay, lower latency, work queues and RPC; Kafka for event streaming/analytics/fan-out, Rabbit for task distribution and complex routing*

**400. Where do you set topics, partitions and all other settings?**
   🔑 *Topic creation via admin CLI/AdminClient/Terraform or Spring NewTopic @Bean (auto-create disabled in prod); partitions and replication factor at creation; broker-level defaults in server.properties; per-topic overrides (retention.ms, cleanup.policy, min.insync.replicas) via kafka-configs; client settings in application.yml (spring.kafka.*) or ProducerFactory/ConsumerFactory config maps*

**401. Kafka consumer side and producer side configuration**
   🔑 *Producer: bootstrap.servers, serializers, acks, retries, enable.idempotence, linger.ms, batch.size, compression.type, max.in.flight.requests, transactional.id; Consumer: bootstrap.servers, group.id, deserializers, auto.offset.reset, enable.auto.commit=false, max.poll.records, max.poll.interval.ms, session/heartbeat timeouts, isolation.level, partition.assignment.strategy*

**402. What are the bare-minimum configurations to write a Kafka consumer?**
   🔑 *bootstrap.servers, group.id, key/value deserializer — plus auto.offset.reset and manual ack mode for anything production-grade*

**403. Consumer group vs consumer**
   🔑 *Consumer = single client instance; consumer group = logical subscriber identified by group.id across which partitions are divided — each partition to exactly one member, offsets tracked per group, adding members scales out, multiple groups each get a full copy of the topic (fan-out)*

**404. 10 partitions and 20 consumers — what is the behavior?**
   🔑 *10 consumers get one partition each, the other 10 stay idle — parallelism is capped by partition count; they act as hot standbys that take over on rebalance; to use all 20 you must increase partitions to ≥20 (which changes key→partition mapping and breaks per-key ordering for existing keys)*

**405. Schema registry in Kafka — how do you handle versioning in Kafka?**
   🔑 *Confluent Schema Registry stores versioned Avro/Protobuf/JSON schemas; producer serializer registers and prefixes the payload with a schema id, consumer fetches by id; compatibility level BACKWARD/FORWARD/FULL (optionally TRANSITIVE); evolve by adding optional fields with defaults, never removing/renaming required ones; validate compatibility in CI; subject naming strategy per topic/record*

**406. Kafka Streams API**
   🔑 *KStream/KTable/GlobalKTable DSL and Processor API, stateless (map/filter/branch) and stateful (aggregate/count/join) operations, windowing, RocksDB state stores with changelog topics, repartitioning, exactly-once-v2, interactive queries, scaling by partitions, standby replicas for fast failover*

**407. How do you publish from an application to a particular topic?**
   🔑 *KafkaTemplate with the topic name externalized in config; choose the key deliberately for partitioning/ordering; set headers (correlation id, trace id, schema version); handle the send callback for failures; prefer transactional outbox publishing for business-critical events*

**408. Republish a message to a Kafka topic**
   🔑 *Re-send the original payload with its key to the target/retry topic, carrying forward headers and an attempt counter; use a DLT replay consumer or admin tool; ensure consumers are idempotent since replay creates duplicates*

**409. Is `send()` sync or asynchronous?**
   🔑 *Asynchronous — returns a future immediately and batches in the background; block with .get() for sync semantics (throughput collapse) or use the callback; durability is controlled by acks/retries/enable.idempotence, not by blocking*

**410. Where have you used sync and async communication — use cases**
   🔑 *Sync for payment authorization, auth/login, price or inventory lookup — caller needs the answer; async for order fulfillment workflow, emails/notifications, invoice and report generation, audit/analytics, cache warming, cross-service data replication*

**411. Report generation and report publishing — sync vs asynchronous tasking with Kafka events**
   🔑 *Return 202 Accepted + report id, publish a report-requested event, a worker generates asynchronously (Spring Batch/chunked query), uploads to S3, publishes report-ready with a pre-signed URL, notification service pushes to the user via WebSocket/email; keep status in a jobs table, retry + DLQ, idempotent by report id; sync only for tiny reports with a hard timeout*

**412. How do you get a response back in Kafka, like payment → invoice → generate?**
   🔑 *ReplyingKafkaTemplate with a reply topic + correlation id header for request/reply, or model the chain as events (payment-completed → invoice-service → invoice-generated); the API returns 202 with a job id and the client polls or receives a WebSocket/webhook push; saga/orchestrator holds the state machine*

## Security — AuthN/AuthZ, OWASP, TLS & Secrets

**413. Explain the Spring Security filter chain end-to-end for a JWT-based API.**
   🔑 *DelegatingFilterProxy → FilterChainProxy → SecurityFilterChain; order = SecurityContextPersistenceFilter, CorsFilter, CsrfFilter (disabled for stateless), your OncePerRequestFilter JWT filter (parse Bearer, validate signature/exp/issuer/audience, build UsernamePasswordAuthenticationToken with authorities, set SecurityContextHolder), BearerTokenAuthenticationFilter (resource server), ExceptionTranslationFilter (401/403 entry point + access denied handler), FilterSecurityInterceptor/AuthorizationFilter (checks authorizeHttpRequests rules); SessionCreationPolicy.STATELESS, clear SecurityContext after request*

**414. Access token vs refresh token — how do you handle token revocation for a stateless JWT approach (since JWTs can't be "revoked" by default)?**
   🔑 *Access token = short-lived (5-15 min), stateless, sent on every call; refresh token = long-lived, stored securely (HttpOnly SameSite cookie), used only at /token to mint a new access token; revocation = keep a denylist/jti blocklist in Redis with TTL = remaining exp, token version/epoch per user bumped on logout or password change, rotate refresh tokens with reuse detection, short access TTL so the window is small; true instant revocation requires introspection (back to stateful)*

**415. OAuth2 grant types — which have you actually implemented (authorization code, client credentials), and why is implicit grant deprecated?**
   🔑 *Authorization Code + PKCE for user-facing web/mobile/SPA; Client Credentials for machine-to-machine; Refresh Token for silent renewal; Device Code for TVs/CLI; Implicit is deprecated because the token comes back in the URL fragment — exposed in browser history/referrer/logs, no client authentication, no refresh token; Resource Owner Password is also deprecated (app sees the password, breaks MFA/SSO)*

**416. How do you prevent common vulnerabilities in a Spring Boot app — SQL injection (parameterized queries/JPA), XSS, CSRF (and why CSRF protection differs for stateless APIs vs session-based apps)?**
   🔑 *SQLi → parameterized queries/JPA binding, never string concatenation, validate sort/filter columns against a whitelist; XSS → output encoding per context, Content-Security-Policy, sanitize rich text (OWASP Java HTML Sanitizer), don't trust stored data; CSRF → token (synchronizer/double-submit) + SameSite cookies for session-based apps; stateless JWT-in-header APIs aren't CSRF-prone because the browser doesn't auto-attach the header, but JWT-in-cookie is; plus security headers, dependency scanning, no verbose errors, actuator lockdown*

**417. How do you secure service-to-service calls in a microservices mesh?**
   🔑 *mTLS between pods (service mesh — Istio/Linkerd handles cert issuance and rotation via sidecar), SPIFFE/SPIRE workload identity, OAuth2 client-credentials with short-lived JWTs validated per hop, network policies/zero-trust (no implicit trust from being inside the cluster), authorization policies per service, propagate end-user context in a signed token, encrypt in transit everywhere, audit logs*

**418. Given a fintech/payments context: how do you ensure PCI-DSS-relevant data (card numbers, CVVs) never lands in logs, and how do you handle tokenization/masking?**
   🔑 *Never log full PAN/CVV — CVV must never be stored at all; tokenize via a vault/payment provider so the app only handles tokens, mask to first6+last4, custom Logback masking converter/pattern + Jackson @JsonIgnore/serializer on DTOs, block CVV fields from toString (avoid Lombok @Data on card models), scrub MDC and exception messages, encrypt at rest (field-level/KMS), restrict log access + retention, scan logs in CI/staging, limit PCI scope by segmentation*

**419. Rate limiting and abuse prevention on public-facing APIs — token bucket vs sliding window, where do you enforce it (gateway vs app)?**
   🔑 *Token bucket = burst-friendly, refill rate + capacity; leaky bucket = smooths output; fixed window = simple but boundary spikes; sliding window log = accurate but memory heavy; sliding window counter = good compromise; enforce at the gateway/edge first (cheapest, protects everything) and defensively in-app for business quotas; distributed counters in Redis (INCR+EXPIRE or Lua for atomicity), key by API key/user/IP; return 429 with Retry-After and X-RateLimit-* headers; add quotas, WAF, bot detection, and backpressure/load shedding*

**420. How do you handle inter-service authentication (mTLS, service tokens, OAuth2 client-credentials)?**
   🔑 *mTLS (strongest, mutual cert verification, automated rotation via mesh/SPIFFE), OAuth2 client-credentials JWT with short TTL and audience checks, signed internal service tokens, API keys (weakest — rotate and scope them); combine with network policies, least privilege scopes per service, and token propagation of user identity for downstream authorization*

**421. OAuth2 grant types — Authorization Code vs Client Credentials — when to use which**
   🔑 *Authorization Code (+PKCE) when a human logs in and you need user consent/identity — browser redirect, code exchanged server-side for tokens; Client Credentials when there is no user — backend jobs, service-to-service, client_id/secret or private_key_jwt exchanged for a token scoped to the service*

**422. JWT structure (header.payload.signature) — how is it verified without a DB call?**
   🔑 *Base64url(header).Base64url(payload).signature; header = alg + kid, payload = registered claims (iss, sub, aud, exp, iat, nbf, jti) + custom claims/roles; verified by recomputing the HMAC with the shared secret or checking the RSA/EC signature with the public key fetched from the JWKS endpoint (cached by kid) — no DB call because identity and authorities are inside the signed token; must also validate exp, iss, aud, alg (reject 'none' and algorithm confusion)*

**423. How do you handle JWT token expiry/refresh token flow?**
   🔑 *Short-lived access token; on 401/expiry the client calls /token with the refresh token (HttpOnly, Secure, SameSite cookie), gets a new pair; rotate refresh tokens and detect reuse (replay → revoke the whole family), absolute + idle timeout, single-flight refresh on the client to avoid stampede, revoke on logout/password change via jti denylist or token version claim*

**424. SSL/TLS handshake — brief walkthrough**
   🔑 *ClientHello (TLS versions, cipher suites, SNI, key share) → ServerHello + certificate chain → client validates chain to a trusted CA, hostname, validity, revocation (OCSP stapling) → key exchange (ECDHE for forward secrecy) → derive session keys → Finished → symmetric encryption (AES-GCM/ChaCha20); TLS 1.3 is 1-RTT (0-RTT resumption), drops RSA key transport and weak ciphers; mTLS adds a client certificate request and verification*

**425. Coding: Write a method to generate and validate a JWT using io.jsonwebtoken (jjwt) library**
   🔑 *Jwts.builder().setSubject().claim("roles", ...).setIssuedAt().setExpiration().signWith(key, HS256/RS256).compact(); validate with Jwts.parserBuilder().setSigningKey(key).requireIssuer().requireAudience().setAllowedClockSkewSeconds(30).build().parseClaimsJws(token) — catch ExpiredJwtException/SignatureException/MalformedJwtException; key from Keys.hmacShaKeyFor(secretBytes) (≥256-bit) or an RSA keypair, secret from Vault/env not code*

**426. Name the OWASP Top 10 and how you'd mitigate SQL injection, XSS, CSRF.**
   🔑 *A01 Broken Access Control (deny by default, server-side authorization, no IDOR), A02 Cryptographic Failures (TLS, strong hashing, no plaintext secrets), A03 Injection (parameterized queries, validation, encoding), A04 Insecure Design (threat modelling), A05 Security Misconfiguration (hardened defaults, lock down actuator, no stack traces), A06 Vulnerable Components (SCA/Dependabot/OWASP Dependency-Check), A07 Identification & Auth Failures (MFA, lockout, strong sessions), A08 Software & Data Integrity (signed artifacts, safe deserialization), A09 Logging & Monitoring Failures, A10 SSRF (allowlist outbound, block metadata endpoint)*

**427. How do you prevent insecure deserialization in Java?**
   🔑 *Never deserialize untrusted data with native Java serialization; prefer JSON/Protobuf with strict typing; disable polymorphic typing in Jackson (never enableDefaultTyping / @JsonTypeInfo on untrusted input — gadget chains), keep a strict allowlist of deserializable types, use ObjectInputFilter/JEP 290 serialFilter, validate after deserialization, keep libraries patched, and avoid shipping gadget-rich libraries on the classpath*

**428. What is input validation vs output encoding?**
   🔑 *Input validation = reject/constrain untrusted data at the boundary by type, length, format, range, allowlist (Bean Validation @Valid, custom validators) — defends integrity and business rules; output encoding = escape data for the sink it goes to (HTML body, HTML attribute, JS, URL, SQL identifier, LDAP, OS command) at render time — defends against injection; you need both, encoding is context-specific and validation alone can't stop XSS*

**429. How do you handle secrets/credentials in microservices (vault, env vars)?**
   🔑 *Vault (dynamic DB creds, leases, auto-rotation, transit encryption), AWS Secrets Manager/Parameter Store with IAM/IRSA, Kubernetes Secrets (enable encryption at rest + RBAC; base64 is not encryption) or External Secrets Operator, Spring Cloud Config with encrypted values; inject via env var or mounted file, never in git/images/logs; rotate regularly, short-lived credentials, least privilege, audit access, scan repos for leaked secrets (gitleaks/trufflehog)*

**430. Explain SEI CERT coding guidelines you've applied.**
   🔑 *Validate all input at trust boundaries; don't expose mutable internal state (defensive copies); make classes immutable and fields private final; avoid native serialization of sensitive classes; don't log sensitive data; use SecureRandom not Random for security values; constant-time comparison for secrets/MACs; close resources with try-with-resources; avoid race conditions on shared state (TOCTOU); don't catch Throwable or swallow exceptions; sanitize data used in format strings and commands*

**431. How do you secure service-to-service communication (mTLS, OAuth2/JWT)?**
   🔑 *mTLS via service mesh (automatic cert issue/rotation, strict mode), OAuth2 client-credentials JWT with audience + scope validation per hop, SPIFFE identities, network policies/zero trust, short-lived credentials, end-user context propagated in a signed token, and authorization policy enforced in each service — not just at the gateway*

**432. Prevent common application valnaribilty, like css, xors, csrf, how csrf work in sessionn based api**
   🔑 *XSS → contextual output encoding, CSP, sanitizer, HttpOnly cookies; CSRF → synchronizer token or double-submit cookie + SameSite=Lax/Strict, validate Origin/Referer; CORS is a browser control, not a defence; SQLi → prepared statements; in session-based APIs CSRF works because the browser automatically attaches the session cookie to a cross-site request, so the attacker's page can trigger an authenticated state-changing call — the unguessable, per-session token the attacker can't read (same-origin policy) is what blocks it; safe methods need no token*

**433. What is SSL? What is an SSL certificate?**
   🔑 *SSL/TLS = protocol providing encryption, integrity and server authentication over TCP; certificate = X.509 document binding a public key to a domain, signed by a CA, containing subject/SAN, issuer, validity, key usage; client validates the chain to a trusted root, hostname and expiry/revocation; types DV/OV/EV, wildcard, SAN; Let's Encrypt for automated issuance/renewal*

**434. What is TLS and SSL and how to implement for API security in first stage,**
   🔑 *SSL is the deprecated predecessor of TLS (use TLS 1.2/1.3 only); first-stage API security = terminate TLS at the LB/ingress with a valid cert, redirect HTTP→HTTPS, HSTS header, strong cipher suites, disable old protocols, automate renewal (cert-manager/ACM), then add mTLS internally; in Spring Boot server.ssl.key-store/key-store-password or offload to the ingress*

**435. Complete authorization and authentically flow, with class and method access**
   🔑 *Authentication = who you are (filter validates credentials/JWT → Authentication in SecurityContextHolder); authorization = what you may do (authorizeHttpRequests URL rules, then method security @PreAuthorize/@PostAuthorize/@Secured with SpEL and hasRole/hasAuthority, @EnableMethodSecurity); roles vs authorities/scopes, domain object ACLs, deny-by-default, 401 vs 403, custom AccessDeniedHandler and AuthenticationEntryPoint*

**436. How you store secure details in DB, and how you store username and password in DB**
   🔑 *Passwords are hashed, never encrypted or stored plaintext — BCrypt/Argon2id/SCrypt with a per-user salt and tuned work factor (Spring Security PasswordEncoder, DelegatingPasswordEncoder for upgrades); other sensitive fields encrypted at rest with AES-GCM and keys in a KMS/Vault (envelope encryption, key rotation), deterministic/blind index only where you must search, column-level or TDE encryption, mask in logs and non-prod data, restrict DB access by least privilege and audit it*

**437. Spring Security layers and filters**
   🔑 *Servlet filter chain registered as springSecurityFilterChain → FilterChainProxy picks the matching SecurityFilterChain; key filters = SecurityContextPersistenceFilter/SecurityContextHolderFilter, CorsFilter, CsrfFilter, LogoutFilter, UsernamePasswordAuthenticationFilter, BearerTokenAuthenticationFilter, custom OncePerRequestFilter, AnonymousAuthenticationFilter, ExceptionTranslationFilter, AuthorizationFilter; supporting beans AuthenticationManager → AuthenticationProvider → UserDetailsService + PasswordEncoder; then method security via AOP*

**438. How do you secure REST APIs?**
   🔑 *HTTPS/TLS everywhere, authenticate with OAuth2/JWT (short TTL, validated signature/iss/aud/exp), authorize deny-by-default with roles/scopes + method security and object-level checks, validate and sanitize input, rate limit and throttle, CORS allowlist, security headers (HSTS, CSP, X-Content-Type-Options, X-Frame-Options), no sensitive data in URLs or logs, lock down actuator/swagger in prod, audit logging, dependency scanning, secrets in a vault*

**439. How do you handle common vulnerabilities like SQL injection?**
   🔑 *Prepared statements/bind parameters (JPA, JdbcTemplate with ?), never concatenate user input, whitelist dynamic identifiers (table/column/sort direction), use Criteria API/QueryDSL for dynamic queries, least-privilege DB user, input validation, stored procedure parameters, WAF as defence in depth, and SAST (SonarQube/Checkmarx) in CI*

**440. How do you handle service secrets and external configuration?**
   🔑 *Externalize everything (12-factor): Spring Cloud Config/Consul/K8s ConfigMap for non-secrets, Vault/Secrets Manager/External Secrets for secrets; inject as env vars or mounted files, @ConfigurationProperties binding, profiles per environment, @RefreshScope + Spring Cloud Bus for live reload, encrypt at rest, rotate, never commit or bake into images, audit access*

**441. How do you handle all DB URLs and other configuration in a centralized way?**
   🔑 *Central config server (Spring Cloud Config backed by Git, Consul KV, or K8s ConfigMaps) with per-service and per-profile files, shared application.yml for common properties, secrets resolved from Vault/Secrets Manager at startup or via sidecar, service discovery instead of hardcoded hosts, versioned in Git with review, refresh without redeploy, and a clear precedence order*

**442. How do you handle security — microservice-to-microservice security (deep and clear answer)?**
   🔑 *Defence in depth: mTLS for transport identity (mesh-issued, auto-rotated certs, SPIFFE IDs) + OAuth2 client-credentials or signed service JWTs for application identity with audience/scope checks per hop + authorization policy in each service (never trust 'inside the cluster') + network policies restricting pod-to-pod + propagate end-user context in a signed token so downstream can do user-level authz + short-lived credentials, secret rotation, audit logging, and rate limiting between services*

**443. What type of data does a JWT hold — is a JWT encrypted or just encoded?**
   🔑 *Header (alg, typ, kid), payload claims (iss, sub, aud, exp, iat, nbf, jti + roles/scopes/tenant), signature; a plain JWS is only Base64url-ENCODED and signed — fully readable by anyone, so never put secrets/PII in it; integrity and authenticity are protected, confidentiality is not; use JWE if you truly need an encrypted token, and always rely on TLS in transit*

**444. Other options instead of JWT, and why JWT?**
   🔑 *Alternatives = opaque reference tokens + introspection (instant revocation, server-side state), classic server sessions with a cookie (simple, revocable, needs sticky/shared session store), PASETO (safer defaults than JWT), SAML assertions, API keys, mTLS certs; JWT wins for stateless horizontal scaling, cross-service trust without a shared session store, self-contained claims and standard library support — at the cost of hard revocation and size*

**445. What is TLS and SSL, and how do you implement it for API security in the first stage?**
   🔑 *Same as above — TLS 1.2/1.3 only, valid CA cert at the edge, HTTP→HTTPS redirect, HSTS, strong ciphers, automated renewal (cert-manager/ACM/Let's Encrypt), then mTLS for internal hops and certificate pinning for high-value mobile clients*

**446. Complete authorization and authentication flow, with class- and method-level access**
   🔑 *Request → authentication filter (JWT/session) → Authentication object with authorities in SecurityContextHolder → URL-level rules in authorizeHttpRequests → controller → method-level @PreAuthorize("hasRole('ADMIN') and #id == authentication.principal.id") / @PostAuthorize / @PostFilter → service enforces domain-object ownership → 401 for unauthenticated, 403 for unauthorized; deny by default, test negative cases*

**447. How do you provide security to a REST API — session management, JWT, certificates, roles? Complete implementation in steps (no code needed)**
   🔑 *Steps: (1) TLS everywhere, (2) choose auth model — stateless JWT/OAuth2 resource server vs server session, (3) identity provider (Keycloak/Cognito/Okta) issues tokens via Authorization Code+PKCE or client credentials, (4) validate token signature/claims in a filter and populate SecurityContext, (5) configure SecurityFilterChain with stateless sessions, CORS, CSRF off for token APIs, deny-by-default rules, (6) add method-level and object-level authorization with roles/scopes, (7) refresh token rotation and revocation strategy, (8) rate limiting, input validation, security headers, (9) lock down actuator/docs, (10) audit logging, monitoring and dependency scanning, (11) rotate secrets in Vault, (12) security tests in CI*

**448. Security checks and additional security layers on top for a service**
   🔑 *Layered: WAF/DDoS at the edge, TLS/mTLS, gateway authn + rate limiting, service-level authz, input validation and output encoding, secrets management, least-privilege IAM and DB users, network policies, container image scanning and non-root read-only filesystem, SAST/DAST/SCA in CI, audit logging and anomaly alerting, pen testing, security headers, and regular patching*

**449. How do you store secure details in a DB, and how do you store username and password in a DB?**
   🔑 *Passwords → BCrypt/Argon2id with per-user salt (never MD5/SHA, never encryption, never plaintext); other sensitive fields → AES-GCM envelope encryption with KMS/Vault-managed keys and rotation, blind index for searchability, masking in logs and lower environments, least-privilege DB access, auditing, and purge/retention policies*

**450. How does CSRF work in session-based APIs?**
   🔑 *The browser automatically attaches the session cookie to any request to your domain, including one triggered by an attacker's site, so a forged POST runs as the logged-in user; defence = a per-session synchronizer token rendered into the form/header that the attacker can't read due to the same-origin policy, plus SameSite=Lax/Strict cookies and Origin/Referer validation; Spring Security enables CsrfFilter by default for session apps (CookieCsrfTokenRepository for SPAs) and it's safely disabled only for stateless token-in-header APIs*

## Performance, Caching & Scalability

**451. How do you find and fix a slow endpoint in production — profiling tools, APM (New Relic/Dynatrace/Datadog), thread dumps, flame graphs?**
   🔑 *APM dashboards for p95/p99 latency and trace waterfall; thread dump for blocked/waiting threads; async-profiler/JProfiler flame graph for CPU hotspots; SQL slow-query log + explain plan for N+1; distributed trace to isolate which service/call is slow; fix: add index, enable caching, go async, tune pool*

**452. Caching strategy — cache-aside vs write-through vs write-behind — Redis vs local (Caffeine/Guava) — when do you use each, and how do you handle cache invalidation/stampede?**
   🔑 *Cache-aside = app reads/writes cache manually (miss → DB → populate); write-through = synchronous write to cache+DB; write-behind = async flush to DB; local Caffeine = ultra-low latency per-pod, no serialization; Redis = shared distributed cache; invalidation: TTL, event-driven eviction, version key; stampede: probabilistic early expiry, lock/mutex, request coalescing*

**453. How do you design a system to handle a 10x traffic spike (e.g., flash sale, batch settlement window) without falling over?**
   🔑 *Horizontal autoscaling (HPA/KEDA), CDN for static assets, pre-warm caches, Kafka/SQS to buffer burst writes, rate limiting (token bucket), circuit breakers to shed load, async processing for non-critical paths, read replicas, load test ahead with Gatling to find saturation point*

**454. Connection pool tuning (HikariCP) — what metrics do you watch, and what's a real pool-exhaustion incident you've debugged?**
   🔑 *Key settings: maximumPoolSize, minimumIdle, connectionTimeout, idleTimeout, maxLifetime; metrics: hikaricp.connections.pending, active, idle; symptom: SQLTimeoutException / HikariPool timed out waiting; root causes: long transactions holding connections, N+1, missing index causing slow queries, undersized pool; fix: reduce transaction scope, add index, size pool = (core count × 2) + effective_spindle_count*

**455. How do you do capacity planning / load testing before a major release (JMeter/Gatling, and what SLAs did you validate against)?**
   🔑 *Define target SLAs (e.g. p99 < 200ms at 1000 RPS, error rate < 0.1%); JMeter/Gatling scripts: ramp-up → steady-state → spike → soak; measure throughput, latency percentiles, error rate, resource saturation; gate in CI pipeline; capacity plan: headroom above peak traffic; chaos engineering for failure modes*

**456. Redis use cases: caching, session store, distributed locks, rate limiting**
   🔑 *Cache: GET/SET with TTL; session store: serialize session object, share across pods; distributed lock: SET key uuid NX PX 5000 + Lua script for atomic release; rate limiting: INCR + EXPIRE (fixed window) or sorted set (sliding window); pub/sub; streams; eviction: LRU/LFU/volatile-ttl; Sentinel/Cluster for HA*

**457. Coding: Implement a Redis-based distributed lock (or cache-aside pattern) in Java**
   🔑 *Distributed lock: SET key UUID NX PX 5000 → returns OK if acquired; release with Lua (GET→compare UUID→DEL atomically); Redisson RLock as production alternative; cache-aside: ValueOperations.get(key) → null: fetch from DB → ops.set(key, value, ttl, SECONDS)*

**458. Explain caching strategies (Redis, local cache) in microservices.**
   🔑 *Cache-aside most common; TTL per entity type; local (Caffeine) avoids network RTT but causes cache drift across pods; Redis = shared source of truth; invalidation hardest problem in distributed cache; use cache for idempotent read-heavy paths; never cache mutable financial state without strong invalidation*

**459. How cache working internally ?**
   🔑 *In-process (Caffeine): hash map + frequency sketch (Window-TinyLFU for eviction); Redis: in-memory hash dict per key type (string, list, hash, zset), event loop (single-threaded command execution), expiry via lazy deletion + active sweep; eviction on memory pressure; persistence: RDB snapshot / AOF log*

**460. How would you identify and resolve performance bottlenecks in a Java application?**
   🔑 *Profile first: async-profiler/JProfiler for CPU flame graph; GC logs (-Xlog:gc) for pause time; thread dump (jstack) for contention; heap dump (jmap/MAT) for memory; SQL slow-query log; APM for end-to-end latency; fix in order: algorithmic complexity → DB indexes → caching → async → JVM tuning*

**461. performance testing**
   🔑 *Load testing tools: JMeter (GUI + scriptable), Gatling (Scala DSL, CI-friendly), k6 (JS, cloud-native); define scenarios: ramp-up, steady-state, spike, soak; metrics: RPS, latency (p50/p95/p99), error rate, CPU/memory saturation; gate in CI; compare against SLA baselines*

**462. TTL, cache key, cache vector — custom configuration**
   🔑 *TTL: entry expiry time — too short = miss storm, too long = stale data; cache key: unique identifier (entity-type:id:version) — must be collision-free; cache vector/version tag: suffix bumped on schema/data change for bulk invalidation (cache busting); custom per-entity TTL in Spring: @Cacheable with cacheManager bean per region*

**463. How do you optimize application startup time?**
   🔑 *Lazy initialization (spring.main.lazy-initialization=true / @Lazy); reduce component scan scope; AOT compilation (Spring Native + GraalVM = instant start, low memory); AppCDS (class data sharing across JVM instances); -XX:TieredStopAtLevel=1 for dev; minimize auto-configuration; Testcontainers reuse for tests*

**464. Scaling for high traffic**
   🔑 *Horizontal scaling (stateless services + LB); autoscaling (HPA/KEDA); externalise session (Redis); read replicas for DB; CDN for static content; caching hot paths; async processing (Kafka/SQS); connection pool tuning; rate limiting; circuit breakers; eventual consistency where acceptable*

**465. How do you handle a service that has trillions of requests of traffic?**
   🔑 *Sharding/partitioning (data + traffic), consistent hashing; distributed caching (Redis cluster); async event-driven (Kafka partitions for parallelism); CQRS (separate read/write paths); read replicas; multi-region active-active; stateless pods + Kubernetes autoscaling; bulkheads and circuit breakers; backpressure*

**466. How do you handle 100k requests per second?**
   🔑 *Reactive/non-blocking I/O (WebFlux/virtual threads to avoid thread-per-request blocking); horizontal scaling behind load balancer; Redis for hot lookups; DB read replicas + connection pool; Kafka for async write fan-out; CDN; autoscaling; avoid synchronous external calls on hot path; benchmark with Gatling to find bottleneck*

**467. If we have 50M requests per second, which part of the service should we focus on more — consistency handling, traffic handling, DB connection pool, locking, asynchronous processing, query issues?**
   🔑 *Priority order: (1) traffic handling (LB, autoscaling, CDN); (2) async processing (decouple write path via Kafka); (3) caching (eliminate DB reads for hot data); (4) query optimization + indexes; (5) connection pool sizing; (6) locking minimisation (optimistic > pessimistic, avoid distributed locks on hot path); (7) consistency model (choose eventual where safe)*

**468. How do you handle traffic?**
   🔑 *Load balancer (round-robin / least-connections / IP-hash); autoscaling; rate limiting (token bucket per client); request queuing with backpressure; circuit breaker for downstream protection; bulkhead (thread pool isolation); async non-blocking I/O; graceful degradation (return cached/default response)*

**469. Cache**
   🔑 *Temporary fast-access storage for expensive DB queries or computations; cache hit = serve from memory; miss = fetch from source and populate; TTL for freshness; eviction policies (LRU/LFU); local (Caffeine) vs distributed (Redis); invalidation strategies (TTL, event-based, manual evict); consistency vs performance trade-off*

**470. How do you handle cache with time-based update and forced update?**
   🔑 *Time-based: TTL expiry → on next miss re-fetch and repopulate (lazy refresh); scheduled refresh (background thread reloads cache before TTL expires = stale-while-revalidate); forced update: explicit eviction via @CacheEvict or RedisTemplate.delete(key); event-driven invalidation (on entity update, publish eviction event)*

**471. How does caching work internally?**
   🔑 *Same as 459 — hash map data structure; Caffeine: Window-TinyLFU frequency sketch for admission + eviction; Redis: per-type encoding (embstr, ziplist, quicklist, skiplist), event loop for atomic ops, expiry heap + lazy/active expiry; Spring @Cacheable: AOP proxy intercepts method call, checks CacheManager, returns cached value or proceeds*

**472. Memory optimization because the service runs out of memory**
   🔑 *Diagnose: enable -XX:+HeapDumpOnOutOfMemoryError; analyze heap dump with Eclipse MAT (leak suspects, dominator tree, retained heap); common leaks: unbounded static collections, unclosed streams/connections, ThreadLocal not removed, large cache without maxSize; fix: bound Caffeine/Redis cache, weak references, close resources in try-with-resources, tune -Xmx, use off-heap for large buffers*

**473. How do you optimize JVM startup time?**
   🔑 *-XX:TieredStopAtLevel=1 (skip C2 JIT, fast start, lower peak throughput — dev only); AppCDS: dump class list → archive → reuse on next start; GraalVM native-image: AOT compiled, millisecond start, low memory (no JIT, limited reflection); Spring Boot 3 AOT + native; CRaC (Checkpoint/Restore) for JVM snapshot; reduce bean count and classpath scan*

**474. How do you optimize your code?**
   🔑 *Algorithm first (reduce O complexity); avoid premature optimization; profile before optimizing; StringBuilder over String concatenation in loops; stream lazy evaluation + short-circuit; batch DB calls (findAllById vs N×findById); async for I/O-bound paths; cache hot computations; use primitives over boxed types; avoid unnecessary object allocation in hot paths*

## Production Troubleshooting, Logging & Observability

**475. Observability — how do you correlate a request across 6 microservices (distributed tracing — Sleuth/Zipkin/OpenTelemetry, correlation IDs)?**
   🔑 *TraceId generated at entry point, propagated via HTTP headers (B3 / W3C traceparent); each service creates child spans; Micrometer Tracing + Brave/OTel auto-instruments Spring Boot; spans exported to Zipkin/Jaeger; MDC.put("traceId", ...) for log correlation; correlation ID in every log line enables cross-service log search in ELK/Splunk; sampling rate to control volume*

**476. Structured logging strategy — what do you log, what do you deliberately never log, and how do you keep log volume/cost sane at scale?**
   🔑 *Log: request ID, user/tenant ID, timing, business events (payment processed), errors with stack trace, state transitions; NEVER log: passwords, tokens, PAN/card numbers, PII (GDPR), secrets; keep sane: JSON structured logs, INFO in prod (DEBUG only on demand via log level API), sampling for high-volume paths, log aggregation with ILM/retention policies, alert on errors not on every log line*

**477. How do you handle logging/monitoring/observability (ELK, Prometheus, Grafana)?**
   🔑 *ELK: Filebeat ships logs → Logstash parses/enriches → Elasticsearch indexes → Kibana dashboards + alerts; Prometheus: scrapes /actuator/prometheus metrics (JVM, HikariCP, custom); Grafana: dashboards for latency p99, error rate, GC pause, throughput; Alertmanager: alert rules → PagerDuty/Slack; three pillars: logs + metrics + traces*

**478. Loggin, alert, production failure, monitoring**
   🔑 *Logging: SLF4J + Logback/Log4j2, structured JSON via logstash-logback-encoder, MDC for correlation; alerting: threshold-based on error rate/latency (Grafana/Prometheus alerts → PagerDuty); production failure: runbook-driven triage, on-call rotation; monitoring: APM + custom business metrics + health checks (/actuator/health)*

**479. Service throwing connection pool exhausted error in production, how would you diagnosis**
   🔑 *Check hikaricp.connections.pending and hikaricp.connections.active metrics; thread dump to find threads blocked waiting for connection; slow query log for long-running transactions holding connections; check for connection leaks (getConnection without close in try-with-resources); increase pool size temporarily; fix root cause: reduce transaction scope, add index, close connections properly*

**480. ELK implementation**
   🔑 *Elasticsearch: distributed search/storage (shards + replicas); Logstash: ingest pipeline (input → filter/grok/mutate → output); Kibana: visualization + dashboards + alerting; Filebeat: lightweight shipper on each pod; JSON logs for easy parsing; ILM (Index Lifecycle Management) for retention/rollover; structured logs avoid regex parsing; consider OpenSearch as open-source alternative*

**481. What do you use for logging? How do you configure it?**
   🔑 *SLF4J API (facade) + Logback impl (or Log4j2); logback-spring.xml: appenders (ConsoleAppender, RollingFileAppender), encoders (PatternLayout or LogstashEncoder for JSON), log levels per package; async appender (AsyncAppender) to avoid I/O blocking hot path; MDC for traceId/userId; spring.logging.level in application.yml; actuator /loggers for runtime level change*

**482. How did you debug out of memory exception and how to solve**
   🔑 *Enable -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/; GC logs (-Xlog:gc*) to see if GC overhead limit exceeded; generate heap dump (jmap -dump:format=b,file=heap.hprof <pid>); analyze with Eclipse MAT: leak suspects, dominator tree, retained heap by class; common causes: static collection growing unboundedly, unclosed streams, oversized cache without maxSize, ThreadLocal not removed; fix: bound cache, try-with-resources, tune -Xmx*

**483. Heap dump analysis**
   🔑 *Generate: -XX:+HeapDumpOnOutOfMemoryError, or jmap -dump:format=b,file=heap.hprof <pid>, or JVisualVM; analyze in Eclipse MAT: Overview → Leak Suspects wizard; Dominator Tree (largest retained objects); Histogram (instances by class); OQL for custom queries; find: large char[] (String leaks), HashMap$Entry chains, class loader leaks (PermGen/Metaspace); correlate with recent code changes*

**484. How to handle production issue**
   🔑 *Triage: who is affected, severity (S1/S2/S3), blast radius; check dashboards + alerts; correlate with recent deploy (rollback candidate); pull logs for error patterns; check resource metrics (CPU, memory, DB connections, GC); escalate per on-call runbook; if clear root cause → hotfix; else rollback; post-incident: blameless postmortem, 5-whys, action items, runbook update*

**485. How do you check for crashes and failures?**
   🔑 *Health check endpoints (/actuator/health with DB, Redis, Kafka indicators); K8s liveness probe (pod restart on failure); exception tracking (Sentry/Datadog error tracking); GC overhead alerts; OOM kill events (K8s OOMKilled status); deadlock (thread dump via jstack / kill -3); circuit breaker open events; alert on error rate spike; log-based alerting (ERROR count threshold)*

**486. Application is slower in production compared to dev — what could be the issue?**
   🔑 *Data volume (prod has millions of rows → full table scan); network latency to DB/external services (dev is local); JVM JIT warmup (C2 compilation takes time under load); connection pool contention under concurrent load; GC pressure with real data; missing index on production data volume; prod config differences (pool size, timeouts, log level → synchronous log I/O); environment resource limits (CPU/memory throttling)*

**487. API is suddenly slow — what will you check first and how do you handle it?**
   🔑 *Step 1: APM dashboard — which endpoint, what latency, since when; Step 2: correlate with deploy (rollback?); Step 3: DB slow-query log — N+1 or missing index; Step 4: downstream service latency (distributed trace); Step 5: thread dump — blocked threads; Step 6: GC pause log — stop-the-world events; Step 7: connection pool wait time; Step 8: CPU/memory metrics; fix the first confirmed bottleneck*

**488. Production issue at midnight — how do you handle and investigate it?**
   🔑 *Acknowledge PagerDuty alert; check status page/dashboards for scope; pull recent logs + APM trace; check for recent deploys (auto-rollback trigger); follow on-call runbook; hotfix or rollback as fastest path to recovery; communicate status to stakeholders via incident channel; preserve artefacts (logs, thread dumps) for postmortem; blameless postmortem next business day*

**489. DB CPU is low but API latency is high — what could be the reason?**
   🔑 *Application-level bottleneck: thread pool exhausted (Tomcat threads waiting for connection/lock); slow external service call blocking request thread; GC stop-the-world pause; in-app lock contention or synchronised block; insufficient DB connection pool (waiting to acquire connection, not executing query); serialization overhead; N+1 queries (many small fast queries overwhelming the pool instead of one slow query); check thread dump and connection pool wait metric*

**490. A critical service goes down during business hours — what steps do you take?**
   🔑 *Immediate: declare incident, notify stakeholders; check K8s pod status (OOMKilled/CrashLoopBackOff/pending); check logs for crash cause; check dependencies (DB/Kafka/Redis health); if recent deploy → rollback; restore service (scale up, restart, fix config); preserve crash artefacts; communicate ETA; postmortem: root cause, detection gap, preventive measures, runbook update*

**491. One production issue and how you fixed it, and the most difficult bug you solved**
   🔑 *STAR format: Situation (context/impact), Task (your role), Action (diagnostic steps — thread dump, heap dump, slow query, trace; the fix applied), Result (resolution metrics + preventive measures); common examples: N+1 queries causing DB overload, memory leak from unbounded cache, deadlock in distributed lock, race condition in idempotency check, misconfigured connection pool timeout*

**492. How did you debug an OutOfMemoryError and how did you solve it?**
   🔑 *Enable -XX:+HeapDumpOnOutOfMemoryError; heap dump → Eclipse MAT → leak suspects; identified: static Map in a singleton accumulating request objects without eviction; fix: replaced with Caffeine cache (maximumSize + TTL); also tuned -Xmx; added GC log alerting; added unit test to verify cache bounded; mem usage dropped from 95% to 60% under same load*

**493. Logging, alerting, production failure and monitoring**
   🔑 *Same triad: Logging (SLF4J + Logback JSON + MDC traceId); alerting (Prometheus rules / Grafana alerts → PagerDuty on error rate > 1% or p99 > SLA threshold); production failure (runbook-driven triage → fix/rollback → postmortem); monitoring (APM for latency/throughput, Grafana for JVM/infra metrics, distributed tracing for cross-service correlation)*

**494. What is Splunk, how do you use it and why?**
   🔑 *Splunk: log aggregation, search and analytics platform; SPL (Splunk Processing Language) for ad-hoc queries and dashboards; ingest logs from Filebeat/HEC; dashboards for error rate, latency percentiles, business KPIs; alerts on log patterns (ERROR/EXCEPTION count spike, specific error codes); used to correlate logs across services by traceId; licensed by daily data ingestion volume; alternative: ELK/OpenSearch (open-source)*

## Testing & Code Quality

**495. Unit vs integration vs contract testing — what's your team's actual test pyramid look like in practice (not textbook)?**
   🔑 *Unit: fast, isolated, mock deps, many (Mockito + JUnit 5); integration: real DB/Kafka/Redis via Testcontainers, fewer but catch real issues; contract (Pact): consumer publishes expectations, provider verifies — prevents silent breaking changes; E2E: minimal (slow, flaky); in practice: heavy integration layer because mocked tests passed but prod diverged; test pyramid is inverted in legacy codebases*

**496. Mockito — mocking pitfalls you've seen (over-mocking, testing implementation instead of behavior).**
   🔑 *Over-mocking: mocking everything = testing nothing real, implementation changes break tests without real bugs; verifying internals (verify(repo).save(any())) couples test to impl; deep stubs (mock.getX().getY()) = design smell; mocking value objects; using @Spy on classes with complex constructors; missing when() setup returns null silently (causes NPE in test); prefer behavior assertions over interaction verification*

**497. Testcontainers — how have you used it to test real DB/Kafka interactions instead of mocking them away?**
   🔑 *@Container PostgreSQLContainer / KafkaContainer / RedisContainer; @DynamicPropertySource to inject JDBC URL / bootstrap servers into Spring context; @Testcontainers on class; lifecycle: per-test (isolation) or static/per-class (faster); avoids mock drift from real driver/broker behavior; catches migration issues (Flyway runs against real Postgres); slower than H2 but catches real constraint/type/serialization issues*

**498. How do you test asynchronous/event-driven flows reliably (avoiding flaky sleep-based tests)?**
   🔑 *Awaitility: await().atMost(10, SECONDS).until(() -> repo.findById(id).isPresent()); CountDownLatch in listener; @KafkaListener test with @EmbeddedKafka + KafkaTemplate to publish + Awaitility to poll result; CompletableFuture.get(timeout, SECONDS); never Thread.sleep (time-dependent, flaky on slow CI); use mutable state + assertion or capture via ArgumentCaptor with polling*

**499. What's your approach to testing @Transactional rollback behavior and idempotency logic?**
   🔑 *Rollback: @DataJpaTest with @Rollback (default true); manually inject TransactionTemplate to verify rollback; throw exception mid-transaction and assert state unchanged in separate query; idempotency: send same request twice with same idempotency key, assert DB has single record and same response returned; test unique constraint violation is handled gracefully*

**500. Mockito: @Mock vs @MockBean vs @Spy**
   🔑 *@Mock: plain Mockito mock (no Spring context), used with @ExtendWith(MockitoExtension.class), fast; @MockBean: replaces/registers bean in Spring ApplicationContext (used with @SpringBootTest or @WebMvcTest), slower — restarts context; @Spy: real object with ability to stub specific methods, calls real impl for unstubbed; @InjectMocks: injects @Mock/@Spy into class under test; @Captor: captures arguments passed to mock*

**501. How do you write an integration test for a REST controller with @SpringBootTest + MockMvc or TestRestTemplate?**
   🔑 *@SpringBootTest(webEnvironment=RANDOM_PORT) + TestRestTemplate for full HTTP; or @WebMvcTest(MyController.class) for controller-slice (no full context, mock service with @MockBean) + @AutoConfigureMockMvc; MockMvc: mockMvc.perform(post("/api/orders").contentType(JSON).content(body)).andExpect(status().isCreated()).andExpect(jsonPath("$.id").exists()); @DirtiesContext when state changes between tests*

**502. How do you mock a Kafka producer/consumer or an external REST call in a unit test?**
   🔑 *Kafka: @EmbeddedKafka in integration test (real broker in-process); @MockBean KafkaTemplate in unit test; WireMock for external REST: @AutoConfigureWireMock or WireMockServer; stubFor(get(urlEqualTo("/external/api")).willReturn(aResponse().withBody("{}").withStatus(200))); verify(getRequestedFor(urlEqualTo(...))); Feign client: @MockBean in @WebMvcTest; RestTemplate: MockRestServiceServer*

**503. Coding: Write a JUnit 5 + Mockito test for a service method that calls a repository and an external Feign client**
   🔑 *@ExtendWith(MockitoExtension.class); @Mock UserRepository repo; @Mock ExternalClient client; @InjectMocks UserService service; given: when(repo.findById(1L)).thenReturn(Optional.of(user)); when(client.getDetails(1L)).thenReturn(details); act: Result result = service.process(1L); assert: assertThat(result.getName()).isEqualTo("expected"); verify: verify(client).getDetails(1L); test edge case: when repo returns empty → throw ResourceNotFoundException*

**504. Difference between unit, integration, and service-level tests.**
   🔑 *Unit: single class in isolation, all deps mocked, fast (ms), many; integration: multiple real components (DB, Kafka, Redis via Testcontainers), tests real interactions, slower (seconds); service-level/component test: tests entire service as black box via HTTP (Pact provider test, smoke test); contract test: verifies API contract between services; E2E: full stack, all services running, slowest*

**505. What tools have you used for mocking (Mockito, WireMock)?**
   🔑 *Mockito: @Mock, @Spy, @Captor, @InjectMocks, when/thenReturn/thenThrow, verify, ArgumentCaptor, doReturn for void methods; WireMock: stub HTTP endpoints (stubFor/givenThat), verify interactions, record/replay, fault simulation (delay, connection reset); EmbeddedKafka for Kafka; MockMvc for Spring controllers; H2 in-memory DB for repository tests; Testcontainers for real infra*

**506. How do you measure code coverage, and what's a good target?**
   🔑 *JaCoCo: instrument bytecode, generate line/branch/method/class coverage reports; integrate with Maven (jacoco-maven-plugin) or Gradle; SonarQube imports JaCoCo XML; CI quality gate fails if coverage drops below threshold; typical target: 70–80% (higher for business logic, lower for generated code); coverage ≠ quality — mutation testing (PIT) measures how many bugs tests can detect; focus coverage on domain logic, not DTOs/configs*

**507. Explain contract testing (Pact) in microservices.**
   🔑 *Consumer-driven contract testing: consumer defines expected interactions (request/response) and publishes Pact file to Pact Broker; provider runs pact:verify to confirm it satisfies all consumer expectations; catches breaking API changes before deployment; prevents the "who broke what" problem in microservices; more reliable than shared integration environments; integrates in CI; can-i-deploy command checks compatibility before release*

**508. What is SonarQube used for? How do you fix code smells it flags?**
   🔑 *Static analysis: bugs (potential NPE, resource leak), vulnerabilities (SQL injection, hardcoded credential), code smells (long method, deep nesting, magic numbers, duplications), security hotspots, test coverage; quality gate blocks PR merge if thresholds violated; fix code smells: extract method (long method), introduce constants (magic numbers), reduce cyclomatic complexity, remove duplication (extract shared util); runs in CI pipeline via SonarScanner*

**509. Have you used Checkmarx or similar SAST tools? Walk through a finding you remediated.**
   🔑 *SAST (Static Application Security Testing): scans source code for security vulnerabilities without running app; Checkmarx/Veracode/Semgrep; common findings: SQL injection (parameterized queries fix), XSS (output encoding), path traversal (validate/canonicalize file paths), hardcoded credentials (move to Vault/env), insecure deserialization; triage false positives; fix → rescan → verify resolved; integrate in CI as build-breaking gate for critical findings*

**510. What is Zally, and how is it used for API linting/governance?**
   🔑 *Zally: open-source API linting tool by Zalando; validates OpenAPI/Swagger specs against API guidelines (REST maturity, naming conventions, versioning, pagination, error response structure, required headers); run as CLI or CI step; custom rules for org-specific standards; enforces API governance across teams; prevents inconsistent APIs before they are published*

**511. What is mock and sub , other methods**
   🔑 *Mock: fake implementation with no real logic, returns configured values; Stub: canned response for specific input; Spy: real object with some methods overridden; Fake: working implementation (in-memory) for tests; Dummy: placeholder object never used; Mockito provides Mock/Spy/Captor; verify() checks interactions; ArgumentCaptor captures actual arguments; doReturn/when for void and spy methods*

**512. Integration test vs unit test**
   🔑 *Unit: tests single class with all dependencies mocked, fast, isolated, many; integration: tests real interactions between components (DB, Kafka, HTTP), slower, fewer; use unit for business logic + edge cases; integration for repository queries, Kafka consumers, REST controller with real context; both needed — unit catches logic bugs, integration catches wiring/config/query bugs*

**513. What annotations do you use for testing, and how have you used Mockito?**
   🔑 *JUnit 5: @Test, @BeforeEach, @AfterEach, @ParameterizedTest, @ExtendWith; Mockito: @ExtendWith(MockitoExtension.class), @Mock, @Spy, @InjectMocks, @Captor; Spring: @SpringBootTest, @WebMvcTest, @DataJpaTest, @AutoConfigureMockMvc, @MockBean, @TestPropertySource, @DirtiesContext; Testcontainers: @Testcontainers, @Container; WireMock: @AutoConfigureWireMock*

**514. What does Sonar check in your project and how did you use it?**
   🔑 *In project: integrated in Jenkins/GitLab CI; sonar-maven-plugin runs after unit tests; JaCoCo XML fed into Sonar for coverage; quality gate configured: no new critical bugs, coverage ≥ 80%, no new blocker smells, no new security vulnerabilities; Sonar PR decoration shows findings inline in GitHub; resolved: eliminated long methods by extraction, fixed potential NPEs, removed duplicated code blocks*

**515. Automation testing**
   🔑 *Levels: unit (Mockito/JUnit), integration (Testcontainers/Spring), API/service (RestAssured/Karate), UI (Selenium/Playwright); BDD: Cucumber + Gherkin for business-readable scenarios; test data management (factories, fixtures); parallel execution (JUnit 5 parallel, Selenium Grid); CI integration (fail fast); reporting (Allure, ExtentReports); page object model for UI; contract tests (Pact) for microservice boundaries*

## Cloud, DevOps, Docker, Kubernetes & CI/CD

**516. Walk through your CI/CD pipeline end-to-end — build, test gates, image build, deployment strategy, rollback trigger.**
   🔑 *Code push → GitHub/GitLab triggers pipeline → mvn/gradle build → unit + integration tests → SonarQube quality gate → Docker image build (multi-stage) → push to ECR/Artifactory → deploy to K8s via Helm/ArgoCD → smoke test → automatic rollback on error rate spike or failed health check; approval gate before prod; feature flags for progressive rollout*

**517. Blue-green vs canary deployment — trade-offs, and which have you actually run in production?**
   🔑 *Blue-green: two identical environments, instant traffic switch at LB, zero downtime, easy rollback, doubled infra cost, DB migrations must be backward-compatible; canary: gradual traffic shift (1%→10%→50%→100%), catches issues with subset of users, complex routing (Istio/Argo Rollouts), slower rollout; blue-green for high-confidence releases, canary for risk-averse changes or ML models*

**518. How do you handle zero-downtime deployments when the schema itself is changing (expand-contract migration pattern)?**
   🔑 *Expand: add new column (nullable/with default), old code ignores it; (2) Migrate: deploy new code that writes to both old and new columns; (3) Backfill: populate new column for existing rows; (4) Contract: deploy code that reads from new column only; (5) Cleanup: drop old column; each step is independently deployable; Flyway/Liquibase for versioned migrations; never rename or drop in same deploy as usage*

**519. Kubernetes — how do you configure readiness vs liveness probes correctly for a Spring Boot app (and what goes wrong if you get them backwards)?**
   🔑 *Readiness: is the app ready to serve traffic (DB connected, caches warmed, Kafka consumer up) — K8s removes from Service endpoints if failing; Liveness: is the app alive (not deadlocked or hung) — K8s restarts pod if failing; wrong: if liveness checks DB and DB is slow → restart loop; liveness should be minimal (e.g., /actuator/health/liveness checks only internal state, not external deps); readiness can check external deps; /actuator/health/readiness and /liveness auto-configured in Spring Boot 2.3+*

**520. How do you handle graceful shutdown so in-flight requests aren't dropped during a pod termination/scale-down?**
   🔑 *K8s sends SIGTERM → app stops accepting new requests → drains in-flight requests → exits; Spring Boot: server.shutdown=graceful + spring.lifecycle.timeout-per-shutdown-phase=30s; K8s: terminationGracePeriodSeconds ≥ timeout; preStop lifecycle hook (sleep 5s) to wait for LB to deregister pod before SIGTERM; Kafka consumers: commitSync on shutdown, finish current batch*

**521. What build tool do you use (Maven/Gradle) and why — any multi-module project experience?**
   🔑 *Maven: XML POM, convention-heavy (lifecycle phases: validate/compile/test/package/install/deploy), ubiquitous, straightforward; Gradle: Groovy/Kotlin DSL, incremental builds (only rebuild changed modules), build cache, faster for large projects, more flexible; multi-module Maven: parent POM with <modules>, <dependencyManagement> for version alignment; Gradle: settings.gradle + include(':module'), subprojects block; Maven wrapper (mvnw) for reproducibility*

**522. Difference between EC2, Lambda, ECS — when serverless (Lambda) vs container (ECS/EKS)**
   🔑 *EC2: full VM, persistent, you manage OS/patching, flexible, long-running workloads; Lambda: serverless FaaS, event-triggered (API GW, SQS, S3, EventBridge), auto-scales to 0, pay-per-invocation, 15-min limit, cold starts, stateless; ECS (Fargate): managed container service, no node management, persistent microservices; EKS: Kubernetes on AWS; use Lambda for simple event handlers and intermittent workloads; ECS/EKS for always-on microservices*

**523. How does an SQS queue help decouple microservices?**
   🔑 *Producer puts message and returns immediately (async); consumer polls at its own rate (independent scaling); SQS buffers traffic bursts; DLQ for failed messages after max retries; visibility timeout = processing window; FIFO queue for ordered processing; at-least-once delivery (idempotent consumers required); decouples availability — producer and consumer can be down independently without data loss*

**524. Docker: image vs container, multi-stage builds — why use them?**
   🔑 *Image: immutable read-only blueprint of filesystem layers (FROM, COPY, RUN each add a layer); container: running instance of image (writable layer on top); multi-stage: first stage uses JDK to compile (FROM eclipse-temurin:21 AS builder; RUN mvn package), second stage copies only the JAR to slim JRE image (FROM eclipse-temurin:21-jre) → smaller final image (no build tools, maven cache, source code), faster pull, smaller attack surface*

**525. Kubernetes: Pod, Deployment, Service, ConfigMap/Secret — how does a Service discover Pods?**
   🔑 *Pod: smallest deployable unit (1+ containers); Deployment: manages desired replica count + rolling update strategy; Service: stable DNS name + virtual IP that load balances to matching Pods via label selector (app=myservice); discovery: kube-proxy updates iptables/IPVS rules; CoreDNS resolves service-name.namespace.svc.cluster.local; ConfigMap: non-sensitive config (env vars or mounted file); Secret: base64-encoded sensitive values (use ExternalSecrets for real security)*

**526. How do you set up a CI/CD pipeline in Jenkins/GitLab for a Spring Boot app (build → test → Docker image → push to JFrog Artifactory → deploy to K8s)?**
   🔑 *Jenkinsfile/GitLab CI YAML: stages: checkout → mvn clean verify (unit + integration) → SonarQube → docker build -t myapp:${GIT_SHA} → docker push to Artifactory/ECR → helm upgrade --install myapp ./chart --set image.tag=${GIT_SHA} -n prod; credentials via Jenkins credentials store; shared library for reuse; environment parameters; approval stage before prod*

**527. How do you manage secrets (DB passwords, JWT secret) in Kubernetes/AWS securely?**
   🔑 *Never hardcode; best practice: ExternalSecrets Operator pulls from AWS Secrets Manager / HashiCorp Vault into K8s Secrets; IRSA (IAM Roles for Service Accounts) for pod-level AWS access without access keys; Vault agent sidecar for dynamic short-lived credentials; Sealed Secrets for GitOps (encrypted in Git, decrypted in cluster); encrypt etcd at rest; never log or expose secrets in env-var dumps*

**528. Walk through a CI/CD pipeline you've built (tools: Jenkins, GitLab CI, GitHub Actions).**
   🔑 *Same structure as 516/526: trigger on push/PR → build → test gates (unit, integration, sonar) → package (Docker image) → publish to registry → deploy (Helm/ArgoCD) → verify (smoke tests) → monitor; key decisions: parallel stages for speed, fail-fast, separate staging and prod pipelines with approval gate, rollback automation on error spike*

**529. Explain Docker basics: image vs container, Dockerfile layers.**
   🔑 *Image: layered filesystem (each RUN/COPY/ADD creates a layer, cached); container: running image instance with writable layer; Dockerfile: FROM (base), RUN (execute command → new layer), COPY (add files), EXPOSE (document port), ENV, ARG, CMD/ENTRYPOINT; .dockerignore to exclude build context; layers cached → put infrequently changing steps first (dependencies before source code)*

**530. What is Kubernetes and how does it help with container orchestration?**
   🔑 *Kubernetes: container orchestration platform; automatically schedules containers on nodes, restarts failed pods (self-healing), rolling updates with zero downtime, horizontal autoscaling (HPA), service discovery + load balancing via Services, config/secret management, storage orchestration; control plane: API server, etcd (state store), scheduler, controller manager; worker nodes: kubelet + container runtime + kube-proxy*

**531. How do you handle blue-green or canary deployments?**
   🔑 *Blue-green: deploy green (new) alongside blue (current); switch traffic at LB/Ingress; rollback = flip back; Kubernetes: two Deployments with same Service, change selector; canary: Argo Rollouts or Istio traffic splitting (weight-based); Ingress annotations; gradually increase canary weight; automated analysis (success rate, latency) triggers rollback; feature flags as software-level canary*

**532. Explain Infrastructure as Code (Terraform, Ansible).**
   🔑 *IaC: infrastructure defined in version-controlled code; Terraform: declarative HCL, plan→apply, state file (S3 + DynamoDB locking), modules for reuse, providers for AWS/GCP/Azure, drift detection; Ansible: imperative YAML playbooks, idempotent, SSH-based agent-less, good for config management; Terraform provisions infra, Ansible configures it; Pulumi/CDK as programmatic alternatives*

**533. Deployment and AWS deep, Jenkins, script, pipeline, policies,**
   🔑 *Jenkins declarative pipeline: agent, stages, steps, credentials binding, shared library; AWS deployment: CodeDeploy (EC2/Lambda), ECS service update, K8s (kubectl/Helm), Lambda update-function-code; IAM policies: least-privilege, role per service; scripts: Groovy (Jenkinsfile), bash for AWS CLI; deploy gate: approval step, smoke test, auto-rollback on CloudWatch alarm*

**534. Clouds skill and rule**
   🔑 *Core AWS services: EC2, ECS/EKS, Lambda, S3, RDS, ElastiCache (Redis), SQS/SNS, API Gateway, CloudFront (CDN), Route53 (DNS), VPC, IAM, CloudWatch, Secrets Manager; rules: least-privilege IAM, VPC with private subnets for DBs, security groups restrict by port/source, encryption at rest (KMS) and in transit (TLS), multi-AZ for HA, autoscaling for elasticity, cost monitoring (Cost Explorer/Budgets)*

**535. What is AWS EC2 instance and how to deploy jar in it?**
   🔑 *EC2: virtual machine in AWS cloud (instance type = CPU/memory profile, AMI = OS image); deploy jar: (1) SSH to instance (key pair), (2) scp jar, (3) java -jar app.jar &; better: systemd service for auto-restart; or CodeDeploy agent with appspec.yml; or containerize → push to ECR → deploy via ECS/EKS; configure security groups to allow port 8080; Elastic IP for stable address*

**536. Ham charts in kubrnatis**
   🔑 *(Helm charts in Kubernetes) Helm: package manager for K8s; Chart: Chart.yaml (metadata), values.yaml (configurable defaults), templates/ (K8s YAML with Go templating {{.Values.image.tag}}); helm install/upgrade/rollback; release = named installation; helm repo for chart distribution; environment-specific: --values prod-values.yaml; simplifies complex multi-resource deployments*

**537. How Trigger and develop lambda**
   🔑 *Triggers: API Gateway (HTTP/REST), SQS (message processing), S3 (object events), SNS (notifications), EventBridge (scheduled/event-driven), Kinesis (stream), DynamoDB Streams; develop: handler class implements RequestHandler<Input, Output>; Lambda context for timeout/logging; deploy: zip → S3 → update-function-code; SAM (sam local invoke) for local testing; CDK/Terraform for IaC; IAM execution role; environment variables for config; provisioned concurrency for cold start*

**538. What is terraform and how you used it**
   🔑 *Terraform: declarative IaC tool; HCL defines resources (aws_ecs_service, aws_rds_cluster, aws_security_group); workflow: init (download providers) → plan (preview diff) → apply (create/update) → destroy; state stored in S3 + DynamoDB for team locking; modules for reuse (VPC module, ECS module); workspaces for dev/staging/prod; used to provision: VPC, subnets, security groups, RDS, ECS cluster, ALB, IAM roles*

**539. Basic command for terraform**
   🔑 *terraform init (download providers + modules); terraform plan (dry run, show changes); terraform apply (create/update resources, prompts confirmation); terraform destroy (tear down); terraform fmt (format HCL); terraform validate (syntax/type check); terraform state list/show/rm (manage state); terraform import (import existing resource); terraform output; terraform workspace new/select*

**540. How to increate intense in kubernatis, by script and y CLI**
   🔑 *(Increase instances/replicas in Kubernetes) CLI: kubectl scale deployment myapp --replicas=5; edit: kubectl edit deployment myapp → change spec.replicas; YAML: update replicas field + kubectl apply -f deployment.yaml; HPA (auto): kubectl autoscale deployment myapp --min=2 --max=10 --cpu-percent=70; KEDA for event-driven scaling; script: kubectl patch deployment myapp -p '{"spec":{"replicas":5}}'*

**541. How you used kubernatis in current project**
   🔑 *Deployed microservices as K8s Deployments; ConfigMaps for non-secret config (application.yml); Secrets via ExternalSecrets Operator (pulling from AWS Secrets Manager); Services (ClusterIP) for internal communication; Ingress + ALB for external traffic with TLS termination; HPA for autoscaling on CPU; resource requests/limits on all pods; PodDisruptionBudget for availability during node drain; Helm charts for packaging; ArgoCD for GitOps*

**542. CICD tools we used in our project**
   🔑 *Jenkins/GitLab CI/GitHub Actions (pipeline orchestration); Maven/Gradle (build); SonarQube + JaCoCo (quality gate + coverage); JFrog Artifactory / AWS ECR (artifact + image registry); Docker (containerization); Helm + ArgoCD/Flux (K8s deployment/GitOps); Testcontainers (integration tests); Terraform (IaC); PagerDuty (alerting); Slack (notifications)*

**543. Lambda service, how server less, triggering , EC2, SQS, SNS, how to deploy 2-3 ways,**
   🔑 *Serverless: no server management, auto-scale to 0, pay-per-invocation; triggers: SQS (event source mapping, batch), SNS (push notification), API Gateway (HTTP), S3 events, EventBridge (scheduled); vs EC2: EC2 is persistent VM you manage; deploy: (1) ZIP upload via console/CLI, (2) container image from ECR, (3) SAM (Serverless Application Model) / CDK / Terraform for IaC; cold start: mitigate with provisioned concurrency*

**544. Blue-green deployments**
   🔑 *Two identical environments (blue=current, green=new); deploy new version to green; run smoke tests on green; switch LB/DNS to green (instant cutover); rollback = flip back to blue (instant); zero downtime; DB migrations must be backward-compatible (old code still running against same DB during cutover); requires double infrastructure; simple rollback is key advantage over rolling updates*

**545. Kubernetes and Jenkins for deployment**
   🔑 *Jenkins builds and pushes image; Jenkinsfile stage: sh "helm upgrade --install myapp ./chart --set image.tag=${env.GIT_SHA} --namespace prod"; or kubectl apply -f deployment.yaml with image tag substituted; Jenkins credentials for kubeconfig; ArgoCD as alternative (GitOps: Jenkins pushes image tag update to Git, ArgoCD detects and syncs to cluster); Jenkins agent pods run inside K8s cluster*

**546. Virtual machine vs Docker**
   🔑 *VM: full OS + kernel, hypervisor (Type 1: VMware/Hyper-V, Type 2: VirtualBox), isolated kernel, heavy (GBs image, minutes to start), strong isolation; Docker container: shares host Linux kernel, namespaces for isolation (PID/net/mount/UTS), cgroups for resource limits, lightweight (MBs, milliseconds to start), portable; VMs better for different OS or strict kernel isolation; containers better for microservices density and speed*

**547. Why is Docker used?**
   🔑 *Consistent environment (same image dev=staging=prod, eliminates "works on my machine"); lightweight vs VMs (shared kernel, MBs not GBs); fast startup (seconds); layered images with build cache; easy distribution via registry (ECR/DockerHub); environment isolation (separate containers, no dependency conflicts); reproducible builds; natural fit for microservices packaging and deployment*

**548. How does Docker help run a container on all types of machines — how does it work?**
   🔑 *Docker uses Linux kernel features: namespaces (process isolation: PID, network, mount, UTS, IPC) + cgroups (resource limits: CPU, memory, I/O); container runtime (containerd → runc) creates isolated process; image layers via UnionFS (overlayfs) form container filesystem; Docker Desktop on Mac/Windows runs a lightweight Linux VM; same image runs identically on any host with Docker daemon (same kernel syscall interface)*

**549. How do you manage all containers of multiple services, and the commands?**
   🔑 *Local dev: Docker Compose (docker-compose.yml defines services, networks, volumes); commands: docker-compose up -d, down, ps, logs, exec; production: Kubernetes (kubectl get pods/services/deployments, describe, logs, exec, apply, delete, scale); docker ps (list containers), docker logs, docker exec -it, docker stop/rm, docker images, docker pull/push; K8s replaces Compose in production*

**550. What is Kubernetes and what are its commands?**
   🔑 *K8s: container orchestration; auto-scheduling, self-healing, rolling updates, autoscaling, service discovery; core commands: kubectl get pods/services/deployments/nodes; kubectl describe pod <name>; kubectl logs <pod> -f; kubectl exec -it <pod> -- bash; kubectl apply -f manifest.yaml; kubectl delete pod <name>; kubectl scale deployment <name> --replicas=3; kubectl rollout status/undo deployment/<name>; kubectl port-forward, kubectl top nodes/pods*

**551. Where are Docker images stored — versioning and ECS**
   🔑 *Local: Docker daemon's local image store; remote registries: Docker Hub (public), AWS ECR (private, IAM-controlled, regional, integrated with ECS/EKS), JFrog Artifactory, GitHub Container Registry; versioning: semantic tags (myapp:1.2.3), git SHA (myapp:abc1234), latest (avoid in prod — mutable); ECR image lifecycle policy to clean old images; ECS task definition references ECR image URI; image scanning in ECR for vulnerabilities*

**552. Docker image versioning**
   🔑 *Tag with: semantic version (1.0.0), git commit SHA (most reproducible), branch name; avoid latest in production (mutable, no rollback guarantee); immutable tags (lock tag to digest); ECR immutable tags setting; promote image by tag across environments (dev:abc1234 → staging:abc1234 → prod:abc1234 = same image, no rebuild); image digest (sha256:...) for true immutability; lifecycle policy to retire old tags*

**553. How do you create a Docker image?**
   🔑 *Write Dockerfile: FROM eclipse-temurin:21-jre-alpine; WORKDIR /app; COPY target/app.jar app.jar; EXPOSE 8080; ENTRYPOINT ["java","-jar","app.jar"]; multi-stage: FROM maven:3.9-eclipse-temurin-21 AS builder → RUN mvn package → FROM eclipse-temurin:21-jre-alpine → COPY --from=builder target/app.jar; docker build -t myapp:1.0 .; docker push registry/myapp:1.0; .dockerignore excludes target/, .git*

**554. Docker volume**
   🔑 *Persistent storage that survives container restart/removal; types: named volumes (docker volume create mydata, managed by Docker, stored in /var/lib/docker/volumes), bind mounts (host path mapped to container path, -v /host/path:/container/path), tmpfs (in-memory, not persisted); docker volume ls/inspect/rm; K8s equivalent: PersistentVolume + PersistentVolumeClaim; use for DB data, logs, shared config*

**555. Kubernetes ConfigMap**
   🔑 *ConfigMap: stores non-sensitive config as key-value pairs; sources: literal (--from-literal), file (--from-file), YAML manifest; consumed as: env vars (envFrom: configMapRef), single env var (valueFrom: configMapKeyRef), or volume-mounted files (auto-updated in pod on change, with configmap-reload sidecar); separate config from image; update ConfigMap → rolling restart pods (or Reloader controller); use Secret for sensitive values*

**556. Helm charts in Kubernetes**
   🔑 *Helm: K8s package manager; Chart structure: Chart.yaml (name, version, appVersion), values.yaml (default config), templates/ (K8s YAML with {{.Values.image.tag}} Go templating), NOTES.txt; helm install myapp ./chart; helm upgrade myapp ./chart --set image.tag=1.2 --values prod-values.yaml; helm rollback myapp 1; helm list; Helm repo (Artifact Hub); simplifies parameterised multi-environment deployments*

**557. How do you increase instances in Kubernetes — by script and by CLI?**
   🔑 *CLI: kubectl scale deployment myapp --replicas=5; patch: kubectl patch deployment myapp -p '{"spec":{"replicas":5}}'; YAML: edit spec.replicas → kubectl apply; HPA (auto): kubectl autoscale deployment myapp --min=2 --max=10 --cpu-percent=70; KEDA: scales on custom metrics (Kafka lag, SQS queue depth); script: use kubectl in bash/Groovy pipeline stage*

**558. How have you used Kubernetes in your current project?**
   🔑 *Same as 541 — Deployments, ConfigMaps, ExternalSecrets, ClusterIP Services, Ingress (ALB), HPA, Helm charts, ArgoCD GitOps; also: namespace per team, resource requests/limits to prevent noisy-neighbour, PodDisruptionBudget during node upgrades, readiness/liveness probes for Spring Boot actuator endpoints, initContainers for DB migration job*

**559. Steps for CI/CD**
   🔑 *1. Code commit/PR; 2. CI trigger; 3. Build (mvn/gradle); 4. Unit tests; 5. Integration tests (Testcontainers); 6. Static analysis (SonarQube quality gate); 7. Docker image build (multi-stage); 8. Push to registry (ECR/Artifactory); 9. Deploy to dev (Helm/kubectl); 10. Smoke/integration tests; 11. Promote to staging; 12. Approval gate; 13. Deploy to prod (blue-green/canary); 14. Smoke test; 15. Monitor; 16. Rollback on failure*

**560. CI/CD tools used in your project**
   🔑 *Same as 542 — Jenkins/GitLab CI/GitHub Actions + Maven/Gradle + SonarQube + JaCoCo + ECR/Artifactory + Docker + Helm + ArgoCD + Terraform + PagerDuty*

**561. How do you follow different modules in the pipeline — explain the complete flow of your pipeline**
   🔑 *Source (Git webhook) → Build (Maven multi-module, parallel module builds) → Test (unit parallel, then integration Testcontainers) → Quality gate (SonarQube, SAST scan) → Package (Docker multi-stage, tag with git SHA) → Publish (push to ECR) → Deploy-dev (Helm upgrade, namespace=dev) → Verify (RestAssured smoke tests) → Promote-staging → Manual approval → Deploy-prod (canary via Argo Rollouts) → Monitor (Datadog alert)*

**562. Different deployment strategies — blue-green, canary, etc.**
   🔑 *Rolling (default K8s): replace pods incrementally (maxSurge/maxUnavailable), zero downtime but mixed versions briefly; blue-green: two envs, instant switch, easy rollback, double infra; canary: gradual traffic shift (1%→100%), catches issues early, requires traffic splitting (Istio/Argo Rollouts); recreate: all down then all up (downtime, simple); A/B: user-segment based (feature flags); shadow: duplicate traffic to new version for testing without user impact*

**563. Which AWS service can we use to deploy?**
   🔑 *ECS (Fargate): managed containers, no cluster management; EKS: Kubernetes on AWS; Elastic Beanstalk: PaaS (manages EC2/LB/autoscaling, less control); CodeDeploy: deploy to EC2 or Lambda; Lambda: serverless functions; App Runner: container-based PaaS (simplest); EC2 directly (least managed); CodePipeline orchestrates the pipeline; CodeBuild for build steps; all integrate with ECR for images*

**564. How can we use Docker for deployment?**
   🔑 *Build image via Dockerfile → tag with version → push to ECR/Artifactory → deploy: (1) EC2: docker pull + docker run; (2) ECS: task definition references image URI, ECS service rolling update; (3) EKS/K8s: Deployment manifest with image:tag, kubectl apply or Helm upgrade; (4) Lambda: container image (up to 10GB); benefits: immutable artifact, same image in all envs, easy rollback by reverting tag*

**565. How can we manage environments, secrets and load balancers?**
   🔑 *Environments: K8s namespaces (dev/staging/prod) + Helm values per environment; secrets: ExternalSecrets Operator from AWS Secrets Manager / Vault; LB: K8s Ingress + AWS ALB Ingress Controller; cert-manager for TLS; Route53 for DNS with weighted routing for canary; environment promotion: same image tag deployed to successive namespaces via GitOps*

**566. How do you manage common properties and secrets?**
   🔑 *Common properties: Spring Cloud Config Server (Git-backed, per-profile application.yml); or K8s ConfigMaps; secrets: AWS Secrets Manager + ExternalSecrets Operator (syncs to K8s Secret); HashiCorp Vault for dynamic credentials (DB passwords auto-rotated); 12-factor app: all config from environment; never hardcode; secret rotation without restart via Vault agent sidecar*

**567. How do you trigger and develop a Lambda?**
   🔑 *Same as 537 — triggers: API GW, SQS event source mapping, S3, SNS, EventBridge (cron), Kinesis; develop: implement RequestHandler<TEvent, TResult>; test locally with sam local invoke; deploy: sam deploy / CDK / Terraform; Lambda layers for shared dependencies; environment variables for config; IAM execution role for permissions; X-Ray for tracing; CloudWatch Logs auto-created*

**568. Lambda service — how is it serverless, triggering, EC2, SQS, SNS, and 2–3 ways to deploy**
   🔑 *Same as 543/537*

**569. What is Terraform and how have you used it?**
   🔑 *Same as 538*

**570. Basic commands for Terraform**
   🔑 *Same as 539*

**571. IaC — infrastructure as code**
   🔑 *Infrastructure defined as code (version-controlled, reviewed, tested); tools: Terraform (declarative, multi-cloud), AWS CDK (TypeScript/Python, generates CloudFormation), Pulumi (general-purpose languages), CloudFormation (AWS native); benefits: reproducible environments, drift detection, audit trail, enables dev=staging=prod parity, self-service infrastructure via PR; GitOps: infra changes via PR → apply in CI; immutable infrastructure pattern*

## Design Patterns & SOLID

**572. Explain all 5 SOLID principles with a real code example for each (not textbook definitions — actual violation → fix)**
   🔑 *S(SRP): God class with fetch+format+email → split into separate classes; O(OCP): switch-on-type for payment → interface + polymorphism, new type = new class; L(LSP): subclass throws exception parent doesn't → fix inheritance or use composition; I(ISP): fat interface forces empty implement → split into focused interfaces; D(DIP): new ConcreteService() → inject via constructor with interface, Spring @Autowired*

**573. How would you refactor a class that violates Single Responsibility?**
   🔑 *Identify multiple reasons to change (data access, business logic, formatting, sending notifications); extract each responsibility to its own class; e.g., OrderService that fetches, validates, discounts, saves, emails → split into OrderRepository, OrderValidator, DiscountCalculator, OrderEmailer; each class has one reason to change; improves testability and reuse*

**574. Give an example where you applied Dependency Inversion in a Spring Boot project**
   🔑 *Define interface: PaymentGateway { process(Payment); }; implementations: StripeGateway, PayPalGateway; service: @Autowired PaymentGateway gateway (Spring injects correct impl via @Qualifier or @Primary); test: inject mock PaymentGateway; high-level module (service) depends on abstraction, not on concrete Stripe SDK; makes it easy to switch payment provider without changing service logic*

**575. Coding: Design a PaymentProcessor system where you can add new payment types (UPI, Card, Wallet) without modifying existing code — apply Open/Closed Principle**
   🔑 *Interface PaymentProcessor { boolean process(Payment p); String type(); }; implementations: UpiProcessor, CardProcessor, WalletProcessor; registry: Map<String, PaymentProcessor> built from @Component list; PaymentService.process(payment): registry.get(payment.getType()).process(payment); adding new type = new @Component class implementing interface; no changes to PaymentService; Strategy pattern*

**576. Explain Factory, Singleton, Adapter, Composite, Observer, Strategy patterns with real examples.**
   🔑 *Factory: PaymentProcessorFactory creates correct processor by type, hides construction; Singleton: Spring beans are singleton by default (one instance per context); Adapter: converts incompatible interface (legacy XML API → modern JSON API via wrapper); Composite: tree structure (file system, org hierarchy, discount rules); Observer: ApplicationEvent + @EventListener for domain events; Strategy: interchangeable algorithm (sorting, discount calculation, payment method)*

**577. What is Inversion of Control / Dependency Injection? How does Spring implement it?**
   🔑 *IoC: framework controls object creation + lifecycle, not application code; DI: dependencies provided externally (constructor/setter/field injection); Spring BeanFactory/ApplicationContext: reads @Component/@Service/@Repository, creates BeanDefinitions, resolves dependency graph, instantiates and injects; @Autowired on constructor (preferred — immutable, testable); @Configuration + @Bean for explicit wiring; bean scope: singleton (default), prototype, request, session*

**578. When would you use Strategy vs Factory pattern in a microservice?**
   🔑 *Strategy: runtime behavior varies — same operation done differently per context (different discount rules per customer tier, different payment processors per method); behaviour is interchangeable at runtime; Factory: creation logic varies — you need the right object created based on type/config, but once created it's used the same way; often combined: Factory creates the right Strategy; both avoid if-else/switch-case on type*

**579. Desing pattern**
   🔑 *Creational: Factory (decouple creation), Abstract Factory (family of objects), Builder (step-by-step complex object), Singleton (one instance), Prototype (clone); Structural: Adapter (interface conversion), Decorator (add behavior), Proxy (access control/lazy), Facade (simplified interface), Composite (tree); Behavioral: Strategy (interchangeable algorithm), Observer (event/listener), Command (encapsulate request), Template Method (skeleton algorithm), Chain of Responsibility*

**580. Enum singleton**
   🔑 *enum MySingleton { INSTANCE; public void doWork() {...} }; JVM guarantees one instance per class loader; thread-safe without synchronization; serialization-safe (enum serialization mechanism prevents new instance); reflection-safe (cannot call private constructor via reflection unlike class-based singleton); preferred over double-checked locking or static holder pattern; accessed as MySingleton.INSTANCE.doWork()*

**581. Design patterns, how you used , implement, where you used , why we need**
   🔑 *Why: reuse proven solutions, improve communication (shared vocabulary), guide design decisions; used in projects: Singleton (Spring beans, config holders), Builder (complex DTOs/request objects), Strategy (payment/discount/notification channel selection), Observer (@EventListener for domain events), Factory (processor selection by type), Decorator (caching/logging wrappers via AOP Proxy), Template Method (AbstractBatchJob base class)*

**582. Solid principle, why, where, how**
   🔑 *Why: maintainable, testable, extensible, readable code; reduces coupling, increases cohesion; where: in class/interface design decisions, during code review; how: SRP → extract responsibilities (when class has >1 reason to change); OCP → program to interfaces, use polymorphism; LSP → ensure subclass substitutability, test with base type reference; ISP → split fat interfaces by client; DIP → constructor inject abstractions, no new ConcreteClass() in business logic*

**583. Design patterns — why used and why important? Which pattern solves which problem?**
   🔑 *Problems → patterns: one shared instance (Singleton); complex object creation (Builder); create without specifying class (Factory); incompatible interfaces (Adapter); add behavior at runtime (Decorator); protect/control access (Proxy); multiple notification recipients (Observer); interchangeable algorithms (Strategy); encapsulate request (Command); tree structure (Composite); simplified complex API (Facade); step-by-step algorithm (Template Method)*

**584. Why and which design pattern did you use?**
   🔑 *Real example answer structure: problem faced (e.g., adding new payment types required modifying existing code) → chose Open-Closed/Strategy → interface PaymentProcessor + per-type implementation + registry → result: adding new payment type = new class, zero modification to existing code + easy to unit test each processor in isolation; explain why this pattern over alternatives (vs if-else: not scalable; vs inheritance: not flexible)*

**585. How do you implement SOLID?**
   🔑 *SRP: extract class when you see multiple responsibilities (>1 section of tests); OCP: add interface when you anticipate variation, use Spring polymorphism; LSP: test subclasses via base type reference, avoid overriding to throw exceptions; ISP: when a class implements an interface but leaves methods empty/unsupported; DIP: always inject interfaces, use @Autowired on interface type; code review: flag direct instantiation of services, flag god classes, flag methods with >3–4 responsibilities*

## System Design & Design Scenarios

**586. Design a payment/transaction processing system that must guarantee exactly-once processing and handle retries safely.**
   🔑 *Idempotency key (client-generated UUID) → unique constraint in DB; on retry: return cached response; outbox pattern for reliable event publishing (write event + DB update in one transaction, async relay to Kafka); saga pattern for multi-service atomicity (orchestrated or choreographed); distributed lock for in-flight dedup; exactly-once in Kafka: transactional producer + idempotent consumer; monitoring: duplicate detection metrics*

**587. Design a rate limiter for an API gateway serving millions of requests/day.**
   🔑 *Algorithms: token bucket (bursty traffic, smooth), fixed window (simple but boundary spike), sliding window log (accurate, memory-heavy), sliding window counter (hybrid, practical); Redis INCR+EXPIRE for fixed window; ZADD+ZRANGEBYSCORE for sliding window log; Lua script for atomicity; limit by: API key, user ID, IP; 429 Too Many Requests + Retry-After header; API Gateway built-in (AWS WAF, Kong, Nginx); distributed rate limiting across nodes via Redis*

**588. Design a notification service that fans out to email/SMS/push with retry and dedup.**
   🔑 *Kafka topics per channel (email-topic, sms-topic, push-topic); consumer groups per channel; retry topics with exponential backoff (notification-retry-1m, notification-retry-10m); DLQ after max retries; dedup: unique notification_id checked before send (Redis SET NX / DB unique constraint); template engine for message rendering; delivery tracking table (status: PENDING/SENT/FAILED); unsubscribe management; priority queues for urgent notifications*

**589. Design an idempotent order/transaction API where the client may retry the same request due to a network timeout.**
   🔑 *Client generates UUID idempotency key, sends in header; server checks idempotency_keys table: if exists → return cached response (status + body); if not → process, insert key+response atomically (within transaction or with unique constraint); TTL on stored results; 409 Conflict if different body with same key; PUT/PATCH naturally idempotent (resource update); GET always idempotent; Stripe's idempotency-key header as reference implementation*

**590. Design a system for real-time fraud detection on transaction events (streaming, low latency, false-positive trade-offs).**
   🔑 *Kafka for transaction event stream; Flink/Spark Streaming for real-time processing (<100ms decision); feature store for entity profiles (velocity, pattern history); rule engine (Drools) + ML model ensemble; CQRS: write path triggers fraud check, read path for fraud analyst dashboard; false positive = friction/block for legitimate user; false negative = fraud passes; threshold tuning; async enrichment for non-latency-sensitive signals; alert pipeline for review queue*

**591. How would you design a distributed unique ID generator (Snowflake-style) for transaction IDs across multiple regions?**
   🔑 *Snowflake: 64-bit integer = sign bit (0) + timestamp ms (41 bits, ~69 years) + datacenter ID (5 bits) + machine ID (5 bits) + sequence (12 bits, 4096/ms per node); monotonically increasing → good for DB index locality; no coordination needed; clock skew issue → reject future timestamps, use max seen; alternatives: UUID v4 (random, no order, no index locality), ULID (128-bit, lexicographically sortable, URL-safe), DB sequence (bottleneck at scale)*

**592. Design a reconciliation system that compares two large datasets (e.g., internal ledger vs external settlement file) and flags mismatches.**
   🔑 *Batch job (nightly or T+1); load datasets into staging tables; sorted merge join on transaction ID + amount + date; hash comparison for large partitions; categorise mismatches: missing in source, missing in target, amount mismatch; persist to mismatch table; auto-reconcile obvious cases (timing difference); alert for manual review; idempotent job (safe to re-run); schedule during off-peak; handle eventual consistency window (T+1 settlement delay)*

**593. Design a payment transaction system with idempotency (very relevant to Mastercard) — how do you prevent double charging on retry?**
   🔑 *Same as 589/586 — idempotency key as primary dedup mechanism; unique constraint on idempotency_key column; distributed lock for concurrent in-flight requests with same key; transaction isolation (SERIALIZABLE for idempotency check+insert); return same response on retry; audit log all attempts; charge only once; test: simulate network timeout → client retries → assert single charge in DB*

**594. Design a rate limiter for an API Gateway**
   🔑 *Same as 587 — choose algorithm per use case; Redis-backed for distributed; per-key limits; 429 + Retry-After; also consider API Gateway built-in (AWS API GW throttling, Kong rate-limit plugin); sliding window counter is recommended balance of accuracy and performance*

**595. Design a notification service using Kafka + microservices**
   🔑 *Same as 588 — Kafka topics per channel; consumer per channel; retry + DLQ; dedup; delivery tracking; template engine; also: notification-service as separate microservice subscribing to domain events (OrderPlaced → send email); preference management for user opt-out; webhook for third-party delivery (Twilio for SMS, FCM for push)*

**596. How would you scale a service handling millions of transactions/day — caching, DB read replicas, async processing, partitioning**
   🔑 *Caching: Redis for hot account/product data (cache-aside, TTL); DB read replicas: route read queries to replica, writes to primary; async processing: Kafka for non-critical downstream (notifications, audit log, reporting); DB partitioning: partition transaction table by date or account_id range; CQRS: separate read model (denormalized) from write model; horizontal pod autoscaling; connection pooling (PgBouncer for Postgres); CDN for API responses where cacheable*

**597. How leetcode works? If you have to develop a coding platform what will be your approach**
   🔑 *Components: Problem service (store + serve questions), User service (auth, profile), Submission service (receive code), Execution service (sandboxed runner per language in Docker container with resource limits + timeout), Judge service (compare output with expected), Result service (store + query results); Kafka/SQS for submission queue; Redis sorted set for leaderboard; WebSocket for real-time result push; rate limiting per user; S3 for test cases*

**598. Getting diff event for orders, have some service to handle that order, after service we have to create a bill. How to design this system, one order can have multiple service and all service has UID., what we have 2 type of amount, like amount from service and calculate amount,**
   🔑 *Event-driven: OrderCreated event → fan-out to service handlers (each with UID); orchestrator saga tracks which services completed; bill created when all services complete (check completion via status table); two amounts: service_amount (from service) and calculated_amount (business rule applied); store both; discrepancy alert; idempotent service processing via service_uid unique constraint; DLQ for failed service handlers; saga compensate on failure*

**599. Design a DB for org, department, user, role, permissions, and exceptional permissions**
   🔑 *Tables: organizations (id, name); departments (id, org_id FK, name); users (id, dept_id FK, email); roles (id, org_id, name); user_roles (user_id FK, role_id FK, PK composite); permissions (id, resource, action); role_permissions (role_id FK, permission_id FK); exceptional_permissions (user_id FK, permission_id FK, granted BOOL, reason); RBAC model with exception overlay; query: user permissions = role_permissions UNION exceptional_permissions WHERE granted; index on user_id + permission_id; audit_log table for changes*

**600. We have a ride booking application with millions of riders — how can we find the nearest riders from a pickup location (with JPA and SQL)?**
   🔑 *PostGIS extension (Postgres): store rider location as GEOGRAPHY(POINT); ST_DWithin(rider.location, pickup_point, 5000) for radius search; ST_Distance for ordering; spatial index (GIST) on location column; Redis GEOADD/GEORADIUS for in-memory real-time location (riders update location every few seconds); JPA: use native query with ST_Distance; bounded search radius + rider availability filter; GeoHash for approximate bucketing at scale*

**601. We have a large-scale application that needs some real-time data in multiple regions — how can we manage it in a timely manner across regions?**
   🔑 *Options: CockroachDB/Spanner for globally distributed strong-consistency DB; Aurora Global Database (primary region + read replicas in other regions, <1s replication lag); Kafka MirrorMaker 2 for cross-region event replication; CDN + edge caching for read-heavy; active-active with conflict resolution (last-write-wins, CRDTs); eventual consistency for non-critical data; strong consistency (Raft/Paxos consensus) for financial data; regional failover via Route53 health checks + latency routing*

**602. UTC concept in a large-scale multi-region application — how do we use LocalTime, and how does LocalTime work for India and America at the same time?**
   🔑 *Always store timestamps in UTC in DB (TIMESTAMP WITH TIME ZONE / Instant); never LocalDateTime in DB (has no timezone info); Instant = UTC epoch; ZonedDateTime for timezone-aware operations; convert at presentation layer (ZonedDateTime.of(dbTimestamp, ZoneId.of("Asia/Kolkata"))); LocalTime is timezone-free (just "14:30") — ambiguous across regions; UTC avoids DST issues; Hibernate: @Column + @Temporal or use Instant type with UTC configuration*

**603. We have an offer table that stores promo codes — how many fields would you add, and how do we enable a code only for a particular day or the first week of every month? Is storing all promo codes in the DB a good approach, or is there an alternative?**
   🔑 *Fields: code (unique), type (PERCENT/FLAT/BOGO), discount_value, valid_from, valid_to, day_of_week_mask (bitmask 1-127), week_of_month (1-5 or bitmask), max_uses, current_uses, min_order_amount, user_id (null=all); store in DB: yes for auditable/finite codes; alternative for large scale: HMAC-signed codes (encode rules in token, validate on-the-fly without DB lookup); Redis cache for hot promo lookups; rule engine for complex validity; background job for expired code cleanup*

**604. How do you track a driver and a trip — should we store everything in the DB? For a large application that would be too many DB queries**
   🔑 *Active/real-time: Redis (GEOADD for driver location, TTL per update — only current location, very fast); persist to DB: batch-flush significant events (trip start/end, waypoints every N seconds) not every GPS ping; trip state machine (REQUESTED→MATCHED→ONGOING→COMPLETED→BILLED) in DB with event sourcing; WebSocket/SSE for real-time location push to client; history in DB for billing/audit; cold storage (S3) for raw GPS traces older than 90 days*

**605. If 2 people at the same cafe book a ride and both get the same rider on the first attempt, will it book the same rider for two trips — how do you handle it?**
   🔑 *Prevent double booking: distributed lock (Redis SETNX on rider_id for duration of matching + assignment transaction); or pessimistic DB lock (SELECT rider_id FROM riders WHERE id=? FOR UPDATE); atomic check-and-assign: check rider status=AVAILABLE and set to ASSIGNED in single UPDATE with WHERE status='AVAILABLE' (optimistic); first booking wins, second gets 0 rows updated → retry with next available rider; saga/orchestrator rolls back if booking fails after lock acquired*

## Kotlin vs Java

**606. Null safety in Kotlin (?, !!, ?:) vs Java's Optional/NPE handling**
   🔑 *Kotlin: ? makes type nullable (String?); !! asserts non-null (throws KotlinNullPointerException if null — avoid in prod); ?: Elvis operator provides default (name ?: "Unknown"); safe call ?. short-circuits to null if receiver is null; compile-time null safety, not runtime; Java Optional: explicit wrapper type, orElse/orElseThrow/map/flatMap/filter; Kotlin preferred — zero boilerplate, enforced at compile time*

**607. Data classes vs POJOs — what does Kotlin generate automatically?**
   🔑 *Data class: compiler generates equals() + hashCode() (by primary constructor properties), toString() ("User(id=1, name=Alice)"), copy() (immutable-style copy with override), componentN() functions (for destructuring); eliminates boilerplate; Java equivalent: records (Java 16+, immutable, also compiler-generated); difference: Kotlin data class allows var (mutable) fields; records are always immutable; Lombok @Data as Java alternative before records*

**608. val vs var, coroutines vs Java threads (basic difference)**
   🔑 *val: immutable reference (like Java final, value can be mutable object but reference is fixed); var: mutable reference; Kotlin coroutines: lightweight (millions can coexist), suspend functions (pause without blocking OS thread), structured concurrency (scope controls lifetime), cooperative cancellation, run on thread pool; Java threads: 1:1 OS thread mapping, expensive (1MB stack), context switch overhead; Java 21 virtual threads: lightweight like coroutines but transparent (no async/await syntax needed)*

**609. Extension functions — what problem do they solve?**
   🔑 *Add methods to existing classes without inheritance or modifying source code; e.g., fun String.toTitleCase(): String = ...; fun List<T>.second(): T = this[1]; call as instance method: "hello world".toTitleCase(); compiled to static method with receiver as first param (no runtime overhead); useful for utility functions on third-party/stdlib classes; no access to private members; Kotlin stdlib uses extensively (e.g., .filter{}, .map{}, .let{})*

**610. Coding: Write a function in Kotlin using ?.let{} to safely chain nullable calls**
   🔑 *val city = user?.address?.let { addr -> addr.city ?: "Unknown" }; let{} executes block only if receiver is non-null, block receives it as 'it'; also run{}, apply{}, also{}; equivalent Java: Optional.ofNullable(user).map(User::getAddress).map(Address::getCity).orElse("Unknown"); Kotlin is more concise and readable; chain: user?.let { fetchProfile(it) }?.let { sendEmail(it) } — stops at first null*

## Version Control, SDLC & Documentation

**611. Explain Gitflow branching strategy.**
   🔑 *main (production), develop (integration branch), feature/* (branch from develop, merge back via PR), release/* (branch from develop for release prep, merge to main + develop, tag), hotfix/* (branch from main for emergency fix, merge to main + develop); long-lived: main + develop; short-lived: feature/release/hotfix; alternatives: trunk-based development (short-lived branches, feature flags, CI-friendly, preferred for fast delivery) + GitHub Flow (main + feature branches only)*

**612. What's your peer review process — what do you look for in a PR?**
   🔑 *Check: correctness (does it solve the stated problem), tests (edge cases covered, not just happy path), security (input validation, no hardcoded secrets, SQL injection risk), performance (N+1, inefficient algorithm, missing index hint), breaking changes (API contract, DB migration safety), naming + readability, SOLID violations (god class, direct instantiation), business logic gaps; constructive comments; approve with nits; request changes only for real issues; mob review for complex changes*

**613. Compare Waterfall, Scrum, Kanban, SAFe — when would each be used?**
   🔑 *Waterfall: sequential phases (requirements→design→build→test→deploy), fixed scope, best for stable requirements + regulatory compliance; Scrum: sprints (2 weeks), backlog, ceremonies (planning/standup/retro), velocity tracking, suits evolving requirements + cross-functional teams; Kanban: continuous flow, WIP limits, visualise work, suits ops/support/maintenance; SAFe (Scaled Agile Framework): enterprise-scale, Program Increments (PIs), ARTs, suitable for large multi-team programmes*

**614. How do you estimate effort/story points?**
   🔑 *Story points: relative complexity + effort + uncertainty (not hours); Fibonacci scale (1,2,3,5,8,13,21) to force explicit choices; planning poker for consensus; reference story as baseline; T-shirt sizing for epics (S/M/L/XL); break stories >8 points before sprint; account for: unknowns, tech debt, dependencies, testing effort, PR review time; track velocity (avg points/sprint) for capacity planning; re-estimate if scope discovered to be larger mid-sprint*

**615. How do you document APIs (Swagger/OpenAPI)?**
   🔑 *springdoc-openapi: auto-generates OpenAPI 3.0 spec from code; annotations: @Operation(summary,description), @ApiResponse(responseCode,description,content), @Schema on DTOs, @Parameter, @SecurityRequirement; serve at /v3/api-docs (JSON) + /swagger-ui.html; API-first approach: write spec first (OpenAPI YAML in Git) → generate stubs with OpenAPI Generator → implement; version spec alongside API versioning; publish to developer portal (Confluence/Stoplight)*

**616. What makes good technical documentation for services you own?**
   🔑 *README: quick start (clone→build→run), env vars, architecture overview, dependencies; API docs: OpenAPI spec auto-generated + human-readable notes for complex business rules; Architecture Decision Records (ADRs): record why, not what; runbook: operational procedures (deploy, rollback, restart, common incident steps); onboarding guide: setup + first contribution; diagrams: architecture (C4), data flow, sequence for complex flows; kept close to code (in repo), updated on change, reviewed in PR*

**617. Merge vs rebase — how do you resolve conflicts?**
   🔑 *Merge: creates merge commit, preserves history, safe on shared branches; rebase: replays commits on top of target, linear history, cleaner log, DON'T rebase shared/public branches (rewrites history); squash merge: collapse feature branch into single commit on main; conflict resolution: git merge/rebase → conflict markers → edit file → git add → git merge --continue / git rebase --continue; git rerere for recurring conflicts; git diff --merge for three-way view; interactive rebase (rebase -i) for commit cleanup*

**618. Challenges in migrating to a new Spring and Java version**
   🔑 *Spring Boot 2→3: javax.* → jakarta.* namespace change (requires find+replace everywhere); Spring Security config API completely changed (WebSecurityConfigurerAdapter removed); Hibernate 6 behavior changes (dialect, column naming); removed deprecated APIs; Java 11→17→21: module system (JPMS) restricts reflective access (Mockito/Spring may need --add-opens); new sealed classes/records may require pattern matching updates; dependency compatibility matrix (check all libraries); run full integration test suite; fix test failures first; GraalVM native compilation if used*

## Behavioral / Managerial

**619. Tell me about the most critical production incident you've owned — what broke, how did you find root cause, and what changed afterward (postmortem culture)?**
   🔑 *STAR: Situation (what broke, user impact, severity); Task (your role — incident lead, on-call); Action (triage → correlate with deploy → pull logs/traces → find root cause → hotfix/rollback → communicate status); Result (MTTR, users affected, recovery); postmortem: blameless, 5-whys root cause, action items (new test, alert, runbook, architectural fix), follow-through; show that you improved the system, not just fixed the symptom*

**620. Describe a time you had to make a trade-off between shipping fast and doing it "right" — how did you decide, and what was the outcome?**
   🔑 *Framework: articulate the technical debt explicitly (not hide it); get business alignment on risk; time-box the shortcut; create a ticket for the proper fix immediately; document the shortcut in code + ADR; when NOT to shortcut: security, data integrity, compliance, customer PII; outcome: shipped on time, tracked debt, paid it back in next sprint or iteration; shows pragmatism + responsibility*

**621. Tell me about a time your service caused a cascading failure in another team's system — what did you learn about blast-radius design?**
   🔑 *Lessons: circuit breakers prevent cascade (if your service degrades, callers fail fast instead of piling up); bulkheads isolate failure (separate thread pools per downstream); async decoupling (Kafka vs sync call) reduces blast radius; timeout budgets matter (your slow response cascades upstream); graceful degradation (return cached/default instead of error); backward-compatible changes; proactive blast-radius mapping (dependency graph); cross-team incident communication*

**622. Describe a schema/API change you had to roll out without breaking existing consumers — how did you sequence it?**
   🔑 *Schema (expand-contract): add nullable column → deploy code writing both → backfill → deploy code reading new → drop old; API breaking change: add new versioned endpoint /v2/...; run v1 + v2 in parallel; notify consumers with deprecation timeline; consumer-driven contract tests (Pact) to verify v1 still works; feature flag to enable v2 for opted-in consumers; sunset v1 after all consumers migrate*

**623. Tell me about a time you disagreed with an architectural decision from a senior engineer/architect — how did you handle it?**
   🔑 *Approach: gather evidence first (benchmark, PoC, reference architectures); present alternative with clear trade-offs (not just "I think"); bring it to design review with data; disagree and commit if overruled and it's not a safety/compliance issue; escalate if it has serious security or compliance implications; document dissent in ADR ("we considered X, decided Y because Z, team member A raised concern B"); follow up with data post-implementation to inform future decisions*

**624. Describe a performance problem you diagnosed that turned out to be something non-obvious (not the code you first suspected).**
   🔑 *Non-obvious causes: GC stop-the-world pauses causing latency spikes (not slow code); DNS resolution timeout for external service calls; connection pool exhaustion (app waiting for connection, not query running slow); JVM JIT warmup (cold start shows different perf than steady state); serialization overhead; thread contention on shared lock; infrastructure throttling (cloud burst limit); diagnosis: flame graph + thread dump + GC log + distributed trace — eliminate suspects systematically*

**625. Tell me about mentoring a junior engineer or leading a small team through a delivery — how did you balance hands-on coding vs unblocking others?**
   🔑 *Balance: daily check-ins (5 min) to catch blockers early; pair programming for complex/unfamiliar areas; code review as teaching opportunity (explain why, not just what); let junior struggle productively for a bounded time before stepping in; delegate with safety net (review before merge); unblocking > coding (1 multiplied by 4 > 4 multiplied by 1); track team velocity, not individual output; celebrate team success; adjust based on individual confidence level*

**626. Describe a time you had to push back on unrealistic timelines from product/business — how did you frame the conversation?**
   🔑 *Frame with data: velocity-based estimate (story points + team velocity), risk of rushing (quality, security debt, rework cost), options (MVP scope, phased delivery, parallel tracks); present trade-offs not just problems; offer solutions: (1) reduce scope to must-haves, (2) add resource, (3) accept risk with explicit sign-off; escalate with facts not emotions; document agreed scope in writing; retrospective if timeline was still missed to improve estimates*

**627. Walk me through how you'd approach debugging a bug that only reproduces in production and not in staging.**
   🔑 *Steps: (1) add more structured logging + tracing around the suspect area and deploy; (2) enable debug log level via actuator /loggers for specific package; (3) compare prod vs staging config (env vars, feature flags, pool sizes, timeouts); (4) check if data-dependent (prod has real data, staging may not); (5) reproduce with production data anonymised in staging; (6) shadow traffic to staging; (7) check if scale-dependent (load); (8) add metrics around suspect code path*

**628. Tell me about a design decision you made early in a project that you'd change if you started over — what did you learn?**
   🔑 *Common examples: chose wrong data model (denormalized too early, or over-normalized causing expensive joins); premature optimisation; monolith when modular would have been better; over-engineered abstraction for hypothetical use case; tight coupling between services; lesson: validate assumptions with PoC before committing architecture; involve team in design early (avoid ivory-tower decisions); YAGNI principle; document why not just what in ADRs*

**629. What are your 3 weak and 3 strong points?**
   🔑 *Strengths: deep Java/Spring/microservices expertise, systematic debugging and root cause analysis, distributed systems design with focus on reliability; weaknesses (with growth story): frontend/UI tech (actively learning React), can invest too much time in code quality optimisation before shipping (now use time-boxing), sometimes assume colleagues have same context (now focus on clearer written communication); keep weaknesses honest but show self-awareness + improvement action*

**630. How will you explain microservices to an 8-year-old kid?**
   🔑 *Instead of one big Lego castle that does everything, you have many small Lego pieces — each piece does exactly one job (one piece handles orders, another handles payments, another sends emails); if one piece breaks, only that piece needs fixing, the rest still work; different kids can build different pieces at the same time (separate teams); pieces talk to each other by passing notes (API calls/messages); you can swap out a piece without rebuilding the whole castle (independent deployment)*

**631. Whole process for current project, for rogers, SNB and old projects**
   🔑 *Prepare a STAR-structured walkthrough per project: (1) what the system does, (2) your role + team size, (3) architecture (tech stack, key design decisions, why those choices), (4) biggest challenge solved (technical or delivery), (5) production issue and how it was resolved, (6) what you'd improve; memorise key metrics (transactions/sec, uptime SLA, team size, timeline); tailor story to interviewer's domain (payments → Mastercard; telco → Rogers)*

**632. managerial**
   🔑 *Prepare STAR stories for: leadership (led delivery of feature X with 3 engineers), conflict resolution (differing technical views → facilitated discussion with data), delivery under pressure (missed milestone → re-scoped with PM), stakeholder communication (explained technical risk to non-technical audience), team building (hired/onboarded junior → productive in 2 weeks), technical mentorship (code review culture, pair programming), disagreeing with manager (presented data, committed to decision)*

**633. Whole process for the current project and previous projects**
   🔑 *Same as 631 — structured narrative per project: system purpose → your contribution → architecture → key decisions → challenges → outcomes; connect each project to skills relevant to the interviewer's domain; have 2-3 war stories ready (incident, performance fix, architectural decision) that you can reference across multiple behavioural questions*

## Coding Problems

**634. problem on number-to-words conversion.**
   🔑 *Divide into chunks: billions/millions/thousands/hundreds; word arrays: ones[]={"","one","two",...}, teens[]={"eleven","twelve",...}, tens[]={"","","twenty",...}; recursive helper converts chunk < 1000; handle edge cases: 0="zero", negative, exact millions/thousands; e.g. 1234 → "one thousand two hundred thirty four"; test: 0, 11, 100, 1000, 1000000, Integer.MAX_VALUE*

**635. Next palindrome in 100 to 1000, or next palidrome for 123 or 124 , ask for this is 131**
   🔑 *Brute force: iterate from n+1 upward; isPalindrome: s = Integer.toString(n); return s.equals(new StringBuilder(s).reverse().toString()); for 123 → 131; for 124 → 131; optimised: mirror first half (take first half, reverse it, compare; if less than second half increment first half); edge case: n=999 → 1001 (crosses digit boundary)*

**636. rotate list**
   🔑 *Three-reversal trick (O(n) time, O(1) space): k = k % n; reverse(0, n-1); reverse(0, k-1); reverse(k, n-1); or use Collections.rotate(list, k) for ArrayList; LinkedList: reconnect tail to head, advance to new tail; modulo handles k > n; handle k=0 (no-op); handle empty list; test: rotate [1,2,3,4,5] by 2 → [4,5,1,2,3]*

**637. shift all null or 0's in string list or int list**
   🔑 *Two-pointer: maintain write pointer (starts at 0); iterate read pointer; if element is non-null and non-zero, write to write-pointer position and advance; fill remaining positions with null/0; preserves relative order of non-zero elements; O(n) time O(1) space; Stream alternative: list.stream().filter(x -> x != null && x != 0).collect(toList()) then append zeros (creates new list); test: [1,0,2,null,3] → [1,2,3,0,null]*

**638. find second highest/lowest using steam**
   🔑 *Second highest: list.stream().filter(Objects::nonNull).distinct().sorted(Comparator.reverseOrder()).skip(1).findFirst().orElseThrow(); or .sorted(reverseOrder()).distinct() for de-dup before skip; IntStream: IntStream.of(arr).boxed().distinct().sorted(reverseOrder()).skip(1).findFirst(); O(n log n); O(n) approach: single pass tracking top1 and top2; handle all equal elements (no second distinct) → empty Optional*

**639. Find the mountain (start index, end index, width):**
   🔑 *Mountain: sequence that strictly increases then strictly decreases; find peak: index of max value (or binary search); expand left from peak while decreasing (start index); expand right from peak while decreasing (end index); width = end - start; handle edge cases: no mountain (always increasing or always decreasing → no valid start or end), single peak (width = 0 if no neighbours), multiple peaks (find widest or first); iterate all possible peaks for multiple mountains*

```
I/p: 1,2,1,2,3,4,5,3,2,4,6,8,9,10,9,8,7,10,12,14,15,16,17,18,19,10,5,2
Output:  2 8 6   ->(Start Index, End Index, Width of Mountain)
```

### Special Numbers in an Array

For **every** integer in **arr** perform the following operation:

* Take the sum of each digit until the sum is **less than 10** and this value is known as the **final sum.**
* Calculate the **factorial of the final sum.** Let this result be known as the **factorial-sum** of an integer.

For a given integer, **num**, if each of its digits is also present somewhere in the **factorial-sum** of **num**, **num** is a **special number**. This means that **num= 122** is a special number since it's **final sum=5** and it's **factorial sum=120** which contains both digits 1 and 2.

Find the **total number of special numbers** in **arr**.

**Notes:**
* The sum of digits of a single digit number is the number itself (e.g. **final sum** of 4 is 4)
* When determining whether a number is a **special number** or not, the order and position of digits is NOT important. The only condition is that **every digit in num** must be present somewhere in the factorial-sum of **num** for **num** to be considered a **special number**. For example, **122 is a special number** because (1+2+2)! = 5! = 120. By observation, the only digits in 122 are 1 and 2. These digits are present in 120, the factorial sum of 122.
* The factorial of 5 can be described as 5! = 5 × 4 × 3 × 2 × 1 = 120.
* Let an integer **num**=8795 then its **final sum** can be found as follows: 8795 → 8+7+9+5 = 29 → 2+9 = 11 → 1+1 = 2. Thus, the **final sum** of 8795 is 2.

### Predict the Output: Comparator + List\<Object\>

```java
List<Integer> ls = new ArrayList<Integer>();
ls.add(2);
ls.add(1);
ls.add(5);
ls.add(4);
ls.sort(new Comparator<Integer>() {
    @Override
    public int compare(Integer num1, Integer num2) {
        return num2 + num1;
    }
});
List<Object> lo = ls;
for (Object o : lo)
    System.out.println(o);
```

### Predict the Output: `class sample` with Optional

```java
class sample
{
    String[] str = new String[5];
    str[2] = "This is a sample code.";
    Optional<String> empty = Optional.empty();
    System.out.println(empty);
    Optional<String> value = Optional.of(str[2]);
    System.out.println(value);
}
```

### ReentrantLock / ExecutorService Non-Blocking Check

```java
ExecutorService excalibur = Executors.newFixedThreadPool(2);
Reentrant lock= new ReentrantLock();
excalibur.submit(() ->{ lock.lock();
try{ sleep(2);}finally{lock.unlock(); }
});
excalibur.submit(()->{ lock.lock()});
stop.excalibur(excalibur);
```

When doing so, you want to check for access to the lock without affecting the current thread execution. Which of the following alternatives can be utilized while doing so?

---

## Appendix — Context & Prep Notes

* *Prepared for a Senior Software Engineer (~10 YOE) interview loop*
* **Note on the interviewers:** LinkedIn blocks automated scraping of profile pages, so I couldn't pull full experience/skills sections for either profile. The one signal I could confirm: the nishit-shah-146707153 profile is titled "Senior Software Engineer at Mastercard." I couldn't verify anything for the kdpatel profile — no match was confirmable. Given the Mastercard signal, I've weighted the scenario section toward payments/fintech-style concerns (transaction integrity, idempotency, high throughput, security/compliance) — treat that as an educated guess, not a fact about what they'll ask.
* **How to use this:** For a 10-year-experience loop, interviewers rarely stay at "define X." Expect the pattern: define → probe internals → "why not the alternative?" → "tell me about a time you hit this in production." For every topic below, be ready to (1) explain it, (2) explain a failure mode/edge case, (3) tie it to a real project you shipped.
* **Suggested prep approach**
* For every "explain X" question, prepare a 60-second definition + a 2-minute real example from your own projects — that's the format senior interviewers actually reward.
* For the system design questions (#83–89), practice sketching on a whiteboard/paper: components, data flow, failure modes, and one explicit trade-off you're making.
* For the war-story questions (#90–99), pre-select 4–5 real incidents from your career and map each one to multiple questions above — you'll reuse the same stories across several answers.
* Given this stack, here's a full interview question set organized by category — theory questions interviewers typically ask, plus coding problems tied to the same skills.
