# Java / Spring Boot / Microservices — Complete Interview Question Bank

A **stack-wise consolidated compilation** of every question collected so far. All sources (the senior Java/Spring full question bank, the stack-based set, both loose question lists, and the screenshot coding questions) have been merged and regrouped by technology stack instead of by source. **Repeated questions have been collapsed into one** — where the same topic appeared multiple times, all their keywords are now combined on a single question, and the freed slots are used for new/advanced questions. **Abbreviations are given with their full forms** (e.g. REST, gRPC, SOAP, SAM, PECS). Questions only — **no full answers**. Each question is followed by a 🔑 *keyword line*: the minimum set of terms to hit so you can recall the structure of a good answer and explain it in your own words. Use the **⬆ Back to top** link at the end of each section to jump back to the Contents.

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
11. [Reactive Programming — WebFlux & Reactor](#reactive-programming--webflux--reactor)
12. [Microservices Architecture & Resilience](#microservices-architecture--resilience)
13. [Kafka, Messaging & Event-Driven Architecture](#kafka-messaging--event-driven-architecture)
14. [Security — AuthN/AuthZ, OWASP, TLS & Secrets](#security--authnauthz-owasp-tls--secrets)
15. [Performance, Caching & Scalability](#performance-caching--scalability)
16. [Production Troubleshooting, Logging & Observability](#production-troubleshooting-logging--observability)
17. [Testing & Code Quality](#testing--code-quality)
18. [Cloud, DevOps, Docker, Kubernetes & CI/CD](#cloud-devops-docker-kubernetes--cicd)
19. [Design Patterns & SOLID](#design-patterns--solid)
20. [System Design & Design Scenarios](#system-design--design-scenarios)
21. [Kotlin vs Java](#kotlin-vs-java)
22. [Version Control, SDLC & Documentation](#version-control-sdlc--documentation)
23. [Behavioral / Managerial](#behavioral--managerial)
24. [Quick Config & Setup Reference](#quick-config--setup-reference)
25. [Coding Problems](#coding-problems)

---

## Core Java & OOP Fundamentals

**1. How does String pooling work? Why is String immutable, what is the StringPool trick (literal vs `new String()`), and what's the performance implication of heavy `+` concatenation in loops?**
   🔑 *String pool in heap (String Constant Pool), literals interned vs `new String()` = separate heap object, `==` vs `equals`, `intern()`, compile-time constant folding; immutability → thread-safe + cached hashCode + security + safe map keys; `+` in loops = O(n²) → use StringBuilder; compact strings (Java 9)*

**2. Explain generics type erasure — why can't you do `new T[]` or `instanceof T`?**
   🔑 *Erasure at compile time, bytecode has Object/bound, no reified types, no `new T[]`, unchecked cast, bridge methods, `Class<T>` token workaround*

**3. Difference between polymorphism (compile-time vs runtime) with code.**
   🔑 *Compile-time = overloading/static binding, runtime = overriding/dynamic dispatch, vtable (virtual method table), reference type vs object type*

**4. Explain differences between abstract class and interface (Java 8+ default methods).**
   🔑 *Abstract = state + constructor + single inheritance; interface = contract + multiple inheritance + default/static/private methods; diamond resolution*

**5. Explain control statements and control flow in Java.**
   🔑 *Selection (if/else, switch + switch expressions Java 14+), iteration (for, enhanced-for, while, do-while), jump (break/continue/return, labeled break)*

**6. What is polymorphism? Explain the JVM's role in dynamic (runtime) polymorphism.**
   🔑 *Same reference, many forms; JVM `invokevirtual` + vtable/itable lookup, late binding, method table per class*

**7. Which object-oriented concepts support polymorphism and how?**
   🔑 *Inheritance + overriding + upcasting + interfaces + dynamic binding; overloading for compile-time*

**8. Instance block vs static block — what runs when, and what is allowed on a class vs an interface?**
   🔑 *Instance block runs before constructor body, per object, in declaration order; static block runs once at class load; class allows instance block + static block + constructor; interface allows only constants + default/static/private methods — no init blocks or constructor*

**9. What are the benefits of a marker interface?**
   🔑 *Empty/tag interface = metadata (Serializable, Cloneable, RandomAccess), checked via `instanceof`, JVM/library special handling, now largely replaced by annotations*

**10. Can a constructor be `final` (or `static`/`abstract`)?**
   🔑 *No — constructors are never inherited or overridden, so `final`/`abstract` are meaningless; not `static` either (runs on an instance); all are compile errors*

**11. What is `assert` and how do you use it?**
   🔑 *`assert expr : msg;` disabled by default, enable with `-ea`; for internal invariants, not for validating user input*

**12. What is Reflection in Java, and what does it cost?**
   🔑 *`Class.forName`, `getDeclaredMethods/Fields`, `setAccessible`, constructor `newInstance`; used by Spring/Jackson/JUnit; cost = slower + breaks encapsulation + no compile-time safety + module/`setAccessible` restrictions (Java 9+)*

**13. How can you print to the console without a `main` method?**
   🔑 *Static initializer block runs at class load (worked pre-Java 7); modern JVMs require `main` → demo with static block + `System.exit(0)` to avoid the "no main" error*

**14. What is a static block, and can we have more than one?**
   🔑 *`static{}` runs once at class loading (initialization phase), multiple allowed and run top-to-bottom, before `main`; used for constants / driver loading*

**15. How can a singleton be broken, and how do you prevent it (including cloning)?**
   🔑 *Broken by: reflection on private constructor, deserialization, `clone()`, multiple classloaders, race in lazy init; fixes: enum singleton, `readResolve()`, throw in constructor if instance exists, override `clone()` to throw, double-checked locking + `volatile`*

**16. Can we override or overload the `main` method?**
   🔑 *Overload yes (different params); override no — `main` is static → method hiding, JVM only invokes the `String[]` version*

**17. Does overloading only work for objects?**
   🔑 *No — overloading is compile-time on the declared type and applies to primitives too; resolution order = exact → widening → autoboxing → varargs*

**18. How does `new` work internally?**
   🔑 *Memory alloc in heap (TLAB — Thread-Local Allocation Buffer), default init to zero/null, super constructor chain, instance blocks, constructor body, reference assigned*

**19. What is the role of the constructor?**
   🔑 *Initialize state, enforce invariants, no return type, chained via `this()`/`super()`, implicit no-arg constructor if none declared*

**20. How is Java pass-by-value and not pass-by-reference?**
   🔑 *Everything is pass-by-value — for objects the copied value is the reference; reassigning the parameter doesn't affect the caller, mutating the object does*

**21. Can a singleton be serialized? What is serialization and how do you keep the singleton intact?**
   🔑 *Implement Serializable + `serialVersionUID`; deserialization creates a new object → break fix = `readResolve()` returning the single instance, or use an enum singleton*

**22. Why does an abstract class have a constructor if we never instantiate it? Can it be private?**
   🔑 *Constructor runs during subclass instantiation (super() chain) to init common state; a private constructor blocks all subclasses → compile error (unless all subtypes are nested)*

**23. What are the methods of the `Object` class?**
   🔑 *equals, hashCode, toString, getClass, clone, finalize, wait, notify, notifyAll*

**24. How do you design an immutable class — and name some built-in ones.**
   🔑 *`final` class + `private final` fields + no setters + init in constructor + defensive deep copy in constructor/getters + no `this` escape; String, wrapper classes, LocalDate, BigDecimal, UUID*

**25. How does the compiler force a class to implement all methods of an interface?**
   🔑 *Compiler checks the abstract contract; class must be `abstract` or implement all abstract methods — else compile error; interface methods are implicitly `public abstract`*

**26. How do overriding and overloading work internally? What is late (dynamic) binding?**
   🔑 *Overloading = static/early binding resolved by compile-time signature; overriding = dynamic/late binding via `invokevirtual` + vtable at runtime*

**27. Explain the Object → class → abstract class → interface hierarchy and parent-child relationships.**
   🔑 *`Object` is root of all classes; single class inheritance, multiple interface implementation; abstract class = partial impl; IS-A, upcasting, covariant returns, `super`, constructor chaining; interfaces don't implicitly extend Object*

**28. Why use the `@Override` annotation — is it mandatory?**
   🔑 *Not mandatory but recommended — compile-time safety against typos/signature mismatch, documents intent, flags an invalid override on static/private*

**29. What happens if you put a return type on a constructor?**
   🔑 *It becomes an ordinary method with the same name, silently never called as a constructor; related: instance init block runs before the real constructor body*

**30. What is `finalize()` in the Object class?**
   🔑 *Called once by GC before reclaim; deprecated (Java 9+), unpredictable, resurrection risk, delays collection → use try-with-resources / `Cleaner` / `PhantomReference`*

**31. Explain generics bounds: lower bound, upper bound and PECS.**
   🔑 *`? extends T` = upper bound (producer, read-only), `? super T` = lower bound (consumer, write), PECS (Producer-Extends-Consumer-Super), unbounded `?`, no instantiation of a wildcard type*

**32. Explain the `static` keyword.**
   🔑 *Class-level, loaded once, shared across instances; static method/field/block/nested class; no `this`, can't be overridden (hidden instead)*

**33. Explain access modifiers and their override rules.**
   🔑 *private < default (package-private) < protected < public; an override can widen but not narrow visibility; top-level class only public or package-private*

**34. What is the order of constructor, instance block and static block across parent and child?**
   🔑 *Static blocks once at class load (parent first) → then per object: super constructor → instance blocks in declaration order → constructor body (parent chain first)*

**35. How does a `switch` work internally?**
   🔑 *`tableswitch` (dense, O(1)) vs `lookupswitch` (sparse, binary search); String switch = hashCode + equals; enum = ordinal array; switch expressions use arrow + yield*

**36. Can we use `abstract` and `final` together on a class?**
   🔑 *No — abstract needs a subclass, final forbids subclassing; contradiction → compile error*

**37. Why do we need a singleton, and what is NOT a singleton?**
   🔑 *One instance per JVM for shared state / costly resource (config, cache, connection pool); not-singleton = prototype/new per request; Spring beans are singleton per container, not per JVM*

**38. What is a static instance with a private constructor?**
   🔑 *Static instance = class-level shared field created at class load; private constructor controls instantiation (eager singleton); static init order matters*

**39. Explain enum internals — can an enum have fields, a constructor, and abstract methods?**
   🔑 *Enum = final class extending `java.lang.Enum`, constants are `public static final` singletons; `values()`/`valueOf()`/`ordinal()`/`name()`; can have fields + private constructor + methods + constant-specific bodies (abstract method per constant); `EnumSet`/`EnumMap`; best thread-safe singleton*

**40. Explain nested classes — static nested vs inner vs local vs anonymous.**
   🔑 *Static nested = no outer instance; inner (non-static) holds implicit outer reference → can leak the outer object; local class inside a method; anonymous class = one-shot subclass/impl; captures effectively-final variables*

**41. How does varargs work and how does overload resolution handle it?**
   🔑 *`T... args` compiles to a `T[]`; varargs is the last parameter; chosen only after exact and widening/boxing matches (lowest priority); ambiguity + possible `null`/array-vs-element pitfalls*

**42. Explain try-with-resources and `AutoCloseable`.**
   🔑 *Resource declared in `try(...)` auto-closed in reverse order; resource must implement `AutoCloseable`/`Closeable`; suppressed exceptions (`getSuppressed()`); replaces finally boilerplate*

**43. What are the pitfalls of autoboxing/unboxing and the Integer cache?**
   🔑 *Autobox/unbox implicit conversion; `Integer` cache -128..127 → `==` true in range, false outside (use `equals`); `NullPointerException` unboxing a `null` Integer; boxing in loops = GC churn*

**44. How do annotations work — retention, target, meta-annotations, and custom annotations?**
   🔑 *`@Retention` (SOURCE/CLASS/RUNTIME), `@Target`, `@Inherited`, `@Documented`, `@Repeatable`; meta-annotations; read at runtime via reflection or at compile time via annotation processors (APT); drives Spring/JPA/validation*

**45. Deep copy vs shallow copy — and how do you copy an object correctly?**
   🔑 *Shallow = copies references (shared nested objects); deep = recursively copies nested state; `Object.clone()` is shallow + `Cloneable` contract pitfalls → prefer copy constructor, static factory, or serialization-based deep copy*

**46. `Serializable` vs `Externalizable`, `transient`, and `serialVersionUID`.**
   🔑 *Serializable = automatic via reflection; `transient`/`static` skipped; `serialVersionUID` controls version compatibility; `Externalizable` = manual `writeExternal`/`readExternal` (needs public no-arg ctor); custom `writeObject`/`readObject`/`readResolve` hooks; security risks of deserialization*

**47. Explain the `final` keyword in all its forms.**
   🔑 *final variable = assign-once (blank final set in constructor), final field + immutability + safe publication, final method = no override, final class = no subclass; "effectively final" required for lambda/anonymous capture*

**48. Explain `this` vs `super` and constructor chaining.**
   🔑 *`this` = current instance / `this()` calls another constructor; `super` = parent members / `super()` calls parent constructor (must be first statement); avoid `this` escaping during construction*

**49. POJO vs JavaBean vs DTO vs Entity vs record vs Value Object — how do they differ?**
   🔑 *POJO = plain class, no framework constraints; JavaBean = POJO with no-arg constructor + getters/setters + Serializable (tooling convention); DTO = data carrier across layers/boundaries (no behaviour, often a record); Entity = JPA-managed, has identity + lifecycle; Value Object (DDD) = immutable, equality by value not id (Money, Address); record = concise immutable data carrier (great for DTO/VO)*

**50. How do you use an enum as a state machine or strategy (constant-specific behaviour)?**
   🔑 *Each constant overrides an abstract method (constant-specific body) → polymorphic dispatch without if/switch; enum holds transition rules (`next()`), `EnumMap` for state→handler dispatch, `EnumSet` for allowed transitions; thread-safe singletons; models order/payment status machines cleanly*

**51. How do sealed interfaces + records model algebraic data types, and when do you prefer them over polymorphism?**
   🔑 *`sealed interface Shape permits Circle, Square` + record implementations → closed set the compiler knows → exhaustive switch with pattern matching + destructuring, no default; prefer for closed data-centric hierarchies / result types (visitor-style, external operations); prefer classic polymorphism when behaviour lives with the type and the set is open/extensible*

**52. How do you build and process a custom annotation (runtime vs compile-time)?**
   🔑 *Define with `@Retention` (RUNTIME for reflection, SOURCE/CLASS for processors) + `@Target` + meta-annotations + `@Repeatable`; runtime = read via reflection (`getAnnotation`) — how Spring/JPA/validation work; compile-time = annotation processor (APT, `AbstractProcessor`, generates code like Lombok/MapStruct/Dagger); bytecode weaving (AspectJ) as a third option*

**53. Explain advanced generics — recursive bounds, wildcard capture, and heap pollution.**
   🔑 *Recursive bound `<T extends Comparable<T>>` (self-referential, enables builders/enums); wildcard capture (compiler infers a capture type for `?` — helper method to "capture"); PECS; type inference + diamond; heap pollution from unchecked varargs generics → `@SafeVarargs`; can't create `new T[]` / generic arrays due to erasure*

**54. Object identity vs equality — ==, equals, and identity hash code.**
   🔑 *`==` compares references (identity) for objects, values for primitives; `equals` = logical equality (override with hashCode); `System.identityHashCode` = JVM identity hash regardless of override; interning/canonicalization (String pool, Integer cache) makes `==` accidentally work; always `equals` for value comparison*

**55. Explain the equals/hashCode/compareTo consistency contract — especially for JPA entities.**
   🔑 *`equals` true ⇒ equal hashCode; `compareTo` should be consistent with `equals` (else TreeSet/TreeMap "lose" elements); for JPA entities use a stable business/natural key or assigned UUID (NOT the DB-generated id — it's null before persist, breaks Set membership across the transient→managed transition); avoid Lombok `@Data`/`@EqualsAndHashCode` over all fields on entities*

**56. Explain Java I/O vs NIO (channels, buffers, selectors) and when each matters.**
   🔑 *Classic java.io = blocking streams (one thread per connection); NIO = channels + buffers + selectors (non-blocking multiplexed I/O, one thread many connections — Netty), memory-mapped files, `Files`/`Path` (NIO.2); use NIO/Netty for high-connection servers, `Files.readString`/try-with-resources for simple file work; zero-copy `transferTo`*

**57. What is the Java Platform Module System (JPMS) and why does it matter?**
   🔑 *Java 9 modules (`module-info.java`) = strong encapsulation (`exports`/`opens`/`requires`), reliable configuration, smaller runtimes (jlink); `opens` needed for reflection (Spring/Jackson/Mockito) → `--add-opens`; most apps still run on the classpath, but libraries and `jlink`/native images rely on it*

**58. How do dynamic proxies, reflection and MethodHandles power frameworks?**
   🔑 *JDK dynamic proxy (interface + InvocationHandler) + CGLIB (subclass) back Spring AOP/`@Transactional`; reflection drives DI/serialization/ORM; MethodHandles/VarHandle (java.lang.invoke) = faster, type-safe low-level access + `invokedynamic` (lambdas, string concat, pattern switch); cost = startup + no compile-time safety*

[⬆️ Back to top](#contents)

## JVM Internals, Memory Model & Garbage Collection

**59. Walk through the JVM memory areas — heap, stack, metaspace, PC register, native method stack, code cache and direct memory.**
   🔑 *Heap (young/old, shared) + stack per thread (frames, locals, operand stack) + metaspace (class metadata, native memory, replaced PermGen in Java 8) + PC (Program Counter) register per thread + native method stack + JIT code cache + off-heap direct memory; method area = constant pool + statics*

**60. How does garbage collection work, and compare the collectors — Serial, Parallel, G1, ZGC, Shenandoah, Epsilon. When would you pick each?**
   🔑 *Reachability from GC roots, mark-sweep-compact; Serial = single-thread small heaps; Parallel = throughput batch; G1 = region-based balanced default; ZGC/Shenandoah = sub-millisecond pauses on huge heaps (concurrent/colored pointers); Epsilon = no-op for benchmarks*

**61. What causes a memory leak in Java despite GC? Give real examples.**
   🔑 *Static/long-lived collections, unbounded cache (no eviction), unclosed streams/listeners, ThreadLocal not removed in pooled threads, ClassLoader leaks, inner class holding outer, mutated map key; diagnose with heap dump + dominator tree (MAT)*

**62. Explain ClassNotFoundException vs NoClassDefFoundError vs NoSuchMethodError — how do you debug a dependency conflict?**
   🔑 *ClassNotFoundException = reflective load miss; NoClassDefFoundError = present at compile, missing/failed-init at runtime; NoSuchMethodError = compile-vs-runtime version mismatch (jar hell); fix via `mvn dependency:tree`, exclusions, shading, `-verbose:class`*

**63. What is an island of isolation?**
   🔑 *Group of objects referencing each other but unreachable from any GC root → still collected (reachability, not reference counting, so no leak)*

**64. Explain the GC phases — marking, sweeping and compaction.**
   🔑 *Mark reachable from roots → sweep/free unreachable → compact (defragment, move objects + fix references); avoids fragmentation, costs pause time; concurrent marking in G1/ZGC*

**65. Explain generational GC — heap layout, the weak generational hypothesis, aging and promotion.**
   🔑 *Weak generational hypothesis ("die young"); Eden (TLAB bump-pointer alloc) → Survivor S0/S1 (copying, one always empty) → Old/Tenured; age counter per survived minor GC, `MaxTenuringThreshold` (default 15) or full survivor → promotion; humongous objects straight to Old; PermGen → Metaspace*

**66. Minor GC vs Major GC vs Full GC.**
   🔑 *Minor = young gen only, frequent + fast, stop-the-world; Major = old gen; Full = whole heap + metaspace, long pause; G1 mixed collections and concurrent cycles reduce full GCs*

**67. Which thread performs GC? What is a daemon thread, and is it only for GC?**
   🔑 *GC runs on internal daemon threads; daemon = background thread that doesn't block JVM exit — also timers, Spring schedulers, finalizer thread; `setDaemon(true)` before start*

**68. Why override `finalize`? Do records have `finalize`, and do records have methods?**
   🔑 *Rarely — legacy resource cleanup, deprecated/unpredictable → use Cleaner/try-with-resources; records don't need a finalize override; records DO have methods (accessors, equals/hashCode/toString, compact constructor, static + custom methods)*

**69. How does Just-In-Time (JIT) compilation work?**
   🔑 *Interpreter first → profiles hot methods → C1 (client) then C2 (server) tiered compilation → native code in code cache; On-Stack Replacement (OSR) for hot loops, inlining, escape analysis; deoptimization when assumptions break; JVM warmup effect*

**70. Explain the class loading lifecycle and the classloader hierarchy.**
   🔑 *Loading → Linking (Verify, Prepare = default-init statics, Resolve) → Initialization (static blocks + static field assignment, lazy on first use); Bootstrap → Platform → Application classloaders, parent-delegation model; custom classloaders (plugins, hot reload) break jar-hell*

**71. What are the types of OutOfMemoryError and how do you diagnose each?**
   🔑 *`Java heap space` (leak/undersized → heap dump), `GC overhead limit exceeded`, `Metaspace` (classloader leak / too many classes), `Direct buffer memory` (off-heap), `unable to create new native thread` (OS thread limit / stack size); `-XX:+HeapDumpOnOutOfMemoryError`*

**72. StackOverflowError vs OutOfMemoryError — what's the difference?**
   🔑 *StackOverflowError = per-thread stack exhausted (deep/infinite recursion), tune `-Xss`; OutOfMemoryError = heap/metaspace/native exhausted; different memory regions, different fixes*

**73. Explain reference types — strong, soft, weak and phantom — and their use cases.**
   🔑 *Strong = normal (never GC'd while reachable); Soft = cleared under memory pressure (memory-sensitive cache); Weak = cleared at next GC (WeakHashMap, canonical maps); Phantom = post-mortem cleanup via ReferenceQueue (Cleaner replaces finalize)*

**74. What are escape analysis, scalar replacement and lock elision?**
   🔑 *JIT proves an object doesn't escape the method/thread → stack allocation / scalar replacement (no heap alloc) and lock elision/coarsening (removes needless synchronization); reduces GC and contention*

**75. Which JVM flags do you use to tune heap and GC?**
   🔑 *`-Xms`/`-Xmx` (initial/max heap), `-Xss` (thread stack), `-XX:MaxMetaspaceSize`, GC select (`-XX:+UseG1GC`/`UseZGC`), GC logging (`-Xlog:gc*`), `-XX:+HeapDumpOnOutOfMemoryError`, `-XX:MaxRAMPercentage` (containers)*

**76. What is a safepoint and a stop-the-world (STW) pause?**
   🔑 *Safepoint = a point where all threads are paused at a known state so the JVM can run GC/deopt/biased-lock revocation; time-to-safepoint latency; STW = all app threads frozen; long safepoints hurt p99 latency*

**77. How do you optimize JVM/application startup time?**
   🔑 *Class Data Sharing (CDS/AppCDS), Ahead-Of-Time (AOT) compilation, GraalVM native image (near-instant startup, low memory), tiered compilation tuning, lazy bean init, fewer classpath scans; trade-off = warmup vs peak throughput*

**78. How does off-heap / direct memory work and how do you monitor native memory?**
   🔑 *`ByteBuffer.allocateDirect`/Netty for zero-copy I/O, lives outside heap (not bound by `-Xmx`), freed via Cleaner; `-XX:MaxDirectMemorySize`; Native Memory Tracking (NMT `-XX:NativeMemoryTracking`), compressed ordinary object pointers (oops) under 32 GB heap*

**79. How is the JVM container-aware, and how do you size heap in Kubernetes?**
   🔑 *Modern JVMs honour cgroup limits (`-XX:+UseContainerSupport` default) — detect container CPU/memory not host; set heap with `-XX:MaxRAMPercentage` (e.g. 75%) instead of fixed `-Xmx` so it scales with the pod limit; leave headroom for metaspace, thread stacks, direct/off-heap; align K8s memory limit with heap + non-heap or risk OOMKilled; CPU limits affect GC/JIT thread counts*

**80. How do you profile a production JVM with low overhead (JFR, async-profiler)?**
   🔑 *Java Flight Recorder (JFR) = built-in, ~1% overhead, continuous recording of allocation/GC/locks/IO/exceptions → analyze in JDK Mission Control; async-profiler = low-overhead CPU + allocation flame graphs (no safepoint bias); `jcmd JFR.start/dump`; use for CPU hotspots, allocation pressure, lock contention, without stopping the app*

**81. How do you choose and tune a garbage collector for a latency-sensitive service?**
   🔑 *G1 (default, balanced, `MaxGCPauseMillis` target, region sizing) for most; ZGC/Shenandoah for sub-millisecond pauses on large heaps (concurrent, colored pointers / Brooks pointers); Parallel for batch throughput; size the heap so old-gen churn is low, tune young-gen for allocation rate; measure with GC logs (`-Xlog:gc*`) + pause percentiles; avoid full GCs*

**82. How do you reduce a service's JVM memory footprint?**
   🔑 *Right-size heap (`MaxRAMPercentage`), cap metaspace, reduce thread count + stack size (`-Xss`), bound caches/pools, prefer primitives + compact data, watch off-heap/direct buffers (Netty), compressed oops under 32 GB; String deduplication (G1); GraalVM native image for drastically lower memory; measure with NMT (Native Memory Tracking)*

[⬆️ Back to top](#contents)

## Collections & Data Structures

**83. Explain the equals()/hashCode() contract — what breaks if you override one but not the other, and what happens if you mutate a key used in a HashMap?**
   🔑 *Equal objects must have equal hashCodes (reverse not required); break it → lost/duplicate entries in HashMap/HashSet; consistency; bucket lookup = hashCode then equals; mutating a key changes its bucket → entry unreachable (effective leak); Lombok `@EqualsAndHashCode`, records auto-generate*

**84. Compare HashMap, LinkedHashMap, Hashtable and ConcurrentHashMap internals — how does ConcurrentHashMap stay thread-safe without locking the whole map?**
   🔑 *HashMap = unordered, not thread-safe, 1 null key; LinkedHashMap = insertion/access order via doubly-linked list (LRU); Hashtable = legacy, fully synchronized, no nulls; ConcurrentHashMap Java 7 = segment locks, Java 8+ = CAS on empty bin + `synchronized` on head node + treeify, weakly-consistent iterators, no null keys/values*

**85. Difference between List, Set and Map — when would you use LinkedHashMap vs TreeMap?**
   🔑 *List = ordered + duplicates; Set = unique; Map = key-value; LinkedHashMap keeps insertion/access order (LRU cache); TreeMap = sorted by Comparable/Comparator, O(log n), navigation methods*

**86. Why is an array's size fixed (from the JVM's perspective), and how does ArrayList grow dynamically?**
   🔑 *Array length stored in object header at allocation, one contiguous fixed block, no realloc; ArrayList = `Object[]` + lazy default capacity 10, grows when size == capacity via `grow()` → newCap = old + old/2 (1.5×) + `Arrays.copyOf`; presize with `new ArrayList<>(n)` to avoid copies*

**87. If I add two logically-equal custom objects to a Set (e.g. `new User("a",12)` twice), will the Set allow both? How do you control uniqueness?**
   🔑 *Without overriding `equals`/`hashCode`, identity hash → both added; fix = override `equals`/`hashCode` (or use a record / Lombok `@Data`) or a `TreeSet` with a Comparator; key should be immutable so the hash never changes*

**88. Explain LinkedList, trees and graphs — common algorithms and implementations.**
   🔑 *LinkedList = doubly-linked nodes/pointers; trees = BST, AVL, red-black, trie, heap/PriorityQueue; graphs = adjacency list/matrix, BFS/DFS, Dijkstra, topological sort, cycle detection; recursion vs iteration*

**89. We can access a LinkedList by index, so why is an array (ArrayList) faster?**
   🔑 *Array = contiguous memory → O(1) random access + CPU cache locality; LinkedList index = O(n) traversal + pointer chasing + per-node object overhead; LinkedList wins only on middle insert/delete via an existing iterator*

**90. What is the load factor and initial capacity of a HashMap?**
   🔑 *Default load factor 0.75 = space/collision trade-off; resize (double capacity + rehash) when size > capacity × loadFactor; capacity always a power of two; set initial capacity to avoid rehashing*

**91. Can a HashMap cause a memory leak?**
   🔑 *Yes — mutable key (hashCode changes → unfindable), strong references in a static/unbounded map, no eviction; use WeakHashMap or a bounded cache (Caffeine) with TTL/size eviction*

**92. Comparator vs Comparable — and how do you chain comparators?**
   🔑 *Comparable = natural order, `compareTo`, inside the class; Comparator = external/multiple orders, `compare`, lambdas + `comparing`/`thenComparing`/`reversed`/`nullsFirst`; keep consistent with `equals`*

**93. Which collections have you used in your project, and for what purpose?**
   🔑 *ArrayList (read-heavy list), HashMap (lookup/cache), HashSet (dedup), LinkedHashMap (LRU/ordered response), ConcurrentHashMap (shared counters), TreeMap (sorted reports), BlockingQueue (producer-consumer), ArrayDeque (stack/queue)*

**94. Why does Java's HashMap convert a bucket to a red-black tree (treeify)?**
   🔑 *Many collisions in one bucket degrade lookup to O(n) list → treeify at 8 nodes AND capacity ≥ 64 giving O(log n), untreeify back to list at 6; mitigates hash-collision DoS (Denial of Service)*

**95. Why are Strings considered good keys for a map?**
   🔑 *Immutable → hashCode can't change; cached hashCode; well-distributed hash; final `equals`/`hashCode`; interning*

**96. Explain the internal working of ArrayList, LinkedList, HashSet and TreeMap.**
   🔑 *ArrayList = `Object[]` + 1.5× grow; LinkedList = doubly-linked nodes; HashSet = backed by a HashMap (dummy value); TreeMap = red-black tree (self-balancing BST); HashMap = array of bins + hash spread + treeify*

**97. Fail-fast vs fail-safe iterators — what is ConcurrentModificationException?**
   🔑 *Fail-fast (ArrayList/HashMap) track `modCount` → `ConcurrentModificationException` on structural change during iteration; fail-safe (CopyOnWriteArrayList, ConcurrentHashMap) iterate over a snapshot / weakly-consistent view, never throw but may miss recent writes*

**98. Iterator vs ListIterator vs Enumeration — how do you remove safely while iterating?**
   🔑 *Iterator = forward + `remove()`; ListIterator = bidirectional + `add`/`set`; Enumeration = legacy read-only; remove only via `iterator.remove()` or `removeIf` (not a for-each + `list.remove`) to avoid ConcurrentModificationException*

**99. ArrayList vs LinkedList vs Vector — when would you use each?**
   🔑 *ArrayList = default, random access, cache-friendly; LinkedList = frequent head/middle insert-delete via iterator, also a Deque; Vector = legacy synchronized (avoid, use `Collections.synchronizedList`/`CopyOnWriteArrayList`)*

**100. Explain Queue, Deque, Stack, PriorityQueue and BlockingQueue.**
   🔑 *Queue FIFO; Deque double-ended (`ArrayDeque` preferred over legacy `Stack` for LIFO); PriorityQueue = binary heap (min by default, O(log n) offer/poll); BlockingQueue (ArrayBlockingQueue/LinkedBlockingQueue) for producer-consumer + backpressure*

**101. Explain NavigableMap/NavigableSet and TreeMap navigation methods.**
   🔑 *`floor`/`ceiling`/`lower`/`higher`/`firstKey`/`lastKey`, `headMap`/`tailMap`/`subMap`, `descendingMap`; sorted range queries; backed by red-black tree, O(log n); ConcurrentSkipListMap for concurrent sorted*

**102. How do immutable and unmodifiable collections differ (List.of / copyOf vs Collections.unmodifiableList)?**
   🔑 *`List.of`/`Map.of`/`copyOf` (Java 9+) = truly immutable, reject nulls, throw `UnsupportedOperationException` on write; `Collections.unmodifiableList` = read-only VIEW over a mutable backing list (changes to backing still show); defensive copy for true immutability*

**103. Explain the specialized maps — WeakHashMap, IdentityHashMap and EnumMap.**
   🔑 *WeakHashMap = weak keys, auto-removed when key GC'd (caches/metadata); IdentityHashMap = `==` instead of `equals` (serialization graphs); EnumMap = array-backed, fast + ordered by enum ordinal*

**104. How does HashMap spread hashes and size its buckets?**
   🔑 *`hash = h ^ (h >>> 16)` perturbation to mix high bits; capacity power-of-two → index = `(n-1) & hash`; resize doubles capacity and re-distributes nodes (split lo/hi lists) to cut collisions*

**105. What are the pitfalls of `Arrays.asList` and array↔list conversions?**
   🔑 *`Arrays.asList` returns a fixed-size list backed by the array → `add`/`remove` throw `UnsupportedOperationException`, and primitive arrays become a single-element `List`; use `new ArrayList<>(Arrays.asList(...))` or `Arrays.stream().boxed().toList()`*

**106. What is the time/space complexity of common collection operations?**
   🔑 *ArrayList: get O(1), add-end amortized O(1), add/remove-middle O(n), contains O(n); LinkedList: get O(n), add/remove-ends O(1); HashMap: get/put O(1) avg, O(log n) treeified worst; TreeMap/TreeSet: O(log n) sorted; ArrayDeque: O(1) ends; PriorityQueue: O(log n) offer/poll, O(1) peek; know these to justify a choice*

**107. How do you choose the right collection for a use case?**
   🔑 *Need order + index → ArrayList; frequent ends insert/queue → ArrayDeque/LinkedList; uniqueness → HashSet (LinkedHashSet for order, TreeSet for sorted); key-value lookup → HashMap (LinkedHashMap LRU, TreeMap sorted, EnumMap for enums); concurrent → ConcurrentHashMap/CopyOnWriteArrayList/ConcurrentSkipListMap; priority → PriorityQueue; immutable → List.of/copyOf; decide by access pattern + ordering + concurrency + complexity*

**108. How do you make and safely publish an immutable or defensive collection?**
   🔑 *`List.of`/`Map.of`/`copyOf` (truly immutable, Java 9+); defensive copy in constructors/getters (`new ArrayList<>(src)` / `Collections.unmodifiableList(new ArrayList<>(src))`); don't expose the internal mutable collection; deep copy for nested mutables; safe publication via final fields / immutability for thread safety*

[⬆️ Back to top](#contents)

## Concurrency, Multithreading & Virtual Threads

**109. Explain the Java Memory Model (JMM) — happens-before, visibility, volatile vs synchronized, and final-field semantics.**
   🔑 *Happens-before edges (lock release→acquire, volatile write→read, thread start/join, final-field freeze); visibility vs atomicity; CPU caches + instruction reordering; volatile = visibility + ordering only; synchronized = visibility + mutual exclusion*

**110. Difference between Future, CompletableFuture and reactive (Mono/Flux) — when do you reach for which?**
   🔑 *Future = blocking `get()`, no chaining; CompletableFuture = non-blocking composition (thenApply/thenCompose/allOf), custom executor; Mono/Flux (Project Reactor) = lazy, backpressure, full reactive stack*

**111. Explain synchronized internals and how synchronization works — monitor locks, biased/lightweight/heavyweight locking.**
   🔑 *Object header mark word, `monitorenter`/`monitorexit`, reentrant; biased (removed 15+) → thin/lightweight CAS → inflated heavyweight (OS mutex, park/unpark); lock on `this` / Class object for static; release-acquire happens-before; lock elision/coarsening*

**112. How would you implement producer-consumer — BlockingQueue vs wait/notify vs Semaphore?**
   🔑 *BlockingQueue (ArrayBlockingQueue/LinkedBlockingQueue) = simplest, handles signalling + backpressure; wait/notify guarded by a `while` (spurious wakeups); Semaphore for permit counting; bounded queue = backpressure*

**113. What is a deadlock, and how do you detect/diagnose one (including a blocked thread) in production?**
   🔑 *Circular wait on 2+ locks, 4 Coffman conditions; thread dump (jstack/`kill -3`/`jcmd Thread.print`) → "Found one Java-level deadlock" + BLOCKED + "waiting to lock" + owner; `ThreadMXBean.findDeadlockedThreads` via actuator, JFR, APM; DB deadlocks (`SHOW ENGINE INNODB STATUS`)*

**114. How do you avoid or fix a deadlock? Give a real production example.**
   🔑 *Global lock ordering (e.g. order by account id), `tryLock` with timeout + backoff, reduce lock scope, single/coarse lock or lock-free structures, avoid nested locks & calling foreign code while holding one; banking example = two transfers A→B / B→A, batch job + online update on same rows, retry on deadlock error code*

**115. How do you size a thread pool for CPU-bound vs I/O-bound work, and how do you configure ExecutorService in production?**
   🔑 *CPU-bound ≈ cores+1; I/O-bound = cores × (1 + wait/service) or Little's law; bounded queue + named ThreadFactory + rejection policy (CallerRuns = backpressure) + timeouts + metrics (queue depth, active); separate pools per dependency (bulkhead); avoid `newCachedThreadPool` (unbounded → OutOfMemoryError)*

**116. ThreadLocal — uses, and the memory-leak risk in a pooled-thread environment.**
   🔑 *Per-thread state (user context, SimpleDateFormat, transaction); pooled thread never dies → ThreadLocalMap entry (weak key, strong value) keeps value + its classloader alive; always `remove()` in a `finally`; InheritableThreadLocal for child threads*

**117. Explain CountDownLatch vs CyclicBarrier vs Phaser vs Semaphore vs Exchanger.**
   🔑 *CountDownLatch = one-shot wait for N events (fan-in/startup); CyclicBarrier = reusable rendezvous of N threads (batch rounds); Phaser = dynamic parties + phases; Semaphore = permit/rate limiting; Exchanger = swap data between two threads*

**118. What is false sharing and how do you deal with it?**
   🔑 *Independent variables on the same 64-byte cache line → cross-core invalidation; fix with `@Contended`/padding, `LongAdder` (striped cells); seen in high-throughput counters*

**119. A service throws intermittent TimeoutException only at peak traffic — how do you tell thread starvation from GC pause, connection-pool exhaustion, or downstream latency?**
   🔑 *GC logs (pause/old-gen), thread dump (BLOCKED/WAITING counts), HikariCP metrics (pending threads, active vs max), downstream p99 + APM traces; correlate timing before blaming code*

**120. Explain multithreading basics — synchronized, volatile, ExecutorService.**
   🔑 *synchronized (mutual exclusion + happens-before), volatile (visibility, not atomicity), ExecutorService (pool + Future + shutdown), atomic classes, concurrent collections*

**121. A counter inside a synchronized method still produces wrong totals with multiple threads — what are the possible causes?**
   🔑 *Lock on different objects/instances, static vs instance lock, non-atomic read-modify-write outside the lock, field not volatile for other readers, multiple JVMs → use AtomicLong/LongAdder or a DB sequence*

**122. How does a Java thread pool work — what happens with no tasks and with too many tasks?**
   🔑 *Core threads → queue → extra threads up to max → rejection handler; idle non-core threads die after keepAlive; corePoolSize/maxPoolSize/keepAlive/BlockingQueue/ThreadFactory/RejectedExecutionHandler; unbounded queue = memory growth, bounded = rejection/backpressure*

**123. Before Java 8, how could you do parallel programming?**
   🔑 *Raw Thread/Runnable, ExecutorService + Future/invokeAll (Java 5), Fork/Join + RecursiveTask work-stealing (Java 7), CountDownLatch, manual partitioning*

**124. Why is Future blocking?**
   🔑 *Only exposes blocking `get()` / `isDone` polling — no callback or composition; CompletableFuture added completion callbacks and chaining*

**125. Explain CompletableFuture and how it works internally.**
   🔑 *`supplyAsync`/`runAsync`, `thenApply`/`thenCompose`/`thenCombine`, `allOf`/`anyOf`, `exceptionally`/`handle`, `orTimeout`, custom executor, `join` vs `get`; internally a completion stack of dependent actions fired non-blocking on completion (default `ForkJoinPool.commonPool`), exceptions wrapped as CompletionException*

**126. Explain notify() vs notifyAll() and the wait/notify protocol.**
   🔑 *`notify()` wakes one arbitrary waiter, `notifyAll()` wakes all; must hold the monitor (else IllegalMonitorStateException); `wait` in a `while` loop (spurious wakeups); prefer `notifyAll` or `Condition.await`/`signal` to avoid lost signals*

**127. When does volatile fail?**
   🔑 *Compound actions (`count++`), check-then-act, invariants across multiple fields, element writes to arrays/objects (only the reference is volatile); use atomics/locks instead*

**128. Why can't you use volatile on local variables, and what about static fields?**
   🔑 *Locals live on the thread's own stack → never shared, so visibility is moot; `static volatile` IS legal; what's illegal is `volatile` + `final` together*

**129. How do you make a class and a collection thread-safe?**
   🔑 *Immutability, thread confinement, synchronized/Locks, atomic classes, concurrent collections, `Collections.synchronizedXxx`, copy-on-write, defensive copies, no `this` escape*

**130. Normal thread creation vs the Executor framework.**
   🔑 *`new Thread` = manual lifecycle, no reuse/queueing, no result; ExecutorService = pooling, Future/CompletableFuture, scheduling, graceful shutdown, rejection policy*

**131. What are lock striping and object pooling, and how do they help?**
   🔑 *Lock striping = split one lock into N per-bucket locks (ConcurrentHashMap, LongAdder cells) → less contention; object pooling reuses expensive objects (connections, threads, buffers) → less allocation/GC*

**132. Synchronous vs asynchronous — trade-offs.**
   🔑 *Sync = caller blocks, simple, immediate consistency; async = non-blocking, callbacks/queues, higher throughput, harder error handling + ordering, eventual consistency*

**133. How does distributed locking work?**
   🔑 *Redis `SET NX PX` + unique token + Lua release (Redlock caveats), ZooKeeper ephemeral sequential node, DB row/advisory lock; TTL to avoid stuck locks, fencing token, idempotency as backup*

**134. Virtual thread limitations — how do you control them, and what happens if you create too many (no pool by design)?**
   🔑 *Not for CPU-bound work; pinning on `synchronized`/native frames (use ReentrantLock); unbounded creation → memory + downstream overload; not pooled by design (cheap, no reuse); control with Semaphore, structured concurrency, bounded queues, rate limiting*

**135. What happens if you create a separate virtual thread per request?**
   🔑 *Fine — thread-per-request becomes cheap (few KB heap stack); the real bottleneck shifts downstream (DB pool, external API) → must rate-limit / bulkhead*

**136. Can virtual threads overload a connection pool?**
   🔑 *Yes — millions of virtual threads can all demand connections while the pool stays small → queueing/timeouts; use a Semaphore/bulkhead sized to the pool*

**137. What is Project Loom?**
   🔑 *JEP 444 virtual threads + continuations + carrier threads + structured concurrency + scoped values; goal = simple blocking code with async scalability*

**138. Explain the CPU → OS thread → platform thread → virtual thread relationship.**
   🔑 *CPU core runs a kernel/OS thread; platform thread = 1:1 wrapper over an OS thread (~1 MB stack); virtual thread = JVM-scheduled, mounted on a carrier thread (ForkJoinPool) and unmounted when it blocks*

**139. What is a thread dump vs a heap dump, and how do you capture them in production?**
   🔑 *Thread dump = thread states/stacks (jstack, `kill -3`, `jcmd <pid> Thread.print`) for deadlock/blocking; heap dump = objects/memory (`jmap`, `jcmd GC.heap_dump`, `-XX:+HeapDumpOnOutOfMemoryError`, MAT); k8s = ephemeral container + `kubectl cp`; plus actuator/Micrometer, Prometheus+Grafana, JFR*

**140. How do the concurrent data structures work — ConcurrentHashMap, CopyOnWriteArrayList, ConcurrentSkipListMap?**
   🔑 *CHM = CAS + per-bin synchronized + treeify, weakly-consistent iterators; CopyOnWriteArrayList/Set = snapshot on write, great read-heavy; ConcurrentSkipListMap = sorted lock-free; BlockingQueues for producer-consumer*

**141. How does CAS work, how does volatile apply to objects, and how do Atomic classes work?**
   🔑 *CAS = compare-and-swap on a memory address (Unsafe/VarHandle, lock-free, ABA risk → AtomicStampedReference); volatile gives visibility of the reference, not the object's fields; AtomicInteger/AtomicReference use CAS retry loops; LongAdder uses striping under contention*

**142. Can two threads execute on the same object simultaneously with no locking? If yes, how?**
   🔑 *Yes — different methods, or non-synchronized methods, run concurrently on the same object; only threads contending for the same monitor are serialized; immutable/stateless objects are always safe*

**143. ReentrantLock vs synchronized — and when do you use ReadWriteLock / StampedLock?**
   🔑 *ReentrantLock = `tryLock`(timeout), interruptible, fairness, multiple `Condition`s, must `unlock()` in finally; ReadWriteLock = many readers / one writer; StampedLock = optimistic read (no lock, validate) for read-heavy; synchronized = simpler, JVM-optimized, works with virtual threads better (no pinning in recent JDKs)*

**144. Explain the Fork/Join framework and work-stealing.**
   🔑 *`ForkJoinPool` + `RecursiveTask`/`RecursiveAction`, divide-and-conquer `fork()`/`join()`, work-stealing deques balance load across workers; backs `parallelStream`; avoid blocking I/O inside it*

**145. What is structured concurrency and what are scoped values (Java 21)?**
   🔑 *`StructuredTaskScope` ties subtasks to a scope → all finish/cancel together, clear error propagation (ShutdownOnFailure/ShutdownOnSuccess), no leaked threads; ScopedValue = immutable, inheritable alternative to ThreadLocal for virtual threads*

**146. Explain ThreadPoolExecutor saturation, queue choice and rejection policies.**
   🔑 *core → queue → up to max → rejection; queue type matters: unbounded LinkedBlockingQueue (max never reached, OOM risk), bounded ArrayBlockingQueue (backpressure), SynchronousQueue (hand-off, grows threads); rejection policies = AbortPolicy (throws, default), CallerRunsPolicy (backpressure — caller executes), DiscardPolicy, DiscardOldestPolicy; size + queue + policy together define behaviour under load; separate pools per dependency (bulkhead)*

**147. What are the key CompletableFuture orchestration patterns?**
   🔑 *`supplyAsync` on a custom executor; `thenApply` (sync transform) vs `thenCompose` (flatten dependent future) vs `thenCombine` (join two); `allOf`/`anyOf` for fan-out; `orTimeout`/`completeOnTimeout`; `exceptionally`/`handle`/`whenComplete` for recovery; avoid blocking `get()` inside a stage; don't starve the common pool — supply your own executor*

**148. How do you propagate context (MDC, SecurityContext, trace id) across async tasks and virtual threads?**
   🔑 *ThreadLocal doesn't cross thread boundaries → use a TaskDecorator (Spring) to copy MDC/SecurityContext/trace into the worker thread, `DelegatingSecurityContextExecutor`, Micrometer context propagation; for virtual threads + structured concurrency prefer ScopedValue (immutable, inheritable) over ThreadLocal; always clear in a finally to avoid leakage in pooled threads*

**149. How do you design backpressure in a producer-consumer pipeline?**
   🔑 *Bounded BlockingQueue so a slow consumer blocks fast producers; or Reactive Streams `request(n)` demand signalling (Flux + buffer/onBackpressureBuffer/Drop/Latest); drop vs block vs buffer trade-off; Kafka lag as natural backpressure; size the queue with Little's law; monitor queue depth + shed load (reject/429) before OOM*

**150. What are the best practices and pitfalls with virtual threads?**
   🔑 *Use for I/O-bound thread-per-request, not CPU-bound; avoid `synchronized` around blocking calls (pinning — use ReentrantLock; largely fixed in recent JDKs); don't pool them (create per task); don't use them as a drop-in for CPU parallelism (use a fixed pool/ForkJoin); downstream (DB pool, external API) becomes the bottleneck → bound with a Semaphore; ScopedValue over ThreadLocal*

[⬆️ Back to top](#contents)

## Java 8+ — Functional, Streams, Records & Sealed Classes

**151. What's new and relevant from Java 11 → 17 → 21 that you've actually used?**
   🔑 *Java 11 LTS: `var`, new HttpClient, String/Files helpers; Java 17 LTS: records, sealed classes, text blocks, switch pattern matching (preview); Java 21 LTS: virtual threads, structured concurrency, record patterns, pattern matching for switch, sequenced collections; ZGC/G1 improvements*

**152. Explain functional interfaces and default/static/private interface methods.**
   🔑 *`@FunctionalInterface` = exactly one abstract method (SAM — Single Abstract Method), enables lambdas/method refs; default methods add behavior without breaking implementers; static + private interface methods for shared helpers; built-ins Function/Supplier/Consumer/Predicate*

**153. Why can a functional interface have only one abstract method, and can it extend another interface?**
   🔑 *A lambda must map to exactly one signature → one abstract method (Object's public methods + default/static don't count); it can extend another interface and stay functional only if the total abstract count stays 1*

**154. How do you use Optional correctly (and misuse it)?**
   🔑 *`of`/`ofNullable`/`empty`, `map`/`flatMap`/`filter`, `orElse`/`orElseGet`/`orElseThrow`, `ifPresent`; return type for "maybe absent", NOT for fields/params/collections; avoid `Optional.get()` and `isPresent()`+`get()`*

**155. Explain the Stream API — the pipeline, lazy intermediate vs terminal ops, and how it works internally.**
   🔑 *Source Spliterator → lazy intermediate ops build a stage pipeline → terminal op triggers a single pass; short-circuiting (findFirst/limit/anyMatch); `ReferencePipeline` + Sink chain; streams are single-use*

**156. map vs flatMap — main usages.**
   🔑 *`map` = 1:1 transform; `flatMap` = 1:many + flattens nested streams/Optionals (List<List<T>> → List<T>, `List<Order>` → `List<Item>`, unwrap Optional chains); Java 16 `mapMulti` for low-overhead flattening*

**157. What does peek() do, and when should you use it?**
   🔑 *Intermediate op for debugging/logging only; lazily executed, may be elided when the pipeline is short-circuited/optimized; never use for mutation or side effects*

**158. Parallel stream vs sequential stream — how does a parallel stream use CPU/memory, and when should you avoid it?**
   🔑 *Sequential = one thread, ordered; parallel = `ForkJoinPool.commonPool`, splits via Spliterator, needs stateless + associative + side-effect-free ops; good for large CPU-bound splittable data; bad for I/O (blocks common pool), small sets, shared mutable state; measure first, use a custom pool for isolation*

**159. How do pattern matching and method references work internally?**
   🔑 *`instanceof` pattern binds a typed variable (compiler inserts the cast); switch patterns compile to `invokedynamic` + a type-switch bootstrap; method refs desugar to the same `invokedynamic`/LambdaMetafactory as lambdas*

**160. Explain the record class — constructor, methods, static members; can it take part in inheritance?**
   🔑 *Implicit `private final` fields + canonical constructor + accessors + equals/hashCode/toString; compact constructor for validation; can have static members + custom methods; implicitly final → no inheritance, but can implement interfaces; record patterns (Java 21) destructure*

**161. Explain sealed classes/interfaces — with state, inheritance and exhaustive switch.**
   🔑 *`sealed` + `permits`; permitted subclasses must be `final`/`sealed`/`non-sealed`, same module/package; can hold state + statics; enables exhaustive switch (no default); models closed hierarchies/ADTs*

**162. Find the second largest number in an array using Streams.**
   🔑 *`stream().distinct().sorted(Comparator.reverseOrder()).skip(1).findFirst()`, or `boxed().collect(toCollection(TreeSet::new))`; handle duplicates and size < 2 / empty*

**163. Explain the Collectors API in depth.**
   🔑 *`groupingBy` (+ downstream like counting/mapping/summing), `partitioningBy`, `toMap` (with merge function for dup keys), `joining`, `collectingAndThen`, `teeing` (Java 12), `reducing`; `toUnmodifiableList`; custom `Collector` (supplier/accumulator/combiner/finisher)*

**164. reduce vs collect — when to use which?**
   🔑 *`reduce` = immutable fold, needs identity + associative accumulator (+ combiner for parallel); `collect` = mutable reduction into a container (supplier/accumulator/combiner); use `collect` for collections/StringBuilder, `reduce` for a single value*

**165. What are the common Stream pitfalls?**
   🔑 *Reusing a consumed stream (IllegalStateException), stateful/side-effecting lambdas, `Collectors.toMap` duplicate-key crash, `null` elements, unbounded `iterate`/`generate` without limit, parallel on non-associative ops, forgetting streams are lazy (no terminal = nothing runs)*

**166. Explain switch expressions, text blocks, var and record patterns.**
   🔑 *Switch expressions (`->`, `yield`, exhaustive, no fall-through); text blocks `"""..."""`; `var` local type inference (not for fields/params); record patterns + pattern matching for switch destructure + guard `when` clauses (Java 21)*

**167. Describe the standard functional interfaces and how you compose them.**
   🔑 *Function/BiFunction, Supplier, Consumer/BiConsumer, Predicate/BiPredicate, UnaryOperator/BinaryOperator; composition `andThen`/`compose`, Predicate `and`/`or`/`negate`; primitive variants (IntFunction, ToIntFunction) avoid boxing*

**168. Explain primitive streams and why they matter.**
   🔑 *`IntStream`/`LongStream`/`DoubleStream` avoid boxing overhead; `mapToInt`/`boxed`/`asLongStream`; `sum`/`average`/`summaryStatistics`/`range`/`rangeClosed`; use for numeric aggregation on large data*

**169. How do you create infinite/generated streams safely?**
   🔑 *`Stream.iterate(seed, next)` / `iterate(seed, hasNext, next)` (Java 9), `Stream.generate(supplier)`, must bound with `limit`/short-circuit; `Stream.ofNullable`, `IntStream.range`*

**170. Explain the java.time Date/Time API and why it replaced Date/Calendar.**
   🔑 *`LocalDate`/`LocalTime`/`LocalDateTime` (no zone), `ZonedDateTime`/`OffsetDateTime`, `Instant` (epoch), `Duration`/`Period`, `DateTimeFormatter`; immutable + thread-safe + fluent, ISO-8601, clear zone handling vs mutable, non-thread-safe legacy `Date`/`SimpleDateFormat`/`Calendar`*

**171. What is the Java 11 HttpClient and other everyday API additions?**
   🔑 *`java.net.http.HttpClient` sync (`send`) + async (`sendAsync` → CompletableFuture), HTTP/2, `BodyHandlers`; plus `String.isBlank/strip/repeat/lines`, `Files.readString/writeString`, `Collection.toArray(generator)`, `Stream.toList()` (Java 16)*

**172. How do you write a custom Collector?**
   🔑 *`Collector.of(supplier, accumulator, combiner, finisher, characteristics)` — supplier creates the container, accumulator folds one element, combiner merges partials (needed for parallel), finisher transforms the result; characteristics CONCURRENT/UNORDERED/IDENTITY_FINISH; or compose existing collectors (groupingBy + downstream); use when built-ins don't fit (custom aggregation)*

**173. When should you NOT use streams?**
   🔑 *Hot loops where a plain for-loop is faster + clearer; code that throws checked exceptions (lambdas can't propagate); heavy debugging (stack traces/stepping are worse); stateful/side-effecting logic; simple index-based iteration; tiny collections (overhead); readability-first when the stream pipeline gets unreadable; parallelStream on non-associative ops or I/O*

**174. What are sequenced collections and other recent API additions (Java 21)?**
   🔑 *SequencedCollection/SequencedSet/SequencedMap = defined encounter order with `getFirst`/`getLast`/`addFirst`/`reversed()` (unifies List/Deque/LinkedHashMap); plus record patterns + pattern matching for switch + guards, virtual threads, `Stream.toList()`, `Math.clamp`; know the LTS feature deltas 17→21*

[⬆️ Back to top](#contents)

## Exceptions & Error Handling

**175. Checked vs unchecked exceptions — the difference, when to use each, and how the compiler enforces them.**
   🔑 *Checked (IOException, SQLException) = recoverable, compiler "handle or declare" rule against the `throws` clause; unchecked (RuntimeException, IllegalArgument, NPE) = programming errors, exempt; Error = JVM-level (don't catch); modern practice = unchecked + domain exceptions, never swallow, wrap with context*

**176. finally vs try-with-resources — what happens if both the try block and close() throw?**
   🔑 *A `finally` that throws can swallow the original exception; try-with-resources closes in reverse order and attaches the close() failure as a suppressed exception (`getSuppressed()`), preserving the primary one*

**177. Can we have multiple finally blocks, and which one runs?**
   🔑 *One `finally` per `try`, but nested trys each have their own — innermost runs first; `finally` always runs except `System.exit`/JVM crash/infinite loop; avoid `return` inside finally (overrides the real result)*

**178. Explain exception rules in a class hierarchy (parent-child, overriding, multi-catch).**
   🔑 *Catch subclass before superclass (else unreachable catch); an overriding method can't throw broader checked exceptions (can narrow or throw unchecked); multi-catch (`A | B`) requires unrelated types; rethrow more precise type*

**179. How do you handle exceptions in the Executor framework? Explain throw vs throws.**
   🔑 *`submit()` captures the exception in the Future (surfaces on `get()` as ExecutionException); `execute()` propagates to the thread's UncaughtExceptionHandler; `throw` = statement, `throws` = declaration; always `get()` the Future or set a handler*

**180. How does `@ControllerAdvice` work for global exception handling?**
   🔑 *`@RestControllerAdvice` + `@ExceptionHandler` per type, ordered most-specific-first, returns ResponseEntity + ProblemDetail (RFC 7807) / `@ResponseStatus`; handles controller-layer exceptions only (NOT filters/security — those need an AuthenticationEntryPoint/AccessDeniedHandler or error page)*

**181. How do you design a good custom exception?**
   🔑 *Extend RuntimeException (unchecked) for most, meaningful name + error code + message, preserve the cause (`super(msg, cause)`), a small hierarchy (BusinessException/ValidationException/NotFoundException), immutable, don't log-and-rethrow (log once at the boundary)*

**182. What is exception chaining/wrapping and why does it matter?**
   🔑 *Wrap a low-level exception in a higher-level one with `initCause`/constructor cause → preserves the root-cause stack trace while abstracting the layer; `getCause()`; avoid losing the original by swallowing; `@Repository` translates persistence exceptions to DataAccessException*

**183. What are the best practices and anti-patterns in exception handling?**
   🔑 *Don't swallow (empty catch), don't catch `Throwable`/`Exception` broadly, don't catch Error (OOM/StackOverflow), fail fast, throw early + catch late, add context, no control flow via exceptions, log once, use `finally`/try-with-resources for cleanup*

**184. How do you handle checked exceptions inside streams and lambdas?**
   🔑 *Lambdas/functional interfaces can't propagate checked exceptions → wrap in a RuntimeException inside the lambda, or a helper that rethrows unchecked (sneaky throw), or use a wrapper functional interface; prefer unchecked domain exceptions in stream-heavy code*

**185. How do exceptions propagate in CompletableFuture / async code?**
   🔑 *Exception completes the future; surfaces as CompletionException (async) or ExecutionException (`get`); handle with `exceptionally`/`handle`/`whenComplete`; set an AsyncUncaughtExceptionHandler for void `@Async`; uncaught in a plain thread → UncaughtExceptionHandler*

**186. What is the difference between an Exception and an Error?**
   🔑 *Exception = conditions an app can reasonably catch/recover (checked + unchecked); Error = serious JVM problems (OutOfMemoryError, StackOverflowError, NoClassDefFoundError) — generally not catchable/recoverable; both extend Throwable*

**187. How do you design an application-wide error contract / error-code catalog?**
   🔑 *Stable machine-readable error codes (domain.subdomain.reason) + human message + trace id + field errors, served as RFC 7807 problem+json; central `@ControllerAdvice`; map domain exceptions → HTTP status consistently (400/401/403/404/409/422/429/5xx); never leak stack traces/internals; version the contract; document codes for clients; i18n messages separate from codes*

**188. Retry vs fail-fast vs fallback — how do you choose per failure type?**
   🔑 *Transient (timeout, 503, deadlock) → retry with exponential backoff + jitter (idempotent only, bounded attempts); permanent (400/validation/auth) → fail fast, no retry; degraded dependency → fallback (cached/default/queue-for-later) + circuit breaker; classify exceptions as retryable vs non-retryable; combine with timeout + bulkhead; never retry non-idempotent writes without an idempotency key*

[⬆️ Back to top](#contents)

## Spring Core & Spring Boot

> *Transaction management (`@Transactional`, propagation, isolation, proxy/self-invocation) lives in the **JPA / Hibernate / Transactions** section. The Spring Security filter chain and JWT live in the **Security** section.*

**189. Explain the Spring bean lifecycle end-to-end (instantiation → DI → BeanPostProcessor → init → destroy).**
   🔑 *Scan/definition → instantiate (constructor) → populate deps → *Aware callbacks → BeanPostProcessor.before → `@PostConstruct`/`afterPropertiesSet`/init-method → BPP.after (AOP proxy created here) → in use → `@PreDestroy`/destroy; prototype beans aren't destroyed by the container*

**190. Constructor injection vs field vs setter injection — why is constructor injection preferred?**
   🔑 *Constructor = fully-initialized immutable `final` fields, fails fast on missing/circular deps at startup, testable without a container/reflection, no hidden deps; field injection needs reflection, hides bloated constructors, lets circular deps slip through; setter for optional deps*

**191. How does Spring Boot auto-configuration work under the hood?**
   🔑 *`@EnableAutoConfiguration` → AutoConfigurationImportSelector reads `META-INF/spring/...AutoConfiguration.imports` (`spring.factories` pre-2.7) → `@Conditional` eval (OnClass/OnMissingBean/OnProperty/OnWebApplication) → ordering via `@AutoConfigureBefore/After`, user beans win (`@ConditionalOnMissingBean`); `--debug` prints the conditions report; exclude via property/annotation*

**192. Explain bean scopes, and a real bug from injecting a prototype into a singleton.**
   🔑 *singleton (default, per container — controllers/services/repositories are singletons), prototype (new per lookup), request/session/application/websocket (web); prototype injected into a singleton is created once → use `ObjectProvider`/`@Lookup`/scoped proxy; a singleton holding request state = thread-safety bug*

**193. Explain the difference between @Component, @Service, @Repository and @Controller — including what @Repository actually adds.**
   🔑 *All are `@Component` stereotypes (same scanning/bean creation); `@Repository` adds persistence exception translation to DataAccessException; `@Controller`/`@RestController` enable handler mapping; `@Service` = semantics + AOP pointcut target; swapping them still works but loses translation/clarity/mapping*

**194. Difference between @RestController and @Controller.**
   🔑 *`@RestController` = `@Controller` + `@ResponseBody` → returns the body serialized by HttpMessageConverters (JSON); `@Controller` returns a view name resolved by a ViewResolver*

**195. What is @Qualifier used for, and when do you need it?**
   🔑 *Disambiguates multiple beans of the same type (with `@Primary` as the default winner); match by bean name or a custom qualifier annotation; alternative = inject `List`/`Map` of all beans of a type*

**196. What does @SpringBootApplication bundle?**
   🔑 *`@SpringBootConfiguration` (`@Configuration`) + `@EnableAutoConfiguration` + `@ComponentScan` of the current package downward*

**197. What's the difference between @RequestParam, @PathVariable and @RequestBody?**
   🔑 *`@RequestParam` = query/form param (required, defaultValue); `@PathVariable` = URI template segment; `@RequestBody` = JSON body deserialized by an HttpMessageConverter + `@Valid`; `@RequestHeader`/`@CookieValue` similar*

**198. What does @Async do, and what's the self-invocation gotcha?**
   🔑 *Runs the method on a TaskExecutor returning void/Future/CompletableFuture; needs `@EnableAsync` + a dedicated `ThreadPoolTaskExecutor`; proxy-based → self-invocation runs synchronously, private/static skipped; set AsyncUncaughtExceptionHandler; security/transaction context not propagated by default*

**199. What is the reactive/WebFlux stack, and when would you NOT use it over Spring MVC?**
   🔑 *Non-blocking Netty + Reactor (Mono/Flux), backpressure, few threads for high concurrency, needs R2DBC/WebClient to stay non-blocking; avoid when the stack is blocking (JDBC), the team is unfamiliar, debugging/stack traces matter, or load is modest — MVC + virtual threads is often simpler*

**200. How do you manage configuration across environments — profiles, property precedence and @ConfigurationProperties?**
   🔑 *`application-{profile}.yml` + `spring.profiles.active`; precedence (low→high): defaults < jar yml < profile yml < external config < OS env < system props < CLI args < `@TestPropertySource`; `@ConfigurationProperties` typed + relaxed binding + `@Validated`, constructor binding for immutability; `@Profile` beans*

**201. How do you activate a Spring profile, and what is a profile?**
   🔑 *Profile = named grouping of beans/config per environment; activate via `spring.profiles.active`, `-D`/`SPRING_PROFILES_ACTIVE` env var, CLI arg, `@ActiveProfiles` in tests, `spring.profiles.group` for composites*

**202. @Value vs @ConfigurationProperties — when to use which?**
   🔑 *`@Value("${...}")` = single property / SpEL, no relaxed binding or validation; `@ConfigurationProperties` = type-safe bulk binding of a prefix to a POJO, relaxed binding, nested/lists, JSR-303 validation — preferred for grouped config*

**203. What does Spring Boot Actuator provide, how does it work, and how do you add/secure/relocate endpoints?**
   🔑 *Production endpoints over HTTP/JMX (`/health` with readiness/liveness groups, `/metrics` via Micrometer → Prometheus, `/info`, `/env`, `/loggers`, `/threaddump`, `/heapdump`); custom `HealthIndicator`/`@Endpoint`; `management.endpoints.web.exposure.include`, separate `management.server.port`, secure behind auth*

**204. How have you used AOP for cross-cutting concerns, and what are its proxy limitations?**
   🔑 *`@Aspect` + pointcut + `@Around`/`@Before`/`@AfterThrowing` for logging, auditing, metrics, retry, security, caching; join point/pointcut/advice/weaving; proxy-based → no self-invocation, no final/private/static methods, only Spring beans; AspectJ load/compile-time weaving for the rest*

**205. Spring vs Spring Boot.**
   🔑 *Spring = modular framework, manual config (XML/JavaConfig), external server; Boot = opinionated auto-config, starters + dependency BOM, embedded server, actuator, externalized config, fat jar, production-ready defaults*

**206. How does the embedded web server work, and how do you switch it or use an external one?**
   🔑 *Embedded Tomcat by default (`spring-boot-starter-web`); swap to Jetty/Undertow by excluding the Tomcat starter + adding another; Netty for WebFlux; configure `server.port`/threads; or build a WAR + `SpringBootServletInitializer` + `provided` scope for external Tomcat/WebLogic*

**207. Explain IoC vs DI and how the container stores/resolves beans internally.**
   🔑 *IoC = principle (container owns creation/lifecycle); DI = mechanism (constructor/setter/field); `ApplicationContext`/DefaultListableBeanFactory holds a ConcurrentHashMap of singletons + BeanDefinitionRegistry; `@Autowired` resolves by type → qualifier → name; 3-level cache resolves circular references*

**208. BeanFactory vs ApplicationContext.**
   🔑 *BeanFactory = basic lazy DI container; ApplicationContext = BeanFactory + eager singletons + i18n + event publishing + AOP + resource loading + Environment — what Boot uses*

**209. How do you remove a cyclic dependency between beans?**
   🔑 *Redesign responsibilities (extract a shared bean), use application events / async messaging, interface + callback, `@Lazy` or setter injection as a stopgap; Boot 2.6+ fails on circular refs by default (`spring.main.allow-circular-references`)*

**210. What is @PostConstruct (and its alternatives)?**
   🔑 *Runs after DI, before the bean is in use; one per class, no args, void; alternatives `InitializingBean.afterPropertiesSet` / `@Bean(initMethod)`; use for warm-up/validation, not heavy blocking work; `@PreDestroy` for cleanup*

**211. What happens in the background when a Spring Boot application starts (SpringApplication.run)?**
   🔑 *Deduce web type → load ApplicationContextInitializers/Listeners → prepare Environment + profiles → banner → create context → load bean definitions (component scan + auto-config) → refresh (BeanFactoryPostProcessors, BeanPostProcessors, instantiate singletons, create AOP proxies) → start embedded server → publish ApplicationReadyEvent → run ApplicationRunner/CommandLineRunner*

**212. Explain the full request flow through a Spring Boot web application.**
   🔑 *Request → Servlet filters (incl. security filter chain) → DispatcherServlet → HandlerMapping → HandlerInterceptor.preHandle → HandlerAdapter → controller → service → repository → return → HttpMessageConverter → interceptor.postHandle → response; exceptions → HandlerExceptionResolver/`@ControllerAdvice`*

**213. Filter vs Interceptor vs ControllerAdvice — how do they differ and in what order?**
   🔑 *Servlet Filter = outermost, works on raw Servlet request/response (CORS, auth, logging), before DispatcherServlet; HandlerInterceptor = Spring MVC, around the handler (pre/post/afterCompletion), has handler context; `@ControllerAdvice` = exception handling + model/binder advice at controller layer*

**214. CommandLineRunner vs ApplicationRunner.**
   🔑 *Both run once after the context is ready (ApplicationReadyEvent area); CommandLineRunner gets raw `String[] args`, ApplicationRunner gets parsed `ApplicationArguments`; order with `@Order`; use for startup tasks/seed data (keep them fast)*

**215. How does Spring's caching abstraction work (@Cacheable/@CacheEvict/@CachePut)?**
   🔑 *`@EnableCaching` + proxy; `@Cacheable` (compute-if-absent), `@CachePut` (always update), `@CacheEvict` (remove/clear); key via SpEL/KeyGenerator, `unless`/`condition`; CacheManager (Caffeine/Redis/Ehcache); same proxy self-invocation + null-caching caveats*

**216. How do you do validation in Spring (@Valid vs @Validated, custom validators)?**
   🔑 *Bean Validation (Jakarta) `@NotNull`/`@Size`/`@Email` + `@Valid` on `@RequestBody`/nested; `@Validated` enables groups + method-level validation on beans; custom `ConstraintValidator` + annotation; handle MethodArgumentNotValidException in `@ControllerAdvice`*

**217. How do you publish and listen to application events in Spring?**
   🔑 *`ApplicationEventPublisher.publishEvent` + `@EventListener` (or implement ApplicationListener); sync by default, `@Async` for async; `@TransactionalEventListener` fires on commit/rollback phases; decouples modules; Spring built-in events (ContextRefreshed, ApplicationReady)*

**218. How do you schedule tasks in Spring (@Scheduled)?**
   🔑 *`@EnableScheduling` + `@Scheduled(cron/fixedRate/fixedDelay)`; single-threaded scheduler by default → configure a `TaskScheduler`/pool; cluster-safe scheduling needs ShedLock/Quartz to avoid duplicate runs across instances*

**219. How do you write a custom @Conditional / auto-configuration?**
   🔑 *Implement `Condition`/`@Conditional`, or use `@ConditionalOnClass`/`OnMissingBean`/`OnProperty`; create an `@AutoConfiguration` class listed in `AutoConfiguration.imports` (a starter), `@ConditionalOnMissingBean` so user config wins, order with before/after*

**220. What changed in Spring Boot 3 (Jakarta, native image, AOT)?**
   🔑 *Baseline Java 17, `javax.*` → `jakarta.*` namespace, Spring Framework 6, GraalVM native image support + AOT processing (faster startup, lower memory), Micrometer Observation/tracing, Problem Details by default, HTTP interface clients*

**221. @Data (Lombok) — when is it safe and when does it cause silent bugs?**
   🔑 *`@Data` = getters/setters/equals/hashCode/toString/RequiredArgsConstructor; dangerous on JPA entities (equals/hashCode over all fields + lazy loading + bidirectional toString → StackOverflow/N+1); prefer `@Getter` + explicit equals/hashCode on the business id, or records for DTOs*

**222. Why do we need Spring Batch, and what is a batch pipeline (incl. reading a 2M-row sheet into MongoDB)?**
   🔑 *Chunk-oriented: Job → Step → ItemReader/ItemProcessor/ItemWriter, chunk size + commit interval, JobRepository for restart/skip/retry, partitioned/multithreaded steps, scheduling; 2M rows → streaming reader (SAX/`FlatFileItemReader`) → validate/map → `MongoItemWriter`, chunk 1k–5k, skip policy + restartability, build indexes after load, watch memory*

**223. How do you optimize Spring application startup time?**
   🔑 *Lazy bean init (`spring.main.lazy-initialization`), fewer classpath scans / spring-context-indexer, AOT + GraalVM native image, slimmer starters, defer non-critical ApplicationRunners, tiered compilation; trade-off = first-request latency vs startup*

**224. Explain the Maven POM and dependency management (starters, BOM, scopes, transitive deps).**
   🔑 *POM = coordinates (groupId:artifactId:version) + dependencies + build; `spring-boot-starter-parent` or the `spring-boot-dependencies` BOM (`<dependencyManagement>` import) aligns versions; starters = curated dependency bundles; scopes = compile (default), provided (servlet API), runtime (JDBC driver), test (JUnit), import (BOM); transitive deps resolved by nearest-wins → use `<exclusions>` + `mvn dependency:tree` to fix conflicts; plugins (compiler, surefire/failsafe, spring-boot) drive the build*

**225. How do you configure a custom async executor for @Async?**
   🔑 *`@EnableAsync` + a `ThreadPoolTaskExecutor` bean (corePoolSize, maxPoolSize, queueCapacity, keepAlive, thread-name prefix, rejection policy); name it and reference via `@Async("executorName")`; add a TaskDecorator to propagate MDC/SecurityContext/trace; set an AsyncUncaughtExceptionHandler for void methods; separate executors per workload (bulkhead); never use the default SimpleAsyncTaskExecutor in prod (unbounded)*

**226. How do you configure WebClient/RestClient (timeouts, pooling, retries, error handling)?**
   🔑 *Base URL + default headers; connect/response timeouts (HttpClient `responseTimeout`, connection pool via Reactor Netty `ConnectionProvider`); `ExchangeFilterFunction` for auth/logging/trace; `retryWhen` with backoff + jitter (idempotent only); `onStatus` → map 4xx/5xx to domain exceptions; integrate Resilience4j (circuit breaker/time limiter); `RestClient` (Spring 6.1) = synchronous fluent alternative; avoid RestTemplate (maintenance mode)*

**227. How does Spring resolve ambiguous beans and handle overriding?**
   🔑 *`@Primary` (default winner), `@Qualifier`/bean name (explicit), `@Conditional`/`@ConditionalOnMissingBean` (auto-config backs off), `@Profile`, inject `List`/`Map<String,Bean>` for all; bean-definition overriding disabled by default (`spring.main.allow-bean-definition-overriding`); `@Order`/`@Priority` for collection ordering; `@DependsOn` for init order*

**228. How do you map between entities and DTOs (MapStruct vs ModelMapper vs manual)?**
   🔑 *MapStruct = compile-time generated mappers (fast, type-safe, no reflection, `@Mapper`, explicit field maps) — preferred; ModelMapper = runtime reflection (convenient, slower, surprises); manual/constructor/record mapping for simple cases; keep mapping out of the domain, map entity↔DTO at the boundary to avoid leaking entities / LazyInitializationException*

**229. How do @Transactional, @Async and events interact (and what breaks)?**
   🔑 *`@Async` runs on a new thread → new/no transaction + no propagated security/transaction context by default (combine carefully — don't assume the caller's tx); publish a domain event and use `@TransactionalEventListener(phase=AFTER_COMMIT)` to run side effects only after commit; self-invocation bypasses both proxies; an `@Async` method that needs a tx must itself be `@Transactional` on a separate bean*

**230. How do you deploy to / run on an external application server instead of the embedded one?**
   🔑 *Package as WAR: `extends SpringBootServletInitializer` + override `configure(SpringApplicationBuilder)`, set packaging `war`, mark the embedded Tomcat starter `provided`; deploy to external Tomcat/JBoss/WebLogic; or keep the embedded server but swap Tomcat→Jetty/Undertow via starter exclusion; prefer the embedded fat-jar + container model for microservices*

[⬆️ Back to top](#contents)

## JPA / Hibernate / Transactions

**231. N+1 query problem — how do you detect and fix it (JOIN FETCH, @EntityGraph, batch fetching)?**
   🔑 *1 parent query + N child queries; detect via show-sql / `hibernate.generate_statistics` / datasource-proxy / p6spy; fix = `JOIN FETCH`, `@EntityGraph(attributePaths=...)`, `@BatchSize` / `default_batch_fetch_size`, DTO projection, `@Fetch(SUBSELECT)`; beware cartesian product with multiple collections + pagination needing a count query*

**232. Explain first-level vs second-level cache — and the risks of L2 in a multi-instance deployment.**
   🔑 *L1 = Session/EntityManager scope, mandatory, per-transaction identity map; L2 = SessionFactory scope, shared (Ehcache/Hazelcast/Redis), needs `@Cacheable` + cache mode; query cache is separate (needs L2); distributed risk = stale reads, invalidation storms, split-brain → clustered/replicated cache + TTL; never cache hot-mutating entities*

**233. Lazy vs eager loading — what is LazyInitializationException and how do you avoid it cleanly?**
   🔑 *LAZY = proxy loaded on access; EAGER = join at load (causes N+1/over-fetch, can't disable per query); LazyInitializationException when the session is closed → fix with JOIN FETCH / `@EntityGraph` / DTO projection / proper `@Transactional` service boundary; OSIV (Open-Session-In-View) defers connection release → pool exhaustion*

**234. Optimistic vs pessimistic locking — how do you implement each, and how do they behave under concurrency?**
   🔑 *Optimistic = `@Version` column, no DB lock, both read version n, first commit wins, second gets OptimisticLockException → retry; best for low contention; Pessimistic = `PESSIMISTIC_READ/WRITE` → `SELECT ... FOR UPDATE` blocks other writers (readers still read via MVCC unless FOR SHARE), risk of deadlock/timeout → set lock timeout*

**235. Two threads/instances update the same row and you see lost updates — how do you fix it?**
   🔑 *Lost update = read-modify-write race; fixes = `@Version` optimistic + retry (return 409/ETag conflict to user), `SELECT FOR UPDATE` pessimistic, atomic conditional `UPDATE ... SET x=x-? WHERE status=?` + check affected rows, DB unique/idempotency constraint, distributed lock, or redesign to append-only ledger/event + state machine*

**236. Explain the @Transactional proxy mechanism, self-invocation, and private/final methods.**
   🔑 *Spring wraps the bean in a JDK dynamic (interface) / CGLIB (class) proxy; TransactionInterceptor begins/commits/rolls back around the call, binding the connection to a ThreadLocal via TransactionSynchronizationManager; self-invocation (`this.method()`) bypasses the proxy → no transaction; private/static/final methods aren't intercepted; fix = self-inject, AopContext, move to another bean, or AspectJ weaving*

**237. Explain propagation types (REQUIRED, REQUIRES_NEW, NESTED) and isolation, with a parent/child rollback scenario.**
   🔑 *REQUIRED joins or creates (default — parent + both children roll back together); REQUIRES_NEW suspends the outer tx (independent audit/log that survives the parent rollback); NESTED = savepoints (partial rollback, JDBC only); also MANDATORY/SUPPORTS/NEVER/NOT_SUPPORTED; isolation READ_COMMITTED default*

**238. What are the transaction rollback rules for checked vs unchecked exceptions?**
   🔑 *By default only RuntimeException + Error roll back; a checked exception COMMITS → use `@Transactional(rollbackFor = Exception.class)` to force rollback, `noRollbackFor` to deliberately commit; catching without rethrow = no rollback; `setRollbackOnly` marks it unrecoverable*

**239. Why is @Transactional not working — what are the possible reasons?**
   🔑 *Self-invocation (proxy bypass), non-public/private/final method, class not a Spring bean, exception caught without rethrow, checked exception (no rollbackFor), wrong PlatformTransactionManager, non-transactional engine (MyISAM), called from a different thread, `@Transactional` on the wrong layer*

**240. How do you create and roll back a transaction programmatically?**
   🔑 *`TransactionTemplate.execute` or `PlatformTransactionManager` (`getTransaction`/`commit`/`rollback`); rollback via the DB's undo log/WAL; `setRollbackOnly()`; savepoints for partial rollback (NESTED); use when you need fine-grained control or non-public boundaries*

**241. How do you manage transactions in production (keeping them short)?**
   🔑 *Short, explicit boundaries at the service layer, `readOnly=true` for queries, timeout + lock timeout, no remote calls/file IO inside, chunk large writes (batch size + flush/clear), retry on optimistic/deadlock errors, idempotency keys, outbox for events, monitor long-running tx + lock waits*

**242. Difference between save(), saveAndFlush(), persist() and merge().**
   🔑 *`persist` (JPA) = transient→managed, no id guarantee, throws if detached; `save` (Hibernate) = returns id, merges detached; `merge` = copies detached state into a managed copy and returns it; `saveAndFlush` = save + immediate flush (same transaction)*

**243. What is Hibernate's dirty checking mechanism?**
   🔑 *On flush, Hibernate compares the entity's current state with the snapshot taken at load → generates UPDATE for changed columns only (all columns unless `@DynamicUpdate`); no explicit `save()` needed inside a transaction*

**244. Explain Hibernate internals — entity states, persistence context, flush and commit.**
   🔑 *EntityManagerFactory → EntityManager → persistence context (identity map + dirty checking); entity states transient → persistent/managed → detached → removed; flush pushes SQL (uncommitted) — auto before queries + at commit; commit makes durable; `clear`/`detach` evicts; FlushMode.AUTO; ActionQueue; dialect → JDBC batch*

**245. JPA vs Hibernate vs Spring Data JPA.**
   🔑 *JPA = specification/API (EntityManager, JPQL, annotations); Hibernate = implementation + extras (Criteria, `@DynamicUpdate`, filters, natural id); Spring Data JPA = repository abstraction, derived query method names (`findByXAndYOrderByZ`), proxy over SimpleJpaRepository*

**246. How do Spring Data JPA repositories work internally?**
   🔑 *At startup Spring Data creates a proxy over SimpleJpaRepository; query lookup = derived from method name (PartTree parsing), `@Query` (JPQL/native), or named query; QueryLookupStrategy; `@Transactional` applied on SimpleJpaRepository; Pageable/Sort/Specification support*

**247. @OneToMany vs @ManyToMany mapping, and owning side / fetch defaults.**
   🔑 *`@OneToMany(mappedBy)` = inverse side, FK in child; `@ManyToMany` = join table (`@JoinTable`) — prefer two `@OneToMany` to an explicit join entity when it has attributes; collections default LAZY, `@ManyToOne`/`@OneToOne` default EAGER → make them LAZY*

**248. Explain cascade types and orphanRemoval.**
   🔑 *CascadeType PERSIST/MERGE/REMOVE/REFRESH/DETACH/ALL propagate operations parent→child; `orphanRemoval=true` deletes a child removed from the collection; cascade REMOVE ≠ orphanRemoval; avoid CascadeType.ALL on `@ManyToMany`*

**249. Explain entity inheritance strategies (SINGLE_TABLE, JOINED, TABLE_PER_CLASS).**
   🔑 *SINGLE_TABLE = one table + discriminator (fast, nullable columns); JOINED = normalized, one table per class + joins (integrity, slower reads); TABLE_PER_CLASS = one full table per concrete class (union queries); `@MappedSuperclass` = shared mapping, not an entity; `@Embeddable`/`@Embedded`/`@ElementCollection` for value types*

**250. Explain id generation strategies (IDENTITY, SEQUENCE, TABLE, UUID).**
   🔑 *IDENTITY = DB auto-increment (disables JDBC batch inserts); SEQUENCE = DB sequence + `allocationSize` pooling (preferred, batch-friendly); TABLE = portable but slow; UUID/`@GeneratedValue` for distributed ids; natural vs surrogate key; `@EmbeddedId`/`@IdClass` for composite keys*

**251. How do you tune batch inserts/updates in Hibernate?**
   🔑 *`hibernate.jdbc.batch_size`, `order_inserts`/`order_updates=true`, `batch_versioned_data`; avoid IDENTITY (prevents batching) → use SEQUENCE; `flush()`+`clear()` every N rows to bound the persistence context; or bulk JPQL/native / JdbcTemplate batchUpdate for huge loads*

**252. How do you build DTO projections, and why avoid returning entities?**
   🔑 *Interface or class (constructor) projections, JPQL `new DTO(...)`, `@EntityGraph` + mapping, or MapStruct; avoids over-fetching, LazyInitializationException, and exposing entities over the API; read-only + lighter than managed entities*

**253. How do you build dynamic queries (Criteria API, Specifications, QueryDSL)?**
   🔑 *JPA Criteria API (typesafe, verbose), Spring Data `Specification`/`JpaSpecificationExecutor` (composable predicates), QueryDSL `BooleanBuilder` (fluent), or `@Query` with `:x IS NULL OR col=:x`; whitelist sort columns (injection), use for optional filters + date ranges + pagination*

**254. How do you call a stored procedure from JPA/Hibernate?**
   🔑 *`@NamedStoredProcedureQuery` + `@Procedure` on the repository, or `EntityManager.createStoredProcedureQuery` with IN/OUT/INOUT params + `getOutputParameterValue`; `StoredProcedureQuery`; map result sets; use for set-based DB-side logic, but keep business logic in the app layer*

**255. How do you implement auditing (created/modified by/at)?**
   🔑 *Spring Data `@EnableJpaAuditing` + `@CreatedDate`/`@LastModifiedDate`/`@CreatedBy`/`@LastModifiedBy` + `AuditingEntityListener` + `AuditorAware`; Hibernate Envers for full entity revision history/versioned audit tables*

**256. Soft delete vs hard delete.**
   🔑 *Hard = row removed (FK/audit loss, unrecoverable); soft = `is_deleted`/`deleted_at` flag + `@SQLDelete` + `@SQLRestriction`/`@Where`; costs = index bloat, unique-constraint conflicts (partial index), every query must filter, needs an archival/purge job*

**257. Should you store foreign keys directly or model associations — and how do you handle historical accuracy?**
   🔑 *Model as `@ManyToOne` associations (FK) for referential integrity + joins; add denormalized snapshot columns (e.g. name/price at transaction time) for historical accuracy + read speed; avoid unnecessary bidirectional mappings; `@MapsId` for shared PK*

**258. How do you handle multiple writes where one must persist even if the rest roll back?**
   🔑 *Parent `@Transactional(REQUIRED)` so failures roll back together; put the audit/failure-state write in `REQUIRES_NEW` (its own tx survives the rollback), or use a TransactionSynchronization `afterCompletion` hook, or `noRollbackFor`; for cross-service use a compensating transaction / Saga*

**259. How do you configure multiple datasources in one Spring Boot service?**
   🔑 *Two `DataSource` beans (one `@Primary`), each with its own `EntityManagerFactory` + `PlatformTransactionManager`; `@EnableJpaRepositories(basePackages, entityManagerFactoryRef, transactionManagerRef)` per repo/entity package; separate `@ConfigurationProperties` (`spring.datasource.a/b`); no single transaction spans both unless XA/JTA (avoid — blocking, complex) → per-DB transactions + Saga/outbox for cross-DB consistency; separate HikariCP pools*

**260. How do you implement read/write (primary/replica) splitting?**
   🔑 *`AbstractRoutingDataSource` picks primary vs replica by a routing key (e.g. `@Transactional(readOnly=true)` → replica via a `TransactionSynchronization`/AOP flag); writes → primary; beware replication lag (read-your-writes → route critical reads to primary or use a version token); separate pools; or let the DB proxy (ProxySQL/PgBouncer) route; monitor lag*

[⬆️ Back to top](#contents)

## Databases — SQL, NoSQL & Query Optimization

**261. How do you run schema migrations with zero downtime (Flyway/Liquibase, expand-contract)?**
   🔑 *Expand-contract (parallel change): add nullable column → dual-write → backfill in batches → switch reads → stop writing old → drop; Flyway/Liquibase versioned + repeatable + checksums + baseline; never rename/drop in the same release; avoid long locks (ADD COLUMN default safe PG 11+, CREATE INDEX CONCURRENTLY), `lock_timeout`/`statement_timeout`, backward-compatible for the N-1 app during rolling deploy; separate DDL from big data backfills*

**262. How do you evolve a schema in a backward-compatible way, including constraint/column changes?**
   🔑 *Additive-only, tolerant reader, version the DTO not the table; add constraint `NOT VALID` then `VALIDATE` separately (PG) to avoid long locks; widen types safely, never narrow in one step; add nullable → backfill → set NOT NULL; dual-write during transition; keep old columns readable until all consumers migrate*

**263. How do you migrate an application to a new database with no downtime?**
   🔑 *Dual-write or CDC (Change Data Capture, e.g. Debezium) replication, backfill history, shadow reads + reconciliation, feature-flag the read switch then the write switch, keep a rollback path, strangler-fig cutover, decommission old DB last*

**264. Explain isolation levels and a real dirty-read/phantom-read bug.**
   🔑 *READ UNCOMMITTED → dirty read; READ COMMITTED → non-repeatable read (PG default); REPEATABLE READ → phantom read (MySQL default; gap locks prevent it); SERIALIZABLE → full isolation (locking/SSI); MVCC snapshots; classic bug = balance check then debit under READ COMMITTED → double spend (fix: SELECT FOR UPDATE / SERIALIZABLE / atomic UPDATE)*

**265. Explain the ACID properties with examples.**
   🔑 *Atomicity = all-or-nothing (transfer debit+credit); Consistency = constraints/invariants hold; Isolation = concurrent txns don't interfere; Durability = committed survives crash (WAL/redo log + fsync)*

**266. Explain the CAP theorem (and PACELC) and how it guides design.**
   🔑 *Under a network Partition you choose Consistency or Availability; CP (strong consistency, may reject — RDBMS/ZooKeeper/HBase) vs AP (always answer, eventual — Dynamo/Cassandra); no "CA" in a distributed system; PACELC adds the latency-vs-consistency trade-off when there's no partition; drives DB + replication choices*

**267. How do you paginate efficiently over a huge table without OFFSET collapse?**
   🔑 *Keyset/seek pagination: `WHERE (created_at, id) < (:ts,:id) ORDER BY ... LIMIT n` — index-driven, O(1) per page; OFFSET scans+discards N rows → O(N); cursor/token APIs, covering index, avoid `COUNT(*)` for totals (estimate / "has next")*

**268. Explain indexing — B-tree indexes and when an index does NOT help.**
   🔑 *B-tree = balanced, sorted, O(log n); supports `=`, range, ORDER BY, composite prefix; doesn't help on low cardinality (use partial/bitmap), leading-wildcard `LIKE '%x'`, function on a column (unless a functional index), tiny tables, poor selectivity (optimizer picks seq scan); also GIN/GiST/BRIN/hash index types*

**269. How do you design composite indexes, and can two indexes be used at once?**
   🔑 *Order = equality → range → sort (ESR rule); leftmost-prefix rule; high-selectivity columns first; covering/INCLUDE index to avoid table lookup; partial index for filtered subsets; usually one index per table access, but optimizer can combine via bitmap index scan (PG) / index merge (MySQL); cost = slower writes + storage*

**270. How do you optimize a slow SQL query?**
   🔑 *EXPLAIN ANALYZE first; index WHERE/JOIN/ORDER BY columns, keep predicates SARGable (no function on column), SELECT only needed columns (projection), avoid N+1 + correlated subqueries, EXISTS over IN for large sets, update statistics, partition/archive huge tables, batch writes, covering index, caching*

**271. How do you read and use an EXPLAIN plan (join strategies)?**
   🔑 *Plan shows access path (seq/index scan), join algorithm (nested loop / hash / merge), estimated vs actual rows, cost, sort/hash spill to disk; nested loop = small driving set + indexed inner; hash join = large unsorted; merge join = pre-sorted; fix misestimates with ANALYZE, add composite index on join+filter keys*

**272. Explain the different SQL joins.**
   🔑 *INNER (matches only), LEFT/RIGHT OUTER (keep all of one side + NULLs), FULL OUTER, CROSS (cartesian), SELF, SEMI (EXISTS), ANTI (NOT EXISTS); watch row multiplication on one-to-many; join on indexed keys; NULL behaviour in outer joins*

**273. WHERE vs HAVING.**
   🔑 *WHERE filters rows before grouping (no aggregates); HAVING filters groups after GROUP BY (can use aggregates); logical order FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT; push conditions into WHERE for performance*

**274. COUNT(*) vs COUNT(1) vs COUNT(column) vs COUNT(DISTINCT).**
   🔑 *COUNT(*) counts rows (optimizer picks cheapest index) — identical to COUNT(1) in practice; COUNT(column) skips NULLs; COUNT(DISTINCT col) is much more expensive (sort/hash); for huge tables use estimates (`pg_class.reltuples`)*

**275. Explain window functions and CTEs.**
   🔑 *Window functions compute over a partition without collapsing rows: `ROW_NUMBER/RANK/DENSE_RANK`, `LAG/LEAD`, running `SUM/AVG OVER (PARTITION BY ... ORDER BY ...)`; CTE (`WITH`) for readability/recursion (hierarchies); beware CTE materialization costs; use for top-N-per-group, de-dup, running totals*

**276. SQL vs NoSQL — differences, when to pick each, and the cons of Postgres vs MongoDB.**
   🔑 *SQL = schema-on-write, ACID, joins, strong consistency, vertical scale, reporting; NoSQL = schema-on-read, BASE/eventual, horizontal scale, denormalized, access-pattern-first; decide by consistency/relationships/query shape/scale; Postgres cons = scaling limits + connection cost (PgBouncer) + VACUUM/bloat; Mongo cons = weak joins ($lookup), memory-hungry working set, eventual consistency on secondaries, schema drift; often polyglot persistence*

**277. What is connection pooling, how do you size a pool, and how do you diagnose pool exhaustion?**
   🔑 *Reuse DB connections (expensive TCP+auth+session) via HikariCP; size small (≈ `cores × 2` or `connections = ((core_count × 2) + effective_spindle)`, NOT hundreds) — DB can't parallelize beyond its capacity; `maximumPoolSize`, `connectionTimeout`, `idleTimeout`, `maxLifetime` < DB timeout; exhaustion = threads waiting on `getConnection` (pending > 0) → slow queries holding connections, OSIV, leaks (unclosed), long transactions; PgBouncer for many clients*

**278. Polling vs push/event-driven — how do you avoid inefficient DB polling?**
   🔑 *Polling = repeated SELECTs (simple but wasteful, lag vs load trade-off) → tune interval, index the status column, `SELECT ... FOR UPDATE SKIP LOCKED` for a work queue; prefer push: DB triggers/LISTEN-NOTIFY (PG), CDC/outbox (Debezium → Kafka), webhooks, message queues, long-polling / Server-Sent Events / WebSocket for clients; batch instead of row-by-row polling*

**279. What are stored procedures and functions — when should you use them (and not)?**
   🔑 *Precompiled SQL/procedural code (PL/pgSQL, T-SQL) running in the DB; pros = set-based performance, fewer round-trips, central data-side logic, atomic; cons = business logic hidden from VCS/tests, DB-vendor lock-in, hard to debug/version, scaling tied to the DB; use for heavy set operations/ETL/bulk, keep domain logic in the app; function returns a value (usable in queries), procedure can do transactions/multiple result sets*

**280. What are cursors and triggers — when should you use a trigger?**
   🔑 *Cursor = row-by-row pointer over a result set (heavy — prefer set-based SQL); Trigger = procedural code fired on INSERT/UPDATE/DELETE (BEFORE/AFTER, row/statement); use for audit trails, enforcing invariants, CDC/outbox; avoid for business logic (hidden side effects, cascading triggers, hard to test)*

**281. How are rows stored physically, and how do tables relate?**
   🔑 *Row stored in a fixed-size page/block (8KB PG, 16KB InnoDB) in a heap/segment file; located by ctid/RID (page+slot) or, in InnoDB, in the clustered PK B-tree leaf; secondary index stores key + pointer (ctid/PK); relations are logical via FK values, not physical pointers; TOAST/overflow for large values*

**282. Explain MVCC, WAL and VACUUM (Postgres).**
   🔑 *MVCC = each transaction sees a snapshot; writers don't block readers (old row versions kept with xmin/xmax); WAL (Write-Ahead Log) = durability + crash recovery + replication source; dead tuples accumulate → VACUUM reclaims space + updates visibility map, ANALYZE refreshes stats, autovacuum tuning; bloat + transaction-id wraparound risks*

**283. Explain database replication and read replicas.**
   🔑 *Primary-replica (streaming/logical) replication; read replicas offload read traffic (eventual consistency → replication lag → read-your-writes issues); synchronous (durable, slower) vs asynchronous (fast, data-loss window); automatic failover/promotion (Patroni), split read/write at the app via `AbstractRoutingDataSource`; multi-primary caveats*

**284. Partitioning vs sharding — what's the difference?**
   🔑 *Partitioning = split one table within a DB by range/list/hash (pruning, easier archival, same node); sharding = horizontal split across nodes by a shard key (scale-out); sharding costs = cross-shard joins/transactions, resharding, global unique ids (Snowflake), fan-out; consistent hashing to limit movement; pick a shard key with even distribution + matches query pattern*

**285. How do you handle multiple databases in a single service?**
   🔑 *Multiple `DataSource` beans (+`@Primary`), separate EntityManagerFactory/TransactionManager per source, `@EnableJpaRepositories(basePackages, entityManagerFactoryRef=...)`, separate entity/repo packages; no single transaction across them unless XA/JTA (avoid) → per-DB transactions + Saga/outbox for consistency; `AbstractRoutingDataSource` for read/write or tenant routing*

**286. Explain database normalization (and when to denormalize).**
   🔑 *1NF atomic values, 2NF no partial dependency on part of a composite key, 3NF no transitive dependency, BCNF stricter; benefits = fewer anomalies + redundancy; denormalize deliberately for read performance with a refresh/consistency strategy (materialized views, snapshot columns, CQRS read model)*

**287. What is a materialized view and when do you use it?**
   🔑 *A view whose result is physically stored (unlike a normal view); fast reads for expensive aggregations/joins; must be refreshed (`REFRESH MATERIALIZED VIEW [CONCURRENTLY]`) → serves slightly stale data; good for dashboards/reports; index it; alternative to denormalized read tables / CQRS*

**288. What is database sharding and how do you choose a shard key?**
   🔑 *Horizontal partitioning across nodes; strategies = range, hash, directory/lookup, geo; good key = even distribution, no hotspot, matches the main query; costs = cross-shard joins/tx, rebalancing, global ids, fan-out queries; consistent hashing to minimize data movement on resize*

**289. Explain MongoDB's aggregation pipeline.**
   🔑 *`$match` early (uses index) → `$project` → `$group` (`$sum`/`$avg`/`$push`) → `$sort` before `$limit` → `$lookup` (left join) → `$unwind` → `$facet`/`$addFields` → `$out`/`$merge`; order matters (filter first), `allowDiskUse` for large sorts, index the `$match`/`$sort` fields*

**290. When do you use Elasticsearch over SQL LIKE queries?**
   🔑 *Inverted index = term → posting list of doc ids via an analyzer (tokenize/lowercase/stem/stopwords); gives relevance scoring (BM25/TF-IDF), fuzzy/typo tolerance, facets, millisecond full-text; SQL `LIKE '%x%'` = full scan, no ranking; ES is near-real-time, NOT a system of record (sync via CDC/outbox)*

**291. What ORMs/SQL frameworks exist beyond JPA, and when would you pick them?**
   🔑 *JPA/Hibernate (ORM), Spring Data JPA, MyBatis (SQL mapper), jOOQ (typesafe SQL DSL), Spring JdbcTemplate, QueryDSL; pick jOOQ/MyBatis/JdbcTemplate when SQL is complex, performance-critical, or you want full control over the generated SQL*

**292. What is an RDBMS?**
   🔑 *Relational model = tables/rows/columns, keys (PK/FK/unique/composite), normalization, SQL, ACID transactions, constraints + referential integrity, schema-on-write; examples Postgres, MySQL, Oracle, SQL Server*

**293. How do you choose a database for a given requirement (decision framework)?**
   🔑 *Drive from: data shape + relationships (relational vs document vs graph), access pattern (point lookup, range, full-text, analytical), consistency needs (strong vs eventual), scale + write throughput (vertical vs horizontal/sharding), latency SLA, transactions, durability, operational maturity + cost; relational default for transactional integrity; specialized store when one dimension dominates; often polyglot persistence + CQRS read stores; don't pick by hype*

**294. Explain the NoSQL families and when to use each.**
   🔑 *Key-value (Redis/DynamoDB) = caching, sessions, simple fast lookups; Document (MongoDB) = flexible nested aggregates read together; Wide-column (Cassandra/HBase) = massive write throughput, time-series, known query patterns, tunable consistency; Graph (Neo4j) = relationship-heavy traversals (fraud, recommendations, social); Time-series (InfluxDB/Timescale) = metrics/IoT with retention + downsampling; Search (Elasticsearch) = full-text + relevance; model around access patterns, not normalization*

**295. OLTP vs OLAP — and data warehouse vs data lake.**
   🔑 *OLTP = many small low-latency transactions, normalized, row-store (Postgres/MySQL); OLAP = analytical aggregations over large scans, denormalized/star schema, column-store (Redshift/BigQuery/Snowflake/ClickHouse); warehouse = structured, schema-on-write, curated; lake = raw/any format, schema-on-read (S3 + Spark); lakehouse combines both; move OLTP→analytics via ETL/ELT + CDC, don't run heavy analytics on the OLTP DB*

**296. What is NewSQL / distributed SQL, and when would you use it?**
   🔑 *Distributed SQL (CockroachDB, Google Spanner, YugabyteDB, Citus, Vitess) = horizontal scale + SQL + ACID + strong consistency via consensus (Raft/Paxos) + automatic sharding/replication; use when you've outgrown a single-node RDBMS but still need transactions + joins + global consistency (multi-region); trade-off = higher write latency (cross-node consensus), cost, operational complexity*

**297. How do you make database access resilient (failover, retries, pool health)?**
   🔑 *Connection pool with `maxLifetime` < DB timeout + validation query/`keepaliveTime`; retry transient errors (deadlock, connection reset) with backoff (idempotent); fast-fail with `connectionTimeout`; read replicas + automatic primary failover (Patroni/RDS Multi-AZ) with DNS/endpoint switch; circuit breaker around the DB; `statement_timeout`/`lock_timeout`; health-check indicator; graceful degradation to cache when the DB is down*

[⬆️ Back to top](#contents)

## REST API Design & Web Layer

> *Full forms: **REST** = Representational State Transfer · **SOAP** = Simple Object Access Protocol · **gRPC** = gRPC Remote Procedure Calls (over HTTP/2 + Protobuf) · **CORS** = Cross-Origin Resource Sharing · **HATEOAS** = Hypermedia As The Engine Of Application State.*

**298. What are the core REST principles and the Richardson Maturity Model?**
   🔑 *REST (Representational State Transfer) = resources + URIs + HTTP verbs + representations (JSON), stateless, cacheable, uniform interface, layered, client-server; Richardson levels 0 (RPC-over-HTTP) → 1 (resources) → 2 (HTTP verbs + status codes) → 3 (hypermedia/HATEOAS); idempotency + safety per verb*

**299. Explain HTTP methods and status codes, including safe vs idempotent.**
   🔑 *GET (safe/idempotent), HEAD, OPTIONS, PUT (idempotent full replace), DELETE (idempotent), PATCH (partial, not necessarily idempotent), POST (neither); status 2xx success (200/201/202/204), 3xx redirect (301/302/304), 4xx client (400/401/403/404/409/422/429), 5xx server (500/502/503/504); safe ⇒ idempotent, not vice versa; you can't invent 6xx (IANA registry) — use a standard code + app error code in the body*

**300. How do you design idempotent APIs (and why it matters for payments)?**
   🔑 *Client-generated `Idempotency-Key` header → unique index on (key, endpoint) + stored request fingerprint + response; same key+payload replays the stored response, different payload → 409; TTL cleanup; naturally idempotent = PUT with client id / UPSERT / state-machine transition guarded by `WHERE status=?`; matters because network timeouts cause retries → double charge*

**301. How do you version a REST API and evolve the contract without breaking clients?**
   🔑 *URI (`/v1/`) = simple/visible/cacheable; header (`Accept-Version`) = clean URLs; media type (`application/vnd.x.v2+json`) = purest, poor tooling; version only on breaking change, additive-only/tolerant reader, consumer-driven contract tests (Pact/Spring Cloud Contract), N-1 support, `Sunset`/deprecation headers; separate controllers per version sharing a service; gateway routing*

**302. Can you use controller method overloading for versioning, and how do you handle a v2 that does extra work?**
   🔑 *No — Java overloading can't dispatch on version and Spring fails on ambiguous mappings; use distinct paths (`/v1`,`/v2`) or `headers=`/`produces=` on `@RequestMapping`, separate controllers, shared service; a v2 that makes an extra DB call enriches the response (new field is additive, old clients ignore it), v1 skips it; feature-flag + metrics until deprecation*

**303. Difference between PUT and PATCH.**
   🔑 *PUT = full replace, idempotent, missing fields reset; PATCH = partial update, not necessarily idempotent, JSON Merge Patch (RFC 7396) or JSON Patch (RFC 6902 op/path/value); PATCH needs careful null-vs-absent semantics (Optional/JsonNullable)*

**304. SOAP vs REST — differences, and why would payments still use SOAP?**
   🔑 *SOAP (Simple Object Access Protocol) = XML envelope, WSDL contract + codegen, WS-Security (message-level signing/encryption), WS-AtomicTransaction, protocol-agnostic, heavy, non-repudiation; REST = resources + HTTP verbs, JSON, stateless, lightweight, cacheable; payments/banking keep SOAP for legacy rails (ISO 20022, SWIFT), formal contracts, message-level security, regulatory/partner mandate; integrate legacy SOAP via JAX-WS/wsimport, Spring WS, Apache CXF*

**305. REST vs GraphQL vs gRPC — when would you choose each?**
   🔑 *REST = public APIs, simplicity, HTTP caching, ubiquitous tooling; GraphQL = client-driven queries (no over/under-fetch), one endpoint + schema + resolvers, but N+1 (DataLoader), caching/rate-limiting harder; gRPC (gRPC Remote Procedure Calls) = HTTP/2 + Protobuf (binary), 4 streaming modes (unary/server/client/bidi), deadlines, codegen, low latency for internal service-to-service, poor browser support (needs grpc-web/gateway)*

**306. What is the HTTP protocol, and what alternatives exist for client-server communication?**
   🔑 *HTTP = stateless request/response application protocol over TCP — methods, status codes, headers, caching, content negotiation; text-based, firewall/proxy-friendly, universal; HTTP/2 multiplexing, HTTP/3 over QUIC; alternatives = WebSocket (full-duplex), gRPC, Server-Sent Events (SSE), MQTT/AMQP, raw TCP/UDP*

**307. How do you notify the frontend that a long task is complete?**
   🔑 *Return 202 Accepted + job id + status endpoint; push via WebSocket (STOMP/SockJS), Server-Sent Events (one-way), long polling fallback, or a webhook callback; back it with a job-status table and a completion event (Kafka) consumed by a notification service*

**308. Explain CORS and common pitfalls.**
   🔑 *CORS (Cross-Origin Resource Sharing) relaxes the browser same-origin policy; server sends `Access-Control-Allow-Origin/Methods/Headers/Credentials`; preflight `OPTIONS` for non-simple requests; Spring `@CrossOrigin`/CorsConfigurationSource — register before the Security filter chain; never `*` with credentials; it's a browser-side control only (Postman/curl bypass it)*

**309. What is ResponseEntity and why return a DTO instead of a raw String?**
   🔑 *`ResponseEntity` = full control over status + headers + body (`ok`, `status(CREATED).header().body()`, `noContent`); return a typed DTO → serialized by Jackson (`application/json`), parsable, versionable, structured errors; a raw String → `text/plain`, breaks client parsing + content negotiation*

**310. Which RestTemplate methods have you used, and why does PATCH fail with it?**
   🔑 *`getForObject/Entity`, `postForObject/Entity`, `exchange` (full control + `ParameterizedTypeReference`), `put`, `delete`; PATCH fails with the default `SimpleClientHttpRequestFactory` (HttpURLConnection lacks PATCH) → use `HttpComponentsClientHttpRequestFactory` (Apache HttpClient) or WebClient; RestTemplate is in maintenance mode → prefer WebClient/RestClient*

**311. How do you handle partial failure when a request fans out to multiple downstream services?**
   🔑 *Decide per-call criticality (fail-fast vs degrade); aggregate with `CompletableFuture.allOf`/`Mono.zip` + per-call timeout + circuit breaker; return 207 Multi-Status or a partial payload with an errors block; compensating action/Saga for writes; retry only idempotent calls with backoff + jitter; never leave the client in an unknown state*

**312. How do you design pagination, filtering and sorting for a large collection endpoint?**
   🔑 *Offset = simple + random page access but O(N) scan, unstable under inserts; cursor/keyset = stable, index-friendly, O(1), opaque token of the last sort key, no jump-to-page; whitelist sortable/filterable fields (injection + index coverage), cap page size, return `hasNext` not exact totals*

**313. Explain HTTP caching (ETag, Cache-Control, 304).**
   🔑 *`Cache-Control` (max-age, no-store, private/public), `ETag` + `If-None-Match` / `Last-Modified` + `If-Modified-Since` → 304 Not Modified (saves bandwidth); strong vs weak ETag; CDN/reverse-proxy caching; also used for optimistic concurrency (ETag precondition `If-Match` → 412)*

**314. How do you design a consistent error response (RFC 7807)?**
   🔑 *`application/problem+json` (ProblemDetail: type, title, status, detail, instance + custom fields); stable app error codes, no stack traces/internal details, correlation/trace id, field-level validation errors; centralized via `@ControllerAdvice`; same envelope across all endpoints*

**315. How do you validate a request body across multiple API versions?**
   🔑 *Version-specific DTOs each with their own Bean Validation annotations (`@NotNull`/`@Valid`/groups), shared base for common fields, validation groups for version-specific rules, `@Validated` on the controller, custom `ConstraintValidator`; map all versions to one domain model and re-validate invariants there*

**316. APIs work in Postman but fail from the frontend — what's the likely issue?**
   🔑 *CORS preflight failure, missing/incorrect auth header or cookie (SameSite/HttpOnly/`credentials:'include'`), different base URL/port/proxy, HTTPS mixed content, Content-Type mismatch, missing CSRF token, gateway/WAF rule, caching — check browser devtools Network tab + server access logs*

**317. How do you document APIs (OpenAPI/Swagger)?**
   🔑 *OpenAPI 3 spec (springdoc-openapi / Swagger UI), annotate with `@Operation`/`@Schema`, generate contract-first or code-first, versioned docs, examples + error schemas, try-it-out; lock down/secure Swagger UI in prod; can generate clients/server stubs*

**318. Explain HATEOAS — is it practical?**
   🔑 *Hypermedia As The Engine Of Application State — responses embed `_links` (self, next, cancel) so clients navigate by links not hardcoded URLs; Spring HATEOAS, HAL; Richardson level 3; in practice rarely used — clients couple anyway, extra payload, limited tooling*

**319. Explain content negotiation and HttpMessageConverters.**
   🔑 *Client `Accept` header + server `produces`/`Content-Type` select a representation (JSON/XML/CSV); Spring picks an HttpMessageConverter (Jackson for JSON, Jackson-XML, etc.) to (de)serialize the body; `@RequestBody`/`@ResponseBody`; register custom converters for new media types*

**320. How do you design bulk / batch APIs?**
   🔑 *Accept an array/NDJSON payload with a cap on batch size; return 207 Multi-Status or a per-item result list with individual statuses (don't fail the whole batch on one bad item); idempotency key per item; async for large batches (202 + job id + status endpoint); pagination for bulk reads; backpressure + rate limits; transactional-per-item vs all-or-nothing decision stated explicitly*

**321. How do you design a long-running operation over REST?**
   🔑 *Return 202 Accepted + a job id + `Location` of a status resource; client polls the status endpoint (or subscribes via WebSocket/SSE/webhook); persist job state (PENDING/RUNNING/DONE/FAILED); make submission idempotent (idempotency key so retries don't start duplicate jobs); expose progress + result URL; set TTL/cleanup on completed jobs*

**322. How do you handle API security basics at the web layer (summary)?**
   🔑 *HTTPS/TLS, authN via OAuth2/JWT validated in a filter, deny-by-default authZ (URL + method security), input validation (`@Valid`) + output encoding, CORS allowlist, rate limiting, security headers (HSTS/CSP), no secrets in URLs/logs, lock down Swagger/actuator in prod — see the Security section for depth*

[⬆️ Back to top](#contents)

## Reactive Programming — WebFlux & Reactor

**323. Mono vs Flux — what are they and how do they differ?**
   🔑 *Reactor publishers: `Mono<T>` = 0 or 1 element (async single value/empty), `Flux<T>` = 0..N elements (async stream); both lazy (nothing runs until subscribed), support backpressure + operators; `Mono` for a single response, `Flux` for a stream/collection; convert with `Flux.from(mono)` / `mono.flux()` / `flux.collectList()`*

**324. Why is reactive lazy, and what does "nothing happens until you subscribe" mean?**
   🔑 *A pipeline is just a blueprint of operators; the data flows only when `subscribe()` is called (or the framework subscribes, e.g. WebFlux on the HTTP response); so side effects must be inside operators (`flatMap`/`doOnNext`), not before; `block()` subscribes + waits (avoid in reactive code); cold (per-subscriber) vs hot (shared) publishers*

**325. map vs flatMap vs concatMap in Reactor.**
   🔑 *`map` = synchronous 1:1 transform; `flatMap` = async 1:N, subscribes to inner publishers concurrently (order NOT preserved) — for calling another reactive service; `concatMap` = sequential, preserves order; `flatMapSequential` = concurrent but ordered output; `switchMap` = cancel previous on new emission; pick by ordering + concurrency needs*

**326. How does backpressure work in Reactive Streams?**
   🔑 *Consumer signals demand via `request(n)` (Reactive Streams spec: Publisher/Subscriber/Subscription/Processor); if the producer is faster → strategies `onBackpressureBuffer`/`Drop`/`Latest`/`Error`; `limitRate`, bounded buffers; natural fit for streaming; prevents the fast producer from overwhelming a slow consumer without blocking threads*

**327. WebFlux vs Spring MVC — when do you NOT use WebFlux?**
   🔑 *WebFlux = non-blocking Netty + Reactor, few event-loop threads handle high concurrency, needs a fully non-blocking stack (R2DBC, WebClient); avoid when the stack is blocking (JDBC), the team is unfamiliar, debugging/stack traces matter, or load is modest — MVC + virtual threads (Java 21) often gets similar scalability with simpler code*

**328. What is R2DBC and why can't you use JDBC in WebFlux?**
   🔑 *R2DBC = Reactive Relational Database Connectivity (non-blocking DB driver returning Mono/Flux); JDBC is blocking → a blocking call on an event-loop thread stalls many requests (thread starvation); if you must call blocking code in reactive, offload to `Schedulers.boundedElastic()`; R2DBC lacks some JPA features (no lazy loading/relationships) → more manual mapping*

**329. How do you handle errors in a reactive pipeline?**
   🔑 *`onErrorResume` (fallback publisher), `onErrorReturn` (fallback value), `onErrorMap` (translate exception), `onErrorContinue` (skip bad element — use cautiously), `retryWhen(Retry.backoff(...))`, `timeout`, `doOnError` (side effect); errors are terminal signals that propagate downstream; combine with Resilience4j reactive operators*

**330. What are subscribeOn and publishOn (Schedulers)?**
   🔑 *`subscribeOn` sets the thread for the WHOLE subscription (the source), placement-independent; `publishOn` switches the thread for operators DOWNSTREAM of it; Schedulers = `parallel` (CPU), `boundedElastic` (blocking I/O offload), `single`, `immediate`; by default reactive runs on the subscribing/event-loop thread — don't block it*

**331. How do you combine multiple reactive sources?**
   🔑 *`Mono.zip`/`Flux.zip` (combine latest pairwise, wait for all), `Mono.when` (wait for completion, ignore values), `flatMap` (dependent calls), `merge` (interleave as they arrive), `concat` (sequential), `Flux.combineLatest`; parallel fan-out to services then aggregate; `zipWith`/`zipDelayError`*

**332. How do you test reactive code?**
   🔑 *`StepVerifier.create(flux).expectNext(...).expectComplete().verify()` / `.expectError()`; `StepVerifier.withVirtualTime` for time-based operators (no real waiting); `WebTestClient` for WebFlux endpoints; `expectNextCount`, `thenAwait(Duration)`; avoid `block()` in tests — assert on signals*

**333. What are hot vs cold publishers, and what is the Reactor Context?**
   🔑 *Cold = each subscriber gets its own fresh execution from the start (HTTP call per subscribe); hot = emits regardless of subscribers, late subscribers miss earlier items (`share()`, `Sinks`, `ConnectableFlux`); Reactor Context = immutable key-value propagated UPSTREAM through the chain (replaces ThreadLocal for reactive) — carries security/trace/tenant context (`contextWrite`/`deferContextual`)*

**334. When should you choose reactive programming at all?**
   🔑 *High concurrency with I/O-bound work + a fully non-blocking stack (streaming, SSE, many slow downstream calls, websockets, API gateway); NOT for CPU-bound work, simple CRUD, or blocking dependencies; cost = steeper learning curve, harder debugging, viral (all-or-nothing non-blocking); with Java 21 virtual threads, imperative blocking code often scales comparably with far less complexity*

[⬆️ Back to top](#contents)

## Microservices Architecture & Resilience

**335. What actually justifies splitting a monolith into microservices, and what do you decompose first?**
   🔑 *Triggers = independent scaling, team autonomy (Conway's law), deploy coupling/slow releases, divergent tech needs, fault isolation — NOT "microservices are modern"; strangler-fig a bounded context that is read-heavy, low-coupling, high-change (notifications/reporting/auth) first; split code then data (keep a shared DB behind an API temporarily); measure with DORA metrics; start as a modular monolith*

**336. Microservices vs monolith — pros/cons, and why might you move BACK to a monolith?**
   🔑 *Microservices = independent deploy/scale, autonomy, fault isolation, tech diversity — but distributed complexity, network latency, eventual consistency, hard testing/debugging, ops cost, data duplication; monolith = simple, local calls, single transaction, easy testing — but all-or-nothing scaling, slow releases, large blast radius; go back when over-decomposed ("distributed monolith"), chatty sync calls, small team, Amazon Prime Video case → merge contexts that always change together*

**337. How do you handle distributed transactions across services (Saga — choreography vs orchestration)?**
   🔑 *No 2PC/XA in practice (blocking, poor availability); Saga = sequence of local transactions + compensating actions; choreography (events via Kafka, no central brain, loose coupling, hard to trace) vs orchestration (central orchestrator/state machine — Temporal, Camunda/Zeebe, AWS Step Functions, Netflix Conductor — easier to debug/monitor); compensations must be idempotent + commutative; persist saga state with timeouts; alternative = TCC (Try-Confirm-Cancel)*

**338. How do you revert/compensate an operation when a downstream service fails?**
   🔑 *No distributed rollback → compensating transactions (refund, release reservation, cancel), idempotent compensation handlers, persisted saga state with timeout detection, outbox to guarantee the compensation event is emitted, DLQ + manual remediation queue, and a reconciliation/audit job to catch stragglers*

**339. Explain the Outbox pattern — why is it needed with message publishing?**
   🔑 *Dual-write problem = DB commit succeeds but broker publish fails (or vice versa) → inconsistency; Outbox = write the event into an outbox table in the SAME local transaction, then a relay (polling publisher or CDC/Debezium) publishes to Kafka and marks sent; gives at-least-once + ordering; consumer must dedupe; alternative = transaction-log tailing*

**340. How do service discovery and load balancing work (Eureka/Consul vs Kubernetes-native)?**
   🔑 *Client-side (Eureka + Spring Cloud LoadBalancer): registry, heartbeat/lease, client caches registry + picks instance, self-preservation (AP); Consul adds health checks + KV + DNS (CP via Raft); Kubernetes-native: Service + EndpointSlice, cluster DNS, kube-proxy (iptables/IPVS), readiness probes gate endpoints, headless service for pod IPs; on K8s you usually don't need Eureka*

**341. How do you balance load evenly and keep services stateless?**
   🔑 *LB algorithm (round-robin, least-connections, least-request), client-side LB with instance refresh, K8s Service/mesh LB; avoid sticky sessions (keep services stateless — externalize session to Redis); beware long-lived HTTP/2 connections pinning traffic (periodic reconnect / mesh); Kafka partition assignment for consumers; verify with per-instance RPS metrics*

**342. What does an API Gateway do, and how does it route to services?**
   🔑 *Single entry point: routing/path rewrite, authn/authz + token validation, rate limiting/quotas, request aggregation/BFF, protocol translation, TLS termination, caching, CORS, canary/traffic-splitting, correlation IDs + tracing; routes = static config (Spring Cloud Gateway predicates: Path/Host/Method/Header) or dynamic from registry/K8s Ingress; reactive non-blocking (Spring Cloud Gateway); keep business logic out and avoid it being a SPOF*

**343. Explain the circuit breaker pattern (Resilience4j vs Hystrix) and how you tune it.**
   🔑 *Prevents cascading failure/resource exhaustion — fail fast instead of piling up threads; states CLOSED → OPEN → HALF_OPEN; Resilience4j = lightweight functional decorators, sliding window (count/time); Hystrix = thread-pool isolation, maintenance mode; tune failureRateThreshold (~50%), slowCallRateThreshold + slowCallDurationThreshold, minimumNumberOfCalls (don't trip on tiny samples), waitDurationInOpenState, permittedCallsInHalfOpen, recordExceptions/ignoreExceptions; Micrometer metrics on state changes*

**344. Explain the full resilience toolkit — timeout, retry, bulkhead, rate limiter, fallback — and their order.**
   🔑 *Timeout/TimeLimiter FIRST (a breaker without a client timeout never counts the hung call), retry with exponential backoff + jitter (idempotent only), circuit breaker, bulkhead (semaphore/thread-pool isolation so one dependency can't starve the rest), rate limiter/load shedding, fallback (cached/default/queue-for-later); Resilience4j order = Retry(CircuitBreaker(RateLimiter(TimeLimiter(Bulkhead(call)))))*

**345. How do you handle a slow or failing dependent service without taking yourself down?**
   🔑 *Aggressive timeout so slowness becomes fast failure, slowCallRateThreshold so the breaker opens on latency not just errors, bulkhead to cap concurrent calls, serve cached/last-known-good or degraded response, make the call async (202 + callback/event), define hard vs soft dependencies, retry only idempotent calls, alert on dependency error budget*

**346. How do microservices communicate — sync vs async, with real examples?**
   🔑 *Sync = REST/gRPC/GraphQL (immediate result, temporal coupling) — payment authorization, login, price/inventory lookup; async = Kafka/RabbitMQ/SQS events or commands (decoupled, buffered, replayable, eventual consistency) — order fulfillment workflow, emails/notifications, invoice/report generation, analytics, cache warming; pick async for workflows, sync for queries*

**347. Event-driven architecture vs request-response — trade-offs.**
   🔑 *Request-response = caller waits, tight temporal coupling, easy to reason about, failure propagates; event-driven = publish facts, consumers react independently, loose coupling, buffering/replay, scales fan-out — but harder debugging, ordering, eventual consistency, duplicate handling; often hybrid*

**348. Design an order + payment + inventory microservice workflow (sync REST vs async Kafka).**
   🔑 *Order service owns the saga: sync REST only for fast validation/reads, async Kafka events for the workflow (OrderCreated → PaymentRequested → PaymentCompleted/Failed → InventoryReserved → OrderConfirmed); outbox for reliable publishing, idempotency keys, compensating events (refund/release stock), state machine on the order, DLQ + retry; sync couples availability+latency, async gives resilience but eventual consistency*

**349. How do you ensure high availability and fault tolerance?**
   🔑 *Redundancy across AZs, stateless services + horizontal autoscaling, health probes, timeouts/retries/circuit breakers/bulkheads, graceful degradation + fallback, async buffering with queues + DLQ, idempotency, DB replicas + failover, rate limiting/load shedding, chaos testing, DR plan (RTO/RPO), blue-green/canary, SLOs + error budgets*

**350. How does one service detect that another has crashed?**
   🔑 *Health checks/heartbeats — K8s liveness/readiness probes remove the pod from Service endpoints, Eureka lease expiry, Consul health check; at call time connection-refused/timeout → circuit breaker opens; plus metric alerts, Kafka consumer-group rebalance, distributed-tracing gaps*

**351. Database-per-service vs shared database — how do you decide?**
   🔑 *Database-per-service is the default — independent schema evolution/scaling/failure isolation, no hidden coupling; shared DB only as a temporary migration step or a tightly-coupled legacy pair (deploy coupling + contention); engine per access pattern (Postgres transactional, Mongo documents, Redis cache, Elasticsearch search); cross-service data via API, events/CDC, or a read-model projection*

**352. What is CQRS, what are its drawbacks, and how do you keep read/write consistent?**
   🔑 *CQRS (Command Query Responsibility Segregation) = separate write model (normalized, transactional) from read model (denormalized projections), often with event sourcing; drawbacks = eventual-consistency lag, duplicate storage, projection rebuild complexity, more moving parts; consistency via read-your-writes routing, version/sequence tokens, optimistic UI, idempotent + ordered projection handlers, lag monitoring, replay to rebuild*

**353. What is event sourcing, and when do you use it?**
   🔑 *Persist state as an append-only sequence of immutable events (not current state); rebuild state by replaying, snapshots for speed; gives full audit/time-travel + natural CQRS source; costs = event schema evolution, replay cost, eventual consistency, steep learning curve; fits ledgers/audit-heavy domains, not simple CRUD*

**354. How do you make consumers idempotent / achieve exactly-once effects across services?**
   🔑 *At-least-once delivery + idempotent consumer = dedupe table/cache keyed by event/business id (unique constraint), conditional UPSERT, state-machine guarded by current status, Redis SETNX pre-check; store processed id transactionally with the business write; outbox on the producer; never rely on broker exactly-once for external side effects*

**355. What is a service mesh and the sidecar pattern?**
   🔑 *Sidecar = helper container in the same pod sharing network/volume (proxy/mTLS via Envoy, log/metric shipping, config sync), language-agnostic, independently upgradable; service mesh (Istio/Linkerd) = control plane + sidecar data plane handling mTLS, traffic shaping, retries/timeouts, canary, observability — moves cross-cutting concerns out of app code; cost = extra resources + latency + operational complexity*

**356. How do you implement distributed tracing and correlation across services?**
   🔑 *Propagate a trace id + span id (W3C `traceparent`) via headers; Micrometer Tracing / OpenTelemetry → Zipkin/Jaeger/Tempo; correlation id in logs (MDC) + structured logging; sampling; tie traces + metrics + logs together; essential for debugging latency and partial failures in a request chain*

**357. What is the Anti-Corruption Layer (ACL) pattern?**
   🔑 *A translation layer (DDD) between your bounded context and a legacy/external model so their model doesn't leak into yours; adapter/facade + DTO mapping; isolates change, prevents a bad upstream schema from corrupting your domain; heavily used in strangler-fig migrations*

**358. What is a Backend-For-Frontend (BFF)?**
   🔑 *A dedicated API layer per client type (web/mobile/partner) that aggregates + shapes downstream responses, reduces round trips and over-fetching, owns client-specific logic/auth; avoids a bloated generic gateway; trade-off = more services + some duplication; GraphQL can play the same role (API composition/aggregator pattern)*

**359. Feign client vs RestTemplate vs WebClient.**
   🔑 *Feign = declarative interface client, integrates with discovery/LoadBalancer/Resilience4j, less boilerplate but reflective + blocking; RestTemplate = imperative blocking, maintenance mode; WebClient = non-blocking reactive (also usable blocking), best timeout/streaming control; RestClient = modern fluent blocking; Feign for many simple internal calls, WebClient for reactive/streaming/fine control*

**360. How do you reduce and handle latency in a microservices call chain?**
   🔑 *Sources = extra hops, serialization, N+1 service calls, cold start, GC; fix with parallel/async calls (CompletableFuture/WebClient), caching, batching/aggregation (BFF), gRPC/Protobuf, connection pooling + keep-alive, co-location, per-hop timeout budgets; measure p95/p99 (not averages) with distributed tracing*

**361. How do you handle a large file upload in a service?**
   🔑 *Stream it (don't buffer in memory): multipart streaming, chunked/resumable, `StreamingResponseBody`/`InputStreamResource`; better — pre-signed S3 URLs so the client uploads directly and the service only gets a notification; validate size/content-type, virus scan, async processing via event, backpressure (WebFlux), raise multipart limits deliberately*

**362. How do you decide the number of instances of a service?**
   🔑 *Peak RPS ÷ sustainable RPS per instance (from load tests) + headroom for spikes and rolling deploys; minimum 2-3 for HA across AZs; HPA on CPU/memory/custom metrics (RPS, queue lag) with min/max; bounded by DB connection pool, downstream rate limits, Kafka partition count; cost review*

**363. Explain the leader-follower pattern and leader election.**
   🔑 *One leader handles writes/coordination, followers replicate + can serve reads; single ordering point, avoids split-brain, failover via consensus (Raft/ZAB/Paxos) or election (ZooKeeper/etcd, Kafka controller); used in Kafka partitions, DB replication, and singleton scheduled jobs across instances (ShedLock)*

**364. List the key microservice design patterns and the main difficulties.**
   🔑 *Patterns = decomposition (by capability/subdomain, strangler-fig), database-per-service, Saga, CQRS, event sourcing, Outbox/CDC, API Gateway, BFF/Aggregator, service discovery, circuit breaker, bulkhead, retry, sidecar/ambassador, ACL, service mesh, health-check API, externalized config, blue-green/canary; difficulties = distributed debugging/tracing, data consistency, partial failure, testing (contract/e2e), versioning, infra + observability cost, service sprawl, local dev, security surface*

**365. Synchronous vs asynchronous service communication — how do you decide?**
   🔑 *Sync (REST/gRPC) when the caller needs an immediate result and the operation is fast + the dependency is highly available (queries, validation, auth) — but creates temporal coupling + latency addition + cascading failure risk; async (events/commands via Kafka/SQS) for workflows, fan-out, decoupling, buffering, resilience — but eventual consistency + ordering + duplicate handling; decide by: need-answer-now?, coupling tolerance, availability, consistency requirement; often sync for reads, async for state-changing workflows*

**366. How do you manage centralized configuration and dynamic refresh across services?**
   🔑 *Spring Cloud Config Server (Git-backed, per-service + per-profile files) or Consul/etcd KV / K8s ConfigMaps; `@ConfigurationProperties` + `@RefreshScope` + `/actuator/refresh` (Spring Cloud Bus to broadcast); secrets from Vault/Secrets Manager; feature flags (LaunchDarkly/Unleash/Flagsmith) for runtime toggles + progressive rollout + kill switches; version config in Git with review; precedence + environment promotion*

**367. How do you handle multi-tenancy in a microservice?**
   🔑 *Isolation models: database-per-tenant (strong isolation, costly), schema-per-tenant, or shared schema + tenant_id column (cheap, needs row-level enforcement); resolve tenant from JWT claim/subdomain/header → propagate via context (ThreadLocal/ScopedValue) → Hibernate filter / RLS (Row-Level Security) / routing datasource; per-tenant rate limits + noisy-neighbour isolation; tenant-aware caching keys*

[⬆️ Back to top](#contents)

## Kafka, Messaging & Event-Driven Architecture

**368. Explain event-driven architecture with Kafka — partitioning, consumer-group rebalancing, and exactly-once vs at-least-once.**
   🔑 *Partition key = entity id (orderId/accountId) for ordering + even spread, avoid hot keys, partitions only grow (adding them breaks key→partition mapping); rebalancing = group coordinator reassigns on join/leave/timeout, cooperative-sticky + static membership (`group.instance.id`) to reduce churn, `max.poll.interval.ms` for slow processing; EOS = idempotent producer + transactions (`read_committed`) within Kafka; across external systems use at-least-once + idempotent consumer*

**369. Explain Kafka core concepts — topic, partition, offset, consumer group, replication factor.**
   🔑 *Topic = named log; partition = ordered, immutable, append-only sequence (unit of parallelism + ordering); offset = per-partition position, committed per consumer group; consumer group = subscribers sharing a subscription, each partition to exactly one member; replication factor = copies across brokers (leader + ISR followers); retention by time/size, log compaction by key*

**370. How does Kafka guarantee message ordering, and how do you preserve it?**
   🔑 *Only within a partition — same key → same partition via `murmur2(key) % partitions`; keep `max.in.flight.requests.per.connection ≤ 5` with idempotence (or 1 without) so retries don't reorder; single consumer per partition; no parallel/async processing inside the consumer unless you key-shard the worker pool; ordering breaks if you add partitions or use retry topics*

**371. At-least-once vs at-most-once vs exactly-once delivery — how do you achieve each?**
   🔑 *At-most-once = commit offset before processing (loss on crash); at-least-once = process then commit (duplicates — common default, needs idempotent consumer); exactly-once = idempotent producer + transactions + `isolation.level=read_committed`, or read-process-write committing offsets inside the producer transaction (`sendOffsetsToTransaction`); EOS is only within Kafka — external side effects still need idempotency*

**372. How do you configure durability (acks, replication, ISR, min.insync.replicas)?**
   🔑 *`acks=all` + `min.insync.replicas=2` (with replication factor 3) = no data loss on single-broker failure; ISR (In-Sync Replicas) = replicas caught up to the leader; `enable.idempotence=true` (default) prevents producer-retry duplicates; `unclean.leader.election=false` prevents electing a stale replica; trade durability vs latency*

**373. How do you handle duplicate messages (producer and consumer side)?**
   🔑 *Producer: `enable.idempotence=true` (acks=all, retries) stops broker-side retry duplicates; transactions for read-process-write; Consumer: make it idempotent — dedupe table/cache keyed by event/business id with a unique constraint, conditional UPSERT, state-machine guarded by status, Redis SETNX; store processed id/offset transactionally with the business write*

**374. How do you design retries and a dead-letter queue so failures don't vanish or infinite-loop?**
   🔑 *Classify retryable (transient → backoff) vs non-retryable (deserialization/validation → straight to DLT); bounded in-memory retries (DefaultErrorHandler + ExponentialBackOff), then non-blocking retry topics with increasing delay (`@RetryableTopic`), finally a DLQ topic with original headers (exception, stack, source topic/partition/offset, attempt count); ErrorHandlingDeserializer for poison pills; alert on DLQ depth + lag; provide a replay tool; never swallow or auto-commit past a failure*

**375. Kafka vs RabbitMQ — when to use which?**
   🔑 *Kafka = distributed commit log, pull-based, retention + replay, high throughput, partitioned ordering, consumer groups, stream processing → event sourcing/analytics/fan-out; RabbitMQ = broker with exchanges/queues/bindings, push-based, flexible routing (direct/topic/fanout/headers), per-message ack/priority/TTL/delay, lower per-message latency → task queues + RPC; Kafka messages survive consumption, Rabbit's are deleted*

**376. Explain the partition ↔ consumer ↔ group relationship (e.g. 10 partitions, 20 consumers).**
   🔑 *Within a group each partition goes to exactly ONE consumer, but one consumer can own many partitions; with 20 consumers + 10 partitions, 10 consume and 10 sit idle (hot standbys for failover) — parallelism is capped by partition count; multiple groups each get a full copy (fan-out); increasing partitions changes key→partition mapping and breaks existing per-key ordering*

**377. Explain consumer rebalancing and how to reduce its impact.**
   🔑 *Triggered on join/leave/crash, missed heartbeats, `max.poll.interval.ms` exceeded, or partition-count change; coordinator + leader run the assignor (range/round-robin/sticky/cooperative-sticky); eager = stop-the-world revoke-all, cooperative = incremental; reduce impact with static membership, longer `max.poll.interval`, smaller `max.poll.records`, cooperative-sticky assignor; commit offsets in `onPartitionsRevoked`*

**378. What are the offset commit strategies?**
   🔑 *`enable.auto.commit` (risky — commits on a timer regardless of processing) vs manual (`enable.auto.commit=false` + ack); commit AFTER processing for at-least-once; Spring AckMode (RECORD/BATCH/MANUAL/MANUAL_IMMEDIATE); `auto.offset.reset` (earliest/latest/none) when no committed offset; store offsets with the business write for exactly-once effect*

**379. How do you schema-evolve Kafka messages with a Schema Registry?**
   🔑 *Avro/Protobuf + Confluent Schema Registry (serializer registers + prefixes payload with a schema id, consumer fetches by id); compatibility BACKWARD (add optional fields with defaults — roll out consumers first) / FORWARD / FULL / TRANSITIVE; never remove/rename/retype a required field; validate compatibility in CI; tolerant reader; subject naming strategy*

**380. How do you troubleshoot a consumer that isn't consuming?**
   🔑 *Check group assignment + lag (`kafka-consumer-groups --describe`), correct topic/partition/bootstrap servers, `auto.offset.reset=latest` skipping old messages, offsets already committed past the data, changed `group.id`, stuck in rebalance, `max.poll.interval.ms` exceeded → kicked out, deserialization exception killing the container (poison pill blocking the partition), ACL/auth failure, broker/leadership connectivity, container not started*

**381. How do you get a response back in Kafka (request/reply), e.g. payment → invoice → generate?**
   🔑 *Kafka is fire-and-forget → `ReplyingKafkaTemplate` with a reply topic + correlation id header, OR model it as a chained event flow (payment-completed → invoice-service → invoice-generated); for the caller return 202 Accepted + job id and push completion via WebSocket/SSE/webhook; a saga/orchestrator holds the state machine*

**382. What are the bare-minimum and production configs for a consumer and producer?**
   🔑 *Consumer min: `bootstrap.servers`, `group.id`, key/value deserializer — prod adds `auto.offset.reset`, `enable.auto.commit=false`, `max.poll.records`, `max.poll.interval.ms`, session/heartbeat timeouts, `isolation.level`, assignment strategy; Producer: `bootstrap.servers`, serializers, `acks`, `retries`, `enable.idempotence`, `linger.ms`, `batch.size`, `compression.type`, `max.in.flight.requests`, `transactional.id`*

**383. Where do you configure topics, partitions and settings?**
   🔑 *Topic creation via AdminClient/CLI/Terraform or Spring `NewTopic` @Bean (disable auto-create in prod); partitions + replication factor at creation; broker defaults in `server.properties`; per-topic overrides (`retention.ms`, `cleanup.policy`, `min.insync.replicas`) via `kafka-configs`; client settings in `application.yml` (`spring.kafka.*`) or Producer/ConsumerFactory maps*

**384. Explain the Kafka Streams API.**
   🔑 *Stream-processing library (not a cluster) — KStream/KTable/GlobalKTable DSL + Processor API, stateless (map/filter/branch) + stateful (aggregate/count/join), windowing (tumbling/hopping/sliding/session), RocksDB state stores backed by changelog topics, repartition topics on key change, exactly-once-v2, interactive queries, standby replicas; alternative to Flink/Spark for Kafka-native pipelines*

**385. How do you publish a message to a topic, and is send() sync or async?**
   🔑 *`KafkaTemplate.send(topic, key, payload)` / `ProducerRecord` with headers; topic externalized in config, key chosen for partitioning/ordering, headers for correlation/trace/schema version; `send()` is ASYNCHRONOUS — buffers into the RecordAccumulator and returns a Future immediately, batched by `linger.ms`/`batch.size`; `.get()` makes it sync (throughput collapse) or use the callback; prefer transactional outbox publishing for critical events*

**386. How do you republish/replay a message to a topic?**
   🔑 *Re-send the original payload with its key (ordering) to the target/retry topic, carrying forward headers + an attempt counter; use a DLT replay consumer or admin tool; cap attempts to avoid infinite loops; consumers must be idempotent since replay creates duplicates*

**387. Why do we have partitions, and what are the costs of more partitions?**
   🔑 *Partition = unit of parallelism, ordering, and storage distribution; more partitions → more consumer concurrency + throughput, spread across brokers; key keeps related events ordered; costs = more open file handles/memory, longer leader election + rebalance, higher end-to-end latency, more replication overhead; partition count can only increase*

**388. Explain log compaction and retention.**
   🔑 *`cleanup.policy=delete` = retention by `retention.ms`/`retention.bytes` (event streams); `cleanup.policy=compact` = keep the latest value per key (changelog/snapshot semantics, enables rebuilding state/KTable, tombstone = null value deletes a key); can combine `compact,delete`; tiered storage offloads old segments to object storage*

**389. How do you monitor consumer lag and Kafka health?**
   🔑 *Consumer lag = log-end-offset − committed-offset per partition (`kafka-consumer-groups`, Burrow, Kafka Exporter → Prometheus/Grafana); alert on growing lag; monitor under-replicated partitions, ISR shrink, broker disk/CPU, request latency, rebalance frequency; lag spike = slow consumer / poison pill / downstream slowness*

**390. Report generation — sync vs async with Kafka.**
   🔑 *Return 202 Accepted + report id, publish a report-requested event; a worker generates asynchronously (Spring Batch/chunked query), uploads to S3, publishes report-ready with a pre-signed URL; notification service pushes to the user; keep status in a jobs table, retry + DLQ, idempotent by report id; sync only for tiny reports with a hard timeout*

**391. What is KRaft and how does it change Kafka (ZooKeeper removal)?**
   🔑 *KRaft (Kafka Raft) replaces ZooKeeper for metadata/controller management → self-managed quorum, faster failover, higher partition scalability, simpler ops (one system); metadata stored in an internal Kafka log; default in recent Kafka versions*

**392. How do you configure Spring Kafka listener concurrency and error handling?**
   🔑 *`ConcurrentKafkaListenerContainerFactory` with `concurrency` = number of consumer threads (≤ partitions); manual ack (`AckMode.MANUAL`/`RECORD`) + `enable-auto-commit=false`; `DefaultErrorHandler` with backoff + `addNotRetryableExceptions` → DLT via `DeadLetterPublishingRecoverer`; `ErrorHandlingDeserializer` for poison pills; batch listeners for throughput; pause/resume container for downstream outages; `@RetryableTopic` for non-blocking retries*

**393. What are Kafka Connect and CDC, and when do you use them over custom producers/consumers?**
   🔑 *Kafka Connect = config-driven framework of source/sink connectors (DB→Kafka, Kafka→S3/Elasticsearch) with SMTs (Single Message Transforms), scaling + offset management built in; CDC (Change Data Capture, Debezium) streams DB row changes from the transaction log → Kafka → powers the outbox/replication/search-sync without dual writes; use Connect/CDC for integration + data movement, custom consumers for business logic*

[⬆️ Back to top](#contents)

## Security — AuthN/AuthZ, OWASP, TLS & Secrets

> *Full forms: **JWT** = JSON Web Token · **OAuth** = Open Authorization · **TLS/SSL** = Transport Layer Security / Secure Sockets Layer · **mTLS** = mutual TLS · **CSRF** = Cross-Site Request Forgery · **XSS** = Cross-Site Scripting · **SQLi** = SQL Injection · **OWASP** = Open Worldwide Application Security Project · **PCI-DSS** = Payment Card Industry Data Security Standard · **CSP** = Content Security Policy · **RBAC/ABAC** = Role/Attribute-Based Access Control · **KMS** = Key Management Service · **MFA** = Multi-Factor Authentication.*

**394. Explain the Spring Security filter chain end-to-end for a JWT-based API.**
   🔑 *DelegatingFilterProxy → FilterChainProxy → matching SecurityFilterChain; order = SecurityContextHolderFilter, CorsFilter, CsrfFilter (disabled for stateless), LogoutFilter, your `OncePerRequestFilter` JWT filter (parse Bearer → validate signature/exp/iss/aud → build Authentication with authorities → SecurityContextHolder), BearerTokenAuthenticationFilter (resource server), AnonymousAuthenticationFilter, ExceptionTranslationFilter (401 entry point / 403 access-denied handler), AuthorizationFilter (authorizeHttpRequests rules); `SessionCreationPolicy.STATELESS`; supporting beans AuthenticationManager → AuthenticationProvider → UserDetailsService + PasswordEncoder*

**395. Access token vs refresh token — how do you handle revocation for stateless JWT?**
   🔑 *Access = short-lived (5-15 min), stateless, sent on every call; refresh = long-lived, stored in an HttpOnly+Secure+SameSite cookie, used only at `/token`; revocation = jti denylist in Redis (TTL = remaining exp), token-version/epoch per user bumped on logout/password change, rotate refresh tokens with reuse detection (replay → revoke the family), short access TTL; true instant revocation needs introspection (back to stateful)*

**396. Explain OAuth2 grant types — which to use when, and why is implicit deprecated?**
   🔑 *Authorization Code + PKCE for user-facing web/mobile/SPA; Client Credentials for machine-to-machine (no user); Refresh Token for silent renewal; Device Code for TVs/CLI; Implicit deprecated (token in URL fragment → leaks via history/referrer/logs, no client auth, no refresh); Resource Owner Password deprecated (app sees password, breaks MFA/SSO)*

**397. Explain JWT structure, and how it is verified without a DB call. Is it encrypted?**
   🔑 *`Base64url(header).Base64url(payload).signature`; header = alg + kid, payload = claims (iss, sub, aud, exp, iat, nbf, jti + roles/scopes); verified by recomputing HMAC with the shared secret or checking RSA/EC signature with the public key from the JWKS endpoint (cached by kid) — identity + authorities are inside the signed token; a plain JWS is only ENCODED + signed (readable) → never put secrets/PII in it; validate exp/iss/aud/alg (reject `none` + algorithm confusion); use JWE if you need encryption*

**398. How do you prevent SQL injection, XSS and CSRF — and how does CSRF differ for session vs stateless APIs?**
   🔑 *SQLi → parameterized/bind queries (JPA, JdbcTemplate `?`), never concatenate, whitelist dynamic identifiers (sort/table), least-privilege DB user; XSS → contextual output encoding, Content-Security-Policy, OWASP HTML sanitizer, HttpOnly cookies; CSRF → session apps need a synchronizer token (attacker can't read it, same-origin) + SameSite cookies + Origin/Referer check (browser auto-attaches the session cookie → forged state-changing call); stateless JWT-in-header APIs aren't CSRF-prone (header not auto-sent), but JWT-in-cookie is*

**399. Name the OWASP Top 10 and key mitigations.**
   🔑 *A01 Broken Access Control (deny-by-default, server-side authz, no IDOR), A02 Cryptographic Failures (TLS, strong hashing), A03 Injection (parameterized + encoding), A04 Insecure Design (threat modelling), A05 Security Misconfiguration (hardened defaults, lock down actuator), A06 Vulnerable Components (SCA/Dependabot/OWASP Dependency-Check), A07 Auth Failures (MFA, lockout), A08 Integrity Failures (signed artifacts, safe deserialization), A09 Logging/Monitoring Failures, A10 SSRF (allowlist outbound, block cloud metadata endpoint)*

**400. How do you secure a REST API end-to-end (steps + layers)?**
   🔑 *(1) TLS everywhere + HSTS, (2) choose model (stateless JWT/OAuth2 resource server vs session), (3) IdP (Keycloak/Cognito/Okta) issues tokens via Auth Code+PKCE / client creds, (4) validate token in a filter → SecurityContext, (5) SecurityFilterChain: stateless, CORS allowlist, CSRF off for token APIs, deny-by-default rules, (6) method + object-level authz (roles/scopes), (7) refresh rotation + revocation, (8) rate limiting, input validation, security headers, (9) lock down actuator/Swagger, (10) audit logging + monitoring + dependency scanning, (11) secrets in Vault; layered defence = WAF/DDoS → TLS/mTLS → gateway authn + rate limit → service authz → validation → least privilege*

**401. How do you secure service-to-service communication?**
   🔑 *Defence in depth: mTLS for transport identity (mesh-issued + auto-rotated certs, SPIFFE/SPIRE IDs) + OAuth2 client-credentials or signed service JWTs with audience/scope checks per hop + authorization policy in EACH service (never trust "inside the cluster") + network policies/zero-trust + propagate end-user context in a signed token for downstream authz + short-lived credentials, rotation, audit logging, rate limiting between services*

**402. Explain the complete authentication + authorization flow with class- and method-level access.**
   🔑 *Authentication (who you are) = filter validates credentials/JWT → Authentication with authorities in SecurityContextHolder; authorization (what you may do) = URL rules (`authorizeHttpRequests`) then method security `@PreAuthorize("hasRole('ADMIN') and #id==authentication.principal.id")`/`@PostAuthorize`/`@PostFilter` (`@EnableMethodSecurity`) then domain-object/ownership checks; roles vs authorities/scopes, deny-by-default, 401 unauthenticated vs 403 unauthorized, custom AuthenticationEntryPoint + AccessDeniedHandler*

**403. RBAC vs ABAC — how do you model authorization?**
   🔑 *RBAC (Role-Based) = permissions grouped into roles assigned to users — simple, coarse, can explode with combinations; ABAC (Attribute-Based) = policy over subject/resource/action/environment attributes (e.g. owner, department, time) — fine-grained, dynamic; often hybrid (roles + attribute conditions in `@PreAuthorize` SpEL); externalize complex policy (OPA/Cerbos); always enforce object-level ownership to prevent IDOR*

**404. How do you store passwords and other sensitive data in a database?**
   🔑 *Passwords = HASHED (never encrypted/plaintext) with BCrypt/Argon2id/SCrypt + per-user salt + tuned work factor (Spring Security PasswordEncoder, DelegatingPasswordEncoder for upgrades); other sensitive fields = AES-GCM envelope encryption with KMS/Vault-managed keys + rotation, deterministic/blind index only where you must search, column-level/TDE at rest, mask in logs + lower environments, least-privilege DB access + audit*

**405. How do you keep PCI-DSS data (card/CVV) out of logs (tokenization/masking)?**
   🔑 *Never log full PAN/CVV — CVV must never be STORED; tokenize via a vault/payment provider (app handles tokens only), mask to first6+last4, custom Logback masking converter + Jackson `@JsonIgnore`/serializer, keep card fields out of `toString` (avoid Lombok `@Data` on card models), scrub MDC + exception messages, field-level/KMS encryption at rest, restrict log access + retention, scan logs in CI, segment to limit PCI scope*

**406. How do you implement rate limiting (token bucket vs sliding window), and where?**
   🔑 *Token bucket = burst-friendly (refill rate + capacity); leaky bucket smooths; fixed window = simple but boundary spikes; sliding-window log = accurate but memory-heavy; sliding-window counter = good compromise; enforce at the gateway/edge first (cheapest) + defensively in-app for business quotas; distributed counters in Redis (INCR+EXPIRE / Lua for atomicity), key by API key/user/IP; return 429 + Retry-After + X-RateLimit-* headers; add WAF + load shedding*

**407. How do you manage secrets and centralized configuration across services?**
   🔑 *Non-secrets = Spring Cloud Config (Git-backed, `@RefreshScope`, Spring Cloud Bus), Consul/etcd KV, K8s ConfigMap; secrets = Vault (dynamic DB creds, leases, auto-rotation, transit encryption), AWS Secrets Manager/Parameter Store (IAM/IRSA), K8s Secret (enable encryption at rest + RBAC; base64 ≠ encryption) or External Secrets Operator; inject via env/mounted file, never in git/images/logs, rotate, least privilege, audit, scan repos (gitleaks/trufflehog)*

**408. How do you prevent insecure deserialization in Java?**
   🔑 *Never deserialize untrusted data with native Java serialization; prefer JSON/Protobuf with strict typing; disable Jackson polymorphic typing on untrusted input (no `enableDefaultTyping`/`@JsonTypeInfo` — gadget chains), allowlist deserializable types, use `ObjectInputFilter` (JEP 290 serialFilter), validate after deserialization, patch libraries, avoid gadget-rich libs on the classpath*

**409. Input validation vs output encoding — what's the difference?**
   🔑 *Input validation = reject/constrain untrusted data at the boundary by type/length/format/range/allowlist (Bean Validation `@Valid`, custom validators) → integrity + business rules; output encoding = escape data for its sink (HTML body/attr, JS, URL, SQL identifier, LDAP, OS command) at render time → injection defence; need BOTH — encoding is context-specific and validation alone can't stop XSS*

**410. Explain the TLS/SSL handshake and what an SSL certificate is.**
   🔑 *ClientHello (versions, cipher suites, SNI, key share) → ServerHello + certificate chain → client validates chain to a trusted CA + hostname + validity + revocation (OCSP stapling) → key exchange (ECDHE for forward secrecy) → session keys → Finished → symmetric AES-GCM/ChaCha20; TLS 1.3 = 1-RTT (0-RTT resumption), drops RSA key transport + weak ciphers; certificate = X.509 binding a public key to a domain, signed by a CA (subject/SAN, issuer, validity); mTLS adds a client cert*

**411. How do you implement TLS for API security as a first step?**
   🔑 *TLS 1.2/1.3 only, valid CA cert at the LB/ingress, HTTP→HTTPS redirect, HSTS header, strong cipher suites, disable old protocols, automated renewal (cert-manager/ACM/Let's Encrypt); Spring Boot `server.ssl.*` or offload to ingress; then mTLS for internal hops + cert pinning for high-value mobile clients*

**412. What security headers should an API/app set?**
   🔑 *HSTS (Strict-Transport-Security), Content-Security-Policy (XSS), X-Content-Type-Options: nosniff, X-Frame-Options/`frame-ancestors` (clickjacking), Referrer-Policy, Permissions-Policy, Cache-Control for sensitive responses; set via Spring Security `headers()` or the gateway; remove Server/version banners*

**413. Coding: generate and validate a JWT with jjwt.**
   🔑 *`Jwts.builder().subject().claim("roles",...).issuedAt().expiration().signWith(key, HS256/RS256).compact()`; validate with `Jwts.parser().verifyWith(key).requireIssuer().requireAudience().clockSkewSeconds(30).build().parseSignedClaims(token)` → catch ExpiredJwtException/SignatureException/MalformedJwtException; key from `Keys.hmacShaKeyFor(≥256-bit secret)` or RSA keypair; secret from Vault/env, not code*

**414. What alternatives to JWT exist, and why choose JWT?**
   🔑 *Alternatives = opaque reference tokens + introspection (instant revocation, server-side state), server sessions + cookie (simple, revocable, needs shared/sticky store), PASETO (safer defaults), SAML assertions, API keys, mTLS certs; JWT wins for stateless horizontal scaling, cross-service trust without a shared session store, self-contained claims, standard tooling — at the cost of hard revocation + size*

**415. Explain session management and session fixation/hijacking defences.**
   🔑 *Server session + cookie; regenerate the session id on login (fixation defence — Spring `sessionManagement().sessionFixation().newSession()`), HttpOnly + Secure + SameSite cookies, idle + absolute timeout, bind to device/IP cautiously, concurrent-session control, invalidate on logout, store sessions in Redis for scale-out; stateless JWT avoids server sessions but loses easy revocation*

**416. What SEI CERT / secure-coding practices do you apply?**
   🔑 *Validate input at trust boundaries, defensive copies (no mutable internal state leak), immutable classes + private final fields, avoid native serialization of sensitive classes, don't log secrets, SecureRandom (not Random) for security values, constant-time comparison for secrets/MACs, try-with-resources, avoid TOCTOU races, don't catch/ swallow Throwable, sanitize format strings + command inputs*

**417. How do you implement multi-factor authentication (MFA) and strong auth?**
   🔑 *Something you know + have + are; TOTP (RFC 6238 — authenticator apps), WebAuthn/FIDO2 passkeys (phishing-resistant), SMS/email OTP (weakest), push approval; delegate to the IdP (Keycloak/Cognito/Okta); step-up auth for sensitive actions, account lockout + anomaly detection, backup codes; never roll your own crypto*

**418. How do you handle SSRF and outbound request safety?**
   🔑 *SSRF (Server-Side Request Forgery) = app fetches an attacker-controlled URL → block private/link-local ranges + cloud metadata endpoint (169.254.169.254), allowlist destinations, resolve + validate DNS (rebinding), disable redirects to internal hosts, no raw user URLs to internal services, egress network policies, use IMDSv2 on AWS*

**419. OAuth2 vs OpenID Connect (OIDC) — what's the difference?**
   🔑 *OAuth2 = authorization framework (delegated access via access tokens + scopes — "what you can do"); OIDC = an identity layer ON TOP of OAuth2 (adds the id_token = a JWT with authenticated-user claims — "who you are") + the `/userinfo` + discovery (`.well-known/openid-configuration`) + JWKS; use OIDC (Keycloak/Cognito/Okta/Auth0) for login/SSO, OAuth2 scopes for API authorization; access_token for calling APIs, id_token for the client's identity*

**420. How do you rotate secrets and manage certificates without downtime?**
   🔑 *Overlapping validity (issue new before revoking old), dual-accept window; Vault dynamic secrets with short leases + auto-renew; versioned keys with a `kid` so tokens signed by old + new keys both validate during rollout (JWKS lists both); automated cert rotation (cert-manager/ACM, mTLS mesh rotates sidecar certs); rolling restart or hot-reload config; audit + alert on expiry; never hardcode — pull at startup/refresh*

[⬆️ Back to top](#contents)

## Performance, Caching & Scalability

**421. How do you find and fix a slow endpoint / performance bottleneck in production?**
   🔑 *APM (New Relic/Dynatrace/Datadog) for p95/p99 + trace waterfall → which service/call is slow; async-profiler/JProfiler flame graph for CPU hotspots; GC logs (`-Xlog:gc`) for pause time; thread dump (jstack) for BLOCKED/WAITING; heap dump (MAT) for memory; SQL slow-query log + EXPLAIN for N+1/missing index; HikariCP pending-connections metric for pool waits; fix in order: algorithm → DB index → caching → async → pool/JVM tuning; profile before optimizing*

**422. Explain caching strategies — cache-aside vs write-through vs write-behind, local vs distributed.**
   🔑 *Cache-aside (lazy, most common) = app reads cache, miss → DB → populate; write-through = synchronous write to cache + DB (consistent, slower writes); write-behind = async flush to DB (fast, risk of loss); local Caffeine = ultra-low latency per-pod but cache drift across pods; Redis = shared distributed cache; never cache hot-mutating financial state without strong invalidation*

**423. How do you handle cache invalidation and cache stampede?**
   🔑 *Invalidation = TTL, event-driven eviction (`@CacheEvict` / publish eviction event), version/tag key (cache busting on schema change); stampede (thundering herd on expiry) = probabilistic early expiry (stale-while-revalidate), per-key mutex/lock on recompute, request coalescing/single-flight, background refresh before TTL; staggered TTLs to avoid synchronized expiry*

**424. How does caching work internally (Caffeine, Redis)?**
   🔑 *Caffeine (in-process) = hash map + Window-TinyLFU frequency sketch for admission/eviction, O(1); Redis = single-threaded event loop (atomic commands), per-type encodings (embstr/ziplist/quicklist/skiplist/hashtable), expiry via lazy + active sweep, eviction policies (LRU/LFU/volatile-ttl), persistence RDB snapshot + AOF; Spring `@Cacheable` = AOP proxy checks CacheManager before invoking the method*

**425. How do you configure TTL, cache keys and versioned invalidation?**
   🔑 *TTL per entity type (too short = miss storm, too long = stale); key = collision-free `entity:id:version`; version/tag suffix bumped on data/schema change for bulk invalidation; per-region CacheManager beans (`@Cacheable(cacheManager=...)`); negative caching for not-found; size + TTL bound to prevent OOM*

**426. What are Redis's main use cases?**
   🔑 *Cache (GET/SET + TTL), session store (shared across pods), distributed lock (`SET key uuid NX PX` + Lua release — Redisson RLock), rate limiting (INCR+EXPIRE / sorted set sliding window), pub/sub, Streams, leaderboards (sorted set), geo (GEOADD/GEORADIUS); eviction LRU/LFU; HA via Sentinel/Cluster*

**427. Coding: implement a Redis-based distributed lock (and cache-aside).**
   🔑 *Lock: `SET key UUID NX PX 5000` → OK if acquired; release with a Lua script (GET→compare UUID→DEL atomically) to avoid releasing someone else's lock; Redisson RLock (with watchdog lease renewal) in production; cache-aside: `ops.get(key)` → miss → fetch DB → `ops.set(key, value, ttl)`; set a fencing token for correctness-critical locks*

**428. How do you design for a 10x traffic spike or very high throughput (100k+ RPS)?**
   🔑 *Horizontal autoscaling (HPA/KEDA) of stateless pods, CDN for static + cacheable responses, pre-warm caches, Redis for hot lookups, DB read replicas + tuned pool (PgBouncer), buffer burst writes via Kafka/SQS, async non-blocking I/O (WebFlux/virtual threads) to avoid thread-per-request blocking, rate limiting + circuit breakers to shed load, partition/shard data, multi-region active-active; load test (Gatling) to find the saturation point first*

**429. When traffic is extreme, what do you prioritize — traffic, async, pool, locking, consistency, queries?**
   🔑 *Priority: (1) traffic handling (LB, autoscaling, CDN), (2) async decoupling of the write path (Kafka), (3) caching to eliminate hot DB reads, (4) query + index optimization, (5) connection-pool sizing, (6) minimize locking (optimistic > pessimistic, no distributed lock on hot path), (7) relax consistency to eventual where safe; measure p99, not averages*

**430. How do you tune a HikariCP connection pool and diagnose exhaustion?**
   🔑 *Settings = maximumPoolSize (small: ~cores×2, NOT hundreds), minimumIdle, connectionTimeout, idleTimeout, maxLifetime (< DB-side timeout); metrics = `hikaricp.connections.pending/active/idle`; exhaustion symptom = "HikariPool timed out waiting for connection"; causes = long transactions, N+1, slow queries holding connections, OSIV, leaks (unclosed); fix = shorten transactions, add index, go async, size correctly*

**431. How do you do capacity planning and load testing before a release?**
   🔑 *Define SLAs (e.g. p99 < 200ms @ 1000 RPS, error < 0.1%); JMeter/Gatling/k6 scenarios = ramp-up → steady-state → spike → soak; measure throughput + latency percentiles + error rate + resource saturation; gate in CI; plan headroom above peak + for a rolling deploy + one-AZ loss; chaos testing for failure modes*

**432. Horizontal vs vertical scaling, and the layers of caching.**
   🔑 *Vertical = bigger box (simple, limited, SPOF, costly); horizontal = more stateless instances behind an LB (elastic, needs externalized state); caching layers = browser → CDN/edge → API/app (Caffeine/Redis) → DB (query/buffer/materialized view); push work to the cheapest cacheable layer; read/write splitting via replicas*

**433. How do you optimize Java code in hot paths?**
   🔑 *Algorithm first (reduce Big-O), profile before optimizing (no premature optimization); StringBuilder over `+` in loops, batch DB calls (`findAllById` vs N×`findById`), lazy/short-circuit streams, primitives over boxed, avoid allocation in hot loops, cache expensive computations, async for I/O-bound; measure with JMH microbenchmarks*

**434. What is backpressure and how do you apply load shedding?**
   🔑 *Backpressure = consumer signals it can't keep up so the producer slows (Reactive Streams `request(n)`, bounded queues, Kafka lag); load shedding = reject/queue excess early (429, bounded executor + CallerRuns, admission control) to protect the system; graceful degradation (serve cached/default); Little's law (L = λ×W) to reason about queue length vs latency*

**435. How do you optimize application/JVM startup time (summary)?**
   🔑 *See JVM (CDS/AppCDS, AOT, GraalVM native image, CRaC checkpoint/restore, tiered compilation) and Spring (lazy init, fewer scans, AOT); dev-only `-XX:TieredStopAtLevel=1`; trade-off = startup speed vs peak throughput/warmup*

**436. What is APM (Application Performance Monitoring) and how do you use it?**
   🔑 *APM (New Relic/Dynatrace/Datadog/AppDynastics/Elastic APM) auto-instruments the app → distributed traces (span waterfall per request), service maps/dependencies, latency percentiles (p50/p95/p99), throughput, error rate, DB/external-call breakdown, JVM metrics; use it to find the slow hop, correlate with deploys, set SLO alerts; RED (Rate/Errors/Duration) for services, USE (Utilization/Saturation/Errors) for resources; sampling to control cost; ties logs+metrics+traces via trace id*

**437. How do you add a read/query caching layer to a read-heavy service?**
   🔑 *Cache-aside with Redis for hot entities + computed read models; `@Cacheable` on service methods keyed by inputs; TTL + event-driven eviction on writes; near-cache (Caffeine) in front of Redis to cut network RTT; materialized views / denormalized read tables (CQRS) for expensive aggregations; cache stampede protection; measure hit ratio; never cache strongly-consistent financial reads without tight invalidation*

[⬆️ Back to top](#contents)

## Production Troubleshooting, Logging & Observability

**438. How do you correlate a request across many microservices (distributed tracing)?**
   🔑 *Trace id generated at entry, propagated via headers (W3C `traceparent` / B3); each service emits child spans; Micrometer Tracing / OpenTelemetry auto-instruments Spring Boot → Zipkin/Jaeger/Tempo; `MDC.put("traceId",...)` so every log line carries it → cross-service search in ELK/Splunk; sampling to control volume; ties the three pillars (logs + metrics + traces) together*

**439. What is your structured logging strategy — what do you log and never log?**
   🔑 *Log = request/trace id, user/tenant id, timing, business events, state transitions, errors with stack; NEVER log = passwords, tokens, PAN/CVV, PII (GDPR), secrets; JSON structured logs (logstash-logback-encoder), INFO in prod (DEBUG on demand via actuator `/loggers`), async appender to avoid blocking, sampling for high-volume paths, retention/ILM policies, alert on error rate not every line*

**440. How do you set up observability (ELK, Prometheus, Grafana) — the three pillars?**
   🔑 *Logs = Filebeat → Logstash/parse → Elasticsearch → Kibana (ILM for retention); Metrics = Prometheus scrapes `/actuator/prometheus` (JVM, HikariCP, custom Micrometer) → Grafana dashboards (p99 latency, error rate, GC pause, throughput); Traces = OpenTelemetry → Jaeger; Alertmanager → PagerDuty/Slack; three pillars = logs + metrics + traces; Splunk/OpenSearch as alternatives*

**441. How do you debug an OutOfMemoryError / analyze a heap dump?**
   🔑 *`-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=...` or `jmap -dump:format=b,file=heap.hprof <pid>`; GC logs to see if it's GC-overhead-limit; analyze in Eclipse MAT → Leak Suspects + Dominator Tree + Histogram (retained heap by class); common = unbounded static collection/cache without eviction, unclosed streams, ThreadLocal not removed, oversized cache; fix = bound cache (Caffeine maxSize+TTL), try-with-resources, weak refs, tune `-Xmx`; add GC alerting*

**442. How do you handle a production incident (on-call, midnight, critical service down)?**
   🔑 *Acknowledge alert → assess scope/severity (S1/S2) + blast radius → check dashboards + recent deploys (rollback candidate) → pull logs/traces + resource metrics (CPU/mem/DB conns/GC) → check pod status (OOMKilled/CrashLoopBackOff) + dependency health → fastest recovery (rollback/restart/scale/config fix) → communicate status → preserve artifacts (thread/heap dump) → blameless postmortem + 5-whys + action items + runbook update*

**443. An API is suddenly slow (and prod is slower than dev) — what do you check?**
   🔑 *APM: which endpoint, since when, correlate with deploy; DB slow-query log (N+1 / missing index on prod data volume); downstream latency via trace; thread dump (blocked threads); GC pause log (STW); connection-pool wait; CPU/memory throttling; prod-vs-dev differences = data volume → full scans, network latency, JIT warmup, pool/timeout config, synchronous log I/O, resource limits; fix the first confirmed bottleneck*

**444. DB CPU is low but API latency is high — what's the cause?**
   🔑 *App-side bottleneck: Tomcat/thread pool exhausted waiting on a connection or lock, slow external call blocking the request thread, GC stop-the-world pause, in-app lock contention, undersized connection pool (waiting to acquire, not executing), serialization overhead, N+1 (many tiny fast queries exhausting the pool); check thread dump + pool pending metric + GC log*

**445. How do you detect crashes and failures?**
   🔑 *Health endpoints (`/actuator/health` with DB/Redis/Kafka indicators, liveness/readiness groups); K8s liveness probe restarts hung pods, OOMKilled status; error tracking (Sentry/Datadog); GC-overhead + OOM alerts; deadlock via thread dump (jstack/`kill -3`); circuit-breaker-open events; alert on error-rate spike + ERROR log count threshold*

**446. What are SLI, SLO, SLA and error budgets?**
   🔑 *SLI = measured signal (latency, availability, error rate); SLO = internal target (99.9% availability, p99 < 200ms); SLA = external contract + penalties; error budget = 1 − SLO (allowed failure) → governs release pace (burn too fast → freeze features, fix reliability); RED method (Rate/Errors/Duration) for services, USE method (Utilization/Saturation/Errors) for resources*

**447. What is Splunk and how do you use it?**
   🔑 *Log aggregation + search/analytics; SPL (Splunk Processing Language) for ad-hoc queries + dashboards; ingest via Filebeat/HEC; dashboards for error rate, latency percentiles, business KPIs; alerts on log patterns/error spikes; correlate across services by traceId; licensed by daily ingest volume; ELK/OpenSearch is the open-source alternative*

**448. What logging framework do you use and how do you configure it?**
   🔑 *SLF4J facade + Logback (or Log4j2); `logback-spring.xml` = appenders (Console, RollingFile), encoders (JSON via LogstashEncoder), levels per package, AsyncAppender to avoid blocking; MDC for traceId/userId; `logging.level.*` in application.yml; runtime level change via actuator `/loggers`; profile-specific config*

**449. How do you tell a war story about a production issue / the hardest bug you solved?**
   🔑 *STAR: Situation (impact/severity), Task (your role), Action (diagnostic steps — thread/heap dump, slow query, trace; the fix), Result (metrics + preventive measures: new test, alert, runbook, architectural fix); examples = N+1 overloading the DB, memory leak from an unbounded cache, distributed-lock deadlock, race in an idempotency check, misconfigured pool timeout; show you improved the system, not just patched it*

**450. How do you design alerting that avoids fatigue?**
   🔑 *Alert on symptoms users feel (SLO burn, error rate, p99 latency, saturation) not every cause; actionable + severity-tiered (page for S1, ticket for S2); every alert links a runbook; deduplicate + group (Alertmanager), set sensible thresholds + for-durations to avoid flapping; multi-window burn-rate alerts on the error budget; silence during deploys; review + prune noisy alerts regularly; on-call rotation + escalation policy*

**451. What is a postmortem culture and how do you run one?**
   🔑 *Blameless (focus on systems/process, not people), written within days; timeline + impact + detection + root cause (5-whys / contributing factors), what went well/poorly; concrete action items with owners + due dates (new test, alert, runbook, guardrail, architectural fix), tracked to completion; share org-wide to spread learning; measure MTTR/MTTD trends*

[⬆️ Back to top](#contents)

## Testing & Code Quality

**452. Explain the test pyramid — unit vs integration vs contract vs E2E.**
   🔑 *Unit = single class, deps mocked, fast (ms), many (JUnit 5 + Mockito); integration = real DB/Kafka/Redis via Testcontainers, fewer, catch wiring/config/query bugs; contract (Pact) = consumer publishes expectations, provider verifies (prevents silent breaking changes); E2E = full stack, minimal (slow/flaky); in practice the pyramid is often inverted in legacy code — heavy integration layer because mocked tests passed but prod diverged*

**453. Explain Mockito — @Mock vs @MockBean vs @Spy vs @InjectMocks, and mock vs stub vs fake.**
   🔑 *@Mock = plain Mockito mock (`@ExtendWith(MockitoExtension.class)`, fast, no context); @MockBean = replaces a bean in the Spring context (`@SpringBootTest`/`@WebMvcTest`, slower); @Spy = real object with selective stubbing; @InjectMocks = inject mocks into the class under test; @Captor = capture arguments; mock = configured fake, stub = canned response, spy = partial real, fake = working in-memory impl, dummy = placeholder; `doReturn/when` for void + spies*

**454. What are common Mockito pitfalls?**
   🔑 *Over-mocking (testing nothing real), verifying internals (`verify(repo).save()`) couples test to implementation, deep stubs (`mock.getX().getY()`) = design smell, mocking value objects, unstubbed call returns null → NPE, `@Spy` on classes with heavy constructors; prefer behaviour assertions over interaction verification*

**455. How have you used Testcontainers?**
   🔑 *`@Testcontainers` + `@Container PostgreSQLContainer`/`KafkaContainer`/`RedisContainer`, `@DynamicPropertySource` to inject JDBC URL / bootstrap servers; per-class static container (faster) vs per-test (isolation); catches real driver/broker/constraint/migration behaviour that H2/mocks miss (Flyway runs against real Postgres); slower but higher fidelity*

**456. How do you test asynchronous / event-driven flows without flaky sleeps?**
   🔑 *Awaitility (`await().atMost(10, SECONDS).until(() -> repo.findById(id).isPresent())`); CountDownLatch in the listener; `@EmbeddedKafka` + KafkaTemplate publish + Awaitility poll; `CompletableFuture.get(timeout)`; ArgumentCaptor with polling; NEVER `Thread.sleep` (time-dependent, flaky on slow CI)*

**457. How do you test @Transactional rollback and idempotency?**
   🔑 *Rollback: `@DataJpaTest` (`@Rollback` default), or throw mid-transaction + assert state unchanged in a separate query via TransactionTemplate; idempotency: send the same request twice with the same key → assert a single DB record + same response; assert unique-constraint violation is handled gracefully*

**458. How do you integration-test a REST controller?**
   🔑 *`@SpringBootTest(webEnvironment=RANDOM_PORT)` + TestRestTemplate for full HTTP; or `@WebMvcTest(Controller.class)` slice + `@MockBean` service + MockMvc (`mockMvc.perform(post(...).contentType(JSON).content(body)).andExpect(status().isCreated()).andExpect(jsonPath("$.id").exists())`); `@DirtiesContext` when state leaks between tests*

**459. How do you mock a Kafka producer/consumer or an external REST call?**
   🔑 *Kafka: `@EmbeddedKafka` (real broker in-process) for integration, `@MockBean KafkaTemplate` for unit; external REST: WireMock (`stubFor(get(urlEqualTo(...)).willReturn(aResponse().withStatus(200).withBody(...)))`, verify + fault injection) or `MockRestServiceServer` for RestTemplate; Feign client `@MockBean`*

**460. Coding: write a JUnit 5 + Mockito test for a service calling a repository and a Feign client.**
   🔑 *`@ExtendWith(MockitoExtension.class)`, `@Mock Repo repo`, `@Mock Client client`, `@InjectMocks Service service`; given `when(repo.findById(1L)).thenReturn(Optional.of(user))` + `when(client.get(1L)).thenReturn(details)`; act `service.process(1L)`; assert result; `verify(client).get(1L)`; edge case: empty Optional → throws ResourceNotFoundException*

**461. How do you measure code coverage, and what's a good target?**
   🔑 *JaCoCo (line/branch/method coverage) via Maven/Gradle plugin → SonarQube quality gate fails below threshold; ~70-80% (higher for domain logic, lower for DTOs/config); coverage ≠ quality → mutation testing (PIT) measures whether tests actually catch bugs; focus on business logic*

**462. Explain contract testing with Pact.**
   🔑 *Consumer-driven: consumer defines expected request/response → publishes a Pact file to the Pact Broker; provider runs `pact:verify` to confirm it satisfies all consumers; catches breaking API changes pre-deploy; `can-i-deploy` gate in CI; more reliable than shared integration environments; solves the "who broke what" problem*

**463. What is SonarQube, and how do you fix what it flags?**
   🔑 *Static analysis: bugs (NPE, resource leak), vulnerabilities (SQLi, hardcoded creds), code smells (long method, deep nesting, duplication, magic numbers), security hotspots, coverage import; quality gate blocks PR merge on violations; fixes = extract method, introduce constants, reduce cyclomatic complexity, remove duplication; runs in CI via SonarScanner, PR decoration*

**464. Have you used SAST (Checkmarx/Veracode/Semgrep)? Walk through a finding.**
   🔑 *SAST = scans source for vulnerabilities without running the app; findings = SQLi (→ parameterized queries), XSS (→ output encoding), path traversal (→ canonicalize/validate), hardcoded credentials (→ Vault/env), insecure deserialization; triage false positives, fix → rescan → verify; build-breaking gate for critical findings in CI*

**465. What is Zally and API linting/governance?**
   🔑 *Zally (Zalando) = lints OpenAPI/Swagger specs against API guidelines (naming, versioning, pagination, error structure, required headers); CLI or CI step; custom org rules; enforces consistent APIs across teams before publishing; Spectral is a popular alternative*

**466. Explain TDD and BDD.**
   🔑 *TDD = Red (failing test) → Green (minimal code) → Refactor; drives design + coverage; BDD = Given/When/Then scenarios (Cucumber + Gherkin) readable by business, maps to acceptance tests; both shift testing left; use TDD for logic, BDD for cross-team acceptance criteria*

**467. What's your overall automation-testing approach?**
   🔑 *Unit (Mockito/JUnit), integration (Testcontainers/Spring slices), API (RestAssured/Karate), contract (Pact), UI (Selenium/Playwright, page-object model), BDD (Cucumber); test data factories/fixtures, parallel execution, fail-fast in CI, reporting (Allure); parameterized + edge-case tests; flaky-test quarantine*

**468. How do you manage test data and keep tests deterministic and isolated?**
   🔑 *Test data builders/object mothers + factories (not shared fixtures); fresh schema per run (Testcontainers + Flyway) or `@Sql`/`@Transactional` rollback for isolation; seed minimal data per test; avoid shared mutable static state + test ordering dependence; control time (`Clock` injection) + randomness (fixed seed) + UUIDs; clean up or use unique keys; parallel-safe (unique tenant/ids); no reliance on prod data*

**469. How do you include performance/load and security testing in CI?**
   🔑 *Performance: Gatling/k6 smoke in the pipeline against a staging deploy, assert p99 + error-rate thresholds (fail the build on regression), full soak nightly; Security: SAST (SonarQube/Semgrep/Checkmarx) + SCA (OWASP Dependency-Check/Snyk) as build gates, DAST (OWASP ZAP) against staging, secret scanning (gitleaks); shift-left, fail fast on critical findings*

[⬆️ Back to top](#contents)

## Cloud, DevOps, Docker, Kubernetes & CI/CD

**470. Walk through a CI/CD pipeline end-to-end, and the tools you use.**
   🔑 *Push/PR → CI trigger (Jenkins/GitLab CI/GitHub Actions) → build (Maven/Gradle, parallel modules) → unit + integration tests (Testcontainers) → quality gate (SonarQube + JaCoCo + SAST) → Docker multi-stage image tagged with git SHA → push to registry (ECR/Artifactory) → deploy dev (Helm/ArgoCD) → smoke tests → promote staging → manual approval → deploy prod (blue-green/canary) → monitor → auto-rollback on error spike/failed health check; secrets in credentials store, feature flags for progressive rollout*

**471. Explain deployment strategies — rolling, blue-green, canary, recreate, shadow.**
   🔑 *Rolling (K8s default) = incremental pod replace (maxSurge/maxUnavailable), zero downtime but mixed versions briefly; blue-green = two envs, instant LB switch, easy rollback, double infra, DB must be backward-compatible; canary = gradual traffic shift (1%→100%) with automated analysis (Argo Rollouts/Istio), catches issues early; recreate = all down then up (downtime); shadow = mirror traffic to new version without user impact; A/B = segment-based (feature flags)*

**472. How do you achieve zero-downtime deployment when the schema is changing?**
   🔑 *Expand-contract: (1) add nullable column, (2) deploy code writing both old+new, (3) backfill, (4) deploy code reading new only, (5) drop old; each step independently deployable; Flyway/Liquibase versioned migrations; never rename/drop in the same deploy as usage; backward-compatible for the N-1 running version during rollout (see Databases section)*

**473. Docker — image vs container, Dockerfile layers, multi-stage builds, and how to create one.**
   🔑 *Image = immutable layered filesystem (each FROM/RUN/COPY = a cached layer); container = running instance with a writable top layer; multi-stage = JDK stage compiles (`FROM maven AS builder; RUN mvn package`) → slim JRE stage copies only the jar (`FROM eclipse-temurin:21-jre-alpine; COPY --from=builder ...; ENTRYPOINT ["java","-jar","app.jar"]`) → smaller image, smaller attack surface; put stable layers first (deps before source), `.dockerignore` excludes target/.git*

**474. VM vs Docker — how does a container run on any machine?**
   🔑 *VM = full OS + kernel via hypervisor, heavy (GBs, minutes), strong isolation; container = shares the host Linux kernel via namespaces (PID/net/mount/UTS/IPC isolation) + cgroups (CPU/memory/IO limits), lightweight (MBs, ms); UnionFS/overlayfs layers form the filesystem; runtime containerd→runc; Docker Desktop runs a small Linux VM on Mac/Windows; same image runs anywhere with the same kernel syscall interface → "works on my machine" solved*

**475. What is a Docker volume?**
   🔑 *Persistent storage surviving container restart/removal; named volumes (Docker-managed), bind mounts (host path → container), tmpfs (in-memory); `docker volume ls/inspect/rm`; K8s equivalent = PersistentVolume + PersistentVolumeClaim; use for DB data, logs, shared config*

**476. Where are Docker images stored, and how do you version them?**
   🔑 *Local daemon store + remote registries (Docker Hub, AWS ECR private + IAM + image scanning, Artifactory, GHCR); tag with semantic version + git SHA (reproducible); avoid `latest` in prod (mutable, no rollback); immutable tags / pin by digest (`sha256:...`); promote the SAME image across envs by tag (no rebuild); ECR lifecycle policy to retire old images*

**477. How do you manage multiple containers locally and in production?**
   🔑 *Local = Docker Compose (`docker-compose up/down/ps/logs/exec`, services/networks/volumes); prod = Kubernetes; `docker ps/logs/exec -it/stop/rm/images/pull/push`; Compose is dev-only, K8s orchestrates in prod*

**478. What is Kubernetes, and what are its key commands?**
   🔑 *Container orchestration: scheduling, self-healing (restart failed pods), rolling updates, autoscaling (HPA), service discovery + LB, config/secret + storage orchestration; control plane = API server + etcd + scheduler + controller-manager, workers = kubelet + runtime + kube-proxy; commands = `kubectl get/describe pods|svc|deploy`, `logs -f`, `exec -it`, `apply -f`, `scale`, `rollout status/undo`, `port-forward`, `top`*

**479. Explain Pod, Deployment, Service, ConfigMap, Secret — and how a Service finds Pods.**
   🔑 *Pod = smallest unit (1+ containers); Deployment = desired replicas + rolling update; Service = stable DNS + virtual IP load-balancing to Pods by label selector; discovery = kube-proxy iptables/IPVS + CoreDNS (`svc.namespace.svc.cluster.local`); ConfigMap = non-secret config (env/mounted file); Secret = base64 (NOT encryption — enable etcd encryption at rest + RBAC, or ExternalSecrets); StatefulSet for stateful, DaemonSet for per-node*

**480. How do you configure readiness vs liveness probes for Spring Boot (and what breaks if reversed)?**
   🔑 *Readiness = ready to serve (DB connected, caches warm, Kafka up) → K8s removes from Service endpoints if failing (no restart); Liveness = alive/not hung → K8s RESTARTS the pod if failing; keep liveness minimal (internal state only) — if liveness checks a slow DB → restart loop; readiness may check external deps; Spring Boot 2.3+ auto-configures `/actuator/health/readiness` + `/liveness`; set correct initialDelay/timeout*

**481. How do you do graceful shutdown so in-flight requests aren't dropped?**
   🔑 *K8s sends SIGTERM → stop accepting new requests → drain in-flight → exit; Spring `server.shutdown=graceful` + `spring.lifecycle.timeout-per-shutdown-phase=30s`; K8s `terminationGracePeriodSeconds ≥ timeout`; preStop hook (`sleep 5`) to let the LB deregister before SIGTERM; Kafka consumers commit + finish the current batch; PodDisruptionBudget protects availability during drains*

**482. How do you scale replicas in Kubernetes (CLI, declarative, autoscaling)?**
   🔑 *`kubectl scale deployment app --replicas=5`; patch/edit spec.replicas + `kubectl apply`; HPA (`kubectl autoscale ... --min 2 --max 10 --cpu-percent 70`) on CPU/mem/custom metrics; KEDA for event-driven (Kafka lag, SQS depth); VPA for right-sizing requests; set resource requests/limits so the scheduler + HPA work correctly*

**483. What are Helm charts?**
   🔑 *Helm = K8s package manager; Chart = Chart.yaml (metadata), values.yaml (defaults), templates/ (K8s YAML with Go templating `{{.Values.image.tag}}`); `helm install/upgrade --set/--values prod.yaml`, `helm rollback app 1`, `helm list`; release = named install; repos for distribution; simplifies parameterized multi-environment deploys; Kustomize is the templating-free alternative*

**484. How do you manage secrets in Kubernetes/AWS securely?**
   🔑 *Never hardcode; ExternalSecrets Operator syncs from AWS Secrets Manager/Vault into K8s Secrets; IRSA (IAM Roles for Service Accounts) for keyless pod AWS access; Vault agent sidecar for dynamic short-lived DB creds; Sealed Secrets for GitOps (encrypted in Git); encrypt etcd at rest; never log/dump secrets in env; rotate + least privilege + audit*

**485. Maven vs Gradle — which and why, and multi-module experience?**
   🔑 *Maven = XML POM, convention lifecycle (validate/compile/test/package/install/deploy), ubiquitous; Gradle = Groovy/Kotlin DSL, incremental builds + build cache (faster for large repos), more flexible; multi-module Maven = parent POM + `<modules>` + `<dependencyManagement>`; Gradle = settings.gradle `include(':mod')`; use the wrapper (mvnw/gradlew) for reproducibility*

**486. EC2 vs Lambda vs ECS/EKS — when serverless vs containers?**
   🔑 *EC2 = full VM you manage (long-running, flexible); Lambda = serverless FaaS, event-triggered (API GW/SQS/S3/EventBridge), scales to 0, pay-per-invocation, 15-min limit, cold starts, stateless; ECS (Fargate) = managed containers, no node management; EKS = Kubernetes on AWS; Lambda for intermittent event handlers, ECS/EKS for always-on microservices, EC2 when you need OS control*

**487. How do you deploy a JAR to EC2?**
   🔑 *SSH (key pair) + scp jar + `java -jar app.jar`; better = systemd service for auto-restart, or CodeDeploy agent + appspec.yml, or containerize → ECR → ECS/EKS; security group opens the app port; Elastic IP for a stable address; user-data script for bootstrap; prefer immutable AMIs/containers over in-place*

**488. How do you trigger and develop an AWS Lambda?**
   🔑 *Triggers = API Gateway (HTTP), SQS (event source mapping, batch), S3, SNS, EventBridge (cron/event), Kinesis, DynamoDB Streams; develop = `RequestHandler<In,Out>`, Lambda context (timeout/logging), env vars for config, layers for shared deps, IAM execution role, X-Ray tracing; deploy = ZIP → S3, container image (ECR), or SAM/CDK/Terraform; mitigate cold start with provisioned concurrency*

**489. How does SQS help decouple microservices?**
   🔑 *Producer sends + returns immediately (async); consumer polls at its own rate (independent scaling); SQS buffers bursts; visibility timeout = processing window; DLQ after max receives; FIFO queue for ordering + dedup; at-least-once (idempotent consumers required); producer and consumer can be down independently without data loss*

**490. What is Infrastructure as Code / Terraform, and its basic commands?**
   🔑 *IaC = version-controlled, reviewed, reproducible infra (drift detection, dev=prod parity); Terraform = declarative HCL, state in S3 + DynamoDB locking, modules for reuse, providers for AWS/GCP/Azure, workspaces per env; commands = `init` (providers), `plan` (dry-run diff), `apply`, `destroy`, `fmt`, `validate`, `state list/show/rm`, `import`, `output`; Ansible = config management (imperative, agentless); CDK/Pulumi = IaC in real languages*

**491. Which AWS services do you use, and the key best practices?**
   🔑 *EC2, ECS/EKS, Lambda, S3, RDS, ElastiCache (Redis), SQS/SNS, API Gateway, CloudFront (CDN), Route53 (DNS + latency/weighted routing), VPC, IAM, CloudWatch, Secrets Manager/KMS; best practices = least-privilege IAM + roles, private subnets for DBs, security groups by port/source, encryption at rest (KMS) + in transit (TLS), multi-AZ for HA, autoscaling, cost monitoring*

**492. What is GitOps (ArgoCD/Flux)?**
   🔑 *Git is the single source of truth for desired cluster state; a controller (ArgoCD/Flux) continuously reconciles the cluster to the Git manifests/Helm; deploy = merge a PR (image tag bump), controller syncs; benefits = auditability, easy rollback (git revert), drift detection, no cluster credentials in CI; pull-based vs push-based deploys*

**493. How do you manage environments, config and load balancers across deployments?**
   🔑 *Environments = K8s namespaces (dev/staging/prod) + Helm values per env; promote the same image tag across namespaces via GitOps; common config = Spring Cloud Config/ConfigMaps, secrets via ExternalSecrets/Vault; LB = K8s Ingress + AWS ALB Ingress Controller, cert-manager for TLS, Route53 weighted routing for canary; 12-factor config from the environment*

**494. Explain the Maven build lifecycle and the key plugins.**
   🔑 *Lifecycle phases: validate → compile → test (surefire, unit) → package → verify (failsafe, integration *IT) → install → deploy; each phase runs bound plugin goals; key plugins = compiler, surefire/failsafe, spring-boot (repackage fat jar / build-image via Buildpacks), jib (daemonless image), shade/assembly (uber jar), jacoco, versions, enforcer; `mvn clean verify`; profiles for env-specific builds; `dependency:tree` to debug conflicts*

**495. What backend development tools do you use day to day, and why?**
   🔑 *Build = Maven/Gradle; IDE = IntelliJ; boilerplate = Lombok, mapping = MapStruct; API = OpenAPI/Swagger (springdoc), testing = JUnit 5/Mockito/Testcontainers/WireMock/RestAssured; API clients = Postman/Insomnia/Newman; quality = SonarQube/JaCoCo, security = OWASP Dependency-Check/Snyk; DB migration = Flyway/Liquibase; containers = Docker/Compose; local infra = Testcontainers; observability = Actuator/Micrometer/Prometheus/Grafana; CI/CD = Jenkins/GitHub Actions/ArgoCD; secrets = Vault*

**496. Which API testing and mocking tools do you use?**
   🔑 *Manual/exploratory = Postman/Insomnia; automated API = Newman (Postman in CI), RestAssured (JVM), Karate (BDD); load = JMeter/Gatling/k6; mocking externals = WireMock (stub + fault injection), MockServer; Kafka = EmbeddedKafka/Testcontainers; contract = Pact/Spring Cloud Contract; Swagger UI for try-it-out; keep API tests in CI with a deployed/staged service*

**497. What is the twelve-factor app methodology?**
   🔑 *Codebase (one repo, many deploys), explicit dependencies, config in env, backing services as attached resources, build/release/run separation, stateless processes, port binding, concurrency via process scale-out, disposability (fast start/graceful shutdown), dev/prod parity, logs as event streams (stdout), admin tasks as one-off processes; underpins cloud-native + containerized microservices*

[⬆️ Back to top](#contents)

## Design Patterns & SOLID

**498. Explain all 5 SOLID principles with a real violation → fix for each.**
   🔑 *S (SRP): God class doing fetch+format+email → split into focused classes (one reason to change); O (OCP): switch-on-type → interface + polymorphism, new type = new class; L (LSP): subclass throws where parent doesn't / breaks contract → fix inheritance or use composition, test via base-type reference; I (ISP): fat interface forcing empty methods → split into client-focused interfaces; D (DIP): `new ConcreteService()` → inject an interface via constructor (Spring `@Autowired`)*

**499. How do you refactor a class that violates the Single Responsibility Principle?**
   🔑 *Spot multiple reasons to change (data access + business logic + formatting + notification); extract each into its own class — e.g. OrderService → OrderRepository, OrderValidator, DiscountCalculator, OrderEmailer; one reason to change each; improves testability + reuse; watch for >1 "section" of tests as a smell*

**500. Give an example of Dependency Inversion / IoC & DI in Spring.**
   🔑 *Define `PaymentGateway` interface; impls StripeGateway/PayPalGateway; `@Autowired PaymentGateway` (Spring injects via `@Qualifier`/`@Primary`); high-level service depends on the abstraction, not the Stripe SDK → swap provider without touching the service, inject a mock in tests; IoC = framework owns creation/lifecycle, DI = constructor/setter/field injection of dependencies*

**501. Coding: design a PaymentProcessor so new types can be added without modifying existing code (OCP/Strategy).**
   🔑 *`interface PaymentProcessor { boolean process(Payment p); String type(); }`; impls UpiProcessor/CardProcessor/WalletProcessor as `@Component`; registry `Map<String,PaymentProcessor>` auto-built from the bean list; `service.process(p) = registry.get(p.getType()).process(p)`; new type = new class, zero changes to the service — Strategy pattern, no if-else/switch*

**502. Explain the GoF design patterns (creational, structural, behavioral) with real examples.**
   🔑 *Creational: Factory (create by type, hide construction), Abstract Factory (families), Builder (complex immutable objects/DTOs), Singleton (Spring beans), Prototype (clone); Structural: Adapter (legacy XML→JSON wrapper), Decorator (add caching/logging), Proxy (Spring AOP/lazy/access), Facade (simplified API), Composite (tree — org/discount rules); Behavioral: Strategy (interchangeable algorithm), Observer (ApplicationEvent + @EventListener), Command, Template Method (AbstractBatchJob), Chain of Responsibility (filters)*

**503. Strategy vs Factory — when do you use which?**
   🔑 *Strategy = behaviour varies at runtime (same operation done differently per context — discount per tier, processor per method), interchangeable algorithm; Factory = creation varies (produce the right object by type/config, then use it uniformly); often combined — a Factory creates the right Strategy; both replace if-else/switch on type*

**504. Why is the enum singleton the best singleton?**
   🔑 *`enum Singleton { INSTANCE; }` — JVM guarantees one instance per classloader, thread-safe without synchronization, serialization-safe (enum mechanism blocks a new instance on deserialize), reflection-safe (can't call the constructor via reflection); preferred over double-checked locking / static-holder*

**505. Which design pattern did you use, and why (over the alternatives)?**
   🔑 *STAR: problem (adding payment types required editing existing code) → chose Strategy/OCP → interface + per-type impls + registry → result: new type = new class, zero modification, each unit-testable; why over alternatives: vs if-else (not scalable/open-closed), vs inheritance (not flexible, deep hierarchies); tie the choice to the problem it solved*

**506. Explain Builder, Decorator, Proxy and Template Method with where you'd use them.**
   🔑 *Builder = step-by-step construction of complex/immutable objects (DTOs, request objects, Lombok `@Builder`); Decorator = wrap to add behaviour without subclassing (buffered streams, caching/logging wrappers); Proxy = stand-in for access control/lazy/remote (Spring AOP + `@Transactional`/`@Async` use CGLIB/JDK proxies); Template Method = skeleton algorithm with overridable steps (AbstractBatchJob, Spring's JdbcTemplate)*

**507. Explain Observer and Chain of Responsibility.**
   🔑 *Observer = publish/subscribe, subject notifies observers of state change (Spring ApplicationEvent + `@EventListener`, message brokers) → decouples producer from consumers; Chain of Responsibility = request passed along a chain until one handles it (Servlet filters, Spring Security filter chain, validation chains, middleware)*

**508. What design anti-patterns do you watch for in review?**
   🔑 *God class (does everything → SRP violation), singleton abuse (hidden global state, hard tests), anemic domain model (logic leaked into services), service locator over DI, premature abstraction (YAGNI), deep inheritance over composition, primitive obsession, long parameter lists, feature envy, magic numbers, circular dependencies*

**509. Why do design patterns matter — which problem does each solve?**
   🔑 *Shared vocabulary + proven solutions + guided design; problem→pattern: one shared instance → Singleton; complex creation → Builder; create by type → Factory; incompatible interfaces → Adapter; add behaviour → Decorator; control access → Proxy; many listeners → Observer; interchangeable algorithm → Strategy; tree → Composite; simplified API → Facade; skeleton algorithm → Template Method; apply judiciously, not everywhere*

**510. Explain the DDD tactical building blocks.**
   🔑 *Entity (identity + lifecycle), Value Object (immutable, equality by value), Aggregate + Aggregate Root (consistency boundary — only the root is referenced, enforces invariants, one transaction per aggregate), Repository (collection-like persistence of aggregates), Domain Service (logic not belonging to one entity), Domain Event, Factory; strategic = Bounded Context + Ubiquitous Language + Context Map; guides microservice boundaries*

**511. Explain the Repository and Unit of Work patterns.**
   🔑 *Repository = abstraction over persistence that looks like an in-memory collection of aggregates (hides queries/ORM) → testable, swappable; Unit of Work = tracks changes in a business transaction and commits them atomically (JPA persistence context + flush/commit IS a Unit of Work); Spring Data `JpaRepository` implements Repository; keep domain logic out of repositories*

**512. Explain the Specification and Null Object patterns.**
   🔑 *Specification = encapsulate a business rule/predicate as an object, composable (and/or/not) → reusable query/validation criteria (Spring Data `Specification`, dynamic queries); Null Object = a do-nothing implementation of an interface instead of null → removes null checks + NPEs (e.g. a no-op logger/handler); both reduce conditional clutter and centralize rules*

**513. What are the common resilience/concurrency patterns beyond GoF?**
   🔑 *Resilience = Circuit Breaker, Retry, Timeout, Bulkhead, Rate Limiter, Fallback, Outbox, Saga; concurrency = Producer-Consumer, Thread Pool, Future/Promise, Read-Write Lock, Immutable/Thread Confinement, Monitor, Active Object; integration = Gateway, Adapter, Ambassador/Sidecar, Anti-Corruption Layer; cloud = Strangler Fig, CQRS, Event Sourcing, Leader Election*

[⬆️ Back to top](#contents)

## System Design & Design Scenarios

**514. How do you approach a system design problem (requirements, NFRs, estimation, trade-offs)?**
   🔑 *Clarify functional requirements + NFRs (Non-Functional Requirements: scalability, availability, latency, consistency, durability, security, cost); back-of-envelope estimation (QPS, storage, bandwidth, read:write ratio); define the API; high-level components + data model; identify bottlenecks → scale (cache, replicas, sharding, queue, CDN); state one explicit trade-off per decision (consistency vs availability, latency vs cost); discuss failure modes + monitoring*

**515. Explain load balancing — L4 vs L7, algorithms, and where it sits.**
   🔑 *L4 (transport, TCP/UDP) = fast, connection-level; L7 (application, HTTP) = routing by path/header/cookie, TLS termination, retries; algorithms = round-robin, least-connections, least-response-time, IP/consistent hash (sticky); client-side vs server-side LB; health checks remove bad nodes; global LB (DNS/GSLB) + regional; avoid sticky sessions (keep services stateless)*

**516. What is consistent hashing and why is it used?**
   🔑 *Maps keys + nodes onto a hash ring → a key goes to the next node clockwise; adding/removing a node only remaps ~1/N keys (vs rehash-everything with `mod N`); virtual nodes smooth distribution + avoid hotspots; used in distributed caches (Redis Cluster), sharded DBs, Cassandra/DynamoDB partitioning, load balancers*

**517. Explain data consistency models (strong, eventual, causal) and how to tune them.**
   🔑 *Strong = read sees the latest write (RDBMS, quorum R+W>N, linearizable) — higher latency; eventual = replicas converge after a lag (Dynamo/Cassandra) — high availability; causal = preserves cause-effect order; read-your-writes / monotonic reads session guarantees; tune via quorum (N/R/W), read from primary vs replica, versioning/vector clocks; CAP/PACELC drives the choice*

**518. Design an idempotent payment/transaction API that prevents double-charging on retry.**
   🔑 *Client-generated idempotency key (UUID) in a header → unique constraint on `idempotency_key`; on retry return the cached response; atomic check-and-insert (unique constraint or SERIALIZABLE), distributed lock for concurrent in-flight dup; outbox for reliable event publishing; saga for multi-service atomicity; audit every attempt; charge exactly once; test = simulate timeout → retry → assert single charge (Stripe's idempotency-key as reference)*

**519. Design a rate limiter for an API gateway at scale.**
   🔑 *Algorithms = token bucket (bursty, smooth refill), fixed window (simple, boundary spike), sliding-window log (accurate, memory-heavy), sliding-window counter (practical hybrid); Redis INCR+EXPIRE / sorted set + Lua for atomic distributed counting; key by API key/user/IP; 429 + Retry-After + X-RateLimit-* headers; enforce at gateway (AWS WAF/Kong/Nginx) + in-app for business quotas*

**520. Design a notification service (email/SMS/push) with retry and dedup.**
   🔑 *Notification service subscribes to domain events (OrderPlaced → send); Kafka topic per channel + consumer group per channel; retry topics with exponential backoff → DLQ after max; dedup by unique notification_id (Redis SETNX / DB unique); template engine for rendering; delivery-tracking table (PENDING/SENT/FAILED); user preference/opt-out; third-party delivery (Twilio/FCM); priority queues for urgent*

**521. Design a distributed unique ID generator (Snowflake) across regions.**
   🔑 *Snowflake 64-bit = sign(1) + timestamp-ms(41, ~69y) + datacenter(5) + machine(5) + sequence(12 → 4096/ms/node); monotonic → good index locality; no coordination; handle clock skew (reject/wait on backwards clock); alternatives = UUIDv4 (random, no order), ULID (sortable, URL-safe), DB sequence (bottleneck at scale), Redis INCR*

**522. Design a real-time fraud detection system on transaction events.**
   🔑 *Kafka transaction stream → Flink/Kafka Streams for <100ms decisions; feature store for entity profiles (velocity, history); rule engine (Drools) + ML model ensemble; CQRS (write path triggers check, read path for analysts); threshold tuning (false positive = friction, false negative = fraud passes); async enrichment for non-latency-sensitive signals; review queue + feedback loop to retrain*

**523. Design a reconciliation system comparing two large datasets (ledger vs settlement file).**
   🔑 *Batch (nightly/T+1); load into staging; sorted-merge join on txn id + amount + date (or hash per partition); categorize mismatches (missing-in-source, missing-in-target, amount mismatch); persist to a mismatch table; auto-reconcile timing differences, alert the rest for manual review; idempotent + re-runnable; off-peak schedule; account for the settlement delay window*

**524. How do you scale a service handling millions of transactions/day?**
   🔑 *Redis cache for hot data (cache-aside + TTL), DB read replicas (reads → replica, writes → primary), async non-critical work via Kafka (notifications/audit/reporting), partition/shard the transaction table (by date or account id), CQRS read model, horizontal pod autoscaling, connection pooling (PgBouncer), CDN for cacheable responses, multi-region for DR*

**525. Design a URL shortener.**
   🔑 *Generate a short key = base62 of an auto-increment id or a Snowflake/hash (collision check); KV store (Redis/DynamoDB) `shortKey → longURL`; 301/302 redirect; read-heavy → cache + CDN; custom aliases + expiry; analytics via async event; estimate: billions of URLs → ~7 base62 chars; avoid predictable sequential keys if enumeration is a concern*

**526. Design a news feed / timeline.**
   🔑 *Fan-out-on-write (push to each follower's feed cache — fast reads, costly for celebrities) vs fan-out-on-read (pull + merge at read — cheap writes, slower reads); hybrid (push for most, pull for celebrities); feed store in Redis per user; ranking (time + ML relevance); pagination via cursor; precompute + cache; CDN for media*

**527. Design a chat / messaging system.**
   🔑 *WebSocket for real-time bidirectional; message service persists to a partitioned store (Cassandra by conversation id); delivery receipts + read status; online presence (Redis + heartbeat); push notifications when offline; ordering per conversation (sequence/timestamp); fan-out for group chats; E2E encryption optional; at-least-once + client dedup by message id*

**528. Design a typeahead / autocomplete.**
   🔑 *Trie/prefix tree of top queries + frequency; precompute top-k per prefix, cache in Redis; update frequencies async from a query log (stream); rank by popularity + personalization + recency; debounce client requests; Elasticsearch completion suggester as an alternative; sharding by prefix; sub-100ms latency target*

**529. Design a distributed cache.**
   🔑 *Partition keys across nodes via consistent hashing (virtual nodes); replication for availability; eviction (LRU/LFU); write policy (write-through/back/around); invalidation (TTL/events); client-side routing or a proxy; handle hot keys (local cache/replication); Redis Cluster/Memcached; cache stampede protection; monitor hit ratio + evictions*

**530. Design a system for real-time data across multiple regions.**
   🔑 *Globally distributed DB (Spanner/CockroachDB — strong consistency via consensus) or Aurora Global DB (primary + <1s cross-region replicas); Kafka MirrorMaker 2 for cross-region event replication; CDN + edge cache for reads; active-active with conflict resolution (last-write-wins/CRDTs) for AP data, consensus (Raft/Paxos) for financial CP data; Route53 latency routing + health-check failover*

**531. How do you handle UTC and time zones in a multi-region application?**
   🔑 *Store timestamps in UTC (`Instant` / TIMESTAMP WITH TIME ZONE) — never `LocalDateTime` in the DB (no zone); convert at the presentation layer (`ZonedDateTime` with `ZoneId.of("Asia/Kolkata")`); `LocalTime` is zone-free (ambiguous across regions); UTC avoids DST bugs; Hibernate configured to UTC; send ISO-8601 with offset over the wire*

**532. Design a DB schema for org → department → user → role → permission (with exceptions).**
   🔑 *organizations, departments (org_id FK), users (dept_id FK), roles (org_id), user_roles (composite PK), permissions (resource, action), role_permissions, exceptional_permissions (user_id, permission_id, granted BOOL, reason); RBAC with an exception overlay; effective permissions = role_permissions UNION granted exceptions MINUS revoked; index (user_id, permission_id); audit_log for changes*

**533. Design a promo/offer system — which fields, and how to enable a code for a specific day or week of month?**
   🔑 *Fields: code (unique), type (PERCENT/FLAT/BOGO), value, valid_from/to, day_of_week_mask (bitmask), week_of_month, max_uses, current_uses, min_order_amount, user_id (null=all); store in DB for auditable finite codes; for huge scale use HMAC-signed codes (rules encoded in the token, validated without a lookup); Redis cache for hot lookups; rule engine for complex validity; cleanup job for expired codes*

**534. Design driver/trip tracking without overloading the DB with GPS pings.**
   🔑 *Real-time location in Redis (GEOADD + TTL — only current position, fast); persist to DB only significant events (trip start/end, waypoints every N seconds) not every ping; trip state machine (REQUESTED→MATCHED→ONGOING→COMPLETED→BILLED) in DB; WebSocket/SSE pushes live location to the client; history in DB for billing/audit; raw GPS traces to cold storage (S3) after 90 days*

**535. Find the nearest drivers to a pickup location at scale.**
   🔑 *Redis GEOADD/GEOSEARCH for in-memory real-time proximity (drivers update every few seconds); or PostGIS (`GEOGRAPHY(POINT)` + `ST_DWithin`/`ST_Distance` + GiST spatial index); GeoHash bucketing for approximate sharding; filter by availability + bounded radius; expand radius if too few results; JPA native query for PostGIS*

**536. Two riders at the same cafe get matched to the same driver — how do you prevent double booking?**
   🔑 *Atomic check-and-assign: `UPDATE drivers SET status='ASSIGNED' WHERE id=? AND status='AVAILABLE'` → first wins (affected rows=1), second gets 0 → retry next driver; or `SELECT ... FOR UPDATE` pessimistic lock, or Redis SETNX on driver_id for the matching window; saga rolls back if booking fails after assignment; idempotent assignment by request id*

**537. How would you design a coding platform (LeetCode-style)?**
   🔑 *Problem service (store/serve), user service (auth), submission service → queue (Kafka/SQS); execution service = sandboxed per-language runner in a Docker container with CPU/memory/time limits + no network; judge compares output vs expected test cases (in S3); result service stores + queries; Redis sorted set for leaderboard; WebSocket for live result push; rate limit per user; strong isolation for untrusted code*

**538. Design a distributed job scheduler / task queue.**
   🔑 *Durable job store (DB/Redis) with status + run_at + priority; workers poll with `SELECT ... FOR UPDATE SKIP LOCKED` (or a broker like SQS/Kafka) so one worker per job; leader election or DB lock (ShedLock/Quartz cluster) so cron fires once across instances; retries with backoff + max attempts → DLQ; idempotent jobs; visibility timeout/lease to recover crashed workers; exactly-once effect via idempotency keys; scale workers horizontally; observability on queue depth + lag*

**539. Design an object storage service (S3-like).**
   🔑 *Metadata service (bucket/object/version → location, in a scalable DB) + data stored as chunks across storage nodes with replication/erasure coding for durability; consistent hashing to place chunks; multipart + resumable upload; content-addressing (hash) for dedup; pre-signed URLs for direct client upload/download; eventual vs strong read-after-write consistency; lifecycle/tiering (hot→cold/Glacier); CDN in front; integrity checksums*

**540. Design a metrics / monitoring and alerting system.**
   🔑 *Agents/exporters expose metrics → pull (Prometheus scrape) or push (StatsD/OTel collector); time-series DB (Prometheus/VictoriaMetrics/Timescale) with downsampling + retention; labels/dimensions + cardinality control; query/dashboards (Grafana); alerting rules + burn-rate on SLOs → Alertmanager → PagerDuty; high write throughput + aggregation; traces/logs correlation; scale via sharding/federation*

**541. Design a reliable webhook delivery system.**
   🔑 *Persist outbound events (outbox) → delivery workers POST to subscriber URLs; retries with exponential backoff + jitter + max attempts → dead-letter + manual replay; HMAC signature header for authenticity + timestamp (replay protection); at-least-once → consumers must be idempotent (event id); per-subscriber rate limiting + circuit breaker; delivery status + audit; respect subscriber 2xx as ack; ordering optional via per-subscriber queue*

**542. How do you do back-of-the-envelope capacity estimation in a design?**
   🔑 *Estimate DAU → QPS (peak ≈ 2-10× average), read:write ratio; storage = records × size × retention × replication; bandwidth = QPS × payload; memory for cache = hot-set size; derive #servers from per-instance throughput + headroom; sanity-check numbers (seconds/day ≈ 86400, 1M writes/day ≈ ~12/s avg); state assumptions explicitly; use these to justify sharding/caching/replicas*

**543. Design a double-entry ledger / wallet for payments.**
   🔑 *Append-only immutable journal: every transaction = balanced debit + credit entries (sum to zero); account balances = sum of entries (or maintained + reconciled); idempotency key per transaction (no double-post); ACID within a transaction, outbox for events; never UPDATE/DELETE entries (reversal = compensating entries); strong consistency (CP), optimistic locking on balance, audit + reconciliation job; partition by account, snapshots for fast balance reads*

[⬆️ Back to top](#contents)

## Kotlin vs Java

**544. Explain null safety in Kotlin (?, !!, ?:, ?.) vs Java's Optional/NPE.**
   🔑 *`?` nullable type (`String?`), `!!` non-null assertion (throws — avoid), `?:` Elvis default, `?.` safe call (short-circuits to null); compile-time null safety; Java `Optional` = explicit wrapper (of/ofNullable/map/orElseThrow); Kotlin preferred — zero boilerplate, enforced by the compiler; platform types from Java interop need care*

**545. Data classes vs POJOs/records — what does Kotlin generate?**
   🔑 *`data class` auto-generates equals/hashCode (by primary-constructor props), toString, copy() (immutable-style copy), componentN() (destructuring); Java equivalent = records (always immutable, Java 16+); Kotlin data class allows `var`; Lombok `@Data` was the pre-record Java stand-in*

**546. val vs var, and coroutines vs Java threads/virtual threads.**
   🔑 *`val` = immutable reference (like final), `var` = mutable; coroutines = lightweight (millions), `suspend` functions pause without blocking an OS thread, structured concurrency scopes, cooperative cancellation; Java threads = 1:1 OS threads (expensive); Java 21 virtual threads = coroutine-like scalability but transparent blocking (no suspend/async syntax)*

**547. What problem do extension functions solve?**
   🔑 *Add methods to existing/third-party classes without inheritance or editing source (`fun String.toTitleCase(): String`); compiled to a static method with the receiver as first param (no runtime overhead); no access to private members; the Kotlin stdlib (`filter`/`map`/`let`) is built on them*

**548. Coding: safely chain nullable calls with ?.let{}.**
   🔑 *`val city = user?.address?.let { it.city ?: "Unknown" }`; `let` runs only if non-null (receiver as `it`); scope functions = let/run/with/apply/also; Java equivalent = `Optional.ofNullable(user).map(User::getAddress).map(Address::getCity).orElse("Unknown")` — Kotlin is more concise*

**549. Explain Kotlin scope functions (let, run, with, apply, also).**
   🔑 *`let` (transform, receiver=`it`, returns lambda result), `run` (receiver=`this`, returns result), `with` (non-extension, `this`), `apply` (configure, `this`, returns receiver), `also` (side-effect, `it`, returns receiver); pick by receiver reference (this vs it) and return value (lambda result vs receiver)*

**550. Explain sealed classes and when-expressions in Kotlin.**
   🔑 *`sealed` = closed hierarchy known at compile time (like Java sealed) → exhaustive `when` with no `else`; model ADTs / result types (Success/Error); smart-casts in each branch; pairs well with data classes + pattern-style destructuring*

**551. What are inline functions, reified types and higher-order functions?**
   🔑 *Higher-order = functions taking/returning functions (lambdas); `inline` = lambda body inlined at call site → no lambda object allocation + enables `reified` type params (access `T` at runtime, unlike Java erasure — `inline fun <reified T> ...`); use for hot higher-order calls + type-based dispatch*

**552. How does Kotlin interoperate with Java?**
   🔑 *Fully bidirectional — call Java from Kotlin and vice-versa; `@JvmStatic`/`@JvmOverloads`/`@JvmField`/`@JvmName` control the generated bytecode; platform types (`String!`) from Java have no null info (handle carefully); SAM conversion for Java functional interfaces; Kotlin stdlib compiles to JVM bytecode; mix in the same Spring Boot project*

[⬆️ Back to top](#contents)

## Version Control, SDLC & Documentation

**553. Explain the Gitflow branching strategy (and alternatives).**
   🔑 *main (prod), develop (integration), feature/* (from develop, PR back), release/* (prep, merge to main+develop, tag), hotfix/* (from main, merge to main+develop); long-lived = main+develop; alternatives = trunk-based development (short-lived branches + feature flags, CI-friendly, preferred for fast delivery), GitHub Flow (main + feature branches only)*

**554. Merge vs rebase — and how do you resolve conflicts?**
   🔑 *Merge = merge commit, preserves history, safe on shared branches; rebase = replays commits for linear history (never rebase public/shared branches — rewrites history); squash merge = collapse a feature into one commit; conflict = edit markers → `git add` → `--continue`; `git rerere` for recurring conflicts; interactive rebase (`-i`) to clean commits*

**555. reset vs revert vs cherry-pick — when do you use each?**
   🔑 *`git reset` moves the branch pointer (--soft keeps staged, --mixed unstages, --hard discards) — local history rewrite, don't on shared; `git revert` creates a new commit undoing a prior one — safe on shared branches; `git cherry-pick` applies a specific commit onto the current branch (hotfix backport); `git bisect` to binary-search a regression*

**556. What's your peer-review process — what do you look for in a PR?**
   🔑 *Correctness (solves the problem), tests (edge cases not just happy path), security (input validation, no secrets, injection risk), performance (N+1, missing index, bad algorithm), breaking changes (API/DB migration safety), readability + naming, SOLID violations; constructive comments, approve-with-nits, request-changes only for real issues; keep PRs small*

**557. Compare Waterfall, Scrum, Kanban and SAFe.**
   🔑 *Waterfall = sequential, fixed scope, stable/regulated requirements; Scrum = sprints + backlog + ceremonies + velocity, evolving requirements; Kanban = continuous flow + WIP limits, ops/support/maintenance; SAFe = scaled agile (Program Increments, ARTs) for large multi-team programmes*

**558. How do you estimate effort / story points?**
   🔑 *Story points = relative complexity + effort + uncertainty (not hours); Fibonacci scale, planning poker for consensus, a reference story as baseline, T-shirt sizing for epics; split stories > 8 points; account for unknowns, tech debt, testing, review; track velocity for capacity; re-estimate on scope discovery*

**559. How do you document APIs and write good service documentation?**
   🔑 *springdoc-openapi auto-generates OpenAPI 3 from `@Operation`/`@Schema`/`@ApiResponse`, served at `/swagger-ui.html`; API-first = write the spec first → generate stubs; also README (quick start, env vars, architecture), ADRs (why not what), runbook (deploy/rollback/incident steps), C4 diagrams; keep docs in the repo, updated + reviewed in PRs*

**560. What semantic versioning do you use for artifacts/APIs?**
   🔑 *SemVer MAJOR.MINOR.PATCH — MAJOR = breaking, MINOR = backward-compatible feature, PATCH = backward-compatible fix; pre-release/build metadata (`-rc.1`, `+sha`); drives dependency ranges + API deprecation; tag releases in git; separate internal build number (git SHA) from the public version*

**561. What are the challenges in migrating to a new Spring/Java version?**
   🔑 *Spring Boot 2→3 = `javax.*`→`jakarta.*`, Security config API rewrite (WebSecurityConfigurerAdapter removed), Hibernate 6 changes; Java 11→17→21 = module/reflective-access restrictions (`--add-opens` for Mockito/Spring), removed deprecated APIs, dependency compatibility matrix; run full integration suite, fix tests first, consider GraalVM native if used*

[⬆️ Back to top](#contents)

## Behavioral / Managerial

> *Answer with STAR (Situation · Task · Action · Result). Pre-select 4–5 real incidents and reuse them across several questions.*

**562. Tell me about the most critical production incident you've owned.**
   🔑 *STAR: Situation (what broke, user impact, severity); Task (your role — incident lead/on-call); Action (triage → correlate with deploy → logs/traces → root cause → hotfix/rollback → communicate); Result (MTTR, users affected); postmortem = blameless, 5-whys, action items (test/alert/runbook/architectural fix), follow-through; show you improved the system, not just fixed the symptom*

**563. Describe a time you traded off shipping fast vs doing it right.**
   🔑 *Articulate the tech debt explicitly, get business alignment on risk, time-box the shortcut, create a ticket + ADR for the proper fix; never shortcut security/data integrity/compliance/PII; outcome = shipped on time + tracked + paid back; shows pragmatism + responsibility*

**564. Tell me about a time your service caused a cascading failure in another team's system.**
   🔑 *Lessons on blast-radius design: circuit breakers (callers fail fast), bulkheads (isolate per downstream), async decoupling (Kafka vs sync), timeout budgets, graceful degradation (cached/default), backward-compatible changes, dependency-graph mapping, cross-team incident comms*

**565. Describe a schema/API change you rolled out without breaking consumers.**
   🔑 *Expand-contract for schema (add nullable → dual-write → backfill → read-new → drop); API = new versioned endpoint, run v1+v2 in parallel, consumer-driven contract tests (Pact), deprecation timeline + Sunset header, feature flag, sunset after migration*

**566. Tell me about disagreeing with an architect/senior engineer.**
   🔑 *Gather evidence first (benchmark/PoC/references), present alternatives with trade-offs (not opinion), take it to design review with data, disagree-and-commit if overruled (unless safety/compliance), escalate for serious risk, document the dissent in an ADR, follow up with data post-implementation*

**567. Describe a performance problem whose cause was non-obvious.**
   🔑 *Non-obvious causes = GC stop-the-world pauses, DNS resolution timeouts, connection-pool exhaustion (waiting not executing), JIT warmup, serialization overhead, lock contention, cloud burst throttling; diagnosis = flame graph + thread dump + GC log + distributed trace, eliminate suspects systematically (not the first code you blame)*

**568. Tell me about mentoring a junior or leading a small delivery.**
   🔑 *Balance: short daily check-ins to catch blockers, pair on complex areas, code review as teaching (explain why), let them struggle productively within bounds, delegate with a review safety net, unblocking > personal coding, track team velocity not individual output, adjust to confidence level*

**569. Describe pushing back on an unrealistic timeline.**
   🔑 *Frame with data: velocity-based estimate, risk of rushing (quality/security/rework), options (MVP scope / add resource / accept risk with sign-off); present trade-offs + solutions, escalate with facts, document agreed scope in writing, retrospect to improve estimates*

**570. How would you debug a bug that only reproduces in production, not staging?**
   🔑 *Add structured logging/tracing around the suspect + deploy; raise log level via actuator `/loggers`; diff prod vs staging config (env/flags/pool/timeouts); check data-dependence (anonymized prod data in staging); shadow traffic; check scale/load dependence; add metrics around the suspect path*

**571. Describe an early design decision you'd change in hindsight.**
   🔑 *Examples = wrong data model (over/under-normalized), premature optimization, monolith-vs-modular, over-engineered abstraction, tight coupling; lesson = validate assumptions with a PoC, involve the team early, YAGNI, document why in ADRs*

**572. What are your strengths and weaknesses?**
   🔑 *Strengths = deep Java/Spring/microservices, systematic root-cause debugging, reliability-focused distributed design; weaknesses with a growth story = frontend (learning React), over-polishing before shipping (now time-box), assuming shared context (now clearer written comms); honest + self-aware + improving*

**573. Explain microservices to an 8-year-old.**
   🔑 *Instead of one big Lego castle doing everything, many small pieces each doing one job (orders / payments / emails); if one breaks only that piece needs fixing, the rest keep working; different kids build different pieces (teams); pieces pass notes to talk (APIs/messages); swap a piece without rebuilding the castle (independent deploy)*

**574. Walk me through your current and previous projects.**
   🔑 *STAR per project: what the system does, your role + team size, architecture + key decisions (and why), biggest challenge solved, a production issue + resolution, what you'd improve; memorize key metrics (TPS, uptime SLA, team size, timeline); tailor the story to the interviewer's domain (payments/telco/etc.)*

**575. Prepare managerial STAR stories.**
   🔑 *Have ready: leadership (led delivery with N engineers), conflict resolution (differing views → facilitated with data), delivery under pressure (re-scoped with PM), stakeholder communication (explained risk to non-technical audience), team building/onboarding, mentorship (review culture, pairing), disagreeing with a manager (data → commit)*

[⬆️ Back to top](#contents)

## Quick Config & Setup Reference

> *Copy-paste-style snippets for fast revision. Versions/props may differ slightly across Spring Boot 3.x — treat as a memory jog, not production config. Keep secrets out of files (use env/Vault).*

### Async (@Async + ThreadPoolTaskExecutor)

```java
@Configuration
@EnableAsync
public class AsyncConfig {
    @Bean("taskExecutor")
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor ex = new ThreadPoolTaskExecutor();
        ex.setCorePoolSize(8);
        ex.setMaxPoolSize(16);
        ex.setQueueCapacity(100);          // bounded = backpressure
        ex.setThreadNamePrefix("async-");
        ex.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        ex.setTaskDecorator(new MdcTaskDecorator());   // propagate MDC/SecurityContext
        ex.setWaitForTasksToCompleteOnShutdown(true);
        ex.initialize();
        return ex;
    }
    @Bean
    public AsyncUncaughtExceptionHandler asyncExceptionHandler() {
        return (ex, method, params) -> log.error("Async error in {}", method, ex);
    }
}
// usage: @Async("taskExecutor") returns void / CompletableFuture<T>
```

### Scheduling (@Scheduled + cluster-safe)

```java
@Configuration @EnableScheduling
class SchedulingConfig {
    @Bean TaskScheduler taskScheduler() {               // default scheduler is single-threaded!
        ThreadPoolTaskScheduler s = new ThreadPoolTaskScheduler();
        s.setPoolSize(5); s.setThreadNamePrefix("sched-"); return s;
    }
}
@Scheduled(cron = "0 0 2 * * *", zone = "UTC")   // 2 AM daily
@Scheduled(fixedDelay = 5000, initialDelay = 10000)   // ms, waits for prev to finish
@Scheduled(fixedRate = 5000)                          // ms, fixed cadence
// cron: sec min hour day month weekday
// Cluster: use ShedLock (@SchedulerLock) or Quartz so a job fires once across instances
```

### JPA / DataSource / Hibernate (application.yml)

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/app
    username: ${DB_USER}
    password: ${DB_PASS}
    hikari:
      maximum-pool-size: 10        # ~ cores*2, NOT hundreds
      minimum-idle: 5
      connection-timeout: 30000    # ms waiting for a connection
      idle-timeout: 600000
      max-lifetime: 1700000        # < DB-side timeout
  jpa:
    hibernate.ddl-auto: validate   # never 'update'/'create' in prod -> use Flyway
    open-in-view: false            # avoid OSIV connection-hold
    properties:
      hibernate:
        jdbc.batch_size: 50
        order_inserts: true
        order_updates: true
        default_batch_fetch_size: 16   # mitigate N+1
    show-sql: false
```

### Multiple DataSources (skeleton)

```java
@Primary @Bean @ConfigurationProperties("spring.datasource.a")
DataSource aDataSource() { return DataSourceBuilder.create().build(); }
@Bean @ConfigurationProperties("spring.datasource.b")
DataSource bDataSource() { return DataSourceBuilder.create().build(); }
// each gets its own LocalContainerEntityManagerFactoryBean + JpaTransactionManager
@EnableJpaRepositories(basePackages="com.app.a.repo",
    entityManagerFactoryRef="aEmf", transactionManagerRef="aTxManager")
// No single tx across both without XA/JTA -> per-DB tx + Saga/outbox
```

### REST / Web (Jackson, server, interceptors)

```yaml
server:
  port: 8080
  shutdown: graceful
  compression.enabled: true
spring:
  jackson:
    default-property-inclusion: non_null
    serialization.write-dates-as-timestamps: false   # ISO-8601
    time-zone: UTC
  mvc.problemdetails.enabled: true       # RFC 7807 error bodies
  lifecycle.timeout-per-shutdown-phase: 30s
```
```java
@Configuration
class WebConfig implements WebMvcConfigurer {
    @Override public void addInterceptors(InterceptorRegistry r) {
        r.addInterceptor(new RequestLoggingInterceptor());
    }
}
```

### CORS (global + Security integration)

```java
@Bean
CorsConfigurationSource corsConfigurationSource() {
    CorsConfiguration c = new CorsConfiguration();
    c.setAllowedOrigins(List.of("https://app.example.com"));  // never "*" with credentials
    c.setAllowedMethods(List.of("GET","POST","PUT","PATCH","DELETE","OPTIONS"));
    c.setAllowedHeaders(List.of("Authorization","Content-Type"));
    c.setAllowCredentials(true);
    c.setMaxAge(3600L);
    UrlBasedCorsConfigurationSource s = new UrlBasedCorsConfigurationSource();
    s.registerCorsConfiguration("/**", c);
    return s;
}
// In SecurityFilterChain: http.cors(cors -> cors.configurationSource(corsConfigurationSource()))
```

### Security Filter Chain (stateless JWT resource server)

```java
@Configuration @EnableWebSecurity
@EnableMethodSecurity   // enables @PreAuthorize/@PostAuthorize
class SecurityConfig {
  @Bean SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
      .cors(Customizer.withDefaults())
      .csrf(csrf -> csrf.disable())                                  // stateless token API
      .sessionManagement(s -> s.sessionCreationPolicy(STATELESS))
      .authorizeHttpRequests(a -> a
          .requestMatchers("/actuator/health", "/public/**").permitAll()
          .requestMatchers("/admin/**").hasRole("ADMIN")
          .anyRequest().authenticated())
      .oauth2ResourceServer(o -> o.jwt(Customizer.withDefaults()))   // validates JWT via JWKS
      .exceptionHandling(e -> e
          .authenticationEntryPoint(restAuthEntryPoint)   // 401
          .accessDeniedHandler(restAccessDeniedHandler)); // 403
    // custom filter: http.addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);
    return http.build();
  }
  @Bean PasswordEncoder passwordEncoder() { return new BCryptPasswordEncoder(); }
}
```

### Kafka Producer (application.yml + template)

```yaml
spring:
  kafka:
    bootstrap-servers: ${KAFKA:localhost:9092}
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
      acks: all                 # durability
      retries: 3
      properties:
        enable.idempotence: true
        linger.ms: 10
        max.in.flight.requests.per.connection: 5
      compression-type: snappy
```
```java
kafkaTemplate.send("orders", order.getId(), order);   // key -> partition/ordering
```

### Kafka Consumer (yml + listener + error handling + DLT)

```yaml
spring:
  kafka:
    consumer:
      group-id: order-service
      auto-offset-reset: earliest
      enable-auto-commit: false
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.ErrorHandlingDeserializer
      properties:
        spring.deserializer.value.delegate.class: org.springframework.kafka.support.serializer.JsonDeserializer
        spring.json.trusted.packages: "com.app.*"
        max.poll.records: 100
        max.poll.interval.ms: 300000
    listener:
      ack-mode: manual
      concurrency: 3            # <= partition count
```
```java
@Bean
DefaultErrorHandler errorHandler(KafkaTemplate<?,?> t) {
    var recoverer = new DeadLetterPublishingRecoverer(t);        // -> <topic>.DLT
    var h = new DefaultErrorHandler(recoverer, new ExponentialBackOff());
    h.addNotRetryableExceptions(IllegalArgumentException.class); // straight to DLT
    return h;
}
@KafkaListener(topics = "orders", groupId = "order-service")
public void onOrder(Order o, Acknowledgment ack) { process(o); ack.acknowledge(); }
```

### Kafka Topic / Partition (NewTopic bean)

```java
@Bean NewTopic ordersTopic() {
    return TopicBuilder.name("orders")
        .partitions(6)                 // parallelism cap; can only increase later
        .replicas(3)
        .config("min.insync.replicas", "2")     // with acks=all => no loss on 1 broker down
        .config("retention.ms", "604800000")    // 7 days
        .config("cleanup.policy", "delete")      // or "compact" for changelog
        .build();
}
```

### Feign Client (declarative HTTP + resilience)

```yaml
spring.cloud.openfeign.client.config:
  default: { connectTimeout: 2000, readTimeout: 5000, loggerLevel: basic }
  payment-service: { readTimeout: 3000 }
feign.circuitbreaker.enabled: true
```
```java
@EnableFeignClients
@FeignClient(name = "payment-service", url = "${payment.url}",
             configuration = FeignConfig.class, fallback = PaymentFallback.class)
interface PaymentClient {
    @PostMapping("/charge") ChargeResult charge(@RequestBody ChargeRequest r);
}
```

### WebClient (non-blocking) / RestClient (blocking)

```java
@Bean WebClient webClient(WebClient.Builder b) {
    HttpClient http = HttpClient.create()
        .responseTimeout(Duration.ofSeconds(5))
        .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 2000);
    return b.baseUrl("https://api.example.com")
            .clientConnector(new ReactorClientHttpConnector(http))
            .defaultHeader("Accept", "application/json")
            .filter(logRequest()).build();
}
// RestClient rc = RestClient.builder().baseUrl(url).requestInterceptor(auth).build();
```

### Resilience4j Circuit Breaker / Retry (application.yml)

```yaml
resilience4j:
  circuitbreaker.instances.paymentCb:
    sliding-window-type: COUNT_BASED
    sliding-window-size: 20
    minimum-number-of-calls: 10
    failure-rate-threshold: 50
    slow-call-rate-threshold: 80
    slow-call-duration-threshold: 2s
    wait-duration-in-open-state: 10s
    permitted-number-of-calls-in-half-open-state: 3
  retry.instances.paymentRetry:
    max-attempts: 3
    wait-duration: 500ms
    enable-exponential-backoff: true
  timelimiter.instances.paymentCb.timeout-duration: 3s
```
```java
@CircuitBreaker(name="paymentCb", fallbackMethod="fallback")
@Retry(name="paymentRetry")
public Result call() { ... }
public Result fallback(Throwable t) { return Result.cached(); }
```

### Caching (Redis / Caffeine)

```yaml
spring.cache.type: redis
spring.data.redis: { host: localhost, port: 6379 }
spring.cache.redis.time-to-live: 600000   # ms
```
```java
@EnableCaching
@Cacheable(value="products", key="#id", unless="#result == null")
@CacheEvict(value="products", key="#p.id")   // on update
// Caffeine: spring.cache.type=caffeine; spring.cache.caffeine.spec=maximumSize=10000,expireAfterWrite=5m
```

### Actuator (expose + health groups + separate port)

```yaml
management:
  server.port: 9090                      # separate management port
  endpoints.web.exposure.include: health,info,prometheus,metrics,loggers
  endpoint.health.probes.enabled: true   # /health/liveness & /readiness
  endpoint.health.show-details: when_authorized
  metrics.tags.application: ${spring.application.name}
```

### Flyway / Liquibase (DB migration)

```yaml
spring.flyway:
  enabled: true
  locations: classpath:db/migration    # V1__init.sql, V2__add_col.sql
  baseline-on-migrate: true
# Liquibase: spring.liquibase.change-log: classpath:db/changelog/db.changelog-master.yaml
```

### Validation (Bean Validation + global handler)

```java
@PostMapping("/users")
ResponseEntity<?> create(@Valid @RequestBody UserDto dto) { ... }

@RestControllerAdvice
class ApiExceptionHandler {
  @ExceptionHandler(MethodArgumentNotValidException.class)
  ProblemDetail onValidation(MethodArgumentNotValidException e) {
    var pd = ProblemDetail.forStatus(HttpStatus.BAD_REQUEST);
    pd.setDetail("Validation failed");
    pd.setProperty("errors", e.getBindingResult().getFieldErrors().stream()
        .collect(toMap(FieldError::getField, FieldError::getDefaultMessage)));
    return pd;
  }
}
```

### Profiles & Externalized Config

```yaml
# application.yml
spring.profiles.active: ${SPRING_PROFILES_ACTIVE:dev}
---
spring.config.activate.on-profile: prod
logging.level.root: WARN
# Override precedence (low->high): jar yml < profile yml < env vars < system props < CLI args
# Type-safe: @ConfigurationProperties(prefix="app") + @Validated
```

### CQRS (structural skeleton — not a property file)

```text
Command side (write):  Controller -> CommandHandler -> Aggregate -> write DB (normalized, @Transactional)
                       -> publish domain event (outbox) ->
Query side (read):     event consumer -> projection updater -> read DB (denormalized / materialized view)
                       Query API reads ONLY the read model (fast, no joins)
Consistency: eventual (projection lag); read-your-writes via version token / route to write model;
idempotent + ordered projection handlers; replay events to rebuild projections.
```

### Logging (logback-spring.xml JSON + levels)

```xml
<!-- logback-spring.xml : JSON logs + MDC traceId, async -->
<configuration>
  <appender name="JSON" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
  </appender>
  <appender name="ASYNC" class="ch.qos.logback.classic.AsyncAppender">
    <appender-ref ref="JSON"/>
  </appender>
  <root level="INFO"><appender-ref ref="ASYNC"/></root>
</configuration>
```
```yaml
logging.level: { com.app: DEBUG, org.hibernate.SQL: DEBUG }
# runtime change: POST /actuator/loggers/com.app  {"configuredLevel":"DEBUG"}
```

### Basic POM dependencies (Spring Boot starters)

```xml
<parent><groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId><version>3.3.x</version></parent>
<dependencies>
  <dependency><groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId></dependency>        <!-- REST + Tomcat -->
  <dependency><groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId></dependency>
  <dependency><groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId></dependency>
  <dependency><groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId></dependency>
  <dependency><groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId></dependency>
  <dependency><groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId></dependency>
  <dependency><groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId><scope>runtime</scope></dependency>
  <dependency><groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId><scope>test</scope></dependency>
</dependencies>
<!-- build: spring-boot-maven-plugin (fat jar / build-image) -->
```

[⬆️ Back to top](#contents)

## Coding Problems

**576. problem on number-to-words conversion.**
   🔑 *Divide into chunks: billions/millions/thousands/hundreds; word arrays: ones[]={"","one","two",...}, teens[]={"eleven","twelve",...}, tens[]={"","","twenty",...}; recursive helper converts chunk < 1000; handle edge cases: 0="zero", negative, exact millions/thousands; e.g. 1234 → "one thousand two hundred thirty four"; test: 0, 11, 100, 1000, 1000000, Integer.MAX_VALUE*

**577. Next palindrome in 100 to 1000, or next palidrome for 123 or 124 , ask for this is 131**
   🔑 *Brute force: iterate from n+1 upward; isPalindrome: s = Integer.toString(n); return s.equals(new StringBuilder(s).reverse().toString()); for 123 → 131; for 124 → 131; optimised: mirror first half (take first half, reverse it, compare; if less than second half increment first half); edge case: n=999 → 1001 (crosses digit boundary)*

**578. rotate list**
   🔑 *Three-reversal trick (O(n) time, O(1) space): k = k % n; reverse(0, n-1); reverse(0, k-1); reverse(k, n-1); or use Collections.rotate(list, k) for ArrayList; LinkedList: reconnect tail to head, advance to new tail; modulo handles k > n; handle k=0 (no-op); handle empty list; test: rotate [1,2,3,4,5] by 2 → [4,5,1,2,3]*

**579. shift all null or 0's in string list or int list**
   🔑 *Two-pointer: maintain write pointer (starts at 0); iterate read pointer; if element is non-null and non-zero, write to write-pointer position and advance; fill remaining positions with null/0; preserves relative order of non-zero elements; O(n) time O(1) space; Stream alternative: list.stream().filter(x -> x != null && x != 0).collect(toList()) then append zeros (creates new list); test: [1,0,2,null,3] → [1,2,3,0,null]*

**580. find second highest/lowest using steam**
   🔑 *Second highest: list.stream().filter(Objects::nonNull).distinct().sorted(Comparator.reverseOrder()).skip(1).findFirst().orElseThrow(); or .sorted(reverseOrder()).distinct() for de-dup before skip; IntStream: IntStream.of(arr).boxed().distinct().sorted(reverseOrder()).skip(1).findFirst(); O(n log n); O(n) approach: single pass tracking top1 and top2; handle all equal elements (no second distinct) → empty Optional*

**581. Find the mountain (start index, end index, width):**
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

### SQL Query Problems

**582. Find the Nth highest salary.**
   🔑 *`SELECT DISTINCT salary FROM emp ORDER BY salary DESC LIMIT 1 OFFSET N-1`; or window `SELECT salary FROM (SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) r FROM emp) t WHERE r = N`; DENSE_RANK handles ties; handle N > distinct count → NULL*

**583. Second highest salary without LIMIT.**
   🔑 *`SELECT MAX(salary) FROM emp WHERE salary < (SELECT MAX(salary) FROM emp)`; returns NULL if only one value; or DENSE_RANK = 2*

**584. Highest-paid employee per department.**
   🔑 *`SELECT * FROM (SELECT e.*, RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) r FROM emp e) t WHERE r = 1`; RANK/DENSE_RANK keeps ties, ROW_NUMBER picks exactly one; correlated-subquery alternative is slower*

**585. Top N rows per group.**
   🔑 *`ROW_NUMBER() OVER (PARTITION BY group_col ORDER BY sort_col DESC)` then `WHERE rn <= N`; index on (group_col, sort_col) for performance; lateral join alternative in Postgres*

**586. Find duplicate rows.**
   🔑 *`SELECT col, COUNT(*) FROM t GROUP BY col HAVING COUNT(*) > 1`; for full-row dups group by all columns; window `COUNT(*) OVER (PARTITION BY col)` to keep row detail*

**587. Delete duplicates keeping one.**
   🔑 *Postgres: `DELETE FROM t WHERE ctid NOT IN (SELECT MIN(ctid) FROM t GROUP BY dup_cols)`; or CTE with `ROW_NUMBER() OVER (PARTITION BY dup_cols ORDER BY id)` and delete where rn > 1; keep the smallest id/ctid*

**588. Employees earning more than their manager (self-join).**
   🔑 *`SELECT e.name FROM emp e JOIN emp m ON e.manager_id = m.id WHERE e.salary > m.salary`; self-join the table aliased twice on manager_id = id*

**589. Running / cumulative total.**
   🔑 *`SUM(amount) OVER (PARTITION BY account ORDER BY txn_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)`; window frame controls running vs moving-average; `ORDER BY` defines accumulation order*

**590. Month-over-month growth %.**
   🔑 *`LAG(revenue) OVER (ORDER BY month)` for the previous value → `(cur - prev)/prev * 100`; NULLIF to avoid divide-by-zero; `DATE_TRUNC('month', ...)` + GROUP BY to build monthly totals first*

**591. Find consecutive records (e.g., 3 logins in a row / islands).**
   🔑 *`LAG`/`LEAD` to compare neighbours, or gaps-and-islands: `ROW_NUMBER() - value/date` grouping trick to label consecutive runs, then HAVING COUNT >= 3; classic "consecutive numbers" / "consecutive days" pattern*

**592. Find gaps in a sequence of ids.**
   🔑 *`SELECT id + 1 AS gap_start FROM t t1 WHERE NOT EXISTS (SELECT 1 FROM t t2 WHERE t2.id = t1.id + 1)`; or compare `id` with `LEAD(id)`; generate_series to find all missing values in a range*

**593. Departments with no employees (anti-join).**
   🔑 *`SELECT d.* FROM dept d LEFT JOIN emp e ON e.dept_id = d.id WHERE e.id IS NULL`; or `NOT EXISTS`; count-including-zero = LEFT JOIN + `COUNT(e.id)` (not `COUNT(*)`) GROUP BY dept*

**594. Pivot rows to columns.**
   🔑 *Conditional aggregation `SUM(CASE WHEN month='Jan' THEN amt END) AS jan, ...`; Postgres `crosstab` (tablefunc) or `FILTER (WHERE ...)`; unpivot with UNION ALL or `LATERAL`/`unnest`*

**595. Median salary.**
   🔑 *`PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary)` (Postgres), or window with `ROW_NUMBER` + `COUNT` picking the middle row(s); MySQL lacks a native median → emulate with ranking*

**596. Customers with no orders in the last 90 days.**
   🔑 *`LEFT JOIN orders ... AND o.created_at >= now() - interval '90 days'` then `WHERE o.id IS NULL`; or `NOT EXISTS` with the date predicate; index on (customer_id, created_at)*

**597. Count distinct active users per day (DAU).**
   🔑 *`SELECT DATE(ts) d, COUNT(DISTINCT user_id) FROM events GROUP BY DATE(ts) ORDER BY d`; COUNT(DISTINCT) is expensive at scale → approximate with HyperLogLog (`approx_count_distinct`/Redis PFCOUNT)*

### More Java Coding Problems

**598. Reverse the words in a sentence.**
   🔑 *`String.join(" ", new StringBuilder-reverse of split)` or `Arrays.asList(s.trim().split("\\s+"))` → `Collections.reverse` → join; or two-pointer in a char[] for O(1) space; handle multiple spaces*

**599. First non-repeating character in a string.**
   🔑 *`LinkedHashMap<Character,Integer>` counts preserving order → first entry with count 1; or int[256] frequency + second pass; stream: `s.chars().boxed().collect(groupingBy(..., LinkedHashMap::new, counting()))`*

**600. Check anagrams / group anagrams.**
   🔑 *Anagram = sort both strings and compare, or compare frequency arrays (int[26]); group anagrams = HashMap keyed by sorted string (or frequency signature) → list of words; O(n·k log k)*

**601. Two-sum (indices summing to target).**
   🔑 *HashMap of value→index, for each x check if `target - x` seen → O(n); return the pair of indices; sorted-array variant = two pointers O(n) after sort*

**602. Merge overlapping intervals.**
   🔑 *Sort by start; iterate, if current.start <= last.end merge (`end = max(end, cur.end)`) else add new; O(n log n); classic for calendars/ranges*

**603. Implement an LRU cache.**
   🔑 *`LinkedHashMap(capacity, 0.75f, true)` + override `removeEldestEntry` → O(1) get/put with access-order eviction; or HashMap + doubly-linked list manually; thread-safe variant uses locks / Caffeine*

**604. Check balanced parentheses.**
   🔑 *Stack: push opens, on a close pop and match the pair; valid if stack empty at end; map closing→opening; handle `()[]{}`; empty stack on close = invalid*

**605. Find the missing number in 1..n.**
   🔑 *Sum formula `n*(n+1)/2 - actualSum`, or XOR of all indices and values (overflow-safe); for multiple missing use a boolean/bitset; O(n) time O(1) space*

**606. Maximum subarray sum (Kadane's algorithm).**
   🔑 *`cur = max(x, cur + x); best = max(best, cur)`; O(n); track start/end indices for the subarray; handle all-negative (best starts at first element)*

**607. Count word frequencies (streams).**
   🔑 *`Arrays.stream(text.toLowerCase().split("\\W+")).filter(not(String::isBlank)).collect(groupingBy(identity(), counting()))`; sort entries by value desc for top-k; `TreeMap` for sorted keys*

**608. Longest substring without repeating characters.**
   🔑 *Sliding window + HashMap/int[] of last-seen index; move left to `max(left, lastSeen+1)`; track max window length; O(n)*

**609. Reverse a linked list.**
   🔑 *Iterative: prev=null, cur=head, while cur { next=cur.next; cur.next=prev; prev=cur; cur=next }; return prev; O(n) O(1); recursive variant for practice*

**610. Detect a cycle in a linked list (Floyd's).**
   🔑 *Slow (1 step) + fast (2 steps) pointers; meet = cycle; to find the start, reset one to head and advance both by 1; O(n) O(1); HashSet alternative uses O(n) space*

**611. Producer-consumer with BlockingQueue.**
   🔑 *`BlockingQueue<T> q = new ArrayBlockingQueue<>(cap)`; producer `q.put(x)` (blocks when full), consumer `q.take()` (blocks when empty) → backpressure for free; poison-pill or `AtomicBoolean` + poll(timeout) for shutdown; multiple producers/consumers scale naturally*

**612. Group and aggregate with streams (e.g., total salary per department).**
   🔑 *`emps.stream().collect(groupingBy(Emp::getDept, summingDouble(Emp::getSalary)))`; downstream collectors (counting, averagingInt, mapping, maxBy); `collectingAndThen` to post-process*

**613. Sort a Map by value (top-N).**
   🔑 *`map.entrySet().stream().sorted(Map.Entry.comparingByValue(reverseOrder())).limit(n).collect(toMap(..., ..., (a,b)->a, LinkedHashMap::new))`; LinkedHashMap preserves the sorted order*

[⬆️ Back to top](#contents)

## Appendix — Context & Prep Notes

* *Prepared for a Senior Software Engineer (~10 YOE) interview loop*
* **Note on the interviewers:** LinkedIn blocks automated scraping of profile pages, so I couldn't pull full experience/skills sections for either profile. The one signal I could confirm: the nishit-shah-146707153 profile is titled "Senior Software Engineer at Mastercard." I couldn't verify anything for the kdpatel profile — no match was confirmable. Given the Mastercard signal, I've weighted the scenario section toward payments/fintech-style concerns (transaction integrity, idempotency, high throughput, security/compliance) — treat that as an educated guess, not a fact about what they'll ask.
* **How to use this:** For a 10-year-experience loop, interviewers rarely stay at "define X." Expect the pattern: define → probe internals → "why not the alternative?" → "tell me about a time you hit this in production." For every topic below, be ready to (1) explain it, (2) explain a failure mode/edge case, (3) tie it to a real project you shipped.
* **Suggested prep approach**
* For every "explain X" question, prepare a 60-second definition + a 2-minute real example from your own projects — that's the format senior interviewers actually reward.
* For the **System Design & Design Scenarios** section, practice sketching on a whiteboard/paper: components, data flow, failure modes, and one explicit trade-off you're making.
* For the **Behavioral / Managerial** war-story questions, pre-select 4–5 real incidents from your career and map each one to multiple questions above — you'll reuse the same stories across several answers.
* Given this stack, here's a full interview question set organized by category — theory questions interviewers typically ask, plus coding problems tied to the same skills.

[⬆️ Back to top](#contents)
