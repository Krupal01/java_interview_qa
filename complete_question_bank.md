# Java / Spring Boot / Microservices — Complete Interview Question Bank

This file is a **verbatim compilation** of every question from every source shared in this conversation. Nothing has been reworded, merged, or removed — each source is kept as its own section, in its original order, so it's copy-to-copy identical to the originals.

Sources included:
1. "Senior Java / Spring Boot Interview — Full Question Bank" document
2. "Given this stack..." stack-based question set document
3. The additional loose question list (JVM memory area ... managerial)
4. The four screenshot-based coding questions
5. Additional loose question list — Round 2 (DB migration ... org/dept/role/permission DB design)

---

## Source 1: Senior Java / Spring Boot Interview — Full Question Bank

*Prepared for a Senior Software Engineer (~10 YOE) interview loop*

**Note on the interviewers:** LinkedIn blocks automated scraping of profile pages, so I couldn't pull full experience/skills sections for either profile. The one signal I could confirm: the nishit-shah-146707153 profile is titled "Senior Software Engineer at Mastercard." I couldn't verify anything for the kdpatel profile — no match was confirmable. Given the Mastercard signal, I've weighted the scenario section toward payments/fintech-style concerns (transaction integrity, idempotency, high throughput, security/compliance) — treat that as an educated guess, not a fact about what they'll ask.

**How to use this:** For a 10-year-experience loop, interviewers rarely stay at "define X." Expect the pattern: define → probe internals → "why not the alternative?" → "tell me about a time you hit this in production." For every topic below, be ready to (1) explain it, (2) explain a failure mode/edge case, (3) tie it to a real project you shipped.

### 1. Core Java — Deep
1. Explain the Java Memory Model (JMM): happens-before, visibility, volatile vs synchronized.
2. Walk through JVM memory areas — heap, stack, metaspace, PC register, native method stack.
3. How does garbage collection actually work? Compare G1, ZGC, and Parallel GC — when would you pick each?
4. What causes a memory leak in Java despite GC? Give a real example (e.g., static collections, unclosed listeners, ThreadLocal misuse).
5. equals()/hashCode() contract — what breaks if you override one but not the other? What happens if you mutate a key used in a HashMap?
6. Difference between HashMap, ConcurrentHashMap, and Hashtable internals — how does ConcurrentHashMap achieve thread safety without locking the whole map (segment locking in Java 7 vs CAS/bins in Java 8+)?
7. How does String pooling work? Why is String immutable, and what's the performance implication of heavy + concatenation in loops?
8. Explain generics type erasure — why can't you do new T[] or instanceof T?
9. Checked vs unchecked exceptions — what's your team's actual policy, and do you agree with it?
10. finally vs try-with-resources — what happens if both the try block and the close() throw?
11. What's new/relevant from Java 11 → 17 → 21 you've actually used: records, sealed classes, pattern matching for switch, virtual threads (Project Loom)? Have you used virtual threads in production — what changed for your thread-pool sizing assumptions?
12. Difference between CompletableFuture, Future, and reactive (Mono/Flux) — when do you reach for which?
13. Explain the difference between shallow and deep class loading issues — how have you debugged a ClassNotFoundException/NoSuchMethodError from a dependency conflict?

### 2. Concurrency & Multithreading (deep-dive favorite)
1. Explain synchronized internals — monitor locks, biased/lightweight/heavyweight locking.
2. Producer-consumer implementation — how would you build it with BlockingQueue vs wait/notify vs Semaphore?
3. What is a deadlock, and how have you actually detected/diagnosed one in production (thread dumps, jstack)?
4. ExecutorService — how do you size a thread pool for a CPU-bound vs I/O-bound workload? What's the danger of Executors.newCachedThreadPool() in production?
5. ThreadLocal — what's the memory leak risk in a pooled-thread environment (e.g., app servers), and how do you avoid it?
6. Explain CountDownLatch vs CyclicBarrier vs Phaser with a real use case.
7. What is false sharing, and have you ever had to deal with it?
8. Scenario: A service under load starts throwing intermittent TimeoutException only during peak traffic. Walk me through how you'd diagnose whether it's thread starvation, GC pause, connection pool exhaustion, or downstream latency.

### 3. Spring Core / Spring Boot
1. Explain the Spring bean lifecycle end-to-end (instantiation → dependency injection → BeanPostProcessor → init → destroy).
2. Constructor injection vs field injection — why is constructor injection preferred at scale, and what does it protect against (circular dependencies, immutability, testability)?
3. How does Spring Boot auto-configuration actually work under the hood (@EnableAutoConfiguration, spring.factories / AutoConfiguration.imports, conditional annotations)?
4. Explain bean scopes: singleton, prototype, request, session — what's a real bug you've seen from injecting a prototype bean into a singleton incorrectly?
5. @Transactional — how does the proxy work, and why does a self-invocation call (calling an @Transactional method from within the same class) silently not get intercepted?
6. Explain propagation types (REQUIRED, REQUIRES_NEW, NESTED) with a real scenario for each.
7. How do you handle configuration across environments (dev/stage/prod) — profiles, @ConfigurationProperties, Spring Cloud Config, Vault?
8. What does Spring Boot Actuator give you in production, and which custom health indicators/metrics have you built?
9. AOP in Spring — how have you used it for cross-cutting concerns (logging, auditing, retry)? What are the proxy limitations (final methods/classes, self-invocation)?
10. Explain the difference between @Component, @Service, @Repository, @Controller beyond "just semantics" — what does @Repository actually add (exception translation)?

### 4. Data Layer — JPA / Hibernate / Transactions
1. N+1 query problem — how do you detect it and fix it (JOIN FETCH, @EntityGraph, batch fetching)?
2. Explain first-level vs second-level cache in Hibernate — risks of enabling second-level cache in a distributed/multi-instance deployment.
3. Lazy vs eager loading — what's LazyInitializationException and how do you avoid it cleanly (not just OpenSessionInView, which has its own trade-offs)?
4. Explain optimistic vs pessimistic locking — when have you used @Version to solve a real concurrency bug (e.g., double-processing a payment)?
5. How do you handle schema migrations safely in a live system with zero downtime (Flyway/Liquibase, expand-contract pattern)?
6. Explain isolation levels (Read Committed, Repeatable Read, Serializable) and a real phantom-read/dirty-read bug you've hit.
7. Scenario: Two threads/instances update the same row concurrently and you're seeing lost updates. How do you fix it — DB constraint, versioning, distributed lock, or redesign?
8. How do you paginate efficiently over a very large table without OFFSET performance collapse?

### 5. REST API Design
1. How do you design idempotent APIs — why does this matter especially for POST-based payment/transaction endpoints (idempotency keys)?
2. Versioning strategy for public APIs — URI versioning vs header vs content negotiation — trade-offs?
3. How do you handle partial failures in a request that fans out to 3 downstream services?
4. Explain HATEOAS — have you actually used it, or is it mostly theoretical in your experience?
5. Designing pagination, filtering, and sorting for a large collection endpoint — cursor-based vs offset-based, and why.
6. How do you version and evolve a contract without breaking existing clients (backward compatibility, consumer-driven contracts, Pact)?

### 7. Security
1. Explain the Spring Security filter chain end-to-end for a JWT-based API.
2. Access token vs refresh token — how do you handle token revocation for a stateless JWT approach (since JWTs can't be "revoked" by default)?
3. OAuth2 grant types — which have you actually implemented (authorization code, client credentials), and why is implicit grant deprecated?
4. How do you prevent common vulnerabilities in a Spring Boot app — SQL injection (parameterized queries/JPA), XSS, CSRF (and why CSRF protection differs for stateless APIs vs session-based apps)?
5. How do you secure service-to-service calls in a microservices mesh?
6. Given a fintech/payments context: how do you ensure PCI-DSS-relevant data (card numbers, CVVs) never lands in logs, and how do you handle tokenization/masking?
7. Rate limiting and abuse prevention on public-facing APIs — token bucket vs sliding window, where do you enforce it (gateway vs app)?

### 6. Microservices Architecture (breadth)
8. Monolith → microservices migration — what are the actual triggers that justify the split, and what's the first thing you decompose?
9. How do you handle distributed transactions across services — Saga pattern (choreography vs orchestration), compensating transactions?
10. Explain the Outbox pattern — why is it needed alongside message publishing to avoid dual-write inconsistency?
11. Service discovery and load balancing — Eureka/Consul vs Kubernetes-native (DNS + kube-proxy) — which have you used and why?
12. API Gateway responsibilities — routing, auth, rate limiting, request aggregation — what have you built with Spring Cloud Gateway/Kong/Zuul?
13. Circuit breaker pattern — Resilience4j vs Hystrix — how do you tune thresholds, and what's a real incident where a missing circuit breaker caused cascading failure?
14. How do you do centralized configuration and secret management across dozens of services (Spring Cloud Config, Vault, AWS Secrets Manager)?
15. Explain the CAP theorem in the context of a real design decision you made (choosing consistency vs availability).
16. How do you handle inter-service authentication (mTLS, service tokens, OAuth2 client-credentials)?
17. Explain event-driven architecture with Kafka — partitioning strategy, consumer group rebalancing, exactly-once vs at-least-once semantics, and how you've handled duplicate message processing.
18. How do you ensure message ordering when it matters (e.g., processing events for the same account/entity) in a partitioned topic?
19. What's your strategy for schema evolution in Kafka messages (Avro/Protobuf + schema registry) without breaking consumers?
20. Dead-letter queues — how do you design retry + DLQ handling so failed messages don't silently vanish or infinite-loop?

### 8. Performance, Caching, Scalability
1. How do you find and fix a slow endpoint in production — profiling tools, APM (New Relic/Dynatrace/Datadog), thread dumps, flame graphs?
2. Caching strategy — cache-aside vs write-through vs write-behind — Redis vs local (Caffeine/Guava) — when do you use each, and how do you handle cache invalidation/stampede?
3. How do you design a system to handle a 10x traffic spike (e.g., flash sale, batch settlement window) without falling over?
4. Connection pool tuning (HikariCP) — what metrics do you watch, and what's a real pool-exhaustion incident you've debugged?
5. How do you do capacity planning / load testing before a major release (JMeter/Gatling, and what SLAs did you validate against)?

### 9. Testing
1. Unit vs integration vs contract testing — what's your team's actual test pyramid look like in practice (not textbook)?
2. Mockito — mocking pitfalls you've seen (over-mocking, testing implementation instead of behavior).
3. Testcontainers — how have you used it to test real DB/Kafka interactions instead of mocking them away?
4. How do you test asynchronous/event-driven flows reliably (avoiding flaky sleep-based tests)?
5. What's your approach to testing @Transactional rollback behavior and idempotency logic?

### 10. DevOps / Cloud / Deployment
1. Walk through your CI/CD pipeline end-to-end — build, test gates, image build, deployment strategy, rollback trigger.
2. Blue-green vs canary deployment — trade-offs, and which have you actually run in production?
3. How do you handle zero-downtime deployments when the schema itself is changing (expand-contract migration pattern)?
4. Kubernetes — how do you configure readiness vs liveness probes correctly for a Spring Boot app (and what goes wrong if you get them backwards)?
5. How do you handle graceful shutdown so in-flight requests aren't dropped during a pod termination/scale-down?
6. Observability — how do you correlate a request across 6 microservices (distributed tracing — Sleuth/Zipkin/OpenTelemetry, correlation IDs)?
7. Structured logging strategy — what do you log, what do you deliberately never log, and how do you keep log volume/cost sane at scale?

### 11. System Design (senior-level, expect at least one full design question)
1. Design a payment/transaction processing system that must guarantee exactly-once processing and handle retries safely.
2. Design a rate limiter for an API gateway serving millions of requests/day.
3. Design a notification service that fans out to email/SMS/push with retry and dedup.
4. Design an idempotent order/transaction API where the client may retry the same request due to a network timeout.
5. Design a system for real-time fraud detection on transaction events (streaming, low latency, false-positive trade-offs).
6. How would you design a distributed unique ID generator (Snowflake-style) for transaction IDs across multiple regions?
7. Design a reconciliation system that compares two large datasets (e.g., internal ledger vs external settlement file) and flags mismatches.

### 12. Scenario / Behavioral-Technical (production war stories — very commonly asked at this level)
1. Tell me about the most critical production incident you've owned — what broke, how did you find root cause, and what changed afterward (postmortem culture)?
2. Describe a time you had to make a trade-off between shipping fast and doing it "right" — how did you decide, and what was the outcome?
3. Tell me about a time your service caused a cascading failure in another team's system — what did you learn about blast-radius design?
4. Describe a schema/API change you had to roll out without breaking existing consumers — how did you sequence it?
5. Tell me about a time you disagreed with an architectural decision from a senior engineer/architect — how did you handle it?
6. Describe a performance problem you diagnosed that turned out to be something non-obvious (not the code you first suspected).
7. Tell me about mentoring a junior engineer or leading a small team through a delivery — how did you balance hands-on coding vs unblocking others?
8. Describe a time you had to push back on unrealistic timelines from product/business — how did you frame the conversation?
9. Walk me through how you'd approach debugging a bug that only reproduces in production and not in staging.
10. Tell me about a design decision you made early in a project that you'd change if you started over — what did you learn?

### 13. Rapid-fire "wide but shallow" round (they often close with these to check breadth)
1. Difference between @RestController and @Controller.
2. What is @Qualifier used for, and when do you need it?
3. Difference between List, Set, Map — when would you use LinkedHashMap vs TreeMap?
4. What does @SpringBootApplication actually bundle (@Configuration, @EnableAutoConfiguration, @ComponentScan)?
5. Difference between PUT and PATCH.
6. What's the difference between @RequestParam, @PathVariable, and @RequestBody?
7. What does @Async do, and what's the gotcha with calling an @Async method from the same class?
8. Difference between SOAP and REST — have you had to maintain a legacy SOAP integration?
9. What's a WebFlux/reactive stack, and when would you not use it over traditional Spring MVC?
10. Explain application.yml profile-specific overrides and property precedence order.
11. What build tool do you use (Maven/Gradle) and why — any multi-module project experience?

**Suggested prep approach**
* For every "explain X" question, prepare a 60-second definition + a 2-minute real example from your own projects — that's the format senior interviewers actually reward.
* For the system design questions (#83–89), practice sketching on a whiteboard/paper: components, data flow, failure modes, and one explicit trade-off you're making.
* For the war-story questions (#90–99), pre-select 4–5 real incidents from your career and map each one to multiple questions above — you'll reuse the same stories across several answers.

---

## Source 2: Stack-Based Question Set ("Given this stack...")

Given this stack, here's a full interview question set organized by category — theory questions interviewers typically ask, plus coding problems tied to the same skills.

### OOP & SOLID Principles
* Explain all 5 SOLID principles with a real code example for each (not textbook definitions — actual violation → fix)
* How would you refactor a class that violates Single Responsibility?
* Give an example where you applied Dependency Inversion in a Spring Boot project
* Difference between polymorphism (compile-time vs runtime) with code
* Coding: Design a PaymentProcessor system where you can add new payment types (UPI, Card, Wallet) without modifying existing code — apply Open/Closed Principle

### Kotlin vs Java
* Null safety in Kotlin (?, !!, ?:) vs Java's Optional/NPE handling
* Data classes vs POJOs — what does Kotlin generate automatically?
* val vs var, coroutines vs Java threads (basic difference)
* Extension functions — what problem do they solve?
* Coding: Write a function in Kotlin using ?.let{} to safely chain nullable calls

### Spring Boot / Spring MVC / Spring Security
* How does Spring Boot auto-configuration work internally (@Conditional, spring.factories/AutoConfiguration.imports)
* Bean scopes, bean lifecycle, @PostConstruct/@PreDestroy
* Spring Security filter chain — how does a request flow through it?
* How do you implement JWT-based authentication in Spring Security (filter, token validation, SecurityContext)?
* @Transactional — propagation types (REQUIRED, REQUIRES_NEW), isolation levels, what happens with self-invocation (proxy issue)
* Coding: Implement a custom OncePerRequestFilter to validate JWT tokens

### Microservices & APIs
* REST vs GraphQL vs gRPC — when would you choose each? (gRPC for internal high-perf service-to-service, GraphQL for flexible client queries, REST for simplicity/public APIs)
* What does an API Gateway do — routing, auth, rate limiting, aggregation
* How do you handle service-to-service communication failures? (Circuit breaker — Resilience4j, retries, fallback)
* SOAP vs REST — why would a system (like payments) still use SOAP?
* How do you version a REST API?
* Design question: How would you design an inter-service communication pattern for an order + payment + inventory microservice setup? (sync REST vs async Kafka — trade-offs)

### Kafka / Messaging / Event-Driven Architecture
* Kafka core concepts: topic, partition, consumer group, offset, replication factor
* How does Kafka guarantee message ordering? (only within a partition)
* At-least-once vs exactly-once vs at-most-once delivery — how do you achieve each?
* Difference between Kafka and RabbitMQ — when to use which (Kafka: high throughput/event streaming/replay; RabbitMQ: complex routing, lower latency per message, task queues)
* How do you handle duplicate message processing (idempotency)?
* Coding/design: Design an event-driven flow for "Order Placed → Inventory Updated → Payment Processed → Notification Sent" using Kafka topics

### Databases (PostgreSQL/MySQL/Oracle + MongoDB + Redis + Elasticsearch)
* ACID properties with examples
* Indexing — B-tree index, when does an index NOT help (low cardinality columns)?
* N+1 query problem in JPA/Hibernate — how do you fix it? (JOIN FETCH, @EntityGraph)
* SQL vs NoSQL — when would you pick MongoDB over PostgreSQL?
* Redis use cases: caching, session store, distributed locks, rate limiting
* Elasticsearch — inverted index concept, when to use it over SQL LIKE queries
* Coding: Write a JPQL/native query to find N+1 problem and fix using @EntityGraph
* Coding: Implement a Redis-based distributed lock (or cache-aside pattern) in Java

### Hibernate/JPA
* First-level vs second-level cache
* @OneToMany vs @ManyToMany mapping, fetch = LAZY vs EAGER
* Difference between save(), saveAndFlush(), persist(), merge()
* What is the Hibernate dirty checking mechanism?

### Cloud & DevOps (AWS, Docker, Kubernetes, CI/CD)
* Difference between EC2, Lambda, ECS — when serverless (Lambda) vs container (ECS/EKS)
* How does an SQS queue help decouple microservices?
* Docker: image vs container, multi-stage builds — why use them?
* Kubernetes: Pod, Deployment, Service, ConfigMap/Secret — how does a Service discover Pods?
* How do you set up a CI/CD pipeline in Jenkins/GitLab for a Spring Boot app (build → test → Docker image → push to JFrog Artifactory → deploy to K8s)?
* How do you manage secrets (DB passwords, JWT secret) in Kubernetes/AWS securely?

### Security (OAuth2, JWT, SSL/TLS)
* OAuth2 grant types — Authorization Code vs Client Credentials — when to use which
* JWT structure (header.payload.signature) — how is it verified without a DB call?
* How do you handle JWT token expiry/refresh token flow?
* SSL/TLS handshake — brief walkthrough
* Coding: Write a method to generate and validate a JWT using io.jsonwebtoken (jjwt) library

### Testing
* Mockito: @Mock vs @MockBean vs @Spy
* How do you write an integration test for a REST controller with @SpringBootTest + MockMvc or TestRestTemplate?
* How do you mock a Kafka producer/consumer or an external REST call in a unit test?
* Coding: Write a JUnit 5 + Mockito test for a service method that calls a repository and an external Feign client

### System Design (likely for SE2 level)
* Design a payment transaction system with idempotency (very relevant to Mastercard) — how do you prevent double charging on retry?
* Design a rate limiter for an API Gateway
* Design a notification service using Kafka + microservices
* How would you scale a service handling millions of transactions/day — caching, DB read replicas, async processing, partitioning

### Core Java
* Explain differences between abstract class and interface (Java 8+ default methods).
* How does garbage collection work? Explain generational GC.
* Explain equals()/hashCode() contract and where it breaks things.
* What are checked vs unchecked exceptions? When to use each?
* Explain multithreading basics: synchronized, volatile, ExecutorService.
* What's new in Java 17/21 (records, sealed classes, virtual threads)?

### Secure Coding (OWASP/CWE)
* Name the OWASP Top 10 and how you'd mitigate SQL injection, XSS, CSRF.
* How do you prevent insecure deserialization in Java?
* What is input validation vs output encoding?
* How do you handle secrets/credentials in microservices (vault, env vars)?
* Explain SEI CERT coding guidelines you've applied.

### Design Patterns
* Explain Factory, Singleton, Adapter, Composite, Observer, Strategy patterns with real examples.
* What is Inversion of Control / Dependency Injection? How does Spring implement it?
* When would you use Strategy vs Factory pattern in a microservice?

### Microservices Architecture
* How do microservices communicate (REST, gRPC, messaging)?
* Explain event-driven architecture vs request-response.
* How do you handle distributed transactions (Saga pattern)?
* What is service discovery? How does it work (Eureka, Consul)?
* Explain circuit breaker pattern (Resilience4j/Hystrix) and why it's needed.
* How do you handle API versioning?
* Explain API Gateway's role.
* How do you design idempotent APIs?

### Testing
* Difference between unit, integration, and service-level tests.
* What tools have you used for mocking (Mockito, WireMock)?
* How do you measure code coverage, and what's a good target?
* Explain contract testing (Pact) in microservices.

### Code Quality Tools
* What is SonarQube used for? How do you fix code smells it flags?
* Have you used Checkmarx or similar SAST tools? Walk through a finding you remediated.
* What is Zally, and how is it used for API linting/governance?

### Version Control / SDLC
* Explain Gitflow branching strategy.
* What's your peer review process — what do you look for in a PR?
* Compare Waterfall, Scrum, Kanban, SAFe — when would each be used?
* How do you estimate effort/story points?

### CI/CD & DevOps
* Walk through a CI/CD pipeline you've built (tools: Jenkins, GitLab CI, GitHub Actions).
* Explain Docker basics: image vs container, Dockerfile layers.
* What is Kubernetes and how does it help with container orchestration?
* How do you handle blue-green or canary deployments?
* Explain Infrastructure as Code (Terraform, Ansible).

### System Design / Engineering Principles
* How do you ensure a service is highly available and fault-tolerant?
* Explain caching strategies (Redis, local cache) in microservices.
* How do you handle logging/monitoring/observability (ELK, Prometheus, Grafana)?
* How do you secure service-to-service communication (mTLS, OAuth2/JWT)?

### Documentation
* How do you document APIs (Swagger/OpenAPI)?
* What makes good technical documentation for services you own?

---

## Source 3: Additional Loose Question List

JVM memory area

Counter in synchronize method, multiple thread working on it but still producing wrong totals, what are the possible cause

How leak memory in java application even JVM has GC , give real example

Loggin, alert, production failure, monitoring

How you handled duplicate msg from produce in kafka,

How you troubleshoot kafka consumer that is unable to consume message

How you handle kafka consumer that continues failing while processing message

What is actuator, how it is work.

How auto configuration worked internally , beans on condition, auto configuration.import file

Service throwing connection pool exhausted error in production, how would you diagnosis

Any functional difference between, @controller, @service @repository

Functional interface, default method, optional class, Stream API, parallel Stream, advancetage of parallel stream, how parallel stream work on memory CPU and alls ,

Executor framework, Exception handling, Exception type, throw vs throws,

Spring vs springboot,

Webserver in springboot

Microservice architecture

Explain project architecture and work flow, kafka,

Deployment and AWS deep, Jenkins, script, pipeline, policies,

advantage and disadvantage of micro service, and monolithic architecture

Clouds skill and rule

IOC and DI relationship

Desing pattern

IOC and DI relation

How cache working internally ?

Control statement in java

Control flow statement in java

ELK implementation

Prevent common application valnaribilty, like css, xors, csrf, how csrf work in sessionn based api

How would you identify and resolve performance bottlenecks in a Java application?

How to optimize query, DB projection, polling, how to find which query is slow, SQL vs NOSQL, disadvantage of both Postgres and mongodb

How to get response bake in kafka, like payment -> invoice -> generate

* What are Cursor and Trigger? When to use trigger?
* What is polymorphism? Explain JVM's role in Dynamic Polymorphism.
* How rows are stored in Databases. At which physical location it is stored? And how they're inter-relating with each other.
* Why the size of the array is fixed? Think from the perspective of JVM. And state JVM's role in it. What needs to be changed to make the array dynamic?
* Which object-oriented concepts are supporting polymorphism and how?

-> How leetcode works? If you have to develop a coding platform what will be your approach

```
I/p: 1,2,1,2,3,4,5,3,2,4,6,8,9,10,9,8,7,10,12,14,15,16,17,18,19,10,5,2
Output:  2 8 6   ->(Start Index, End Index, Width of Mountain)
```

* What is SSL? What is an SSL certificate?
* What is HTTP protocol and why it is used for communication between server and client? Any other we have
* What do you use for logging? How do you configure it?
* What is AWS EC2 instance and how to deploy jar in it?
* Difference between SQL and NoSQL. And how to decide when to use SQL or NoSQL,
* We have an employee table with attributes: first name, Lastname, and embed. Two admin want to change employees with the same id at the same time.
* What is Java Thread Pool? How it works.
* What are your 3 weak and 3 strong points?
* How will you explain microservices to an 8-year-old kid?
* problem on number-to-words conversion.

Collection class, Like I have custom user object and I have to add two same type of object in set like new USer("a", 12) and new User("a", 12), so set will allow me to add ?
How or what can we use to do this

What is gRPC, main difference in SOAP and REST other that xml and json ? Strong point

What is mock and sub , other methods

How to notify frontend, like tell frontend like this task is complete…

Instance block and other things on class, interface , on which

How you implemented SAGA, like for choreography we have kafka , so what is for Orchestration

CQRS and it's drawback , how would you achieve consistency In this , specially in read and write

What are bare minimum configuration to write kafka consumer

Consumer rebalancing

Leader follower pattern, why

Kafka stream api

How you publish application to perticular topic

Republish msg to kafka topic

Send() is sync or asynchronous

Before java 8, how can we do parallel programming

Why future is blocking

@ConfigurationProperties

Ham charts in kubrnatis

How Trigger and develop lambda

What is terraform and how you used it

Basic command for terraform

How to increate intense in kubernatis, by script and y CLI

How you used kubernatis in current project

Whole process for current project, for rogers, SNB and old projects

CICD tools we used in our project

Enum singleton

Pick in stream

Parellalel stream vs sequesntial stream

LiskList, tree, graph, and various algorithms and implementation

What is TLS and SSL and how to implement for API security in first stage,

How to avoid dead lock

How did you debug out of memory exception and how to solve

Heap dump analysis

Dead lock example how to fix it

How to optimize SQL query

SQL explain join and indexing to optimize

Having vs where in sql

Diff joins in sql

@post construct

Main usages of flat map and map

We can access lisklist by index but why array is faster the linklist

Benefits of marker interface

Lambda service, how server less, triggering , EC2, SQS, SNS, how to deploy 2-3 ways,

How to handle production issue

How you decide instance of service ?

performance testing

StringPoolTrick

CompletableFuture

Constructor can be final ?

Assert property

Hash table

Why function interface don't have more than one abstract method

getNotify() and notifyAll()

Sync and asynchronous communication real time example

Kafka partition and it's use, why we have partition

Patition consumer relation, same consumer with multiple partition ? what if more consumer then partition

Joins in sql, sql vs no sql db

Getting diff event for orders, have some service to handle that order, after service we have to create a bill. How to design this system, one order can have multiple service and all service has UID., what we have 2 type of amount, like amount from service and calculate amount,

create sql query with optional filter, date range, ascending and DESC order, range of date, and order to service joins, and customer id, also pagination and batching

Java specifications, how to decide DB indexing, composite index, order of indexing, at a time two index can be used or only one,

Reverting operation or transaction on failure of other service

We have parent method which is handling 2 child method, if second call fail then how can we revert first method ? Which type of annotations we can use, how can we use. Why we should use?

How to define annotations on all 3 methods, if what, I don't want to rollback but have to store state on failure.

Pessimistic locks and optimistic locks with DB, how to impolement. If we have pessimistic lock, can other thread read that row at a time ? And what for optimistic lock, like how it behave when 2 thread trying to access it.

Complete authorization and authentically flow, with class and method access

REST, GraphQL, stateless and stateful, SOAP, response codes

flow of springboot application, annotations, methods, architecture, diff modulers and micro service.

JPA vs hibernate, internal methods, naming, how to use

Design patterns, how you used , implement, where you used , why we need

Solid principle, why, where, how

How you store secure details in DB, and how you store username and password in DB

managerial

### CODING

Next palindrome in 100 to 1000, or next palidrome for 123 or 124 , ask for this is 131

rotate list

shift all null or 0's in string list or int list

find second highest/lowest using steam

---

## Source 4: Screenshot-Based Coding Questions

### 4.1 — Special Numbers in an Array

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

### 4.2 — Predict the Output: Comparator + List\<Object\>

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

### 4.3 — Predict the Output: `class sample` with Optional

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

### 4.4 — ReentrantLock / ExecutorService Non-Blocking Check

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

## Source 5: Additional Loose Question List — Round 2

*Only questions that were **not** already present in Sources 1–4 are listed here. (The OOP/SOLID + Kotlin + Spring + Kafka + DB + Cloud + Security + Testing + System Design stack set, the "JVM memory area ... managerial" list, the Cursor/Trigger + polymorphism + SSL + EC2 + thread pool block, and the coding list were already captured above and are not repeated.)*

### 5.1 — Database, Persistence & Migration

* DB migration
* Column validation changes in DB
* Postgres and NoSQL DB
* RDBMS
* External SQL framework
* How JPA works internally
* Soft delete vs hard delete
* DB locks, and too much DB operation in a single transaction — how do you handle that?
* 2 services updating the same DB row — how do you handle it?
* What is DB sharding?
* How do you handle multiple databases in a single service?
* Migrating an application to a new DB without downtime
* How do you develop schema with the new and old way (backward-compatible schema evolution)?
* Count(*) vs count(1) vs count(column)
* Normalisation
* How do you handle simultaneous DB calls on a single row in the trip table?
* Should we keep rider/driver id directly in the trip table or make joins from trip to rider and driver tables — why use this relationship?
* Lazy fetching vs eager fetching — which is the better approach?
* Design a DB for org, department, user, role, permissions, and exceptional permissions
* Aggregation stages in MongoDB
* What if `@Transactional` is not working — what could be the reasons?
* `@Transactional` on a private method
* `@Transactional` internally — how does the proxy mechanism work?
* What happens with a checked exception in a transaction — does it continue (safe) or roll back?
* What exactly is the proxy in a transaction?
* How transactions work, how can you create one yourself, and how does it roll back?
* DB connection methods — save, commit, flush, etc.
* How do you handle transactions in production?
* JPA repository internals
* How does Hibernate work — all the terms

### 5.2 — Spring / Spring Boot Internals

* CORS
* TTL, cache key, cache vector — custom configuration
* What happens if we override configuration?
* What is a profile?
* How do you activate a profile?
* AOP — what is it and where/why do we use it, with a use case
* Bean factory vs application context
* Async in Spring
* WebFlux
* Why is field injection a bad injection style?
* How do you optimize application startup time?
* How do you change the server in Spring (external server instead of Tomcat)?
* What is ResponseEntity?
* What happens in the background when we start a Spring application?
* Role of `SpringApplication.run()`
* How does auto-configuration work internally, and how does it decide what to enable?
* Actuators and how to change the actuator port
* How do you manage service/management endpoints — what if we want to add new ones?
* `@ControllerAdvice` exception checking
* `@Data` — when is it safe and when not? How can it create bugs silently?
* IoC storage and DI internal working
* Bean init and working, scanning, injection
* Spring Security layers and filters
* Spring Boot application flow
* How can we remove a cyclic dependency between services?
* Spring Batch — why do we need it, and what is a pipeline?
* I want to read a sheet with 2M rows and store it in MongoDB — how do you build that with Spring Batch?
* Few RestTemplate methods used in a project — how does PATCH work with RestTemplate?
* `@ConfigurationProperties`
* Are `@Controller`, `@Service`, `@Repository` singleton by default?
* Can we use `@Component` instead of `@Service` and `@Repository`?
* Integration test vs unit test
* What annotations do you use for testing, and how have you used Mockito?

### 5.3 — Core Java, JVM & Memory

* Load factor of a HashMap
* Can a HashMap cause a memory leak?
* When does volatile fail?
* Reflection in Java
* Comparator vs Comparable
* Print to console without a main method
* Can we have a static block — and what is a static block?
* Can we have multiple finally blocks? If yes, which one runs?
* What type of collection have you used in your project — give a real purpose for using it
* Can we set a class object as a key in a HashMap?
* Can a functional interface extend another interface?
* How can we prevent singleton cloning?
* Can we override the main method, or overload it?
* Island of isolation
* Why override finalize? Do we have finalize in a record class? Do record classes have methods?
* How does cleanup work in GC?
* Which thread performs GC? What is a daemon thread, and is it only for GC?
* Marking, deletion and compaction
* Generational GC strategy and how age is calculated
* "Die young" hypothesis
* Heap areas for GC — young generation, tenured, permanent generation
* Eden space, survivor space
* What is promotion?
* Minor GC vs major GC
* Why can't we use (volatile) on local variables and static fields?
* Does overloading only work for objects?
* Idempotent and safe operations
* How do you make a class thread-safe and a collection thread-safe?
* How does `new` work internally?
* Role of the constructor
* How is Java pass-by-value and not pass-by-reference?
* Can a singleton instance be serialized? What is serialization, and how do you ensure it can be serialized?
* Why does Java HashMap use a red-black tree? Why convert to a red-black tree, what is the time complexity, and why change from linked list to red-black tree?
* Constructor in an abstract class — why do we have one even though we don't create objects of abstract classes? Can we make it private — what compilation issues arise?
* Why are Strings considered good keys for a map?
* Object class methods
* Normal thread creation vs Executor class
* Immutable class — name any immutable class
* Custom immutable class
* How to break a singleton
* Hashmap vs LinkedHashMap vs Hashtable
* Relation between hashCode() and equals()
* Benefits of a marker interface
* How does the compiler force a class to implement all methods of an interface?
* How do overriding and overloading work internally? What is late (lazy) binding?
* Class and interface relationship — Object, class, interface, abstract class hierarchy
* Why `@Override` annotation — is it mandatory?
* Constructor, init block — what happens when we put a return type in a constructor?
* What is finalize in the Object class?
* Parent-child relationship across classes, interfaces and abstract classes
* Lower bound, upper bound, generics
* Static keyword
* Use of method area, native memory and PC registers
* Access modifiers and their rules, hierarchy, parent-child rules
* Exceptions and their rules — parent-child
* Constructor rules — parent-child, instance block, static block
* Streams internally
* Why `@FunctionalInterface` annotation — is it mandatory?
* Why can't a functional interface have more than one abstract method?
* Pattern matching and method references — how do they work internally?
* Switch statement internally
* Record class — constructor, methods, static members; can it participate in inheritance?
* Sealed class — with state, inheritance, static, singleton
* Checked and unchecked exceptions at compile stage — how does the compiler check and force handling of checked exceptions?
* Can we use abstract and final together on a class?
* Why ConcurrentHashMap after Hashtable?
* Second largest number in an array using Streams
* Peek in Stream
* Parallel stream vs sequential stream
* Main usages of map vs flatMap
* LinkedList, tree, graph — various algorithms and implementations
* We can access a LinkedList by index, so why is an array faster than a LinkedList?
* Can a constructor be final?
* Assert property
* `getNotify()` / `notify()` and `notifyAll()`
* StringPool trick
* Enum singleton
* Why do we need a singleton, and what is not a singleton?
* Static instance and constructor for a class
* Instance block and other members — on a class vs on an interface, which is allowed where?

### 5.4 — Concurrency, Threads & Virtual Threads

* Lock striping and object pooling — how do they help us?
* Sync vs asynchronous
* Distributed locking
* Real scenario where a deadlock happened in production for a banking system
* How do you detect a deadlock in production?
* Virtual thread limitations — how do you control them? What happens if we create too many virtual threads? There is no thread pool for virtual threads, so does it affect resources, memory, or anything else?
* What happens if we create a separate virtual thread for each request?
* Can virtual threads cause connection pool overload?
* What is Project Loom?
* CPU → OS thread → platform thread → virtual thread relationship
* How do you handle ExecutorService and its parameters in a production system — how do you decide pool size?
* What is a thread dump and a heap dump, and how do you monitor application health?
* How do you find a blocked thread?
* How does CompletableFuture work internally?
* Why is Future blocking?
* Before Java 8, how could we do parallel programming?
* How do concurrent data structures work — ConcurrentHashMap, concurrent set, CopyOnWriteArrayList?
* Collections internal working — map, set, list, ArrayList
* How does a map know a key is unique? Same for set
* How does ArrayList know it needs to increase its size — if we create an ArrayList with 2 elements vs 100 elements, at which point and how does it know to resize?
* How does a thread pool work — what happens with no tasks and with too many tasks?
* We don't have a thread pool for virtual threads, so how do we control virtual threads, and is there any option to reuse them?
* How does CAS work for objects, how does volatile work for objects, and how do Atomic classes work?
* How does synchronization work in Java?
* Can 2 threads execute on the same object simultaneously — if there is no locking? If yes, how?
* Counter in a synchronized method with multiple threads still producing wrong totals — what are the possible causes?
* How to check thread dump and heap dump in production?
* How to avoid deadlock

### 5.5 — Microservices, Resilience & Communication

* Circuit breaker and different patterns
* How do you implement a timeout in a circuit breaker?
* Custom settings for a circuit breaker — sliding window, threshold, timeout, wait duration, etc.
* How do you implement a fallback mechanism in a circuit breaker, how does it work internally, and how does it trigger the fallback?
* What if one service is slow in the circuit — how can we solve it when we are passing the result to another service?
* If one microservice crashes, how do other services detect it?
* What is latency in microservices and how do you handle it?
* A service uploads a large file — how do you handle it?
* ACL (anti-corruption layer) pattern
* Sidecar pattern
* Blue-green deployments
* Scaling for high traffic
* How do you handle a service that has trillions of requests of traffic?
* How do you handle 100k requests per second?
* If we have 50M requests per second, which part of the service should we focus on more — consistency handling, traffic handling, DB connection pool, locking, asynchronous processing, query issues?
* Fault tolerance in microservices
* How do you handle traffic?
* Feign client vs REST client
* How do you make sure all instances share the load evenly?
* How do you decide the DB and strategy to use — separate DB for each microservice vs shared DB?
* Distributed transactions and the SAGA pattern
* How do you handle dependent microservices and their failure?
* Why would we need to shift from microservices back to a monolith?
* Convert a monolith to microservices
* How does an API gateway know which APIs map to which service, and what other features do we use in the gateway?
* BFF (backend for frontend) communication
* Eventual consistency, fault tolerance, and fault tolerance with Kafka
* Difficulties in microservices
* Microservice design patterns
* How do you decide the number of instances of a service?
* Leader-follower pattern — why?
* CQRS and its drawbacks — how would you achieve consistency, specially between read and write?
* How did you implement SAGA — for choreography we have Kafka, so what do we use for orchestration?
* Idempotency — what is it and how did you implement it in your project?
* 2 APIs, one sending a proper response object and another sending direct text — what is the difference between the two?
* HTTP status code 3xx — and can we add custom codes like 600–700?
* Service protection from failure of another service — communication strategies

### 5.6 — Security

* How do you secure REST APIs?
* How do you handle common vulnerabilities like SQL injection?
* How do you handle service secrets and external configuration?
* How do you handle all DB URLs and other configuration in a centralized way?
* How do you handle security — microservice-to-microservice security (deep and clear answer)?
* What type of data does a JWT hold — is a JWT encrypted or just encoded?
* Other options instead of JWT, and why JWT?
* What is TLS and SSL, and how do you implement it for API security in the first stage?
* Complete authorization and authentication flow, with class- and method-level access
* How do you provide security to a REST API — session management, JWT, certificates, roles? Complete implementation in steps (no code needed)
* Security checks and additional security layers on top for a service
* How do you store secure details in a DB, and how do you store username and password in a DB?
* How does CSRF work in session-based APIs?

### 5.7 — Kafka & Messaging

* Kafka vs RabbitMQ
* Where do you set topics, partitions and all other settings?
* Kafka consumer side and producer side configuration
* What are the bare-minimum configurations to write a Kafka consumer?
* Consumer rebalancing
* Consumer group vs consumer
* 10 partitions and 20 consumers — what is the behavior?
* Schema registry in Kafka — how do you handle versioning in Kafka?
* Kafka Streams API
* How do you publish from an application to a particular topic?
* Republish a message to a Kafka topic
* Is `send()` sync or asynchronous?
* Where have you used sync and async communication — use cases
* Report generation and report publishing — sync vs asynchronous tasking with Kafka events
* How do you get a response back in Kafka, like payment → invoice → generate?

### 5.8 — Performance, Caching & Production Issues

* Cache
* How do you handle cache with time-based update and forced update?
* How does caching work internally?
* Memory optimization because the service runs out of memory
* How do you optimize JVM startup time?
* How do you optimize your code?
* How do you check for crashes and failures?
* Application is slower in production compared to dev — what could be the issue?
* What if `@Service` is put on a repository class and `@Repository` on a service class?
* APIs work fine in Postman but not from the site for the frontend developer — what would be the issue?
* API is suddenly slow — what will you check first and how do you handle it?
* Production issue at midnight — how do you handle and investigate it?
* DB CPU is low but API latency is high — what could be the reason?
* A critical service goes down during business hours — what steps do you take?
* One production issue and how you fixed it, and the most difficult bug you solved
* How did you debug an OutOfMemoryError and how did you solve it?
* Heap dump analysis
* Deadlock example and how to fix it
* How to optimize a SQL query
* SQL EXPLAIN, joins and indexing to optimize
* HAVING vs WHERE in SQL
* Different joins in SQL
* How do you decide DB indexing, composite index, order of indexing — can two indexes be used at a time or only one?
* How do you optimize queries, DB projection, polling — how do you find which query is slow?
* Performance testing
* Logging, alerting, production failure and monitoring
* ELK implementation
* What is Splunk, how do you use it and why?
* What does Sonar check in your project and how did you use it?

### 5.9 — Cloud, DevOps & CI/CD

* Kubernetes and Jenkins for deployment
* Virtual machine vs Docker
* Why is Docker used?
* How does Docker help run a container on all types of machines — how does it work?
* How do you manage all containers of multiple services, and the commands?
* What is Kubernetes and what are its commands?
* Where are Docker images stored — versioning and ECS
* Docker image versioning
* How do you create a Docker image?
* Docker volume
* Kubernetes ConfigMap
* Helm charts in Kubernetes
* How do you increase instances in Kubernetes — by script and by CLI?
* How have you used Kubernetes in your current project?
* Steps for CI/CD
* CI/CD tools used in your project
* How do you follow different modules in the pipeline — explain the complete flow of your pipeline
* Different deployment strategies — blue-green, canary, etc.
* Which AWS service can we use to deploy?
* How can we use Docker for deployment?
* How can we manage environments, secrets and load balancers?
* How do you manage common properties and secrets?
* How do you trigger and develop a Lambda?
* Lambda service — how is it serverless, triggering, EC2, SQS, SNS, and 2–3 ways to deploy
* What is Terraform and how have you used it?
* Basic commands for Terraform
* IaC — infrastructure as code
* Automation testing
* Merge vs rebase — how do you resolve conflicts?
* Challenges in migrating to a new Spring and Java version
* Whole process for the current project and previous projects

### 5.10 — Design, Architecture & Scenario Design

* Design patterns — why used and why important? Which pattern solves which problem?
* Why and which design pattern did you use?
* How do you implement SOLID?
* IoC and DI relationship
* REST vs SOAP
* What is gRPC?
* How do we validate a request body across multiple URL versions?
* Can we do overloading in a controller to support versioning and backward compatibility for a large user base?
* v2 makes one extra DB call but v1 does not — how do we handle this for backward compatibility?
* We have a ride booking application with millions of riders — how can we find the nearest riders from a pickup location (with JPA and SQL)?
* We have a large-scale application that needs some real-time data in multiple regions — how can we manage it in a timely manner across regions?
* UTC concept in a large-scale multi-region application — how do we use LocalTime, and how does LocalTime work for India and America at the same time?
* We have an offer table that stores promo codes — how many fields would you add, and how do we enable a code only for a particular day or the first week of every month? Is storing all promo codes in the DB a good approach, or is there an alternative?
* How do you track a driver and a trip — should we store everything in the DB? For a large application that would be too many DB queries
* If 2 people at the same cafe book a ride and both get the same rider on the first attempt, will it book the same rider for two trips — how do you handle it?
* How do you notify the frontend that a task is complete?
* How do we handle reverting an operation or transaction on failure of another service?
