# Java / Spring Boot / Microservices — Complete Interview Question Bank

A **stack-wise consolidated compilation** of every question collected so far. All sources (the senior Java/Spring full question bank, the stack-based set, both loose question lists, and the screenshot coding questions) have been merged and regrouped by technology stack instead of by source. Question wording is kept as in the originals; only exact duplicates were collapsed. Questions only — no answers.

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

1. How does String pooling work? Why is String immutable, and what's the performance implication of heavy + concatenation in loops?
2. Explain generics type erasure — why can't you do new T[] or instanceof T?
3. Difference between polymorphism (compile-time vs runtime) with code
4. Explain differences between abstract class and interface (Java 8+ default methods).
5. Control statement in java
6. Control flow statement in java
7. What is polymorphism? Explain JVM's role in Dynamic Polymorphism.
8. Which object-oriented concepts are supporting polymorphism and how?
9. Instance block and other things on class, interface , on which
10. Benefits of marker interface
11. StringPoolTrick
12. Constructor can be final ?
13. Assert property
14. Reflection in Java
15. Print to console without a main method
16. Can we have a static block — and what is a static block?
17. How can we prevent singleton cloning?
18. Can we override the main method, or overload it?
19. Does overloading only work for objects?
20. How does `new` work internally?
21. Role of the constructor
22. How is Java pass-by-value and not pass-by-reference?
23. Can a singleton instance be serialized? What is serialization, and how do you ensure it can be serialized?
24. Constructor in an abstract class — why do we have one even though we don't create objects of abstract classes? Can we make it private — what compilation issues arise?
25. Object class methods
26. Immutable class — name any immutable class
27. Custom immutable class
28. How to break a singleton
29. Benefits of a marker interface
30. How does the compiler force a class to implement all methods of an interface?
31. How do overriding and overloading work internally? What is late (lazy) binding?
32. Class and interface relationship — Object, class, interface, abstract class hierarchy
33. Why `@Override` annotation — is it mandatory?
34. Constructor, init block — what happens when we put a return type in a constructor?
35. What is finalize in the Object class?
36. Parent-child relationship across classes, interfaces and abstract classes
37. Lower bound, upper bound, generics
38. Static keyword
39. Access modifiers and their rules, hierarchy, parent-child rules
40. Constructor rules — parent-child, instance block, static block
41. Switch statement internally
42. Can we use abstract and final together on a class?
43. Can a constructor be final?
44. Why do we need a singleton, and what is not a singleton?
45. Static instance and constructor for a class
46. Instance block and other members — on a class vs on an interface, which is allowed where?

## JVM Internals, Memory Model & Garbage Collection

47. Walk through JVM memory areas — heap, stack, metaspace, PC register, native method stack.
48. How does garbage collection actually work? Compare G1, ZGC, and Parallel GC — when would you pick each?
49. What causes a memory leak in Java despite GC? Give a real example (e.g., static collections, unclosed listeners, ThreadLocal misuse).
50. Explain the difference between shallow and deep class loading issues — how have you debugged a ClassNotFoundException/NoSuchMethodError from a dependency conflict?
51. How does garbage collection work? Explain generational GC.
52. JVM memory area
53. How leak memory in java application even JVM has GC , give real example
54. Island of isolation
55. Why override finalize? Do we have finalize in a record class? Do record classes have methods?
56. How does cleanup work in GC?
57. Which thread performs GC? What is a daemon thread, and is it only for GC?
58. Marking, deletion and compaction
59. Generational GC strategy and how age is calculated
60. "Die young" hypothesis
61. Heap areas for GC — young generation, tenured, permanent generation
62. Eden space, survivor space
63. What is promotion?
64. Minor GC vs major GC
65. Use of method area, native memory and PC registers

## Collections & Data Structures

66. equals()/hashCode() contract — what breaks if you override one but not the other? What happens if you mutate a key used in a HashMap?
67. Difference between HashMap, ConcurrentHashMap, and Hashtable internals — how does ConcurrentHashMap achieve thread safety without locking the whole map (segment locking in Java 7 vs CAS/bins in Java 8+)?
68. Difference between List, Set, Map — when would you use LinkedHashMap vs TreeMap?
69. Explain equals()/hashCode() contract and where it breaks things.
70. Why the size of the array is fixed? Think from the perspective of JVM. And state JVM's role in it. What needs to be changed to make the array dynamic?
71. Collection class, Like I have custom user object and I have to add two same type of object in set like new USer("a", 12) and new User("a", 12), so set will allow me to add ?
72. How or what can we use to do this
73. LiskList, tree, graph, and various algorithms and implementation
74. We can access lisklist by index but why array is faster the linklist
75. Hash table
76. Load factor of a HashMap
77. Can a HashMap cause a memory leak?
78. Comparator vs Comparable
79. What type of collection have you used in your project — give a real purpose for using it
80. Can we set a class object as a key in a HashMap?
81. Why does Java HashMap use a red-black tree? Why convert to a red-black tree, what is the time complexity, and why change from linked list to red-black tree?
82. Why are Strings considered good keys for a map?
83. Hashmap vs LinkedHashMap vs Hashtable
84. Relation between hashCode() and equals()
85. Why ConcurrentHashMap after Hashtable?
86. LinkedList, tree, graph — various algorithms and implementations
87. We can access a LinkedList by index, so why is an array faster than a LinkedList?

## Concurrency, Multithreading & Virtual Threads

88. Explain the Java Memory Model (JMM): happens-before, visibility, volatile vs synchronized.
89. Difference between CompletableFuture, Future, and reactive (Mono/Flux) — when do you reach for which?
90. Explain synchronized internals — monitor locks, biased/lightweight/heavyweight locking.
91. Producer-consumer implementation — how would you build it with BlockingQueue vs wait/notify vs Semaphore?
92. What is a deadlock, and how have you actually detected/diagnosed one in production (thread dumps, jstack)?
93. ExecutorService — how do you size a thread pool for a CPU-bound vs I/O-bound workload? What's the danger of Executors.newCachedThreadPool() in production?
94. ThreadLocal — what's the memory leak risk in a pooled-thread environment (e.g., app servers), and how do you avoid it?
95. Explain CountDownLatch vs CyclicBarrier vs Phaser with a real use case.
96. What is false sharing, and have you ever had to deal with it?
97. Scenario: A service under load starts throwing intermittent TimeoutException only during peak traffic. Walk me through how you'd diagnose whether it's thread starvation, GC pause, connection pool exhaustion, or downstream latency.
98. Explain multithreading basics: synchronized, volatile, ExecutorService.
99. Counter in synchronize method, multiple thread working on it but still producing wrong totals, what are the possible cause
100. What is Java Thread Pool? How it works.
101. Before java 8, how can we do parallel programming
102. Why future is blocking
103. How to avoid dead lock
104. Dead lock example how to fix it
105. CompletableFuture
106. getNotify() and notifyAll()
107. When does volatile fail?
108. Why can't we use (volatile) on local variables and static fields?
109. How do you make a class thread-safe and a collection thread-safe?
110. Normal thread creation vs Executor class
111. `getNotify()` / `notify()` and `notifyAll()`
112. Lock striping and object pooling — how do they help us?
113. Sync vs asynchronous
114. Distributed locking
115. Real scenario where a deadlock happened in production for a banking system
116. How do you detect a deadlock in production?
117. Virtual thread limitations — how do you control them? What happens if we create too many virtual threads? There is no thread pool for virtual threads, so does it affect resources, memory, or anything else?
118. What happens if we create a separate virtual thread for each request?
119. Can virtual threads cause connection pool overload?
120. What is Project Loom?
121. CPU → OS thread → platform thread → virtual thread relationship
122. How do you handle ExecutorService and its parameters in a production system — how do you decide pool size?
123. What is a thread dump and a heap dump, and how do you monitor application health?
124. How do you find a blocked thread?
125. How does CompletableFuture work internally?
126. Why is Future blocking?
127. Before Java 8, how could we do parallel programming?
128. How do concurrent data structures work — ConcurrentHashMap, concurrent set, CopyOnWriteArrayList?
129. Collections internal working — map, set, list, ArrayList
130. How does a map know a key is unique? Same for set
131. How does ArrayList know it needs to increase its size — if we create an ArrayList with 2 elements vs 100 elements, at which point and how does it know to resize?
132. How does a thread pool work — what happens with no tasks and with too many tasks?
133. We don't have a thread pool for virtual threads, so how do we control virtual threads, and is there any option to reuse them?
134. How does CAS work for objects, how does volatile work for objects, and how do Atomic classes work?
135. How does synchronization work in Java?
136. Can 2 threads execute on the same object simultaneously — if there is no locking? If yes, how?
137. Counter in a synchronized method with multiple threads still producing wrong totals — what are the possible causes?
138. How to check thread dump and heap dump in production?
139. Deadlock example and how to fix it

## Java 8+ — Functional, Streams, Records & Sealed Classes

140. What's new/relevant from Java 11 → 17 → 21 you've actually used: records, sealed classes, pattern matching for switch, virtual threads (Project Loom)? Have you used virtual threads in production — what changed for your thread-pool sizing assumptions?
141. What's new in Java 17/21 (records, sealed classes, virtual threads)?
142. Functional interface, default method, optional class, Stream API, parallel Stream, advancetage of parallel stream, how parallel stream work on memory CPU and alls ,
143. Pick in stream
144. Parellalel stream vs sequesntial stream
145. Main usages of flat map and map
146. Why function interface don't have more than one abstract method
147. Can a functional interface extend another interface?
148. Streams internally
149. Why `@FunctionalInterface` annotation — is it mandatory?
150. Why can't a functional interface have more than one abstract method?
151. Pattern matching and method references — how do they work internally?
152. Record class — constructor, methods, static members; can it participate in inheritance?
153. Sealed class — with state, inheritance, static, singleton
154. Second largest number in an array using Streams
155. Peek in Stream
156. Parallel stream vs sequential stream
157. Main usages of map vs flatMap

## Exceptions & Error Handling

158. Checked vs unchecked exceptions — what's your team's actual policy, and do you agree with it?
159. finally vs try-with-resources — what happens if both the try block and the close() throw?
160. What are checked vs unchecked exceptions? When to use each?
161. Executor framework, Exception handling, Exception type, throw vs throws,
162. `@ControllerAdvice` exception checking
163. Can we have multiple finally blocks? If yes, which one runs?
164. Exceptions and their rules — parent-child
165. Checked and unchecked exceptions at compile stage — how does the compiler check and force handling of checked exceptions?

## Spring Core & Spring Boot

166. Explain the Spring bean lifecycle end-to-end (instantiation → dependency injection → BeanPostProcessor → init → destroy).
167. Constructor injection vs field injection — why is constructor injection preferred at scale, and what does it protect against (circular dependencies, immutability, testability)?
168. How does Spring Boot auto-configuration actually work under the hood (@EnableAutoConfiguration, spring.factories / AutoConfiguration.imports, conditional annotations)?
169. Explain bean scopes: singleton, prototype, request, session — what's a real bug you've seen from injecting a prototype bean into a singleton incorrectly?
170. @Transactional — how does the proxy work, and why does a self-invocation call (calling an @Transactional method from within the same class) silently not get intercepted?
171. Explain propagation types (REQUIRED, REQUIRES_NEW, NESTED) with a real scenario for each.
172. How do you handle configuration across environments (dev/stage/prod) — profiles, @ConfigurationProperties, Spring Cloud Config, Vault?
173. What does Spring Boot Actuator give you in production, and which custom health indicators/metrics have you built?
174. AOP in Spring — how have you used it for cross-cutting concerns (logging, auditing, retry)? What are the proxy limitations (final methods/classes, self-invocation)?
175. Explain the difference between @Component, @Service, @Repository, @Controller beyond "just semantics" — what does @Repository actually add (exception translation)?
176. Difference between @RestController and @Controller.
177. What is @Qualifier used for, and when do you need it?
178. What does @SpringBootApplication actually bundle (@Configuration, @EnableAutoConfiguration, @ComponentScan)?
179. What's the difference between @RequestParam, @PathVariable, and @RequestBody?
180. What does @Async do, and what's the gotcha with calling an @Async method from the same class?
181. What's a WebFlux/reactive stack, and when would you not use it over traditional Spring MVC?
182. Explain application.yml profile-specific overrides and property precedence order.
183. How does Spring Boot auto-configuration work internally (@Conditional, spring.factories/AutoConfiguration.imports)
184. Bean scopes, bean lifecycle, @PostConstruct/@PreDestroy
185. Spring Security filter chain — how does a request flow through it?
186. How do you implement JWT-based authentication in Spring Security (filter, token validation, SecurityContext)?
187. @Transactional — propagation types (REQUIRED, REQUIRES_NEW), isolation levels, what happens with self-invocation (proxy issue)
188. Coding: Implement a custom OncePerRequestFilter to validate JWT tokens
189. What is actuator, how it is work.
190. How auto configuration worked internally , beans on condition, auto configuration.import file
191. Any functional difference between, @controller, @service @repository
192. Spring vs springboot,
193. Webserver in springboot
194. IOC and DI relationship
195. IOC and DI relation
196. @ConfigurationProperties
197. @post construct
198. flow of springboot application, annotations, methods, architecture, diff modulers and micro service.
199. What happens if we override configuration?
200. What is a profile?
201. How do you activate a profile?
202. AOP — what is it and where/why do we use it, with a use case
203. Bean factory vs application context
204. Async in Spring
205. WebFlux
206. Why is field injection a bad injection style?
207. How do you change the server in Spring (external server instead of Tomcat)?
208. What happens in the background when we start a Spring application?
209. Role of `SpringApplication.run()`
210. How does auto-configuration work internally, and how does it decide what to enable?
211. Actuators and how to change the actuator port
212. How do you manage service/management endpoints — what if we want to add new ones?
213. `@Data` — when is it safe and when not? How can it create bugs silently?
214. IoC storage and DI internal working
215. Bean init and working, scanning, injection
216. Spring Boot application flow
217. How can we remove a cyclic dependency between services?
218. Spring Batch — why do we need it, and what is a pipeline?
219. I want to read a sheet with 2M rows and store it in MongoDB — how do you build that with Spring Batch?
220. Are `@Controller`, `@Service`, `@Repository` singleton by default?
221. Can we use `@Component` instead of `@Service` and `@Repository`?
222. What if `@Service` is put on a repository class and `@Repository` on a service class?

## JPA / Hibernate / Transactions

223. N+1 query problem — how do you detect it and fix it (JOIN FETCH, @EntityGraph, batch fetching)?
224. Explain first-level vs second-level cache in Hibernate — risks of enabling second-level cache in a distributed/multi-instance deployment.
225. Lazy vs eager loading — what's LazyInitializationException and how do you avoid it cleanly (not just OpenSessionInView, which has its own trade-offs)?
226. Explain optimistic vs pessimistic locking — when have you used @Version to solve a real concurrency bug (e.g., double-processing a payment)?
227. Scenario: Two threads/instances update the same row concurrently and you're seeing lost updates. How do you fix it — DB constraint, versioning, distributed lock, or redesign?
228. N+1 query problem in JPA/Hibernate — how do you fix it? (JOIN FETCH, @EntityGraph)
229. Coding: Write a JPQL/native query to find N+1 problem and fix using @EntityGraph
230. First-level vs second-level cache
231. @OneToMany vs @ManyToMany mapping, fetch = LAZY vs EAGER
232. Difference between save(), saveAndFlush(), persist(), merge()
233. What is the Hibernate dirty checking mechanism?
234. We have parent method which is handling 2 child method, if second call fail then how can we revert first method ? Which type of annotations we can use, how can we use. Why we should use?
235. How to define annotations on all 3 methods, if what, I don't want to rollback but have to store state on failure.
236. Pessimistic locks and optimistic locks with DB, how to impolement. If we have pessimistic lock, can other thread read that row at a time ? And what for optimistic lock, like how it behave when 2 thread trying to access it.
237. JPA vs hibernate, internal methods, naming, how to use
238. How JPA works internally
239. Soft delete vs hard delete
240. DB locks, and too much DB operation in a single transaction — how do you handle that?
241. 2 services updating the same DB row — how do you handle it?
242. How do you handle simultaneous DB calls on a single row in the trip table?
243. Should we keep rider/driver id directly in the trip table or make joins from trip to rider and driver tables — why use this relationship?
244. Lazy fetching vs eager fetching — which is the better approach?
245. What if `@Transactional` is not working — what could be the reasons?
246. `@Transactional` on a private method
247. `@Transactional` internally — how does the proxy mechanism work?
248. What happens with a checked exception in a transaction — does it continue (safe) or roll back?
249. What exactly is the proxy in a transaction?
250. How transactions work, how can you create one yourself, and how does it roll back?
251. DB connection methods — save, commit, flush, etc.
252. How do you handle transactions in production?
253. JPA repository internals
254. How does Hibernate work — all the terms

## Databases — SQL, NoSQL & Query Optimization

255. How do you handle schema migrations safely in a live system with zero downtime (Flyway/Liquibase, expand-contract pattern)?
256. Explain isolation levels (Read Committed, Repeatable Read, Serializable) and a real phantom-read/dirty-read bug you've hit.
257. How do you paginate efficiently over a very large table without OFFSET performance collapse?
258. ACID properties with examples
259. Indexing — B-tree index, when does an index NOT help (low cardinality columns)?
260. SQL vs NoSQL — when would you pick MongoDB over PostgreSQL?
261. Elasticsearch — inverted index concept, when to use it over SQL LIKE queries
262. How to optimize query, DB projection, polling, how to find which query is slow, SQL vs NOSQL, disadvantage of both Postgres and mongodb
263. What are Cursor and Trigger? When to use trigger?
264. How rows are stored in Databases. At which physical location it is stored? And how they're inter-relating with each other.
265. Difference between SQL and NoSQL. And how to decide when to use SQL or NoSQL,
266. We have an employee table with attributes: first name, Lastname, and embed. Two admin want to change employees with the same id at the same time.
267. How to optimize SQL query
268. SQL explain join and indexing to optimize
269. Having vs where in sql
270. Diff joins in sql
271. Joins in sql, sql vs no sql db
272. create sql query with optional filter, date range, ascending and DESC order, range of date, and order to service joins, and customer id, also pagination and batching
273. Java specifications, how to decide DB indexing, composite index, order of indexing, at a time two index can be used or only one,
274. DB migration
275. Column validation changes in DB
276. Postgres and NoSQL DB
277. RDBMS
278. External SQL framework
279. What is DB sharding?
280. How do you handle multiple databases in a single service?
281. Migrating an application to a new DB without downtime
282. How do you develop schema with the new and old way (backward-compatible schema evolution)?
283. Count(*) vs count(1) vs count(column)
284. Normalisation
285. Aggregation stages in MongoDB
286. How to optimize a SQL query
287. SQL EXPLAIN, joins and indexing to optimize
288. Different joins in SQL
289. How do you decide DB indexing, composite index, order of indexing — can two indexes be used at a time or only one?
290. How do you optimize queries, DB projection, polling — how do you find which query is slow?

## REST API Design & Web Layer

291. How do you design idempotent APIs — why does this matter especially for POST-based payment/transaction endpoints (idempotency keys)?
292. Versioning strategy for public APIs — URI versioning vs header vs content negotiation — trade-offs?
293. How do you handle partial failures in a request that fans out to 3 downstream services?
294. Explain HATEOAS — have you actually used it, or is it mostly theoretical in your experience?
295. Designing pagination, filtering, and sorting for a large collection endpoint — cursor-based vs offset-based, and why.
296. How do you version and evolve a contract without breaking existing clients (backward compatibility, consumer-driven contracts, Pact)?
297. Difference between PUT and PATCH.
298. Difference between SOAP and REST — have you had to maintain a legacy SOAP integration?
299. REST vs GraphQL vs gRPC — when would you choose each? (gRPC for internal high-perf service-to-service, GraphQL for flexible client queries, REST for simplicity/public APIs)
300. SOAP vs REST — why would a system (like payments) still use SOAP?
301. How do you version a REST API?
302. How do you handle API versioning?
303. How do you design idempotent APIs?
304. What is HTTP protocol and why it is used for communication between server and client? Any other we have
305. What is gRPC, main difference in SOAP and REST other that xml and json ? Strong point
306. How to notify frontend, like tell frontend like this task is complete…
307. REST, GraphQL, stateless and stateful, SOAP, response codes
308. CORS
309. What is ResponseEntity?
310. Few RestTemplate methods used in a project — how does PATCH work with RestTemplate?
311. Idempotent and safe operations
312. Idempotency — what is it and how did you implement it in your project?
313. 2 APIs, one sending a proper response object and another sending direct text — what is the difference between the two?
314. HTTP status code 3xx — and can we add custom codes like 600–700?
315. APIs work fine in Postman but not from the site for the frontend developer — what would be the issue?
316. REST vs SOAP
317. What is gRPC?
318. How do we validate a request body across multiple URL versions?
319. Can we do overloading in a controller to support versioning and backward compatibility for a large user base?
320. v2 makes one extra DB call but v1 does not — how do we handle this for backward compatibility?
321. How do you notify the frontend that a task is complete?

## Microservices Architecture & Resilience

322. Monolith → microservices migration — what are the actual triggers that justify the split, and what's the first thing you decompose?
323. How do you handle distributed transactions across services — Saga pattern (choreography vs orchestration), compensating transactions?
324. Explain the Outbox pattern — why is it needed alongside message publishing to avoid dual-write inconsistency?
325. Service discovery and load balancing — Eureka/Consul vs Kubernetes-native (DNS + kube-proxy) — which have you used and why?
326. API Gateway responsibilities — routing, auth, rate limiting, request aggregation — what have you built with Spring Cloud Gateway/Kong/Zuul?
327. Circuit breaker pattern — Resilience4j vs Hystrix — how do you tune thresholds, and what's a real incident where a missing circuit breaker caused cascading failure?
328. How do you do centralized configuration and secret management across dozens of services (Spring Cloud Config, Vault, AWS Secrets Manager)?
329. Explain the CAP theorem in the context of a real design decision you made (choosing consistency vs availability).
330. What does an API Gateway do — routing, auth, rate limiting, aggregation
331. How do you handle service-to-service communication failures? (Circuit breaker — Resilience4j, retries, fallback)
332. Design question: How would you design an inter-service communication pattern for an order + payment + inventory microservice setup? (sync REST vs async Kafka — trade-offs)
333. How do microservices communicate (REST, gRPC, messaging)?
334. Explain event-driven architecture vs request-response.
335. How do you handle distributed transactions (Saga pattern)?
336. What is service discovery? How does it work (Eureka, Consul)?
337. Explain circuit breaker pattern (Resilience4j/Hystrix) and why it's needed.
338. Explain API Gateway's role.
339. How do you ensure a service is highly available and fault-tolerant?
340. Microservice architecture
341. Explain project architecture and work flow, kafka,
342. advantage and disadvantage of micro service, and monolithic architecture
343. How you implemented SAGA, like for choreography we have kafka , so what is for Orchestration
344. CQRS and it's drawback , how would you achieve consistency In this , specially in read and write
345. Leader follower pattern, why
346. How you decide instance of service ?
347. Sync and asynchronous communication real time example
348. Reverting operation or transaction on failure of other service
349. Circuit breaker and different patterns
350. How do you implement a timeout in a circuit breaker?
351. Custom settings for a circuit breaker — sliding window, threshold, timeout, wait duration, etc.
352. How do you implement a fallback mechanism in a circuit breaker, how does it work internally, and how does it trigger the fallback?
353. What if one service is slow in the circuit — how can we solve it when we are passing the result to another service?
354. If one microservice crashes, how do other services detect it?
355. What is latency in microservices and how do you handle it?
356. A service uploads a large file — how do you handle it?
357. ACL (anti-corruption layer) pattern
358. Sidecar pattern
359. Fault tolerance in microservices
360. Feign client vs REST client
361. How do you make sure all instances share the load evenly?
362. How do you decide the DB and strategy to use — separate DB for each microservice vs shared DB?
363. Distributed transactions and the SAGA pattern
364. How do you handle dependent microservices and their failure?
365. Why would we need to shift from microservices back to a monolith?
366. Convert a monolith to microservices
367. How does an API gateway know which APIs map to which service, and what other features do we use in the gateway?
368. BFF (backend for frontend) communication
369. Eventual consistency, fault tolerance, and fault tolerance with Kafka
370. Difficulties in microservices
371. Microservice design patterns
372. How do you decide the number of instances of a service?
373. CQRS and its drawbacks — how would you achieve consistency, specially between read and write?
374. How did you implement SAGA — for choreography we have Kafka, so what do we use for orchestration?
375. Service protection from failure of another service — communication strategies
376. How do we handle reverting an operation or transaction on failure of another service?

## Kafka, Messaging & Event-Driven Architecture

377. Explain event-driven architecture with Kafka — partitioning strategy, consumer group rebalancing, exactly-once vs at-least-once semantics, and how you've handled duplicate message processing.
378. How do you ensure message ordering when it matters (e.g., processing events for the same account/entity) in a partitioned topic?
379. What's your strategy for schema evolution in Kafka messages (Avro/Protobuf + schema registry) without breaking consumers?
380. Dead-letter queues — how do you design retry + DLQ handling so failed messages don't silently vanish or infinite-loop?
381. Kafka core concepts: topic, partition, consumer group, offset, replication factor
382. How does Kafka guarantee message ordering? (only within a partition)
383. At-least-once vs exactly-once vs at-most-once delivery — how do you achieve each?
384. Difference between Kafka and RabbitMQ — when to use which (Kafka: high throughput/event streaming/replay; RabbitMQ: complex routing, lower latency per message, task queues)
385. How do you handle duplicate message processing (idempotency)?
386. Coding/design: Design an event-driven flow for "Order Placed → Inventory Updated → Payment Processed → Notification Sent" using Kafka topics
387. How you handled duplicate msg from produce in kafka,
388. How you troubleshoot kafka consumer that is unable to consume message
389. How you handle kafka consumer that continues failing while processing message
390. How to get response bake in kafka, like payment -> invoice -> generate
391. What are bare minimum configuration to write kafka consumer
392. Consumer rebalancing
393. Kafka stream api
394. How you publish application to perticular topic
395. Republish msg to kafka topic
396. Send() is sync or asynchronous
397. Kafka partition and it's use, why we have partition
398. Patition consumer relation, same consumer with multiple partition ? what if more consumer then partition
399. Kafka vs RabbitMQ
400. Where do you set topics, partitions and all other settings?
401. Kafka consumer side and producer side configuration
402. What are the bare-minimum configurations to write a Kafka consumer?
403. Consumer group vs consumer
404. 10 partitions and 20 consumers — what is the behavior?
405. Schema registry in Kafka — how do you handle versioning in Kafka?
406. Kafka Streams API
407. How do you publish from an application to a particular topic?
408. Republish a message to a Kafka topic
409. Is `send()` sync or asynchronous?
410. Where have you used sync and async communication — use cases
411. Report generation and report publishing — sync vs asynchronous tasking with Kafka events
412. How do you get a response back in Kafka, like payment → invoice → generate?

## Security — AuthN/AuthZ, OWASP, TLS & Secrets

413. Explain the Spring Security filter chain end-to-end for a JWT-based API.
414. Access token vs refresh token — how do you handle token revocation for a stateless JWT approach (since JWTs can't be "revoked" by default)?
415. OAuth2 grant types — which have you actually implemented (authorization code, client credentials), and why is implicit grant deprecated?
416. How do you prevent common vulnerabilities in a Spring Boot app — SQL injection (parameterized queries/JPA), XSS, CSRF (and why CSRF protection differs for stateless APIs vs session-based apps)?
417. How do you secure service-to-service calls in a microservices mesh?
418. Given a fintech/payments context: how do you ensure PCI-DSS-relevant data (card numbers, CVVs) never lands in logs, and how do you handle tokenization/masking?
419. Rate limiting and abuse prevention on public-facing APIs — token bucket vs sliding window, where do you enforce it (gateway vs app)?
420. How do you handle inter-service authentication (mTLS, service tokens, OAuth2 client-credentials)?
421. OAuth2 grant types — Authorization Code vs Client Credentials — when to use which
422. JWT structure (header.payload.signature) — how is it verified without a DB call?
423. How do you handle JWT token expiry/refresh token flow?
424. SSL/TLS handshake — brief walkthrough
425. Coding: Write a method to generate and validate a JWT using io.jsonwebtoken (jjwt) library
426. Name the OWASP Top 10 and how you'd mitigate SQL injection, XSS, CSRF.
427. How do you prevent insecure deserialization in Java?
428. What is input validation vs output encoding?
429. How do you handle secrets/credentials in microservices (vault, env vars)?
430. Explain SEI CERT coding guidelines you've applied.
431. How do you secure service-to-service communication (mTLS, OAuth2/JWT)?
432. Prevent common application valnaribilty, like css, xors, csrf, how csrf work in sessionn based api
433. What is SSL? What is an SSL certificate?
434. What is TLS and SSL and how to implement for API security in first stage,
435. Complete authorization and authentically flow, with class and method access
436. How you store secure details in DB, and how you store username and password in DB
437. Spring Security layers and filters
438. How do you secure REST APIs?
439. How do you handle common vulnerabilities like SQL injection?
440. How do you handle service secrets and external configuration?
441. How do you handle all DB URLs and other configuration in a centralized way?
442. How do you handle security — microservice-to-microservice security (deep and clear answer)?
443. What type of data does a JWT hold — is a JWT encrypted or just encoded?
444. Other options instead of JWT, and why JWT?
445. What is TLS and SSL, and how do you implement it for API security in the first stage?
446. Complete authorization and authentication flow, with class- and method-level access
447. How do you provide security to a REST API — session management, JWT, certificates, roles? Complete implementation in steps (no code needed)
448. Security checks and additional security layers on top for a service
449. How do you store secure details in a DB, and how do you store username and password in a DB?
450. How does CSRF work in session-based APIs?

## Performance, Caching & Scalability

451. How do you find and fix a slow endpoint in production — profiling tools, APM (New Relic/Dynatrace/Datadog), thread dumps, flame graphs?
452. Caching strategy — cache-aside vs write-through vs write-behind — Redis vs local (Caffeine/Guava) — when do you use each, and how do you handle cache invalidation/stampede?
453. How do you design a system to handle a 10x traffic spike (e.g., flash sale, batch settlement window) without falling over?
454. Connection pool tuning (HikariCP) — what metrics do you watch, and what's a real pool-exhaustion incident you've debugged?
455. How do you do capacity planning / load testing before a major release (JMeter/Gatling, and what SLAs did you validate against)?
456. Redis use cases: caching, session store, distributed locks, rate limiting
457. Coding: Implement a Redis-based distributed lock (or cache-aside pattern) in Java
458. Explain caching strategies (Redis, local cache) in microservices.
459. How cache working internally ?
460. How would you identify and resolve performance bottlenecks in a Java application?
461. performance testing
462. TTL, cache key, cache vector — custom configuration
463. How do you optimize application startup time?
464. Scaling for high traffic
465. How do you handle a service that has trillions of requests of traffic?
466. How do you handle 100k requests per second?
467. If we have 50M requests per second, which part of the service should we focus on more — consistency handling, traffic handling, DB connection pool, locking, asynchronous processing, query issues?
468. How do you handle traffic?
469. Cache
470. How do you handle cache with time-based update and forced update?
471. How does caching work internally?
472. Memory optimization because the service runs out of memory
473. How do you optimize JVM startup time?
474. How do you optimize your code?

## Production Troubleshooting, Logging & Observability

475. Observability — how do you correlate a request across 6 microservices (distributed tracing — Sleuth/Zipkin/OpenTelemetry, correlation IDs)?
476. Structured logging strategy — what do you log, what do you deliberately never log, and how do you keep log volume/cost sane at scale?
477. How do you handle logging/monitoring/observability (ELK, Prometheus, Grafana)?
478. Loggin, alert, production failure, monitoring
479. Service throwing connection pool exhausted error in production, how would you diagnosis
480. ELK implementation
481. What do you use for logging? How do you configure it?
482. How did you debug out of memory exception and how to solve
483. Heap dump analysis
484. How to handle production issue
485. How do you check for crashes and failures?
486. Application is slower in production compared to dev — what could be the issue?
487. API is suddenly slow — what will you check first and how do you handle it?
488. Production issue at midnight — how do you handle and investigate it?
489. DB CPU is low but API latency is high — what could be the reason?
490. A critical service goes down during business hours — what steps do you take?
491. One production issue and how you fixed it, and the most difficult bug you solved
492. How did you debug an OutOfMemoryError and how did you solve it?
493. Logging, alerting, production failure and monitoring
494. What is Splunk, how do you use it and why?

## Testing & Code Quality

495. Unit vs integration vs contract testing — what's your team's actual test pyramid look like in practice (not textbook)?
496. Mockito — mocking pitfalls you've seen (over-mocking, testing implementation instead of behavior).
497. Testcontainers — how have you used it to test real DB/Kafka interactions instead of mocking them away?
498. How do you test asynchronous/event-driven flows reliably (avoiding flaky sleep-based tests)?
499. What's your approach to testing @Transactional rollback behavior and idempotency logic?
500. Mockito: @Mock vs @MockBean vs @Spy
501. How do you write an integration test for a REST controller with @SpringBootTest + MockMvc or TestRestTemplate?
502. How do you mock a Kafka producer/consumer or an external REST call in a unit test?
503. Coding: Write a JUnit 5 + Mockito test for a service method that calls a repository and an external Feign client
504. Difference between unit, integration, and service-level tests.
505. What tools have you used for mocking (Mockito, WireMock)?
506. How do you measure code coverage, and what's a good target?
507. Explain contract testing (Pact) in microservices.
508. What is SonarQube used for? How do you fix code smells it flags?
509. Have you used Checkmarx or similar SAST tools? Walk through a finding you remediated.
510. What is Zally, and how is it used for API linting/governance?
511. What is mock and sub , other methods
512. Integration test vs unit test
513. What annotations do you use for testing, and how have you used Mockito?
514. What does Sonar check in your project and how did you use it?
515. Automation testing

## Cloud, DevOps, Docker, Kubernetes & CI/CD

516. Walk through your CI/CD pipeline end-to-end — build, test gates, image build, deployment strategy, rollback trigger.
517. Blue-green vs canary deployment — trade-offs, and which have you actually run in production?
518. How do you handle zero-downtime deployments when the schema itself is changing (expand-contract migration pattern)?
519. Kubernetes — how do you configure readiness vs liveness probes correctly for a Spring Boot app (and what goes wrong if you get them backwards)?
520. How do you handle graceful shutdown so in-flight requests aren't dropped during a pod termination/scale-down?
521. What build tool do you use (Maven/Gradle) and why — any multi-module project experience?
522. Difference between EC2, Lambda, ECS — when serverless (Lambda) vs container (ECS/EKS)
523. How does an SQS queue help decouple microservices?
524. Docker: image vs container, multi-stage builds — why use them?
525. Kubernetes: Pod, Deployment, Service, ConfigMap/Secret — how does a Service discover Pods?
526. How do you set up a CI/CD pipeline in Jenkins/GitLab for a Spring Boot app (build → test → Docker image → push to JFrog Artifactory → deploy to K8s)?
527. How do you manage secrets (DB passwords, JWT secret) in Kubernetes/AWS securely?
528. Walk through a CI/CD pipeline you've built (tools: Jenkins, GitLab CI, GitHub Actions).
529. Explain Docker basics: image vs container, Dockerfile layers.
530. What is Kubernetes and how does it help with container orchestration?
531. How do you handle blue-green or canary deployments?
532. Explain Infrastructure as Code (Terraform, Ansible).
533. Deployment and AWS deep, Jenkins, script, pipeline, policies,
534. Clouds skill and rule
535. What is AWS EC2 instance and how to deploy jar in it?
536. Ham charts in kubrnatis
537. How Trigger and develop lambda
538. What is terraform and how you used it
539. Basic command for terraform
540. How to increate intense in kubernatis, by script and y CLI
541. How you used kubernatis in current project
542. CICD tools we used in our project
543. Lambda service, how server less, triggering , EC2, SQS, SNS, how to deploy 2-3 ways,
544. Blue-green deployments
545. Kubernetes and Jenkins for deployment
546. Virtual machine vs Docker
547. Why is Docker used?
548. How does Docker help run a container on all types of machines — how does it work?
549. How do you manage all containers of multiple services, and the commands?
550. What is Kubernetes and what are its commands?
551. Where are Docker images stored — versioning and ECS
552. Docker image versioning
553. How do you create a Docker image?
554. Docker volume
555. Kubernetes ConfigMap
556. Helm charts in Kubernetes
557. How do you increase instances in Kubernetes — by script and by CLI?
558. How have you used Kubernetes in your current project?
559. Steps for CI/CD
560. CI/CD tools used in your project
561. How do you follow different modules in the pipeline — explain the complete flow of your pipeline
562. Different deployment strategies — blue-green, canary, etc.
563. Which AWS service can we use to deploy?
564. How can we use Docker for deployment?
565. How can we manage environments, secrets and load balancers?
566. How do you manage common properties and secrets?
567. How do you trigger and develop a Lambda?
568. Lambda service — how is it serverless, triggering, EC2, SQS, SNS, and 2–3 ways to deploy
569. What is Terraform and how have you used it?
570. Basic commands for Terraform
571. IaC — infrastructure as code

## Design Patterns & SOLID

572. Explain all 5 SOLID principles with a real code example for each (not textbook definitions — actual violation → fix)
573. How would you refactor a class that violates Single Responsibility?
574. Give an example where you applied Dependency Inversion in a Spring Boot project
575. Coding: Design a PaymentProcessor system where you can add new payment types (UPI, Card, Wallet) without modifying existing code — apply Open/Closed Principle
576. Explain Factory, Singleton, Adapter, Composite, Observer, Strategy patterns with real examples.
577. What is Inversion of Control / Dependency Injection? How does Spring implement it?
578. When would you use Strategy vs Factory pattern in a microservice?
579. Desing pattern
580. Enum singleton
581. Design patterns, how you used , implement, where you used , why we need
582. Solid principle, why, where, how
583. Design patterns — why used and why important? Which pattern solves which problem?
584. Why and which design pattern did you use?
585. How do you implement SOLID?

## System Design & Design Scenarios

586. Design a payment/transaction processing system that must guarantee exactly-once processing and handle retries safely.
587. Design a rate limiter for an API gateway serving millions of requests/day.
588. Design a notification service that fans out to email/SMS/push with retry and dedup.
589. Design an idempotent order/transaction API where the client may retry the same request due to a network timeout.
590. Design a system for real-time fraud detection on transaction events (streaming, low latency, false-positive trade-offs).
591. How would you design a distributed unique ID generator (Snowflake-style) for transaction IDs across multiple regions?
592. Design a reconciliation system that compares two large datasets (e.g., internal ledger vs external settlement file) and flags mismatches.
593. Design a payment transaction system with idempotency (very relevant to Mastercard) — how do you prevent double charging on retry?
594. Design a rate limiter for an API Gateway
595. Design a notification service using Kafka + microservices
596. How would you scale a service handling millions of transactions/day — caching, DB read replicas, async processing, partitioning
597. How leetcode works? If you have to develop a coding platform what will be your approach
598. Getting diff event for orders, have some service to handle that order, after service we have to create a bill. How to design this system, one order can have multiple service and all service has UID., what we have 2 type of amount, like amount from service and calculate amount,
599. Design a DB for org, department, user, role, permissions, and exceptional permissions
600. We have a ride booking application with millions of riders — how can we find the nearest riders from a pickup location (with JPA and SQL)?
601. We have a large-scale application that needs some real-time data in multiple regions — how can we manage it in a timely manner across regions?
602. UTC concept in a large-scale multi-region application — how do we use LocalTime, and how does LocalTime work for India and America at the same time?
603. We have an offer table that stores promo codes — how many fields would you add, and how do we enable a code only for a particular day or the first week of every month? Is storing all promo codes in the DB a good approach, or is there an alternative?
604. How do you track a driver and a trip — should we store everything in the DB? For a large application that would be too many DB queries
605. If 2 people at the same cafe book a ride and both get the same rider on the first attempt, will it book the same rider for two trips — how do you handle it?

## Kotlin vs Java

606. Null safety in Kotlin (?, !!, ?:) vs Java's Optional/NPE handling
607. Data classes vs POJOs — what does Kotlin generate automatically?
608. val vs var, coroutines vs Java threads (basic difference)
609. Extension functions — what problem do they solve?
610. Coding: Write a function in Kotlin using ?.let{} to safely chain nullable calls

## Version Control, SDLC & Documentation

611. Explain Gitflow branching strategy.
612. What's your peer review process — what do you look for in a PR?
613. Compare Waterfall, Scrum, Kanban, SAFe — when would each be used?
614. How do you estimate effort/story points?
615. How do you document APIs (Swagger/OpenAPI)?
616. What makes good technical documentation for services you own?
617. Merge vs rebase — how do you resolve conflicts?
618. Challenges in migrating to a new Spring and Java version

## Behavioral / Managerial

619. Tell me about the most critical production incident you've owned — what broke, how did you find root cause, and what changed afterward (postmortem culture)?
620. Describe a time you had to make a trade-off between shipping fast and doing it "right" — how did you decide, and what was the outcome?
621. Tell me about a time your service caused a cascading failure in another team's system — what did you learn about blast-radius design?
622. Describe a schema/API change you had to roll out without breaking existing consumers — how did you sequence it?
623. Tell me about a time you disagreed with an architectural decision from a senior engineer/architect — how did you handle it?
624. Describe a performance problem you diagnosed that turned out to be something non-obvious (not the code you first suspected).
625. Tell me about mentoring a junior engineer or leading a small team through a delivery — how did you balance hands-on coding vs unblocking others?
626. Describe a time you had to push back on unrealistic timelines from product/business — how did you frame the conversation?
627. Walk me through how you'd approach debugging a bug that only reproduces in production and not in staging.
628. Tell me about a design decision you made early in a project that you'd change if you started over — what did you learn?
629. What are your 3 weak and 3 strong points?
630. How will you explain microservices to an 8-year-old kid?
631. Whole process for current project, for rogers, SNB and old projects
632. managerial
633. Whole process for the current project and previous projects

## Coding Problems

634. problem on number-to-words conversion.
635. Next palindrome in 100 to 1000, or next palidrome for 123 or 124 , ask for this is 131
636. rotate list
637. shift all null or 0's in string list or int list
638. find second highest/lowest using steam

639. Find the mountain (start index, end index, width):

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

