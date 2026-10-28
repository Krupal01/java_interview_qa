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
   - [SQL Fundamentals & DDL / DML / Config](#sql-fundamentals--ddl--dml--config) (Q261–273)
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
   🔑 *Erasure at compile time, bytecode has Object/bound, no reified types, no `new T[]`, unchecked cast, bridge methods, `Class<T>` token workaround; type erasure = generics are compile-time only, the JVM sees raw types at runtime; bridge methods are compiler-generated to maintain polymorphism after erasure (e.g. `Comparable`); `instanceof T` forbidden because T is erased → use `clazz.isInstance(obj)` with a `Class<T>` token passed in; `@SuppressWarnings("unchecked")` marks intentional casts; Kotlin reified type params avoid erasure via inlining*

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
   🔑 *`Class.forName`, `getDeclaredMethods/Fields`, `setAccessible`, constructor `newInstance`; used by Spring/Jackson/JUnit; cost = slower + breaks encapsulation + no compile-time safety + module/`setAccessible` restrictions (Java 9+); reflection bypasses private/protected access via `setAccessible(true)` (blocked by module system unless `opens` is declared); `getDeclaredFields()` vs `getFields()` (declared = all incl. private in this class; public = public in this + superclasses); `Method.invoke()` is ~50× slower than direct calls; used by DI containers, ORM mappers, serialization, test frameworks; `MethodHandles.lookup()` + `VarHandle` as a faster modern alternative; GraalVM native image requires reflection config (reflect-config.json)*

**13. How can you print to the console without a `main` method?**
   🔑 *Static initializer block runs at class load (worked pre-Java 7); modern JVMs require `main` → demo with static block + `System.exit(0)` to avoid the "no main" error*

**14. What is a static block, and can we have more than one?**
   🔑 *`static{}` runs once at class loading (initialization phase), multiple allowed and run top-to-bottom, before `main`; used for constants / driver loading*

**15. How can a singleton be broken, and how do you prevent it (including cloning)?**
   🔑 *Broken by: reflection on private constructor, deserialization, `clone()`, multiple classloaders, race in lazy init; fixes: enum singleton, `readResolve()`, throw in constructor if instance exists, override `clone()` to throw, double-checked locking + `volatile`*

**16. Can we override or overload the `main` method?**
   🔑 *Overload yes (different params); override no — `main` is static → method hiding, JVM only invokes the `String[]` version*

**17. Does overloading only work for objects?**
   🔑 *No — overloading is compile-time on the declared type and applies to primitives too; resolution order = exact → widening → autoboxing → varargs; widening of primitives = `byte→short→int→long→float→double`; autoboxing happens AFTER widening — `int` widens to `long` before boxing to `Integer`; varargs = last resort (chosen only if no other match); pitfall: `null` as argument causes ambiguity if multiple methods accept reference types; method reference overloading can cause compilation ambiguity*

**18. How does `new` work internally?**
   🔑 *Memory alloc in heap (TLAB — Thread-Local Allocation Buffer), default init to zero/null, super constructor chain, instance blocks, constructor body, reference assigned; TLAB = each thread has a local allocation buffer carved from Eden — avoids synchronization on every `new`; zero-init fills the memory with 0/false/null before the constructor runs; JVM bytecode `new` + `dup` + `invokespecial <init>`; escape analysis can eliminate heap allocation entirely (stack allocation / scalar replacement); object header = mark word (GC info, lock state, hash) + class pointer (klass); `newInstance()` via reflection vs direct `new` — reflection is slower and can fail if no-arg constructor is absent*

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
   🔑 *Compiler checks the abstract contract; class must be `abstract` or implement all abstract methods — else compile error; interface methods are implicitly `public abstract`; default methods provide optional implementations (class can override); if two interfaces have the same default method, the implementing class MUST override (diamond problem resolution); if a class inherits a method from a superclass that matches an interface method signature, the class method satisfies the interface — no override needed; `@Override` helps verify the contract is actually satisfied; anonymous class / lambda satisfies a functional interface with a single abstract method*

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
   🔑 *`T... args` compiles to a `T[]`; varargs is the last parameter; chosen only after exact and widening/boxing matches (lowest priority); ambiguity + possible `null`/array-vs-element pitfalls; passing `null` is ambiguous — compile error if multiple varargs overloads; passing an array directly is fine (`new int[]{1,2,3}`); calling with zero args = empty array (not null); `printf(String fmt, Object... args)` is the canonical example; `@SafeVarargs` suppresses unchecked-cast warnings on generic varargs (`void log(T... items)` → heap pollution risk); varargs + generics combination = `@SafeVarargs` needed on `final`/`static`/`private` methods*

**42. Explain try-with-resources and `AutoCloseable`.**
   🔑 *Resource declared in `try(...)` auto-closed in reverse order; resource must implement `AutoCloseable`/`Closeable`; suppressed exceptions (`getSuppressed()`); replaces finally boilerplate*

**43. What are the pitfalls of autoboxing/unboxing and the Integer cache?**
   🔑 *Autobox/unbox implicit conversion; `Integer` cache -128..127 → `==` true in range, false outside (use `equals`); `NullPointerException` unboxing a `null` Integer; boxing in loops = GC churn*

**44. How do annotations work — retention, target, meta-annotations, and custom annotations?**
   🔑 *`@Retention` (SOURCE/CLASS/RUNTIME), `@Target`, `@Inherited`, `@Documented`, `@Repeatable`; meta-annotations; read at runtime via reflection or at compile time via annotation processors (APT); drives Spring/JPA/validation*

**45. Deep copy vs shallow copy — and how do you copy an object correctly?**
   🔑 *Shallow = copies references (shared nested objects); deep = recursively copies nested state; `Object.clone()` is shallow + `Cloneable` contract pitfalls → prefer copy constructor, static factory, or serialization-based deep copy*

**46. `Serializable` vs `Externalizable`, `transient`, and `serialVersionUID`.**
   🔑 *Serializable = automatic via reflection; `transient`/`static` skipped; `serialVersionUID` controls version compatibility; `Externalizable` = manual `writeExternal`/`readExternal` (needs public no-arg ctor); custom `writeObject`/`readObject`/`readResolve` hooks; security risks of deserialization; `serialVersionUID` mismatch → `InvalidClassException` at deserialization; if not declared, JVM computes it from the class structure — any field/method change breaks compatibility; `readResolve()` used to enforce singleton after deserialization; `writeObject`/`readObject` for custom serialization logic; Externalizable is faster (no reflection) but requires explicit implementation; never deserialize untrusted data (gadget chains → RCE); prefer JSON/Protobuf for persistence/wire; `ObjectInputFilter` (JEP 290) to allowlist deserializable classes*

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
   🔑 *Define with `@Retention` (RUNTIME for reflection, SOURCE/CLASS for processors) + `@Target` + meta-annotations + `@Repeatable`; runtime = read via reflection (`getAnnotation`) — how Spring/JPA/validation work; compile-time = annotation processor (APT, `AbstractProcessor`, generates code like Lombok/MapStruct/Dagger); bytecode weaving (AspectJ) as a third option; annotation elements = methods (e.g. `String value() default ""`), can only be primitives/String/Class/enum/annotation/arrays of these; `@Inherited` makes annotation visible on subclasses (only for class-level); `@Documented` includes in Javadoc; `@Repeatable` + container annotation for multiple instances; Spring meta-annotation composition (e.g. `@RestController` is `@Controller`+`@ResponseBody`); APT generates files during compilation — `@AutoService` for service registration*

**53. Explain advanced generics — recursive bounds, wildcard capture, and heap pollution.**
   🔑 *Recursive bound `<T extends Comparable<T>>` (self-referential, enables builders/enums); wildcard capture (compiler infers a capture type for `?` — helper method to "capture"); PECS; type inference + diamond; heap pollution from unchecked varargs generics → `@SafeVarargs`; can't create `new T[]` / generic arrays due to erasure*

**54. Object identity vs equality — ==, equals, and identity hash code.**
   🔑 *`==` compares references (identity) for objects, values for primitives; `equals` = logical equality (override with hashCode); `System.identityHashCode` = JVM identity hash regardless of override; interning/canonicalization (String pool, Integer cache) makes `==` accidentally work; always `equals` for value comparison*

**55. Explain the equals/hashCode/compareTo consistency contract — especially for JPA entities.**
   🔑 *`equals` true ⇒ equal hashCode; `compareTo` should be consistent with `equals` (else TreeSet/TreeMap "lose" elements); for JPA entities use a stable business/natural key or assigned UUID (NOT the DB-generated id — it's null before persist, breaks Set membership across the transient→managed transition); avoid Lombok `@Data`/`@EqualsAndHashCode` over all fields on entities*

**56. Explain Java I/O vs NIO (channels, buffers, selectors) and when each matters.**
   🔑 *Classic java.io = blocking streams (one thread per connection); NIO = channels + buffers + selectors (non-blocking multiplexed I/O, one thread many connections — Netty), memory-mapped files, `Files`/`Path` (NIO.2); use NIO/Netty for high-connection servers, `Files.readString`/try-with-resources for simple file work; zero-copy `transferTo`; NIO key concepts: Channel (bidirectional, `FileChannel`/`SocketChannel`), Buffer (`ByteBuffer` — capacity/position/limit, `flip()`/`clear()`/`compact()`), Selector (monitors multiple channels for readiness events); `MappedByteBuffer` for memory-mapped files (direct OS page cache access); `AsynchronousFileChannel`/`AsynchronousSocketChannel` for async I/O; NIO.2 (`java.nio.file`) = `Path`/`Files`/`WatchService`/`FileVisitor`; with Java 21 virtual threads, blocking I/O scales comparably to NIO for most apps*

**57. What is the Java Platform Module System (JPMS) and why does it matter?**
   🔑 *Java 9 modules (`module-info.java`) = strong encapsulation (`exports`/`opens`/`requires`), reliable configuration, smaller runtimes (jlink); `opens` needed for reflection (Spring/Jackson/Mockito) → `--add-opens`; most apps still run on the classpath, but libraries and `jlink`/native images rely on it; `module-info.java` keywords: `module`, `requires` (compile+runtime dep), `requires transitive` (re-exports), `exports` (public API to all), `exports ... to` (qualified), `opens` (reflective access), `uses`/`provides` (ServiceLoader SPI); automatic modules = JARs on module path without module-info; unnamed module = classpath code sees everything; `--add-opens java.base/java.lang=ALL-UNNAMED` is the common patch for frameworks; `jlink` creates a custom JRE with only needed modules; `jdeps` analyzes dependencies*

**58. How do dynamic proxies, reflection and MethodHandles power frameworks?**
   🔑 *JDK dynamic proxy (interface + InvocationHandler) + CGLIB (subclass) back Spring AOP/`@Transactional`; reflection drives DI/serialization/ORM; MethodHandles/VarHandle (java.lang.invoke) = faster, type-safe low-level access + `invokedynamic` (lambdas, string concat, pattern switch); cost = startup + no compile-time safety*

[⬆️ Back to top](#contents)

## JVM Internals, Memory Model & Garbage Collection

**59. Walk through the JVM memory areas — heap, stack, metaspace, PC register, native method stack, code cache and direct memory.**
   🔑 *Heap (young/old, shared) + stack per thread (frames, locals, operand stack) + metaspace (class metadata, native memory, replaced PermGen in Java 8) + PC (Program Counter) register per thread + native method stack + JIT code cache + off-heap direct memory; method area = constant pool + statics*

**60. How does garbage collection work, and compare the collectors — Serial, Parallel, G1, ZGC, Shenandoah, Epsilon. When would you pick each?**
   🔑 *Reachability from GC roots, mark-sweep-compact; Serial = single-thread small heaps; Parallel = throughput batch; G1 = region-based balanced default; ZGC/Shenandoah = sub-millisecond pauses on huge heaps (concurrent/colored pointers); Epsilon = no-op for benchmarks; GC roots = stack frames, static fields, JNI references, class objects; mark phase = traverses object graph from roots; sweep = reclaims unreachable; compact = moves live objects to eliminate fragmentation; G1 = divides heap into equal-sized regions (~1-32MB), tracks liveness per region, picks the most-garbage regions first (Garbage-First), pauseMillis target via `MaxGCPauseMillis`; ZGC = concurrent relocation using colored pointer bits (load barriers), pauses <1ms even on multi-TB heaps; Shenandoah = Brooks pointers for concurrent compaction; Parallel = multiple GC threads but STW; `-XX:+UseG1GC` / `+UseZGC` / `+UseParallelGC`; enable GC logs: `-Xlog:gc*:file=gc.log:time,uptime:filecount=5,filesize=20m`*

**61. What causes a memory leak in Java despite GC? Give real examples.**
   🔑 *Static/long-lived collections, unbounded cache (no eviction), unclosed streams/listeners, ThreadLocal not removed in pooled threads, ClassLoader leaks, inner class holding outer, mutated map key; diagnose with heap dump + dominator tree (MAT)*

**62. Explain ClassNotFoundException vs NoClassDefFoundError vs NoSuchMethodError — how do you debug a dependency conflict?**
   🔑 *ClassNotFoundException = reflective load miss; NoClassDefFoundError = present at compile, missing/failed-init at runtime; NoSuchMethodError = compile-vs-runtime version mismatch (jar hell); fix via `mvn dependency:tree`, exclusions, shading, `-verbose:class`; ClassNotFoundException = thrown explicitly by `Class.forName()`/`ClassLoader.loadClass()` when the class is not found; NoClassDefFoundError = the class was available at compile time but not at runtime (JAR missing from runtime classpath, or static init block threw an exception causing the class to be in error state); NoSuchMethodError = a method existed in the version compiled against but was removed/changed in the runtime JAR; jar hell = multiple versions of the same library on the classpath; fix: `mvn dependency:tree | grep <artifact>`, add `<exclusion>`, use a BOM for version alignment, shading with `maven-shade-plugin` to relocate conflicting packages*

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
   🔑 *Interpreter first → profiles hot methods → C1 (client) then C2 (server) tiered compilation → native code in code cache; On-Stack Replacement (OSR) for hot loops, inlining, escape analysis; deoptimization when assumptions break; JVM warmup effect; tiered compilation levels 0-4: 0=interpreter, 1-3=C1 with profiling, 4=C2 (optimized native); hot threshold ~10k invocations; inlining = inserts callee code at call site (reduces call overhead + enables further optimization); escape analysis = proves an object doesn't escape → stack allocate or scalar replace; speculative optimizations = devirtualization (assume only one impl) → deoptimize back to interpreter if assumption breaks (class loading, new subclass); code cache size tuned with `-XX:ReservedCodeCacheSize`; `-XX:+PrintCompilation` to see compiled methods; GraalVM = advanced JIT with better inlining + partial escape analysis*

**70. Explain the class loading lifecycle and the classloader hierarchy.**
   🔑 *Loading → Linking (Verify, Prepare = default-init statics, Resolve) → Initialization (static blocks + static field assignment, lazy on first use); Bootstrap → Platform → Application classloaders, parent-delegation model; custom classloaders (plugins, hot reload) break jar-hell*

**71. What are the types of OutOfMemoryError and how do you diagnose each?**
   🔑 *`Java heap space` (leak/undersized → heap dump), `GC overhead limit exceeded`, `Metaspace` (classloader leak / too many classes), `Direct buffer memory` (off-heap), `unable to create new native thread` (OS thread limit / stack size); `-XX:+HeapDumpOnOutOfMemoryError`*

**72. StackOverflowError vs OutOfMemoryError — what's the difference?**
   🔑 *StackOverflowError = per-thread stack exhausted (deep/infinite recursion), tune `-Xss`; OutOfMemoryError = heap/metaspace/native exhausted; different memory regions, different fixes*

**73. Explain reference types — strong, soft, weak and phantom — and their use cases.**
   🔑 *Strong = normal (never GC'd while reachable); Soft = cleared under memory pressure (memory-sensitive cache); Weak = cleared at next GC (WeakHashMap, canonical maps); Phantom = post-mortem cleanup via ReferenceQueue (Cleaner replaces finalize)*

**74. What are escape analysis, scalar replacement and lock elision?**
   🔑 *JIT proves an object doesn't escape the method/thread → stack allocation / scalar replacement (no heap alloc) and lock elision/coarsening (removes needless synchronization); reduces GC and contention; escape analysis = C2 JIT tracks whether an object reference can be accessed from outside the current method/thread; if not, stack allocation (avoids heap alloc + GC pressure); scalar replacement = if the object escapes no JVM boundary, it's decomposed into individual fields (primitives) on the stack (no object header overhead); lock elision = if the locked object doesn't escape (thread-local), the lock is removed entirely; lock coarsening = adjacent synchronized blocks on the same monitor merged; `-XX:+DoEscapeAnalysis` (default on); view with `-XX:+PrintEliminateLocks`; StringBuffer in a local scope = classic lock elision example*

**75. Which JVM flags do you use to tune heap and GC?**
   🔑 *`-Xms`/`-Xmx` (initial/max heap), `-Xss` (thread stack), `-XX:MaxMetaspaceSize`, GC select (`-XX:+UseG1GC`/`UseZGC`), GC logging (`-Xlog:gc*`), `-XX:+HeapDumpOnOutOfMemoryError`, `-XX:MaxRAMPercentage` (containers); full production set: `-Xms512m -Xmx2g -Xss512k -XX:MaxMetaspaceSize=256m -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -Xlog:gc*:file=gc.log:time,uptime:filecount=5,filesize=20m -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/dumps/ -XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0`; G1 tuning: `NewRatio`, `G1NewSizePercent`, `G1HeapRegionSize`; ZGC: `-XX:+UseZGC -XX:ZUncommitDelay=300`; `+PrintFlagsFinal` to see all active flags; `-verbose:gc` older equivalent; never set `-Xms == -Xmx` in containers (wastes memory until needed)*

**76. What is a safepoint and a stop-the-world (STW) pause?**
   🔑 *Safepoint = a point where all threads are paused at a known state so the JVM can run GC/deopt/biased-lock revocation; time-to-safepoint latency; STW = all app threads frozen; long safepoints hurt p99 latency*

**77. How do you optimize JVM/application startup time?**
   🔑 *Class Data Sharing (CDS/AppCDS), Ahead-Of-Time (AOT) compilation, GraalVM native image (near-instant startup, low memory), tiered compilation tuning, lazy bean init, fewer classpath scans; trade-off = warmup vs peak throughput; CDS = pre-loads class metadata into a shared archive (`-Xshare:dump`/`-Xshare:on`), eliminates parse+verify on subsequent startups; AppCDS (Java 10+) = includes app classes in the archive; Spring AOT (Boot 3+) = pre-generates bean definitions + proxy bytecode at build time, eliminates runtime reflection/classpath scanning; GraalVM native-image = compiles to standalone binary (no JVM startup), near-instant start + low RSS — at cost of build time + no dynamic classloading; CRaC (Coordinated Restore at Checkpoint) = snapshot a warm JVM process + restore in ms; `-XX:TieredStopAtLevel=1` (dev only — disables C2, faster start, poor peak perf); `spring.main.lazy-initialization=true` delays non-critical beans*

**78. How does off-heap / direct memory work and how do you monitor native memory?**
   🔑 *`ByteBuffer.allocateDirect`/Netty for zero-copy I/O, lives outside heap (not bound by `-Xmx`), freed via Cleaner; `-XX:MaxDirectMemorySize`; Native Memory Tracking (NMT `-XX:NativeMemoryTracking`), compressed ordinary object pointers (oops) under 32 GB heap*

**79. How is the JVM container-aware, and how do you size heap in Kubernetes?**
   🔑 *Modern JVMs honour cgroup limits (`-XX:+UseContainerSupport` default) — detect container CPU/memory not host; set heap with `-XX:MaxRAMPercentage` (e.g. 75%) instead of fixed `-Xmx` so it scales with the pod limit; leave headroom for metaspace, thread stacks, direct/off-heap; align K8s memory limit with heap + non-heap or risk OOMKilled; CPU limits affect GC/JIT thread counts; before Java 8u191, JVM saw host memory, not cgroup limit → pod OOMKilled when heap was 75% of host; `UseContainerSupport` reads `/sys/fs/cgroup/memory.limit_in_bytes`; non-heap memory = Metaspace + thread stacks (~1MB each) + code cache + direct buffers + off-heap; rule of thumb: heap = 50-75% of container limit, leaving room for non-heap; CPU throttling causes GC/JIT threads to compete → set `GCTasksPerThread` or let the JVM auto-scale; `ActiveProcessorCount` overrides CPU detection (`-XX:ActiveProcessorCount=N`); K8s pod: set both `requests` and `limits` to the same value (Guaranteed QoS) for predictable scheduling*

**80. How do you profile a production JVM with low overhead (JFR, async-profiler)?**
   🔑 *Java Flight Recorder (JFR) = built-in, ~1% overhead, continuous recording of allocation/GC/locks/IO/exceptions → analyze in JDK Mission Control; async-profiler = low-overhead CPU + allocation flame graphs (no safepoint bias); `jcmd JFR.start/dump`; use for CPU hotspots, allocation pressure, lock contention, without stopping the app; JFR commands: `jcmd <pid> JFR.start duration=60s filename=recording.jfr settings=profile` → `JFR.dump`; JMC = JDK Mission Control analyzes the .jfr file — flame graph, method hotspots, GC events, lock contention, allocation by class; async-profiler: `./profiler.sh -d 30 -f flamegraph.html <pid>` — uses perf_events/JVMTI, avoids safepoint bias (traditional profilers only sample at safepoints → misleading); safepoint bias = methods that can only be interrupted at specific points appear over-represented; for allocation: `-e alloc` mode shows which code paths allocate the most; `jstack <pid>` for one-shot thread dump; `jmap -histo <pid>` for live histogram without a full heap dump*

**81. How do you choose and tune a garbage collector for a latency-sensitive service?**
   🔑 *G1 (default, balanced, `MaxGCPauseMillis` target, region sizing) for most; ZGC/Shenandoah for sub-millisecond pauses on large heaps (concurrent, colored pointers / Brooks pointers); Parallel for batch throughput; size the heap so old-gen churn is low, tune young-gen for allocation rate; measure with GC logs (`-Xlog:gc*`) + pause percentiles; avoid full GCs; decision matrix: p99 < 50ms on <16GB heap → G1 with `MaxGCPauseMillis=100`; p99 < 5ms or heap >16GB → ZGC; max throughput batch job → Parallel; G1 tuning: `-XX:G1HeapRegionSize=N` (1-32MB, larger for big heaps), `-XX:G1NewSizePercent`, `G1MaxNewSizePercent`, increase `-XX:MaxGCPauseMillis` budget to reduce frequency; diagnose full GC causes: humongous allocations (increase region size), concurrent mark not completing in time (increase heap), explicit `System.gc()` calls; GC log analysis tools: GCEasy.io, GCViewer*

**82. How do you reduce a service's JVM memory footprint?**
   🔑 *Right-size heap (`MaxRAMPercentage`), cap metaspace, reduce thread count + stack size (`-Xss`), bound caches/pools, prefer primitives + compact data, watch off-heap/direct buffers (Netty), compressed oops under 32 GB; String deduplication (G1); GraalVM native image for drastically lower memory; measure with NMT (Native Memory Tracking); NMT: `-XX:NativeMemoryTracking=summary` then `jcmd <pid> VM.native_memory summary` — shows heap/metaspace/thread/code/internal breakdown; compressed oops (UseCompressedOops, default <32GB heap) reduces reference size from 8→4 bytes → 30-40% less heap; String deduplication: `-XX:+UseStringDeduplication` with G1 — merges identical char arrays; reduce threads: use virtual threads (cheap) over platform threads; `-Xss256k` instead of default 1MB per platform thread saves memory for high thread-count services; off-heap tracking: `-XX:MaxDirectMemorySize=256m`; find memory hogs with MAT's Dominator Tree on a heap dump; GraalVM native-image = 10-100MB RSS vs 200-500MB+ for JVM*

[⬆️ Back to top](#contents)

## Collections & Data Structures

**83. Explain the equals()/hashCode() contract — what breaks if you override one but not the other, and what happens if you mutate a key used in a HashMap?**
   🔑 *Equal objects must have equal hashCodes (reverse not required); break it → lost/duplicate entries in HashMap/HashSet; consistency; bucket lookup = hashCode then equals; mutating a key changes its bucket → entry unreachable (effective leak); Lombok `@EqualsAndHashCode`, records auto-generate*

**84. Compare HashMap, LinkedHashMap, Hashtable and ConcurrentHashMap internals — how does ConcurrentHashMap stay thread-safe without locking the whole map?**
   🔑 *HashMap = unordered, not thread-safe, 1 null key; LinkedHashMap = insertion/access order via doubly-linked list (LRU); Hashtable = legacy, fully synchronized, no nulls; ConcurrentHashMap Java 7 = segment locks, Java 8+ = CAS on empty bin + `synchronized` on head node + treeify, weakly-consistent iterators, no null keys/values*

**85. Difference between List, Set and Map — when would you use LinkedHashMap vs TreeMap?**
   🔑 *List = ordered + duplicates; Set = unique; Map = key-value; LinkedHashMap keeps insertion/access order (LRU cache); TreeMap = sorted by Comparable/Comparator, O(log n), navigation methods*

**86. Why is an array's size fixed (from the JVM's perspective), and how does ArrayList grow dynamically?**
   🔑 *Array length stored in object header at allocation, one contiguous fixed block, no realloc; ArrayList = `Object[]` + lazy default capacity 10, grows when size == capacity via `grow()` → newCap = old + old/2 (1.5×) + `Arrays.copyOf`; presize with `new ArrayList<>(n)` to avoid copies; arrays are a special JVM type — the length is baked into the object at allocation time and immutable; `Arrays.copyOf` creates a NEW larger array and copies — O(n) time; amortized O(1) add due to geometric growth; Java 10+ uses `newLength = max(minCapacity, oldCapacity + max(oldCapacity>>1, 1))` ensuring at minimum 1 extra slot; `trimToSize()` releases unused capacity; `ensureCapacity(n)` pre-allocates; array covariance (`String[] isa Object[]`) is a type-system hole (ArrayStoreException at runtime) that generics fixed; 2D arrays = array of array references (jagged), not contiguous*

**87. If I add two logically-equal custom objects to a Set (e.g. `new User("a",12)` twice), will the Set allow both? How do you control uniqueness?**
   🔑 *Without overriding `equals`/`hashCode`, identity hash → both added; fix = override `equals`/`hashCode` (or use a record / Lombok `@Data`) or a `TreeSet` with a Comparator; key should be immutable so the hash never changes*

**88. Explain LinkedList, trees and graphs — common algorithms and implementations.**
   🔑 *LinkedList = doubly-linked nodes/pointers; trees = BST, AVL, red-black, trie, heap/PriorityQueue; graphs = adjacency list/matrix, BFS/DFS, Dijkstra, topological sort, cycle detection; recursion vs iteration*

**89. We can access a LinkedList by index, so why is an array (ArrayList) faster?**
   🔑 *Array = contiguous memory → O(1) random access + CPU cache locality; LinkedList index = O(n) traversal + pointer chasing + per-node object overhead; LinkedList wins only on middle insert/delete via an existing iterator*

**90. What is the load factor and initial capacity of a HashMap?**
   🔑 *Default load factor 0.75 = space/collision trade-off; resize (double capacity + rehash) when size > capacity × loadFactor; capacity always a power of two; set initial capacity to avoid rehashing; load factor = ratio of entries to buckets before resize; 0.75 balances space (too low = many buckets, low fill, memory waste) vs collisions (too high = more collisions, chain traversal); initial capacity default = 16; resize threshold = capacity × loadFactor = 16 × 0.75 = 12 entries before first resize; to avoid resize for N elements: `new HashMap<>((int)(N / 0.75) + 1)`; Guava `Maps.newHashMapWithExpectedSize(N)` does this automatically; after resize all entries are rehashed to new buckets (Java 8: hi/lo bit split avoids full rehash); `LinkedHashMap` same defaults but maintains insertion-order doubly-linked list*

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
   🔑 *Iterator = forward + `remove()`; ListIterator = bidirectional + `add`/`set`; Enumeration = legacy read-only; remove only via `iterator.remove()` or `removeIf` (not a for-each + `list.remove`) to avoid ConcurrentModificationException; Iterator `remove()` removes the element returned by the last `next()` call — only once per `next()`; `list.removeIf(predicate)` = bulk remove, internally uses iterator (safe); for index-based removal loop backwards: `for (int i = list.size()-1; i >= 0; i--) if (cond) list.remove(i)`; CopyOnWriteArrayList allows removal during iteration (snapshot); for maps: `map.entrySet().removeIf(e -> ...)` or `Iterator<Map.Entry>` + `remove()`; Java 8+ `Collection.removeIf` is the preferred idiomatic approach*

**99. ArrayList vs LinkedList vs Vector — when would you use each?**
   🔑 *ArrayList = default, random access, cache-friendly; LinkedList = frequent head/middle insert-delete via iterator, also a Deque; Vector = legacy synchronized (avoid, use `Collections.synchronizedList`/`CopyOnWriteArrayList`)*

**100. Explain Queue, Deque, Stack, PriorityQueue and BlockingQueue.**
   🔑 *Queue FIFO; Deque double-ended (`ArrayDeque` preferred over legacy `Stack` for LIFO); PriorityQueue = binary heap (min by default, O(log n) offer/poll); BlockingQueue (ArrayBlockingQueue/LinkedBlockingQueue) for producer-consumer + backpressure; Queue API: `offer` (add, returns false if full), `poll` (remove head, returns null if empty), `peek` (view head, null if empty) — prefer these over `add`/`remove`/`element` which throw; Deque = double-ended queue: `addFirst`/`addLast`/`peekFirst`/`peekLast` — use as stack (`push`/`pop`) or queue; ArrayDeque is faster than LinkedList for stack/queue (no node allocation); PriorityQueue natural ordering or custom `Comparator`; `PriorityBlockingQueue` for concurrent priority; BlockingQueue methods: `put` (blocks if full), `take` (blocks if empty), `offer(e, timeout)`, `poll(timeout)` — ideal for producer-consumer; `LinkedTransferQueue` for hand-off; `DelayQueue` for scheduled tasks; `SynchronousQueue` = no capacity, each put waits for a take (used in `Executors.newCachedThreadPool`)*

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
   🔑 *ArrayList: get O(1), add-end amortized O(1), add/remove-middle O(n), contains O(n); LinkedList: get O(n), add/remove-ends O(1); HashMap: get/put O(1) avg, O(log n) treeified worst; TreeMap/TreeSet: O(log n) sorted; ArrayDeque: O(1) ends; PriorityQueue: O(log n) offer/poll, O(1) peek; know these to justify a choice; HashSet: add/remove/contains O(1) avg; LinkedHashMap: O(1) get/put + O(1) insertion-order iteration (extra linked-list overhead vs HashMap); ConcurrentHashMap: O(1) read (no lock needed), O(1) write with per-bin locking; CopyOnWriteArrayList: O(n) write (copy whole array), O(1) read; TreeMap: O(log n) all key operations; sorting: `Collections.sort`/`List.sort` = Timsort O(n log n) time O(n) space; space: ArrayList O(n) backing array; LinkedList O(n) nodes but higher constant (2 pointers + object header per node); HashMap O(n) with potential O(n) wasted buckets; always mention amortized vs worst-case in interviews*

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
   🔑 *BlockingQueue (ArrayBlockingQueue/LinkedBlockingQueue) = simplest, handles signalling + backpressure; wait/notify guarded by a `while` (spurious wakeups); Semaphore for permit counting; bounded queue = backpressure; BlockingQueue: producer calls `put(item)` (blocks if full), consumer calls `take()` (blocks if empty) — the simplest correct implementation; wait/notify: must hold the monitor lock, always use `while (!condition) wait()` not `if` (spurious wakeups), call `notifyAll()` to avoid missed signals; Semaphore: `new Semaphore(N)` limits N concurrent producers/consumers, `acquire()`/`release()`; for high-throughput: Disruptor (LMAX) = lock-free ring buffer; reactive approach: `Flux` + `onBackpressureBuffer`; difference: BlockingQueue is the recommended idiomatic approach; wait/notify is low-level and error-prone; Semaphore is for rate-limiting, not producer-consumer; executor queue is itself a BlockingQueue; choose ArrayBlockingQueue (bounded, fair option) vs LinkedBlockingQueue (optionally bounded, separate head/tail locks)*

**113. What is a deadlock, and how do you detect/diagnose one (including a blocked thread) in production?**
   🔑 *Circular wait on 2+ locks, 4 Coffman conditions; thread dump (jstack/`kill -3`/`jcmd Thread.print`) → "Found one Java-level deadlock" + BLOCKED + "waiting to lock" + owner; `ThreadMXBean.findDeadlockedThreads` via actuator, JFR, APM; DB deadlocks (`SHOW ENGINE INNODB STATUS`); 4 Coffman conditions: mutual exclusion, hold-and-wait, no preemption, circular wait — remove ANY ONE to prevent deadlock; thread dump anatomy: thread state = BLOCKED, "waiting to lock <0x...>" while another thread shows "locked <0x...>"; `jstack <pid>` or `kill -3 <pid>` (on Linux) prints to stdout; `jcmd <pid> Thread.print` — preferred in modern JDKs; Spring Boot Actuator: `GET /actuator/threaddump` returns all thread states as JSON; `ThreadMXBean.findDeadlockedThreads()` = programmatic detection (returns null if no deadlock); JFR monitors lock contention events; DB deadlock: MySQL `SHOW ENGINE INNODB STATUS` → "LATEST DETECTED DEADLOCK" section; PostgreSQL `pg_locks` + `pg_stat_activity` join; typical Java deadlock = two threads locking two objects in opposite order*

**114. How do you avoid or fix a deadlock? Give a real production example.**
   🔑 *Global lock ordering (e.g. order by account id), `tryLock` with timeout + backoff, reduce lock scope, single/coarse lock or lock-free structures, avoid nested locks & calling foreign code while holding one; banking example = two transfers A→B / B→A, batch job + online update on same rows, retry on deadlock error code*

**115. How do you size a thread pool for CPU-bound vs I/O-bound work, and how do you configure ExecutorService in production?**
   🔑 *CPU-bound ≈ cores+1; I/O-bound = cores × (1 + wait/service) or Little's law; bounded queue + named ThreadFactory + rejection policy (CallerRuns = backpressure) + timeouts + metrics (queue depth, active); separate pools per dependency (bulkhead); avoid `newCachedThreadPool` (unbounded → OutOfMemoryError)*

**116. ThreadLocal — uses, and the memory-leak risk in a pooled-thread environment.**
   🔑 *Per-thread state (user context, SimpleDateFormat, transaction); pooled thread never dies → ThreadLocalMap entry (weak key, strong value) keeps value + its classloader alive; always `remove()` in a `finally`; InheritableThreadLocal for child threads*

**117. Explain CountDownLatch vs CyclicBarrier vs Phaser vs Semaphore vs Exchanger.**
   🔑 *CountDownLatch = one-shot wait for N events (fan-in/startup); CyclicBarrier = reusable rendezvous of N threads (batch rounds); Phaser = dynamic parties + phases; Semaphore = permit/rate limiting; Exchanger = swap data between two threads; CountDownLatch: `new CountDownLatch(N)`, each worker calls `countDown()`, main thread calls `await()` — cannot be reset (one-shot); use for: wait for N services to start, fan-in N parallel tasks; CyclicBarrier: `new CyclicBarrier(N, barrierAction)`, each thread calls `await()`, when N reach the barrier they all proceed + optional barrierAction runs; reusable for repeating rounds (e.g. parallel simulation steps); Phaser: supports dynamic registration (`register`/`arriveAndDeregister`), multiple phases, tiered; Semaphore: `new Semaphore(permits)`, `acquire()`/`release()`, fair/unfair; use for rate-limiting, connection pool (permits = pool size), bulkhead; Exchanger: two threads swap objects at a synchronization point; use for pipeline stages exchanging buffers (producer fills a buffer while consumer processes the other)*

**118. What is false sharing and how do you deal with it?**
   🔑 *Independent variables on the same 64-byte cache line → cross-core invalidation; fix with `@Contended`/padding, `LongAdder` (striped cells); seen in high-throughput counters*

**119. A service throws intermittent TimeoutException only at peak traffic — how do you tell thread starvation from GC pause, connection-pool exhaustion, or downstream latency?**
   🔑 *GC logs (pause/old-gen), thread dump (BLOCKED/WAITING counts), HikariCP metrics (pending threads, active vs max), downstream p99 + APM traces; correlate timing before blaming code; GC pause: check GC logs for STW events correlating with timeout timestamps (`-Xlog:gc*:file=gc.log:time`); thread starvation: thread dump shows many threads BLOCKED on the same lock or WAITING for a connection; HikariCP exhaustion: `hikaricp.connections.pending > 0` metric, "HikariPool-1 - Connection is not available, request timed out" in logs; downstream latency: distributed trace shows the slow span is in an outbound call; CPU throttling (K8s limit): high system CPU + throttled_time in cgroup metrics; diagnosis order: (1) GC logs, (2) thread dump at peak, (3) pool metrics, (4) downstream traces; fix matrix: GC → tune collector/heap; thread starvation → increase pool or fix the lock; pool exhaustion → OSIV/long tx/add more connections; downstream → circuit breaker + timeout*

**120. Explain multithreading basics — synchronized, volatile, ExecutorService.**
   🔑 *synchronized (mutual exclusion + happens-before), volatile (visibility, not atomicity), ExecutorService (pool + Future + shutdown), atomic classes, concurrent collections*

**121. A counter inside a synchronized method still produces wrong totals with multiple threads — what are the possible causes?**
   🔑 *Lock on different objects/instances, static vs instance lock, non-atomic read-modify-write outside the lock, field not volatile for other readers, multiple JVMs → use AtomicLong/LongAdder or a DB sequence; root causes: (1) synchronizing on `this` but creating multiple instances — each locks on itself, no mutual exclusion; (2) method is `synchronized` but reads happen in unsynchronized code — visibility problem; (3) counter is `static` but lock is instance-level; (4) multiple JVM nodes — each has its own counter, no shared state; (5) `long` field on 32-bit JVM — non-atomic 64-bit write without `volatile`; fixes: `synchronized(MyClass.class)` for static/shared, `volatile` for visibility-only scenarios, `AtomicLong` for lock-free CAS, `LongAdder` for high-contention counters (striped cells, slower `sum()` but faster `increment()`), DB `SELECT ... FOR UPDATE` or atomic `UPDATE SET count=count+1` for distributed counters*

**122. How does a Java thread pool work — what happens with no tasks and with too many tasks?**
   🔑 *Core threads → queue → extra threads up to max → rejection handler; idle non-core threads die after keepAlive; corePoolSize/maxPoolSize/keepAlive/BlockingQueue/ThreadFactory/RejectedExecutionHandler; unbounded queue = memory growth, bounded = rejection/backpressure*

**123. Before Java 8, how could you do parallel programming?**
   🔑 *Raw Thread/Runnable, ExecutorService + Future/invokeAll (Java 5), Fork/Join + RecursiveTask work-stealing (Java 7), CountDownLatch, manual partitioning; `new Thread(runnable).start()` — no reuse, no result; `ExecutorService` (Java 5) — `submit()` returns `Future`, `invokeAll()` fans out tasks; `Callable<T>` for tasks that return a result or throw; `ForkJoinPool` (Java 7) — work-stealing, `RecursiveTask<T>` (returns result) / `RecursiveAction` (void), `fork()`/`join()`/`compute()` pattern; `Semaphore`/`CountDownLatch`/`CyclicBarrier` for coordination; `volatile` + `AtomicInteger` for shared state; Java 7 had `invokedynamic` but no lambdas yet — anonymous inner classes everywhere; Java 8 added parallel streams, lambdas, CompletableFuture as a step-change*

**124. Why is Future blocking?**
   🔑 *Only exposes blocking `get()` / `isDone` polling — no callback or composition; CompletableFuture added completion callbacks and chaining; `Future.get()` blocks the calling thread until the result is available — wastes a thread waiting; `Future.get(timeout, unit)` avoids indefinite block; `isDone()`/`isCancelled()` for polling (busy-wait anti-pattern); no way to attach a callback for when the result is ready; no chaining of multiple futures; cancel with `cancel(mayInterruptIfRunning)` — only works if the task checks `Thread.isInterrupted()`; limitation: can't combine two Futures, can't handle exceptions in the callback, can't compose; CompletableFuture (Java 8) adds: `thenApply`/`thenCompose`/`exceptionally`/`handle`/`allOf`/`anyOf` — proper async composition without blocking; `ListenableFuture` (Guava) was the pre-Java8 workaround*

**125. Explain CompletableFuture and how it works internally.**
   🔑 *`supplyAsync`/`runAsync`, `thenApply`/`thenCompose`/`thenCombine`, `allOf`/`anyOf`, `exceptionally`/`handle`, `orTimeout`, custom executor, `join` vs `get`; internally a completion stack of dependent actions fired non-blocking on completion (default `ForkJoinPool.commonPool`), exceptions wrapped as CompletionException; creation: `CompletableFuture.supplyAsync(() -> expensiveOp(), executor)` — runs on executor; `thenApply(fn)` = sync transform (runs on the completing thread); `thenApplyAsync(fn)` = runs on executor; `thenCompose(f -> ...)` = flatMap for dependent async calls; `thenCombine(cf2, (r1,r2) -> ...)` = zip two; `allOf(cf1,cf2,...)` → waits for all (returns `Void` — collect with `thenApply`); `anyOf` → first to complete; `exceptionally(ex -> fallback)` handles exception; `handle((r,ex) -> ...)` handles both; `join()` vs `get()`: both block — `join` throws unchecked `CompletionException`, `get` throws checked `InterruptedException`/`ExecutionException`; `orTimeout(5, SECONDS)` + `completeOnTimeout(default, 5, SECONDS)` for timeouts; pitfall: default `ForkJoinPool.commonPool` is shared + bounded → provide a custom executor for production code*

**126. Explain notify() vs notifyAll() and the wait/notify protocol.**
   🔑 *`notify()` wakes one arbitrary waiter, `notifyAll()` wakes all; must hold the monitor (else IllegalMonitorStateException); `wait` in a `while` loop (spurious wakeups); prefer `notifyAll` or `Condition.await`/`signal` to avoid lost signals*

**127. When does volatile fail?**
   🔑 *Compound actions (`count++`), check-then-act, invariants across multiple fields, element writes to arrays/objects (only the reference is volatile); use atomics/locks instead; `count++` is read-modify-write (3 steps: read, increment, write) — volatile provides visibility but not atomicity → use `AtomicInteger.incrementAndGet()` or `synchronized`; check-then-act: `if (counter < MAX) counter++` — not atomic even with volatile; invariants spanning 2+ fields: `volatile int x, y` — another thread can see x updated but y not yet → use `synchronized` or an immutable holder object; `volatile int[] arr`: the array reference is volatile but element reads/writes are not → use `AtomicIntegerArray`; volatile IS sufficient for: a single boolean flag (stop loop), a reference that is completely replaced (not mutated), lazy init double-checked locking pattern (`volatile` + `synchronized`)*

**128. Why can't you use volatile on local variables, and what about static fields?**
   🔑 *Locals live on the thread's own stack → never shared, so visibility is moot; `static volatile` IS legal; what's illegal is `volatile` + `final` together*

**129. How do you make a class and a collection thread-safe?**
   🔑 *Immutability, thread confinement, synchronized/Locks, atomic classes, concurrent collections, `Collections.synchronizedXxx`, copy-on-write, defensive copies, no `this` escape*

**130. Normal thread creation vs the Executor framework.**
   🔑 *`new Thread` = manual lifecycle, no reuse/queueing, no result; ExecutorService = pooling, Future/CompletableFuture, scheduling, graceful shutdown, rejection policy*

**131. What are lock striping and object pooling, and how do they help?**
   🔑 *Lock striping = split one lock into N per-bucket locks (ConcurrentHashMap, LongAdder cells) → less contention; object pooling reuses expensive objects (connections, threads, buffers) → less allocation/GC; lock striping example: instead of one lock for the whole map, have 16 locks (one per segment/bucket) → concurrent writes to different segments don't block each other; ConcurrentHashMap Java 8 uses per-bin `synchronized` (finer than segment locks); LongAdder = array of Cells, each with its own lock — thread hashes to a Cell, only conflicts on the same Cell; object pooling: creating objects is expensive (DB connections, thread stacks, ByteBuffers) → pool pre-creates + reuses; HikariCP = DB connection pool; `Executors.newFixedThreadPool` = thread pool; Netty's `PooledByteBufAllocator` = ByteBuffer pool; pool too small → starvation; pool too large → overhead; measure with pool metrics (active/idle/pending); object pool anti-pattern: pooling cheap objects (simple POJOs) adds overhead without benefit*

**132. Synchronous vs asynchronous — trade-offs.**
   🔑 *Sync = caller blocks, simple, immediate consistency; async = non-blocking, callbacks/queues, higher throughput, harder error handling + ordering, eventual consistency*

**133. How does distributed locking work?**
   🔑 *Redis `SET NX PX` + unique token + Lua release (Redlock caveats), ZooKeeper ephemeral sequential node, DB row/advisory lock; TTL to avoid stuck locks, fencing token, idempotency as backup; Redis single-node: `SET lock:<key> <UUID> NX PX 5000` — `NX` = only set if not exists, `PX 5000` = expire in 5s; release via Lua: `if redis.call("get",KEYS[1])==ARGV[1] then return redis.call("del",KEYS[1]) end` — atomic check+delete prevents releasing another holder's lock; Redlock (multi-node Redis) = acquire on N/2+1 nodes within deadline → controversial (Martin Kleppmann critique: clock skew + GC pause); Redisson `RLock` = production-ready with watchdog lease renewal; ZooKeeper ephemeral nodes: create a sequential ephemeral node, watch the node with the next-lowest sequence, delete when done — clean failover; DB advisory lock: `SELECT pg_try_advisory_lock(key)` (PG) — tied to DB session; fencing token = monotonically increasing number from the lock service, included in the protected operation to detect stale lock holders*

**134. Virtual thread limitations — how do you control them, and what happens if you create too many (no pool by design)?**
   🔑 *Not for CPU-bound work; pinning on `synchronized`/native frames (use ReentrantLock); unbounded creation → memory + downstream overload; not pooled by design (cheap, no reuse); control with Semaphore, structured concurrency, bounded queues, rate limiting; virtual thread = JVM-managed, mounted on carrier thread (platform thread in ForkJoinPool); pinning = virtual thread can't unmount when inside a `synchronized` block or native frame → carrier thread blocked too (same as a platform thread blocking); fix: replace `synchronized` with `ReentrantLock`; Java 24 mostly fixes `synchronized` pinning; million virtual threads = ~few GB RAM (each has a small initial stack that grows on demand); downstream DB pool = 10 connections → 1M virtual threads compete → pool exhaustion; control with `Semaphore(poolSize)` wrapping the DB call; `Thread.ofVirtual().start(runnable)` / `Executors.newVirtualThreadPerTaskExecutor()`; don't pool virtual threads (creates unnecessary overhead and defeats the purpose)*

**135. What happens if you create a separate virtual thread per request?**
   🔑 *Fine — thread-per-request becomes cheap (few KB heap stack); the real bottleneck shifts downstream (DB pool, external API) → must rate-limit / bulkhead*

**136. Can virtual threads overload a connection pool?**
   🔑 *Yes — millions of virtual threads can all demand connections while the pool stays small → queueing/timeouts; use a Semaphore/bulkhead sized to the pool; the problem: virtual threads make synchronous blocking code scale well for I/O — but if 10k virtual threads each try to get a DB connection from a 10-connection pool, 9,990 threads queue (potentially timing out); HikariCP `connectionTimeout=30s` will throw after 30s for waiting threads; fix: `Semaphore sem = new Semaphore(poolSize); sem.acquire(); try { doDBWork(); } finally { sem.release(); }` — caps concurrent DB access; or use Resilience4j Bulkhead; JDBC is blocking: virtual threads unmount during I/O wait, but acquiring a connection from the pool is a blocking operation that does NOT unmount if pool is empty; monitor `hikaricp.connections.pending` and `hikaricp.connections.timeout` metrics*

**137. What is Project Loom?**
   🔑 *JEP 444 virtual threads + continuations + carrier threads + structured concurrency + scoped values; goal = simple blocking code with async scalability; Project Loom = JVM enhancement to make thread-per-request architecture scale like reactive without reactive's complexity; key JEPs: 444 (Virtual Threads, GA in Java 21), 453 (Structured Concurrency, preview), 446 (Scoped Values, preview); continuation = a suspendable computation unit — virtual thread lifecycle maps to continuations mounting/unmounting carrier threads; carrier threads = platform threads in a `ForkJoinPool` (default = number of CPU cores); structured concurrency (`StructuredTaskScope`) = subtasks owned by a scope — all complete or are cancelled together, prevents thread leaks; scoped values = immutable per-virtual-thread context (replaces `ThreadLocal` for Loom code); enables writing `service.call()` in plain blocking style with millions of concurrent virtual threads*

**138. Explain the CPU → OS thread → platform thread → virtual thread relationship.**
   🔑 *CPU core runs a kernel/OS thread; platform thread = 1:1 wrapper over an OS thread (~1 MB stack); virtual thread = JVM-scheduled, mounted on a carrier thread (ForkJoinPool) and unmounted when it blocks*

**139. What is a thread dump vs a heap dump, and how do you capture them in production?**
   🔑 *Thread dump = thread states/stacks (jstack, `kill -3`, `jcmd <pid> Thread.print`) for deadlock/blocking; heap dump = objects/memory (`jmap`, `jcmd GC.heap_dump`, `-XX:+HeapDumpOnOutOfMemoryError`, MAT); k8s = ephemeral container + `kubectl cp`; plus actuator/Micrometer, Prometheus+Grafana, JFR; thread dump: `jstack <pid>` → prints all threads with state + stack trace; `jcmd <pid> Thread.print -l` includes locked monitors; Spring Boot Actuator `GET /actuator/threaddump` (JSON); take 3 dumps 5-10s apart to identify stuck vs transient threads; look for BLOCKED (waiting for monitor), WAITING (park/wait), TIMED_WAITING (sleep/await); heap dump: `jmap -dump:format=b,file=heap.hprof <pid>` (live objects only: add `live`); `jcmd <pid> GC.heap_dump /path/heap.hprof`; `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/dumps/` for automatic capture on OOM; analyze with Eclipse MAT (Leak Suspects, Dominator Tree, Retained Heap); K8s: use an ephemeral debug container or `kubectl exec` + `kubectl cp heap.hprof` to local; avoid heap dumps in prod during peak (they pause the JVM); JFR as low-overhead alternative*

**140. How do the concurrent data structures work — ConcurrentHashMap, CopyOnWriteArrayList, ConcurrentSkipListMap?**
   🔑 *CHM = CAS + per-bin synchronized + treeify, weakly-consistent iterators; CopyOnWriteArrayList/Set = snapshot on write, great read-heavy; ConcurrentSkipListMap = sorted lock-free; BlockingQueues for producer-consumer*

**141. How does CAS work, how does volatile apply to objects, and how do Atomic classes work?**
   🔑 *CAS = compare-and-swap on a memory address (Unsafe/VarHandle, lock-free, ABA risk → AtomicStampedReference); volatile gives visibility of the reference, not the object's fields; AtomicInteger/AtomicReference use CAS retry loops; LongAdder uses striping under contention*

**142. Can two threads execute on the same object simultaneously with no locking? If yes, how?**
   🔑 *Yes — different methods, or non-synchronized methods, run concurrently on the same object; only threads contending for the same monitor are serialized; immutable/stateless objects are always safe; example: `obj.methodA()` (synchronized) and `obj.methodB()` (not synchronized) can run simultaneously — `methodB` doesn't need the lock; two synchronized methods on the SAME object cannot run simultaneously (share the object's monitor); two threads on DIFFERENT instances never block each other (each instance has its own monitor); stateless service beans (Spring `@Service` singletons) are safe because they have no instance state; immutable objects (String, Integer, LocalDate) are inherently thread-safe; `synchronized` is reentrant — a thread can call a second synchronized method on the same object without deadlocking itself*

**143. ReentrantLock vs synchronized — and when do you use ReadWriteLock / StampedLock?**
   🔑 *ReentrantLock = `tryLock`(timeout), interruptible, fairness, multiple `Condition`s, must `unlock()` in finally; ReadWriteLock = many readers / one writer; StampedLock = optimistic read (no lock, validate) for read-heavy; synchronized = simpler, JVM-optimized, works with virtual threads better (no pinning in recent JDKs)*

**144. Explain the Fork/Join framework and work-stealing.**
   🔑 *`ForkJoinPool` + `RecursiveTask`/`RecursiveAction`, divide-and-conquer `fork()`/`join()`, work-stealing deques balance load across workers; backs `parallelStream`; avoid blocking I/O inside it; work-stealing: each worker thread has a deque of tasks; when a worker runs out, it steals from the tail of another worker's deque → better load distribution than a shared queue; `RecursiveTask<V>` = fork for sub-problems, `join()` to collect; canonical pattern: `if (size <= threshold) computeDirectly(); else { left.fork(); right.fork(); return left.join() + right.join(); }`; `ForkJoinPool.commonPool()` = shared, backing parallelStream; provide a custom pool for isolation: `new ForkJoinPool(N).submit(() -> list.parallelStream()...)`; pitfall: blocking I/O inside ForkJoin blocks a carrier thread → stalls the pool; `ManagedBlocker` to compensate by adding more threads; compare with ExecutorService: ForkJoin is for recursive decomposable tasks, ExecutorService for independent tasks*

**145. What is structured concurrency and what are scoped values (Java 21)?**
   🔑 *`StructuredTaskScope` ties subtasks to a scope → all finish/cancel together, clear error propagation (ShutdownOnFailure/ShutdownOnSuccess), no leaked threads; ScopedValue = immutable, inheritable alternative to ThreadLocal for virtual threads; structured concurrency: `try (var scope = new StructuredTaskScope.ShutdownOnFailure()) { Future<A> a = scope.fork(taskA); Future<B> b = scope.fork(taskB); scope.join().throwIfFailed(); use(a.resultNow(), b.resultNow()); }` — if either task fails, the other is cancelled; ShutdownOnSuccess = return first success, cancel the rest; prevents "orphaned" threads (a common async bug); scoped values: `ScopedValue<User> USER = ScopedValue.newInstance(); ScopedValue.where(USER, currentUser).run(() -> service.call())` — immutable per virtual-thread context available to all called code; unlike ThreadLocal: immutable (no set), inheritable without copying, no leak risk (cleared at scope exit); replaces ThreadLocal for request-context propagation (user, tenant, trace) in virtual-thread code; both are preview/incubator features becoming stable in Java 21+*

**146. Explain ThreadPoolExecutor saturation, queue choice and rejection policies.**
   🔑 *core → queue → up to max → rejection; queue type matters: unbounded LinkedBlockingQueue (max never reached, OOM risk), bounded ArrayBlockingQueue (backpressure), SynchronousQueue (hand-off, grows threads); rejection policies = AbortPolicy (throws, default), CallerRunsPolicy (backpressure — caller executes), DiscardPolicy, DiscardOldestPolicy; size + queue + policy together define behaviour under load; separate pools per dependency (bulkhead)*

**147. What are the key CompletableFuture orchestration patterns?**
   🔑 *`supplyAsync` on a custom executor; `thenApply` (sync transform) vs `thenCompose` (flatten dependent future) vs `thenCombine` (join two); `allOf`/`anyOf` for fan-out; `orTimeout`/`completeOnTimeout`; `exceptionally`/`handle`/`whenComplete` for recovery; avoid blocking `get()` inside a stage; don't starve the common pool — supply your own executor*

**148. How do you propagate context (MDC, SecurityContext, trace id) across async tasks and virtual threads?**
   🔑 *ThreadLocal doesn't cross thread boundaries → use a TaskDecorator (Spring) to copy MDC/SecurityContext/trace into the worker thread, `DelegatingSecurityContextExecutor`, Micrometer context propagation; for virtual threads + structured concurrency prefer ScopedValue (immutable, inheritable) over ThreadLocal; always clear in a finally to avoid leakage in pooled threads; MDC (Mapped Diagnostic Context) = SLF4J's per-thread key-value store for structured log fields (traceId, userId); problem: `@Async` runs on a different thread → MDC is empty; fix with Spring `TaskDecorator`: `executor.setTaskDecorator(runnable -> { Map<String,String> ctx = MDC.getCopyOfContextMap(); return () -> { MDC.setContextMap(ctx); try { runnable.run(); } finally { MDC.clear(); } }; })`; SecurityContext: use `DelegatingSecurityContextExecutorService` or `SecurityContextHolder.setStrategyName(INHERITABLETHREADLOCAL)` (careful with pooled threads); Micrometer `ContextSnapshot` propagates Reactor/Brave/OTel context; CompletableFuture chains: capture context before submission, restore in each stage; virtual threads: prefer `ScopedValue.where(...).run(...)` — inheritable, immutable, no leak*

**149. How do you design backpressure in a producer-consumer pipeline?**
   🔑 *Bounded BlockingQueue so a slow consumer blocks fast producers; or Reactive Streams `request(n)` demand signalling (Flux + buffer/onBackpressureBuffer/Drop/Latest); drop vs block vs buffer trade-off; Kafka lag as natural backpressure; size the queue with Little's law; monitor queue depth + shed load (reject/429) before OOM; Little's law: L = λ × W (queue length = arrival rate × wait time) — use to size the queue and predict latency; BlockingQueue: `put()` blocks the producer when full — the simplest form of backpressure, but careful not to block the entire thread pool; drop strategies: `onBackpressureDrop` (discard new), `onBackpressureLatest` (keep only latest), `onBackpressureBuffer(N)` (buffer up to N, then error); Kafka: consumer-side lag = producer is faster; increase consumer instances (up to partition count), use `max.poll.records` to pace; rate limiting at entry: return 429 with `Retry-After` header when queue is full; circuit breaker opens when downstream can't keep up; async handoff with SQS/Kafka provides durable buffering vs in-memory BlockingQueue (data loss on crash)*

**150. What are the best practices and pitfalls with virtual threads?**
   🔑 *Use for I/O-bound thread-per-request, not CPU-bound; avoid `synchronized` around blocking calls (pinning — use ReentrantLock; largely fixed in recent JDKs); don't pool them (create per task); don't use them as a drop-in for CPU parallelism (use a fixed pool/ForkJoin); downstream (DB pool, external API) becomes the bottleneck → bound with a Semaphore; ScopedValue over ThreadLocal; best practices: `Executors.newVirtualThreadPerTaskExecutor()` for thread-per-request servers; Spring Boot 3.2+ auto-configures virtual threads with `spring.threads.virtual.enabled=true`; pinning detection: `-Djdk.tracePinnedThreads=full` logs pinned virtual threads; ThreadLocal still works but is slow (each virtual thread gets its own copy) — prefer ScopedValue; don't use virtual threads for CPU-bound tasks (they don't unpin during CPU work → carrier thread monopolized); `Thread.isVirtual()` to check; monitoring: JFR event `jdk.VirtualThreadPinned`; native memory tracking shows virtual thread stack usage; Tomcat with virtual threads: each request gets a virtual thread, `maxThreads` no longer the bottleneck — downstream becomes it*

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
   🔑 *`groupingBy` (+ downstream like counting/mapping/summing), `partitioningBy`, `toMap` (with merge function for dup keys), `joining`, `collectingAndThen`, `teeing` (Java 12), `reducing`; `toUnmodifiableList`; custom `Collector` (supplier/accumulator/combiner/finisher); `groupingBy(keyFn, downstream)` examples: `groupingBy(Person::getDept, counting())`, `groupingBy(Person::getDept, mapping(Person::getName, toList()))`, `groupingBy(Person::getDept, summarizingInt(Person::getAge))`; `partitioningBy(pred)` = two groups (true/false) as `Map<Boolean, List>`; `toMap(keyFn, valueFn, mergeFunction)` — merge function required when keys can collide (else `IllegalStateException`); `joining(", ", "[", "]")` for CSV; `collectingAndThen(toList(), Collections::unmodifiableList)` wraps result; `teeing(c1, c2, merger)` = collect into two collectors simultaneously (e.g. count + sum); `Collectors.toUnmodifiableMap/List/Set` (Java 10); `flatMapping(fn, downstream)` (Java 9); `filtering(pred, downstream)` (Java 9); `counting()` shorthand for `reducing(0L, e->1L, Long::sum)`*

**164. reduce vs collect — when to use which?**
   🔑 *`reduce` = immutable fold, needs identity + associative accumulator (+ combiner for parallel); `collect` = mutable reduction into a container (supplier/accumulator/combiner); use `collect` for collections/StringBuilder, `reduce` for a single value; `reduce(identity, accumulator)` = folds stream into one value without mutable state (e.g. `stream.reduce(0, Integer::sum)`); `reduce(BinaryOperator)` = no identity, returns `Optional`; `reduce(identity, accumulator, combiner)` = for parallel streams where identity+accumulator+combiner must be associative+compatible; `collect(Collector)` accumulates into a mutable container (List, Map, StringBuilder) — thread-safe in parallel via the combiner; anti-pattern: `reduce` to build a List (creates a new List per element → O(n²)); use `collect(toList())` instead; `reduce` is pure functional but `collect` is more efficient for containers; `Stream.toList()` (Java 16) = unmodifiable list, shorthand for `collect(toUnmodifiableList())`*

**165. What are the common Stream pitfalls?**
   🔑 *Reusing a consumed stream (IllegalStateException), stateful/side-effecting lambdas, `Collectors.toMap` duplicate-key crash, `null` elements, unbounded `iterate`/`generate` without limit, parallel on non-associative ops, forgetting streams are lazy (no terminal = nothing runs); detailed pitfalls: (1) reuse: stream is single-use — consuming it marks it as used, second terminal op throws `IllegalStateException`; (2) side effects in `map`/`filter`: lambdas must be stateless + non-interfering → shared mutable state causes race conditions in parallel; (3) `Collectors.toMap` with duplicate keys: throws `IllegalStateException` unless a merge function is provided; (4) `null` in stream: `NullPointerException` in many operations (use `Stream.ofNullable` or filter nulls); (5) infinite streams: always bound with `limit()` or a short-circuit terminal; (6) `peek` for debugging: may not run if the stream is short-circuited; (7) parallel stream on ordered operations: `findFirst`/`sorted` on a parallel stream can be slower than sequential; (8) exception in lambda: checked exceptions require wrapping; (9) `sorted()` on large streams: materializes all elements into memory*

**166. Explain switch expressions, text blocks, var and record patterns.**
   🔑 *Switch expressions (`->`, `yield`, exhaustive, no fall-through); text blocks `"""..."""`; `var` local type inference (not for fields/params); record patterns + pattern matching for switch destructure + guard `when` clauses (Java 21)*

**167. Describe the standard functional interfaces and how you compose them.**
   🔑 *Function/BiFunction, Supplier, Consumer/BiConsumer, Predicate/BiPredicate, UnaryOperator/BinaryOperator; composition `andThen`/`compose`, Predicate `and`/`or`/`negate`; primitive variants (IntFunction, ToIntFunction) avoid boxing*

**168. Explain primitive streams and why they matter.**
   🔑 *`IntStream`/`LongStream`/`DoubleStream` avoid boxing overhead; `mapToInt`/`boxed`/`asLongStream`; `sum`/`average`/`summaryStatistics`/`range`/`rangeClosed`; use for numeric aggregation on large data; boxing overhead: `Stream<Integer>` stores Integer objects (16 bytes each) + GC pressure; `IntStream` stores raw `int` primitives (4 bytes) — up to 4× memory saving; conversion: `stream.mapToInt(Person::getAge)` → `IntStream`; `intStream.boxed()` → `Stream<Integer>`; `IntStream.range(0, 10)` = [0,10), `rangeClosed(1, 10)` = [1,10]; numeric terminals: `sum()`, `min()`, `max()`, `average()` (returns `OptionalDouble`), `summaryStatistics()` (count+sum+min+max+average in one pass); `IntStream.of(1,2,3)`, `IntStream.iterate(1, n->n*2).limit(10)`; `Arrays.stream(int[])` = IntStream directly; missing from primitive streams: no `flatMap` to Object stream without boxing; `mapToObj(i -> ...)` converts back to `Stream<T>`*

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
   🔑 *`submit()` captures the exception in the Future (surfaces on `get()` as ExecutionException); `execute()` propagates to the thread's UncaughtExceptionHandler; `throw` = statement, `throws` = declaration; always `get()` the Future or set a handler; `submit(Callable)` wraps any exception (checked or unchecked) in `ExecutionException` — retrieve with `future.get()` which then throws `ExecutionException`; `future.get()` also throws `InterruptedException` (if the waiting thread is interrupted) and `CancellationException`; `execute(Runnable)` is fire-and-forget — uncaught exceptions go to `Thread.getDefaultUncaughtExceptionHandler()` or `ThreadFactory`-set handler; set a global handler: `Thread.setDefaultUncaughtExceptionHandler((t,ex) -> log.error("Uncaught", ex))`; `CompletableFuture.runAsync` also swallows: check with `whenComplete((r,ex) -> if (ex != null) handle)`; `throw` = throws a Throwable instance (statement in code); `throws` = declares that a method may propagate a checked exception (part of the method signature)*

**180. How does `@ControllerAdvice` work for global exception handling?**
   🔑 *`@RestControllerAdvice` + `@ExceptionHandler` per type, ordered most-specific-first, returns ResponseEntity + ProblemDetail (RFC 7807) / `@ResponseStatus`; handles controller-layer exceptions only (NOT filters/security — those need an AuthenticationEntryPoint/AccessDeniedHandler or error page); `@ControllerAdvice` = applies to all controllers; `@RestControllerAdvice` = `@ControllerAdvice` + `@ResponseBody` (returns JSON); `@ExceptionHandler(MyException.class)` method maps exception type to HTTP response; exception resolution order: most specific exception type wins (ChildException before ParentException); multiple `@ExceptionHandler` on same advice class: ordered by specificity; `ResponseEntityExceptionHandler` base class handles Spring MVC built-in exceptions (MethodArgumentNotValidException, HttpMessageNotReadableException, etc.); ProblemDetail (Spring 6): `ProblemDetail.forStatusAndDetail(HttpStatus.NOT_FOUND, "User not found")` + `setProperty("code", "USR-001")`; security exceptions bypass MVC: `AuthenticationException` → `AuthenticationEntryPoint`, `AccessDeniedException` → `AccessDeniedHandler`*

**181. How do you design a good custom exception?**
   🔑 *Extend RuntimeException (unchecked) for most, meaningful name + error code + message, preserve the cause (`super(msg, cause)`), a small hierarchy (BusinessException/ValidationException/NotFoundException), immutable, don't log-and-rethrow (log once at the boundary)*

**182. What is exception chaining/wrapping and why does it matter?**
   🔑 *Wrap a low-level exception in a higher-level one with `initCause`/constructor cause → preserves the root-cause stack trace while abstracting the layer; `getCause()`; avoid losing the original by swallowing; `@Repository` translates persistence exceptions to DataAccessException; example: `catch (SQLException e) { throw new DataAccessException("Failed to load user " + id, e); }` — the `e` is preserved as the cause; `Throwable.getCause()` returns the wrapped exception; `Throwable.getCause().getCause()` traverses the chain; full chain visible in stack trace output ("Caused by:"); never: `throw new MyException("msg")` without passing `e` — original stack trace lost; `Throwable.initCause()` = set cause after construction (deprecated pattern); prefer the constructor form `new MyException(msg, cause)`; Spring's `@Repository` + PersistenceExceptionTranslationPostProcessor translates JPA/Hibernate exceptions to Spring's DataAccessException hierarchy (HibernateJdbcException, JpaObjectRetrievalFailureException, etc.) — allows switching ORM without changing service code*

**183. What are the best practices and anti-patterns in exception handling?**
   🔑 *Don't swallow (empty catch), don't catch `Throwable`/`Exception` broadly, don't catch Error (OOM/StackOverflow), fail fast, throw early + catch late, add context, no control flow via exceptions, log once, use `finally`/try-with-resources for cleanup*

**184. How do you handle checked exceptions inside streams and lambdas?**
   🔑 *Lambdas/functional interfaces can't propagate checked exceptions → wrap in a RuntimeException inside the lambda, or a helper that rethrows unchecked (sneaky throw), or use a wrapper functional interface; prefer unchecked domain exceptions in stream-heavy code; standard `Function<T,R>` doesn't declare `throws` → checked exceptions require wrapping; approach 1: wrap inline: `.map(s -> { try { return parse(s); } catch (ParseException e) { throw new RuntimeException(e); } })`; approach 2: utility wrapper: `@FunctionalInterface interface ThrowingFunction<T,R> { R apply(T t) throws Exception; } static <T,R> Function<T,R> wrap(ThrowingFunction<T,R> fn) { return t -> { try { return fn.apply(t); } catch (Exception e) { throw new RuntimeException(e); } }; }` — then `.map(wrap(MyClass::parse))`; approach 3: sneaky throw (Lombok `@SneakyThrows` / Unsafe trick) — rethrows checked as unchecked without wrapping; approach 4: use `Either<Error, T>` or `Result<T, E>` monad-style (vavr library); prefer making domain exceptions `RuntimeException` subclasses in the first place*

**185. How do exceptions propagate in CompletableFuture / async code?**
   🔑 *Exception completes the future; surfaces as CompletionException (async) or ExecutionException (`get`); handle with `exceptionally`/`handle`/`whenComplete`; set an AsyncUncaughtExceptionHandler for void `@Async`; uncaught in a plain thread → UncaughtExceptionHandler; CompletableFuture: if any stage throws, it completes exceptionally; downstream `thenApply`/`thenCompose` stages are skipped; `exceptionally(ex -> fallback)` = catch and return a fallback value, recovers the chain; `handle((result, ex) -> ...)` = runs always (result OR ex is non-null), can recover or rethrow; `whenComplete((result, ex) -> ...)` = side effect, doesn't recover; `join()` throws unchecked `CompletionException(cause)` — get cause with `getCause()`; `get()` throws checked `ExecutionException(cause)`; `@Async` void methods: exceptions are silently swallowed unless `AsyncUncaughtExceptionHandler` is configured; `@Async` returning `CompletableFuture<T>`: exception is propagated to the future; Reactor/WebFlux: `onErrorResume`/`onErrorMap`/`onErrorReturn` operators; structured concurrency: `StructuredTaskScope.ShutdownOnFailure` propagates any subtask exception to the scope's `join().throwIfFailed()`*

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
   🔑 *`@EnableAutoConfiguration` → AutoConfigurationImportSelector reads `META-INF/spring/...AutoConfiguration.imports` (`spring.factories` pre-2.7) → `@Conditional` eval (OnClass/OnMissingBean/OnProperty/OnWebApplication) → ordering via `@AutoConfigureBefore/After`, user beans win (`@ConditionalOnMissingBean`); `--debug` prints the conditions report; exclude via property/annotation; auto-config mechanism: `@SpringBootApplication` includes `@EnableAutoConfiguration` → scans all JARs on the classpath for `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`; each listed class is a candidate auto-configuration; `@Conditional` annotations gate whether it activates: `@ConditionalOnClass(DataSource.class)` → only if DataSource on classpath; `@ConditionalOnMissingBean(DataSource.class)` → only if user hasn't defined their own; `@ConditionalOnProperty(prefix="spring.datasource", name="url")` → only if property set; user-defined beans ALWAYS win over auto-config; debug: `--debug` flag or `spring.autoconfigure.report.enabled=true` → prints "Positive matches" + "Negative matches" + "Exclusions"; exclude: `@SpringBootApplication(exclude=DataSourceAutoConfiguration.class)` or `spring.autoconfigure.exclude` property*

**192. Explain bean scopes, and a real bug from injecting a prototype into a singleton.**
   🔑 *singleton (default, per container — controllers/services/repositories are singletons), prototype (new per lookup), request/session/application/websocket (web); prototype injected into a singleton is created once → use `ObjectProvider`/`@Lookup`/scoped proxy; a singleton holding request state = thread-safety bug*

**193. Explain the difference between @Component, @Service, @Repository and @Controller — including what @Repository actually adds.**
   🔑 *All are `@Component` stereotypes (same scanning/bean creation); `@Repository` adds persistence exception translation to DataAccessException; `@Controller`/`@RestController` enable handler mapping; `@Service` = semantics + AOP pointcut target; swapping them still works but loses translation/clarity/mapping; `@Component` = generic Spring-managed component, foundation of all stereotypes; `@Service` = business logic layer, no extra Spring behavior but documents intent + can be target of AOP (e.g. `@Transactional`); `@Repository` = DAO/data layer + PersistenceExceptionTranslationPostProcessor translates native exceptions (HibernateException, PersistenceException) → Spring DataAccessException hierarchy — important for keeping persistence-agnostic service layer; `@Controller` = handler for HTTP requests — DispatcherServlet maps to `@RequestMapping` methods; `@RestController` = `@Controller` + `@ResponseBody`; all meta-annotated with `@Component` → picked up by component scan; semantics matter for readability, AOP pointcuts, and exception translation*

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
   🔑 *Non-blocking Netty + Reactor (Mono/Flux), backpressure, few threads for high concurrency, needs R2DBC/WebClient to stay non-blocking; avoid when the stack is blocking (JDBC), the team is unfamiliar, debugging/stack traces matter, or load is modest — MVC + virtual threads is often simpler; WebFlux = Spring's reactive web framework built on Project Reactor + Netty event loop (8-16 threads handle thousands of connections); `Mono<T>` = 0-1 element, `Flux<T>` = 0-N elements; reactive wins: SSE, WebSockets, streaming responses, high-concurrency microservices calling many downstream services; reactive loses: simple CRUD with JDBC (blocking DB driver poisons the event loop → need R2DBC or offload to `Schedulers.boundedElastic()`), complex debugging (reactive stack traces are unreadable), learning curve for team, many frameworks don't support reactive; Java 21 virtual threads + Spring MVC = thread-per-request with virtual threads, near-reactive scalability, familiar imperative code — often the better choice for new projects*

**200. How do you manage configuration across environments — profiles, property precedence and @ConfigurationProperties?**
   🔑 *`application-{profile}.yml` + `spring.profiles.active`; precedence (low→high): defaults < jar yml < profile yml < external config < OS env < system props < CLI args < `@TestPropertySource`; `@ConfigurationProperties` typed + relaxed binding + `@Validated`, constructor binding for immutability; `@Profile` beans*

**201. How do you activate a Spring profile, and what is a profile?**
   🔑 *Profile = named grouping of beans/config per environment; activate via `spring.profiles.active`, `-D`/`SPRING_PROFILES_ACTIVE` env var, CLI arg, `@ActiveProfiles` in tests, `spring.profiles.group` for composites*

**202. @Value vs @ConfigurationProperties — when to use which?**
   🔑 *`@Value("${...}")` = single property / SpEL, no relaxed binding or validation; `@ConfigurationProperties` = type-safe bulk binding of a prefix to a POJO, relaxed binding, nested/lists, JSR-303 validation — preferred for grouped config*

**203. What does Spring Boot Actuator provide, how does it work, and how do you add/secure/relocate endpoints?**
   🔑 *Production endpoints over HTTP/JMX (`/health` with readiness/liveness groups, `/metrics` via Micrometer → Prometheus, `/info`, `/env`, `/loggers`, `/threaddump`, `/heapdump`); custom `HealthIndicator`/`@Endpoint`; `management.endpoints.web.exposure.include`, separate `management.server.port`, secure behind auth*

**204. How have you used AOP for cross-cutting concerns, and what are its proxy limitations?**
   🔑 *`@Aspect` + pointcut + `@Around`/`@Before`/`@AfterThrowing` for logging, auditing, metrics, retry, security, caching; join point/pointcut/advice/weaving; proxy-based → no self-invocation, no final/private/static methods, only Spring beans; AspectJ load/compile-time weaving for the rest; AOP concepts: JoinPoint = point in execution (method call), Pointcut = expression selecting JoinPoints (e.g. `execution(* com.example.service.*.*(..))`), Advice = code to run (Before/After/Around/AfterReturning/AfterThrowing), Aspect = class combining pointcuts + advice, Weaving = applying aspects; `@Around` example: `Object around(ProceedingJoinPoint pjp) throws Throwable { log.info("Before"); try { return pjp.proceed(); } finally { log.info("After"); } }`; limitations: proxy-based AOP intercepts only EXTERNAL method calls through the Spring proxy — `this.method()` bypasses the proxy; private/static/final methods can't be proxied; non-Spring objects (new MyService()) bypass Spring AOP entirely; solution: AspectJ compile-time weaving (transforms bytecode at build time) or `AopContext.currentProxy()` for self-invocation (anti-pattern); common uses: `@Transactional`, `@Cacheable`, `@Async`, method-level security*

**205. Spring vs Spring Boot.**
   🔑 *Spring = modular framework, manual config (XML/JavaConfig), external server; Boot = opinionated auto-config, starters + dependency BOM, embedded server, actuator, externalized config, fat jar, production-ready defaults; Spring Framework: IoC container, AOP, MVC, Data, Security, Batch — each requires manual configuration (bean definitions, datasource setup, MVC config); Spring Boot = opinionated defaults + auto-configuration removes boilerplate; starters = curated transitive dependencies (e.g. `spring-boot-starter-web` pulls Tomcat, Jackson, Spring MVC, validation); embedded server: Tomcat/Jetty/Undertow bundled in the fat JAR — deploy with `java -jar`; Actuator: built-in /health, /metrics, /info endpoints; `spring-boot-dependencies` BOM = version alignment for hundreds of dependencies; `@SpringBootApplication` = 3 annotations in one; `application.yml` = centralized config with profile support; Spring Boot 3 requires Java 17+, Jakarta namespace, AOT support; Spring Framework 6 added Observation API, HTTP interface clients; Boot sits ON TOP of Spring — you can use Spring without Boot (e.g. legacy enterprise apps on WebSphere)*

**206. How does the embedded web server work, and how do you switch it or use an external one?**
   🔑 *Embedded Tomcat by default (`spring-boot-starter-web`); swap to Jetty/Undertow by excluding the Tomcat starter + adding another; Netty for WebFlux; configure `server.port`/threads; or build a WAR + `SpringBootServletInitializer` + `provided` scope for external Tomcat/WebLogic; embedded server mechanism: Spring Boot creates an embedded `TomcatServletWebServerFactory` bean → starts Tomcat programmatically inside the JVM → registers the Spring `DispatcherServlet`; swap to Jetty: exclude `spring-boot-starter-tomcat` + add `spring-boot-starter-jetty`; swap to Undertow: add `spring-boot-starter-undertow`; WebFlux uses Netty (non-blocking) by default or Undertow; configure: `server.port=8080`, `server.tomcat.threads.max=200`, `server.tomcat.accept-count=100`, `server.ssl.*`, `server.compression.*`; WAR for external server: `<packaging>war</packaging>`, `extends SpringBootServletInitializer` + override `configure()`, mark Tomcat as `<scope>provided</scope>`; JAR is simpler + preferred for containers/microservices; `TomcatServletWebServerCustomizer` for advanced Tomcat tuning (connector, connector attributes, access log)*

**207. Explain IoC vs DI and how the container stores/resolves beans internally.**
   🔑 *IoC = principle (container owns creation/lifecycle); DI = mechanism (constructor/setter/field); `ApplicationContext`/DefaultListableBeanFactory holds a ConcurrentHashMap of singletons + BeanDefinitionRegistry; `@Autowired` resolves by type → qualifier → name; 3-level cache resolves circular references*

**208. BeanFactory vs ApplicationContext.**
   🔑 *BeanFactory = basic lazy DI container; ApplicationContext = BeanFactory + eager singletons + i18n + event publishing + AOP + resource loading + Environment — what Boot uses*

**209. How do you remove a cyclic dependency between beans?**
   🔑 *Redesign responsibilities (extract a shared bean), use application events / async messaging, interface + callback, `@Lazy` or setter injection as a stopgap; Boot 2.6+ fails on circular refs by default (`spring.main.allow-circular-references`)*

**210. What is @PostConstruct (and its alternatives)?**
   🔑 *Runs after DI, before the bean is in use; one per class, no args, void; alternatives `InitializingBean.afterPropertiesSet` / `@Bean(initMethod)`; use for warm-up/validation, not heavy blocking work; `@PreDestroy` for cleanup*

**211. What happens in the background when a Spring Boot application starts (SpringApplication.run)?**
   🔑 *Deduce web type → load ApplicationContextInitializers/Listeners → prepare Environment + profiles → banner → create context → load bean definitions (component scan + auto-config) → refresh (BeanFactoryPostProcessors, BeanPostProcessors, instantiate singletons, create AOP proxies) → start embedded server → publish ApplicationReadyEvent → run ApplicationRunner/CommandLineRunner; detailed sequence: (1) SpringApplication created, SpringApplicationListeners registered (from spring.factories); (2) `prepareEnvironment()` — PropertySources loaded, profiles activated; (3) Banner printed; (4) `createApplicationContext()` — AnnotationConfigServletWebServerApplicationContext created; (5) `prepareContext()` — ApplicationContextInitializers called, BeanDefinitions registered; (6) `refreshContext()` — `AbstractApplicationContext.refresh()`: invokeBeanFactoryPostProcessors (ConfigurationClassPostProcessor = component scan + auto-config resolution), registerBeanPostProcessors, initMessageSource, initApplicationEventMulticaster, `onRefresh()` = start embedded server, finishBeanFactoryInitialization = instantiate all non-lazy singletons + AOP proxies created by AbstractAutoProxyCreator; (7) ApplicationReadyEvent published; (8) ApplicationRunner/CommandLineRunner beans invoked; total time tracked by `ApplicationStartup`; `@PostConstruct` runs during step 6 per bean*

**212. Explain the full request flow through a Spring Boot web application.**
   🔑 *Request → Servlet filters (incl. security filter chain) → DispatcherServlet → HandlerMapping → HandlerInterceptor.preHandle → HandlerAdapter → controller → service → repository → return → HttpMessageConverter → interceptor.postHandle → response; exceptions → HandlerExceptionResolver/`@ControllerAdvice`; detailed flow: (1) HTTP arrives at Tomcat/Netty; (2) Servlet Filter chain (OncePerRequestFilter subclasses, SecurityFilterChain with CorsFilter/CsrfFilter/JwtFilter/AuthorizationFilter); (3) DelegatingFilterProxy → FilterChainProxy for Spring Security; (4) DispatcherServlet (`doDispatch`) selected by URL mapping; (5) `HandlerMapping.getHandler()` — finds the handler (controller method) and its interceptors via `RequestMappingHandlerMapping` (parses @RequestMapping); (6) `HandlerInterceptor.preHandle()` — can short-circuit by returning false; (7) `HandlerAdapter` (RequestMappingHandlerAdapter) processes argument resolving (`@RequestBody` → Jackson, `@PathVariable`, `@RequestParam`, `@Valid` bean validation); (8) Controller method executes → calls service → calls repository; (9) Return value handler + `HttpMessageConverter` (Jackson for JSON) serializes the response body; (10) `HandlerInterceptor.postHandle()` then `afterCompletion()`; exception path: any exception → `HandlerExceptionResolver` → `@ExceptionHandler` in `@ControllerAdvice`*

**213. Filter vs Interceptor vs ControllerAdvice — how do they differ and in what order?**
   🔑 *Servlet Filter = outermost, works on raw Servlet request/response (CORS, auth, logging), before DispatcherServlet; HandlerInterceptor = Spring MVC, around the handler (pre/post/afterCompletion), has handler context; `@ControllerAdvice` = exception handling + model/binder advice at controller layer*

**214. CommandLineRunner vs ApplicationRunner.**
   🔑 *Both run once after the context is ready (ApplicationReadyEvent area); CommandLineRunner gets raw `String[] args`, ApplicationRunner gets parsed `ApplicationArguments`; order with `@Order`; use for startup tasks/seed data (keep them fast); CommandLineRunner: `@Component class MyRunner implements CommandLineRunner { public void run(String... args) { ... } }` — args = raw command-line strings; ApplicationRunner: `run(ApplicationArguments args)` — `args.getOptionValues("name")` parses `--name=value` syntax; both run AFTER the full application context is ready (all beans initialized, embedded server started); run in `@Order` sequence (lower = earlier); if one throws, the app fails to start; alternatives: `@PostConstruct` (runs during bean initialization, before server starts), `ApplicationListener<ApplicationReadyEvent>`, `InitializingBean.afterPropertiesSet()`; use for: database seeding, cache warming, health check preflight, registering with service discovery; keep them fast — if they block, the app appears hung during startup*

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
   🔑 *Chunk-oriented: Job → Step → ItemReader/ItemProcessor/ItemWriter, chunk size + commit interval, JobRepository for restart/skip/retry, partitioned/multithreaded steps, scheduling; 2M rows → streaming reader (SAX/`FlatFileItemReader`) → validate/map → `MongoItemWriter`, chunk 1k–5k, skip policy + restartability, build indexes after load, watch memory; Spring Batch architecture: `Job` = named batch process; `Step` = one phase (tasklet or chunk-oriented); `JobRepository` = stores execution state in a DB (allows restart from last checkpoint on failure); `ItemReader` reads one item at a time; `ItemProcessor` transforms/validates; `ItemWriter` writes a chunk atomically; chunk commit interval = how many items per transaction (tune for throughput vs memory); restart: failed job can rerun from the last committed chunk; skip policy: tolerate N bad records before failing; retry on transient errors (DB timeout); partitioned step = split data range across multiple threads/nodes for parallel processing; 2M row Excel: use Apache POI streaming SAX reader (`XSSFReader`) or `FlatFileItemReader` for CSV; chunk 1k-5k → `MongoItemWriter` bulk insert; build compound indexes after load (faster than during); `JobLauncher` from `@Scheduled` or REST endpoint; `JobExecution` tracks status*

**223. How do you optimize Spring application startup time?**
   🔑 *Lazy bean init (`spring.main.lazy-initialization`), fewer classpath scans / spring-context-indexer, AOT + GraalVM native image, slimmer starters, defer non-critical ApplicationRunners, tiered compilation; trade-off = first-request latency vs startup; `spring.main.lazy-initialization=true` = beans created on first use, not at startup — faster startup, but first request is slower (each bean initialized on demand); `spring-context-indexer` (apt-generated `spring.components`) = avoids classpath scanning, uses a pre-built index; AOT (Spring Boot 3): `SpringApplication.run` has an ahead-of-time mode that pre-computes bean definitions/proxy bytecode → eliminates runtime reflection/scanning; GraalVM native image: compile to native binary — near-instant startup (10-100ms vs 2-10s for JVM), low RSS — at cost of build time + restrictions (no dynamic proxies unless configured); CRaC (Coordinated Restore at Checkpoint): snapshot a warm JVM and restore it — sub-100ms restart; remove unused auto-configurations via `spring.autoconfigure.exclude`; profile startup with `ApplicationStartup`: `SpringApplication.setApplicationStartup(BufferingApplicationStartup(2048))`; `GET /actuator/startup` shows the slowest init steps*

**224. Explain the Maven POM and dependency management (starters, BOM, scopes, transitive deps).**
   🔑 *POM = coordinates (groupId:artifactId:version) + dependencies + build; `spring-boot-starter-parent` or the `spring-boot-dependencies` BOM (`<dependencyManagement>` import) aligns versions; starters = curated dependency bundles; scopes = compile (default), provided (servlet API), runtime (JDBC driver), test (JUnit), import (BOM); transitive deps resolved by nearest-wins → use `<exclusions>` + `mvn dependency:tree` to fix conflicts; plugins (compiler, surefire/failsafe, spring-boot) drive the build*

**225. How do you configure a custom async executor for @Async?**
   🔑 *`@EnableAsync` + a `ThreadPoolTaskExecutor` bean (corePoolSize, maxPoolSize, queueCapacity, keepAlive, thread-name prefix, rejection policy); name it and reference via `@Async("executorName")`; add a TaskDecorator to propagate MDC/SecurityContext/trace; set an AsyncUncaughtExceptionHandler for void methods; separate executors per workload (bulkhead); never use the default SimpleAsyncTaskExecutor in prod (unbounded)*

**226. How do you configure WebClient/RestClient (timeouts, pooling, retries, error handling)?**
   🔑 *Base URL + default headers; connect/response timeouts (HttpClient `responseTimeout`, connection pool via Reactor Netty `ConnectionProvider`); `ExchangeFilterFunction` for auth/logging/trace; `retryWhen` with backoff + jitter (idempotent only); `onStatus` → map 4xx/5xx to domain exceptions; integrate Resilience4j (circuit breaker/time limiter); `RestClient` (Spring 6.1) = synchronous fluent alternative; avoid RestTemplate (maintenance mode); WebClient setup: `WebClient.builder().baseUrl("https://api.example.com").clientConnector(new ReactorClientHttpConnector(HttpClient.create().option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 3000).responseTimeout(Duration.ofSeconds(10)).connectionProvider(ConnectionProvider.builder("myPool").maxConnections(50).pendingAcquireMaxCount(200).maxIdleTime(Duration.ofSeconds(30)).build()))).defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE).filter(ExchangeFilterFunctions.basicAuthentication(user, pass)).build()`; error handling: `.onStatus(HttpStatusCode::is4xxClientError, resp -> resp.bodyToMono(String.class).flatMap(body -> Mono.error(new ClientException(body))))`, `.onStatus(HttpStatusCode::is5xxServerError, resp -> Mono.error(new ServerException()))`; retry: `.retryWhen(Retry.backoff(3, Duration.ofMillis(100)).jitter(0.5).filter(ex -> ex instanceof ServerException))`; RestClient (blocking): `RestClient.builder().baseUrl(...).build().get().uri("/users/{id}", id).retrieve().onStatus(...).body(UserDto.class)`*

**227. How does Spring resolve ambiguous beans and handle overriding?**
   🔑 *`@Primary` (default winner), `@Qualifier`/bean name (explicit), `@Conditional`/`@ConditionalOnMissingBean` (auto-config backs off), `@Profile`, inject `List`/`Map<String,Bean>` for all; bean-definition overriding disabled by default (`spring.main.allow-bean-definition-overriding`); `@Order`/`@Priority` for collection ordering; `@DependsOn` for init order*

**228. How do you map between entities and DTOs (MapStruct vs ModelMapper vs manual)?**
   🔑 *MapStruct = compile-time generated mappers (fast, type-safe, no reflection, `@Mapper`, explicit field maps) — preferred; ModelMapper = runtime reflection (convenient, slower, surprises); manual/constructor/record mapping for simple cases; keep mapping out of the domain, map entity↔DTO at the boundary to avoid leaking entities / LazyInitializationException; MapStruct: `@Mapper(componentModel="spring") interface UserMapper { UserDto toDto(User user); User toEntity(UserDto dto); @Mapping(source="address.city", target="city") UserDto toDtoWithCity(User user); }`; MapStruct generates an implementation at compile time (no reflection → fast, detects mapping mistakes at compile time); `@BeanMapping(nullValuePropertyMappingStrategy=IGNORE)` for partial updates; `@InheritInverseConfiguration` for reverse mapping; `@AfterMapping` for custom post-processing; ModelMapper: `modelMapper.map(user, UserDto.class)` — uses reflection, can map incorrectly without warning (e.g. maps same-name fields even if logically different); manual/record: `new UserDto(user.getId(), user.getName())` — explicit, fast, compile-safe, but verbose; LazyInitializationException: if you access a lazy field AFTER closing the transaction, in a MapStruct mapper — ensure the mapping runs inside `@Transactional` or use DTO projection queries*

**229. How do @Transactional, @Async and events interact (and what breaks)?**
   🔑 *`@Async` runs on a new thread → new/no transaction + no propagated security/transaction context by default (combine carefully — don't assume the caller's tx); publish a domain event and use `@TransactionalEventListener(phase=AFTER_COMMIT)` to run side effects only after commit; self-invocation bypasses both proxies; an `@Async` method that needs a tx must itself be `@Transactional` on a separate bean; detailed interactions: (1) `@Async` creates a new thread → `@Transactional` on the caller's thread is NOT visible; if the `@Async` method must participate in a transaction, annotate it with `@Transactional` — it gets its own independent transaction; (2) `@TransactionalEventListener(phase=TransactionPhase.AFTER_COMMIT)` = event listener fires only after the publishing transaction commits — prevents sending emails/calling services before the DB write is durable; `BEFORE_COMMIT`, `AFTER_COMMIT`, `AFTER_ROLLBACK`, `AFTER_COMPLETION` phases; (3) self-invocation: `this.asyncMethod()` and `this.transactionalMethod()` both bypass their proxies — move to a separate bean or inject `self`; (4) `@Async` + void = exceptions are silently swallowed unless `AsyncUncaughtExceptionHandler` configured; (5) SecurityContext: not propagated to `@Async` threads by default — use `DelegatingSecurityContextExecutor`*

**230. How do you deploy to / run on an external application server instead of the embedded one?**
   🔑 *Package as WAR: `extends SpringBootServletInitializer` + override `configure(SpringApplicationBuilder)`, set packaging `war`, mark the embedded Tomcat starter `provided`; deploy to external Tomcat/JBoss/WebLogic; or keep the embedded server but swap Tomcat→Jetty/Undertow via starter exclusion; prefer the embedded fat-jar + container model for microservices; WAR deployment steps: (1) `pom.xml`: `<packaging>war</packaging>`; (2) exclude embedded Tomcat: `<dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-tomcat</artifactId><scope>provided</scope></dependency>`; (3) main class: `public class App extends SpringBootServletInitializer { @Override protected SpringApplicationBuilder configure(SpringApplicationBuilder b) { return b.sources(App.class); } public static void main(String[] args) { SpringApplication.run(App.class, args); } }`; (4) `mvn package` → target/app.war; (5) drop into `CATALINA_HOME/webapps/`; context path = WAR filename; external server manages lifecycle (JNDI, shared classloaders); enterprise use case: multiple apps on one WebLogic/WildFly, centralized JNDI datasources, corporate governance*

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
   🔑 *Spring wraps the bean in a JDK dynamic (interface) / CGLIB (class) proxy; TransactionInterceptor begins/commits/rolls back around the call, binding the connection to a ThreadLocal via TransactionSynchronizationManager; self-invocation (`this.method()`) bypasses the proxy → no transaction; private/static/final methods aren't intercepted; fix = self-inject, AopContext, move to another bean, or AspectJ weaving; proxy selection: JDK dynamic proxy if the bean implements an interface (proxy is an instance of the interface); CGLIB subclass proxy if no interface (or `proxyTargetClass=true`); both intercept method calls on the proxy object; TransactionInterceptor: calls `PlatformTransactionManager.getTransaction()` → binds connection to `TransactionSynchronizationManager` (ThreadLocal) → calls target method → commits (or rolls back on exception) → releases connection; `@Transactional` only on Spring-managed beans (in the Spring context); self-invocation fix options: (1) inject the bean into itself `@Autowired MyService self;` then `self.method()`; (2) `AopContext.currentProxy()` (must enable `exposeProxy=true`); (3) move the transactional method to a separate bean; (4) AspectJ compile-time weaving (applies aspects directly to bytecode, no proxy needed)*

**237. Explain propagation types (REQUIRED, REQUIRES_NEW, NESTED) and isolation, with a parent/child rollback scenario.**
   🔑 *REQUIRED joins or creates (default — parent + both children roll back together); REQUIRES_NEW suspends the outer tx (independent audit/log that survives the parent rollback); NESTED = savepoints (partial rollback, JDBC only); also MANDATORY/SUPPORTS/NEVER/NOT_SUPPORTED; isolation READ_COMMITTED default; propagation details: REQUIRED (default) = join existing or create new — all code participates in one transaction, any exception rolls back everything; REQUIRES_NEW = suspend the current transaction, start a brand new one — use for audit logging (save the audit record even if the business op fails); NESTED = uses JDBC savepoints — if the nested transaction fails, roll back to the savepoint and retry (parent can catch + continue), if parent fails everything rolls back; MANDATORY = must have an existing transaction or throw; SUPPORTS = use existing if present, otherwise run without; NOT_SUPPORTED = suspend existing, run without transaction; NEVER = throw if called inside a transaction; isolation levels: READ_UNCOMMITTED (dirty reads), READ_COMMITTED (PG default — no dirty reads), REPEATABLE_READ (MySQL default — no non-repeatable reads), SERIALIZABLE (full isolation); `@Transactional(propagation=REQUIRES_NEW)` for audit; scenario: `placeOrder()` (REQUIRED) calls `chargePayment()` (REQUIRED) and `sendEmail()` (REQUIRES_NEW) — if email sending fails, the email tx rolls back but the order + payment are committed*

**238. What are the transaction rollback rules for checked vs unchecked exceptions?**
   🔑 *By default only RuntimeException + Error roll back; a checked exception COMMITS → use `@Transactional(rollbackFor = Exception.class)` to force rollback, `noRollbackFor` to deliberately commit; catching without rethrow = no rollback; `setRollbackOnly` marks it unrecoverable*

**239. Why is @Transactional not working — what are the possible reasons?**
   🔑 *Self-invocation (proxy bypass), non-public/private/final method, class not a Spring bean, exception caught without rethrow, checked exception (no rollbackFor), wrong PlatformTransactionManager, non-transactional engine (MyISAM), called from a different thread, `@Transactional` on the wrong layer*

**240. How do you create and roll back a transaction programmatically?**
   🔑 *`TransactionTemplate.execute` or `PlatformTransactionManager` (`getTransaction`/`commit`/`rollback`); rollback via the DB's undo log/WAL; `setRollbackOnly()`; savepoints for partial rollback (NESTED); use when you need fine-grained control or non-public boundaries; TransactionTemplate: `transactionTemplate.execute(status -> { try { repo.save(entity); return result; } catch (Exception e) { status.setRollbackOnly(); throw e; } })`; PlatformTransactionManager directly: `TransactionStatus status = txManager.getTransaction(new DefaultTransactionDefinition()); try { doWork(); txManager.commit(status); } catch (Exception e) { txManager.rollback(status); throw e; }`; `status.setRollbackOnly()` = marks the transaction for rollback without throwing (useful in multi-step flows); `TransactionSynchronizationManager.isActualTransactionActive()` = check if inside a transaction; use programmatic over `@Transactional` when: non-public/private method, lambda scope, complex conditional rollback, fine-grained savepoint control; `TransactionTemplate` is thread-safe and reusable (configure once, inject); `DefaultTransactionDefinition` = set propagation/isolation/timeout/readOnly*

**241. How do you manage transactions in production (keeping them short)?**
   🔑 *Short, explicit boundaries at the service layer, `readOnly=true` for queries, timeout + lock timeout, no remote calls/file IO inside, chunk large writes (batch size + flush/clear), retry on optimistic/deadlock errors, idempotency keys, outbox for events, monitor long-running tx + lock waits; best practices: (1) `@Transactional(readOnly=true)` for SELECT-only methods — DB can optimize (no dirty checking, no locking in some DBs, replica routing); (2) set timeout: `@Transactional(timeout=5)` seconds — fails if the tx runs longer; (3) never make external HTTP/Kafka/file calls inside a transaction (holds DB connections/locks while waiting for I/O); (4) large batch: `session.flush(); session.clear()` every N records to bound the persistence context size; (5) pessimistic lock timeout: `javax.persistence.lock.timeout=5000` (ms) prevents indefinite blocking; (6) retry: `@Retryable(OptimisticLockingFailureException.class, backoff=@Backoff(100))` for `@Version` conflicts; (7) monitor: `pg_stat_activity` / `SHOW PROCESSLIST` for long-running queries; HikariCP pending connections metric; (8) OSIV off (`spring.jpa.open-in-view=false`) prevents long transactions spanning the web layer; (9) keep transactions at the service layer, NOT the controller or repository*

**242. Difference between save(), saveAndFlush(), persist() and merge().**
   🔑 *`persist` (JPA) = transient→managed, no id guarantee, throws if detached; `save` (Hibernate) = returns id, merges detached; `merge` = copies detached state into a managed copy and returns it; `saveAndFlush` = save + immediate flush (same transaction)*

**243. What is Hibernate's dirty checking mechanism?**
   🔑 *On flush, Hibernate compares the entity's current state with the snapshot taken at load → generates UPDATE for changed columns only (all columns unless `@DynamicUpdate`); no explicit `save()` needed inside a transaction*

**244. Explain Hibernate internals — entity states, persistence context, flush and commit.**
   🔑 *EntityManagerFactory → EntityManager → persistence context (identity map + dirty checking); entity states transient → persistent/managed → detached → removed; flush pushes SQL (uncommitted) — auto before queries + at commit; commit makes durable; `clear`/`detach` evicts; FlushMode.AUTO; ActionQueue; dialect → JDBC batch; entity state lifecycle: TRANSIENT = `new Entity()` — not associated with any persistence context, no id; MANAGED/PERSISTENT = associated with an open EntityManager, Hibernate tracks changes (dirty checking); DETACHED = was managed but EntityManager closed or `detach(entity)` called — id exists but not tracked; REMOVED = scheduled for DELETE on flush; persistence context = first-level cache (identity map by entity type + id), prevents loading the same row twice per session; dirty checking: at flush, Hibernate compares managed entities against their load-time snapshot → generates minimal UPDATE; `EntityManager.flush()` sends pending SQL to DB (still within the transaction); `EntityManager.clear()` evicts all managed entities (use during batch processing); FlushMode.AUTO = flush before queries (to make writes visible) + at commit; ActionQueue = ordered list of DML actions to execute at flush (INSERT → UPDATE → DELETE); JDBC batch: `hibernate.jdbc.batch_size=50` groups statements for fewer round-trips*

**245. JPA vs Hibernate vs Spring Data JPA.**
   🔑 *JPA = specification/API (EntityManager, JPQL, annotations); Hibernate = implementation + extras (Criteria, `@DynamicUpdate`, filters, natural id); Spring Data JPA = repository abstraction, derived query method names (`findByXAndYOrderByZ`), proxy over SimpleJpaRepository*

**246. How do Spring Data JPA repositories work internally?**
   🔑 *At startup Spring Data creates a proxy over SimpleJpaRepository; query lookup = derived from method name (PartTree parsing), `@Query` (JPQL/native), or named query; QueryLookupStrategy; `@Transactional` applied on SimpleJpaRepository; Pageable/Sort/Specification support; mechanism: Spring Data scans interfaces extending `Repository<T,ID>` → creates a JDK dynamic proxy (`RepositoryFactoryBean`) backed by `SimpleJpaRepository<T,ID>`; derived queries: `findByFirstNameAndLastName(String fn, String ln)` → parser creates JPQL from method name (PartTree): find=SELECT, By=WHERE, And=AND; supports: `findBy`, `existsBy`, `countBy`, `deleteBy`, `findTop5By`, `findAllBy`, `OrderBy`, `Containing`, `Between`, `GreaterThan`, etc.; `@Query("SELECT u FROM User u WHERE u.email = :email")` for complex queries; `nativeQuery=true` for SQL; `@Modifying` + `@Transactional` for DML queries; `Pageable page = PageRequest.of(0, 20, Sort.by("name"))` → `Page<User> findAll(Pageable p)` returns page with total count; `Specification<User>` (JpaSpecificationExecutor) for dynamic queries; `@EntityGraph(attributePaths={"orders"})` on repository method for fetch control*

**247. @OneToMany vs @ManyToMany mapping, and owning side / fetch defaults.**
   🔑 *`@OneToMany(mappedBy)` = inverse side, FK in child; `@ManyToMany` = join table (`@JoinTable`) — prefer two `@OneToMany` to an explicit join entity when it has attributes; collections default LAZY, `@ManyToOne`/`@OneToOne` default EAGER → make them LAZY*

**248. Explain cascade types and orphanRemoval.**
   🔑 *CascadeType PERSIST/MERGE/REMOVE/REFRESH/DETACH/ALL propagate operations parent→child; `orphanRemoval=true` deletes a child removed from the collection; cascade REMOVE ≠ orphanRemoval; avoid CascadeType.ALL on `@ManyToMany`; cascade PERSIST = `em.persist(parent)` also persists child entities in the collection (no need to persist each child manually); cascade MERGE = `em.merge(parent)` also merges child state; cascade REMOVE = `em.remove(parent)` also removes children (dangerous — cascades deletes); cascade REFRESH = synchronizes from DB; cascade DETACH = detaches children too; `orphanRemoval=true` vs cascade REMOVE: orphanRemoval deletes a child when it's REMOVED FROM THE COLLECTION (`order.getItems().remove(item)` → item deleted), cascade REMOVE deletes children when the parent is deleted; both together = full lifecycle management of children; `@OneToMany(cascade={PERSIST,MERGE}, orphanRemoval=true)` = most common safe pattern for owned collections; avoid `CascadeType.ALL` on `@ManyToMany` (removes shared entities from the other side's perspective); cascade REMOVE risk on `@ManyToOne` = delete the foreign-key target, breaking other rows*

**249. Explain entity inheritance strategies (SINGLE_TABLE, JOINED, TABLE_PER_CLASS).**
   🔑 *SINGLE_TABLE = one table + discriminator (fast, nullable columns); JOINED = normalized, one table per class + joins (integrity, slower reads); TABLE_PER_CLASS = one full table per concrete class (union queries); `@MappedSuperclass` = shared mapping, not an entity; `@Embeddable`/`@Embedded`/`@ElementCollection` for value types; SINGLE_TABLE: `@Inheritance(strategy=SINGLE_TABLE)` + `@DiscriminatorColumn(name="type", discriminatorType=STRING)` + `@DiscriminatorValue("CAT")` on each subclass; one table with all columns (nullable for subclass-specific fields); fastest query (no join), simplest; downside = many nullable columns, hard to enforce NOT NULL constraints; JOINED: separate table per class, all joined on PK; `@Inheritance(strategy=JOINED)`; normalized (no nulls), FK integrity; downside = JOIN on every query; TABLE_PER_CLASS: each concrete class has its own full table (all inherited columns duplicated); `@Inheritance(strategy=TABLE_PER_CLASS)`; no JOIN for single-type queries; polymorphic queries use UNION ALL (expensive); no shared PK strategy; `@MappedSuperclass`: not an entity, can't be queried/persisted directly — just shared mapping (no inheritance of DB behavior); `@Embeddable`: value type embedded in the owner's table (Address embedded in Person); `@ElementCollection`: collection of basic types or embeddables stored in a separate table*

**250. Explain id generation strategies (IDENTITY, SEQUENCE, TABLE, UUID).**
   🔑 *IDENTITY = DB auto-increment (disables JDBC batch inserts); SEQUENCE = DB sequence + `allocationSize` pooling (preferred, batch-friendly); TABLE = portable but slow; UUID/`@GeneratedValue` for distributed ids; natural vs surrogate key; `@EmbeddedId`/`@IdClass` for composite keys; IDENTITY (`GenerationType.IDENTITY`): DB auto-increment (MySQL `AUTO_INCREMENT`, SQL Server `IDENTITY`) — Hibernate must do an INSERT to get the id, so batch inserts are DISABLED (each insert requires a round-trip); SEQUENCE (`GenerationType.SEQUENCE`): uses a DB sequence object (PostgreSQL, Oracle) — Hibernate fetches multiple ids at once via `allocationSize` (default 50) → enables JDBC batch inserts; `@SequenceGenerator(name="seq", sequenceName="user_seq", allocationSize=50)`; TABLE: a separate table stores the next value — portable but slow (row locking); UUID: `@GeneratedValue(generator="UUID")` + `@UuidGenerator` (Hibernate 6) — generated by the app before INSERT → batch-friendly, distributed-safe; natural key (email) as PK → unstable (updates cascade), longer index; surrogate (BIGINT) → stable, short, but meaningless; composite keys: `@EmbeddedId` (embeddable PK class) or `@IdClass` (separate class, fields duplicated); modern: prefer `SEQUENCE` + `allocationSize` for RDBMS, UUID for distributed/event-sourced*

**251. How do you tune batch inserts/updates in Hibernate?**
   🔑 *`hibernate.jdbc.batch_size`, `order_inserts`/`order_updates=true`, `batch_versioned_data`; avoid IDENTITY (prevents batching) → use SEQUENCE; `flush()`+`clear()` every N rows to bound the persistence context; or bulk JPQL/native / JdbcTemplate batchUpdate for huge loads; `spring.jpa.properties.hibernate.jdbc.batch_size=50` sets the JDBC batch size (how many statements are grouped into one DB round-trip); `hibernate.order_inserts=true` / `hibernate.order_updates=true` sorts statements by entity type so the DB can execute them in batches (without ordering, a mix of entity types breaks batching); `hibernate.batch_versioned_data=true` needed for versioned entities (optimistic lock) to batch correctly; IDENTITY id generation prevents batching because Hibernate needs the DB-generated id immediately after each INSERT — switch to SEQUENCE + allocationSize; flush-clear pattern in batch processing: `for (int i=0; i < entities.size(); i++) { session.persist(entities.get(i)); if (i % 50 == 0) { session.flush(); session.clear(); } }` — flush sends SQL, clear evicts managed entities to prevent OutOfMemoryError; `StatelessSession` = no first-level cache or dirty checking — more efficient for bulk writes; JPQL bulk: `UPDATE Entity e SET e.status='DONE' WHERE e.createdAt < :date` — no persistence-context overhead, issues directly as SQL; `JdbcTemplate.batchUpdate` for plain JDBC batch (bypasses Hibernate entirely, fastest); Spring Batch `ItemWriter` with `JdbcBatchItemWriter` for large ETL loads*

**252. How do you build DTO projections, and why avoid returning entities?**
   🔑 *Interface or class (constructor) projections, JPQL `new DTO(...)`, `@EntityGraph` + mapping, or MapStruct; avoids over-fetching, LazyInitializationException, and exposing entities over the API; read-only + lighter than managed entities; interface projection: `interface UserSummary { String getName(); String getEmail(); }` + `List<UserSummary> findByStatus(String status)` — Spring Data generates a proxy (fetches only projected columns); class (constructor) projection: `@Query("SELECT new com.example.UserDto(u.id, u.name) FROM User u WHERE u.active=true")` — faster than interface projection (no proxy), exact columns; `@Value("#{target.firstName + ' ' + target.lastName}")` SpEL in interface projections for computed fields; `@EntityGraph(attributePaths={"orders"})` on repository method avoids N+1 while fetching needed associations; MapStruct mapper at service boundary: `UserDto dto = userMapper.toDto(entity)` — keep mapping out of the repository; why NOT return entities from APIs: (1) exposes internal DB structure (tight coupling), (2) triggers LazyInitializationException after session closes, (3) serializes more data than needed (over-fetching), (4) makes entity managed→detached→serialized path error-prone, (5) accidental PUT of the entity reference updates the DB; @JsonIgnore or @JsonView as workarounds are leaky; correct approach = DTOs/Records at the controller/service boundary*

**253. How do you build dynamic queries (Criteria API, Specifications, QueryDSL)?**
   🔑 *JPA Criteria API (typesafe, verbose), Spring Data `Specification`/`JpaSpecificationExecutor` (composable predicates), QueryDSL `BooleanBuilder` (fluent), or `@Query` with `:x IS NULL OR col=:x`; whitelist sort columns (injection), use for optional filters + date ranges + pagination; Criteria API: `CriteriaBuilder cb = em.getCriteriaBuilder(); CriteriaQuery<User> q = cb.createQuery(User.class); Root<User> r = q.from(User.class); List<Predicate> preds = new ArrayList<>(); if (name != null) preds.add(cb.like(r.get("name"), "%" + name + "%")); q.where(preds.toArray(new Predicate[0]));` — verbose but type-safe at compile time; Spring Data Specification: `interface UserRepo extends JpaRepository<User,Long>, JpaSpecificationExecutor<User> {}` then `Specification<User> hasEmail = (r,q,cb) -> email == null ? null : cb.equal(r.get("email"), email); repo.findAll(Specification.where(hasEmail).and(isActive), pageable)` — composable, testable predicates; QueryDSL: generates Q-types from entities at compile time; `new JPAQueryFactory(em).selectFrom(QUser.user).where(QUser.user.name.containsIgnoreCase(name).and(QUser.user.active.isTrue())).fetch()` — fluent, IDE-autocomplete; SQL injection: NEVER concatenate user input into JPQL/SQL strings; always parameterize or whitelist sort columns (`if (!ALLOWED_SORTS.contains(sort)) throw`)*

**254. How do you call a stored procedure from JPA/Hibernate?**
   🔑 *`@NamedStoredProcedureQuery` + `@Procedure` on the repository, or `EntityManager.createStoredProcedureQuery` with IN/OUT/INOUT params + `getOutputParameterValue`; `StoredProcedureQuery`; map result sets; use for set-based DB-side logic, but keep business logic in the app layer; `@NamedStoredProcedureQuery(name="User.getByStatus", procedureName="GET_USERS_BY_STATUS", parameters={@StoredProcedureParameter(mode=IN,name="status",type=String.class),@StoredProcedureParameter(mode=OUT,name="count",type=Integer.class)})` on entity class; Spring Data repository: `@Procedure(procedureName="GET_USERS_BY_STATUS") List<User> getByStatus(String status);`; using EntityManager directly: `StoredProcedureQuery q = em.createStoredProcedureQuery("MY_PROC"); q.registerStoredProcedureParameter("in_id", Long.class, ParameterMode.IN); q.registerStoredProcedureParameter("out_name", String.class, ParameterMode.OUT); q.setParameter("in_id", 1L); q.execute(); String name = (String) q.getOutputParameterValue("out_name");`; result sets: `q.getResultList()` for SELECT-returning procs; `@Query(value="{CALL MY_PROC(:id)}", nativeQuery=true)` for simple cases; stored procs best for set-based bulk operations (UPDATE/DELETE on millions of rows) that would be slow row-by-row in app code; avoid business logic in stored procs (hard to test, version-control, and deploy)*

**255. How do you implement auditing (created/modified by/at)?**
   🔑 *Spring Data `@EnableJpaAuditing` + `@CreatedDate`/`@LastModifiedDate`/`@CreatedBy`/`@LastModifiedBy` + `AuditingEntityListener` + `AuditorAware`; Hibernate Envers for full entity revision history/versioned audit tables*

**256. Soft delete vs hard delete.**
   🔑 *Hard = row removed (FK/audit loss, unrecoverable); soft = `is_deleted`/`deleted_at` flag + `@SQLDelete` + `@SQLRestriction`/`@Where`; costs = index bloat, unique-constraint conflicts (partial index), every query must filter, needs an archival/purge job*

**257. Should you store foreign keys directly or model associations — and how do you handle historical accuracy?**
   🔑 *Model as `@ManyToOne` associations (FK) for referential integrity + joins; add denormalized snapshot columns (e.g. name/price at transaction time) for historical accuracy + read speed; avoid unnecessary bidirectional mappings; `@MapsId` for shared PK; FK directly (`Long userId`) vs association (`@ManyToOne User user`): FK-only = simpler, avoids lazy-loading, but lose referential integrity enforcement at JPA level and no JOIN navigation; association = referential integrity via FK constraint, type-safe navigation, but must load the entity or use a proxy; rule of thumb = model association (`@ManyToOne`) for domain aggregates that are always queried together; historical accuracy: if you store `@ManyToOne Product product` in an OrderItem, but the product name/price changes later, the historical order now shows the wrong price — fix = snapshot the price at order time with a `BigDecimal priceAtPurchase` column; audit history options: (1) Hibernate Envers — `@Audited` on entity → revision tables track all changes; (2) temporal tables (SQL Server) / `valid_from`/`valid_to` columns; (3) event-sourced log (immutable events = source of truth); `@MapsId`: `@OneToOne @MapsId @JoinColumn(name="id") User user;` on UserProfile — shares the User's PK (no separate sequence); avoid bidirectional mappings unless you truly need `parent.getChildren()` (adds complexity + StackOverflow risk in toString/equals)*

**258. How do you handle multiple writes where one must persist even if the rest roll back?**
   🔑 *Parent `@Transactional(REQUIRED)` so failures roll back together; put the audit/failure-state write in `REQUIRES_NEW` (its own tx survives the rollback), or use a TransactionSynchronization `afterCompletion` hook, or `noRollbackFor`; for cross-service use a compensating transaction / Saga*

**259. How do you configure multiple datasources in one Spring Boot service?**
   🔑 *Two `DataSource` beans (one `@Primary`), each with its own `EntityManagerFactory` + `PlatformTransactionManager`; `@EnableJpaRepositories(basePackages, entityManagerFactoryRef, transactionManagerRef)` per repo/entity package; separate `@ConfigurationProperties` (`spring.datasource.a/b`); no single transaction spans both unless XA/JTA (avoid — blocking, complex) → per-DB transactions + Saga/outbox for cross-DB consistency; separate HikariCP pools*

**260. How do you implement read/write (primary/replica) splitting?**
   🔑 *`AbstractRoutingDataSource` picks primary vs replica by a routing key (e.g. `@Transactional(readOnly=true)` → replica via a `TransactionSynchronization`/AOP flag); writes → primary; beware replication lag (read-your-writes → route critical reads to primary or use a version token); separate pools; or let the DB proxy (ProxySQL/PgBouncer) route; monitor lag; `AbstractRoutingDataSource`: override `determineCurrentLookupKey()` to return "primary" or "replica" based on a `ThreadLocal` flag set by AOP; AOP `@Around("@annotation(Transactional)")` reads `readOnly` attribute → sets `DataSourceContextHolder.setDataSource(readOnly ? "replica" : "primary")`; `@Transactional(readOnly=true)` hints Hibernate (no dirty checking) AND routes to replica; replication lag: async streaming replication means replica may be N ms behind; for reads-after-writes, route to primary (or check replication lag via `pg_stat_replication`); version token pattern: after a write, remember the replication position; route reads to a replica only when it has caught up to that position; DB proxy approach: ProxySQL/PgBouncer + MaxScale handle routing transparently at the network layer — the app uses one connection string; AWS Aurora Reader Endpoint does this automatically; separate `HikariDataSource` pools per datasource (primary: 20 connections, replica: 50 connections for read-heavy); monitor: `SHOW SLAVE STATUS` / `pg_stat_replication` lag metric*

[⬆️ Back to top](#contents)

## Databases — SQL, NoSQL & Query Optimization

### SQL Fundamentals & DDL / DML / Config

**261. What are DDL, DML, TCL, and DCL — and what are the key statements in each?**
   🔑 *DDL (Data Definition Language) = defines/modifies schema: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`; DML (Data Manipulation Language) = manipulates data: `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE/UPSERT`; TCL (Transaction Control Language) = controls transactions: `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `SET TRANSACTION`; DCL (Data Control Language) = controls access: `GRANT`, `REVOKE`; DDL is auto-committed in most DBs (no rollback); DML within a transaction can be rolled back*

**262. What is the difference between DROP, TRUNCATE, and DELETE?**
   🔑 *DELETE = DML, removes rows matching WHERE clause, fires triggers, writes undo log (rollback-able), slow for large tables; TRUNCATE = DDL, removes ALL rows instantly (no WHERE), does NOT fire row-level triggers, minimal logging, auto-commits (usually not rollback-able), resets identity/sequence; DROP = DDL, removes the entire table structure + data + indexes + constraints permanently; memory trick: DELETE = surgical, TRUNCATE = bulldoze the data, DROP = demolish the building; TRUNCATE is faster for clearing a table (no undo log); DELETE is needed when you need WHERE clause or trigger-based audit; DROP is irreversible (use with caution in prod)*

**263. What is the syntax for CREATE TABLE, and how do you define constraints?**
   🔑 *`CREATE TABLE orders (id BIGSERIAL PRIMARY KEY, user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE, status VARCHAR(20) NOT NULL DEFAULT 'PENDING', amount NUMERIC(12,2) NOT NULL CHECK (amount > 0), created_at TIMESTAMP NOT NULL DEFAULT NOW(), idempotency_key VARCHAR(64) UNIQUE)` — inline constraints; table-level: `CONSTRAINT pk_orders PRIMARY KEY (id)`, `CONSTRAINT fk_user FOREIGN KEY (user_id) REFERENCES users(id)`; constraint types: PRIMARY KEY (unique + NOT NULL), FOREIGN KEY (referential integrity), UNIQUE, NOT NULL, CHECK (expression), DEFAULT; `ON DELETE CASCADE` = child rows deleted when parent is deleted; `ON DELETE SET NULL` = FK set to null; `ON DELETE RESTRICT` = prevent parent deletion if children exist*

**264. What is ALTER TABLE — what can you add, drop, or modify?**
   🔑 *`ALTER TABLE users ADD COLUMN phone VARCHAR(20)` — add nullable column (safe in Postgres 11+ with a default); `ALTER TABLE users DROP COLUMN phone` — remove a column (irreversible, breaks old code); `ALTER TABLE users ALTER COLUMN email TYPE TEXT` — change type (may require a full table rewrite); `ALTER TABLE users ALTER COLUMN phone SET NOT NULL` — add NOT NULL constraint (scans + locks); `ALTER TABLE users ADD CONSTRAINT uq_email UNIQUE(email)` — add constraint; `ALTER TABLE users DROP CONSTRAINT uq_email` — remove constraint; `ALTER TABLE users RENAME COLUMN mobile TO phone` — rename (breaks old code in same deploy!); `ALTER TABLE orders ADD COLUMN archived BOOLEAN NOT NULL DEFAULT false` — adding NOT NULL with DEFAULT is safe in PG 11+ (stored as metadata, no row rewrite); cautious ops: type changes and adding NOT NULL without a default require a full table scan (locks in prod — do expand-contract)*

**265. What are primary keys, foreign keys, and composite keys?**
   🔑 *Primary key (PK): uniquely identifies a row; must be NOT NULL + UNIQUE; one per table; creates a clustered index in MySQL InnoDB; `@Id @GeneratedValue` in JPA; surrogate PK (BIGSERIAL/UUID) vs natural PK (email — unstable, avoid for FKs); Foreign key (FK): references the PK of another table — enforces referential integrity; `REFERENCES users(id) ON DELETE CASCADE/SET NULL/RESTRICT`; prevents orphaned rows; adds an index in PostgreSQL; Composite key: PK spanning multiple columns — `PRIMARY KEY (order_id, product_id)` for a join table; `@IdClass` or `@EmbeddedId` in JPA; composite PK as FK: reference all columns in the FK definition; choose: surrogate PK for most entities (stable, short, indexable); natural/composite PK for pure join tables (`order_items`) where the combination is the identity*

**266. Explain clustered vs non-clustered indexes.**
   🔑 *Clustered index: rows are physically stored in index order; MySQL InnoDB: the PRIMARY KEY is always the clustered index — row data is in the B-tree leaf nodes; PG: no true clustered index (CLUSTER command re-sorts once, not maintained); exactly one per table; range scans on PK are fast (sequential I/O); Non-clustered (secondary) index: a separate B-tree storing key + pointer (in InnoDB: PK value; in PG: heap tuple id); multiple per table; lookup = find key in index → follow pointer to heap row (extra I/O); covering index: include all query columns in the index → index-only scan avoids the heap lookup (`CREATE INDEX ON orders (user_id) INCLUDE (status, amount)`); clustered = data page, non-clustered = separate page; implication: wide PKs in InnoDB increase all secondary index sizes (secondary indexes store the PK)*

**267. What are SQL constraints — UNIQUE, CHECK, NOT NULL, DEFAULT, EXCLUSION?**
   🔑 *NOT NULL: column cannot be NULL — enforced on INSERT/UPDATE; DEFAULT: `DEFAULT NOW()`, `DEFAULT 0`, `DEFAULT 'PENDING'` — used when INSERT omits the column; UNIQUE: no two rows may have the same value; allows one NULL in PG (NULLs are distinct in SQL standard); unique index is created automatically; CHECK: `CHECK (amount > 0)`, `CHECK (status IN ('PENDING','PAID','FAILED'))` — arbitrary boolean expression; enforced at INSERT/UPDATE; FOREIGN KEY: referential integrity between tables; ON DELETE/UPDATE behavior; EXCLUSION (PostgreSQL): generalized constraint using an operator (e.g. no overlapping date ranges: `EXCLUDE USING gist (room_id WITH =, during WITH &&)`); constraint naming: `CONSTRAINT ck_positive_amount CHECK (amount > 0)` — named constraints are easier to drop and error messages are clearer; deferrable constraints: `DEFERRABLE INITIALLY DEFERRED` — checked at commit time instead of per-statement (useful for circular FKs)*

**268. What SQL data types should you use for common fields?**
   🔑 *Integer: `SMALLINT` (2B), `INTEGER`/`INT` (4B), `BIGINT` (8B — use for IDs), `BIGSERIAL`/`SERIAL` for auto-increment; Decimal: `NUMERIC(12,2)` for money (exact), `FLOAT`/`DOUBLE PRECISION` for scientific (floating point — never for money); Text: `VARCHAR(n)` (with limit), `TEXT` (unlimited — PG), `CHAR(n)` (fixed-width, padded — rarely useful); Boolean: `BOOLEAN`; Date/Time: `DATE`, `TIME`, `TIMESTAMP` (no TZ — avoid for events), `TIMESTAMP WITH TIME ZONE` / `TIMESTAMPTZ` (stores in UTC — use for events), `INTERVAL`; UUID: `UUID` (PG native) or `CHAR(36)` / `BINARY(16)`; JSON: `JSON` (text stored as-is), `JSONB` (binary, indexed, preferred); money pitfalls: never use `FLOAT` for money (0.1+0.2 ≠ 0.3 in floating point); use `NUMERIC(19,4)` or store in cents as `BIGINT`; timestamp pitfalls: `LocalDateTime` in Java maps to `TIMESTAMP` (no TZ); use `Instant` → `TIMESTAMPTZ`; binary: `BYTEA` (PG) for small blobs, S3 for large files*

**269. What are sequences and auto-increment in SQL?**
   🔑 *Sequence: a DB object that generates unique integers; `CREATE SEQUENCE order_seq START 1 INCREMENT 1 CACHE 20`; `NEXTVAL('order_seq')` returns the next value; `CURRVAL` returns the last value for this session; auto-increment shorthand: `BIGSERIAL` (PG) = BIGINT + auto-created sequence + NOT NULL + DEFAULT NEXTVAL; MySQL: `AUTO_INCREMENT` on the column; Oracle/SQL Server: `IDENTITY`; Hibernate allocationSize: Hibernate pre-fetches N ids from the sequence (default 50) in one call → allocates ids 1-50 locally → next DB call for 51-100; reduces round-trips for batch inserts; must match `INCREMENT BY` on the DB sequence (`ALTER SEQUENCE user_seq INCREMENT BY 50`); resetting a sequence: `ALTER SEQUENCE order_seq RESTART WITH 1000`; sequence gaps: sequences skip values on transaction rollback or server restart (normal — gaps are expected, don't rely on sequential ids); UUID alternative: `gen_random_uuid()` (PG 13+) or `uuid_generate_v4()` — distributed, no coordination needed*

**270. How do you configure database connectivity in Spring Boot (datasource, JPA, HikariCP)?**
   🔑 *`spring.datasource.url=jdbc:postgresql://localhost:5432/mydb`, `spring.datasource.username=user`, `spring.datasource.password=${DB_PASSWORD}`, `spring.datasource.driver-class-name=org.postgresql.Driver`; HikariCP (default pool): `spring.datasource.hikari.maximum-pool-size=20`, `.minimum-idle=5`, `.connection-timeout=30000`, `.idle-timeout=600000`, `.max-lifetime=1800000`, `.keepalive-time=60000`; JPA: `spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect`, `spring.jpa.hibernate.ddl-auto=validate` (prod — use Flyway instead of `update`/`create`), `spring.jpa.show-sql=false` (prod), `spring.jpa.properties.hibernate.jdbc.batch_size=50`, `spring.jpa.open-in-view=false`; JDBC URL formats: PostgreSQL: `jdbc:postgresql://host:5432/db?ssl=true&sslmode=require`; MySQL: `jdbc:mysql://host:3306/db?serverTimezone=UTC&useSSL=true`; Oracle: `jdbc:oracle:thin:@host:1521:sid`; SQL Server: `jdbc:sqlserver://host:1433;databaseName=db;encrypt=true`; secrets: never hardcode password in `application.yml` — use env var, Vault, or AWS Secrets Manager; `ddl-auto` values: `none` (no DDL), `validate` (check schema matches entities — recommended with Flyway), `update` (incremental — never in prod), `create` (drop+create on start — dev only), `create-drop` (drop on stop — tests)*

**271. What is GRANT and REVOKE in SQL (DCL)?**
   🔑 *GRANT: gives a user/role privileges on a database object; `GRANT SELECT, INSERT, UPDATE ON orders TO app_user`; `GRANT ALL PRIVILEGES ON DATABASE mydb TO admin_user`; `GRANT USAGE, SELECT ON SEQUENCE order_seq TO app_user`; `GRANT EXECUTE ON PROCEDURE calculate_tax() TO app_user`; REVOKE: removes previously granted privileges; `REVOKE DELETE ON orders FROM app_user`; roles: `CREATE ROLE readonly; GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly; GRANT readonly TO reporting_user;`; principle of least privilege: app DB user should have only SELECT/INSERT/UPDATE/DELETE on specific tables — never DROP/CREATE/TRUNCATE in prod; `GRANT SELECT ON pg_stat_statements TO monitoring_user` — give monitoring user access to query stats; `WITH GRANT OPTION`: the grantee can further grant the privilege to others; `REVOKE GRANT OPTION FOR SELECT ON orders FROM app_user` — remove the ability to grant without revoking the privilege itself; ownership: table owner always has all privileges; schema permissions: `GRANT USAGE ON SCHEMA myschema TO app_user` required to access objects in a non-public schema*

**272. What are the basic SELECT, INSERT, UPDATE, DELETE and UPSERT syntax patterns?**
   🔑 *SELECT: `SELECT id, name, email FROM users WHERE status='ACTIVE' AND created_at > NOW()-INTERVAL '30 days' ORDER BY created_at DESC LIMIT 20 OFFSET 0`; INSERT: `INSERT INTO users (name, email, status) VALUES ('Alice', 'alice@example.com', 'ACTIVE')` + `RETURNING id`; multi-row: `INSERT INTO users (name, email) VALUES ('A','a@x.com'), ('B','b@x.com')`; INSERT from SELECT: `INSERT INTO archive SELECT * FROM users WHERE created_at < '2023-01-01'`; UPDATE: `UPDATE orders SET status='CANCELLED', updated_at=NOW() WHERE id=42 AND status='PENDING'`; conditional update: `UPDATE accounts SET balance=balance-100 WHERE id=1 AND balance >= 100` + check affected rows (0 = insufficient balance); DELETE: `DELETE FROM sessions WHERE expired_at < NOW()`; UPSERT (INSERT … ON CONFLICT): `INSERT INTO user_settings (user_id, theme) VALUES (1,'dark') ON CONFLICT (user_id) DO UPDATE SET theme=EXCLUDED.theme` (PostgreSQL); MySQL: `INSERT INTO … ON DUPLICATE KEY UPDATE`; SQL Server: `MERGE INTO … USING … ON … WHEN MATCHED THEN UPDATE … WHEN NOT MATCHED THEN INSERT …`; best practices: always use WHERE on UPDATE/DELETE (no accidental full-table modification), use `RETURNING` to get generated values, prefer `UPSERT` over read-then-write for idempotency*

**273. What is SAVEPOINT and how do you use it for partial rollback?**
   🔑 *SAVEPOINT: a named marker within a transaction to which you can partially rollback without aborting the whole transaction; `BEGIN; UPDATE accounts SET balance=balance-100 WHERE id=1; SAVEPOINT after_debit; INSERT INTO payments (account_id, amount) VALUES (1, 100); ROLLBACK TO SAVEPOINT after_debit; COMMIT;` — the debit is committed but the payment insert is rolled back; multiple savepoints: `SAVEPOINT sp1; ... SAVEPOINT sp2; ... ROLLBACK TO SAVEPOINT sp1;` — rolls back sp2 and sp1's work; `RELEASE SAVEPOINT sp1` — destroys the savepoint but does not rollback; JPA: `NESTED` propagation type uses savepoints — nested transaction can rollback to the savepoint while the outer transaction continues; use cases: batch processing (retry a failed item without aborting the batch), multi-step workflows where some steps are optional, complex business logic where partial success is acceptable; caveats: SAVEPOINTs are within a single connection/transaction — they don't work across services; database support: PostgreSQL (full support), MySQL (InnoDB), Oracle; not supported in REQUIRES_NEW (that is a separate transaction, not a savepoint)*

**274. How do you run schema migrations with zero downtime (Flyway/Liquibase, expand-contract)?**
   🔑 *Expand-contract (parallel change): add nullable column → dual-write → backfill in batches → switch reads → stop writing old → drop; Flyway/Liquibase versioned + repeatable + checksums + baseline; never rename/drop in the same release; avoid long locks (ADD COLUMN default safe PG 11+, CREATE INDEX CONCURRENTLY), `lock_timeout`/`statement_timeout`, backward-compatible for the N-1 app during rolling deploy; separate DDL from big data backfills; Flyway: `V1__create_users.sql`, `V2__add_email_index.sql` — versioned migrations run once and are checksummed (any change breaks flyway validate); `flyway.baseline-on-migrate=true` for existing DBs; `R__refresh_mv.sql` = repeatable migrations (run when checksum changes); Liquibase: `changelog.xml`/`changelog.yaml` with `changeset` blocks — rollback support built-in; `liquibase rollback <tag>` reverts; expand-contract steps: (1) EXPAND: `ALTER TABLE users ADD COLUMN phone VARCHAR(20)` (nullable — no lock, no N-1 app break); (2) dual-write: app writes both `mobile` (old) and `phone` (new); (3) backfill: `UPDATE users SET phone=mobile WHERE phone IS NULL` in batches (small transactions); (4) switch reads to new column; (5) stop writing old column; (6) CONTRACT: `ALTER TABLE users DROP COLUMN mobile` in a later release; `CREATE INDEX CONCURRENTLY` = builds index without a full table lock (PG); `lock_timeout=5s` on DDL statements so they fail fast rather than blocking all writes; separate large data migrations (UPDATE millions of rows) from schema changes — run via a script outside the deploy*

**275. How do you evolve a schema in a backward-compatible way, including constraint/column changes?**
   🔑 *Additive-only, tolerant reader, version the DTO not the table; add constraint `NOT VALID` then `VALIDATE` separately (PG) to avoid long locks; widen types safely, never narrow in one step; add nullable → backfill → set NOT NULL; dual-write during transition; keep old columns readable until all consumers migrate; safe changes (backward-compatible): adding a nullable column, adding a new table, adding an index (CONCURRENTLY), widening a VARCHAR, adding a default; unsafe changes (breaking): renaming/dropping a column (old code references it), adding NOT NULL without a default, narrowing a type, dropping a table; adding NOT NULL safely: `ALTER TABLE t ADD COLUMN c VARCHAR(50)` (nullable, safe) → backfill `UPDATE t SET c='' WHERE c IS NULL` → `ALTER TABLE t ALTER COLUMN c SET NOT NULL` (scan + lock, brief); PG: `ALTER TABLE t ALTER COLUMN c SET NOT NULL` + `ALTER TABLE t ADD CONSTRAINT c_not_null CHECK (c IS NOT NULL) NOT VALID` then `VALIDATE CONSTRAINT` — VALIDATE acquires ShareUpdateExclusiveLock (allows reads+writes); index locks: `CREATE INDEX CONCURRENTLY` = long build, no write lock; `DROP INDEX CONCURRENTLY`; renaming: add new column → dual-write → migrate reads → drop old (never rename directly — breaks old pods); tolerant reader pattern: ignore unknown JSON/DB fields in the app so N-1 deploy works while new fields are being added*

**276. How do you migrate an application to a new database with no downtime?**
   🔑 *Dual-write or CDC (Change Data Capture, e.g. Debezium) replication, backfill history, shadow reads + reconciliation, feature-flag the read switch then the write switch, keep a rollback path, strangler-fig cutover, decommission old DB last; steps: (1) set up target DB + run schema DDL; (2) backfill historical data (bulk copy, pk-range batches, idempotent); (3) enable CDC on source (Debezium captures WAL changes → Kafka → target DB) to keep in sync during backfill; (4) shadow reads: route a % of reads to the new DB, compare results, reconcile discrepancies; (5) feature flag read switch: 100% reads → new DB; (6) feature flag write switch: writes → new DB only (disable CDC sync); (7) decommission old DB; CDC with Debezium: captures PostgreSQL WAL / MySQL binlog → Kafka → consumer writes to target; zero data loss, near real-time lag; Strangler Fig: if migrating SQL→NoSQL, wrap the old DB calls in a service interface, redirect implementations incrementally; rollback: keep old DB writable until confidence; test rollback — can you switch back in < 5 min? key pitfalls: transaction ordering differences, sequence number gaps, time zone mismatches, driver differences; reconciliation: compare row counts + checksum per table; record discrepancies; stop migration only when discrepancy rate < 0.001%*

**277. Explain isolation levels and a real dirty-read/phantom-read bug.**
   🔑 *READ UNCOMMITTED → dirty read; READ COMMITTED → non-repeatable read (PG default); REPEATABLE READ → phantom read (MySQL default; gap locks prevent it); SERIALIZABLE → full isolation (locking/SSI); MVCC snapshots; classic bug = balance check then debit under READ COMMITTED → double spend (fix: SELECT FOR UPDATE / SERIALIZABLE / atomic UPDATE)*

**278. Explain the ACID properties with examples.**
   🔑 *Atomicity = all-or-nothing (transfer debit+credit); Consistency = constraints/invariants hold; Isolation = concurrent txns don't interfere; Durability = committed survives crash (WAL/redo log + fsync)*

**279. Explain the CAP theorem (and PACELC) and how it guides design.**
   🔑 *Under a network Partition you choose Consistency or Availability; CP (strong consistency, may reject — RDBMS/ZooKeeper/HBase) vs AP (always answer, eventual — Dynamo/Cassandra); no "CA" in a distributed system; PACELC adds the latency-vs-consistency trade-off when there's no partition; drives DB + replication choices*

**280. How do you paginate efficiently over a huge table without OFFSET collapse?**
   🔑 *Keyset/seek pagination: `WHERE (created_at, id) < (:ts,:id) ORDER BY ... LIMIT n` — index-driven, O(1) per page; OFFSET scans+discards N rows → O(N); cursor/token APIs, covering index, avoid `COUNT(*)` for totals (estimate / "has next"); keyset (cursor) pagination: `SELECT * FROM orders WHERE (created_at, id) < (:lastCreatedAt, :lastId) ORDER BY created_at DESC, id DESC LIMIT 20` — only reads 20 rows regardless of page number; encode cursor as Base64 token in the API response: `{"data":[...],"nextCursor":"eyJjcmVhdGVkQXQiOiIyMDI0...","hasNext":true}`; client passes cursor to the next request; requirements: (1) stable sort order (include a unique tiebreaker like id), (2) composite index `(created_at, id)` matches the ORDER BY; OFFSET pagination `OFFSET 1000000 LIMIT 20` = DB scans 1,000,020 rows, discards 1,000,000 → extremely slow at high offsets; still usable for small tables or early pages; total count: `COUNT(*)` on a huge table is slow — instead use an estimate (`SELECT reltuples FROM pg_class WHERE relname='orders'`), or just return "hasNext=true"; Spring Data: `PageRequest.of(page, size)` uses OFFSET under the hood — fine for small/medium data, but switch to keyset for high-page access; deferred join trick: `SELECT * FROM orders o JOIN (SELECT id FROM orders ORDER BY created_at DESC LIMIT 20 OFFSET 100000) ids ON o.id=ids.id` — faster OFFSET by fetching only ids first (narrower scan)*

**281. Explain indexing — B-tree indexes and when an index does NOT help.**
   🔑 *B-tree = balanced, sorted, O(log n); supports `=`, range, ORDER BY, composite prefix; doesn't help on low cardinality (use partial/bitmap), leading-wildcard `LIKE '%x'`, function on a column (unless a functional index), tiny tables, poor selectivity (optimizer picks seq scan); also GIN/GiST/BRIN/hash index types; B-tree structure: balanced binary search tree on the index columns; leaf nodes hold key + pointer (heap tid) to the actual row; O(log n) lookup — ~4 levels for a million-row table; supports equality (`=`), range (`<`, `>`, `BETWEEN`), prefix (`LIKE 'abc%'`), ORDER BY (scan already sorted), IS NULL; clustered index (MySQL InnoDB primary key / SQL Server clustered): row data is stored in leaf nodes of the PK index — physically sorted by PK; non-clustered = index points to a heap row; PG has no clustered index concept (CLUSTER command re-sorts once); when an index is NOT used: (1) low cardinality (boolean, status with few values) — optimizer estimates seq scan cheaper; (2) leading wildcard `LIKE '%abc'` — can't use B-tree (use GIN + pg_trgm); (3) function applied to the column: `WHERE UPPER(email) = 'X'` — use a functional index `CREATE INDEX ON users (UPPER(email))`; (4) tiny table — seq scan faster; (5) very high % of rows returned (>20%) — seq scan + bitmap cheaper; (6) implicit cast mismatch (`WHERE int_col = '123'` — cast prevents index use in some DBs); other index types: GIN = inverted index for arrays/JSONB/full-text; GiST = geometric/range types; BRIN = block range (huge sequential tables like log timestamps — tiny size, approx); hash = O(1) equality only, no range*

**282. How do you design composite indexes, and can two indexes be used at once?**
   🔑 *Order = equality → range → sort (ESR rule); leftmost-prefix rule; high-selectivity columns first; covering/INCLUDE index to avoid table lookup; partial index for filtered subsets; usually one index per table access, but optimizer can combine via bitmap index scan (PG) / index merge (MySQL); cost = slower writes + storage*

**283. How do you optimize a slow SQL query?**
   🔑 *EXPLAIN ANALYZE first; index WHERE/JOIN/ORDER BY columns, keep predicates SARGable (no function on column), SELECT only needed columns (projection), avoid N+1 + correlated subqueries, EXISTS over IN for large sets, update statistics, partition/archive huge tables, batch writes, covering index, caching; step-by-step optimization process: (1) `EXPLAIN ANALYZE` → identify Seq Scan on large table, Hash Join spill, sort to disk — find the bottleneck; (2) check if WHERE/JOIN/ORDER BY columns have indexes; add missing ones (`CREATE INDEX CONCURRENTLY`); (3) SARGable = search-ARGument-able — the predicate can use an index: `WHERE created_at >= '2024-01-01'` is SARGable, `WHERE DATE(created_at) >= '2024-01-01'` is NOT (function prevents index); (4) projection: `SELECT id, name` instead of `SELECT *` — less I/O, narrower rows, can use covering index; (5) subquery to JOIN: a correlated subquery runs once per row → rewrite as a JOIN or CTE; (6) `EXISTS` vs `IN`: `WHERE id IN (SELECT user_id FROM orders)` — if subquery returns many rows, EXISTS short-circuits; for small sets IN is fine; (7) update stats: `ANALYZE tablename` — stale statistics cause bad query plans (cardinality misestimates); (8) covering index: add all SELECT/WHERE/ORDER columns to the index — `CREATE INDEX ON orders (user_id, status) INCLUDE (amount, created_at)` — index-only scan avoids heap access; (9) partition huge tables by date range — old partitions are excluded by partition pruning; (10) caching: if the query is read-heavy with tolerable staleness, cache in Redis with a TTL; (11) materialized view: precompute expensive aggregations, refresh on a schedule*

**284. How do you read and use an EXPLAIN plan (join strategies)?**
   🔑 *Plan shows access path (seq/index scan), join algorithm (nested loop / hash / merge), estimated vs actual rows, cost, sort/hash spill to disk; nested loop = small driving set + indexed inner; hash join = large unsorted; merge join = pre-sorted; fix misestimates with ANALYZE, add composite index on join+filter keys; reading EXPLAIN ANALYZE output: `Seq Scan on orders (cost=0.00..52.00 rows=1000 width=60) (actual time=0.01..1.23 rows=987 loops=1)` — cost is in arbitrary units (not ms), rows=estimated, actual shows real numbers; large gap between estimated and actual rows = stale stats (run ANALYZE); `Index Scan using idx_orders_user on orders` = index used (good); `Bitmap Heap Scan` = multiple index lookups combined via bitmap (good for moderate selectivity); `Hash` node = building hash table in memory; `Hash Batches: 8` = hash spilled to disk (insufficient `work_mem` — increase with `SET work_mem='256MB'`); join strategies: Nested Loop = best when outer side is small and inner has an index — O(n*log(m)); Hash Join = build hash table on smaller relation, probe with larger — good for large unsorted tables, O(n+m); Merge Join = both sides pre-sorted on join key — uses indexes or explicit sort; optimizer chooses based on cost estimates; force with `SET enable_hashjoin=off` for testing; actual plan interpretation: sort by actual time → rightmost (deepest) child with highest cumulative time is the bottleneck; `rows removed by filter` = rows scanned but not matched (consider a better index); `Planning time` vs `Execution time`*

**285. Explain the different SQL joins.**
   🔑 *INNER (matches only), LEFT/RIGHT OUTER (keep all of one side + NULLs), FULL OUTER, CROSS (cartesian), SELF, SEMI (EXISTS), ANTI (NOT EXISTS); watch row multiplication on one-to-many; join on indexed keys; NULL behaviour in outer joins*

**286. WHERE vs HAVING.**
   🔑 *WHERE filters rows before grouping (no aggregates); HAVING filters groups after GROUP BY (can use aggregates); logical order FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT; push conditions into WHERE for performance*

**287. COUNT(*) vs COUNT(1) vs COUNT(column) vs COUNT(DISTINCT).**
   🔑 *COUNT(*) counts rows (optimizer picks cheapest index) — identical to COUNT(1) in practice; COUNT(column) skips NULLs; COUNT(DISTINCT col) is much more expensive (sort/hash); for huge tables use estimates (`pg_class.reltuples`)*

**288. Explain window functions and CTEs.**
   🔑 *Window functions compute over a partition without collapsing rows: `ROW_NUMBER/RANK/DENSE_RANK`, `LAG/LEAD`, running `SUM/AVG OVER (PARTITION BY ... ORDER BY ...)`; CTE (`WITH`) for readability/recursion (hierarchies); beware CTE materialization costs; use for top-N-per-group, de-dup, running totals; window function anatomy: `function() OVER (PARTITION BY col ORDER BY col ROWS/RANGE BETWEEN ...)` — the OVER clause defines the window; `ROW_NUMBER()` = unique sequential number per partition (no ties); `RANK()` = ties get the same rank, gap after (`1,1,3`); `DENSE_RANK()` = ties, no gap (`1,1,2`); `NTILE(4)` = divide into quartiles; `LAG(salary, 1)` / `LEAD(salary, 1)` = previous/next row's value; `SUM(amount) OVER (PARTITION BY user_id ORDER BY txn_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)` = running total per user; top-N-per-group: `SELECT * FROM (SELECT *, ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) rn FROM employees) sub WHERE rn <= 3` — top 3 salaries per department; de-duplication: `ROW_NUMBER() OVER (PARTITION BY email ORDER BY created_at DESC) = 1` — keep newest per email; CTE (`WITH`): `WITH recent_orders AS (SELECT * FROM orders WHERE created_at > NOW()-INTERVAL '30 days') SELECT * FROM recent_orders WHERE amount > 100` — readable, avoids repeating subqueries; recursive CTE for hierarchies: `WITH RECURSIVE tree AS (SELECT * FROM categories WHERE parent_id IS NULL UNION ALL SELECT c.* FROM categories c JOIN tree t ON c.parent_id=t.id) SELECT * FROM tree` — traverses org chart/BOM; CTE materialization: PostgreSQL 12+ CTEs are not materialized by default (treated as inline views); add `MATERIALIZED` keyword to force, `NOT MATERIALIZED` to inline*

**289. SQL vs NoSQL — differences, when to pick each, and the cons of Postgres vs MongoDB.**
   🔑 *SQL = schema-on-write, ACID, joins, strong consistency, vertical scale, reporting; NoSQL = schema-on-read, BASE/eventual, horizontal scale, denormalized, access-pattern-first; decide by consistency/relationships/query shape/scale; Postgres cons = scaling limits + connection cost (PgBouncer) + VACUUM/bloat; Mongo cons = weak joins ($lookup), memory-hungry working set, eventual consistency on secondaries, schema drift; often polyglot persistence*

**290. What is connection pooling, how do you size a pool, and how do you diagnose pool exhaustion?**
   🔑 *Reuse DB connections (expensive TCP+auth+session) via HikariCP; size small (≈ `cores × 2` or `connections = ((core_count × 2) + effective_spindle)`, NOT hundreds) — DB can't parallelize beyond its capacity; `maximumPoolSize`, `connectionTimeout`, `idleTimeout`, `maxLifetime` < DB timeout; exhaustion = threads waiting on `getConnection` (pending > 0) → slow queries holding connections, OSIV, leaks (unclosed), long transactions; PgBouncer for many clients*

**291. Polling vs push/event-driven — how do you avoid inefficient DB polling?**
   🔑 *Polling = repeated SELECTs (simple but wasteful, lag vs load trade-off) → tune interval, index the status column, `SELECT ... FOR UPDATE SKIP LOCKED` for a work queue; prefer push: DB triggers/LISTEN-NOTIFY (PG), CDC/outbox (Debezium → Kafka), webhooks, message queues, long-polling / Server-Sent Events / WebSocket for clients; batch instead of row-by-row polling*

**292. What are stored procedures and functions — when should you use them (and not)?**
   🔑 *Precompiled SQL/procedural code (PL/pgSQL, T-SQL) running in the DB; pros = set-based performance, fewer round-trips, central data-side logic, atomic; cons = business logic hidden from VCS/tests, DB-vendor lock-in, hard to debug/version, scaling tied to the DB; use for heavy set operations/ETL/bulk, keep domain logic in the app; function returns a value (usable in queries), procedure can do transactions/multiple result sets; stored procedure anatomy: `CREATE OR REPLACE PROCEDURE update_status(IN p_id BIGINT, IN p_status TEXT) LANGUAGE plpgsql AS $$ BEGIN UPDATE orders SET status=p_status WHERE id=p_id; COMMIT; END; $$; CALL update_status(1, 'SHIPPED');`; function: `CREATE FUNCTION get_balance(p_id BIGINT) RETURNS NUMERIC LANGUAGE sql AS $$ SELECT SUM(amount) FROM ledger WHERE account_id=p_id $$;` — callable in SELECT; differences: function = returns a value, can be used in SQL expressions, CANNOT do explicit COMMIT/ROLLBACK; procedure = called via CALL, can issue COMMIT, can have multiple result sets; performance benefits: (1) reduces network round-trips (one CALL vs many SQL statements), (2) query plan cached (reused), (3) set-based bulk ops much faster than row-by-row app loops; when NOT to use: business rules (hard to test/mock), complex branching logic (use service layer), domain events (invisible to the app), vendor lock-in risk; compromise: use stored procs for ETL/heavy DML, keep all business logic in the app; PL/pgSQL debugging: `RAISE NOTICE 'val=%', my_var;` logs to the DB client*

**293. What are cursors and triggers — when should you use a trigger?**
   🔑 *Cursor = row-by-row pointer over a result set (heavy — prefer set-based SQL); Trigger = procedural code fired on INSERT/UPDATE/DELETE (BEFORE/AFTER, row/statement); use for audit trails, enforcing invariants, CDC/outbox; avoid for business logic (hidden side effects, cascading triggers, hard to test); cursor: `DECLARE cur CURSOR FOR SELECT id, amount FROM orders; FETCH NEXT FROM cur INTO @id, @amt; CLOSE cur; DEALLOCATE cur;` — row-by-row, slow (N round-trips vs 1 set-based query); use only when set-based SQL is impossible; trigger anatomy: `CREATE OR REPLACE TRIGGER audit_trigger AFTER UPDATE ON users FOR EACH ROW EXECUTE FUNCTION log_changes(); CREATE FUNCTION log_changes() RETURNS TRIGGER AS $$ BEGIN INSERT INTO audit_log(table_name,old_val,new_val,changed_at) VALUES('users',OLD.name,NEW.name,NOW()); RETURN NEW; END; $$ LANGUAGE plpgsql;`; BEFORE trigger: can modify NEW row (validation/auto-fill); AFTER trigger: audit/event/CDC; FOR EACH ROW vs FOR EACH STATEMENT; INSTEAD OF trigger: on views (translates view DML to base table); when to use triggers: (1) audit trail (change log), (2) CDC outbox (capture change events for Debezium), (3) enforce invariants that SQL constraints can't express, (4) automatic computed columns (like `updated_at = NOW()`); when NOT to: business logic (hidden from app, bypassed by direct SQL), cascading (hard to trace), triggers calling triggers; `spring.jpa.properties.hibernate.show_sql=true` won't show trigger effects — check the DB directly*

**294. How are rows stored physically, and how do tables relate?**
   🔑 *Row stored in a fixed-size page/block (8KB PG, 16KB InnoDB) in a heap/segment file; located by ctid/RID (page+slot) or, in InnoDB, in the clustered PK B-tree leaf; secondary index stores key + pointer (ctid/PK); relations are logical via FK values, not physical pointers; TOAST/overflow for large values*

**295. Explain MVCC, WAL and VACUUM (Postgres).**
   🔑 *MVCC = each transaction sees a snapshot; writers don't block readers (old row versions kept with xmin/xmax); WAL (Write-Ahead Log) = durability + crash recovery + replication source; dead tuples accumulate → VACUUM reclaims space + updates visibility map, ANALYZE refreshes stats, autovacuum tuning; bloat + transaction-id wraparound risks; MVCC (Multi-Version Concurrency Control): each UPDATE creates a new row version (tuple) with `xmin` (creating txn id) and `xmax` (deleting txn id); a reader sees all versions where `xmin` committed before its snapshot and `xmax` has not yet committed; writers NEVER block readers (no shared lock on reads) — key advantage over lock-based isolation; DELETEs mark rows with `xmax` but don't physically remove them; dead tuples = old row versions no longer visible to any active transaction; WAL (Write-Ahead Log): every change is written to the WAL (a sequential log) BEFORE the data page is written; crash recovery = replay WAL from the last checkpoint; streaming replication = send WAL to replicas; `wal_level=logical` for logical replication/Debezium CDC; `checkpoint_completion_target` tunes how aggressively PG flushes; VACUUM: reclaims space from dead tuples, updates the visibility map (allows index-only scans), prevents txn-id wraparound (XID wraps at 2^31); `VACUUM ANALYZE` = reclaim + refresh stats; `VACUUM FULL` = rewrite table (exclusive lock, shrinks file — avoid on prod); autovacuum: runs in the background, triggered when dead tuple % exceeds threshold; tune `autovacuum_vacuum_cost_delay`, `autovacuum_vacuum_scale_factor`; table bloat: if autovacuum can't keep up (heavy write workload) → table grows without shrinking → slow scans; monitor with `pg_stat_user_tables.n_dead_tup`*

**296. Explain database replication and read replicas.**
   🔑 *Primary-replica (streaming/logical) replication; read replicas offload read traffic (eventual consistency → replication lag → read-your-writes issues); synchronous (durable, slower) vs asynchronous (fast, data-loss window); automatic failover/promotion (Patroni), split read/write at the app via `AbstractRoutingDataSource`; multi-primary caveats*

**297. Partitioning vs sharding — what's the difference?**
   🔑 *Partitioning = split one table within a DB by range/list/hash (pruning, easier archival, same node); sharding = horizontal split across nodes by a shard key (scale-out); sharding costs = cross-shard joins/transactions, resharding, global unique ids (Snowflake), fan-out; consistent hashing to limit movement; pick a shard key with even distribution + matches query pattern*

**298. How do you handle multiple databases in a single service?**
   🔑 *Multiple `DataSource` beans (+`@Primary`), separate EntityManagerFactory/TransactionManager per source, `@EnableJpaRepositories(basePackages, entityManagerFactoryRef=...)`, separate entity/repo packages; no single transaction across them unless XA/JTA (avoid) → per-DB transactions + Saga/outbox for consistency; `AbstractRoutingDataSource` for read/write or tenant routing; configuration: `@Bean @Primary @ConfigurationProperties("spring.datasource.primary") DataSource primaryDs()` + `@Bean @ConfigurationProperties("spring.datasource.secondary") DataSource secondaryDs()`; separate EntityManagerFactory: `LocalContainerEntityManagerFactoryBean` with `packagesToScan` pointing to each DB's entity package; `@EnableJpaRepositories(basePackages="com.example.primary", entityManagerFactoryRef="primaryEmf", transactionManagerRef="primaryTx")`; each DB gets its own `HikariDataSource` pool (size them independently); cross-database consistency: XA/JTA (2-phase commit) = distributed locking, poor throughput, Narayana/Atomikos, avoid unless legacy requirement; Saga pattern = per-DB transactions + compensating actions on failure; Outbox pattern = write to local DB + outbox table, CDC/poller forwards events to trigger updates in the other DB; AbstractRoutingDataSource for tenant routing: `determineCurrentLookupKey()` reads `TenantContextHolder.getTenant()` → returns the tenant-specific datasource key; register each tenant's `HikariDataSource` in the lookup map*

**299. Explain database normalization (and when to denormalize).**
   🔑 *1NF atomic values, 2NF no partial dependency on part of a composite key, 3NF no transitive dependency, BCNF stricter; benefits = fewer anomalies + redundancy; denormalize deliberately for read performance with a refresh/consistency strategy (materialized views, snapshot columns, CQRS read model); 1NF: each column must have atomic (indivisible) values — no arrays, no comma-separated values; each row must be unique; bad: `orders(id, items="pen,book,ruler")`; good: `order_items(order_id, item_name)`; 2NF: no partial dependency — non-key columns must depend on the WHOLE primary key (matters for composite keys); bad: `order_items(order_id, product_id, product_name)` where product_name depends only on product_id; good: move product_name to a `products` table; 3NF: no transitive dependency — non-key columns must depend only on the PK, not on other non-key columns; bad: `employees(id, dept_id, dept_name)` where dept_name depends on dept_id not id; good: separate `departments(id, name)` table; BCNF: stricter — every determinant must be a candidate key (handles edge cases in 3NF); when to denormalize: (1) dashboard/report queries joining 8 tables are slow → materialized view or snapshot column; (2) historical accuracy (price at purchase time) → snapshot `price_paid`; (3) CQRS read model = separate denormalized table updated by events; (4) avoid JOIN cost for read-heavy microservices; always: make denormalization explicit, refresh strategy defined, consistency documented; don't denormalize prematurely*

**300. What is a materialized view and when do you use it?**
   🔑 *A view whose result is physically stored (unlike a normal view); fast reads for expensive aggregations/joins; must be refreshed (`REFRESH MATERIALIZED VIEW [CONCURRENTLY]`) → serves slightly stale data; good for dashboards/reports; index it; alternative to denormalized read tables / CQRS; regular view: `CREATE VIEW monthly_revenue AS SELECT date_trunc('month', order_date), SUM(amount) FROM orders GROUP BY 1;` — executes the underlying query every time (no storage); materialized view: `CREATE MATERIALIZED VIEW monthly_revenue_mv AS SELECT ... ;` — stores the result as a physical table; refresh: `REFRESH MATERIALIZED VIEW monthly_revenue_mv;` — full replacement, briefly unavailable; `REFRESH MATERIALIZED VIEW CONCURRENTLY monthly_revenue_mv;` — allows concurrent reads during refresh (requires a unique index on the MV); scheduling refresh: `pg_cron`, AWS EventBridge → Lambda, Spring `@Scheduled`; use cases: (1) nightly/hourly aggregations for dashboards, (2) expensive multi-table joins with infrequent data changes, (3) pre-aggregated analytics (avoid running on the OLTP DB), (4) read model in CQRS; index a materialized view: `CREATE INDEX ON monthly_revenue_mv (month);` — it's a physical table, supports indexes; staleness: MV is not real-time; for near-real-time, use a normalized view + covering index, or CQRS with event-driven updates; alternatives: Redis cache, Elasticsearch (analytics), dedicated OLAP store*

**301. What is database sharding and how do you choose a shard key?**
   🔑 *Horizontal partitioning across nodes; strategies = range, hash, directory/lookup, geo; good key = even distribution, no hotspot, matches the main query; costs = cross-shard joins/tx, rebalancing, global ids, fan-out queries; consistent hashing to minimize data movement on resize*

**302. Explain MongoDB's aggregation pipeline.**
   🔑 *`$match` early (uses index) → `$project` → `$group` (`$sum`/`$avg`/`$push`) → `$sort` before `$limit` → `$lookup` (left join) → `$unwind` → `$facet`/`$addFields` → `$out`/`$merge`; order matters (filter first), `allowDiskUse` for large sorts, index the `$match`/`$sort` fields; pipeline mechanics: each stage transforms the document stream — `{ $match: { status: "ACTIVE" } }` → filters early (uses index, reduces docs for later stages); `{ $project: { name: 1, email: 1, _id: 0 } }` → shape/rename fields; `{ $group: { _id: "$dept", total: { $sum: "$salary" }, avg: { $avg: "$salary" } } }` → aggregate per group; `{ $sort: { total: -1 } }` → sort (put BEFORE `$limit` for top-N, index-backed if early in pipeline); `{ $limit: 10 }` → first N docs; `{ $lookup: { from: "orders", localField: "_id", foreignField: "user_id", as: "orders" } }` → left outer join (SQL equivalent); `{ $unwind: "$orders" }` → flatten the joined array (one doc per order); `{ $addFields: { fullName: { $concat: ["$first", " ", "$last"] } } }` → computed fields; `{ $facet: { byStatus: [{$group...}], byDate: [{$group...}] } }` → multiple sub-pipelines in one pass; `{ $out: "report_collection" }` / `{ $merge: ... }` → write result to a collection; optimization tips: (1) `$match` + `$sort` as early as possible (index-backed), (2) `allowDiskUse: true` for large sorts that exceed 100MB RAM, (3) use `explain()` to see index usage; Spring Data MongoDB: `mongoTemplate.aggregate(Aggregation.newAggregation(match(...), group("dept").sum("salary").as("total"), sort(DESC,"total"), limit(10)), User.class, ReportDto.class)`*

**303. When do you use Elasticsearch over SQL LIKE queries?**
   🔑 *Inverted index = term → posting list of doc ids via an analyzer (tokenize/lowercase/stem/stopwords); gives relevance scoring (BM25/TF-IDF), fuzzy/typo tolerance, facets, millisecond full-text; SQL `LIKE '%x%'` = full scan, no ranking; ES is near-real-time, NOT a system of record (sync via CDC/outbox); SQL LIKE limitations: `WHERE description LIKE '%payment failed%'` = full table scan (can't use B-tree index for leading wildcard), no relevance ranking, no typo tolerance, no language analysis; Elasticsearch inverted index: tokenizer splits text → analyzer pipeline (lowercase, stemmer, stopwords) → terms stored in inverted index mapping `term → [doc1, doc3, doc7]`; query: `{"query":{"multi_match":{"query":"paymet failed","fields":["title^2","description"],"fuzziness":"AUTO"}}}` — fuzzy matching corrects typos, `^2` boosts title matches; BM25 scoring ranks by term frequency + inverse document frequency; use ES for: (1) full-text search (product catalog, article search), (2) autocomplete/typeahead (`edge_ngram` analyzer), (3) fuzzy/typo-tolerant search, (4) faceted search (filter by category+brand+price range), (5) relevance ranking; keep RDBMS as source of truth: sync via CDC (Debezium → Kafka → ES consumer) or outbox pattern; Spring Data ES: `@Document(indexName="products") class Product { @Id String id; @Field(type=Text,analyzer="english") String description; }` + `ElasticsearchOperations`; don't use ES for transactions, joins, or when latency < 1s is needed for single-record lookups (use Redis/DB)*

**304. What ORMs/SQL frameworks exist beyond JPA, and when would you pick them?**
   🔑 *JPA/Hibernate (ORM), Spring Data JPA, MyBatis (SQL mapper), jOOQ (typesafe SQL DSL), Spring JdbcTemplate, QueryDSL; pick jOOQ/MyBatis/JdbcTemplate when SQL is complex, performance-critical, or you want full control over the generated SQL; JPA/Hibernate: full ORM — entity→table mapping, dirty checking, caching, associations, polymorphism; highest abstraction, can hide complexity and performance issues; MyBatis: SQL mapper — you write SQL in XML/annotations, framework maps ResultSet to objects; `@Select("SELECT * FROM users WHERE id = #{id}") User findById(Long id);`; fine-grained SQL control, no magic, good for complex queries, ported SQL from legacy systems; jOOQ: generates Java classes from the DB schema (typesafe DSL); `dslContext.select(USER.NAME, ORDER.AMOUNT).from(USER).join(ORDER).on(USER.ID.eq(ORDER.USER_ID)).where(USER.ACTIVE.eq(true)).fetch()` — SQL is checked at compile time; vendor-specific SQL dialects, stored proc calls, `RETURNING`, MERGE; JdbcTemplate: Spring's thin wrapper over JDBC; `jdbcTemplate.query("SELECT * FROM users WHERE status=?", new Object[]{"ACTIVE"}, rowMapper)` — no abstraction overhead, full SQL control, batchUpdate; `NamedParameterJdbcTemplate` for named params; choose when: JPA → standard CRUD, associations, lifecycle management; jOOQ → complex SQL, schema-driven, compile-time safety; MyBatis → you want to write SQL but want mapping; JdbcTemplate → simple, fast, no frills, batch; raw JDBC → performance-critical tight loops (avoid in Spring)*

**305. What is an RDBMS?**
   🔑 *Relational model = tables/rows/columns, keys (PK/FK/unique/composite), normalization, SQL, ACID transactions, constraints + referential integrity, schema-on-write; examples Postgres, MySQL, Oracle, SQL Server*

**306. How do you choose a database for a given requirement (decision framework)?**
   🔑 *Drive from: data shape + relationships (relational vs document vs graph), access pattern (point lookup, range, full-text, analytical), consistency needs (strong vs eventual), scale + write throughput (vertical vs horizontal/sharding), latency SLA, transactions, durability, operational maturity + cost; relational default for transactional integrity; specialized store when one dimension dominates; often polyglot persistence + CQRS read stores; don't pick by hype*

**307. Explain the NoSQL families and when to use each.**
   🔑 *Key-value (Redis/DynamoDB) = caching, sessions, simple fast lookups; Document (MongoDB) = flexible nested aggregates read together; Wide-column (Cassandra/HBase) = massive write throughput, time-series, known query patterns, tunable consistency; Graph (Neo4j) = relationship-heavy traversals (fraud, recommendations, social); Time-series (InfluxDB/Timescale) = metrics/IoT with retention + downsampling; Search (Elasticsearch) = full-text + relevance; model around access patterns, not normalization*

**308. OLTP vs OLAP — and data warehouse vs data lake.**
   🔑 *OLTP = many small low-latency transactions, normalized, row-store (Postgres/MySQL); OLAP = analytical aggregations over large scans, denormalized/star schema, column-store (Redshift/BigQuery/Snowflake/ClickHouse); warehouse = structured, schema-on-write, curated; lake = raw/any format, schema-on-read (S3 + Spark); lakehouse combines both; move OLTP→analytics via ETL/ELT + CDC, don't run heavy analytics on the OLTP DB; OLTP characteristics: high concurrency, low latency (ms), small result sets (one user's orders), normalized schema (3NF), row-store (full row accessed per transaction), index-optimized; examples: Postgres, MySQL, Oracle; OLAP characteristics: low concurrency, high throughput scans (minutes on billions of rows), aggregations (SUM/GROUP BY over the whole table), denormalized star schema (fact table + dimension tables), column-store (only the columns needed are read — I/O efficient for aggregation); examples: Redshift, BigQuery, Snowflake, ClickHouse, DuckDB; star schema: `fact_orders(order_id, user_id, product_id, date_id, amount)` joined to `dim_users`, `dim_products`, `dim_date` — enables fast slicing; data warehouse: structured, schema-on-write, ETL transforms data before loading, curated/governed, optimized for BI tools; data lake: raw data (JSON, CSV, Parquet) in S3/GCS, schema-on-read (applied at query time by Spark/Athena), cheap storage, flexible; lakehouse: Delta Lake/Iceberg/Hudi adds ACID + schema enforcement on top of a data lake; ETL pipeline: OLTP DB → Debezium CDC → Kafka → Spark/Flink transform → Parquet in S3 → Snowflake/Redshift; never run analytical queries (full table scans, GROUP BY millions of rows) on the OLTP DB — it kills OLTP performance*

**309. What is NewSQL / distributed SQL, and when would you use it?**
   🔑 *Distributed SQL (CockroachDB, Google Spanner, YugabyteDB, Citus, Vitess) = horizontal scale + SQL + ACID + strong consistency via consensus (Raft/Paxos) + automatic sharding/replication; use when you've outgrown a single-node RDBMS but still need transactions + joins + global consistency (multi-region); trade-off = higher write latency (cross-node consensus), cost, operational complexity; problem it solves: traditional RDBMS (Postgres/MySQL) doesn't scale horizontally for writes — you must shard manually; NoSQL scales but sacrifices SQL/ACID; NewSQL = both; CockroachDB: Raft-based consensus per range (64MB data shards), auto-rebalancing, PostgreSQL-compatible wire protocol, geo-partitioning, Serializable isolation; Google Spanner: TrueTime API (atomic clocks + GPS) for globally synchronized timestamps → external consistency; multi-region writes with ~10-50ms commit latency; YugabyteDB: open-source Spanner-like, PostgreSQL + Cassandra APIs; Citus: PostgreSQL extension that distributes tables across worker nodes via shard keys — same codebase as Postgres; Vitess: horizontal sharding of MySQL (YouTube uses it); when to use: (1) too many writes for one Postgres primary, (2) need multi-region active-active with strong consistency (payments, fintech), (3) want SQL/ACID without manual sharding; trade-offs: write latency higher (cross-node Raft commit), limited cross-shard transactions, operational complexity, cost; for most applications, start with Postgres + read replicas + PgBouncer + careful indexing before moving to distributed SQL*

**310. How do you make database access resilient (failover, retries, pool health)?**
   🔑 *Connection pool with `maxLifetime` < DB timeout + validation query/`keepaliveTime`; retry transient errors (deadlock, connection reset) with backoff (idempotent); fast-fail with `connectionTimeout`; read replicas + automatic primary failover (Patroni/RDS Multi-AZ) with DNS/endpoint switch; circuit breaker around the DB; `statement_timeout`/`lock_timeout`; health-check indicator; graceful degradation to cache when the DB is down; HikariCP tuning: `connectionTimeout=30s` (fail fast on pool exhaustion), `idleTimeout=600s`, `maxLifetime=1800s` (< DB `wait_timeout`/`tcp_keepalives_idle`), `keepaliveTime=60s` (sends a keepalive query to prevent idle connection death by NAT/firewall), `minimumIdle=5`, `maximumPoolSize=20`, `validationTimeout=5s`; test-on-borrow: HikariCP validates connections before giving them out (default); retry: `@Retryable(value={DataAccessException.class}, maxAttempts=3, backoff=@Backoff(delay=100, multiplier=2))` — only retry on idempotent operations (reads + insert-if-not-exists); deadlock retry: catch `CannotAcquireLockException` and retry; primary failover: RDS Multi-AZ = synchronous replication + DNS failover (~30-60s); AWS Aurora = faster failover (~30s); Patroni (self-hosted PG HA): Etcd/Consul leader election → promotes replica → updates DNS; app needs `connectTimeout` and retry on reconnect to the new primary; circuit breaker: Resilience4j `@CircuitBreaker` around the DB call — opens after N failures, returns fallback (cached data, empty) during open state; `spring.datasource.hikari.connection-test-query=SELECT 1`; `GET /actuator/health` → `db` indicator shows pool state*

[⬆️ Back to top](#contents)

## REST API Design & Web Layer

> *Full forms: **REST** = Representational State Transfer · **SOAP** = Simple Object Access Protocol · **gRPC** = gRPC Remote Procedure Calls (over HTTP/2 + Protobuf) · **CORS** = Cross-Origin Resource Sharing · **HATEOAS** = Hypermedia As The Engine Of Application State.*

**311. What are the core REST principles and the Richardson Maturity Model?**
   🔑 *REST (Representational State Transfer) = resources + URIs + HTTP verbs + representations (JSON), stateless, cacheable, uniform interface, layered, client-server; Richardson levels 0 (RPC-over-HTTP) → 1 (resources) → 2 (HTTP verbs + status codes) → 3 (hypermedia/HATEOAS); idempotency + safety per verb*

**312. Explain HTTP methods and status codes, including safe vs idempotent.**
   🔑 *GET (safe/idempotent), HEAD, OPTIONS, PUT (idempotent full replace), DELETE (idempotent), PATCH (partial, not necessarily idempotent), POST (neither); status 2xx success (200/201/202/204), 3xx redirect (301/302/304), 4xx client (400/401/403/404/409/422/429), 5xx server (500/502/503/504); safe ⇒ idempotent, not vice versa; you can't invent 6xx (IANA registry) — use a standard code + app error code in the body*

**313. How do you design idempotent APIs (and why it matters for payments)?**
   🔑 *Client-generated `Idempotency-Key` header → unique index on (key, endpoint) + stored request fingerprint + response; same key+payload replays the stored response, different payload → 409; TTL cleanup; naturally idempotent = PUT with client id / UPSERT / state-machine transition guarded by `WHERE status=?`; matters because network timeouts cause retries → double charge*

**314. How do you version a REST API and evolve the contract without breaking clients?**
   🔑 *URI (`/v1/`) = simple/visible/cacheable; header (`Accept-Version`) = clean URLs; media type (`application/vnd.x.v2+json`) = purest, poor tooling; version only on breaking change, additive-only/tolerant reader, consumer-driven contract tests (Pact/Spring Cloud Contract), N-1 support, `Sunset`/deprecation headers; separate controllers per version sharing a service; gateway routing*

**315. Can you use controller method overloading for versioning, and how do you handle a v2 that does extra work?**
   🔑 *No — Java overloading can't dispatch on version and Spring fails on ambiguous mappings; use distinct paths (`/v1`,`/v2`) or `headers=`/`produces=` on `@RequestMapping`, separate controllers, shared service; a v2 that makes an extra DB call enriches the response (new field is additive, old clients ignore it), v1 skips it; feature-flag + metrics until deprecation*

**316. Difference between PUT and PATCH.**
   🔑 *PUT = full replace, idempotent, missing fields reset; PATCH = partial update, not necessarily idempotent, JSON Merge Patch (RFC 7396) or JSON Patch (RFC 6902 op/path/value); PATCH needs careful null-vs-absent semantics (Optional/JsonNullable)*

**317. SOAP vs REST — differences, and why would payments still use SOAP?**
   🔑 *SOAP (Simple Object Access Protocol) = XML envelope, WSDL contract + codegen, WS-Security (message-level signing/encryption), WS-AtomicTransaction, protocol-agnostic, heavy, non-repudiation; REST = resources + HTTP verbs, JSON, stateless, lightweight, cacheable; payments/banking keep SOAP for legacy rails (ISO 20022, SWIFT), formal contracts, message-level security, regulatory/partner mandate; integrate legacy SOAP via JAX-WS/wsimport, Spring WS, Apache CXF*

**318. REST vs GraphQL vs gRPC — when would you choose each?**
   🔑 *REST = public APIs, simplicity, HTTP caching, ubiquitous tooling; GraphQL = client-driven queries (no over/under-fetch), one endpoint + schema + resolvers, but N+1 (DataLoader), caching/rate-limiting harder; gRPC (gRPC Remote Procedure Calls) = HTTP/2 + Protobuf (binary), 4 streaming modes (unary/server/client/bidi), deadlines, codegen, low latency for internal service-to-service, poor browser support (needs grpc-web/gateway); REST: URL resources, HTTP verbs, JSON, stateless; cacheable (GET via CDN/browser), universal tooling (Postman/curl/browser), versioned via URL/header; best for: public APIs, third-party integrations, microservices without strict performance needs; GraphQL: single `/graphql` endpoint, client specifies exact fields needed (no over-fetching), schema = contract; resolvers fetch data; N+1 solved with DataLoader (batch + deduplicate); caching harder (all POST, body varies); persisted queries for production caching; best for: BFF (backend-for-frontend) aggregating multiple services, mobile (bandwidth-sensitive), complex data graphs; gRPC: Protocol Buffers (binary, typed) → IDL `.proto` generates client/server stubs in any language; streaming = unary (req/resp), server-streaming (one req, many responses), client-streaming, bidirectional; HTTP/2 = multiplexing (no head-of-line blocking), header compression; deadlines propagated; best for: internal service-to-service (service mesh), streaming, polyglot microservices needing strong contracts; not browser-compatible without grpc-web + Envoy; decision: public → REST; BFF/mobile → GraphQL; internal high-perf/streaming → gRPC*

**319. What is the HTTP protocol, and what alternatives exist for client-server communication?**
   🔑 *HTTP = stateless request/response application protocol over TCP — methods, status codes, headers, caching, content negotiation; text-based, firewall/proxy-friendly, universal; HTTP/2 multiplexing, HTTP/3 over QUIC; alternatives = WebSocket (full-duplex), gRPC, Server-Sent Events (SSE), MQTT/AMQP, raw TCP/UDP; HTTP/1.1: one request per TCP connection (or persistent keep-alive), head-of-line blocking; text-based headers, human-readable; HTTP/2: binary framing, multiplexing (many requests over one TCP connection — no head-of-line blocking at HTTP layer), header compression (HPACK), server push; same TCP → still has TCP head-of-line blocking; HTTP/3 over QUIC: UDP-based, QUIC handles reliability/ordering per stream → no TCP head-of-line blocking, faster connection setup (0-RTT); WebSocket: full-duplex over a single long-lived TCP connection; upgrades from HTTP via `Upgrade: websocket` header; bidirectional messages (chat, live feeds, collaborative editing); Spring: `@EnableWebSocket` + `WebSocketHandler` or STOMP over WebSocket; SSE (Server-Sent Events): one-way server→client push over HTTP; `text/event-stream` content type; auto-reconnect; simpler than WebSocket for push-only; Spring: `SseEmitter`; MQTT: lightweight pub/sub for IoT (small payloads, QoS levels 0/1/2); AMQP (RabbitMQ): message queuing protocol; raw TCP/UDP: custom protocols (gaming, HFT), gRPC sits on HTTP/2 over TCP; choose based on: directionality (uni vs bi), latency requirement, browser support, firewall traversal*

**320. How do you notify the frontend that a long task is complete?**
   🔑 *Return 202 Accepted + job id + status endpoint; push via WebSocket (STOMP/SockJS), Server-Sent Events (one-way), long polling fallback, or a webhook callback; back it with a job-status table and a completion event (Kafka) consumed by a notification service; pattern: `POST /reports/generate` → `202 Accepted {"jobId":"abc123","statusUrl":"/jobs/abc123"}`; frontend options: (1) polling: `GET /jobs/abc123` every 2s → `{"status":"RUNNING"}` ... `{"status":"DONE","result":"..."}` — simple, works everywhere, wasteful; (2) WebSocket: persistent bidirectional connection; on task completion, the backend pushes `{jobId, status}` to the user's session; Spring: `SimpMessagingTemplate.convertAndSendToUser(userId, "/queue/jobs", result)`; (3) SSE: `GET /jobs/abc123/events` → `text/event-stream`; server pushes events to this open connection; simpler than WebSocket for server→client; `SseEmitter emitter = new SseEmitter(); emitter.send(SseEmitter.event().data(result));`; (4) long polling: client sends request, server holds it until the result is ready (or timeout), then responds; (5) webhook: backend POSTs to the client's callback URL when done — for server-to-server flows; backend architecture: worker processes task → writes `job_status=DONE` to DB → publishes to Kafka/Redis pub/sub → notification service picks up → pushes to user's WebSocket session or calls webhook; scale: if multiple backend instances, need a shared pub/sub (Redis Streams/Kafka) so ANY instance that receives the Kafka message can push to the user's WebSocket session on the right instance (or use sticky sessions)*

**321. Explain CORS and common pitfalls.**
   🔑 *CORS (Cross-Origin Resource Sharing) relaxes the browser same-origin policy; server sends `Access-Control-Allow-Origin/Methods/Headers/Credentials`; preflight `OPTIONS` for non-simple requests; Spring `@CrossOrigin`/CorsConfigurationSource — register before the Security filter chain; never `*` with credentials; it's a browser-side control only (Postman/curl bypass it); same-origin policy: browser blocks JS on `app.example.com` from calling `api.example.com` unless the server explicitly allows it; CORS response headers: `Access-Control-Allow-Origin: https://app.example.com` (or `*` for public APIs without credentials); `Access-Control-Allow-Methods: GET,POST,PUT,DELETE`; `Access-Control-Allow-Headers: Authorization,Content-Type`; `Access-Control-Allow-Credentials: true` (only with specific origin, never `*`); `Access-Control-Max-Age: 86400` (cache preflight response for 1 day); preflight: browser sends `OPTIONS /api/users` with `Origin` + `Access-Control-Request-Method` → server responds with CORS headers → if allowed, browser sends the actual request; simple requests (no preflight): GET/POST with `text/plain`/`application/x-www-form-urlencoded`/`multipart/form-data`; Spring CORS config: `@CrossOrigin(origins="https://app.example.com")` on controller; or global: `CorsConfigurationSource` bean in `WebSecurityConfiguration` (must be applied BEFORE Spring Security filters — `http.cors()` in SecurityFilterChain); pitfalls: (1) CORS config not applied before Spring Security → OPTIONS blocked with 401; (2) `*` with `allowCredentials=true` → browser rejects; (3) forgetting to allow `Authorization` header for JWTs; (4) thinking server-side security — CORS is browser enforcement only; (5) configuring CORS in the gateway AND the microservice (redundant, potential conflict)*

**322. What is ResponseEntity and why return a DTO instead of a raw String?**
   🔑 *`ResponseEntity` = full control over status + headers + body (`ok`, `status(CREATED).header().body()`, `noContent`); return a typed DTO → serialized by Jackson (`application/json`), parsable, versionable, structured errors; a raw String → `text/plain`, breaks client parsing + content negotiation*

**323. Which RestTemplate methods have you used, and why does PATCH fail with it?**
   🔑 *`getForObject/Entity`, `postForObject/Entity`, `exchange` (full control + `ParameterizedTypeReference`), `put`, `delete`; PATCH fails with the default `SimpleClientHttpRequestFactory` (HttpURLConnection lacks PATCH) → use `HttpComponentsClientHttpRequestFactory` (Apache HttpClient) or WebClient; RestTemplate is in maintenance mode → prefer WebClient/RestClient; RestTemplate methods: `getForObject(url, ResponseType.class, uriVars)` → deserializes body; `getForEntity(url, ResponseType.class)` → `ResponseEntity` with status+headers+body; `postForObject/postForEntity` → POST with a body; `exchange(url, method, entity, responseType)` → full control (custom headers, any method, `ParameterizedTypeReference` for generic types like `List<User>`); `put(url, body, uriVars)` → fire-and-forget PUT; `delete(url, uriVars)`; PATCH issue: `SimpleClientHttpRequestFactory` uses `java.net.HttpURLConnection` which doesn't support PATCH; fix: `RestTemplate rt = new RestTemplate(new HttpComponentsClientHttpRequestFactory())` (Apache HttpClient 4/5); `rt.patchForObject(url, body, UserDto.class)` works; WebClient alternative: `webClient.patch().uri("/users/1").bodyValue(dto).retrieve().bodyToMono(UserDto.class).block()`; RestTemplate is in maintenance mode since Spring 5 (bug fixes only, no new features) — migrate to `RestClient` (synchronous, Spring 6.1) or `WebClient` (reactive/async); `RestClient` example: `restClient.patch().uri("/users/{id}", id).body(dto).retrieve().body(UserDto.class)` — cleaner API, no PATCH issue*

**324. How do you handle partial failure when a request fans out to multiple downstream services?**
   🔑 *Decide per-call criticality (fail-fast vs degrade); aggregate with `CompletableFuture.allOf`/`Mono.zip` + per-call timeout + circuit breaker; return 207 Multi-Status or a partial payload with an errors block; compensating action/Saga for writes; retry only idempotent calls with backoff + jitter; never leave the client in an unknown state; fan-out pattern: `CompletableFuture<InventoryDto> invFut = inventoryClient.checkAsync(orderId); CompletableFuture<PricingDto> priceFut = pricingClient.getAsync(productId); CompletableFuture.allOf(invFut, priceFut).join();` — parallel calls; per-call timeout: wrap each `CompletableFuture` with `orTimeout(3, SECONDS)` or `completeOnTimeout(defaultValue, 3, SECONDS)`; criticality classification: (1) mandatory (payment authorization) — fail the entire request if it fails; (2) optional (recommendations, enrichment) — degrade gracefully (return null/empty/cached); circuit breaker: Resilience4j `@CircuitBreaker` on each downstream client — fails fast after N consecutive failures, returns fallback; 207 Multi-Status: return `{status:"PARTIAL","results":[{"service":"inventory","status":"OK","data":{...}},{"service":"pricing","status":"ERROR","error":"timeout"}]}` — client knows which parts succeeded; Saga for distributed writes: if order creation succeeds but inventory reservation fails → compensating transaction (cancel the order); idempotent retry: only safe to retry read operations and idempotent writes (PUT/UPSERT); never blindly retry POST payment charges; trace across the fan-out: propagate `traceparent` (W3C Trace Context) to all downstream calls — Zipkin/Jaeger shows the full distributed trace*

**325. How do you design pagination, filtering and sorting for a large collection endpoint?**
   🔑 *Offset = simple + random page access but O(N) scan, unstable under inserts; cursor/keyset = stable, index-friendly, O(1), opaque token of the last sort key, no jump-to-page; whitelist sortable/filterable fields (injection + index coverage), cap page size, return `hasNext` not exact totals; API design: `GET /orders?status=ACTIVE&sort=createdAt,desc&page=0&size=20` (offset) or `GET /orders?cursor=eyJjcmVhdGVkQXQiOi...&size=20` (cursor); offset response: `{"data":[...],"page":{"number":0,"size":20,"totalElements":5432,"totalPages":272}}`; cursor response: `{"data":[...],"nextCursor":"eyJjcmVhdGVkQXQi...","hasNext":true}` — no total count needed; filtering: `?status=ACTIVE&from=2024-01-01&to=2024-12-31` → `Specification` predicates or QueryDSL; whitelist allowed filter fields to prevent SQL injection and ensure index coverage; sorting: `?sort=name,asc&sort=createdAt,desc` (Spring Data handles `Sort` from `Pageable`); ALWAYS whitelist: `Set<String> ALLOWED_SORTS = Set.of("name","createdAt","id"); if (!ALLOWED_SORTS.contains(sortField)) throw new IllegalArgumentException()`; index strategy: ensure the sort + filter combination is indexed (composite index covers the most common query); cap: `Math.min(requestedSize, 100)` — never let the client request unlimited data; Spring Data: `PageRequest.of(page, size, Sort.by(Sort.Direction.fromString(dir), field))` → `Page<Entity>` with content + metadata*

**326. Explain HTTP caching (ETag, Cache-Control, 304).**
   🔑 *`Cache-Control` (max-age, no-store, private/public), `ETag` + `If-None-Match` / `Last-Modified` + `If-Modified-Since` → 304 Not Modified (saves bandwidth); strong vs weak ETag; CDN/reverse-proxy caching; also used for optimistic concurrency (ETag precondition `If-Match` → 412)*

**327. How do you design a consistent error response (RFC 7807)?**
   🔑 *`application/problem+json` (ProblemDetail: type, title, status, detail, instance + custom fields); stable app error codes, no stack traces/internal details, correlation/trace id, field-level validation errors; centralized via `@ControllerAdvice`; same envelope across all endpoints; RFC 7807 Problem Details format: `{"type":"https://example.com/errors/validation","title":"Validation Failed","status":400,"detail":"email must be valid","instance":"/api/users/create","traceId":"abc-123","errors":[{"field":"email","message":"must be a valid email address"}]}`; Spring Boot 3: `ProblemDetail.forStatusAndDetail(HttpStatus.BAD_REQUEST, "Validation failed")` — built-in support; custom extension fields: `problem.setProperty("traceId", MDC.get("traceId")); problem.setProperty("errors", fieldErrors);`; centralized: `@RestControllerAdvice class GlobalExceptionHandler { @ExceptionHandler(MethodArgumentNotValidException.class) ProblemDetail handleValidation(ex) { ... } @ExceptionHandler(EntityNotFoundException.class) ProblemDetail handleNotFound(ex) { return ProblemDetail.forStatus(404); } }`; do NOT expose: stack traces (security info leak), internal DB error messages, ORM-specific exceptions; stable error codes (app-level): `{"errorCode":"USER_NOT_FOUND","message":"..."}` — clients can match on codes without parsing messages; validation errors: gather all field violations from `BindingResult`/`ConstraintViolationException` into the errors array; logging: log the exception with trace id BEFORE returning the problem response so you can correlate; status mapping guide: 400=bad input, 401=not authenticated, 403=forbidden, 404=not found, 409=conflict, 422=semantic validation, 429=rate limited, 500=internal, 503=downstream unavailable*

**328. How do you validate a request body across multiple API versions?**
   🔑 *Version-specific DTOs each with their own Bean Validation annotations (`@NotNull`/`@Valid`/groups), shared base for common fields, validation groups for version-specific rules, `@Validated` on the controller, custom `ConstraintValidator`; map all versions to one domain model and re-validate invariants there; version-specific DTOs: `class CreateUserV1Dto { @NotNull String name; @Email String email; }` and `class CreateUserV2Dto { @NotNull String firstName; @NotNull String lastName; @Email String email; @NotBlank String phone; }` — separate classes per breaking version; shared base: `abstract class BaseUserDto { @Email @NotBlank String email; }` + `class CreateUserV1Dto extends BaseUserDto`; validation groups: `interface OnCreate {}; interface OnUpdate {}; @NotNull(groups=OnCreate.class) String name;` then `@Validated(OnCreate.class) @RequestBody CreateUserDto dto`; custom `ConstraintValidator`: `@Constraint(validatedBy=PhoneValidator.class) @interface ValidPhone {}; class PhoneValidator implements ConstraintValidator<ValidPhone,String> { public boolean isValid(String val, ConstraintValidatorContext ctx) { return val==null || val.matches("\\+\\d{10,15}"); } }`; mapping to domain: `UserCommand cmd = v1Mapper.toDomain(dto)` — validate invariants on `UserCommand` regardless of version (single validation point); cross-field validation: `@AssertTrue(message="end must be after start") boolean isDateRangeValid() { return endDate.isAfter(startDate); }` on the DTO; when validation fails: `MethodArgumentNotValidException` → collect `getFieldErrors()` into a structured error response; `@ExceptionHandler(MethodArgumentNotValidException.class)` in `@RestControllerAdvice`*

**329. APIs work in Postman but fail from the frontend — what's the likely issue?**
   🔑 *CORS preflight failure, missing/incorrect auth header or cookie (SameSite/HttpOnly/`credentials:'include'`), different base URL/port/proxy, HTTPS mixed content, Content-Type mismatch, missing CSRF token, gateway/WAF rule, caching — check browser devtools Network tab + server access logs*

**330. How do you document APIs (OpenAPI/Swagger)?**
   🔑 *OpenAPI 3 spec (springdoc-openapi / Swagger UI), annotate with `@Operation`/`@Schema`, generate contract-first or code-first, versioned docs, examples + error schemas, try-it-out; lock down/secure Swagger UI in prod; can generate clients/server stubs*

**331. Explain HATEOAS — is it practical?**
   🔑 *Hypermedia As The Engine Of Application State — responses embed `_links` (self, next, cancel) so clients navigate by links not hardcoded URLs; Spring HATEOAS, HAL; Richardson level 3; in practice rarely used — clients couple anyway, extra payload, limited tooling*

**332. Explain content negotiation and HttpMessageConverters.**
   🔑 *Client `Accept` header + server `produces`/`Content-Type` select a representation (JSON/XML/CSV); Spring picks an HttpMessageConverter (Jackson for JSON, Jackson-XML, etc.) to (de)serialize the body; `@RequestBody`/`@ResponseBody`; register custom converters for new media types*

**333. How do you design bulk / batch APIs?**
   🔑 *Accept an array/NDJSON payload with a cap on batch size; return 207 Multi-Status or a per-item result list with individual statuses (don't fail the whole batch on one bad item); idempotency key per item; async for large batches (202 + job id + status endpoint); pagination for bulk reads; backpressure + rate limits; transactional-per-item vs all-or-nothing decision stated explicitly; bulk API design: `POST /orders/bulk` with body `{"items":[{...},{...}]}`, cap at 1000 items per request; response: `{"results":[{"index":0,"id":"abc","status":"CREATED"},{"index":1,"status":"ERROR","error":"SKU not found"}],"successCount":1,"failureCount":1}` (207 Multi-Status overall); processing strategy: (1) all-or-nothing (single transaction, all fail if one fails — good for data integrity), (2) per-item (each item has its own transaction — more resilient, client must handle partial success); idempotency: include `idempotencyKey` per item or a batch-level key to prevent duplicate processing on retry; large batches: `POST /reports/bulk` → `202 Accepted {"jobId":"xyz","statusUrl":"/jobs/xyz"}` — async processing via Spring Batch / SQS; NDJSON (newline-delimited JSON): for streaming large inputs `{"id":1,"name":"a"}\n{"id":2,"name":"b"}\n` — lower memory than a large JSON array; rate limiting: cap request rate per client + concurrent batch size; validation: validate ALL items first, return all errors before processing any (fail-fast mode) or process and report per-item*

**334. How do you design a long-running operation over REST?**
   🔑 *Return 202 Accepted + a job id + `Location` of a status resource; client polls the status endpoint (or subscribes via WebSocket/SSE/webhook); persist job state (PENDING/RUNNING/DONE/FAILED); make submission idempotent (idempotency key so retries don't start duplicate jobs); expose progress + result URL; set TTL/cleanup on completed jobs*

**335. How do you handle API security basics at the web layer (summary)?**
   🔑 *HTTPS/TLS, authN via OAuth2/JWT validated in a filter, deny-by-default authZ (URL + method security), input validation (`@Valid`) + output encoding, CORS allowlist, rate limiting, security headers (HSTS/CSP), no secrets in URLs/logs, lock down Swagger/actuator in prod — see the Security section for depth; layer-by-layer checklist: (1) TLS: all traffic over HTTPS, redirect HTTP→HTTPS, HSTS header (`Strict-Transport-Security: max-age=31536000; includeSubDomains`); (2) authentication: JWT/OAuth2 bearer token validated in `OncePerRequestFilter` or Spring Security's `jwtDecoder()` bean; stateless (no session); (3) authorization: deny-by-default `http.authorizeHttpRequests(auth -> auth.anyRequest().authenticated())`; method-level `@PreAuthorize("hasRole('ADMIN')")`; (4) input validation: `@Valid` on request bodies, Bean Validation for field constraints, whitelist sort/filter params to prevent injection; (5) output encoding: Jackson handles JSON; use `@JsonProperty` to control serialization; never serialize passwords/secrets; (6) CORS: explicit `allowedOrigins`, not `*` with credentials; (7) rate limiting: bucket4j/Redis token bucket per user/IP; `429 Too Many Requests` with `Retry-After` header; (8) security headers: `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Content-Security-Policy`; Spring Security auto-applies some; (9) secrets: never in URLs (GET params are logged), never in logs, use Vault/Secrets Manager; (10) Swagger/actuator: restrict in prod (`spring.security.user.name=admin`, or secure via Spring Security); Actuator: expose only `health` publicly, secure `/actuator/**` with RBAC*

[⬆️ Back to top](#contents)

## Reactive Programming — WebFlux & Reactor

**336. Mono vs Flux — what are they and how do they differ?**
   🔑 *Reactor publishers: `Mono<T>` = 0 or 1 element (async single value/empty), `Flux<T>` = 0..N elements (async stream); both lazy (nothing runs until subscribed), support backpressure + operators; `Mono` for a single response, `Flux` for a stream/collection; convert with `Flux.from(mono)` / `mono.flux()` / `flux.collectList()`*

**337. Why is reactive lazy, and what does "nothing happens until you subscribe" mean?**
   🔑 *A pipeline is just a blueprint of operators; the data flows only when `subscribe()` is called (or the framework subscribes, e.g. WebFlux on the HTTP response); so side effects must be inside operators (`flatMap`/`doOnNext`), not before; `block()` subscribes + waits (avoid in reactive code); cold (per-subscriber) vs hot (shared) publishers*

**338. map vs flatMap vs concatMap in Reactor.**
   🔑 *`map` = synchronous 1:1 transform; `flatMap` = async 1:N, subscribes to inner publishers concurrently (order NOT preserved) — for calling another reactive service; `concatMap` = sequential, preserves order; `flatMapSequential` = concurrent but ordered output; `switchMap` = cancel previous on new emission; pick by ordering + concurrency needs*

**339. How does backpressure work in Reactive Streams?**
   🔑 *Consumer signals demand via `request(n)` (Reactive Streams spec: Publisher/Subscriber/Subscription/Processor); if the producer is faster → strategies `onBackpressureBuffer`/`Drop`/`Latest`/`Error`; `limitRate`, bounded buffers; natural fit for streaming; prevents the fast producer from overwhelming a slow consumer without blocking threads*

**340. WebFlux vs Spring MVC — when do you NOT use WebFlux?**
   🔑 *WebFlux = non-blocking Netty + Reactor, few event-loop threads handle high concurrency, needs a fully non-blocking stack (R2DBC, WebClient); avoid when the stack is blocking (JDBC), the team is unfamiliar, debugging/stack traces matter, or load is modest — MVC + virtual threads (Java 21) often gets similar scalability with simpler code*

**341. What is R2DBC and why can't you use JDBC in WebFlux?**
   🔑 *R2DBC = Reactive Relational Database Connectivity (non-blocking DB driver returning Mono/Flux); JDBC is blocking → a blocking call on an event-loop thread stalls many requests (thread starvation); if you must call blocking code in reactive, offload to `Schedulers.boundedElastic()`; R2DBC lacks some JPA features (no lazy loading/relationships) → more manual mapping; JDBC blocking problem: WebFlux runs on a small event-loop thread pool (typically N=CPU cores); JDBC calls use blocking I/O (socket read blocks the OS thread); if all event-loop threads are blocked on JDBC, new incoming requests queue up → effectively no concurrency benefit; R2DBC: `spring-boot-starter-data-r2dbc` + `r2dbc-postgresql` (or r2dbc-mysql); entity: same `@Table`/`@Column` annotations; repository: `interface UserRepository extends ReactiveCrudRepository<User,Long> { Flux<User> findByStatus(String status); }`; `DatabaseClient` for custom queries: `databaseClient.sql("SELECT * FROM users WHERE id=:id").bind("id", id).map(row -> new User(row.get("id", Long.class))).one()`; R2DBC limitations vs JPA: no lazy loading, no `@OneToMany`/`@ManyToOne`, no entity lifecycle events, no second-level cache, manual relationship handling; Spring Data JDBC (not R2DBC) is blocking but simpler than JPA for simple mappings; offloading JDBC to boundedElastic: `Mono.fromCallable(() -> jdbcTemplate.queryForObject(...)).subscribeOn(Schedulers.boundedElastic())` — wraps blocking call in a non-reactive scheduler that can grow; not ideal (still blocks a thread) but a migration bridge*

**342. How do you handle errors in a reactive pipeline?**
   🔑 *`onErrorResume` (fallback publisher), `onErrorReturn` (fallback value), `onErrorMap` (translate exception), `onErrorContinue` (skip bad element — use cautiously), `retryWhen(Retry.backoff(...))`, `timeout`, `doOnError` (side effect); errors are terminal signals that propagate downstream; combine with Resilience4j reactive operators*

**343. What are subscribeOn and publishOn (Schedulers)?**
   🔑 *`subscribeOn` sets the thread for the WHOLE subscription (the source), placement-independent; `publishOn` switches the thread for operators DOWNSTREAM of it; Schedulers = `parallel` (CPU), `boundedElastic` (blocking I/O offload), `single`, `immediate`; by default reactive runs on the subscribing/event-loop thread — don't block it*

**344. How do you combine multiple reactive sources?**
   🔑 *`Mono.zip`/`Flux.zip` (combine latest pairwise, wait for all), `Mono.when` (wait for completion, ignore values), `flatMap` (dependent calls), `merge` (interleave as they arrive), `concat` (sequential), `Flux.combineLatest`; parallel fan-out to services then aggregate; `zipWith`/`zipDelayError`*

**345. How do you test reactive code?**
   🔑 *`StepVerifier.create(flux).expectNext(...).expectComplete().verify()` / `.expectError()`; `StepVerifier.withVirtualTime` for time-based operators (no real waiting); `WebTestClient` for WebFlux endpoints; `expectNextCount`, `thenAwait(Duration)`; avoid `block()` in tests — assert on signals*

**346. What are hot vs cold publishers, and what is the Reactor Context?**
   🔑 *Cold = each subscriber gets its own fresh execution from the start (HTTP call per subscribe); hot = emits regardless of subscribers, late subscribers miss earlier items (`share()`, `Sinks`, `ConnectableFlux`); Reactor Context = immutable key-value propagated UPSTREAM through the chain (replaces ThreadLocal for reactive) — carries security/trace/tenant context (`contextWrite`/`deferContextual`)*

**347. When should you choose reactive programming at all?**
   🔑 *High concurrency with I/O-bound work + a fully non-blocking stack (streaming, SSE, many slow downstream calls, websockets, API gateway); NOT for CPU-bound work, simple CRUD, or blocking dependencies; cost = steeper learning curve, harder debugging, viral (all-or-nothing non-blocking); with Java 21 virtual threads, imperative blocking code often scales comparably with far less complexity*

[⬆️ Back to top](#contents)

## Microservices Architecture & Resilience

**348. What actually justifies splitting a monolith into microservices, and what do you decompose first?**
   🔑 *Triggers = independent scaling, team autonomy (Conway's law), deploy coupling/slow releases, divergent tech needs, fault isolation — NOT "microservices are modern"; strangler-fig a bounded context that is read-heavy, low-coupling, high-change (notifications/reporting/auth) first; split code then data (keep a shared DB behind an API temporarily); measure with DORA metrics; start as a modular monolith*

**349. Microservices vs monolith — pros/cons, and why might you move BACK to a monolith?**
   🔑 *Microservices = independent deploy/scale, autonomy, fault isolation, tech diversity — but distributed complexity, network latency, eventual consistency, hard testing/debugging, ops cost, data duplication; monolith = simple, local calls, single transaction, easy testing — but all-or-nothing scaling, slow releases, large blast radius; go back when over-decomposed ("distributed monolith"), chatty sync calls, small team, Amazon Prime Video case → merge contexts that always change together*

**350. How do you handle distributed transactions across services (Saga — choreography vs orchestration)?**
   🔑 *No 2PC/XA in practice (blocking, poor availability); Saga = sequence of local transactions + compensating actions; choreography (events via Kafka, no central brain, loose coupling, hard to trace) vs orchestration (central orchestrator/state machine — Temporal, Camunda/Zeebe, AWS Step Functions, Netflix Conductor — easier to debug/monitor); compensations must be idempotent + commutative; persist saga state with timeouts; alternative = TCC (Try-Confirm-Cancel)*

**351. How do you revert/compensate an operation when a downstream service fails?**
   🔑 *No distributed rollback → compensating transactions (refund, release reservation, cancel), idempotent compensation handlers, persisted saga state with timeout detection, outbox to guarantee the compensation event is emitted, DLQ + manual remediation queue, and a reconciliation/audit job to catch stragglers*

**352. Explain the Outbox pattern — why is it needed with message publishing?**
   🔑 *Dual-write problem = DB commit succeeds but broker publish fails (or vice versa) → inconsistency; Outbox = write the event into an outbox table in the SAME local transaction, then a relay (polling publisher or CDC/Debezium) publishes to Kafka and marks sent; gives at-least-once + ordering; consumer must dedupe; alternative = transaction-log tailing*

**353. How do service discovery and load balancing work (Eureka/Consul vs Kubernetes-native)?**
   🔑 *Client-side (Eureka + Spring Cloud LoadBalancer): registry, heartbeat/lease, client caches registry + picks instance, self-preservation (AP); Consul adds health checks + KV + DNS (CP via Raft); Kubernetes-native: Service + EndpointSlice, cluster DNS, kube-proxy (iptables/IPVS), readiness probes gate endpoints, headless service for pod IPs; on K8s you usually don't need Eureka*

**354. How do you balance load evenly and keep services stateless?**
   🔑 *LB algorithm (round-robin, least-connections, least-request), client-side LB with instance refresh, K8s Service/mesh LB; avoid sticky sessions (keep services stateless — externalize session to Redis); beware long-lived HTTP/2 connections pinning traffic (periodic reconnect / mesh); Kafka partition assignment for consumers; verify with per-instance RPS metrics; stateless service design: ALL state externalized — session → Redis/JWT, file uploads → S3, job state → DB; any instance can handle any request; enables horizontal scaling and rolling deploys without draining sessions; sticky sessions (IP/cookie affinity): breaks horizontal scaling (instance A might be overloaded, instance B idle); only acceptable for WebSocket sessions with in-memory state (then you need sticky sessions + graceful shutdown); load balancing algorithms: round-robin = simplest, equal distribution if requests are equal duration; least-connections = better for variable-duration requests; least-request (Envoy/Istio) = considers in-flight requests; random + two-choices (power-of-two) = surprisingly effective; K8s Service: kube-proxy uses iptables/IPVS round-robin across pod endpoints (readiness probe gates inclusion); client-side LB (Spring Cloud LoadBalancer): client fetches instance list from Eureka/Consul and picks an instance per request; HTTP/2 multiplexing + long-lived connections: all requests from a client go to the same upstream pod (the connection is sticky) → use a service mesh (Istio/Envoy sidecar) that does per-request load balancing; verify: Micrometer `http.server.requests` per-instance counters, HPA scaling metrics, p95 latency per instance*

**355. What does an API Gateway do, and how does it route to services?**
   🔑 *Single entry point: routing/path rewrite, authn/authz + token validation, rate limiting/quotas, request aggregation/BFF, protocol translation, TLS termination, caching, CORS, canary/traffic-splitting, correlation IDs + tracing; routes = static config (Spring Cloud Gateway predicates: Path/Host/Method/Header) or dynamic from registry/K8s Ingress; reactive non-blocking (Spring Cloud Gateway); keep business logic out and avoid it being a SPOF*

**356. Explain the circuit breaker pattern (Resilience4j vs Hystrix) and how you tune it.**
   🔑 *Prevents cascading failure/resource exhaustion — fail fast instead of piling up threads; states CLOSED → OPEN → HALF_OPEN; Resilience4j = lightweight functional decorators, sliding window (count/time); Hystrix = thread-pool isolation, maintenance mode; tune failureRateThreshold (~50%), slowCallRateThreshold + slowCallDurationThreshold, minimumNumberOfCalls (don't trip on tiny samples), waitDurationInOpenState, permittedCallsInHalfOpen, recordExceptions/ignoreExceptions; Micrometer metrics on state changes*

**357. Explain the full resilience toolkit — timeout, retry, bulkhead, rate limiter, fallback — and their order.**
   🔑 *Timeout/TimeLimiter FIRST (a breaker without a client timeout never counts the hung call), retry with exponential backoff + jitter (idempotent only), circuit breaker, bulkhead (semaphore/thread-pool isolation so one dependency can't starve the rest), rate limiter/load shedding, fallback (cached/default/queue-for-later); Resilience4j order = Retry(CircuitBreaker(RateLimiter(TimeLimiter(Bulkhead(call))))); timeout: `TimeLimiterConfig.custom().timeoutDuration(Duration.ofSeconds(3)).build()` — fail fast; without it, a hung downstream blocks a thread for minutes; retry: `RetryConfig.custom().maxAttempts(3).waitDuration(Duration.ofMillis(100)).retryOnException(ex -> ex instanceof ConnectException).intervalFunction(IntervalFunction.ofExponentialBackoff(100, 2)).build()` — jitter prevents thundering herd; only retry idempotent ops; circuit breaker: CLOSED → count failures; when failureRateThreshold (50%) exceeded → OPEN (fail fast, no calls); after waitDurationInOpenState (60s) → HALF_OPEN (allow N test calls); if they succeed → CLOSED; bulkhead: `BulkheadConfig.custom().maxConcurrentCalls(25).build()` (semaphore) limits concurrent calls to a downstream — prevents one slow dependency from consuming all threads; thread-pool bulkhead for physical thread isolation; rate limiter: `RateLimiterConfig.custom().limitForPeriod(100).limitRefreshPeriod(Duration.ofSeconds(1)).build()` — cap calls per second to protect upstream; fallback: `circuitBreaker.executeSupplier(remoteCall).recover(ex -> cachedValue)` — return stale/default data; order matters: a retry wrapping a circuit breaker = retries won't open the breaker; breaker wrapping retry = retries count toward failure rate; standard: Retry → CircuitBreaker → Timeout → Bulkhead (inner) → call; @Annotations with Resilience4j Spring Boot: `@Retry(name="inventory") @CircuitBreaker(name="inventory", fallbackMethod="fallback") public ItemDto getItem(Long id)`*

**358. How do you handle a slow or failing dependent service without taking yourself down?**
   🔑 *Aggressive timeout so slowness becomes fast failure, slowCallRateThreshold so the breaker opens on latency not just errors, bulkhead to cap concurrent calls, serve cached/last-known-good or degraded response, make the call async (202 + callback/event), define hard vs soft dependencies, retry only idempotent calls, alert on dependency error budget; hard vs soft dependency: hard = without it the feature can't function (payment gateway → fail fast, surface 503); soft = nice-to-have (recommendations → fallback to empty list); never let a soft dependency take you down; timeout strategy: connect timeout (3s) + read timeout (10s) — be MORE aggressive for soft dependencies (1s timeout); circuit breaker: `slowCallDurationThreshold(2000, MILLIS)` + `slowCallRateThreshold(50)` — opens when >50% of calls are slow, even if they don't throw; bulkhead: `maxConcurrentCalls(20)` — queue only 10 additional, reject the rest with 503; fallback hierarchy: (1) in-memory cache (Caffeine), (2) Redis stale cache with TTL extension, (3) default/empty response, (4) feature flag disable; async decoupling: if the dependent service is for non-critical work (notification, analytics, indexing) → publish an event to Kafka instead of a sync HTTP call; consumer processes it when ready; error budget: SLO `99.9%` = 43m downtime/month; if the dependency is already burning the budget → page the dependency team, not just your team; monitoring: Resilience4j Micrometer metrics `resilience4j.circuitbreaker.state` → `open` triggers an alert*

**359. How do microservices communicate — sync vs async, with real examples?**
   🔑 *Sync = REST/gRPC/GraphQL (immediate result, temporal coupling) — payment authorization, login, price/inventory lookup; async = Kafka/RabbitMQ/SQS events or commands (decoupled, buffered, replayable, eventual consistency) — order fulfillment workflow, emails/notifications, invoice/report generation, analytics, cache warming; pick async for workflows, sync for queries*

**360. Event-driven architecture vs request-response — trade-offs.**
   🔑 *Request-response = caller waits, tight temporal coupling, easy to reason about, failure propagates; event-driven = publish facts, consumers react independently, loose coupling, buffering/replay, scales fan-out — but harder debugging, ordering, eventual consistency, duplicate handling; often hybrid*

**361. Design an order + payment + inventory microservice workflow (sync REST vs async Kafka).**
   🔑 *Order service owns the saga: sync REST only for fast validation/reads, async Kafka events for the workflow (OrderCreated → PaymentRequested → PaymentCompleted/Failed → InventoryReserved → OrderConfirmed); outbox for reliable publishing, idempotency keys, compensating events (refund/release stock), state machine on the order, DLQ + retry; sync couples availability+latency, async gives resilience but eventual consistency*

**362. How do you ensure high availability and fault tolerance?**
   🔑 *Redundancy across AZs, stateless services + horizontal autoscaling, health probes, timeouts/retries/circuit breakers/bulkheads, graceful degradation + fallback, async buffering with queues + DLQ, idempotency, DB replicas + failover, rate limiting/load shedding, chaos testing, DR plan (RTO/RPO), blue-green/canary, SLOs + error budgets*

**363. How does one service detect that another has crashed?**
   🔑 *Health checks/heartbeats — K8s liveness/readiness probes remove the pod from Service endpoints, Eureka lease expiry, Consul health check; at call time connection-refused/timeout → circuit breaker opens; plus metric alerts, Kafka consumer-group rebalance, distributed-tracing gaps; K8s probes: liveness probe = is the app alive? (fail → pod restarted); readiness probe = is the app ready to accept traffic? (fail → removed from Service endpoints, no new requests); startup probe = grace period for slow-starting apps; `livenessProbe.httpGet /actuator/health/liveness`, `readinessProbe.httpGet /actuator/health/readiness` (Spring Boot Actuator provides these by default); Eureka: services send heartbeat every 30s; if 3 consecutive heartbeats missed (90s) → Eureka deregisters the instance; self-preservation mode = keeps registrations even when many fail (prevents mass deregistration on network partition); Consul: active health check = Consul polls `GET /health` on the service → deregisters if unhealthy; at call time detection: `ConnectionRefusedException` (port closed) or `SocketTimeoutException` (service hung) → circuit breaker records failure; Kafka consumer group: if a consumer crashes, Kafka detects missed heartbeat (`session.timeout.ms=45000`) or missed poll (`max.poll.interval.ms`) → rebalances partitions to remaining consumers; distributed tracing: missing spans/gaps in Jaeger/Zipkin trace show which service in the chain failed; active health monitoring: `GET /actuator/health` from an external prober (UptimeRobot/Pingdom/K8s cronjob) for external visiblity*

**364. Database-per-service vs shared database — how do you decide?**
   🔑 *Database-per-service is the default — independent schema evolution/scaling/failure isolation, no hidden coupling; shared DB only as a temporary migration step or a tightly-coupled legacy pair (deploy coupling + contention); engine per access pattern (Postgres transactional, Mongo documents, Redis cache, Elasticsearch search); cross-service data via API, events/CDC, or a read-model projection*

**365. What is CQRS, what are its drawbacks, and how do you keep read/write consistent?**
   🔑 *CQRS (Command Query Responsibility Segregation) = separate write model (normalized, transactional) from read model (denormalized projections), often with event sourcing; drawbacks = eventual-consistency lag, duplicate storage, projection rebuild complexity, more moving parts; consistency via read-your-writes routing, version/sequence tokens, optimistic UI, idempotent + ordered projection handlers, lag monitoring, replay to rebuild*

**366. What is event sourcing, and when do you use it?**
   🔑 *Persist state as an append-only sequence of immutable events (not current state); rebuild state by replaying, snapshots for speed; gives full audit/time-travel + natural CQRS source; costs = event schema evolution, replay cost, eventual consistency, steep learning curve; fits ledgers/audit-heavy domains, not simple CRUD*

**367. How do you make consumers idempotent / achieve exactly-once effects across services?**
   🔑 *At-least-once delivery + idempotent consumer = dedupe table/cache keyed by event/business id (unique constraint), conditional UPSERT, state-machine guarded by current status, Redis SETNX pre-check; store processed id transactionally with the business write; outbox on the producer; never rely on broker exactly-once for external side effects*

**368. What is a service mesh and the sidecar pattern?**
   🔑 *Sidecar = helper container in the same pod sharing network/volume (proxy/mTLS via Envoy, log/metric shipping, config sync), language-agnostic, independently upgradable; service mesh (Istio/Linkerd) = control plane + sidecar data plane handling mTLS, traffic shaping, retries/timeouts, canary, observability — moves cross-cutting concerns out of app code; cost = extra resources + latency + operational complexity; sidecar pattern: deploy a proxy container (Envoy/Linkerd2-proxy) alongside each app container in the same K8s pod; they share the same network namespace → all inbound/outbound traffic flows through the proxy; language-agnostic: the proxy handles mTLS, retries, tracing regardless of what language the app is in; independently upgradable: upgrade the sidecar without touching the app; service mesh architecture: data plane = sidecar proxies; control plane (Istio Pilot/istiod) = distributes config (xDS API) to all sidecars; features: mTLS between all services (zero-trust network), traffic management (weighted routing, canary, mirroring), retries/timeouts/circuit breaking in the proxy (offloads from app code), observability (automatic distributed traces, golden signal metrics per service pair); Istio canary: `VirtualService` weights `v1: 90%, v2: 10%` for gradual rollout; cost: sidecar = ~50MB RAM + latency overhead (0.5-5ms per hop); control plane memory; debugging is harder (traffic interception); Linkerd2: simpler, lower resource usage than Istio; eBPF-based mesh (Cilium) avoids sidecar entirely*

**369. How do you implement distributed tracing and correlation across services?**
   🔑 *Propagate a trace id + span id (W3C `traceparent`) via headers; Micrometer Tracing / OpenTelemetry → Zipkin/Jaeger/Tempo; correlation id in logs (MDC) + structured logging; sampling; tie traces + metrics + logs together; essential for debugging latency and partial failures in a request chain; W3C Trace Context: `traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01` (version-traceId-parentSpanId-flags); every service reads this header from the incoming request, uses the trace id for its own spans, and forwards the updated header to downstream calls; Spring Boot: `spring-boot-starter-actuator` + Micrometer Tracing (`micrometer-tracing-bridge-otel`) + OpenTelemetry exporter → Tempo/Jaeger/Zipkin; `@Observed` or `Observation.start()/stop()` around service calls; WebClient/RestTemplate auto-propagate trace headers; Kafka: trace context in message headers (producer → consumer); MDC correlation: Micrometer Tracing sets `traceId` + `spanId` in MDC automatically → `%X{traceId}` in log pattern; structured logs: `{"traceId":"4bf9...","message":"order created","orderId":"123"}` → searchable in Loki/Elasticsearch; sampling: 100% in dev, 1-10% in prod (or head-based with tail-sampling for errors); trace visualization: Jaeger waterfall view shows each span's duration, service, parent-child — identify which service adds latency; tie together: (1) trace id links distributed spans, (2) same trace id in logs finds the log lines, (3) Exemplar links a Prometheus metric to a specific trace*

**370. What is the Anti-Corruption Layer (ACL) pattern?**
   🔑 *A translation layer (DDD) between your bounded context and a legacy/external model so their model doesn't leak into yours; adapter/facade + DTO mapping; isolates change, prevents a bad upstream schema from corrupting your domain; heavily used in strangler-fig migrations; DDD context: two bounded contexts (Order Management + Legacy Inventory) have different domain models; without an ACL, you'd pass `LegacyInventoryItem` objects into your Order domain, coupling your model to the legacy schema; ACL structure: `InventoryClient` interface (your domain's language) + `LegacyInventoryAdapter` (implementation) translates `LegacyInventoryItem` → `StockLevel`; interface: `interface InventoryPort { StockLevel checkStock(ProductId productId); }`; adapter: `class LegacyInventoryAdapter implements InventoryPort { LegacyInventoryClient legacy; StockLevel checkStock(ProductId id) { LegacyItem item = legacy.getItem(id.getValue()); return new StockLevel(item.getQtyOnHand(), item.isAvailable()); } }`; the legacy model's quirks (null qty, weird status codes) are handled inside the adapter — your domain never sees them; use cases: (1) integrating with a legacy monolith during strangler-fig migration, (2) consuming a third-party API with a different domain vocabulary, (3) isolating a bounded context from an upstream that changes frequently; hexagonal architecture: the ACL is the "adapter" layer; ports (interfaces) define your domain's requirements, adapters implement them using external systems; change resilience: when the legacy API changes its field names, only the adapter needs updating*

**371. What is a Backend-For-Frontend (BFF)?**
   🔑 *A dedicated API layer per client type (web/mobile/partner) that aggregates + shapes downstream responses, reduces round trips and over-fetching, owns client-specific logic/auth; avoids a bloated generic gateway; trade-off = more services + some duplication; GraphQL can play the same role (API composition/aggregator pattern)*

**372. Feign client vs RestTemplate vs WebClient.**
   🔑 *Feign = declarative interface client, integrates with discovery/LoadBalancer/Resilience4j, less boilerplate but reflective + blocking; RestTemplate = imperative blocking, maintenance mode; WebClient = non-blocking reactive (also usable blocking), best timeout/streaming control; RestClient = modern fluent blocking; Feign for many simple internal calls, WebClient for reactive/streaming/fine control; Feign: `@FeignClient(name="inventory-service", url="${inventory.url}") interface InventoryClient { @GetMapping("/items/{id}") ItemDto getItem(@PathVariable Long id); @PostMapping("/items") @CircuitBreaker(name="inv") ItemDto createItem(@RequestBody ItemDto dto); }` — Spring Cloud OpenFeign; integrates with Spring Cloud LoadBalancer (service discovery), Resilience4j, request interceptors (add auth headers), encoder/decoder; blocking (wraps the HTTP call in a thread); declarative = less boilerplate for many endpoints; cons = reflection-based, harder to customize per-call timeout; RestTemplate: blocking, imperative; `restTemplate.getForEntity(url, UserDto.class)`; `exchange()` for full control; maintenance mode since Spring 5 — use only for legacy codebases; WebClient: `webClient.get().uri("/items/{id}", id).retrieve().bodyToMono(ItemDto.class).timeout(Duration.ofSeconds(3)).block()` — non-blocking by default, but `.block()` works in MVC; best for: fine-grained timeout, retry, error handling per call, streaming, reactive pipelines; RestClient (Spring 6.1): `restClient.get().uri("/items/{id}", id).retrieve().body(ItemDto.class)` — modern fluent blocking alternative to RestTemplate; HTTP interface (Spring 6): `@HttpExchange interface InventoryClient { @GetExchange("/items/{id}") ItemDto getItem(@PathVariable Long id); }` + `HttpServiceProxyFactory` — similar to Feign but Spring-native; choose: Feign for many typed internal service clients with service discovery; WebClient for reactive stacks + fine control; RestClient for modern blocking calls*

**373. How do you reduce and handle latency in a microservices call chain?**
   🔑 *Sources = extra hops, serialization, N+1 service calls, cold start, GC; fix with parallel/async calls (CompletableFuture/WebClient), caching, batching/aggregation (BFF), gRPC/Protobuf, connection pooling + keep-alive, co-location, per-hop timeout budgets; measure p95/p99 (not averages) with distributed tracing*

**374. How do you handle a large file upload in a service?**
   🔑 *Stream it (don't buffer in memory): multipart streaming, chunked/resumable, `StreamingResponseBody`/`InputStreamResource`; better — pre-signed S3 URLs so the client uploads directly and the service only gets a notification; validate size/content-type, virus scan, async processing via event, backpressure (WebFlux), raise multipart limits deliberately; naive approach (wrong): `@RequestParam("file") MultipartFile file` → Spring buffers the ENTIRE file in memory (temp disk file at `spring.servlet.multipart.location`) → OOM for large files; streaming approach: `@RequestMapping(consumes=MediaType.MULTIPART_FORM_DATA_VALUE) void upload(HttpServletRequest req) { MultipartResolver resolver = ...; MultipartHttpServletRequest multi = resolver.resolveMultipart(req); InputStream is = multi.getFile("file").getInputStream(); s3.upload(is, metadata); }` — stream bytes directly to S3 without buffering; `spring.servlet.multipart.max-file-size=500MB`, `spring.servlet.multipart.max-request-size=500MB`; pre-signed URL pattern (preferred for large files): `POST /uploads/init` → `{"uploadId":"xyz","url":"https://s3.../bucket/key?X-Amz-Signature=..."}` (valid 15 min); client PUTs directly to S3 (bypasses your service); S3 calls your webhook/SNS/SQS with a completion event; your service validates + triggers async processing; benefits: (1) no bandwidth/memory on your service, (2) S3 handles multi-part upload natively, (3) resumable; multipart upload for huge files (>5GB): `TransferManager.upload()` (AWS SDK) splits into parts, uploads in parallel, completes with a multi-part complete call; validation: check MIME type from the file header bytes (not the filename extension which can be spoofed); ClamAV/cloud malware scan before making the file available; async processing via Kafka event (`FileUploadedEvent {bucketKey, userId, mimeType}`) → worker processes; `StreamingResponseBody` for downloads: streams response bytes without buffering*

**375. How do you decide the number of instances of a service?**
   🔑 *Peak RPS ÷ sustainable RPS per instance (from load tests) + headroom for spikes and rolling deploys; minimum 2-3 for HA across AZs; HPA on CPU/memory/custom metrics (RPS, queue lag) with min/max; bounded by DB connection pool, downstream rate limits, Kafka partition count; cost review*

**376. Explain the leader-follower pattern and leader election.**
   🔑 *One leader handles writes/coordination, followers replicate + can serve reads; single ordering point, avoids split-brain, failover via consensus (Raft/ZAB/Paxos) or election (ZooKeeper/etcd, Kafka controller); used in Kafka partitions, DB replication, and singleton scheduled jobs across instances (ShedLock)*

**377. List the key microservice design patterns and the main difficulties.**
   🔑 *Patterns = decomposition (by capability/subdomain, strangler-fig), database-per-service, Saga, CQRS, event sourcing, Outbox/CDC, API Gateway, BFF/Aggregator, service discovery, circuit breaker, bulkhead, retry, sidecar/ambassador, ACL, service mesh, health-check API, externalized config, blue-green/canary; difficulties = distributed debugging/tracing, data consistency, partial failure, testing (contract/e2e), versioning, infra + observability cost, service sprawl, local dev, security surface*

**378. Synchronous vs asynchronous service communication — how do you decide?**
   🔑 *Sync (REST/gRPC) when the caller needs an immediate result and the operation is fast + the dependency is highly available (queries, validation, auth) — but creates temporal coupling + latency addition + cascading failure risk; async (events/commands via Kafka/SQS) for workflows, fan-out, decoupling, buffering, resilience — but eventual consistency + ordering + duplicate handling; decide by: need-answer-now?, coupling tolerance, availability, consistency requirement; often sync for reads, async for state-changing workflows*

**379. How do you manage centralized configuration and dynamic refresh across services?**
   🔑 *Spring Cloud Config Server (Git-backed, per-service + per-profile files) or Consul/etcd KV / K8s ConfigMaps; `@ConfigurationProperties` + `@RefreshScope` + `/actuator/refresh` (Spring Cloud Bus to broadcast); secrets from Vault/Secrets Manager; feature flags (LaunchDarkly/Unleash/Flagsmith) for runtime toggles + progressive rollout + kill switches; version config in Git with review; precedence + environment promotion; Spring Cloud Config Server: Git repo → `application.yml` (all services), `order-service.yml` (service-specific), `order-service-prod.yml` (profile-specific); services fetch their config on startup (`spring.config.import=configserver:http://config-server:8888`); dynamic refresh: `@RefreshScope` on beans that hold dynamic config → `POST /actuator/refresh` on the service reinitializes those beans; Spring Cloud Bus: one refresh event published to RabbitMQ/Kafka → all instances refresh simultaneously; K8s ConfigMaps: `kubectl create configmap app-config --from-file=application.yml`; mounted as env vars or file; `reloader` sidecar or Spring's `@EventListener(RefreshScopeRefreshedEvent.class)` to auto-reload; secrets: never in ConfigMaps (plain text in etcd); use HashiCorp Vault (`spring-cloud-vault-config`) or AWS Secrets Manager (`spring-cloud-aws-secrets-manager-config`) — auto-lease renewal; feature flags: `if (featureToggle.isEnabled("new-checkout", userId)) { ... }` — change behavior at runtime without a deploy; LaunchDarkly = SaaS, client SDK; Unleash = self-hosted; Flagsmith = open-source; use for: kill switch (disable a broken feature instantly), A/B test, percentage rollout, user segment targeting; config precedence: command-line args → env vars → system properties → config server → application.yml → defaults*

**380. How do you handle multi-tenancy in a microservice?**
   🔑 *Isolation models: database-per-tenant (strong isolation, costly), schema-per-tenant, or shared schema + tenant_id column (cheap, needs row-level enforcement); resolve tenant from JWT claim/subdomain/header → propagate via context (ThreadLocal/ScopedValue) → Hibernate filter / RLS (Row-Level Security) / routing datasource; per-tenant rate limits + noisy-neighbour isolation; tenant-aware caching keys; isolation models: (1) database-per-tenant: strongest isolation (no cross-tenant data risk), independent backups/scaling, but expensive for many tenants; (2) schema-per-tenant: same DB, separate schema per tenant (`public.users`, `tenant_a.users`); schema-level isolation, Hibernate multitenancy strategy `SCHEMA`; (3) shared schema + tenant_id: cheapest, all tenants share tables; every query MUST include `WHERE tenant_id=?`; risk of data leak if forgotten; tenant resolution: JWT claim `{"tenantId":"acme","sub":"user123"}` → `TenantContextHolder.setTenantId("acme")` in a filter; subdomain `acme.app.example.com` → extract from `Host` header; custom `X-Tenant-Id` header; Hibernate filter: `@FilterDef(name="tenantFilter", parameters=@ParamDef(name="tenantId",type=String.class)) @Filter(name="tenantFilter", condition="tenant_id = :tenantId")` on entity → `session.enableFilter("tenantFilter").setParameter("tenantId", tenantId)` in a Hibernate interceptor; PostgreSQL RLS: `ALTER TABLE orders ENABLE ROW LEVEL SECURITY; CREATE POLICY tenant_isolation ON orders USING (tenant_id = current_setting('app.tenant_id'))` — DB-level enforcement even if the app forgets; routing datasource: `AbstractRoutingDataSource.determineCurrentLookupKey()` returns tenant id → each tenant gets its own pool; caching: `@Cacheable(key="#root.method.name + ':' + T(com.example.TenantContextHolder).get()")` — prefix cache keys with tenant id*

[⬆️ Back to top](#contents)

## Kafka, Messaging & Event-Driven Architecture

**381. Explain event-driven architecture with Kafka — partitioning, consumer-group rebalancing, and exactly-once vs at-least-once.**
   🔑 *Partition key = entity id (orderId/accountId) for ordering + even spread, avoid hot keys, partitions only grow (adding them breaks key→partition mapping); rebalancing = group coordinator reassigns on join/leave/timeout, cooperative-sticky + static membership (`group.instance.id`) to reduce churn, `max.poll.interval.ms` for slow processing; EOS = idempotent producer + transactions (`read_committed`) within Kafka; across external systems use at-least-once + idempotent consumer*

**382. Explain Kafka core concepts — topic, partition, offset, consumer group, replication factor.**
   🔑 *Topic = named log; partition = ordered, immutable, append-only sequence (unit of parallelism + ordering); offset = per-partition position, committed per consumer group; consumer group = subscribers sharing a subscription, each partition to exactly one member; replication factor = copies across brokers (leader + ISR followers); retention by time/size, log compaction by key*

**383. How does Kafka guarantee message ordering, and how do you preserve it?**
   🔑 *Only within a partition — same key → same partition via `murmur2(key) % partitions`; keep `max.in.flight.requests.per.connection ≤ 5` with idempotence (or 1 without) so retries don't reorder; single consumer per partition; no parallel/async processing inside the consumer unless you key-shard the worker pool; ordering breaks if you add partitions or use retry topics*

**384. At-least-once vs at-most-once vs exactly-once delivery — how do you achieve each?**
   🔑 *At-most-once = commit offset before processing (loss on crash); at-least-once = process then commit (duplicates — common default, needs idempotent consumer); exactly-once = idempotent producer + transactions + `isolation.level=read_committed`, or read-process-write committing offsets inside the producer transaction (`sendOffsetsToTransaction`); EOS is only within Kafka — external side effects still need idempotency*

**385. How do you configure durability (acks, replication, ISR, min.insync.replicas)?**
   🔑 *`acks=all` + `min.insync.replicas=2` (with replication factor 3) = no data loss on single-broker failure; ISR (In-Sync Replicas) = replicas caught up to the leader; `enable.idempotence=true` (default) prevents producer-retry duplicates; `unclean.leader.election=false` prevents electing a stale replica; trade durability vs latency; `acks=0` = fire-and-forget (no ack, fastest, data loss possible); `acks=1` = leader-ack only (leader may fail before replication → data loss); `acks=all` = all ISR replicas ack before returning success (safest); `replication.factor=3` = 1 leader + 2 followers per partition (common for prod); `min.insync.replicas=2` = at least 2 replicas (leader + 1 follower) must be in-sync for a write to succeed; if only 1 ISR (the leader), producer gets `NotEnoughReplicasException` → prevents data loss during failover; ISR: a replica stays in ISR if it's caught up to the leader within `replica.lag.time.max.ms` (10s default); if a follower falls behind (slow/network), it's removed from ISR; `unclean.leader.election.enable=false`: prevents a lagging replica (not in ISR) from being elected leader on leader failure → may cause availability loss but preserves durability; `enable.idempotence=true` (Kafka 3 default): each ProducerRecord gets a sequence number; broker detects duplicates on retry → exactly-once at the producer level; requires `acks=all` + `retries > 0`; latency vs durability: `acks=all` adds ~latency of the slowest ISR follower; use `linger.ms=5` + `batch.size=65536` to batch multiple records and amortize the ack latency*

**386. How do you handle duplicate messages (producer and consumer side)?**
   🔑 *Producer: `enable.idempotence=true` (acks=all, retries) stops broker-side retry duplicates; transactions for read-process-write; Consumer: make it idempotent — dedupe table/cache keyed by event/business id with a unique constraint, conditional UPSERT, state-machine guarded by status, Redis SETNX; store processed id/offset transactionally with the business write*

**387. How do you design retries and a dead-letter queue so failures don't vanish or infinite-loop?**
   🔑 *Classify retryable (transient → backoff) vs non-retryable (deserialization/validation → straight to DLT); bounded in-memory retries (DefaultErrorHandler + ExponentialBackOff), then non-blocking retry topics with increasing delay (`@RetryableTopic`), finally a DLQ topic with original headers (exception, stack, source topic/partition/offset, attempt count); ErrorHandlingDeserializer for poison pills; alert on DLQ depth + lag; provide a replay tool; never swallow or auto-commit past a failure; retry classification: transient (DB connection timeout, downstream 503, optimistic lock) → safe to retry with backoff; non-retryable (deserialization error, validation error, business logic error like "account not found") → skip retries, go to DLT immediately; `DefaultErrorHandler`: `new DefaultErrorHandler(recoverer, new ExponentialBackOff(1000L, 2.0))` — retries in-memory on the same thread, blocking the partition; NOT suitable for slow downstream (blocks the consumer); `@RetryableTopic` (Spring Kafka non-blocking): `@RetryableTopic(attempts=3, backoff=@Backoff(delay=1000,multiplier=2), dltTopicSuffix="-dlt") @KafkaListener(topics="orders")` → creates `orders-retry-1000`, `orders-retry-2000` topics; consumer pauses the main topic and processes via retry topics — doesn't block the partition; `DeadLetterPublishingRecoverer`: after exhausting retries, publishes to `orders-dlt` with headers `kafka_dlt-original-topic`, `kafka_dlt-exception-message`, attempt count; `ErrorHandlingDeserializer`: wraps the real deserializer; if deserialization fails → sends to DLT immediately (don't retry a poison pill); monitoring: Grafana alert on `consumer_lag{topic="orders-dlt"}` > 0; replay tool: reprocess DLT messages after fixing the consumer bug; circuit breaker in the consumer: if the downstream DB is down for all records → pause the listener container, wait for recovery*

**388. Kafka vs RabbitMQ — when to use which?**
   🔑 *Kafka = distributed commit log, pull-based, retention + replay, high throughput, partitioned ordering, consumer groups, stream processing → event sourcing/analytics/fan-out; RabbitMQ = broker with exchanges/queues/bindings, push-based, flexible routing (direct/topic/fanout/headers), per-message ack/priority/TTL/delay, lower per-message latency → task queues + RPC; Kafka messages survive consumption, Rabbit's are deleted*

**389. Explain the partition ↔ consumer ↔ group relationship (e.g. 10 partitions, 20 consumers).**
   🔑 *Within a group each partition goes to exactly ONE consumer, but one consumer can own many partitions; with 20 consumers + 10 partitions, 10 consume and 10 sit idle (hot standbys for failover) — parallelism is capped by partition count; multiple groups each get a full copy (fan-out); increasing partitions changes key→partition mapping and breaks existing per-key ordering*

**390. Explain consumer rebalancing and how to reduce its impact.**
   🔑 *Triggered on join/leave/crash, missed heartbeats, `max.poll.interval.ms` exceeded, or partition-count change; coordinator + leader run the assignor (range/round-robin/sticky/cooperative-sticky); eager = stop-the-world revoke-all, cooperative = incremental; reduce impact with static membership, longer `max.poll.interval`, smaller `max.poll.records`, cooperative-sticky assignor; commit offsets in `onPartitionsRevoked`; rebalance triggers: (1) consumer joins/leaves the group, (2) consumer crashes (missed heartbeats within `session.timeout.ms`), (3) consumer exceeds `max.poll.interval.ms` (processing takes too long → coordinator kicks it out), (4) partition count changes, (5) subscription changes; eager rebalance (default): ALL consumers stop, ALL revoke their partitions, coordinator reassigns, ALL rejoin — stop-the-world pause causes lag spike; cooperative/incremental rebalance (`CooperativeStickyAssignor`): only the partitions that need to move are revoked + reassigned, others keep processing — much less disruptive; static membership: `group.instance.id=pod-1` — a pod that restarts reconnects as the SAME member, keeping its partition assignment; `session.timeout.ms` is longer for static members; best practices: (1) use `CooperativeStickyAssignor`; (2) static membership (`spring.kafka.consumer.group-instance-id=${POD_NAME}`); (3) keep processing fast (tune `max.poll.records`, tune `max.poll.interval.ms`); (4) commit offsets in `ConsumerRebalanceListener.onPartitionsRevoked()` before the rebalance; (5) don't add/remove consumers too frequently*

**391. What are the offset commit strategies?**
   🔑 *`enable.auto.commit` (risky — commits on a timer regardless of processing) vs manual (`enable.auto.commit=false` + ack); commit AFTER processing for at-least-once; Spring AckMode (RECORD/BATCH/MANUAL/MANUAL_IMMEDIATE); `auto.offset.reset` (earliest/latest/none) when no committed offset; store offsets with the business write for exactly-once effect; auto-commit (risky): `enable.auto.commit=true` + `auto.commit.interval.ms=5000` — Kafka commits offsets on a schedule; if the app crashes AFTER the timer fires but BEFORE processing completes → data loss (at-most-once); manual commit strategies: `AckMode.RECORD`: commit after each record is processed (safest, lowest throughput); `AckMode.BATCH`: commit after the whole batch (`max.poll.records`) — better throughput; `AckMode.MANUAL`/`MANUAL_IMMEDIATE`: listener gets `Acknowledgment ack` and calls `ack.acknowledge()` when safe — most control; at-least-once: process then commit → on crash, reprocessed from last committed offset (duplicates possible → idempotent consumer); `auto.offset.reset`: what to do when there's no committed offset for the group: `earliest` = start from the beginning of the topic (good for reprocessing), `latest` = start from now (good for new consumers that only care about new data), `none` = throw exception (explicit); storing offsets with the business write (exactly-once effect): `@Transactional` service that (1) processes the message, (2) updates DB, (3) commits the offset — all in the same local transaction (offset is stored in the DB, not Kafka); OR Kafka transactions (`sendOffsetsToTransaction`) for Kafka-to-Kafka exactly-once; Spring: `spring.kafka.consumer.enable-auto-commit=false` + `spring.kafka.listener.ack-mode=MANUAL_IMMEDIATE`*

**392. How do you schema-evolve Kafka messages with a Schema Registry?**
   🔑 *Avro/Protobuf + Confluent Schema Registry (serializer registers + prefixes payload with a schema id, consumer fetches by id); compatibility BACKWARD (add optional fields with defaults — roll out consumers first) / FORWARD / FULL / TRANSITIVE; never remove/rename/retype a required field; validate compatibility in CI; tolerant reader; subject naming strategy*

**393. How do you troubleshoot a consumer that isn't consuming?**
   🔑 *Check group assignment + lag (`kafka-consumer-groups --describe`), correct topic/partition/bootstrap servers, `auto.offset.reset=latest` skipping old messages, offsets already committed past the data, changed `group.id`, stuck in rebalance, `max.poll.interval.ms` exceeded → kicked out, deserialization exception killing the container (poison pill blocking the partition), ACL/auth failure, broker/leadership connectivity, container not started*

**394. How do you get a response back in Kafka (request/reply), e.g. payment → invoice → generate?**
   🔑 *Kafka is fire-and-forget → `ReplyingKafkaTemplate` with a reply topic + correlation id header, OR model it as a chained event flow (payment-completed → invoice-service → invoice-generated); for the caller return 202 Accepted + job id and push completion via WebSocket/SSE/webhook; a saga/orchestrator holds the state machine; `ReplyingKafkaTemplate`: `RequestReplyFuture<K,V,R> future = replyingKafkaTemplate.sendAndReceive(new ProducerRecord("payments-request", null, request), Duration.ofSeconds(10)); ConsumerRecord<K,R> reply = future.get();` — adds a `KafkaHeaders.REPLY_TOPIC` and `KafkaHeaders.CORRELATION_ID` header to the message; the downstream service reads those headers and publishes its response to the reply topic with the same correlation id; `@KafkaListener(topics="payments-reply") void handleReply(@Header(KafkaHeaders.CORRELATION_ID) byte[] correlationId, PaymentResult result)`; limitations: blocking the caller thread, couples availability of request + reply consumers, not idiomatic Kafka; chained event flow (preferred): `POST /orders` → 202 + `jobId="abc"`; Order service publishes `OrderCreated{id=abc}` → Payment service consumes, publishes `PaymentCompleted{orderId=abc}` → Invoice service consumes, publishes `InvoiceGenerated{orderId=abc,url=...}`; Order service updates job status in DB; user polls `GET /jobs/abc` or receives WebSocket push when `InvoiceGenerated` is consumed; saga orchestrator: Temporal / AWS Step Functions holds the state machine and drives each step*

**395. What are the bare-minimum and production configs for a consumer and producer?**
   🔑 *Consumer min: `bootstrap.servers`, `group.id`, key/value deserializer — prod adds `auto.offset.reset`, `enable.auto.commit=false`, `max.poll.records`, `max.poll.interval.ms`, session/heartbeat timeouts, `isolation.level`, assignment strategy; Producer: `bootstrap.servers`, serializers, `acks`, `retries`, `enable.idempotence`, `linger.ms`, `batch.size`, `compression.type`, `max.in.flight.requests`, `transactional.id`*

**396. Where do you configure topics, partitions and settings?**
   🔑 *Topic creation via AdminClient/CLI/Terraform or Spring `NewTopic` @Bean (disable auto-create in prod); partitions + replication factor at creation; broker defaults in `server.properties`; per-topic overrides (`retention.ms`, `cleanup.policy`, `min.insync.replicas`) via `kafka-configs`; client settings in `application.yml` (`spring.kafka.*`) or Producer/ConsumerFactory maps; Spring `NewTopic` bean: `@Bean NewTopic ordersTopic() { return TopicBuilder.name("orders").partitions(12).replicas(3).config(TopicConfig.RETENTION_MS_CONFIG, "604800000").build(); }` — Spring Kafka creates the topic at startup if it doesn't exist; disable auto-create in prod: `auto.create.topics.enable=false` in broker config (prevents accidental topic creation with default settings); CLI: `kafka-topics.sh --create --topic orders --partitions 12 --replication-factor 3 --bootstrap-server localhost:9092`; Terraform: `confluentcloud_kafka_topic` or MSK topic resource for infrastructure-as-code; per-topic overrides: `kafka-configs.sh --alter --entity-type topics --entity-name orders --add-config retention.ms=604800000,cleanup.policy=delete,min.insync.replicas=2`; broker settings (`server.properties`): `num.partitions=1` (default), `default.replication.factor=1`, `log.retention.hours=168`, `log.segment.bytes=1073741824`; client settings (`application.yml`): `spring.kafka.producer.acks=all`, `spring.kafka.producer.batch-size=65536`, `spring.kafka.consumer.max-poll-records=500`, `spring.kafka.consumer.auto-offset-reset=earliest`; custom `ProducerFactory`/`ConsumerFactory` beans for per-topic settings (e.g. different serializers or transactional producers)*

**397. Explain the Kafka Streams API.**
   🔑 *Stream-processing library (not a cluster) — KStream/KTable/GlobalKTable DSL + Processor API, stateless (map/filter/branch) + stateful (aggregate/count/join), windowing (tumbling/hopping/sliding/session), RocksDB state stores backed by changelog topics, repartition topics on key change, exactly-once-v2, interactive queries, standby replicas; alternative to Flink/Spark for Kafka-native pipelines*

**398. How do you publish a message to a topic, and is send() sync or async?**
   🔑 *`KafkaTemplate.send(topic, key, payload)` / `ProducerRecord` with headers; topic externalized in config, key chosen for partitioning/ordering, headers for correlation/trace/schema version; `send()` is ASYNCHRONOUS — buffers into the RecordAccumulator and returns a Future immediately, batched by `linger.ms`/`batch.size`; `.get()` makes it sync (throughput collapse) or use the callback; prefer transactional outbox publishing for critical events*

**399. How do you republish/replay a message to a topic?**
   🔑 *Re-send the original payload with its key (ordering) to the target/retry topic, carrying forward headers + an attempt counter; use a DLT replay consumer or admin tool; cap attempts to avoid infinite loops; consumers must be idempotent since replay creates duplicates; DLT replay: admin consumer reads from `orders-dlt` → republishes to `orders` with the original key and payload; carry forward original headers (`kafka_dlt-original-topic`, `kafka_dlt-exception-message`) and add `kafka_dlt-replay-attempt` counter; cap at max replay attempts → park in a `orders-parking-lot` for manual investigation; reset consumer offsets: `kafka-consumer-groups.sh --reset-offsets --to-earliest --topic orders --group order-processing-group --execute` — re-read ALL messages from the beginning; careful: all records reprocessed (requires idempotent consumers); reset to a specific offset or timestamp: `--to-offset 1000`, `--to-datetime 2024-01-15T10:00:00.000` — replay from a known good point; `kafka-dump-log.sh` to inspect raw segment files; Kafka Mirror Maker 2: replays topic from one cluster to another; admin tool: Spring `KafkaAdmin` + `ListOffsetsResult` to inspect current position; never re-publish without idempotency guarantees (unique constraint, dedup cache); carry the trace id from the original message through the replay for traceability*

**400. Why do we have partitions, and what are the costs of more partitions?**
   🔑 *Partition = unit of parallelism, ordering, and storage distribution; more partitions → more consumer concurrency + throughput, spread across brokers; key keeps related events ordered; costs = more open file handles/memory, longer leader election + rebalance, higher end-to-end latency, more replication overhead; partition count can only increase*

**401. Explain log compaction and retention.**
   🔑 *`cleanup.policy=delete` = retention by `retention.ms`/`retention.bytes` (event streams); `cleanup.policy=compact` = keep the latest value per key (changelog/snapshot semantics, enables rebuilding state/KTable, tombstone = null value deletes a key); can combine `compact,delete`; tiered storage offloads old segments to object storage; retention (`cleanup.policy=delete`): `retention.ms=604800000` (7 days) — Kafka deletes log segments older than 7 days; `retention.bytes=-1` (unlimited size) or `retention.bytes=1073741824` (cap at 1GB per partition); use for event streams where history beyond a window is not needed; log compaction (`cleanup.policy=compact`): for each partition key, Kafka retains only the LATEST record with that key (deduplication by key); old records with the same key are cleaned up by the log cleaner background thread; use case: user profile topic — only the most recent profile per userId matters; Kafka Streams KTable, consumer restart state rebuild, CDC snapshots; tombstone record: publish a record with the target key and `null` value → signals deletion; compaction removes all records with that key; `min.cleanable.dirty.ratio=0.5` (50% of log must be "dirty" = uncompacted before cleaner runs); `segment.ms` + `segment.bytes` control how often a new segment is created (compaction only applies to closed segments); combine: `cleanup.policy=compact,delete` — compact (keep latest per key) AND delete segments older than retention.ms; tiered storage (Kafka 3.6+): offloads old log segments to S3/GCS/Azure Blob — reduces broker disk cost while keeping historical data accessible; `remote.storage.enable=true` on the broker*

**402. How do you monitor consumer lag and Kafka health?**
   🔑 *Consumer lag = log-end-offset − committed-offset per partition (`kafka-consumer-groups`, Burrow, Kafka Exporter → Prometheus/Grafana); alert on growing lag; monitor under-replicated partitions, ISR shrink, broker disk/CPU, request latency, rebalance frequency; lag spike = slow consumer / poison pill / downstream slowness*

**403. Report generation — sync vs async with Kafka.**
   🔑 *Return 202 Accepted + report id, publish a report-requested event; a worker generates asynchronously (Spring Batch/chunked query), uploads to S3, publishes report-ready with a pre-signed URL; notification service pushes to the user; keep status in a jobs table, retry + DLQ, idempotent by report id; sync only for tiny reports with a hard timeout*

**404. What is KRaft and how does it change Kafka (ZooKeeper removal)?**
   🔑 *KRaft (Kafka Raft) replaces ZooKeeper for metadata/controller management → self-managed quorum, faster failover, higher partition scalability, simpler ops (one system); metadata stored in an internal Kafka log; default in recent Kafka versions; ZooKeeper role (old): Kafka used ZooKeeper as an external distributed coordination system — stored broker registration, topic metadata, partition leadership, consumer group offsets (early versions), controller election; ZooKeeper = separate cluster to operate, separate JVM, another failure mode, limited to ~200,000 partitions; KRaft (Kafka 3.3+ stable): metadata stored in an internal Raft-replicated `__cluster_metadata` topic; a quorum of brokers are designated as "controllers" (odd number: 3 or 5); Raft consensus: leader is the active controller, followers replicate; leader failure → new election (~100ms vs ZK's ~30s); KRaft advantages: (1) simpler ops — one system instead of two; (2) faster controller failover (~100ms → faster partition leader election); (3) higher partition count (millions vs ~200K); (4) unified security model; (5) faster broker startup (metadata fetch from local log not ZK); KRaft migration: Kafka 3.x supports running in ZK or KRaft mode; migrate with `kafka-metadata-migration` tool; Kafka 4.0 removes ZooKeeper entirely; Spring Kafka: no code changes for KRaft — it's a broker-level change, transparent to clients*

**405. How do you configure Spring Kafka listener concurrency and error handling?**
   🔑 *`ConcurrentKafkaListenerContainerFactory` with `concurrency` = number of consumer threads (≤ partitions); manual ack (`AckMode.MANUAL`/`RECORD`) + `enable-auto-commit=false`; `DefaultErrorHandler` with backoff + `addNotRetryableExceptions` → DLT via `DeadLetterPublishingRecoverer`; `ErrorHandlingDeserializer` for poison pills; batch listeners for throughput; pause/resume container for downstream outages; `@RetryableTopic` for non-blocking retries; concurrency: `factory.setConcurrency(3)` → creates 3 `KafkaMessageListenerContainer` instances, each a separate consumer thread sharing the group; max useful concurrency = partition count (extra threads idle); each container gets a subset of partitions; ack mode: `factory.getContainerProperties().setAckMode(AckMode.MANUAL_IMMEDIATE)` + `Acknowledgment ack` in the listener; `ack.acknowledge()` after successful processing; error handling: `DefaultErrorHandler handler = new DefaultErrorHandler(new DeadLetterPublishingRecoverer(template, (r, e) -> new TopicPartition(r.topic() + ".dlt", r.partition())), new ExponentialBackOff(1000L, 2.0)); handler.addNotRetryableExceptions(ValidationException.class, DeserializationException.class); factory.setCommonErrorHandler(handler);`; `ErrorHandlingDeserializer`: `factory.setValueDeserializer(new ErrorHandlingDeserializer<>(new JsonDeserializer<>(OrderEvent.class)))` — wraps the deserializer, on error delivers a `DeserializationException` that the handler routes to DLT; batch listener: `@KafkaListener(topics="orders", batch=true) void handle(List<OrderEvent> events, Acknowledgment ack)` — higher throughput; pause/resume: `KafkaListenerEndpointRegistry.getListenerContainer("ordersListener").pause()` when downstream DB is unhealthy → `resume()` when healthy; `@RetryableTopic`: non-blocking retry via delay topics (doesn't block partition); configure in application.yml: `spring.kafka.listener.concurrency=3`; `spring.kafka.listener.ack-mode=manual`*

**406. What are Kafka Connect and CDC, and when do you use them over custom producers/consumers?**
   🔑 *Kafka Connect = config-driven framework of source/sink connectors (DB→Kafka, Kafka→S3/Elasticsearch) with SMTs (Single Message Transforms), scaling + offset management built in; CDC (Change Data Capture, Debezium) streams DB row changes from the transaction log → Kafka → powers the outbox/replication/search-sync without dual writes; use Connect/CDC for integration + data movement, custom consumers for business logic*

[⬆️ Back to top](#contents)

## Security — AuthN/AuthZ, OWASP, TLS & Secrets

> *Full forms: **JWT** = JSON Web Token · **OAuth** = Open Authorization · **TLS/SSL** = Transport Layer Security / Secure Sockets Layer · **mTLS** = mutual TLS · **CSRF** = Cross-Site Request Forgery · **XSS** = Cross-Site Scripting · **SQLi** = SQL Injection · **OWASP** = Open Worldwide Application Security Project · **PCI-DSS** = Payment Card Industry Data Security Standard · **CSP** = Content Security Policy · **RBAC/ABAC** = Role/Attribute-Based Access Control · **KMS** = Key Management Service · **MFA** = Multi-Factor Authentication.*

**407. Explain the Spring Security filter chain end-to-end for a JWT-based API.**
   🔑 *DelegatingFilterProxy → FilterChainProxy → matching SecurityFilterChain; order = SecurityContextHolderFilter, CorsFilter, CsrfFilter (disabled for stateless), LogoutFilter, your `OncePerRequestFilter` JWT filter (parse Bearer → validate signature/exp/iss/aud → build Authentication with authorities → SecurityContextHolder), BearerTokenAuthenticationFilter (resource server), AnonymousAuthenticationFilter, ExceptionTranslationFilter (401 entry point / 403 access-denied handler), AuthorizationFilter (authorizeHttpRequests rules); `SessionCreationPolicy.STATELESS`; supporting beans AuthenticationManager → AuthenticationProvider → UserDetailsService + PasswordEncoder*

**408. Access token vs refresh token — how do you handle revocation for stateless JWT?**
   🔑 *Access = short-lived (5-15 min), stateless, sent on every call; refresh = long-lived, stored in an HttpOnly+Secure+SameSite cookie, used only at `/token`; revocation = jti denylist in Redis (TTL = remaining exp), token-version/epoch per user bumped on logout/password change, rotate refresh tokens with reuse detection (replay → revoke the family), short access TTL; true instant revocation needs introspection (back to stateful)*

**409. Explain OAuth2 grant types — which to use when, and why is implicit deprecated?**
   🔑 *Authorization Code + PKCE for user-facing web/mobile/SPA; Client Credentials for machine-to-machine (no user); Refresh Token for silent renewal; Device Code for TVs/CLI; Implicit deprecated (token in URL fragment → leaks via history/referrer/logs, no client auth, no refresh); Resource Owner Password deprecated (app sees password, breaks MFA/SSO); Authorization Code + PKCE (Proof Key for Code Exchange): user redirected to IdP → authenticates → IdP returns `code` to redirect URI; app exchanges `code` + `code_verifier` (PKCE) for access + refresh tokens; PKCE prevents authorization code interception (mobile/native apps can't keep client_secret); recommended for ALL user-facing apps (web, SPA, mobile); Client Credentials: `POST /token` with `grant_type=client_credentials&client_id=&client_secret=` → access token for the service itself (no user context); for M2M (microservice A calls B, CI/CD systems, background jobs); use `client_assertion` (JWT signed by client) instead of client_secret for higher security; Refresh Token: short-lived access token (5-15min) + long-lived refresh token (days/weeks, stored in HttpOnly cookie); `POST /token` with `grant_type=refresh_token` → new access token silently; rotate refresh tokens on use; revoke refresh token on logout; Device Code: for input-constrained devices (Smart TV, IoT, CLI): device polls for the access token while the user enters a code at a URL on their phone; Implicit (DEPRECATED): token returned in URL fragment (hash) after redirect — leaks in browser history, Referer header, server logs; no client auth, no refresh token; replaced by Auth Code + PKCE for SPAs; ROPC (Resource Owner Password Credentials): app collects username/password and exchanges directly — bypasses IdP (breaks SSO, MFA, phishing resistance); deprecated*

**410. Explain JWT structure, and how it is verified without a DB call. Is it encrypted?**
   🔑 *`Base64url(header).Base64url(payload).signature`; header = alg + kid, payload = claims (iss, sub, aud, exp, iat, nbf, jti + roles/scopes); verified by recomputing HMAC with the shared secret or checking RSA/EC signature with the public key from the JWKS endpoint (cached by kid) — identity + authorities are inside the signed token; a plain JWS is only ENCODED + signed (readable) → never put secrets/PII in it; validate exp/iss/aud/alg (reject `none` + algorithm confusion); use JWE if you need encryption*

**411. How do you prevent SQL injection, XSS and CSRF — and how does CSRF differ for session vs stateless APIs?**
   🔑 *SQLi → parameterized/bind queries (JPA, JdbcTemplate `?`), never concatenate, whitelist dynamic identifiers (sort/table), least-privilege DB user; XSS → contextual output encoding, Content-Security-Policy, OWASP HTML sanitizer, HttpOnly cookies; CSRF → session apps need a synchronizer token (attacker can't read it, same-origin) + SameSite cookies + Origin/Referer check (browser auto-attaches the session cookie → forged state-changing call); stateless JWT-in-header APIs aren't CSRF-prone (header not auto-sent), but JWT-in-cookie is*

**412. Name the OWASP Top 10 and key mitigations.**
   🔑 *A01 Broken Access Control (deny-by-default, server-side authz, no IDOR), A02 Cryptographic Failures (TLS, strong hashing), A03 Injection (parameterized + encoding), A04 Insecure Design (threat modelling), A05 Security Misconfiguration (hardened defaults, lock down actuator), A06 Vulnerable Components (SCA/Dependabot/OWASP Dependency-Check), A07 Auth Failures (MFA, lockout), A08 Integrity Failures (signed artifacts, safe deserialization), A09 Logging/Monitoring Failures, A10 SSRF (allowlist outbound, block cloud metadata endpoint)*

**413. How do you secure a REST API end-to-end (steps + layers)?**
   🔑 *(1) TLS everywhere + HSTS, (2) choose model (stateless JWT/OAuth2 resource server vs session), (3) IdP (Keycloak/Cognito/Okta) issues tokens via Auth Code+PKCE / client creds, (4) validate token in a filter → SecurityContext, (5) SecurityFilterChain: stateless, CORS allowlist, CSRF off for token APIs, deny-by-default rules, (6) method + object-level authz (roles/scopes), (7) refresh rotation + revocation, (8) rate limiting, input validation, security headers, (9) lock down actuator/Swagger, (10) audit logging + monitoring + dependency scanning, (11) secrets in Vault; layered defence = WAF/DDoS → TLS/mTLS → gateway authn + rate limit → service authz → validation → least privilege*

**414. How do you secure service-to-service communication?**
   🔑 *Defence in depth: mTLS for transport identity (mesh-issued + auto-rotated certs, SPIFFE/SPIRE IDs) + OAuth2 client-credentials or signed service JWTs with audience/scope checks per hop + authorization policy in EACH service (never trust "inside the cluster") + network policies/zero-trust + propagate end-user context in a signed token for downstream authz + short-lived credentials, rotation, audit logging, rate limiting between services*

**415. Explain the complete authentication + authorization flow with class- and method-level access.**
   🔑 *Authentication (who you are) = filter validates credentials/JWT → Authentication with authorities in SecurityContextHolder; authorization (what you may do) = URL rules (`authorizeHttpRequests`) then method security `@PreAuthorize("hasRole('ADMIN') and #id==authentication.principal.id")`/`@PostAuthorize`/`@PostFilter` (`@EnableMethodSecurity`) then domain-object/ownership checks; roles vs authorities/scopes, deny-by-default, 401 unauthenticated vs 403 unauthorized, custom AuthenticationEntryPoint + AccessDeniedHandler*

**416. RBAC vs ABAC — how do you model authorization?**
   🔑 *RBAC (Role-Based) = permissions grouped into roles assigned to users — simple, coarse, can explode with combinations; ABAC (Attribute-Based) = policy over subject/resource/action/environment attributes (e.g. owner, department, time) — fine-grained, dynamic; often hybrid (roles + attribute conditions in `@PreAuthorize` SpEL); externalize complex policy (OPA/Cerbos); always enforce object-level ownership to prevent IDOR*

**417. How do you store passwords and other sensitive data in a database?**
   🔑 *Passwords = HASHED (never encrypted/plaintext) with BCrypt/Argon2id/SCrypt + per-user salt + tuned work factor (Spring Security PasswordEncoder, DelegatingPasswordEncoder for upgrades); other sensitive fields = AES-GCM envelope encryption with KMS/Vault-managed keys + rotation, deterministic/blind index only where you must search, column-level/TDE at rest, mask in logs + lower environments, least-privilege DB access + audit; password hashing: `BCryptPasswordEncoder(12)` — BCrypt has a built-in salt (first 22 chars of hash); cost factor 12 = ~300ms per hash (slow = resistant to brute force); `Argon2PasswordEncoder.defaultsForSpringSecurity_v5_8()` — winner of Password Hashing Competition, configurable memory + parallelism + iterations; `DelegatingPasswordEncoder`: `PasswordEncoderFactories.createDelegatingPasswordEncoder()` — stores algorithm prefix in the hash: `{bcrypt}$2a$10$...`; allows migrating from MD5/SHA1 to BCrypt without forcing all users to reset passwords; verify: `passwordEncoder.matches(rawPassword, storedHash)`; NEVER: store plaintext, store reversibly encrypted (encryption key can be stolen), use MD5/SHA1/SHA256 without salt (rainbow table attack); sensitive data (SSN, credit card last4, PII): AES-256-GCM with per-row encrypted DEK (Data Encryption Key) wrapped by a KMS master key (envelope encryption); key rotation: re-encrypt DEKs with new master key, no need to re-encrypt all data; blind index: `SHA-256(salt + email)` stored as a separate column for equality search — can't reverse-engineer the email; `TDE` (Transparent Data Encryption): at-rest encryption at the storage layer (RDS/Oracle); protects against disk theft but not against a compromised DB user; mask in logs: custom Logback converter; never log PII in stack traces or MDC*

**418. How do you keep PCI-DSS data (card/CVV) out of logs (tokenization/masking)?**
   🔑 *Never log full PAN/CVV — CVV must never be STORED; tokenize via a vault/payment provider (app handles tokens only), mask to first6+last4, custom Logback masking converter + Jackson `@JsonIgnore`/serializer, keep card fields out of `toString` (avoid Lombok `@Data` on card models), scrub MDC + exception messages, field-level/KMS encryption at rest, restrict log access + retention, scan logs in CI, segment to limit PCI scope; PCI-DSS rules: PAN (Primary Account Number = card number) must be masked in display (show first6+last4 only); CVV/CVC: MUST NEVER BE STORED after authorization — not in DB, not in logs, not in temp files; tokenization: replace the real PAN with a token (opaque value from a token vault like Stripe/Braintree) — the app stores and handles only the token; the real PAN never touches your servers (iframe/JS SDK); if you DO process PANs: encrypt in transit (TLS 1.2+), at rest (AES-256), key in HSM/KMS, network segmentation (PCI DMZ), WAF, penetration testing annually; custom Logback masking: `class CardMaskingConverter extends ClassicConverter { public String convert(ILoggingEvent e) { return CARD_PATTERN.matcher(e.getFormattedMessage()).replaceAll("****-****-****-$1"); } }`; Jackson `@JsonIgnore` on CVV field, `@JsonSerialize(using=MaskedSerializer.class)` for PAN; `toString()`: override manually on card objects (never Lombok `@Data`); log scrubbing: test with a regex scanner in CI that fails if real card-number patterns appear in test log output; scope minimization: route card data through a dedicated PCI-scope service — keep all other services PCI-descoped*

**419. How do you implement rate limiting (token bucket vs sliding window), and where?**
   🔑 *Token bucket = burst-friendly (refill rate + capacity); leaky bucket smooths; fixed window = simple but boundary spikes; sliding-window log = accurate but memory-heavy; sliding-window counter = good compromise; enforce at the gateway/edge first (cheapest) + defensively in-app for business quotas; distributed counters in Redis (INCR+EXPIRE / Lua for atomicity), key by API key/user/IP; return 429 + Retry-After + X-RateLimit-* headers; add WAF + load shedding*

**420. How do you manage secrets and centralized configuration across services?**
   🔑 *Non-secrets = Spring Cloud Config (Git-backed, `@RefreshScope`, Spring Cloud Bus), Consul/etcd KV, K8s ConfigMap; secrets = Vault (dynamic DB creds, leases, auto-rotation, transit encryption), AWS Secrets Manager/Parameter Store (IAM/IRSA), K8s Secret (enable encryption at rest + RBAC; base64 ≠ encryption) or External Secrets Operator; inject via env/mounted file, never in git/images/logs, rotate, least privilege, audit, scan repos (gitleaks/trufflehog)*

**421. How do you prevent insecure deserialization in Java?**
   🔑 *Never deserialize untrusted data with native Java serialization; prefer JSON/Protobuf with strict typing; disable Jackson polymorphic typing on untrusted input (no `enableDefaultTyping`/`@JsonTypeInfo` — gadget chains), allowlist deserializable types, use `ObjectInputFilter` (JEP 290 serialFilter), validate after deserialization, patch libraries, avoid gadget-rich libs on the classpath; Java native serialization (`ObjectInputStream.readObject()`): arbitrary code execution — any class on the classpath with a gadget chain can be exploited (Apache Commons Collections, Spring, etc.); attack: attacker sends a serialized payload with a gadget chain → `readObject()` executes arbitrary code; fix: NEVER deserialize untrusted binary data with `ObjectInputStream`; `ObjectInputFilter` (JEP 290): `ObjectInputFilter.Config.setSerialFilter(info -> info.serialClass() != null && ALLOWED_CLASSES.contains(info.serialClass().getName()) ? Status.ALLOWED : Status.REJECTED)` — allowlist approach; Jackson polymorphic typing: `mapper.enableDefaultTyping()` allows the JSON payload to specify the class to instantiate → gadget chain RCE; fix: never call `enableDefaultTyping()` on untrusted input; `@JsonTypeInfo(use=Id.NAME)` with `@JsonSubTypes` = explicit subtypes allowlist is safer but still risky with untrusted input; prefer: `mapper.activateDefaultTypingAsProperty(LaissezFaireSubTypeValidator.instance, ...)` with a custom `PolymorphicTypeValidator` that restricts to known safe types; OGNL injection (Struts CVE-2017-5638): expression language interpreters on untrusted input → remote code execution; Protobuf/Avro: typed, schema-validated, no object graph serialization → safe; after deserialization: `@Valid` on the deserialized object with Bean Validation; dependency scanning: OWASP Dependency-Check catches known vulnerable serialization libraries*

**422. Input validation vs output encoding — what's the difference?**
   🔑 *Input validation = reject/constrain untrusted data at the boundary by type/length/format/range/allowlist (Bean Validation `@Valid`, custom validators) → integrity + business rules; output encoding = escape data for its sink (HTML body/attr, JS, URL, SQL identifier, LDAP, OS command) at render time → injection defence; need BOTH — encoding is context-specific and validation alone can't stop XSS; input validation: at trust boundaries (controller, Kafka consumer, file parser); check type, length, format, range, allowlist (permit known-good, not denylist known-bad — attackers find bypass); Bean Validation: `@NotBlank @Size(max=200) @Email String email`; custom validator: `@Constraint(validatedBy=SafeHtmlValidator.class)` — OWASP AntiSamy/jsoup sanitization; `@Pattern(regexp="[a-zA-Z0-9_]{3,20}")` for usernames; validate ALL inputs (path vars, query params, headers, body, files); output encoding: encode data based on WHERE it's going; HTML body: `HtmlUtils.htmlEscape(userInput)` — encode `<>&"'`; HTML attribute: `data-name="${Encode.forHtmlAttribute(name)}"`; JavaScript context: `Encode.forJavaScript(value)` — encode `"';\\`; URL: `URLEncoder.encode(value, UTF_8)`; SQL: always parameterized queries (no encoding needed if using JdbcTemplate/JPA binding); command injection: never pass user input to `Runtime.exec()` — use ProcessBuilder with arg list (not shell); LDAP: OWASP LDAP injection encoder; both are needed: validation catches business rule violations (email must be valid), encoding prevents injection even if validation misses a character; validate on input, encode on output — defense in depth*

**423. Explain the TLS/SSL handshake and what an SSL certificate is.**
   🔑 *ClientHello (versions, cipher suites, SNI, key share) → ServerHello + certificate chain → client validates chain to a trusted CA + hostname + validity + revocation (OCSP stapling) → key exchange (ECDHE for forward secrecy) → session keys → Finished → symmetric AES-GCM/ChaCha20; TLS 1.3 = 1-RTT (0-RTT resumption), drops RSA key transport + weak ciphers; certificate = X.509 binding a public key to a domain, signed by a CA (subject/SAN, issuer, validity); mTLS adds a client cert; TLS 1.2 handshake (2 RTT): (1) ClientHello: TLS version, cipher suites, random, session id, SNI; (2) ServerHello: chosen cipher suite, random + Certificate (server's X.509 cert) + ServerHelloDone; (3) Client validates cert chain (CA signature, hostname matches SAN, validity period, OCSP/CRL revocation); (4) Key exchange: ECDHE → client generates pre-master secret → both derive symmetric keys; (5) Client Finished (MAC of handshake) + Server Finished; (6) Symmetric encryption begins (AES-128-GCM); TLS 1.3 (1 RTT): ClientHello includes key_share (ephemeral DH key) → ServerHello + key_share → both derive keys immediately; removes RSA key transport, static DH, weak ciphers (RC4, DES); 0-RTT resumption: session ticket from previous connection → send data on first message (replay attack risk); X.509 certificate: `Subject: CN=api.example.com`, `SAN: DNS:api.example.com`, `Issuer: Let's Encrypt`, `Validity: 2024-01-01 to 2024-04-01`, `Public Key: RSA-2048`; signed by CA's private key — client verifies with CA's public key (in OS trust store); mTLS: server also requests a client certificate → both sides authenticated; `server.ssl.client-auth=need` in Spring Boot; cert-manager/Istio handles cert distribution in K8s; forward secrecy: ephemeral DH/ECDHE keys discarded after the session → past sessions can't be decrypted even if the server's private key is later stolen*

**424. How do you implement TLS for API security as a first step?**
   🔑 *TLS 1.2/1.3 only, valid CA cert at the LB/ingress, HTTP→HTTPS redirect, HSTS header, strong cipher suites, disable old protocols, automated renewal (cert-manager/ACM/Let's Encrypt); Spring Boot `server.ssl.*` or offload to ingress; then mTLS for internal hops + cert pinning for high-value mobile clients; Spring Boot TLS: `server.ssl.enabled=true`, `server.ssl.key-store=classpath:keystore.p12`, `server.ssl.key-store-password=${SSL_KEYSTORE_PASSWORD}`, `server.ssl.key-store-type=PKCS12`, `server.port=8443`; REDIRECT HTTP→HTTPS: configure `EmbeddedServletContainerCustomizer` or use `server.ssl.redirect=true` (Spring Boot 2.x); ingress offload (recommended for production): TLS terminated at the K8s Ingress (nginx-ingress / AWS ALB) or load balancer; cert-manager handles certificate issuance (Let's Encrypt/ACME) and renewal; `Certificate` CRD + `ClusterIssuer` in K8s; ACM (AWS Certificate Manager): auto-renews, attached to ALB; cipher suites: `server.ssl.ciphers=TLS_AES_128_GCM_SHA256,TLS_AES_256_GCM_SHA384,TLS_CHACHA20_POLY1305_SHA256` (TLS 1.3 suites); disable TLS 1.0/1.1: `server.ssl.protocol=TLS`, `server.ssl.enabled-protocols=TLSv1.3,TLSv1.2`; HSTS: `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload` — browser always uses HTTPS for 1 year; mTLS for internal: Istio service mesh auto-issues and rotates certs for pod-to-pod mTLS; cert pinning (mobile): app hardcodes the expected server cert/public key hash — rejects MitM certs even from trusted CAs; risk: pinned cert expiry breaks the app (implement backup pins)*

**425. What security headers should an API/app set?**
   🔑 *HSTS (Strict-Transport-Security), Content-Security-Policy (XSS), X-Content-Type-Options: nosniff, X-Frame-Options/`frame-ancestors` (clickjacking), Referrer-Policy, Permissions-Policy, Cache-Control for sensitive responses; set via Spring Security `headers()` or the gateway; remove Server/version banners*

**426. Coding: generate and validate a JWT with jjwt.**
   🔑 *`Jwts.builder().subject().claim("roles",...).issuedAt().expiration().signWith(key, HS256/RS256).compact()`; validate with `Jwts.parser().verifyWith(key).requireIssuer().requireAudience().clockSkewSeconds(30).build().parseSignedClaims(token)` → catch ExpiredJwtException/SignatureException/MalformedJwtException; key from `Keys.hmacShaKeyFor(≥256-bit secret)` or RSA keypair; secret from Vault/env, not code; generate: `SecretKey key = Keys.hmacShaKeyFor(Decoders.BASE64.decode(secretBase64)); String token = Jwts.builder().subject(userId).claim("roles", List.of("ADMIN","USER")).issuedAt(new Date()).expiration(new Date(System.currentTimeMillis() + 15_60_000)).issuer("https://auth.example.com").audience().add("api.example.com").and().signWith(key).compact();`; RS256 (preferred for multi-service): `KeyPair kp = Keys.keyPairFor(SignatureAlgorithm.RS256); String token = Jwts.builder()...signWith(kp.getPrivate()).compact(); Jwts.parser().verifyWith(kp.getPublic())...` — services validate with the public key (no shared secret); validate: `try { Claims claims = Jwts.parser().verifyWith(key).requireIssuer("https://auth.example.com").clockSkewSeconds(30).build().parseSignedClaims(token).getPayload(); String userId = claims.getSubject(); List<String> roles = claims.get("roles", List.class); } catch (ExpiredJwtException e) { throw new AuthException("Token expired"); } catch (SignatureException e) { throw new AuthException("Invalid signature"); } catch (MalformedJwtException e) { throw new AuthException("Malformed token"); }`; always validate: signature, expiration (`exp`), issuer (`iss`), audience (`aud`); reject `alg=none` (JJWT 0.12+ does this automatically); key management: store the signing secret in Vault/AWS Secrets Manager, inject via `@Value("${jwt.secret}")`; rotate: add new key to JWKS, keep old key for validation during rollover, then remove; `kid` header identifies which key to use*

**427. What alternatives to JWT exist, and why choose JWT?**
   🔑 *Alternatives = opaque reference tokens + introspection (instant revocation, server-side state), server sessions + cookie (simple, revocable, needs shared/sticky store), PASETO (safer defaults), SAML assertions, API keys, mTLS certs; JWT wins for stateless horizontal scaling, cross-service trust without a shared session store, self-contained claims, standard tooling — at the cost of hard revocation + size*

**428. Explain session management and session fixation/hijacking defences.**
   🔑 *Server session + cookie; regenerate the session id on login (fixation defence — Spring `sessionManagement().sessionFixation().newSession()`), HttpOnly + Secure + SameSite cookies, idle + absolute timeout, bind to device/IP cautiously, concurrent-session control, invalidate on logout, store sessions in Redis for scale-out; stateless JWT avoids server sessions but loses easy revocation*

**429. What SEI CERT / secure-coding practices do you apply?**
   🔑 *Validate input at trust boundaries, defensive copies (no mutable internal state leak), immutable classes + private final fields, avoid native serialization of sensitive classes, don't log secrets, SecureRandom (not Random) for security values, constant-time comparison for secrets/MACs, try-with-resources, avoid TOCTOU races, don't catch/ swallow Throwable, sanitize format strings + command inputs; IDS01-J: normalize strings before validation (Unicode normalization before regex checks); IDS03-J: never concatenate user input into OS commands — use `ProcessBuilder(List.of("cmd","--arg", userInput))`, not shell string; IDS07-J: never pass user input to `Runtime.exec(String)` — one-arg form invokes a shell; MSC02-J: use `SecureRandom` for security-sensitive tokens, nonces, IDs — `new SecureRandom().nextBytes(32)`; `Random` is predictable; MSC61-J: don't use insecure or outdated cryptographic algorithms — no MD5/SHA1 for hashing passwords, no ECB mode, no DES; EXP50-J: don't confuse abstract object identity with value equality — `==` on `Long`/`Integer` objects; OBJ01-J: never return references to mutable internal state without defensive copy — `return Collections.unmodifiableList(this.items)` or `return List.copyOf(this.items)`; immutable classes: `private final` fields, no setters, defensive copy in constructor; constant-time comparison: `MessageDigest.isEqual(expected, actual)` — standard `equals` short-circuits, leaking timing info for MAC comparison; TOCTOU: time-of-check-time-of-use — check if file is writable, then write → race condition; use atomic operations (`Files.move`, DB transactions); EXC06-J: don't suppress exceptions without handling — `catch (Exception e) {}` = swallow; at minimum log + rethrow; try-with-resources: `try (Connection c = ds.getConnection(); PreparedStatement ps = c.prepareStatement(...)) { ... }` — guarantees close even on exception*

**430. How do you implement multi-factor authentication (MFA) and strong auth?**
   🔑 *Something you know + have + are; TOTP (RFC 6238 — authenticator apps), WebAuthn/FIDO2 passkeys (phishing-resistant), SMS/email OTP (weakest), push approval; delegate to the IdP (Keycloak/Cognito/Okta); step-up auth for sensitive actions, account lockout + anomaly detection, backup codes; never roll your own crypto; TOTP (Time-based OTP, RFC 6238): shared secret (Base32) stored at enrollment → authenticator app (Google Authenticator, Authy) generates `HOTP(secret, floor(time/30))` = 6-digit code; server regenerates the same code on validation (±1 time window for clock skew); never reuse a TOTP code (mark as used); `com.warrenstrange:googleauth` or Bouncy Castle for implementation; WebAuthn/FIDO2 passkeys: public-key cryptography; device holds the private key (in secure enclave), server holds the public key; no shared secret → phishing-resistant (challenge is origin-bound); Android/iOS/Windows Hello support; Spring Security WebAuthn support or use Keycloak's built-in; SMS/email OTP: lowest security (SIM swapping, email account compromise, no phishing resistance) — use only as fallback; push notification MFA (Duo/Okta Verify): user taps "approve" on their phone → better UX than TOTP; step-up authentication: standard login with password; for sensitive actions (transfer money, change password, delete account) → re-challenge with MFA; Keycloak: `required_action: CONFIGURE_TOTP`; account lockout: `n` failed attempts → temporary lockout (incremental: 30s, 5min, 1hr) → notify via email → require identity verification to unlock; backup codes: 10 one-time codes generated at MFA enrollment, stored as BCrypt hashes; `acr_values` in OIDC for step-up: IdP re-authenticates with higher assurance level*

**431. How do you handle SSRF and outbound request safety?**
   🔑 *SSRF (Server-Side Request Forgery) = app fetches an attacker-controlled URL → block private/link-local ranges + cloud metadata endpoint (169.254.169.254), allowlist destinations, resolve + validate DNS (rebinding), disable redirects to internal hosts, no raw user URLs to internal services, egress network policies, use IMDSv2 on AWS*

**432. OAuth2 vs OpenID Connect (OIDC) — what's the difference?**
   🔑 *OAuth2 = authorization framework (delegated access via access tokens + scopes — "what you can do"); OIDC = an identity layer ON TOP of OAuth2 (adds the id_token = a JWT with authenticated-user claims — "who you are") + the `/userinfo` + discovery (`.well-known/openid-configuration`) + JWKS; use OIDC (Keycloak/Cognito/Okta/Auth0) for login/SSO, OAuth2 scopes for API authorization; access_token for calling APIs, id_token for the client's identity*

**433. How do you rotate secrets and manage certificates without downtime?**
   🔑 *Overlapping validity (issue new before revoking old), dual-accept window; Vault dynamic secrets with short leases + auto-renew; versioned keys with a `kid` so tokens signed by old + new keys both validate during rollout (JWKS lists both); automated cert rotation (cert-manager/ACM, mTLS mesh rotates sidecar certs); rolling restart or hot-reload config; audit + alert on expiry; never hardcode — pull at startup/refresh*

[⬆️ Back to top](#contents)

## Performance, Caching & Scalability

**434. How do you find and fix a slow endpoint / performance bottleneck in production?**
   🔑 *APM (New Relic/Dynatrace/Datadog) for p95/p99 + trace waterfall → which service/call is slow; async-profiler/JProfiler flame graph for CPU hotspots; GC logs (`-Xlog:gc`) for pause time; thread dump (jstack) for BLOCKED/WAITING; heap dump (MAT) for memory; SQL slow-query log + EXPLAIN for N+1/missing index; HikariCP pending-connections metric for pool waits; fix in order: algorithm → DB index → caching → async → pool/JVM tuning; profile before optimizing; diagnostic workflow: (1) APM / distributed trace: identify which service/endpoint is the outlier, and within the trace which DB call/HTTP call is slowest; (2) p95/p99 latency (not averages — a slow p99 affects 1% of users but is usually the bottleneck to fix); (3) async-profiler: `./profiler.sh -d 60 -f flamegraph.html <pid>` — flame graph shows hot methods in CPU samples; (4) GC: `-Xlog:gc*:file=gc.log:time,uptime:filecount=5,filesize=20m`; look for long STW pauses (>200ms G1 full GC, >50ms ZGC); (5) thread dump: `jcmd <pid> Thread.print` or `jstack <pid>` — look for BLOCKED threads (waiting on a lock or connection pool), WAITING threads stuck on I/O; (6) heap dump: `-XX:+HeapDumpOnOutOfMemoryError`; Eclipse MAT → Leak Suspects + Dominator Tree; (7) DB: `pg_stat_statements` for slow queries, `auto_explain.log_min_duration=1000` for plans; N+1 = many identical small queries; add composite index or JOIN FETCH; (8) HikariCP metric `hikaricp.connections.pending` > 0 = pool exhausted → increase pool size or reduce transaction duration; fix order: algorithm (O complexity) → DB index → caching → async → pool → JVM; never optimize without profiling first*

**435. Explain caching strategies — cache-aside vs write-through vs write-behind, local vs distributed.**
   🔑 *Cache-aside (lazy, most common) = app reads cache, miss → DB → populate; write-through = synchronous write to cache + DB (consistent, slower writes); write-behind = async flush to DB (fast, risk of loss); local Caffeine = ultra-low latency per-pod but cache drift across pods; Redis = shared distributed cache; never cache hot-mutating financial state without strong invalidation; cache-aside (lazy loading): read: `value = cache.get(key); if (value==null) { value = db.get(key); cache.set(key, value, TTL); } return value;`; write: update the DB, then evict the cache key (`@CacheEvict`) or update it (`@CachePut`); most flexible, risk of stale data between DB write and cache evict; write-through: on EVERY write → write to cache + DB synchronously; always consistent (no stale cache), but slower writes (cache + DB both); good for read-heavy data that must be always fresh; write-behind (write-back): write to cache immediately, flush to DB asynchronously after a delay; fastest writes (just cache), risk of data loss if cache crashes before DB flush; good for logging/analytics where eventual persistence is acceptable; read-through: cache pulls from DB on miss automatically (vs cache-aside where app code does it); Caffeine (local in-process): nanosecond latency, TinyLFU eviction, bounded by heap; no network; BUT: each pod has its own cache → pods diverge after a write; invalidation events required (Kafka/Redis pub/sub to tell other pods to evict); Redis (distributed): all pods share one view; ~1ms latency; persist (RDB/AOF); Sentinel/Cluster for HA; eviction policies; multi-layer: Caffeine (L1, <1ms) in front of Redis (L2, ~1ms) in front of DB (~5-50ms); Spring: `CaffeineCacheManager` or `RedisCacheManager`; `@Cacheable("users")`, `@CacheEvict(value="users", key="#id")`, `@CachePut(value="users", key="#user.id")`*

**436. How do you handle cache invalidation and cache stampede?**
   🔑 *Invalidation = TTL, event-driven eviction (`@CacheEvict` / publish eviction event), version/tag key (cache busting on schema change); stampede (thundering herd on expiry) = probabilistic early expiry (stale-while-revalidate), per-key mutex/lock on recompute, request coalescing/single-flight, background refresh before TTL; staggered TTLs to avoid synchronized expiry*

**437. How does caching work internally (Caffeine, Redis)?**
   🔑 *Caffeine (in-process) = hash map + Window-TinyLFU frequency sketch for admission/eviction, O(1); Redis = single-threaded event loop (atomic commands), per-type encodings (embstr/ziplist/quicklist/skiplist/hashtable), expiry via lazy + active sweep, eviction policies (LRU/LFU/volatile-ttl), persistence RDB snapshot + AOF; Spring `@Cacheable` = AOP proxy checks CacheManager before invoking the method; Caffeine internals: `ConcurrentHashMap` for key-value storage; Window-TinyLFU admission policy uses a frequency sketch (count-min sketch) to estimate access frequency — only frequently-accessed items are admitted to the main cache, preventing cache pollution by one-time scans; LRU approximation via a linked list; lock-free design for low contention; `Caffeine.newBuilder().maximumSize(10_000).expireAfterWrite(5, MINUTES).recordStats().build()`; `cache.stats()` → hit rate, eviction count; Redis internals: single-threaded event loop (one CPU core for commands) → atomic without locks; data structures: String (embstr ≤44 bytes, raw), List (quicklist = linked list of ziplist), Hash (ziplist for small hashes, hashtable for large), Set (intset for integers, hashtable for strings), Sorted Set (ziplist for small, skiplist+hashtable for large); expiry: lazy expiry (checked on access) + active sweep (randomly samples keys and deletes expired); eviction policies: `allkeys-lru` (evict any key LRU), `allkeys-lfu` (evict by frequency), `volatile-lru` (only keys with TTL), `noeviction` (return error); persistence: `RDB` (point-in-time snapshot, fast restart, some data loss) + `AOF` (append-only log of commands, near-zero loss, larger file); `appendonly yes` + `appendfsync everysec`; Redis 7: multi-threaded I/O (network) but command execution still single-threaded; Spring @Cacheable: `@Cacheable("users")` → CGLIB/JDK proxy intercepts the method call → checks `CacheManager.getCache("users").get(key)` → on miss, calls the real method + `put(key, result)` → returns cached result on hit; key generation: default = method params hash, custom: `@Cacheable(key="#id")` SpEL*

**438. How do you configure TTL, cache keys and versioned invalidation?**
   🔑 *TTL per entity type (too short = miss storm, too long = stale); key = collision-free `entity:id:version`; version/tag suffix bumped on data/schema change for bulk invalidation; per-region CacheManager beans (`@Cacheable(cacheManager=...)`); negative caching for not-found; size + TTL bound to prevent OOM*

**439. What are Redis's main use cases?**
   🔑 *Cache (GET/SET + TTL), session store (shared across pods), distributed lock (`SET key uuid NX PX` + Lua release — Redisson RLock), rate limiting (INCR+EXPIRE / sorted set sliding window), pub/sub, Streams, leaderboards (sorted set), geo (GEOADD/GEORADIUS); eviction LRU/LFU; HA via Sentinel/Cluster*

**440. Coding: implement a Redis-based distributed lock (and cache-aside).**
   🔑 *Lock: `SET key UUID NX PX 5000` → OK if acquired; release with a Lua script (GET→compare UUID→DEL atomically) to avoid releasing someone else's lock; Redisson RLock (with watchdog lease renewal) in production; cache-aside: `ops.get(key)` → miss → fetch DB → `ops.set(key, value, ttl)`; set a fencing token for correctness-critical locks*

**441. How do you design for a 10x traffic spike or very high throughput (100k+ RPS)?**
   🔑 *Horizontal autoscaling (HPA/KEDA) of stateless pods, CDN for static + cacheable responses, pre-warm caches, Redis for hot lookups, DB read replicas + tuned pool (PgBouncer), buffer burst writes via Kafka/SQS, async non-blocking I/O (WebFlux/virtual threads) to avoid thread-per-request blocking, rate limiting + circuit breakers to shed load, partition/shard data, multi-region active-active; load test (Gatling) to find the saturation point first*

**442. When traffic is extreme, what do you prioritize — traffic, async, pool, locking, consistency, queries?**
   🔑 *Priority: (1) traffic handling (LB, autoscaling, CDN), (2) async decoupling of the write path (Kafka), (3) caching to eliminate hot DB reads, (4) query + index optimization, (5) connection-pool sizing, (6) minimize locking (optimistic > pessimistic, no distributed lock on hot path), (7) relax consistency to eventual where safe; measure p99, not averages*

**443. How do you tune a HikariCP connection pool and diagnose exhaustion?**
   🔑 *Settings = maximumPoolSize (small: ~cores×2, NOT hundreds), minimumIdle, connectionTimeout, idleTimeout, maxLifetime (< DB-side timeout); metrics = `hikaricp.connections.pending/active/idle`; exhaustion symptom = "HikariPool timed out waiting for connection"; causes = long transactions, N+1, slow queries holding connections, OSIV, leaks (unclosed); fix = shorten transactions, add index, go async, size correctly*

**444. How do you do capacity planning and load testing before a release?**
   🔑 *Define SLAs (e.g. p99 < 200ms @ 1000 RPS, error < 0.1%); JMeter/Gatling/k6 scenarios = ramp-up → steady-state → spike → soak; measure throughput + latency percentiles + error rate + resource saturation; gate in CI; plan headroom above peak + for a rolling deploy + one-AZ loss; chaos testing for failure modes; capacity planning: (1) estimate traffic: DAU × actions/day ÷ 86400 = avg RPS; peak ≈ 3-5× average; (2) estimate per-instance throughput from load tests; (3) min instances = peak RPS ÷ per-instance RPS + 30% headroom; (4) factor in: rolling deploy (N-1 instances available), AZ failure (N-1 AZs), GC pauses; (5) DB connections: instances × HikariCP pool size ≤ DB max_connections - PgBouncer reserve; load test scenarios with Gatling DSL: `setUp(scenario.inject(rampUsers(100).during(60.seconds), constantUsersPerSec(200).during(300.seconds), atOnceUsers(500)).protocols(httpProtocol)).assertions(global.responseTime.percentile(99).lt(200), global.failedRequests.percent.lt(1))`; test types: smoke test (1-5 users, CI gate), load test (expected peak, 30min), stress test (find saturation point — double until errors appear), soak test (moderate load 24h — catches memory leaks/connection leaks), spike test (sudden 10x burst); metrics to watch during test: p50/p95/p99/p99.9, error rate, CPU/memory, GC pause time, DB connection pool pending, Kafka lag; chaos testing: kill a pod, fail a DB, simulate network partition — verify circuit breaker + fallback + auto-recovery*

**445. Horizontal vs vertical scaling, and the layers of caching.**
   🔑 *Vertical = bigger box (simple, limited, SPOF, costly); horizontal = more stateless instances behind an LB (elastic, needs externalized state); caching layers = browser → CDN/edge → API/app (Caffeine/Redis) → DB (query/buffer/materialized view); push work to the cheapest cacheable layer; read/write splitting via replicas*

**446. How do you optimize Java code in hot paths?**
   🔑 *Algorithm first (reduce Big-O), profile before optimizing (no premature optimization); StringBuilder over `+` in loops, batch DB calls (`findAllById` vs N×`findById`), lazy/short-circuit streams, primitives over boxed, avoid allocation in hot loops, cache expensive computations, async for I/O-bound; measure with JMH microbenchmarks; identify the hot path: async-profiler flame graph or JProfiler → find the methods with highest cumulative CPU time; never optimize without profiling data (Knuth: premature optimization is the root of all evil); algorithm complexity: O(n²) → O(n log n) or O(n); if processing 10k items, n² = 100M ops, n log n = ~130k ops; String concatenation in loops: `"a" + "b" + "c"` in a loop = O(n²) due to StringBuilder creation per operation; `StringBuilder sb = new StringBuilder(); for (...) sb.append(x); return sb.toString();` = O(n); batch DB: `userRepository.findAllById(ids)` = 1 SQL with IN clause; vs `ids.stream().map(id -> repo.findById(id).orElseThrow())` = N SQL queries; stream optimization: `list.stream().filter(x -> x.isActive()).findFirst()` short-circuits — no `toList()` needed; `.parallelStream()` for CPU-bound ops on large collections; primitives vs boxed: `int` over `Integer` in hot loops (no autoboxing/unboxing, no object allocation); `int[]` over `List<Integer>`; avoid allocation: object pooling (e.g. `ObjectPool<ByteBuffer>`), reuse arrays; Unsafe.allocateMemory for off-heap; cache expensive computations: `Map.computeIfAbsent(key, k -> expensive())` in-memory cache; `@Cacheable` for service-level; JMH (Java Microbenchmark Harness): `@Benchmark public void myMethod(Blackhole bh) { bh.consume(processItem(item)); }` — warm up JIT, steady-state measurement, prevents dead-code elimination; avoid: false sharing (CPU cache line conflicts in `volatile` adjacent fields), lock contention (`synchronized` on shared objects), blocking I/O on hot thread*

**447. What is backpressure and how do you apply load shedding?**
   🔑 *Backpressure = consumer signals it can't keep up so the producer slows (Reactive Streams `request(n)`, bounded queues, Kafka lag); load shedding = reject/queue excess early (429, bounded executor + CallerRuns, admission control) to protect the system; graceful degradation (serve cached/default); Little's law (L = λ×W) to reason about queue length vs latency*

**448. How do you optimize application/JVM startup time (summary)?**
   🔑 *See JVM (CDS/AppCDS, AOT, GraalVM native image, CRaC checkpoint/restore, tiered compilation) and Spring (lazy init, fewer scans, AOT); dev-only `-XX:TieredStopAtLevel=1`; trade-off = startup speed vs peak throughput/warmup; techniques ranked by effort: (1) lazy bean init (`spring.main.lazy-initialization=true`): ~30-50% startup reduction; beans created on first use — first request slower; (2) fewer classpath scans: `spring-context-indexer` (apt plugin generates `spring.components` index at build time, skips runtime classpath scan); (3) `spring-boot-devtools`: fast restart (reloads only app classes, not deps); dev only; (4) CDS (Class Data Sharing): `java -Xshare:dump` → shared archive → reused across restarts; AppCDS: include app classes; (5) tiered compilation dev shortcut: `-XX:TieredStopAtLevel=1` skips C2 compiler → faster startup, much lower peak throughput (dev only!); (6) AOT (Spring Boot 3 ahead-of-time): pre-computes bean definitions at build time; no reflection/classpath scan at runtime; (7) GraalVM native image: compile to native binary; 10-100ms startup; 5-10× lower memory; no JIT warmup; longer build time; restrictions on reflection; (8) CRaC (OpenJDK Coordinated Restore at Checkpoint): snapshot a warm running JVM → restore in <100ms; no native image restrictions; AWS Lambda SnapStart uses CRaC; trade-off: startup speed vs peak throughput (native image has no JIT → lower peak throughput on CPU-intensive code vs a warmed-up JVM); serverless = favor native/CRaC; long-running services = JVM with AOT for moderate improvement*

**449. What is APM (Application Performance Monitoring) and how do you use it?**
   🔑 *APM (New Relic/Dynatrace/Datadog/AppDynastics/Elastic APM) auto-instruments the app → distributed traces (span waterfall per request), service maps/dependencies, latency percentiles (p50/p95/p99), throughput, error rate, DB/external-call breakdown, JVM metrics; use it to find the slow hop, correlate with deploys, set SLO alerts; RED (Rate/Errors/Duration) for services, USE (Utilization/Saturation/Errors) for resources; sampling to control cost; ties logs+metrics+traces via trace id*

**450. How do you add a read/query caching layer to a read-heavy service?**
   🔑 *Cache-aside with Redis for hot entities + computed read models; `@Cacheable` on service methods keyed by inputs; TTL + event-driven eviction on writes; near-cache (Caffeine) in front of Redis to cut network RTT; materialized views / denormalized read tables (CQRS) for expensive aggregations; cache stampede protection; measure hit ratio; never cache strongly-consistent financial reads without tight invalidation*

[⬆️ Back to top](#contents)

## Production Troubleshooting, Logging & Observability

**451. How do you correlate a request across many microservices (distributed tracing)?**
   🔑 *Trace id generated at entry, propagated via headers (W3C `traceparent` / B3); each service emits child spans; Micrometer Tracing / OpenTelemetry auto-instruments Spring Boot → Zipkin/Jaeger/Tempo; `MDC.put("traceId",...)` so every log line carries it → cross-service search in ELK/Splunk; sampling to control volume; ties the three pillars (logs + metrics + traces) together; W3C Trace Context header: `traceparent: 00-{traceId:32hex}-{parentSpanId:16hex}-{flags:2hex}`; every service: reads `traceparent` from incoming request, creates a child span (new `parentSpanId`), forwards the header on outgoing calls; Micrometer Tracing + OTEL bridge: Spring Boot auto-configures this when `micrometer-tracing-bridge-otel` + `opentelemetry-exporter-zipkin` (or `-jaeger`) are on classpath; spans are created for `@RestController` methods, JDBC queries, Kafka sends/receives, WebClient calls; MDC: `traceId` and `spanId` are set in `MDC` by Micrometer Tracing — include in logback pattern: `%X{traceId} %X{spanId}`; in JSON logs: `{"traceId":"abc...","spanId":"def...","message":"..."}`; sampling strategies: head-based (decide at trace start): `management.tracing.sampling.probability=0.1` (10%); tail-based (decide after seeing the full trace — keep slow/error traces, drop fast ones); OpenTelemetry Collector agent: receives spans from multiple services, batches, sends to Jaeger/Tempo; Kafka: trace context in message headers (`W3C_TRACE_CONTEXT` propagator); Spring Kafka auto-propagates if `io.micrometer:micrometer-tracing` is on classpath; finding a request: take the `traceId` from a user's error report → search in Kibana/Splunk + Jaeger — see all service hops, which step was slow, which threw an error*

**452. What is your structured logging strategy — what do you log and never log?**
   🔑 *Log = request/trace id, user/tenant id, timing, business events, state transitions, errors with stack; NEVER log = passwords, tokens, PAN/CVV, PII (GDPR), secrets; JSON structured logs (logstash-logback-encoder), INFO in prod (DEBUG on demand via actuator `/loggers`), async appender to avoid blocking, sampling for high-volume paths, retention/ILM policies, alert on error rate not every line*

**453. How do you set up observability (ELK, Prometheus, Grafana) — the three pillars?**
   🔑 *Logs = Filebeat → Logstash/parse → Elasticsearch → Kibana (ILM for retention); Metrics = Prometheus scrapes `/actuator/prometheus` (JVM, HikariCP, custom Micrometer) → Grafana dashboards (p99 latency, error rate, GC pause, throughput); Traces = OpenTelemetry → Jaeger; Alertmanager → PagerDuty/Slack; three pillars = logs + metrics + traces; Splunk/OpenSearch as alternatives*

**454. How do you debug an OutOfMemoryError / analyze a heap dump?**
   🔑 *`-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=...` or `jmap -dump:format=b,file=heap.hprof <pid>`; GC logs to see if it's GC-overhead-limit; analyze in Eclipse MAT → Leak Suspects + Dominator Tree + Histogram (retained heap by class); common = unbounded static collection/cache without eviction, unclosed streams, ThreadLocal not removed, oversized cache; fix = bound cache (Caffeine maxSize+TTL), try-with-resources, weak refs, tune `-Xmx`; add GC alerting; OOM types: `java.lang.OutOfMemoryError: Java heap space` = heap full (increase `-Xmx` or fix leak); `java.lang.OutOfMemoryError: GC overhead limit exceeded` = GC is spending >98% of time recovering <2% heap (thrashing); `java.lang.OutOfMemoryError: Metaspace` = class metadata full (classloader leak, dynamic code generation); `java.lang.OutOfMemoryError: Direct buffer memory` = off-heap NIO buffers exhausted (increase `-XX:MaxDirectMemorySize`); `StackOverflowError` = deep/infinite recursion; heap dump: `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/heapdump.hprof` — automatic on OOM; `jmap -dump:format=b,live,file=heap.hprof <pid>` — manual (live=only reachable objects); Eclipse MAT analysis: (1) Leak Suspects report: `Problem Suspect 1: 2.1 GB retained by 1 instance of ...` — identifies the biggest retainers; (2) Dominator Tree: which objects retain the most heap; (3) Histogram: how many instances of each class + retained heap; (4) OQL queries: `SELECT * FROM java.util.HashMap WHERE size > 100000`; common causes: (1) unbounded static `Map`/`List` (no eviction) used as a cache — fix: `Caffeine.newBuilder().maximumSize(1000).expireAfterWrite(1, HOURS).build()`; (2) ThreadLocal not removed → classloader leak in web containers; `threadLocal.remove()` in a finally block; (3) unclosed `InputStream`/`Connection` → use try-with-resources; (4) event listeners not deregistered → retained objects; monitoring: JVM metrics `jvm_memory_used_bytes` in Grafana; alert when >80% heap*

**455. How do you handle a production incident (on-call, midnight, critical service down)?**
   🔑 *Acknowledge alert → assess scope/severity (S1/S2) + blast radius → check dashboards + recent deploys (rollback candidate) → pull logs/traces + resource metrics (CPU/mem/DB conns/GC) → check pod status (OOMKilled/CrashLoopBackOff) + dependency health → fastest recovery (rollback/restart/scale/config fix) → communicate status → preserve artifacts (thread/heap dump) → blameless postmortem + 5-whys + action items + runbook update; structured incident response: (1) acknowledge: PagerDuty/Opsgenie ack within SLA; (2) severity triage: S1=production down, data loss risk, >X% users affected; S2=degraded, workaround exists; S3=minor; (3) communicate: set up a war room (Slack/Zoom bridge); update status page for external users; (4) diagnose FAST, fix first: rollback the last deploy if timing correlates (< 2 min); restart crashing pods (`kubectl rollout restart deployment/order-service`); scale up if resource saturation; (5) diagnose: `kubectl get pods` → CrashLoopBackOff, OOMKilled, Pending; `kubectl logs order-service-xxx --previous` → exception before crash; `kubectl describe pod` → eviction reason, events; APM trace waterfall → which call is failing; error log spike in Kibana → first ERROR after the incident started; (6) fix: rollback → `helm rollback order-service 1` or `kubectl rollout undo`; config change → `kubectl apply -f`; mitigation → feature flag disable, circuit breaker manual open, traffic shift; (7) preserve artifacts BEFORE fixing if possible: thread dump, heap dump, pod logs; (8) postmortem: within 24-48h; blameless (systems/process, not people); timeline, root cause, contributing factors, 5-whys; action items: new test/alert/runbook/architectural fix; track completion; share learnings*

**456. An API is suddenly slow (and prod is slower than dev) — what do you check?**
   🔑 *APM: which endpoint, since when, correlate with deploy; DB slow-query log (N+1 / missing index on prod data volume); downstream latency via trace; thread dump (blocked threads); GC pause log (STW); connection-pool wait; CPU/memory throttling; prod-vs-dev differences = data volume → full scans, network latency, JIT warmup, pool/timeout config, synchronous log I/O, resource limits; fix the first confirmed bottleneck*

**457. DB CPU is low but API latency is high — what's the cause?**
   🔑 *App-side bottleneck: Tomcat/thread pool exhausted waiting on a connection or lock, slow external call blocking the request thread, GC stop-the-world pause, in-app lock contention, undersized connection pool (waiting to acquire, not executing), serialization overhead, N+1 (many tiny fast queries exhausting the pool); check thread dump + pool pending metric + GC log*

**458. How do you detect crashes and failures?**
   🔑 *Health endpoints (`/actuator/health` with DB/Redis/Kafka indicators, liveness/readiness groups); K8s liveness probe restarts hung pods, OOMKilled status; error tracking (Sentry/Datadog); GC-overhead + OOM alerts; deadlock via thread dump (jstack/`kill -3`); circuit-breaker-open events; alert on error-rate spike + ERROR log count threshold*

**459. What are SLI, SLO, SLA and error budgets?**
   🔑 *SLI = measured signal (latency, availability, error rate); SLO = internal target (99.9% availability, p99 < 200ms); SLA = external contract + penalties; error budget = 1 − SLO (allowed failure) → governs release pace (burn too fast → freeze features, fix reliability); RED method (Rate/Errors/Duration) for services, USE method (Utilization/Saturation/Errors) for resources*

**460. What is Splunk and how do you use it?**
   🔑 *Log aggregation + search/analytics; SPL (Splunk Processing Language) for ad-hoc queries + dashboards; ingest via Filebeat/HEC; dashboards for error rate, latency percentiles, business KPIs; alerts on log patterns/error spikes; correlate across services by traceId; licensed by daily ingest volume; ELK/OpenSearch is the open-source alternative*

**461. What logging framework do you use and how do you configure it?**
   🔑 *SLF4J facade + Logback (or Log4j2); `logback-spring.xml` = appenders (Console, RollingFile), encoders (JSON via LogstashEncoder), levels per package, AsyncAppender to avoid blocking; MDC for traceId/userId; `logging.level.*` in application.yml; runtime level change via actuator `/loggers`; profile-specific config*

**462. How do you tell a war story about a production issue / the hardest bug you solved?**
   🔑 *STAR: Situation (impact/severity), Task (your role), Action (diagnostic steps — thread/heap dump, slow query, trace; the fix), Result (metrics + preventive measures: new test, alert, runbook, architectural fix); examples = N+1 overloading the DB, memory leak from an unbounded cache, distributed-lock deadlock, race in an idempotency check, misconfigured pool timeout; show you improved the system, not just patched it*

**463. How do you design alerting that avoids fatigue?**
   🔑 *Alert on symptoms users feel (SLO burn, error rate, p99 latency, saturation) not every cause; actionable + severity-tiered (page for S1, ticket for S2); every alert links a runbook; deduplicate + group (Alertmanager), set sensible thresholds + for-durations to avoid flapping; multi-window burn-rate alerts on the error budget; silence during deploys; review + prune noisy alerts regularly; on-call rotation + escalation policy*

**464. What is a postmortem culture and how do you run one?**
   🔑 *Blameless (focus on systems/process, not people), written within days; timeline + impact + detection + root cause (5-whys / contributing factors), what went well/poorly; concrete action items with owners + due dates (new test, alert, runbook, guardrail, architectural fix), tracked to completion; share org-wide to spread learning; measure MTTR/MTTD trends*

[⬆️ Back to top](#contents)

## Testing & Code Quality

**465. Explain the test pyramid — unit vs integration vs contract vs E2E.**
   🔑 *Unit = single class, deps mocked, fast (ms), many (JUnit 5 + Mockito); integration = real DB/Kafka/Redis via Testcontainers, fewer, catch wiring/config/query bugs; contract (Pact) = consumer publishes expectations, provider verifies (prevents silent breaking changes); E2E = full stack, minimal (slow/flaky); in practice the pyramid is often inverted in legacy code — heavy integration layer because mocked tests passed but prod diverged*

**466. Explain Mockito — @Mock vs @MockBean vs @Spy vs @InjectMocks, and mock vs stub vs fake.**
   🔑 *@Mock = plain Mockito mock (`@ExtendWith(MockitoExtension.class)`, fast, no context); @MockBean = replaces a bean in the Spring context (`@SpringBootTest`/`@WebMvcTest`, slower); @Spy = real object with selective stubbing; @InjectMocks = inject mocks into the class under test; @Captor = capture arguments; mock = configured fake, stub = canned response, spy = partial real, fake = working in-memory impl, dummy = placeholder; `doReturn/when` for void + spies; `@Mock`: Mockito creates a proxy object that returns default values (null/0/false/empty collections) unless stubbed; `@ExtendWith(MockitoExtension.class)` processes the annotations; fast (no Spring context); `when(repo.findById(1L)).thenReturn(Optional.of(user))`; `@MockBean` (Spring): replaces the matching Spring bean in the ApplicationContext with a Mockito mock; necessary when you need the real Spring wiring (security, transactions, AOP) but want to mock a specific dependency; `@SpringBootTest @MockBean UserRepository repo`; context is reused across tests with the same `@MockBean` set; `@Spy`: creates a real instance but allows selective method stubbing; other methods call the real implementation; `@Spy List<String> list = new ArrayList<>(); doReturn(5).when(spy).size();` — `add()` is real, `size()` is stubbed; use `doReturn/when` with spies (not `when/thenReturn` — real method might be called first); `@InjectMocks`: Mockito injects `@Mock`/`@Spy` fields into the class under test via constructor/setter/field injection; `@Captor`: `ArgumentCaptor<OrderEvent> captor = ArgumentCaptor.forClass(OrderEvent.class); verify(kafkaTemplate).send(any(), captor.capture()); assertEquals("PLACED", captor.getValue().status())`; test double taxonomy: Mock = verifiable fake (behavior assertions), Stub = canned-response fake (state assertions), Fake = working implementation (in-memory repo), Spy = partial real, Dummy = placeholder never actually used; `verify(repo, times(1)).save(any())` — verifies interaction; prefer state assertions over interaction verification*

**467. What are common Mockito pitfalls?**
   🔑 *Over-mocking (testing nothing real), verifying internals (`verify(repo).save()`) couples test to implementation, deep stubs (`mock.getX().getY()`) = design smell, mocking value objects, unstubbed call returns null → NPE, `@Spy` on classes with heavy constructors; prefer behaviour assertions over interaction verification*

**468. How have you used Testcontainers?**
   🔑 *`@Testcontainers` + `@Container PostgreSQLContainer`/`KafkaContainer`/`RedisContainer`, `@DynamicPropertySource` to inject JDBC URL / bootstrap servers; per-class static container (faster) vs per-test (isolation); catches real driver/broker/constraint/migration behaviour that H2/mocks miss (Flyway runs against real Postgres); slower but higher fidelity; basic setup: `@Testcontainers class OrderRepoTest { @Container static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15"); @DynamicPropertySource static void props(DynamicPropertyRegistry r) { r.add("spring.datasource.url", postgres::getJdbcUrl); r.add("spring.datasource.username", postgres::getUsername); r.add("spring.datasource.password", postgres::getPassword); } @Test void shouldSaveOrder() { ... } }`; `@Container static` = one container shared across all tests in the class (faster); `@Container` (instance field) = new container per test (slower, full isolation); Ryuk: Testcontainers runs a Ryuk container that auto-cleans up test containers after the JVM exits; Spring Boot 3.1+ `@ServiceConnection`: `@Container @ServiceConnection static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15")` — auto-configures the datasource properties (no `@DynamicPropertySource`); `KafkaContainer`: `@Container static KafkaContainer kafka = new KafkaContainer(DockerImageName.parse("confluentinc/cp-kafka:7.4.0")); r.add("spring.kafka.bootstrap-servers", kafka::getBootstrapServers);`; value over H2: runs real Postgres — catches constraint violations, Flyway migration failures, dialect-specific SQL, pg_stat functions; catches real Kafka — partition assignment, exactly-once, schema registry; `@SpringBootTest` + Testcontainers = full integration tests with real infrastructure; CI: requires Docker daemon (GitHub Actions supports it out of the box); speed: mitigate with `@Container static` (reuse) + parallel test classes*

**469. How do you test asynchronous / event-driven flows without flaky sleeps?**
   🔑 *Awaitility (`await().atMost(10, SECONDS).until(() -> repo.findById(id).isPresent())`); CountDownLatch in the listener; `@EmbeddedKafka` + KafkaTemplate publish + Awaitility poll; `CompletableFuture.get(timeout)`; ArgumentCaptor with polling; NEVER `Thread.sleep` (time-dependent, flaky on slow CI)*

**470. How do you test @Transactional rollback and idempotency?**
   🔑 *Rollback: `@DataJpaTest` (`@Rollback` default), or throw mid-transaction + assert state unchanged in a separate query via TransactionTemplate; idempotency: send the same request twice with the same key → assert a single DB record + same response; assert unique-constraint violation is handled gracefully; `@DataJpaTest` rollback: by default every test method is `@Transactional` + `@Rollback(true)` — the test runs in a transaction that is rolled back at the end → DB stays clean between tests; `@Rollback(false)` to commit and verify the committed state; `@Transactional` rollback test: `@Test void shouldRollbackOnException() { assertThrows(RuntimeException.class, () -> orderService.createOrder(invalidOrder)); // In a separate tx: Order saved = transactionTemplate.execute(status -> repo.findById(testId).orElse(null)); assertNull(saved); // order was rolled back }` — the `assertThrows` call is outside the tx so you can verify the state afterwards with a fresh transaction; idempotency test: `@Test void shouldBeIdempotent() { String idempotencyKey = UUID.randomUUID().toString(); ResponseEntity<OrderDto> r1 = restTemplate.postForEntity("/orders", request.withKey(idempotencyKey), OrderDto.class); ResponseEntity<OrderDto> r2 = restTemplate.postForEntity("/orders", request.withKey(idempotencyKey), OrderDto.class); assertEquals(r1.getBody().getId(), r2.getBody().getId()); assertEquals(1, orderRepository.countByIdempotencyKey(idempotencyKey)); }` — same key, same response, one DB record; unique constraint test: `@Test void shouldHandleDuplicateKey() { repo.save(new User("john@example.com")); assertThrows(DataIntegrityViolationException.class, () -> repo.save(new User("john@example.com"))); }` — verify the exception maps to a 409 response in the controller advice*

**471. How do you integration-test a REST controller?**
   🔑 *`@SpringBootTest(webEnvironment=RANDOM_PORT)` + TestRestTemplate for full HTTP; or `@WebMvcTest(Controller.class)` slice + `@MockBean` service + MockMvc (`mockMvc.perform(post(...).contentType(JSON).content(body)).andExpect(status().isCreated()).andExpect(jsonPath("$.id").exists())`); `@DirtiesContext` when state leaks between tests*

**472. How do you mock a Kafka producer/consumer or an external REST call?**
   🔑 *Kafka: `@EmbeddedKafka` (real broker in-process) for integration, `@MockBean KafkaTemplate` for unit; external REST: WireMock (`stubFor(get(urlEqualTo(...)).willReturn(aResponse().withStatus(200).withBody(...)))`, verify + fault injection) or `MockRestServiceServer` for RestTemplate; Feign client `@MockBean`*

**473. Coding: write a JUnit 5 + Mockito test for a service calling a repository and a Feign client.**
   🔑 *`@ExtendWith(MockitoExtension.class)`, `@Mock Repo repo`, `@Mock Client client`, `@InjectMocks Service service`; given `when(repo.findById(1L)).thenReturn(Optional.of(user))` + `when(client.get(1L)).thenReturn(details)`; act `service.process(1L)`; assert result; `verify(client).get(1L)`; edge case: empty Optional → throws ResourceNotFoundException*

**474. How do you measure code coverage, and what's a good target?**
   🔑 *JaCoCo (line/branch/method coverage) via Maven/Gradle plugin → SonarQube quality gate fails below threshold; ~70-80% (higher for domain logic, lower for DTOs/config); coverage ≠ quality → mutation testing (PIT) measures whether tests actually catch bugs; focus on business logic*

**475. Explain contract testing with Pact.**
   🔑 *Consumer-driven: consumer defines expected request/response → publishes a Pact file to the Pact Broker; provider runs `pact:verify` to confirm it satisfies all consumers; catches breaking API changes pre-deploy; `can-i-deploy` gate in CI; more reliable than shared integration environments; solves the "who broke what" problem*

**476. What is SonarQube, and how do you fix what it flags?**
   🔑 *Static analysis: bugs (NPE, resource leak), vulnerabilities (SQLi, hardcoded creds), code smells (long method, deep nesting, duplication, magic numbers), security hotspots, coverage import; quality gate blocks PR merge on violations; fixes = extract method, introduce constants, reduce cyclomatic complexity, remove duplication; runs in CI via SonarScanner, PR decoration; SonarQube categories: Bugs = code that will likely cause a runtime failure (NPE, resource leak, infinite recursion, incorrect null check); Vulnerabilities = security issues (SQLi via string concatenation, hardcoded password, weak cipher usage); Code Smells = maintainability issues (method too long, cognitive complexity too high, duplicated code block, magic number/literal, unused variable, switch without default); Security Hotspots = require human review to determine if a vulnerability (logging user input, using `Random` — may or may not be an issue); rules and quality gate: `sonar.qualitygate.wait=true` in CI → build fails if the quality gate fails; gate: 0 new bugs, 0 new vulnerabilities, < 3% duplication on new code, coverage ≥ 80%; fixing common findings: (1) NPE: add null check or use `Optional`, or annotate with `@NonNull`; (2) resource leak: wrap in try-with-resources; (3) cognitive complexity > 15: extract methods, early returns, flatten nested ifs; (4) duplication: extract to a shared utility or a common base class; (5) magic number: `private static final int MAX_RETRY = 3;`; (6) hardcoded password: `sonar.issue.ignore.multicriteria` to suppress false positive (test data), or move to `application.yml`; CI integration: `./mvnw sonar:sonar -Dsonar.token=${SONAR_TOKEN}` → results on SonarQube server; PR decoration: SonarQube posts comments on GitHub PRs for new issues; SonarCloud: SaaS version for open-source/small teams; Sonar supports: Java, Kotlin, JavaScript, TypeScript, Python, Go, C#, and 30+ languages*

**477. Have you used SAST (Checkmarx/Veracode/Semgrep)? Walk through a finding.**
   🔑 *SAST = scans source for vulnerabilities without running the app; findings = SQLi (→ parameterized queries), XSS (→ output encoding), path traversal (→ canonicalize/validate), hardcoded credentials (→ Vault/env), insecure deserialization; triage false positives, fix → rescan → verify; build-breaking gate for critical findings in CI; SAST tools: Checkmarx (enterprise, deep taint analysis), Veracode (cloud SaaS, CI/CD integration), Semgrep (fast, rule-based, open-source rules + custom), SpotBugs + Find Security Bugs plugin (Java-specific, free), Snyk Code; typical SQL injection finding: "User input at line 45 (`searchTerm`) reaches SQL query at line 78 without sanitization: `"SELECT * FROM users WHERE name = '" + searchTerm + "'"`"; triage: (1) confirm it's real (is `searchTerm` actually from user input?), (2) assess exploitability (is the endpoint authenticated?), (3) confirm it's NOT false-positive (tool misidentified a sanitized value); fix: `jdbcTemplate.query("SELECT * FROM users WHERE name = ?", new Object[]{searchTerm}, rowMapper)` — parameterized query; rescan + verify finding is gone; path traversal finding: `new File(uploadDir + "/" + userProvidedFilename)` → user can supply `../../../../etc/passwd`; fix: `Path base = Paths.get(uploadDir).toAbsolutePath(); Path target = base.resolve(filename).normalize(); if (!target.startsWith(base)) throw new SecurityException("Path traversal detected");`; hardcoded credential finding: `String password = "admin123"` in source; fix: move to `application.yml` → Vault/Secrets Manager; CI gate: `semgrep --config=p/java --error --json | tee semgrep-report.json`; fail build on CRITICAL/HIGH findings; Semgrep custom rule: `rules: - id: no-string-concat-sql patterns: - pattern: jdbcTemplate.query("..." + ...) message: "SQL injection risk"`*

**478. What is Zally and API linting/governance?**
   🔑 *Zally (Zalando) = lints OpenAPI/Swagger specs against API guidelines (naming, versioning, pagination, error structure, required headers); CLI or CI step; custom org rules; enforces consistent APIs across teams before publishing; Spectral is a popular alternative; API linting: checks the OpenAPI spec (not the implementation) against style/governance rules; catches API design issues before they are published and become hard to change; Zally rules (from Zalando RESTful API guidelines): (1) URIs must use lowercase kebab-case (`/order-items`, not `/orderItems`); (2) GET/DELETE must not have a request body; (3) all endpoints must have a description and at least one 4xx error response; (4) pagination must use `limit`/`offset` or cursor; (5) error responses must follow RFC 7807 (Problem Details); (6) no HTTP Basic auth (use OAuth2/JWT); Zally setup: `zally lint api/openapi.yaml`; CI step: `- name: Lint API; run: docker run ghcr.io/zalando/zally:latest lint api/openapi.yaml`; Spectral: open-source, extensible, rule-based linter; supports built-in rulesets (`spectral:oas`) + custom `.spectral.yaml` rules; `spectral lint api/openapi.yaml`; custom rule: `my-org-rules.yaml: rules: require-x-api-owner: message: "All paths must have x-api-owner"; given: "$.paths" then: field: x-api-owner function: truthy`; API governance: spec-first design (write OpenAPI before implementation) + linting gates + contract tests (Pact) + breaking change detection (Optic CLI) in CI; `optic diff --check` compares current spec to the previous version and reports breaking changes; enforce: no new breaking changes without a major version bump*

**479. Explain TDD and BDD.**
   🔑 *TDD = Red (failing test) → Green (minimal code) → Refactor; drives design + coverage; BDD = Given/When/Then scenarios (Cucumber + Gherkin) readable by business, maps to acceptance tests; both shift testing left; use TDD for logic, BDD for cross-team acceptance criteria*

**480. What's your overall automation-testing approach?**
   🔑 *Unit (Mockito/JUnit), integration (Testcontainers/Spring slices), API (RestAssured/Karate), contract (Pact), UI (Selenium/Playwright, page-object model), BDD (Cucumber); test data factories/fixtures, parallel execution, fail-fast in CI, reporting (Allure); parameterized + edge-case tests; flaky-test quarantine*

**481. How do you manage test data and keep tests deterministic and isolated?**
   🔑 *Test data builders/object mothers + factories (not shared fixtures); fresh schema per run (Testcontainers + Flyway) or `@Sql`/`@Transactional` rollback for isolation; seed minimal data per test; avoid shared mutable static state + test ordering dependence; control time (`Clock` injection) + randomness (fixed seed) + UUIDs; clean up or use unique keys; parallel-safe (unique tenant/ids); no reliance on prod data*

**482. How do you include performance/load and security testing in CI?**
   🔑 *Performance: Gatling/k6 smoke in the pipeline against a staging deploy, assert p99 + error-rate thresholds (fail the build on regression), full soak nightly; Security: SAST (SonarQube/Semgrep/Checkmarx) + SCA (OWASP Dependency-Check/Snyk) as build gates, DAST (OWASP ZAP) against staging, secret scanning (gitleaks); shift-left, fail fast on critical findings*

[⬆️ Back to top](#contents)

## Cloud, DevOps, Docker, Kubernetes & CI/CD

**483. Walk through a CI/CD pipeline end-to-end, and the tools you use.**
   🔑 *Push/PR → CI trigger (Jenkins/GitLab CI/GitHub Actions) → build (Maven/Gradle, parallel modules) → unit + integration tests (Testcontainers) → quality gate (SonarQube + JaCoCo + SAST) → Docker multi-stage image tagged with git SHA → push to registry (ECR/Artifactory) → deploy dev (Helm/ArgoCD) → smoke tests → promote staging → manual approval → deploy prod (blue-green/canary) → monitor → auto-rollback on error spike/failed health check; secrets in credentials store, feature flags for progressive rollout*

**484. Explain deployment strategies — rolling, blue-green, canary, recreate, shadow.**
   🔑 *Rolling (K8s default) = incremental pod replace (maxSurge/maxUnavailable), zero downtime but mixed versions briefly; blue-green = two envs, instant LB switch, easy rollback, double infra, DB must be backward-compatible; canary = gradual traffic shift (1%→100%) with automated analysis (Argo Rollouts/Istio), catches issues early; recreate = all down then up (downtime); shadow = mirror traffic to new version without user impact; A/B = segment-based (feature flags)*

**485. How do you achieve zero-downtime deployment when the schema is changing?**
   🔑 *Expand-contract: (1) add nullable column, (2) deploy code writing both old+new, (3) backfill, (4) deploy code reading new only, (5) drop old; each step independently deployable; Flyway/Liquibase versioned migrations; never rename/drop in the same deploy as usage; backward-compatible for the N-1 running version during rollout (see Databases section)*

**486. Docker — image vs container, Dockerfile layers, multi-stage builds, and how to create one.**
   🔑 *Image = immutable layered filesystem (each FROM/RUN/COPY = a cached layer); container = running instance with a writable top layer; multi-stage = JDK stage compiles (`FROM maven AS builder; RUN mvn package`) → slim JRE stage copies only the jar (`FROM eclipse-temurin:21-jre-alpine; COPY --from=builder ...; ENTRYPOINT ["java","-jar","app.jar"]`) → smaller image, smaller attack surface; put stable layers first (deps before source), `.dockerignore` excludes target/.git*

**487. VM vs Docker — how does a container run on any machine?**
   🔑 *VM = full OS + kernel via hypervisor, heavy (GBs, minutes), strong isolation; container = shares the host Linux kernel via namespaces (PID/net/mount/UTS/IPC isolation) + cgroups (CPU/memory/IO limits), lightweight (MBs, ms); UnionFS/overlayfs layers form the filesystem; runtime containerd→runc; Docker Desktop runs a small Linux VM on Mac/Windows; same image runs anywhere with the same kernel syscall interface → "works on my machine" solved*

**488. What is a Docker volume?**
   🔑 *Persistent storage surviving container restart/removal; named volumes (Docker-managed), bind mounts (host path → container), tmpfs (in-memory); `docker volume ls/inspect/rm`; K8s equivalent = PersistentVolume + PersistentVolumeClaim; use for DB data, logs, shared config*

**489. Where are Docker images stored, and how do you version them?**
   🔑 *Local daemon store + remote registries (Docker Hub, AWS ECR private + IAM + image scanning, Artifactory, GHCR); tag with semantic version + git SHA (reproducible); avoid `latest` in prod (mutable, no rollback); immutable tags / pin by digest (`sha256:...`); promote the SAME image across envs by tag (no rebuild); ECR lifecycle policy to retire old images*

**490. How do you manage multiple containers locally and in production?**
   🔑 *Local = Docker Compose (`docker-compose up/down/ps/logs/exec`, services/networks/volumes); prod = Kubernetes; `docker ps/logs/exec -it/stop/rm/images/pull/push`; Compose is dev-only, K8s orchestrates in prod*

**491. What is Kubernetes, and what are its key commands?**
   🔑 *Container orchestration: scheduling, self-healing (restart failed pods), rolling updates, autoscaling (HPA), service discovery + LB, config/secret + storage orchestration; control plane = API server + etcd + scheduler + controller-manager, workers = kubelet + runtime + kube-proxy; commands = `kubectl get/describe pods|svc|deploy`, `logs -f`, `exec -it`, `apply -f`, `scale`, `rollout status/undo`, `port-forward`, `top`*

**492. Explain Pod, Deployment, Service, ConfigMap, Secret — and how a Service finds Pods.**
   🔑 *Pod = smallest unit (1+ containers); Deployment = desired replicas + rolling update; Service = stable DNS + virtual IP load-balancing to Pods by label selector; discovery = kube-proxy iptables/IPVS + CoreDNS (`svc.namespace.svc.cluster.local`); ConfigMap = non-secret config (env/mounted file); Secret = base64 (NOT encryption — enable etcd encryption at rest + RBAC, or ExternalSecrets); StatefulSet for stateful, DaemonSet for per-node*

**493. How do you configure readiness vs liveness probes for Spring Boot (and what breaks if reversed)?**
   🔑 *Readiness = ready to serve (DB connected, caches warm, Kafka up) → K8s removes from Service endpoints if failing (no restart); Liveness = alive/not hung → K8s RESTARTS the pod if failing; keep liveness minimal (internal state only) — if liveness checks a slow DB → restart loop; readiness may check external deps; Spring Boot 2.3+ auto-configures `/actuator/health/readiness` + `/liveness`; set correct initialDelay/timeout*

**494. How do you do graceful shutdown so in-flight requests aren't dropped?**
   🔑 *K8s sends SIGTERM → stop accepting new requests → drain in-flight → exit; Spring `server.shutdown=graceful` + `spring.lifecycle.timeout-per-shutdown-phase=30s`; K8s `terminationGracePeriodSeconds ≥ timeout`; preStop hook (`sleep 5`) to let the LB deregister before SIGTERM; Kafka consumers commit + finish the current batch; PodDisruptionBudget protects availability during drains*

**495. How do you scale replicas in Kubernetes (CLI, declarative, autoscaling)?**
   🔑 *`kubectl scale deployment app --replicas=5`; patch/edit spec.replicas + `kubectl apply`; HPA (`kubectl autoscale ... --min 2 --max 10 --cpu-percent 70`) on CPU/mem/custom metrics; KEDA for event-driven (Kafka lag, SQS depth); VPA for right-sizing requests; set resource requests/limits so the scheduler + HPA work correctly*

**496. What are Helm charts?**
   🔑 *Helm = K8s package manager; Chart = Chart.yaml (metadata), values.yaml (defaults), templates/ (K8s YAML with Go templating `{{.Values.image.tag}}`); `helm install/upgrade --set/--values prod.yaml`, `helm rollback app 1`, `helm list`; release = named install; repos for distribution; simplifies parameterized multi-environment deploys; Kustomize is the templating-free alternative*

**497. How do you manage secrets in Kubernetes/AWS securely?**
   🔑 *Never hardcode; ExternalSecrets Operator syncs from AWS Secrets Manager/Vault into K8s Secrets; IRSA (IAM Roles for Service Accounts) for keyless pod AWS access; Vault agent sidecar for dynamic short-lived DB creds; Sealed Secrets for GitOps (encrypted in Git); encrypt etcd at rest; never log/dump secrets in env; rotate + least privilege + audit*

**498. Maven vs Gradle — which and why, and multi-module experience?**
   🔑 *Maven = XML POM, convention lifecycle (validate/compile/test/package/install/deploy), ubiquitous; Gradle = Groovy/Kotlin DSL, incremental builds + build cache (faster for large repos), more flexible; multi-module Maven = parent POM + `<modules>` + `<dependencyManagement>`; Gradle = settings.gradle `include(':mod')`; use the wrapper (mvnw/gradlew) for reproducibility*

**499. EC2 vs Lambda vs ECS/EKS — when serverless vs containers?**
   🔑 *EC2 = full VM you manage (long-running, flexible); Lambda = serverless FaaS, event-triggered (API GW/SQS/S3/EventBridge), scales to 0, pay-per-invocation, 15-min limit, cold starts, stateless; ECS (Fargate) = managed containers, no node management; EKS = Kubernetes on AWS; Lambda for intermittent event handlers, ECS/EKS for always-on microservices, EC2 when you need OS control*

**500. How do you deploy a JAR to EC2?**
   🔑 *SSH (key pair) + scp jar + `java -jar app.jar`; better = systemd service for auto-restart, or CodeDeploy agent + appspec.yml, or containerize → ECR → ECS/EKS; security group opens the app port; Elastic IP for a stable address; user-data script for bootstrap; prefer immutable AMIs/containers over in-place*

**501. How do you trigger and develop an AWS Lambda?**
   🔑 *Triggers = API Gateway (HTTP), SQS (event source mapping, batch), S3, SNS, EventBridge (cron/event), Kinesis, DynamoDB Streams; develop = `RequestHandler<In,Out>`, Lambda context (timeout/logging), env vars for config, layers for shared deps, IAM execution role, X-Ray tracing; deploy = ZIP → S3, container image (ECR), or SAM/CDK/Terraform; mitigate cold start with provisioned concurrency*

**502. How does SQS help decouple microservices?**
   🔑 *Producer sends + returns immediately (async); consumer polls at its own rate (independent scaling); SQS buffers bursts; visibility timeout = processing window; DLQ after max receives; FIFO queue for ordering + dedup; at-least-once (idempotent consumers required); producer and consumer can be down independently without data loss*

**503. What is Infrastructure as Code / Terraform, and its basic commands?**
   🔑 *IaC = version-controlled, reviewed, reproducible infra (drift detection, dev=prod parity); Terraform = declarative HCL, state in S3 + DynamoDB locking, modules for reuse, providers for AWS/GCP/Azure, workspaces per env; commands = `init` (providers), `plan` (dry-run diff), `apply`, `destroy`, `fmt`, `validate`, `state list/show/rm`, `import`, `output`; Ansible = config management (imperative, agentless); CDK/Pulumi = IaC in real languages*

**504. Which AWS services do you use, and the key best practices?**
   🔑 *EC2, ECS/EKS, Lambda, S3, RDS, ElastiCache (Redis), SQS/SNS, API Gateway, CloudFront (CDN), Route53 (DNS + latency/weighted routing), VPC, IAM, CloudWatch, Secrets Manager/KMS; best practices = least-privilege IAM + roles, private subnets for DBs, security groups by port/source, encryption at rest (KMS) + in transit (TLS), multi-AZ for HA, autoscaling, cost monitoring*

**505. What is GitOps (ArgoCD/Flux)?**
   🔑 *Git is the single source of truth for desired cluster state; a controller (ArgoCD/Flux) continuously reconciles the cluster to the Git manifests/Helm; deploy = merge a PR (image tag bump), controller syncs; benefits = auditability, easy rollback (git revert), drift detection, no cluster credentials in CI; pull-based vs push-based deploys*

**506. How do you manage environments, config and load balancers across deployments?**
   🔑 *Environments = K8s namespaces (dev/staging/prod) + Helm values per env; promote the same image tag across namespaces via GitOps; common config = Spring Cloud Config/ConfigMaps, secrets via ExternalSecrets/Vault; LB = K8s Ingress + AWS ALB Ingress Controller, cert-manager for TLS, Route53 weighted routing for canary; 12-factor config from the environment*

**507. Explain the Maven build lifecycle and the key plugins.**
   🔑 *Lifecycle phases: validate → compile → test (surefire, unit) → package → verify (failsafe, integration *IT) → install → deploy; each phase runs bound plugin goals; key plugins = compiler, surefire/failsafe, spring-boot (repackage fat jar / build-image via Buildpacks), jib (daemonless image), shade/assembly (uber jar), jacoco, versions, enforcer; `mvn clean verify`; profiles for env-specific builds; `dependency:tree` to debug conflicts*

**508. What backend development tools do you use day to day, and why?**
   🔑 *Build = Maven/Gradle; IDE = IntelliJ; boilerplate = Lombok, mapping = MapStruct; API = OpenAPI/Swagger (springdoc), testing = JUnit 5/Mockito/Testcontainers/WireMock/RestAssured; API clients = Postman/Insomnia/Newman; quality = SonarQube/JaCoCo, security = OWASP Dependency-Check/Snyk; DB migration = Flyway/Liquibase; containers = Docker/Compose; local infra = Testcontainers; observability = Actuator/Micrometer/Prometheus/Grafana; CI/CD = Jenkins/GitHub Actions/ArgoCD; secrets = Vault*

**509. Which API testing and mocking tools do you use?**
   🔑 *Manual/exploratory = Postman/Insomnia; automated API = Newman (Postman in CI), RestAssured (JVM), Karate (BDD); load = JMeter/Gatling/k6; mocking externals = WireMock (stub + fault injection), MockServer; Kafka = EmbeddedKafka/Testcontainers; contract = Pact/Spring Cloud Contract; Swagger UI for try-it-out; keep API tests in CI with a deployed/staged service*

**510. What is the twelve-factor app methodology?**
   🔑 *Codebase (one repo, many deploys), explicit dependencies, config in env, backing services as attached resources, build/release/run separation, stateless processes, port binding, concurrency via process scale-out, disposability (fast start/graceful shutdown), dev/prod parity, logs as event streams (stdout), admin tasks as one-off processes; underpins cloud-native + containerized microservices*

[⬆️ Back to top](#contents)

## Design Patterns & SOLID

**511. Explain all 5 SOLID principles with a real violation → fix for each.**
   🔑 *S (SRP): God class doing fetch+format+email → split into focused classes (one reason to change); O (OCP): switch-on-type → interface + polymorphism, new type = new class; L (LSP): subclass throws where parent doesn't / breaks contract → fix inheritance or use composition, test via base-type reference; I (ISP): fat interface forcing empty methods → split into client-focused interfaces; D (DIP): `new ConcreteService()` → inject an interface via constructor (Spring `@Autowired`)*

**512. How do you refactor a class that violates the Single Responsibility Principle?**
   🔑 *Spot multiple reasons to change (data access + business logic + formatting + notification); extract each into its own class — e.g. OrderService → OrderRepository, OrderValidator, DiscountCalculator, OrderEmailer; one reason to change each; improves testability + reuse; watch for >1 "section" of tests as a smell*

**513. Give an example of Dependency Inversion / IoC & DI in Spring.**
   🔑 *Define `PaymentGateway` interface; impls StripeGateway/PayPalGateway; `@Autowired PaymentGateway` (Spring injects via `@Qualifier`/`@Primary`); high-level service depends on the abstraction, not the Stripe SDK → swap provider without touching the service, inject a mock in tests; IoC = framework owns creation/lifecycle, DI = constructor/setter/field injection of dependencies*

**514. Coding: design a PaymentProcessor so new types can be added without modifying existing code (OCP/Strategy).**
   🔑 *`interface PaymentProcessor { boolean process(Payment p); String type(); }`; impls UpiProcessor/CardProcessor/WalletProcessor as `@Component`; registry `Map<String,PaymentProcessor>` auto-built from the bean list; `service.process(p) = registry.get(p.getType()).process(p)`; new type = new class, zero changes to the service — Strategy pattern, no if-else/switch*

**515. Explain the GoF design patterns (creational, structural, behavioral) with real examples.**
   🔑 *Creational: Factory (create by type, hide construction), Abstract Factory (families), Builder (complex immutable objects/DTOs), Singleton (Spring beans), Prototype (clone); Structural: Adapter (legacy XML→JSON wrapper), Decorator (add caching/logging), Proxy (Spring AOP/lazy/access), Facade (simplified API), Composite (tree — org/discount rules); Behavioral: Strategy (interchangeable algorithm), Observer (ApplicationEvent + @EventListener), Command, Template Method (AbstractBatchJob), Chain of Responsibility (filters)*

**516. Strategy vs Factory — when do you use which?**
   🔑 *Strategy = behaviour varies at runtime (same operation done differently per context — discount per tier, processor per method), interchangeable algorithm; Factory = creation varies (produce the right object by type/config, then use it uniformly); often combined — a Factory creates the right Strategy; both replace if-else/switch on type*

**517. Why is the enum singleton the best singleton?**
   🔑 *`enum Singleton { INSTANCE; }` — JVM guarantees one instance per classloader, thread-safe without synchronization, serialization-safe (enum mechanism blocks a new instance on deserialize), reflection-safe (can't call the constructor via reflection); preferred over double-checked locking / static-holder*

**518. Which design pattern did you use, and why (over the alternatives)?**
   🔑 *STAR: problem (adding payment types required editing existing code) → chose Strategy/OCP → interface + per-type impls + registry → result: new type = new class, zero modification, each unit-testable; why over alternatives: vs if-else (not scalable/open-closed), vs inheritance (not flexible, deep hierarchies); tie the choice to the problem it solved*

**519. Explain Builder, Decorator, Proxy and Template Method with where you'd use them.**
   🔑 *Builder = step-by-step construction of complex/immutable objects (DTOs, request objects, Lombok `@Builder`); Decorator = wrap to add behaviour without subclassing (buffered streams, caching/logging wrappers); Proxy = stand-in for access control/lazy/remote (Spring AOP + `@Transactional`/`@Async` use CGLIB/JDK proxies); Template Method = skeleton algorithm with overridable steps (AbstractBatchJob, Spring's JdbcTemplate)*

**520. Explain Observer and Chain of Responsibility.**
   🔑 *Observer = publish/subscribe, subject notifies observers of state change (Spring ApplicationEvent + `@EventListener`, message brokers) → decouples producer from consumers; Chain of Responsibility = request passed along a chain until one handles it (Servlet filters, Spring Security filter chain, validation chains, middleware)*

**521. What design anti-patterns do you watch for in review?**
   🔑 *God class (does everything → SRP violation), singleton abuse (hidden global state, hard tests), anemic domain model (logic leaked into services), service locator over DI, premature abstraction (YAGNI), deep inheritance over composition, primitive obsession, long parameter lists, feature envy, magic numbers, circular dependencies*

**522. Why do design patterns matter — which problem does each solve?**
   🔑 *Shared vocabulary + proven solutions + guided design; problem→pattern: one shared instance → Singleton; complex creation → Builder; create by type → Factory; incompatible interfaces → Adapter; add behaviour → Decorator; control access → Proxy; many listeners → Observer; interchangeable algorithm → Strategy; tree → Composite; simplified API → Facade; skeleton algorithm → Template Method; apply judiciously, not everywhere*

**523. Explain the DDD tactical building blocks.**
   🔑 *Entity (identity + lifecycle), Value Object (immutable, equality by value), Aggregate + Aggregate Root (consistency boundary — only the root is referenced, enforces invariants, one transaction per aggregate), Repository (collection-like persistence of aggregates), Domain Service (logic not belonging to one entity), Domain Event, Factory; strategic = Bounded Context + Ubiquitous Language + Context Map; guides microservice boundaries*

**524. Explain the Repository and Unit of Work patterns.**
   🔑 *Repository = abstraction over persistence that looks like an in-memory collection of aggregates (hides queries/ORM) → testable, swappable; Unit of Work = tracks changes in a business transaction and commits them atomically (JPA persistence context + flush/commit IS a Unit of Work); Spring Data `JpaRepository` implements Repository; keep domain logic out of repositories*

**525. Explain the Specification and Null Object patterns.**
   🔑 *Specification = encapsulate a business rule/predicate as an object, composable (and/or/not) → reusable query/validation criteria (Spring Data `Specification`, dynamic queries); Null Object = a do-nothing implementation of an interface instead of null → removes null checks + NPEs (e.g. a no-op logger/handler); both reduce conditional clutter and centralize rules*

**526. What are the common resilience/concurrency patterns beyond GoF?**
   🔑 *Resilience = Circuit Breaker, Retry, Timeout, Bulkhead, Rate Limiter, Fallback, Outbox, Saga; concurrency = Producer-Consumer, Thread Pool, Future/Promise, Read-Write Lock, Immutable/Thread Confinement, Monitor, Active Object; integration = Gateway, Adapter, Ambassador/Sidecar, Anti-Corruption Layer; cloud = Strangler Fig, CQRS, Event Sourcing, Leader Election*

[⬆️ Back to top](#contents)

## System Design & Design Scenarios

**527. How do you approach a system design problem (requirements, NFRs, estimation, trade-offs)?**
   🔑 *Clarify functional requirements + NFRs (Non-Functional Requirements: scalability, availability, latency, consistency, durability, security, cost); back-of-envelope estimation (QPS, storage, bandwidth, read:write ratio); define the API; high-level components + data model; identify bottlenecks → scale (cache, replicas, sharding, queue, CDN); state one explicit trade-off per decision (consistency vs availability, latency vs cost); discuss failure modes + monitoring*

**528. Explain load balancing — L4 vs L7, algorithms, and where it sits.**
   🔑 *L4 (transport, TCP/UDP) = fast, connection-level; L7 (application, HTTP) = routing by path/header/cookie, TLS termination, retries; algorithms = round-robin, least-connections, least-response-time, IP/consistent hash (sticky); client-side vs server-side LB; health checks remove bad nodes; global LB (DNS/GSLB) + regional; avoid sticky sessions (keep services stateless)*

**529. What is consistent hashing and why is it used?**
   🔑 *Maps keys + nodes onto a hash ring → a key goes to the next node clockwise; adding/removing a node only remaps ~1/N keys (vs rehash-everything with `mod N`); virtual nodes smooth distribution + avoid hotspots; used in distributed caches (Redis Cluster), sharded DBs, Cassandra/DynamoDB partitioning, load balancers*

**530. Explain data consistency models (strong, eventual, causal) and how to tune them.**
   🔑 *Strong = read sees the latest write (RDBMS, quorum R+W>N, linearizable) — higher latency; eventual = replicas converge after a lag (Dynamo/Cassandra) — high availability; causal = preserves cause-effect order; read-your-writes / monotonic reads session guarantees; tune via quorum (N/R/W), read from primary vs replica, versioning/vector clocks; CAP/PACELC drives the choice*

**531. Design an idempotent payment/transaction API that prevents double-charging on retry.**
   🔑 *Client-generated idempotency key (UUID) in a header → unique constraint on `idempotency_key`; on retry return the cached response; atomic check-and-insert (unique constraint or SERIALIZABLE), distributed lock for concurrent in-flight dup; outbox for reliable event publishing; saga for multi-service atomicity; audit every attempt; charge exactly once; test = simulate timeout → retry → assert single charge (Stripe's idempotency-key as reference)*

**532. Design a rate limiter for an API gateway at scale.**
   🔑 *Algorithms = token bucket (bursty, smooth refill), fixed window (simple, boundary spike), sliding-window log (accurate, memory-heavy), sliding-window counter (practical hybrid); Redis INCR+EXPIRE / sorted set + Lua for atomic distributed counting; key by API key/user/IP; 429 + Retry-After + X-RateLimit-* headers; enforce at gateway (AWS WAF/Kong/Nginx) + in-app for business quotas*

**533. Design a notification service (email/SMS/push) with retry and dedup.**
   🔑 *Notification service subscribes to domain events (OrderPlaced → send); Kafka topic per channel + consumer group per channel; retry topics with exponential backoff → DLQ after max; dedup by unique notification_id (Redis SETNX / DB unique); template engine for rendering; delivery-tracking table (PENDING/SENT/FAILED); user preference/opt-out; third-party delivery (Twilio/FCM); priority queues for urgent*

**534. Design a distributed unique ID generator (Snowflake) across regions.**
   🔑 *Snowflake 64-bit = sign(1) + timestamp-ms(41, ~69y) + datacenter(5) + machine(5) + sequence(12 → 4096/ms/node); monotonic → good index locality; no coordination; handle clock skew (reject/wait on backwards clock); alternatives = UUIDv4 (random, no order), ULID (sortable, URL-safe), DB sequence (bottleneck at scale), Redis INCR*

**535. Design a real-time fraud detection system on transaction events.**
   🔑 *Kafka transaction stream → Flink/Kafka Streams for <100ms decisions; feature store for entity profiles (velocity, history); rule engine (Drools) + ML model ensemble; CQRS (write path triggers check, read path for analysts); threshold tuning (false positive = friction, false negative = fraud passes); async enrichment for non-latency-sensitive signals; review queue + feedback loop to retrain*

**536. Design a reconciliation system comparing two large datasets (ledger vs settlement file).**
   🔑 *Batch (nightly/T+1); load into staging; sorted-merge join on txn id + amount + date (or hash per partition); categorize mismatches (missing-in-source, missing-in-target, amount mismatch); persist to a mismatch table; auto-reconcile timing differences, alert the rest for manual review; idempotent + re-runnable; off-peak schedule; account for the settlement delay window*

**537. How do you scale a service handling millions of transactions/day?**
   🔑 *Redis cache for hot data (cache-aside + TTL), DB read replicas (reads → replica, writes → primary), async non-critical work via Kafka (notifications/audit/reporting), partition/shard the transaction table (by date or account id), CQRS read model, horizontal pod autoscaling, connection pooling (PgBouncer), CDN for cacheable responses, multi-region for DR*

**538. Design a URL shortener.**
   🔑 *Generate a short key = base62 of an auto-increment id or a Snowflake/hash (collision check); KV store (Redis/DynamoDB) `shortKey → longURL`; 301/302 redirect; read-heavy → cache + CDN; custom aliases + expiry; analytics via async event; estimate: billions of URLs → ~7 base62 chars; avoid predictable sequential keys if enumeration is a concern*

**539. Design a news feed / timeline.**
   🔑 *Fan-out-on-write (push to each follower's feed cache — fast reads, costly for celebrities) vs fan-out-on-read (pull + merge at read — cheap writes, slower reads); hybrid (push for most, pull for celebrities); feed store in Redis per user; ranking (time + ML relevance); pagination via cursor; precompute + cache; CDN for media*

**540. Design a chat / messaging system.**
   🔑 *WebSocket for real-time bidirectional; message service persists to a partitioned store (Cassandra by conversation id); delivery receipts + read status; online presence (Redis + heartbeat); push notifications when offline; ordering per conversation (sequence/timestamp); fan-out for group chats; E2E encryption optional; at-least-once + client dedup by message id*

**541. Design a typeahead / autocomplete.**
   🔑 *Trie/prefix tree of top queries + frequency; precompute top-k per prefix, cache in Redis; update frequencies async from a query log (stream); rank by popularity + personalization + recency; debounce client requests; Elasticsearch completion suggester as an alternative; sharding by prefix; sub-100ms latency target*

**542. Design a distributed cache.**
   🔑 *Partition keys across nodes via consistent hashing (virtual nodes); replication for availability; eviction (LRU/LFU); write policy (write-through/back/around); invalidation (TTL/events); client-side routing or a proxy; handle hot keys (local cache/replication); Redis Cluster/Memcached; cache stampede protection; monitor hit ratio + evictions*

**543. Design a system for real-time data across multiple regions.**
   🔑 *Globally distributed DB (Spanner/CockroachDB — strong consistency via consensus) or Aurora Global DB (primary + <1s cross-region replicas); Kafka MirrorMaker 2 for cross-region event replication; CDN + edge cache for reads; active-active with conflict resolution (last-write-wins/CRDTs) for AP data, consensus (Raft/Paxos) for financial CP data; Route53 latency routing + health-check failover*

**544. How do you handle UTC and time zones in a multi-region application?**
   🔑 *Store timestamps in UTC (`Instant` / TIMESTAMP WITH TIME ZONE) — never `LocalDateTime` in the DB (no zone); convert at the presentation layer (`ZonedDateTime` with `ZoneId.of("Asia/Kolkata")`); `LocalTime` is zone-free (ambiguous across regions); UTC avoids DST bugs; Hibernate configured to UTC; send ISO-8601 with offset over the wire*

**545. Design a DB schema for org → department → user → role → permission (with exceptions).**
   🔑 *organizations, departments (org_id FK), users (dept_id FK), roles (org_id), user_roles (composite PK), permissions (resource, action), role_permissions, exceptional_permissions (user_id, permission_id, granted BOOL, reason); RBAC with an exception overlay; effective permissions = role_permissions UNION granted exceptions MINUS revoked; index (user_id, permission_id); audit_log for changes*

**546. Design a promo/offer system — which fields, and how to enable a code for a specific day or week of month?**
   🔑 *Fields: code (unique), type (PERCENT/FLAT/BOGO), value, valid_from/to, day_of_week_mask (bitmask), week_of_month, max_uses, current_uses, min_order_amount, user_id (null=all); store in DB for auditable finite codes; for huge scale use HMAC-signed codes (rules encoded in the token, validated without a lookup); Redis cache for hot lookups; rule engine for complex validity; cleanup job for expired codes*

**547. Design driver/trip tracking without overloading the DB with GPS pings.**
   🔑 *Real-time location in Redis (GEOADD + TTL — only current position, fast); persist to DB only significant events (trip start/end, waypoints every N seconds) not every ping; trip state machine (REQUESTED→MATCHED→ONGOING→COMPLETED→BILLED) in DB; WebSocket/SSE pushes live location to the client; history in DB for billing/audit; raw GPS traces to cold storage (S3) after 90 days*

**548. Find the nearest drivers to a pickup location at scale.**
   🔑 *Redis GEOADD/GEOSEARCH for in-memory real-time proximity (drivers update every few seconds); or PostGIS (`GEOGRAPHY(POINT)` + `ST_DWithin`/`ST_Distance` + GiST spatial index); GeoHash bucketing for approximate sharding; filter by availability + bounded radius; expand radius if too few results; JPA native query for PostGIS*

**549. Two riders at the same cafe get matched to the same driver — how do you prevent double booking?**
   🔑 *Atomic check-and-assign: `UPDATE drivers SET status='ASSIGNED' WHERE id=? AND status='AVAILABLE'` → first wins (affected rows=1), second gets 0 → retry next driver; or `SELECT ... FOR UPDATE` pessimistic lock, or Redis SETNX on driver_id for the matching window; saga rolls back if booking fails after assignment; idempotent assignment by request id*

**550. How would you design a coding platform (LeetCode-style)?**
   🔑 *Problem service (store/serve), user service (auth), submission service → queue (Kafka/SQS); execution service = sandboxed per-language runner in a Docker container with CPU/memory/time limits + no network; judge compares output vs expected test cases (in S3); result service stores + queries; Redis sorted set for leaderboard; WebSocket for live result push; rate limit per user; strong isolation for untrusted code*

**551. Design a distributed job scheduler / task queue.**
   🔑 *Durable job store (DB/Redis) with status + run_at + priority; workers poll with `SELECT ... FOR UPDATE SKIP LOCKED` (or a broker like SQS/Kafka) so one worker per job; leader election or DB lock (ShedLock/Quartz cluster) so cron fires once across instances; retries with backoff + max attempts → DLQ; idempotent jobs; visibility timeout/lease to recover crashed workers; exactly-once effect via idempotency keys; scale workers horizontally; observability on queue depth + lag*

**552. Design an object storage service (S3-like).**
   🔑 *Metadata service (bucket/object/version → location, in a scalable DB) + data stored as chunks across storage nodes with replication/erasure coding for durability; consistent hashing to place chunks; multipart + resumable upload; content-addressing (hash) for dedup; pre-signed URLs for direct client upload/download; eventual vs strong read-after-write consistency; lifecycle/tiering (hot→cold/Glacier); CDN in front; integrity checksums*

**553. Design a metrics / monitoring and alerting system.**
   🔑 *Agents/exporters expose metrics → pull (Prometheus scrape) or push (StatsD/OTel collector); time-series DB (Prometheus/VictoriaMetrics/Timescale) with downsampling + retention; labels/dimensions + cardinality control; query/dashboards (Grafana); alerting rules + burn-rate on SLOs → Alertmanager → PagerDuty; high write throughput + aggregation; traces/logs correlation; scale via sharding/federation*

**554. Design a reliable webhook delivery system.**
   🔑 *Persist outbound events (outbox) → delivery workers POST to subscriber URLs; retries with exponential backoff + jitter + max attempts → dead-letter + manual replay; HMAC signature header for authenticity + timestamp (replay protection); at-least-once → consumers must be idempotent (event id); per-subscriber rate limiting + circuit breaker; delivery status + audit; respect subscriber 2xx as ack; ordering optional via per-subscriber queue*

**555. How do you do back-of-the-envelope capacity estimation in a design?**
   🔑 *Estimate DAU → QPS (peak ≈ 2-10× average), read:write ratio; storage = records × size × retention × replication; bandwidth = QPS × payload; memory for cache = hot-set size; derive #servers from per-instance throughput + headroom; sanity-check numbers (seconds/day ≈ 86400, 1M writes/day ≈ ~12/s avg); state assumptions explicitly; use these to justify sharding/caching/replicas*

**556. Design a double-entry ledger / wallet for payments.**
   🔑 *Append-only immutable journal: every transaction = balanced debit + credit entries (sum to zero); account balances = sum of entries (or maintained + reconciled); idempotency key per transaction (no double-post); ACID within a transaction, outbox for events; never UPDATE/DELETE entries (reversal = compensating entries); strong consistency (CP), optimistic locking on balance, audit + reconciliation job; partition by account, snapshots for fast balance reads*

[⬆️ Back to top](#contents)

## Kotlin vs Java

**557. Explain null safety in Kotlin (?, !!, ?:, ?.) vs Java's Optional/NPE.**
   🔑 *`?` nullable type (`String?`), `!!` non-null assertion (throws — avoid), `?:` Elvis default, `?.` safe call (short-circuits to null); compile-time null safety; Java `Optional` = explicit wrapper (of/ofNullable/map/orElseThrow); Kotlin preferred — zero boilerplate, enforced by the compiler; platform types from Java interop need care*

**558. Data classes vs POJOs/records — what does Kotlin generate?**
   🔑 *`data class` auto-generates equals/hashCode (by primary-constructor props), toString, copy() (immutable-style copy), componentN() (destructuring); Java equivalent = records (always immutable, Java 16+); Kotlin data class allows `var`; Lombok `@Data` was the pre-record Java stand-in*

**559. val vs var, and coroutines vs Java threads/virtual threads.**
   🔑 *`val` = immutable reference (like final), `var` = mutable; coroutines = lightweight (millions), `suspend` functions pause without blocking an OS thread, structured concurrency scopes, cooperative cancellation; Java threads = 1:1 OS threads (expensive); Java 21 virtual threads = coroutine-like scalability but transparent blocking (no suspend/async syntax)*

**560. What problem do extension functions solve?**
   🔑 *Add methods to existing/third-party classes without inheritance or editing source (`fun String.toTitleCase(): String`); compiled to a static method with the receiver as first param (no runtime overhead); no access to private members; the Kotlin stdlib (`filter`/`map`/`let`) is built on them*

**561. Coding: safely chain nullable calls with ?.let{}.**
   🔑 *`val city = user?.address?.let { it.city ?: "Unknown" }`; `let` runs only if non-null (receiver as `it`); scope functions = let/run/with/apply/also; Java equivalent = `Optional.ofNullable(user).map(User::getAddress).map(Address::getCity).orElse("Unknown")` — Kotlin is more concise*

**562. Explain Kotlin scope functions (let, run, with, apply, also).**
   🔑 *`let` (transform, receiver=`it`, returns lambda result), `run` (receiver=`this`, returns result), `with` (non-extension, `this`), `apply` (configure, `this`, returns receiver), `also` (side-effect, `it`, returns receiver); pick by receiver reference (this vs it) and return value (lambda result vs receiver)*

**563. Explain sealed classes and when-expressions in Kotlin.**
   🔑 *`sealed` = closed hierarchy known at compile time (like Java sealed) → exhaustive `when` with no `else`; model ADTs / result types (Success/Error); smart-casts in each branch; pairs well with data classes + pattern-style destructuring*

**564. What are inline functions, reified types and higher-order functions?**
   🔑 *Higher-order = functions taking/returning functions (lambdas); `inline` = lambda body inlined at call site → no lambda object allocation + enables `reified` type params (access `T` at runtime, unlike Java erasure — `inline fun <reified T> ...`); use for hot higher-order calls + type-based dispatch*

**565. How does Kotlin interoperate with Java?**
   🔑 *Fully bidirectional — call Java from Kotlin and vice-versa; `@JvmStatic`/`@JvmOverloads`/`@JvmField`/`@JvmName` control the generated bytecode; platform types (`String!`) from Java have no null info (handle carefully); SAM conversion for Java functional interfaces; Kotlin stdlib compiles to JVM bytecode; mix in the same Spring Boot project*

[⬆️ Back to top](#contents)

## Version Control, SDLC & Documentation

**566. Explain the Gitflow branching strategy (and alternatives).**
   🔑 *main (prod), develop (integration), feature/* (from develop, PR back), release/* (prep, merge to main+develop, tag), hotfix/* (from main, merge to main+develop); long-lived = main+develop; alternatives = trunk-based development (short-lived branches + feature flags, CI-friendly, preferred for fast delivery), GitHub Flow (main + feature branches only)*

**567. Merge vs rebase — and how do you resolve conflicts?**
   🔑 *Merge = merge commit, preserves history, safe on shared branches; rebase = replays commits for linear history (never rebase public/shared branches — rewrites history); squash merge = collapse a feature into one commit; conflict = edit markers → `git add` → `--continue`; `git rerere` for recurring conflicts; interactive rebase (`-i`) to clean commits*

**568. reset vs revert vs cherry-pick — when do you use each?**
   🔑 *`git reset` moves the branch pointer (--soft keeps staged, --mixed unstages, --hard discards) — local history rewrite, don't on shared; `git revert` creates a new commit undoing a prior one — safe on shared branches; `git cherry-pick` applies a specific commit onto the current branch (hotfix backport); `git bisect` to binary-search a regression*

**569. What's your peer-review process — what do you look for in a PR?**
   🔑 *Correctness (solves the problem), tests (edge cases not just happy path), security (input validation, no secrets, injection risk), performance (N+1, missing index, bad algorithm), breaking changes (API/DB migration safety), readability + naming, SOLID violations; constructive comments, approve-with-nits, request-changes only for real issues; keep PRs small*

**570. Compare Waterfall, Scrum, Kanban and SAFe.**
   🔑 *Waterfall = sequential, fixed scope, stable/regulated requirements; Scrum = sprints + backlog + ceremonies + velocity, evolving requirements; Kanban = continuous flow + WIP limits, ops/support/maintenance; SAFe = scaled agile (Program Increments, ARTs) for large multi-team programmes*

**571. How do you estimate effort / story points?**
   🔑 *Story points = relative complexity + effort + uncertainty (not hours); Fibonacci scale, planning poker for consensus, a reference story as baseline, T-shirt sizing for epics; split stories > 8 points; account for unknowns, tech debt, testing, review; track velocity for capacity; re-estimate on scope discovery*

**572. How do you document APIs and write good service documentation?**
   🔑 *springdoc-openapi auto-generates OpenAPI 3 from `@Operation`/`@Schema`/`@ApiResponse`, served at `/swagger-ui.html`; API-first = write the spec first → generate stubs; also README (quick start, env vars, architecture), ADRs (why not what), runbook (deploy/rollback/incident steps), C4 diagrams; keep docs in the repo, updated + reviewed in PRs*

**573. What semantic versioning do you use for artifacts/APIs?**
   🔑 *SemVer MAJOR.MINOR.PATCH — MAJOR = breaking, MINOR = backward-compatible feature, PATCH = backward-compatible fix; pre-release/build metadata (`-rc.1`, `+sha`); drives dependency ranges + API deprecation; tag releases in git; separate internal build number (git SHA) from the public version*

**574. What are the challenges in migrating to a new Spring/Java version?**
   🔑 *Spring Boot 2→3 = `javax.*`→`jakarta.*`, Security config API rewrite (WebSecurityConfigurerAdapter removed), Hibernate 6 changes; Java 11→17→21 = module/reflective-access restrictions (`--add-opens` for Mockito/Spring), removed deprecated APIs, dependency compatibility matrix; run full integration suite, fix tests first, consider GraalVM native if used*

[⬆️ Back to top](#contents)

## Behavioral / Managerial

> *Answer with STAR (Situation · Task · Action · Result). Pre-select 4–5 real incidents and reuse them across several questions.*

**575. Tell me about the most critical production incident you've owned.**
   🔑 *STAR: Situation (what broke, user impact, severity); Task (your role — incident lead/on-call); Action (triage → correlate with deploy → logs/traces → root cause → hotfix/rollback → communicate); Result (MTTR, users affected); postmortem = blameless, 5-whys, action items (test/alert/runbook/architectural fix), follow-through; show you improved the system, not just fixed the symptom*

**576. Describe a time you traded off shipping fast vs doing it right.**
   🔑 *Articulate the tech debt explicitly, get business alignment on risk, time-box the shortcut, create a ticket + ADR for the proper fix; never shortcut security/data integrity/compliance/PII; outcome = shipped on time + tracked + paid back; shows pragmatism + responsibility*

**577. Tell me about a time your service caused a cascading failure in another team's system.**
   🔑 *Lessons on blast-radius design: circuit breakers (callers fail fast), bulkheads (isolate per downstream), async decoupling (Kafka vs sync), timeout budgets, graceful degradation (cached/default), backward-compatible changes, dependency-graph mapping, cross-team incident comms*

**578. Describe a schema/API change you rolled out without breaking consumers.**
   🔑 *Expand-contract for schema (add nullable → dual-write → backfill → read-new → drop); API = new versioned endpoint, run v1+v2 in parallel, consumer-driven contract tests (Pact), deprecation timeline + Sunset header, feature flag, sunset after migration*

**579. Tell me about disagreeing with an architect/senior engineer.**
   🔑 *Gather evidence first (benchmark/PoC/references), present alternatives with trade-offs (not opinion), take it to design review with data, disagree-and-commit if overruled (unless safety/compliance), escalate for serious risk, document the dissent in an ADR, follow up with data post-implementation*

**580. Describe a performance problem whose cause was non-obvious.**
   🔑 *Non-obvious causes = GC stop-the-world pauses, DNS resolution timeouts, connection-pool exhaustion (waiting not executing), JIT warmup, serialization overhead, lock contention, cloud burst throttling; diagnosis = flame graph + thread dump + GC log + distributed trace, eliminate suspects systematically (not the first code you blame)*

**581. Tell me about mentoring a junior or leading a small delivery.**
   🔑 *Balance: short daily check-ins to catch blockers, pair on complex areas, code review as teaching (explain why), let them struggle productively within bounds, delegate with a review safety net, unblocking > personal coding, track team velocity not individual output, adjust to confidence level*

**582. Describe pushing back on an unrealistic timeline.**
   🔑 *Frame with data: velocity-based estimate, risk of rushing (quality/security/rework), options (MVP scope / add resource / accept risk with sign-off); present trade-offs + solutions, escalate with facts, document agreed scope in writing, retrospect to improve estimates*

**583. How would you debug a bug that only reproduces in production, not staging?**
   🔑 *Add structured logging/tracing around the suspect + deploy; raise log level via actuator `/loggers`; diff prod vs staging config (env/flags/pool/timeouts); check data-dependence (anonymized prod data in staging); shadow traffic; check scale/load dependence; add metrics around the suspect path*

**584. Describe an early design decision you'd change in hindsight.**
   🔑 *Examples = wrong data model (over/under-normalized), premature optimization, monolith-vs-modular, over-engineered abstraction, tight coupling; lesson = validate assumptions with a PoC, involve the team early, YAGNI, document why in ADRs*

**585. What are your strengths and weaknesses?**
   🔑 *Strengths = deep Java/Spring/microservices, systematic root-cause debugging, reliability-focused distributed design; weaknesses with a growth story = frontend (learning React), over-polishing before shipping (now time-box), assuming shared context (now clearer written comms); honest + self-aware + improving*

**586. Explain microservices to an 8-year-old.**
   🔑 *Instead of one big Lego castle doing everything, many small pieces each doing one job (orders / payments / emails); if one breaks only that piece needs fixing, the rest keep working; different kids build different pieces (teams); pieces pass notes to talk (APIs/messages); swap a piece without rebuilding the castle (independent deploy)*

**587. Walk me through your current and previous projects.**
   🔑 *STAR per project: what the system does, your role + team size, architecture + key decisions (and why), biggest challenge solved, a production issue + resolution, what you'd improve; memorize key metrics (TPS, uptime SLA, team size, timeline); tailor the story to the interviewer's domain (payments/telco/etc.)*

**588. Prepare managerial STAR stories.**
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

**589. problem on number-to-words conversion.**
   🔑 *Divide into chunks: billions/millions/thousands/hundreds; word arrays: ones[]={"","one","two",...}, teens[]={"eleven","twelve",...}, tens[]={"","","twenty",...}; recursive helper converts chunk < 1000; handle edge cases: 0="zero", negative, exact millions/thousands; e.g. 1234 → "one thousand two hundred thirty four"; test: 0, 11, 100, 1000, 1000000, Integer.MAX_VALUE*

**590. Next palindrome in 100 to 1000, or next palidrome for 123 or 124 , ask for this is 131**
   🔑 *Brute force: iterate from n+1 upward; isPalindrome: s = Integer.toString(n); return s.equals(new StringBuilder(s).reverse().toString()); for 123 → 131; for 124 → 131; optimised: mirror first half (take first half, reverse it, compare; if less than second half increment first half); edge case: n=999 → 1001 (crosses digit boundary)*

**591. rotate list**
   🔑 *Three-reversal trick (O(n) time, O(1) space): k = k % n; reverse(0, n-1); reverse(0, k-1); reverse(k, n-1); or use Collections.rotate(list, k) for ArrayList; LinkedList: reconnect tail to head, advance to new tail; modulo handles k > n; handle k=0 (no-op); handle empty list; test: rotate [1,2,3,4,5] by 2 → [4,5,1,2,3]*

**592. shift all null or 0's in string list or int list**
   🔑 *Two-pointer: maintain write pointer (starts at 0); iterate read pointer; if element is non-null and non-zero, write to write-pointer position and advance; fill remaining positions with null/0; preserves relative order of non-zero elements; O(n) time O(1) space; Stream alternative: list.stream().filter(x -> x != null && x != 0).collect(toList()) then append zeros (creates new list); test: [1,0,2,null,3] → [1,2,3,0,null]*

**593. find second highest/lowest using steam**
   🔑 *Second highest: list.stream().filter(Objects::nonNull).distinct().sorted(Comparator.reverseOrder()).skip(1).findFirst().orElseThrow(); or .sorted(reverseOrder()).distinct() for de-dup before skip; IntStream: IntStream.of(arr).boxed().distinct().sorted(reverseOrder()).skip(1).findFirst(); O(n log n); O(n) approach: single pass tracking top1 and top2; handle all equal elements (no second distinct) → empty Optional*

**594. Find the mountain (start index, end index, width):**
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

**595. Find the Nth highest salary.**
   🔑 *`SELECT DISTINCT salary FROM emp ORDER BY salary DESC LIMIT 1 OFFSET N-1`; or window `SELECT salary FROM (SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) r FROM emp) t WHERE r = N`; DENSE_RANK handles ties; handle N > distinct count → NULL*

**596. Second highest salary without LIMIT.**
   🔑 *`SELECT MAX(salary) FROM emp WHERE salary < (SELECT MAX(salary) FROM emp)`; returns NULL if only one value; or DENSE_RANK = 2*

**597. Highest-paid employee per department.**
   🔑 *`SELECT * FROM (SELECT e.*, RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) r FROM emp e) t WHERE r = 1`; RANK/DENSE_RANK keeps ties, ROW_NUMBER picks exactly one; correlated-subquery alternative is slower*

**598. Top N rows per group.**
   🔑 *`ROW_NUMBER() OVER (PARTITION BY group_col ORDER BY sort_col DESC)` then `WHERE rn <= N`; index on (group_col, sort_col) for performance; lateral join alternative in Postgres*

**599. Find duplicate rows.**
   🔑 *`SELECT col, COUNT(*) FROM t GROUP BY col HAVING COUNT(*) > 1`; for full-row dups group by all columns; window `COUNT(*) OVER (PARTITION BY col)` to keep row detail*

**600. Delete duplicates keeping one.**
   🔑 *Postgres: `DELETE FROM t WHERE ctid NOT IN (SELECT MIN(ctid) FROM t GROUP BY dup_cols)`; or CTE with `ROW_NUMBER() OVER (PARTITION BY dup_cols ORDER BY id)` and delete where rn > 1; keep the smallest id/ctid*

**601. Employees earning more than their manager (self-join).**
   🔑 *`SELECT e.name FROM emp e JOIN emp m ON e.manager_id = m.id WHERE e.salary > m.salary`; self-join the table aliased twice on manager_id = id*

**602. Running / cumulative total.**
   🔑 *`SUM(amount) OVER (PARTITION BY account ORDER BY txn_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)`; window frame controls running vs moving-average; `ORDER BY` defines accumulation order*

**603. Month-over-month growth %.**
   🔑 *`LAG(revenue) OVER (ORDER BY month)` for the previous value → `(cur - prev)/prev * 100`; NULLIF to avoid divide-by-zero; `DATE_TRUNC('month', ...)` + GROUP BY to build monthly totals first*

**604. Find consecutive records (e.g., 3 logins in a row / islands).**
   🔑 *`LAG`/`LEAD` to compare neighbours, or gaps-and-islands: `ROW_NUMBER() - value/date` grouping trick to label consecutive runs, then HAVING COUNT >= 3; classic "consecutive numbers" / "consecutive days" pattern*

**605. Find gaps in a sequence of ids.**
   🔑 *`SELECT id + 1 AS gap_start FROM t t1 WHERE NOT EXISTS (SELECT 1 FROM t t2 WHERE t2.id = t1.id + 1)`; or compare `id` with `LEAD(id)`; generate_series to find all missing values in a range*

**606. Departments with no employees (anti-join).**
   🔑 *`SELECT d.* FROM dept d LEFT JOIN emp e ON e.dept_id = d.id WHERE e.id IS NULL`; or `NOT EXISTS`; count-including-zero = LEFT JOIN + `COUNT(e.id)` (not `COUNT(*)`) GROUP BY dept*

**607. Pivot rows to columns.**
   🔑 *Conditional aggregation `SUM(CASE WHEN month='Jan' THEN amt END) AS jan, ...`; Postgres `crosstab` (tablefunc) or `FILTER (WHERE ...)`; unpivot with UNION ALL or `LATERAL`/`unnest`*

**608. Median salary.**
   🔑 *`PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary)` (Postgres), or window with `ROW_NUMBER` + `COUNT` picking the middle row(s); MySQL lacks a native median → emulate with ranking*

**609. Customers with no orders in the last 90 days.**
   🔑 *`LEFT JOIN orders ... AND o.created_at >= now() - interval '90 days'` then `WHERE o.id IS NULL`; or `NOT EXISTS` with the date predicate; index on (customer_id, created_at)*

**610. Count distinct active users per day (DAU).**
   🔑 *`SELECT DATE(ts) d, COUNT(DISTINCT user_id) FROM events GROUP BY DATE(ts) ORDER BY d`; COUNT(DISTINCT) is expensive at scale → approximate with HyperLogLog (`approx_count_distinct`/Redis PFCOUNT)*

### More Java Coding Problems

**611. Reverse the words in a sentence.**
   🔑 *`String.join(" ", new StringBuilder-reverse of split)` or `Arrays.asList(s.trim().split("\\s+"))` → `Collections.reverse` → join; or two-pointer in a char[] for O(1) space; handle multiple spaces*

**612. First non-repeating character in a string.**
   🔑 *`LinkedHashMap<Character,Integer>` counts preserving order → first entry with count 1; or int[256] frequency + second pass; stream: `s.chars().boxed().collect(groupingBy(..., LinkedHashMap::new, counting()))`*

**613. Check anagrams / group anagrams.**
   🔑 *Anagram = sort both strings and compare, or compare frequency arrays (int[26]); group anagrams = HashMap keyed by sorted string (or frequency signature) → list of words; O(n·k log k)*

**614. Two-sum (indices summing to target).**
   🔑 *HashMap of value→index, for each x check if `target - x` seen → O(n); return the pair of indices; sorted-array variant = two pointers O(n) after sort*

**615. Merge overlapping intervals.**
   🔑 *Sort by start; iterate, if current.start <= last.end merge (`end = max(end, cur.end)`) else add new; O(n log n); classic for calendars/ranges*

**616. Implement an LRU cache.**
   🔑 *`LinkedHashMap(capacity, 0.75f, true)` + override `removeEldestEntry` → O(1) get/put with access-order eviction; or HashMap + doubly-linked list manually; thread-safe variant uses locks / Caffeine*

**617. Check balanced parentheses.**
   🔑 *Stack: push opens, on a close pop and match the pair; valid if stack empty at end; map closing→opening; handle `()[]{}`; empty stack on close = invalid*

**618. Find the missing number in 1..n.**
   🔑 *Sum formula `n*(n+1)/2 - actualSum`, or XOR of all indices and values (overflow-safe); for multiple missing use a boolean/bitset; O(n) time O(1) space*

**619. Maximum subarray sum (Kadane's algorithm).**
   🔑 *`cur = max(x, cur + x); best = max(best, cur)`; O(n); track start/end indices for the subarray; handle all-negative (best starts at first element)*

**620. Count word frequencies (streams).**
   🔑 *`Arrays.stream(text.toLowerCase().split("\\W+")).filter(not(String::isBlank)).collect(groupingBy(identity(), counting()))`; sort entries by value desc for top-k; `TreeMap` for sorted keys*

**621. Longest substring without repeating characters.**
   🔑 *Sliding window + HashMap/int[] of last-seen index; move left to `max(left, lastSeen+1)`; track max window length; O(n)*

**622. Reverse a linked list.**
   🔑 *Iterative: prev=null, cur=head, while cur { next=cur.next; cur.next=prev; prev=cur; cur=next }; return prev; O(n) O(1); recursive variant for practice*

**623. Detect a cycle in a linked list (Floyd's).**
   🔑 *Slow (1 step) + fast (2 steps) pointers; meet = cycle; to find the start, reset one to head and advance both by 1; O(n) O(1); HashSet alternative uses O(n) space*

**624. Producer-consumer with BlockingQueue.**
   🔑 *`BlockingQueue<T> q = new ArrayBlockingQueue<>(cap)`; producer `q.put(x)` (blocks when full), consumer `q.take()` (blocks when empty) → backpressure for free; poison-pill or `AtomicBoolean` + poll(timeout) for shutdown; multiple producers/consumers scale naturally*

**625. Group and aggregate with streams (e.g., total salary per department).**
   🔑 *`emps.stream().collect(groupingBy(Emp::getDept, summingDouble(Emp::getSalary)))`; downstream collectors (counting, averagingInt, mapping, maxBy); `collectingAndThen` to post-process*

**626. Sort a Map by value (top-N).**
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
