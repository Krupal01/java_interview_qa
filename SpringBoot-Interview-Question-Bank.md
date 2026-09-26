# Senior Java / Spring Boot Interview — Full Question Bank
*Prepared for a Senior Software Engineer (~10 YOE) interview loop*

> **Note on the interviewers:** LinkedIn blocks automated scraping of profile pages, so I couldn't pull full experience/skills sections for either profile. The one signal I could confirm: the `nishit-shah-146707153` profile is titled **"Senior Software Engineer at Mastercard."** I couldn't verify anything for the `kdpatel` profile — no match was confirmable. Given the Mastercard signal, I've weighted the scenario section toward payments/fintech-style concerns (transaction integrity, idempotency, high throughput, security/compliance) — treat that as an educated guess, not a fact about what they'll ask.

---

## How to use this
For a 10-year-experience loop, interviewers rarely stay at "define X." Expect the pattern: **define → probe internals → "why not the alternative?" → "tell me about a time you hit this in production."** For every topic below, be ready to (1) explain it, (2) explain a failure mode/edge case, (3) tie it to a real project you shipped.

---

## 1. Core Java — Deep

1. Explain the Java Memory Model (JMM): happens-before, visibility, `volatile` vs `synchronized`.
2. Walk through JVM memory areas — heap, stack, metaspace, PC register, native method stack.
3. How does garbage collection actually work? Compare G1, ZGC, and Parallel GC — when would you pick each?
4. What causes a memory leak in Java despite GC? Give a real example (e.g., static collections, unclosed listeners, ThreadLocal misuse).
5. `equals()`/`hashCode()` contract — what breaks if you override one but not the other? What happens if you mutate a key used in a `HashMap`?
6. Difference between `HashMap`, `ConcurrentHashMap`, and `Hashtable` internals — how does `ConcurrentHashMap` achieve thread safety without locking the whole map (segment locking in Java 7 vs CAS/bins in Java 8+)?
7. How does `String` pooling work? Why is `String` immutable, and what's the performance implication of heavy `+` concatenation in loops?
8. Explain generics type erasure — why can't you do `new T[]` or `instanceof T`?
9. Checked vs unchecked exceptions — what's your team's actual policy, and do you agree with it?
10. `finally` vs `try-with-resources` — what happens if both the try block and the close() throw?
11. What's new/relevant from Java 11 → 17 → 21 you've actually used: records, sealed classes, pattern matching for switch, virtual threads (Project Loom)? Have you used virtual threads in production — what changed for your thread-pool sizing assumptions?
12. Difference between `CompletableFuture`, `Future`, and reactive (`Mono`/`Flux`) — when do you reach for which?
13. Explain the difference between shallow and deep class loading issues — how have you debugged a `ClassNotFoundException`/`NoSuchMethodError` from a dependency conflict?

## 2. Concurrency & Multithreading (deep-dive favorite)

14. Explain `synchronized` internals — monitor locks, biased/lightweight/heavyweight locking.
15. Producer-consumer implementation — how would you build it with `BlockingQueue` vs `wait/notify` vs `Semaphore`?
16. What is a deadlock, and how have you actually detected/diagnosed one in production (thread dumps, jstack)?
17. `ExecutorService` — how do you size a thread pool for a CPU-bound vs I/O-bound workload? What's the danger of `Executors.newCachedThreadPool()` in production?
18. `ThreadLocal` — what's the memory leak risk in a pooled-thread environment (e.g., app servers), and how do you avoid it?
19. Explain `CountDownLatch` vs `CyclicBarrier` vs `Phaser` with a real use case.
20. What is false sharing, and have you ever had to deal with it?
21. Scenario: A service under load starts throwing intermittent `TimeoutException` only during peak traffic. Walk me through how you'd diagnose whether it's thread starvation, GC pause, connection pool exhaustion, or downstream latency.

## 3. Spring Core / Spring Boot

22. Explain the Spring bean lifecycle end-to-end (instantiation → dependency injection → `BeanPostProcessor` → init → destroy).
23. Constructor injection vs field injection — why is constructor injection preferred at scale, and what does it protect against (circular dependencies, immutability, testability)?
24. How does Spring Boot auto-configuration actually work under the hood (`@EnableAutoConfiguration`, `spring.factories` / `AutoConfiguration.imports`, conditional annotations)?
25. Explain bean scopes: singleton, prototype, request, session — what's a real bug you've seen from injecting a prototype bean into a singleton incorrectly?
26. `@Transactional` — how does the proxy work, and why does a self-invocation call (calling an `@Transactional` method from within the same class) silently not get intercepted?
27. Explain propagation types (`REQUIRED`, `REQUIRES_NEW`, `NESTED`) with a real scenario for each.
28. How do you handle configuration across environments (dev/stage/prod) — profiles, `@ConfigurationProperties`, Spring Cloud Config, Vault?
29. What does Spring Boot Actuator give you in production, and which custom health indicators/metrics have you built?
30. AOP in Spring — how have you used it for cross-cutting concerns (logging, auditing, retry)? What are the proxy limitations (final methods/classes, self-invocation)?
31. Explain the difference between `@Component`, `@Service`, `@Repository`, `@Controller` beyond "just semantics" — what does `@Repository` actually add (exception translation)?

## 4. Data Layer — JPA / Hibernate / Transactions

32. N+1 query problem — how do you detect it and fix it (`JOIN FETCH`, `@EntityGraph`, batch fetching)?
33. Explain first-level vs second-level cache in Hibernate — risks of enabling second-level cache in a distributed/multi-instance deployment.
34. Lazy vs eager loading — what's `LazyInitializationException` and how do you avoid it cleanly (not just `OpenSessionInView`, which has its own trade-offs)?
35. Explain optimistic vs pessimistic locking — when have you used `@Version` to solve a real concurrency bug (e.g., double-processing a payment)?
36. How do you handle schema migrations safely in a live system with zero downtime (Flyway/Liquibase, expand-contract pattern)?
37. Explain isolation levels (Read Committed, Repeatable Read, Serializable) and a real phantom-read/dirty-read bug you've hit.
38. Scenario: Two threads/instances update the same row concurrently and you're seeing lost updates. How do you fix it — DB constraint, versioning, distributed lock, or redesign?
39. How do you paginate efficiently over a very large table without `OFFSET` performance collapse?

## 5. REST API Design

40. How do you design idempotent APIs — why does this matter especially for POST-based payment/transaction endpoints (idempotency keys)?
41. Versioning strategy for public APIs — URI versioning vs header vs content negotiation — trade-offs?
42. How do you handle partial failures in a request that fans out to 3 downstream services?
43. Explain HATEOAS — have you actually used it, or is it mostly theoretical in your experience?
44. Designing pagination, filtering, and sorting for a large collection endpoint — cursor-based vs offset-based, and why.
45. How do you version and evolve a contract without breaking existing clients (backward compatibility, consumer-driven contracts, Pact)?

## 6. Microservices Architecture (breadth)

46. Monolith → microservices migration — what are the actual triggers that justify the split, and what's the first thing you decompose?
47. How do you handle distributed transactions across services — Saga pattern (choreography vs orchestration), compensating transactions?
48. Explain the Outbox pattern — why is it needed alongside message publishing to avoid dual-write inconsistency?
49. Service discovery and load balancing — Eureka/Consul vs Kubernetes-native (DNS + kube-proxy) — which have you used and why?
50. API Gateway responsibilities — routing, auth, rate limiting, request aggregation — what have you built with Spring Cloud Gateway/Kong/Zuul?
51. Circuit breaker pattern — Resilience4j vs Hystrix — how do you tune thresholds, and what's a real incident where a missing circuit breaker caused cascading failure?
52. How do you do centralized configuration and secret management across dozens of services (Spring Cloud Config, Vault, AWS Secrets Manager)?
53. Explain the CAP theorem in the context of a real design decision you made (choosing consistency vs availability).
54. How do you handle inter-service authentication (mTLS, service tokens, OAuth2 client-credentials)?
55. Explain event-driven architecture with Kafka — partitioning strategy, consumer group rebalancing, exactly-once vs at-least-once semantics, and how you've handled duplicate message processing.
56. How do you ensure message ordering when it matters (e.g., processing events for the same account/entity) in a partitioned topic?
57. What's your strategy for schema evolution in Kafka messages (Avro/Protobuf + schema registry) without breaking consumers?
58. Dead-letter queues — how do you design retry + DLQ handling so failed messages don't silently vanish or infinite-loop?

## 7. Security

59. Explain the Spring Security filter chain end-to-end for a JWT-based API.
60. Access token vs refresh token — how do you handle token revocation for a stateless JWT approach (since JWTs can't be "revoked" by default)?
61. OAuth2 grant types — which have you actually implemented (authorization code, client credentials), and why is implicit grant deprecated?
62. How do you prevent common vulnerabilities in a Spring Boot app — SQL injection (parameterized queries/JPA), XSS, CSRF (and why CSRF protection differs for stateless APIs vs session-based apps)?
63. How do you secure service-to-service calls in a microservices mesh?
64. Given a fintech/payments context: how do you ensure PCI-DSS-relevant data (card numbers, CVVs) never lands in logs, and how do you handle tokenization/masking?
65. Rate limiting and abuse prevention on public-facing APIs — token bucket vs sliding window, where do you enforce it (gateway vs app)?

## 8. Performance, Caching, Scalability

66. How do you find and fix a slow endpoint in production — profiling tools, APM (New Relic/Dynatrace/Datadog), thread dumps, flame graphs?
67. Caching strategy — cache-aside vs write-through vs write-behind — Redis vs local (Caffeine/Guava) — when do you use each, and how do you handle cache invalidation/stampede?
68. How do you design a system to handle a 10x traffic spike (e.g., flash sale, batch settlement window) without falling over?
69. Connection pool tuning (HikariCP) — what metrics do you watch, and what's a real pool-exhaustion incident you've debugged?
70. How do you do capacity planning / load testing before a major release (JMeter/Gatling, and what SLAs did you validate against)?

## 9. Testing

71. Unit vs integration vs contract testing — what's your team's actual test pyramid look like in practice (not textbook)?
72. Mockito — mocking pitfalls you've seen (over-mocking, testing implementation instead of behavior).
73. Testcontainers — how have you used it to test real DB/Kafka interactions instead of mocking them away?
74. How do you test asynchronous/event-driven flows reliably (avoiding flaky sleep-based tests)?
75. What's your approach to testing `@Transactional` rollback behavior and idempotency logic?

## 10. DevOps / Cloud / Deployment

76. Walk through your CI/CD pipeline end-to-end — build, test gates, image build, deployment strategy, rollback trigger.
77. Blue-green vs canary deployment — trade-offs, and which have you actually run in production?
78. How do you handle zero-downtime deployments when the schema itself is changing (expand-contract migration pattern)?
79. Kubernetes — how do you configure readiness vs liveness probes correctly for a Spring Boot app (and what goes wrong if you get them backwards)?
80. How do you handle graceful shutdown so in-flight requests aren't dropped during a pod termination/scale-down?
81. Observability — how do you correlate a request across 6 microservices (distributed tracing — Sleuth/Zipkin/OpenTelemetry, correlation IDs)?
82. Structured logging strategy — what do you log, what do you deliberately never log, and how do you keep log volume/cost sane at scale?

## 11. System Design (senior-level, expect at least one full design question)

83. Design a payment/transaction processing system that must guarantee exactly-once processing and handle retries safely.
84. Design a rate limiter for an API gateway serving millions of requests/day.
85. Design a notification service that fans out to email/SMS/push with retry and dedup.
86. Design an idempotent order/transaction API where the client may retry the same request due to a network timeout.
87. Design a system for real-time fraud detection on transaction events (streaming, low latency, false-positive trade-offs).
88. How would you design a distributed unique ID generator (Snowflake-style) for transaction IDs across multiple regions?
89. Design a reconciliation system that compares two large datasets (e.g., internal ledger vs external settlement file) and flags mismatches.

## 12. Scenario / Behavioral-Technical (production war stories — very commonly asked at this level)

90. Tell me about the most critical production incident you've owned — what broke, how did you find root cause, and what changed afterward (postmortem culture)?
91. Describe a time you had to make a trade-off between shipping fast and doing it "right" — how did you decide, and what was the outcome?
92. Tell me about a time your service caused a cascading failure in another team's system — what did you learn about blast-radius design?
93. Describe a schema/API change you had to roll out without breaking existing consumers — how did you sequence it?
94. Tell me about a time you disagreed with an architectural decision from a senior engineer/architect — how did you handle it?
95. Describe a performance problem you diagnosed that turned out to be something non-obvious (not the code you first suspected).
96. Tell me about mentoring a junior engineer or leading a small team through a delivery — how did you balance hands-on coding vs unblocking others?
97. Describe a time you had to push back on unrealistic timelines from product/business — how did you frame the conversation?
98. Walk me through how you'd approach debugging a bug that only reproduces in production and not in staging.
99. Tell me about a design decision you made early in a project that you'd change if you started over — what did you learn?

## 13. Rapid-fire "wide but shallow" round (they often close with these to check breadth)

100. Difference between `@RestController` and `@Controller`.
101. What is `@Qualifier` used for, and when do you need it?
102. Difference between `List`, `Set`, `Map` — when would you use `LinkedHashMap` vs `TreeMap`?
103. What does `@SpringBootApplication` actually bundle (`@Configuration`, `@EnableAutoConfiguration`, `@ComponentScan`)?
104. Difference between `PUT` and `PATCH`.
105. What's the difference between `@RequestParam`, `@PathVariable`, and `@RequestBody`?
106. What does `@Async` do, and what's the gotcha with calling an `@Async` method from the same class?
107. Difference between SOAP and REST — have you had to maintain a legacy SOAP integration?
108. What's a WebFlux/reactive stack, and when would you *not* use it over traditional Spring MVC?
109. Explain `application.yml` profile-specific overrides and property precedence order.
110. What build tool do you use (Maven/Gradle) and why — any multi-module project experience?

---

### Suggested prep approach
- For every "explain X" question, prepare a 60-second definition + a 2-minute real example from your own projects — that's the format senior interviewers actually reward.
- For the system design questions (#83–89), practice sketching on a whiteboard/paper: components, data flow, failure modes, and one explicit trade-off you're making.
- For the war-story questions (#90–99), pre-select 4–5 real incidents from your career and map each one to multiple questions above — you'll reuse the same stories across several answers.
