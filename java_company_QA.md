**Interview Questions Compendium**

*Compiled from LinkedIn interview-experience posts, organized by
company*

*Duplicate questions removed; differently-worded versions of the same
topic are kept as separate entries.*

*Where a post included a hint, approach, or answer, it is shown in
italics under the question.*

<a id="index"></a>
# Index

*Click a link below to jump straight to that company's questions. Every section ends with a ⬆ Back to Index link.*


**[Part 1 --- Company-Specific Interview Experiences](#part-1-company-specific-interview-experiences)**

- [Thoughtworks](#thoughtworks)
- [PwC](#pwc)
- [Deloitte](#deloitte)
- [PubMatic](#pubmatic)
- [Altimetrik](#altimetrik)
- [KPMG](#kpmg)
- [HCL](#hcl)
- [Paytm](#paytm)
- [NTT Data](#ntt-data)
- [Infosys](#infosys)
- [Tech Mahindra](#tech-mahindra)
- [Goldman Sachs](#goldman-sachs)
- [TCS](#tcs)
- [Accenture](#accenture)
- [Virtusa](#virtusa)
- [Mphasis](#mphasis)
- [PayPal](#paypal)
- [EY](#ey)
- [Airtel](#airtel)
- [Mastercard](#mastercard)
- [Fincart](#fincart)
- [Nagarro](#nagarro)
- [Slice](#slice)
- [JPMorganChase](#jpmorganchase)
- [Barclays / Capgemini / Citi / Infosys / JPMorganChase / TCS](#barclays-capgemini-citi-infosys-jpmorganchase-tcs)
- [Hughes Systique Corporation (HSC)](#hughes-systique-corporation-hsc)
- [EPAM](#epam)
- [Kotak Mahindra Bank](#kotak-mahindra-bank)
- [Wissen](#wissen)
- [IRIS Software Group](#iris-software-group)
- [Visa](#visa)
- [L&T](#lt)
- [Hotfoot](#hotfoot)

**[Part 1b --- Interview Experiences Where the Company Was Not Named](#part-1b-interview-experiences-where-the-company-was-not-named)**

- [Unnamed Company --- "₹12 LPA Backend Developer Offer"](#unnamed-company-12-lpa-backend-developer-offer)
- [Unnamed Service-Based Company --- 1st Technical Round](#unnamed-service-based-company-1st-technical-round)
- [Unnamed Company --- "Position: Java Backend Developer"](#unnamed-company-position-java-backend-developer)
- [Unnamed Company --- Java Developer, Dubai (L2 Round)](#unnamed-company-java-developer-dubai-l2-round)
- [Unnamed Company --- "Today's First-Round Interview Experience"](#unnamed-company-todays-first-round-interview-experience)
- [Unnamed Company --- Multi-Round Scenario & System-Design Process](#unnamed-company-multi-round-scenario-system-design-process)
- [Unnamed Company --- "Java Full Stack Developer" First Technical Round](#unnamed-company-java-full-stack-developer-first-technical-round)
- [Unnamed Company --- "Senior Java Backend Developer -- Spring Boot & Microservices" (Virtual Technical Round)](#unnamed-company-senior-java-backend-developer-spring-boot-microservices-virtual-technical-round)
- [Unnamed Company --- Java Full Stack (Angular) Developer, 2nd Round Technical](#unnamed-company-java-full-stack-angular-developer-2nd-round-technical)

**[Part 2 --- General / Topic-Wise Question Banks (Not Tied to One Company)](#part-2-general-topic-wise-question-banks-not-tied-to-one-company)**

- [Top Java and Spring Boot Interview Questions (general topic list, not tied to a company)](#top-java-and-spring-boot-interview-questions-general-topic-list-not-tied-to-a-company)
- [Production-Scenario Question Banks (multiple compiled lists, not tied to a company)](#production-scenario-question-banks-multiple-compiled-lists-not-tied-to-a-company)
- [Core Java, Concurrency & JVM --- Deep-Dive Question Banks](#core-java-concurrency-jvm-deep-dive-question-banks)
- [Microservices, System Design & API Performance --- Question Banks](#microservices-system-design-api-performance-question-banks)

**[Part 3 --- Reference / Topic Lists (Not Q&A)](#part-3-reference-topic-lists-not-qa)**

- [Kafka Topics --- Reference Playlist](#kafka-topics-reference-playlist)
- [Low Level Design --- 50-Problem Master List](#low-level-design-50-problem-master-list)
- [Complete Java Topic Syllabus ("As per Developer")](#complete-java-topic-syllabus-as-per-developer)

<a id="part-1-company-specific-interview-experiences"></a>
# Part 1 --- Company-Specific Interview Experiences


[⬆ Back to Index](#index)

<a id="thoughtworks"></a>
# Thoughtworks

*Java Backend Developer \| 5--8 Years Experience \| Interview Date:
10-Sep-2026*

## Java

-   Explain HashMap internals and how collisions are handled in Java 8+.

-   When would you use ConcurrentHashMap over synchronizedMap?

-   How do Streams differ from traditional loops, and when can parallel
    streams hurt performance?

-   Explain CompletableFuture composition using thenCompose, allOf and
    exceptionally.

-   How do virtual threads in Java 21 change concurrency and thread-pool
    design?

-   What JVM/GC issues can cause sudden latency spikes in production?

-   How would you use Records, Sealed Classes and Pattern Matching in a
    backend domain model?

-   Explain happens-before, volatile and atomic operations in concurrent
    Java code.

-   How would you diagnose a memory leak or excessive object allocation
    in a Java service?

## Spring + Spring Boot

-   Explain Spring IoC, bean scopes and the lifecycle of a singleton
    bean.

-   How does Spring Boot 3 differ from earlier Boot versions in
    migration and runtime behavior?

-   Explain \@Transactional propagation, isolation and common rollback
    pitfalls.

-   How does Spring AOP work, and why can self-invocation bypass advice?

-   How would you design global REST exception handling and API
    validation?

-   How would you secure REST APIs using Spring Security and OAuth2/JWT?

-   How do Spring Boot Actuator, metrics and distributed tracing support
    observability?

-   How would you implement caching with Spring Cache while avoiding
    stale or inconsistent data?

-   How would you configure resilience using timeouts, retries, circuit
    breakers and bulkheads?

## Microservices

-   How would you define service boundaries and data ownership in a
    microservices architecture?

-   Design an idempotent Kafka consumer handling retries, duplicates and
    ordering.

-   How would you handle distributed transactions without using 2PC?

-   Compare synchronous REST, asynchronous Kafka and event-driven
    communication.

-   How would you diagnose latency across multiple services using
    correlation IDs and tracing?

-   Design a scalable microservice deployment using Docker and
    Kubernetes.

-   How would you implement service discovery, configuration and
    graceful shutdown?

## Coding Questions

-   Find the first non-repeating character in a String using Java
    Streams.

-   Merge overlapping intervals efficiently and return the consolidated
    ranges.

-   Implement a thread-safe LRU cache with O(1) get and put operations.


[⬆ Back to Index](#index)

<a id="pwc"></a>
# PwC

*Java Interview --- Tricky DSA Question*

## Coding / DSA

-   Given a list of meeting intervals, find the minimum number of
    meeting rooms required so that no meetings overlap. Example: Input
    \[\[0,30\],\[5,10\],\[15,20\]\] → Output 2.

-   *Hint / Answer given: Optimal approach: sort meetings by start time,
    use a min-heap to track the earliest ending meeting --- if the
    earliest meeting has ended, reuse its room, otherwise allocate a new
    room. Final heap size = minimum rooms needed. Time O(n log n), Space
    O(n).*

-   Follow-up: Can you return the actual room assigned to each meeting?

-   Follow-up: Can you solve it without using a PriorityQueue?

-   Follow-up: How would you handle 10\^5+ intervals?

-   Follow-up: What happens when two meetings start at the same time?


[⬆ Back to Index](#index)

<a id="deloitte"></a>
# Deloitte

*Multiple candidate experiences --- Java Backend / Full Stack Developer*

## Experience A --- Java Developer, 2 YOE (Round 1, via Naukri referral)

-   Difference between map() and flatMap().

-   Java 8 features --- Streams, intermediate and terminal operations.

-   Functional interface vs marker interface.

-   Implement custom exception handling.

-   super() and the \'this\' keyword.

-   Can we override the main method?

-   *Hint / Answer given: No --- it can be overloaded (it\'s
    case-sensitive too); if overloaded, JVM will fail to find main and
    throw a runtime error. If main is overloaded, JVM picks the one with
    String\[\] args by default.*

-   Types of memory (heap, stack, method area, program counter).

-   Garbage collector --- finalize().

-   Coding --- find the minimum subarray with sum equal to k.

-   Runnable vs Callable --- Thread class.

-   run() vs start(), and wait() vs sleep().

-   Synchronization.

-   SQL --- rank employees based on experience; difference between
    RANK(), DENSE_RANK(), ROW_NUMBER(); question on query execution
    order.

## Experience B --- Java Backend Developer, 4 YOE, Round 1 (25 questions)

-   Abstract Class vs Interface after Java 8.

-   Explain locking in Java.

-   wait() vs notify() vs notifyAll().

-   ReentrantLock vs ReentrantReadWriteLock.

-   Future vs CompletableFuture.

-   Explain the internal working of HashMap.

-   Fail-Fast vs Fail-Safe.

-   What is ConcurrentModificationException and when does it occur?

-   Explain Optional and its practical usage.

-   Explain Generics --- extends vs super.

-   Difference between \@RestController vs \@Service vs \@Repository vs
    \@Component.

-   \@Component vs \@Configuration + \@Bean.

-   How would you implement a Global Exception Handler?

-   Filter vs Interceptor --- the difference and when to use each.

-   Explain Hibernate Second-Level Cache.

-   What is the N+1 Query Problem? How would you identify and solve it?

-   Saga Pattern --- Orchestration vs Choreography.

-   Explain resilience patterns: Circuit Breaker, Retry, Bulkhead,
    Jitter.

-   How do microservices communicate securely?

-   What are the different approaches for microservice-to-microservice
    communication?

-   What is a Kafka Consumer Group?

-   Explain Kafka offsets.

-   What are topics and partitions?

-   What is a Dead Letter Queue (DLQ)?

-   Explain Kafka acknowledgements (ACKs).

-   How does Kafka replication work?

-   What are database indexes?

-   What is a composite key?

-   What are stored procedures and triggers?

-   Write a SQL query to find employees whose salary is greater than
    their manager\'s salary.

-   Given a list of integers: reverse the order, keep distinct values,
    place odd numbers first, then place even numbers after them (using
    streams).

-   Coding --- Valid Brackets: given an expression containing (), \[\],
    {}, determine whether the brackets are correctly balanced.

-   *Hint / Answer given: Follow-up: can you solve it without using a
    Stack data structure?*

## Experience C --- via Lynx Reach opportunity

-   Aggregation vs Composition.

-   this vs super.

-   Method Overriding and Runtime Polymorphism.

-   Fail-Fast vs Fail-Safe Iterators.

-   Liskov Substitution Principle.

-   Exception Handling.

-   map() vs flatMap().

-   Functional Interfaces.

-   Streams and their practical use cases.

-   ExecutorService.

-   Callable vs Runnable.

-   Thread lifecycle.

-   Sleep vs Wait.

-   Synchronization.

-   IoC and Dependency Injection.

-   Spring Bean Lifecycle.

-   Spring Boot Auto Configuration.

-   AOP, and how AOP was implemented in the candidate\'s project.

-   Normalization and its different types.

-   Indexing.

-   View vs Stored Procedure.

-   SQL joins.

-   Query optimization.

-   Microservices architecture and design patterns used in the project.

-   Saga Design Pattern.

-   Communication between microservices.

-   Handling failures in distributed systems.

-   AWS services used in the project, deployment architecture,
    monitoring/logging, scalability.

-   Coding --- convert an Infix Expression to a Postfix Expression.

-   Deep dive into the candidate\'s own project: architecture, why
    certain technologies were chosen, design decisions, challenges
    faced, performance considerations, real-world scenarios.

## Experience D --- Second Round Technical (Java Full Stack), project-focused

-   From the time you graduated, what have you learned during your
    4-year journey?

-   Explain your learning journey year by year and how it differed from
    college.

-   What aspects of deployment have you worked on / actually handled?

-   What is validation? How do you implement validators in Java?

-   Give an end-to-end file structure where you implemented validation.

-   What are the different types of validation?

-   What was your actual contribution to validation --- which feature
    did you validate, and how did you validate the source account?

-   What was your actual work in development --- how did you implement
    it and what specific rules?

-   Who gave you the requirements, and what tool did you use to track
    them?

-   How was the validation feature assigned, and how did it evolve?

-   Give an instance where you used pessimistic locking.

-   What files/classes are created when you implement a feature?

-   You mentioned secure API implementation --- give an example of how
    you implemented it.

-   Which services did you autowire into your Controller?

-   What do you mean by layered architecture? How was your application
    structured, and what are its benefits?

-   What are computed() and effect() in Angular?

-   How do you implement threads in Java?

## Experience E --- Java Full Stack, 3-round process

-   Round 1 -- Technical: How HashMap works internally; hashCode
    contract; HashSet vs HashMap; how to store sensitive data securely
    in Java; Java 8 multithreading features; how to make two parallel
    calls in Java; write a recursive function to reverse a String; find
    two indices in a sorted array whose sum equals a target; what is a
    Spring Bean; Dependency Injection and its benefits; scopes of Spring
    Beans; PUT vs POST; is JavaScript single- or multi-threaded; how
    JavaScript handles async operations; React Hooks and Virtual DOM.

-   Round 2 -- Another Technical: explain the migration process from a
    legacy application to a modern application; how do you analyze
    legacy code; sort an array of strings by frequency; handling zero
    values in a Divide API; perform an INNER JOIN between two tables;
    Error vs Exception in Java; what is a React Component; passing data
    between React components; sequential API calls in React; useEffect
    in React; certifications.

-   Round 3 -- Techno-Managerial: functional and technical aspects of
    your project; managing migration to modern technologies; number of
    microservices created and how they interact with databases; data
    consistency across microservices; tools used for inter-service
    communication; handling failures in async communication; security
    measures for microservices/REST APIs; ensuring performance is not
    compromised; maintaining data consistency between Redis cache and
    the database; Agile experience; estimation for new features;
    ensuring code quality; thoughts on AI tools like GitHub Copilot.

## Experience F --- Java Developer, 3 YOE (Core Java, Multithreading, Coding & SQL)

*Reported by multiple students/candidates with near-identical question
sets from a Deloitte India Round 1 technical interview.*

-   Core Java & Java 8: key features introduced in Java 8; map() vs
    flatMap(); intermediate vs terminal operations in Stream API;
    functional interface vs marker interface; can we overload main(),
    and which one does JVM execute; this vs super keywords; how do you
    create and handle custom exceptions; how does HashMap work
    internally; == vs equals(); why override hashCode() with equals();
    String vs StringBuilder vs StringBuffer.

-   Multithreading & Concurrency: Runnable vs Callable; start() vs
    run(); wait() vs sleep(); what is synchronization; what is a
    deadlock and how can you prevent it; volatile vs synchronized; Java
    memory areas --- Heap, Stack, Method Area, and Program Counter;
    garbage collection and the purpose of finalize(); ExecutorService vs
    manually creating threads.

-   Coding & SQL: find the minimum-length subarray whose sum equals a
    given target; find the first non-repeating character in a string;
    find the second-highest number using Streams; write an SQL query to
    rank employees based on their experience; RANK() vs DENSE_RANK() vs
    ROW_NUMBER(); WHERE vs HAVING; find the highest salary in each
    department; explain the order of execution of SQL query clauses.

-   Project Discussion: explain your project clearly, including the
    application flow, your responsibilities, technical challenges,
    database and API choices, performance optimization, and real-world
    problem-solving.

-   *Key takeaway shared by candidates: prepare Core Java fundamentals,
    Java 8, multithreading, coding problems, SQL queries, and an
    end-to-end project explanation.*

## Experience G --- Java Full Stack Developer, L35 Software Engineer II (Selected)

-   Round 1 --- Technical: tell me about yourself and explain your
    project architecture; internal working of HashMap; HashMap vs
    ConcurrentHashMap; synchronized vs ReentrantLock; design a login
    authentication Controller --- what security measures/headers would
    you use (asked to write complete Java code on notepad); how would
    you handle multiple dependent services and service failures; what
    is an Idempotency Key and how does it help prevent multiple payment
    requests from the same user/client; designing an API that needs to
    interact with a client server --- what phases to consider; how do
    you resolve Git merge conflicts; Angular Pipes & component-to-
    component communication; how do you handle browser sessions.

-   Round 2 --- Technical: explain SOLID principles with examples;
    design an LRU Cache; synchronous vs asynchronous communication;
    dependency injection --- Field vs Constructor vs Setter, which is
    preferred and why; exception handling in Spring Boot;
    \@ControllerAdvice vs \@RestControllerAdvice; handling
    HttpClientErrorException; explain Saga Architecture; caching vs
    pagination --- why use pagination when caching can also store data;
    how would you schedule a batch job, and why do we need to schedule a
    job in a backend application; Micro Frontend vs Mini Frontend; can
    Micro Frontends use multiple tech stacks; application packaging →
    deployment process; AWS services, S3 & Lambda; Kubernetes deployment
    and rollout commands.

-   Round 3 --- Techno-Managerial: explain the complete request flow
    from Postman/Browser to the service response; how do you manage
    conflicts in a project; explain OAuth architecture; what is JWT ---
    is it stateful or stateless; what happens if someone modifies a JWT
    claim such as expiry; how would you handle disagreement with
    colleagues regarding your solution; why Deloitte when you already
    have other offers; are you comfortable working with a new tech
    stack.

## Experience H --- 20 Java Questions Asked

-   Concept of class metadata in the context of Java class loading.

-   What happens when we try to reuse a stream?

-   How can null be handled in TreeSet?

-   How does IdentityHashMap differ from a normal HashMap?

-   Concept of a cache map --- what are some benefits of using Streams.

-   What\\'s the life cycle of a Spring bean?

-   How do you build a REST API in Spring Boot?

-   Difference between POST and PUT --- any other differences?

-   What about the main infrastructure to support services?

-   What happens if a microservice fails? How do you handle it?

-   Can you explain API Gateway --- which features are supported?

-   How do you handle authentication? What\\'s the structure for a JWT
    token?

-   How do you implement a circuit breaker? How do you set up
    thresholds?

-   What is the purpose of a threshold? How does it work?

-   Do you know about the Kafka schema registry?

-   Have you worked on microservices? Can you explain the 12 factors of
    microservices?

-   Can you explain the DRY principle?

-   Can you explain the difference between PUT and QUERY methods in
    HTTP protocols?

-   Concept of parameterized testing in JUnit?

-   Difference between Mock and SPI annotations in JUnit?


[⬆ Back to Index](#index)

<a id="pubmatic"></a>
# PubMatic

*Senior Software Engineer \| 4 Years Experience \| Round 1*

## Java

-   What is compile-time polymorphism? How does method overloading work
    at compile time?

-   Given classes A → B → C with overloaded methods, what will be the
    output?

-   What is the difference between Comparable and Comparator?

-   How does Garbage Collection work in Java?

-   What is the use of Cloneable?

-   Write a Java Stream program to find the second-highest salary.

-   Why do we use Optional.isPresent() in the second-highest-salary
    solution?

-   In which scenario will findFirst() return an empty Optional?

## Spring / Transactions

-   What happens if the 9th statement fails inside a \@Transactional
    method containing 10 statements?

-   How can you make the 5th operation commit even if the 9th operation
    fails?

-   What is transaction propagation?

-   What is the difference between REQUIRED and REQUIRES_NEW?

-   What is pessimistic locking?

## Microservices

-   If a microservice becomes slow, what steps would you take to
    troubleshoot it?

-   What is Spring Boot Actuator?

-   How does Actuator help in identifying performance issues?

-   How would you handle a distributed transaction across Microservice
    A, B and C?

-   If Service C fails, how would you revert the changes made in
    Services A and B?

## Concurrency / System Design

-   Suppose only one theatre seat is remaining and hundreds of users
    request it simultaneously --- how would you handle this situation?

## OOP

-   What is the difference between Aggregation and Composition?

## Spring Security

-   Have you worked with Spring Security? What type of authentication
    does your project use?

-   How did you implement JWT authentication?

-   Is JWT strong enough for a banking application?

## Docker / DevOps

-   What is the difference between a Container and a Virtual Machine?

-   Have you worked with Docker or any messaging queue?

-   What are Spring Profiles? Why do we use different profiles such as
    Dev, QA and Prod?

## Security / Configuration

-   How do you maintain passwords and sensitive information in your
    project?

-   Where do you store the database password?

-   Do you hardcode or commit passwords to Git?

## Project / Experience

-   Tell me about your project and your role.

-   Explain your application architecture.

-   What technologies have you worked with?


[⬆ Back to Index](#index)

<a id="altimetrik"></a>
# Altimetrik

*Java Backend Developer Interview --- 2nd Attempt*

## Core Java

-   What are Records in Java? Do Records have equals() and hashCode()?

-   What is Method Hiding in Java? How would you implement it?

-   What are Functional Interfaces? Write examples using Predicate and
    Consumer.

-   Write a lambda expression for addition and pass it to a method.

-   Explain the SOLID principles with real-world examples.

-   How does HashMap work internally?

-   *Hint / Answer given: Covers: how is the key\'s hash calculated,
    what is the load factor, how does resizing work, how does it impact
    performance.*

-   ArrayList vs LinkedList --- when would you choose each?

-   What changed in HashMap in Java 8 compared to previous versions?

-   If two threads access and modify the same HashMap concurrently, what
    can go wrong?

-   *Hint / Answer given: HashMap is not thread-safe.*

-   How does ConcurrentHashMap solve this problem?

## Java Streams

-   Coding: Count employees in each department.

-   Coding: Find the youngest employee in each department and return
    their name and age.

-   Coding: Find the second-highest salary from a list containing
    duplicate salaries.

## Spring & Spring Boot

-   Difference between Spring and Spring Boot.

-   What happens internally with \@SpringBootApplication?

-   What are Filters and Interceptors in Spring Boot?

-   How does Spring Security work?

-   Explain OAuth 2.0 vs JWT.

-   What is HATEOAS and when would you use it?

## SQL

-   Count employees department-wise.

-   Find the second-highest salary from a table.

-   Focus was on understanding query behavior with duplicate values and
    real-world data, not just writing queries.

## Microservices & System Design

-   How would you handle cascading failures in Microservices?

-   How would you implement observability across Microservices?

-   Your Microservices suddenly receive a huge increase in traffic ---
    what would you do?

-   Why migrate from Monolith to Microservices?

-   Where would you use Microservices and where would you avoid them?

-   For resiliency, which patterns have you implemented? (Circuit
    Breaker, Retry, Timeout, Rate Limiting, Bulkhead, Load Balancing,
    Caching, Auto Scaling)


[⬆ Back to Index](#index)

<a id="kpmg"></a>
# KPMG

*Software Engineer Role (Java Backend) --- via consultancy screening*

## Round 1 --- Screening Call

-   Professional experience and current tech stack.

-   What is \@ControllerAdvice?

-   What are transactions in Spring Boot?

-   Difference between microservices vs monolithic architecture.

-   Experience with CI/CD pipelines.

## Round 2 --- Face-to-Face Technical (DSA, written + explain + execute)

-   Linked List (Easy): given the head of a singly linked list, return
    the middle node --- if two middle nodes exist, return the second
    one.

-   Rotated Sorted Array (Medium): given a rotated sorted array and a
    target, return the index of target in O(log n). Example:
    \[0,1,2,4,5,6,7\] rotated → \[4,5,6,7,0,1,2\].

## Round 3 --- Discussion (CV-based)

-   Spring annotations: \@Service, \@Repository usage.

-   Difference between abstract classes vs inheritance.

-   What is polymorphism? Explain types.

-   What are the pillars of OOP?

-   What are CDNs and how do they work?

-   Explain the Saga design pattern + one use case.

## Round 4 --- Client Round (Booking.com)

-   Scheduled but did not proceed --- the profile was kept on hold by
    the consultancy.


[⬆ Back to Index](#index)

<a id="hcl"></a>
# HCL

*Java Backend Developer \| 3 YOE \| Walk-in Drive*

## Round 1 --- Online Assessment (Coding Round)

-   Given an unsorted ArrayList, remove duplicates and print the result
    using only Streams; code had to compile successfully to be
    evaluated.

-   *Hint / Answer given: Lesson: always double-check your imports, even
    for logic you\'re confident about.*

## Round 2 --- Technical Interview: Core Java

-   Functional interfaces and their methods.

-   Why we use lambda expressions.

-   Can we use lambda expressions without a functional interface?

-   *Hint / Answer given: No --- lambdas rely on functional interfaces
    to provide the target type/context.*

-   Abstract class vs Interface.

## OOP & Design Principles

-   SOLID principles.

-   ACID properties (databases).

## Hibernate / JPA

-   Entity relationships --- one-to-one, one-to-many, and many-to-many
    mappings.

-   Difference between one-to-many and many-to-many relationships, and
    when to use each.

-   Common JPA methods.

## Spring / Spring Boot

-   Spring Bean lifecycle.

-   Commonly used annotations in Spring Boot.

-   \@RequestMapping vs \@PathVariable --- difference and use case.

-   Why and when we use \@RequestMapping.

## Microservices & Communication

-   Core components of microservices architecture.

-   Role of an API Gateway.

-   Inter-service communication --- how data transfer happens between
    two services.

-   Difference between synchronous and asynchronous communication, and
    when to use each.

-   Exception handling strategies.

-   Explaining the overall application flow end to end.

-   Note: almost every \'difference between X and Y\' question was
    followed up with \'when would you use it?\' --- and the interviewer
    went through the candidate\'s resume line by line.


[⬆ Back to Index](#index)

<a id="paytm"></a>
# Paytm

*4 rounds --- DSA, Core Java, Backend/Spring, Security, Multithreading,
System Design, Project Deep Dive*

## Round 1 --- DSA

-   HashMap: internal working, collisions, resizing, load factor,
    ConcurrentHashMap.

-   Linked List: Cycle Detection + Floyd\'s Algorithm.

-   Binary Tree: Preorder Traversal + Maximum Path Sum.

-   Binary Search: Koko Eating Bananas / minimum eating speed.

-   Focus was on approach, optimization, complexity and edge cases.

## Round 2 --- Core Java + Backend

-   Predicate, Function, Consumer, Supplier.

-   Lambda & Functional Interfaces.

-   equals() vs ==, hashCode().

-   String immutability & Immutable Classes.

-   Heap vs Stack.

-   Thread, Runnable, Callable.

-   ExecutorService & Thread Pool.

-   Race Conditions, synchronized, Lock, volatile.

-   Deadlock & prevention.

-   Optimistic vs Pessimistic Locking.

-   Kafka vs RabbitMQ, Consumer Groups, Partitions & Offsets.

## Round 3 --- Spring + Security

-   \@Transactional internals & Spring AOP.

-   Self-invocation problem.

-   Propagation & Isolation.

-   JWT authentication.

-   Access vs Refresh Token.

-   OAuth 2.0 & PKCE.

-   API security.

-   Production scenarios: slow APIs, DB CPU, retries, Circuit Breaker,
    traffic spikes.

## Round 4 --- Project + Design + Managerial

-   Detailed discussion around project architecture, Microservices,
    Kafka, API performance, database optimization, monitoring,
    distributed tracing, scalability and production issues.

-   Design patterns discussed: Singleton, Factory, Strategy, Observer &
    Builder, plus Quick Sort.

-   Managerial questions: \'Why Paytm?\', challenges, production bugs
    and handling pressure.

-   Biggest takeaway shared: the interview wasn\'t just about knowing
    the answer --- the discussion usually moved from theory to \'what
    would you do about it in production\'.


[⬆ Back to Index](#index)

<a id="ntt-data"></a>
# NTT Data

*Java (Kong) Developer \| 4+ Years Experience*

## Core Java & Multithreading

-   Runnable vs Callable.

-   When would you use Runnable and when would you use Callable?

-   What does Callable return?

-   Given this code: \`public String joinWords(String\[\] words) {
    String result = \"\"; for (String word : words) { result += word; }
    return result; }\` --- how would you optimize this code?

-   What is a Future in Java?

-   Why was CompletableFuture introduced?

-   What is the difference between Future and CompletableFuture?

## Spring Boot

-   How would you implement Global Exception Handling in Spring Boot?

-   What is the difference between .properties and .yml configuration
    files?

## Design Patterns

-   Explain the Singleton Design Pattern.

-   Write a Singleton class in Java.

## DSA / Problem Solving

-   Top K Frequent Elements.

## Experience B --- Java Developer Interview, 2026

-   How do you optimize a REST API?

-   How do you group and partition data using Collectors.groupingBy()
    and partitioningBy()?

-   Sequential Stream vs Parallel Stream --- how do they differ?

-   How does GC affect application performance?

-   When should you avoid using parallelStream()?

-   How would you maintain transaction consistency?

-   How do you handle database connection pool exhaustion?

-   How does thread-pool configuration affect performance?

-   What happens if the payment succeeds but your service crashes before
    updating the order?

-   How would you find duplicate elements, frequency of elements, or the
    second-highest value using Java Streams?

-   How would you generate unique short URLs, and how would you handle
    billions of URLs?

-   SQL vs NoSQL?

-   How would you handle expiration, and how would you prevent duplicate
    URLs?

-   How would you scale read-heavy traffic?

-   Explain one major production defect and how you fixed it.

-   How would you improve an API that receives 10,000 requests per
    second?


[⬆ Back to Index](#index)

<a id="infosys"></a>
# Infosys

*Multiple candidate experiences*

## Experience A --- Technical 1st Round (\~2 years experience)

-   What is Java?

-   What is inheritance in Java? Explain with a real-time example.

-   If Class B inherits Class A but Class B is not logically related to
    Class A, what problems can occur?

-   What is polymorphism? Explain compile-time and runtime polymorphism.

-   What are threads in Java?

-   What is the difference between a Thread and a Runnable?

-   What is the Java Collections Framework?

-   What is the difference between List, Set, and Map?

-   What is the difference between final, finally, and finalize()?

-   How can you reverse a String using Java Streams?

-   What is Spring Boot, and why is it important?

-   What are the advantages of using Spring Boot over traditional
    Spring?

-   What is Dependency Injection?

-   What are the different types of Dependency Injection?

-   What is a Spring Bean?

-   What are annotations in Spring Boot? Why are they used?

-   Have you used \@Transactional? Where and why did you use it?

-   Can you explain more about Spring Boot and how it works?

-   Have you worked with Spring Security?

-   Can you explain the basic JWT authentication flow?

## Experience B --- #infosys Java Developer Interview Questions \| 2026

-   What is the internal working of ConcurrentHashMap in Java 8?

-   Explain how JVM handles class loading and garbage collection.

-   What is the difference between volatile, synchronized, and Lock?

-   How does the ForkJoinPool work internally?

-   Explain immutability and how to create a truly immutable class.

-   What is the difference between \@Component, \@Service, and
    \@Repository?

-   How does Spring manage bean lifecycle and dependency injection
    internally?

-   Explain how \@Transactional works under the hood.

-   What is the difference between Spring MVC and Spring WebFlux?

-   How does Spring Boot auto-configuration work?

-   Explain the API Gateway pattern and how routing + filtering works.

-   How do you handle inter-service communication failures? (Retry,
    Circuit Breaker, Fallback)

-   What is service discovery? How does Eureka/Consul work internally?

-   Explain eventual consistency in microservices with an example.

-   How do you design idempotent APIs?

-   How do you optimize slow SQL queries?

-   Explain different isolation levels & the problems they prevent.

-   What is connection pooling & how does HikariCP optimize performance?

-   How does caching work in distributed systems?

-   How do you design high-performance REST APIs?

-   Design a URL Shortener / Payment Service / Notification Service.

-   Explain load balancing strategies (Round-Robin, Least Connection,
    Weighted).

-   How do you scale a microservices-based application?

-   What logging & monitoring stack would you choose and why?

-   How do you secure microservices (OAuth2, JWT, API Tokens)?

## Experience C --- Senior Associate Consultant (Java Backend Engineer), 3-round process

-   Round 1 --- Technical: professional experience (Java version, Spring
    Boot, Angular, frontend exposure); transactions in Spring Boot and
    how annotations work; flow of a client request till response in
    Spring; what is an API Gateway and how does request flow through it;
    how do you scale distributed systems; what is ApplicationContext in
    Spring; \@RestController vs \@Controller; project structure; SQL
    query for the 3rd highest salary per department; handling production
    issues with slower runtime; what to do on 5xx errors; global
    exception handling in Spring Boot; Circuit Breaker pattern;
    live-code the Singleton design pattern; Java 8 streams --- find
    duplicates, find employees in the same department; frontend comfort
    level.

-   Round 2 --- System Design (with Principal Engineer): scale 1M
    requests at a time; handle frequency of requests; optimize a
    database; shard a database and ensure resilience; master-slave
    architecture; monitoring & observability tools; deployment types;
    rollback strategies; preserving logs for production issues;
    documenting RCAs; Spring annotations (@Service, \@Repository,
    \@Valid); implementing in a multi-threaded environment using Java;
    the Executor framework; caching and how it\'s implemented with Redis
    in Spring.

-   Round 3 --- HR & Leadership: handling conflict of interest in a
    team; mentoring/training juniors; managing a team previously;
    willingness to work with commercial/investment bank clients;
    work-life balance; compensation discussion.

## Experience D --- Java Full Stack Developer Interview Experience

-   Java: Lambda Expressions; Stream API; transient vs volatile.

-   Spring Boot: commonly used annotations; how do you handle
    transactions; how do you handle exceptions; how does Spring Security
    work; how do you implement JWT authentication.

-   React.js: React Hooks; React Router; Redux and why it\\'s used;
    useRef(), useCallback() and useMemo() --- why are they used; explain
    BrowserRouter and useNavigate().

-   JavaScript: closures in JavaScript; hoisting in JavaScript.

-   SQL: different types of SQL joins; practical SQL/query-related
    questions.

## Experience E --- Java Backend Developer, 5--8 Years (32-Question Set)

-   Java: how does HashMap work internally in Java 8+; when would you
    choose ConcurrentHashMap over synchronizedMap; how do Streams handle
    intermediate and terminal operations; explain CompletableFuture
    chaining and exception handling; how do virtual threads differ from
    platform threads; how do JVM heap, stack and metaspace interact;
    compare G1, ZGC and Parallel GC for backend workloads; when would
    you use Records and Sealed Classes; how does pattern matching
    improve modern Java code.

-   Spring + Spring Boot: explain Spring Bean lifecycle and scopes; how
    does Spring Boot auto-configuration work; explain \@Transactional
    propagation and isolation levels; why can self-invocation bypass
    Spring AOP; how would you design global REST exception handling; how
    do Spring Boot 3 and Spring 6 differ from older versions; how would
    you secure REST APIs using Spring Security and JWT; how would you
    implement caching with Spring Cache; how would you expose
    application health and metrics.

-   Microservices: how would you design service-to-service
    communication; when would you choose Kafka over synchronous REST;
    how would you handle Kafka retries, ordering and idempotency; how
    would you implement resilience using timeouts, retries and circuit
    breakers; how would you trace a request across distributed services;
    how would you design an observable, highly available microservice.

-   Coding Questions: find the first non-repeating character using Java
    Streams; implement a thread-safe LRU cache with O(1) get and put;
    merge overlapping intervals and return the consolidated ranges.

-   Others: write SQL to find the second-highest salary per department;
    how would you deploy a Spring Boot service using Docker and
    Kubernetes; design a CI/CD pipeline with automated testing and
    rollback; how would you design cloud infrastructure using AWS, GCP
    or Azure; how would you approach security, caching and observability
    in system design.

## Experience F --- 2nd Round Technical Interview

-   Introduction & Project: tell me about yourself; explain your
    project; what are your roles and responsibilities; which
    technologies and features have you worked on.

-   Core Java & Coding: explain a Java coding problem/program; explain
    your approach and logic; questions based on Core Java concepts.

-   JSON & REST: what is JSON; why is JSON used; explain the structure
    of JSON with an example; how is JSON used in REST APIs.

-   MySQL & SQL: write/explain SQL queries; explain different types of
    SQL joins; basic database and query-related questions.

-   STLC: what is STLC; explain the different phases of the Software
    Testing Life Cycle.

-   Jenkins & CI/CD: what is Jenkins; how is Jenkins used in the
    development process; basic understanding of CI/CD.

-   Version Control: SVN vs Git; SVN workflow and commonly used
    commands; Git workflow and basic commands.

-   AI Tools: have you worked with AI tools such as GitHub Copilot,
    Claude, or similar tools; how have you used AI tools during
    development.

-   AWS: do you have experience with AWS; which AWS services have you
    worked with or are familiar with.


[⬆ Back to Index](#index)

<a id="tech-mahindra"></a>
# Tech Mahindra

*Senior Software Engineer --- Java Developer, Round 2*

## Round 2

-   It started with project details, tech stack and responsibilities;
    dug a bit into modules worked on and counter-questions like why
    particular libraries or tools were used.

-   How deployment works there.

-   We use an internal token (not JWT/OAuth) --- how is that configured
    and how does it work?

-   What is \@SpringBootApplication?

-   How do you exclude a JPA repository / embedded Tomcat (given the
    application uses a WebLogic server)?

-   What is the Saga pattern?

-   \@Primary and \@Qualifier (the diamond problem).

-   How to add a security filter in the filter chain.

-   Custom configuration in Spring Boot.

-   Custom exception handling and global exception handling in Spring
    Boot.

-   Role of properties and .yml files.

-   Sync vs async communication; what is a message queue.

-   Kafka --- how you produce an event.

-   What is RestTemplate; how do you consume an external API.

-   SQL Query --- joins, SQL query order, aggregation functions and
    window functions.

-   Candidate\'s note: interviewer focused mostly on project, Spring
    Boot, multi-threading, DB, and caching.

## Round 3 --- Java Backend Developer (3--5 Years)

-   Java Core: BufferedInputStream vs BufferedOutputStream; why is
    String immutable in Java; what are Filter Streams; what are Marker
    Interfaces --- can you name a few examples; what is Serialization,
    and where have you used it.

-   Spring Security & Authentication: how do you store passwords in the
    database; which encryption/hashing mechanism do you use for storing
    passwords; difference between Encryption and Decryption; what is
    RSA, and where is it used; difference between Authentication and
    Authorization; how is CSRF protection implemented; role of
    AuthenticationManager and AuthenticationProvider; explain JWT ---
    what it contains and its authentication flow; difference between
    OAuth and OAuth 2.0; explain \@PreAuthorize and \@PostAuthorize.

-   Spring Boot & Database: how would you configure multiple databases
    in a Spring Boot application.

-   Coding Question: find the number of trailing zeroes in the factorial
    of a given number.

-   *Candidate\\'s observation: the interview was more focused on
    understanding real-world backend development rather than simply
    recalling definitions --- expect clear explanations of security
    concepts, Spring Security internals, and practical implementation
    approaches.*


[⬆ Back to Index](#index)

<a id="goldman-sachs"></a>
# Goldman Sachs

*Java Backend interview --- worked Q&A dialogue on \@Transactional*

## \@Transactional deep-dive scenario

-   Your service saved an incomplete order even though \@Transactional
    is present. What do you check first?

-   *Hint / Answer given: Verify whether a transaction actually started
    --- \@Transactional is only an instruction to Spring; Spring applies
    it through a proxy. If the method call doesn\'t pass through that
    proxy, there is no transaction.*

-   When can that happen (no transaction despite \@Transactional)?

-   *Hint / Answer given: Self-invocation --- if one method calls
    another method in the same class, it\'s effectively a this.method()
    call and the proxy is bypassed. No exception, no warning --- just no
    transaction.*

-   What else should you check?

-   *Hint / Answer given: Method visibility, final methods/classes, and
    thread boundaries --- if work moves to a new thread, the original
    transaction context doesn\'t automatically follow.*

-   Transaction started, but rollback still didn\'t happen --- why?

-   *Hint / Answer given: Check the exception type: Spring rolls back by
    default only for RuntimeException and Error; checked exceptions
    don\'t trigger rollback by default. Verify rollbackFor and check
    whether a try-catch swallowed the exception.*

-   What if one transactional service calls another?

-   *Hint / Answer given: Check transaction propagation --- REQUIRED
    joins the existing transaction, REQUIRES_NEW starts a separate one.
    The wrong choice can leave business or audit data inconsistent.*

-   What about long-running transactions?

-   *Hint / Answer given: They can hold database connections for the
    entire transaction; if a REST call inside it takes 3 seconds, those
    connections remain occupied, and under load the connection pool can
    become a bottleneck.*

-   Overall checklist for this class of bug:

-   *Hint / Answer given: Proxy → Self-invocation → Visibility →
    Exceptions → rollbackFor → Propagation → Threads → Connection usage.
    The real question isn\'t \'did I add \@Transactional?\' --- it\'s
    \'did the transaction actually start, and what is inside it?\'*


[⬆ Back to Index](#index)

<a id="tcs"></a>
# TCS

*Walk-In Interview --- Java + Spring Boot*

## Core Java

-   String s1 = \"abc\"; String s2 = new String(\"abc\"); --- what will
    s1 == s2 return? What about s1.equals(s2)? Where will s1 and s2 be
    stored?

-   Using Java Streams, count the occurrence of each character and print
    only characters whose count is greater than 1.

## DSA

-   Check whether two strings are anagrams.

-   Given an Employee list, group employees by department and find the
    employee with the highest salary in each department.

-   Find the second-highest salary in each department using Java
    Streams.

## Spring Boot

-   How would you secure a REST API?

-   What annotations would you use for REST API security?

-   Difference between \@Controller and \@RestController.

-   How do you return JSON from a \@Controller?

-   How would you implement global exception handling in Spring Boot?

-   What is \@Transactional and why is it used?

## Microservices

-   What is throttling in microservices?

-   Why is throttling required in a microservices architecture?

-   How would you implement rate limiting/throttling for an API?

## DevOps / Release

-   What is a release cycle?

-   Have you been involved in application deployment?

## API Documentation

-   What API documentation tools have you used?


[⬆ Back to Index](#index)

<a id="accenture"></a>
# Accenture

*Senior Software Engineer --- questions across Java, Spring, Hibernate
and SQL*

## Maven and Build

-   What is a Maven build?

-   What does mvn clean install do?

-   How do you push code to production?

-   What is code coverage and line coverage?

-   How do you improve code coverage when a build fails?

## Testing

-   How do you verify a method is called twice in Mockito?

-   In which situations do you use PowerMock?

## Core Java

-   What is the difference between the final keyword and a final
    variable?

-   What is Garbage Collection in Java?

-   How do you debug and fix OutOfMemoryError?

-   What are atomic variables in Java?

-   What is the volatile keyword?

-   What is the difference between volatile and synchronized?

-   How do you avoid performance issues caused by synchronization?

-   What is a BlockingQueue and where do you use it in real
    applications?

-   What changes did you make when migrating from Java 7 to Java 8?

-   What are different ways to ensure thread safety?

-   What is ThreadLocal and where is it used?

## Collections

-   What is the difference between HashMap and Hashtable?

-   Why does Hashtable not allow null keys or values?

-   What is load factor in HashMap?

-   What does the default load factor of 0.75 mean?

## SQL

-   What is the difference between UNION and UNION ALL?

-   What is the difference between LEFT JOIN and RIGHT JOIN?

-   When do you use LEFT JOIN?

-   How do you find records in one table that have no relationship with
    another?

-   How do you handle NULL values and conditional mapping in SQL?

-   How do you map values like A to Apple, B to Banana in SQL?

-   How do you remove duplicates in SQL?

## Hibernate

-   How do you define relationships between tables in Hibernate?

-   What are the types of relationships in Hibernate?

## Spring

-   What is the difference between constructor injection and setter
    injection?

-   What happens if you do not use \@Autowired in Spring?

-   How do you perform constructor injection without \@Autowired?

## DSA

-   Find the longest substring without repeating characters in Java.


[⬆ Back to Index](#index)

<a id="virtusa"></a>
# Virtusa

*Java Backend Developer Interview Experience*

## Mixed theory & problem-solving

-   Tell me about yourself.

-   Explain your project and architecture.

-   Basics of System Design.

-   String and String Pool.

-   What will you do if the application suddenly stops working?

-   What is OutOfMemoryError and how do you fix it?

-   How does a Java application connect to a database?

-   What is Connection Pooling?

-   What is Multithreading?

-   What is Spring MVC?

-   Spring vs Spring Boot.

-   DDL, DML, DQL, TCL and DCL.

-   SQL queries in Oracle --- Joins, GROUP BY and HAVING.

-   Exception Handling and Production Issues.

-   REST API concepts.

-   Microservices vs Monolithic Architecture.

-   How does HashMap work internally?

-   Java 8 --- Stream API and Lambda Expressions.

-   \@Transactional and Dependency Injection.

## Experience B --- #Virtusa for #Citi Client, Senior Consultant

-   Spring Boot Features: main features used day-to-day.

-   What is a memory leak in Java if Garbage Collection is automatic?

-   How does the JVM determine whether an object is eligible for Garbage
    Collection? What are GC Roots?

-   Optimistic vs. Pessimistic Locking.

-   What are the most common causes of memory leaks in Java?

-   How can static collections cause memory leaks?

-   How can ThreadLocal cause a memory leak?

-   How can unbounded caches cause memory problems?

-   How can listeners and callbacks cause memory leaks?

-   What is the difference between a memory leak and high memory usage?

-   OutOfMemoryError vs StackOverflowError?

-   What is a Heap Dump? How do you capture a Heap Dump from a
    production JVM?

-   How do you analyze a Heap Dump? What is a Dominator Tree?

-   How do you identify objects consuming most of the heap? What is a
    Retained Heap?

-   Heap Dump vs Thread Dump?

-   Describe a complex scenario or problem you have faced recently, and
    what techniques you used to troubleshoot it.

-   How would you investigate memory issues in a Java application
    running inside Kubernetes?

-   *Candidate Selected.*


[⬆ Back to Index](#index)

<a id="mphasis"></a>
# Mphasis

*Java Developer \| Experience Level: 4--6 Years \| Interview Date:
23-Aug-2026 \| Virtual --- Technical Round*

## Core Java & Spring Boot

-   How would you optimize a slow Java service under production load?

-   How do you handle exception management in Spring Boot?

-   How would you diagnose high CPU or memory usage in a Java
    application?

-   How would you design a maintainable Spring Boot application?

## Microservices

-   How would you design resilient communication between microservices?

-   How do you handle service failure, retries, and timeouts?

-   How would you troubleshoot intermittent failures across
    microservices?

-   How would you make a microservice scalable and reliable?

## GCP

-   How would you deploy and scale a Java microservice on GCP?

-   How would you troubleshoot a production service running on GCP?

-   How would you design reliable cloud infrastructure for a banking
    application?

## SQL & NoSQL

-   How would you optimize a complex SQL query causing latency?

-   How do indexes affect query performance, and when can they hurt?

-   How would you investigate database bottlenecks in production?

-   When would you choose NoSQL over SQL for a microservice?

## GitLab CI/CD

-   How would you design a GitLab pipeline for build, test, and
    deployment?

-   How would you troubleshoot a failing GitLab CI/CD pipeline?

-   How would you implement safe deployments through CI/CD?

## Testing with Playwright

-   How would you structure integration tests using Playwright?

-   How would you handle flaky Playwright tests in CI/CD?

-   How would you test a critical banking workflow end to end?

## Design & Production Scenarios

-   How would you design a scalable, robust, and highly available Java
    system?

-   A production API suddenly becomes slow --- how would you isolate the
    root cause?

-   How would you design a system that remains stable during traffic
    spikes?

-   How would you handle a critical production incident while
    coordinating with stakeholders?


[⬆ Back to Index](#index)

<a id="paypal"></a>
# PayPal

*SDE-1 (Java Backend) \| Application via Referral \| Virtual Interview*

## DSA / Problem Solving

-   Grid pathfinding with \'+\' and \'0\' → check reachability &
    shortest path.

-   Streaming k-th largest element after each insertion.

-   Intersection point of two singly linked lists (O(1) space).

-   k-th element from the end in a generic linked list.

-   Modified Kadane\'s Algorithm → return start & end indices.

-   Reverse a linked list in groups of size k.

-   Remove duplicates from an unsorted linked list.

-   Count numbers ≤ N with an odd number of divisors.

-   Large-scale anagram check (billions of characters).

-   Sliding window: top 10 most frequent APIs in the last 10 minutes.

-   Minimum window substring containing all characters of a pattern.

-   Implement an LFU Cache with O(1) operations.

## Core Java & Concurrency

-   TreeSet with mixed types (int, String, Object) → compile vs runtime.

-   synchronized vs ReentrantLock vs StampedLock vs ReadWriteLock.

-   Implement a thread-safe LRU cache without ConcurrentHashMap.

-   Why does list.stream().map(List::stream) return List\<Stream\<T\>\>?

-   Why doesn\'t Java support pointers → impact on memory safety.

-   Explain the Java Memory Model & happens-before relation in
    concurrency.

-   Difference between HashMap, ConcurrentHashMap, and Hashtable.

-   Implement a producer--consumer system using BlockingQueue.

-   Explain volatile vs Atomic variables in multithreading.

## System / Low-Level Design

-   Design a digital wallet (transactions, consistency, scaling).

-   Design a rate limiter (per-user, per-API key).

-   Design a logging system (collection, indexing, querying).

-   Design a nearest ATM finder (location-based queries).

-   Handle a sudden 10x traffic spike in backend services.

-   Design a retry mechanism with exponential backoff.

-   Design a notification service (email/SMS) ensuring reliability &
    scalability.


[⬆ Back to Index](#index)

<a id="ey"></a>
# EY

*Two candidate experiences*

## Experience A --- Java developer, 2 YOE, 3 rounds

-   Round 1 -- Technical: Java fundamentals, OOPs, Collections, Stream
    API; Spring Boot, IOC; 2 DSA questions on Array (sliding window) and
    String (two pointer), easy to medium; multithreading; SQL queries
    like second-max salary. Mostly a 45-minute foundational round.

-   Round 2 -- Technical: current roles and responsibilities as
    developer; Spring Boot, Microservices; scenario-based questions on
    production live issues --- how to handle concurrent payments and
    idempotency; Kafka; Hibernate 1st- and 2nd-level caching, Redis,
    failure handling if Redis is down; multi-threading; Resilience4j.

-   Round 3 -- Techno-managerial + Client (3 panelists): real,
    depth-oriented round with lots of counter-questions. Live coding ---
    built a payment integration system; discussing requirements and
    multiple approaches with counter-questions; DB design; indexing; N+1
    queries problem; exception handling; garbage collector;
    ExecutorService.

## Experience B --- L2 Senior Java Backend role

-   DSA: sliding window problem to find the longest substring with at
    most K distinct characters; how to optimize the solution to O(n);
    how to handle character frequencies efficiently.

-   Java and Spring Boot: how HashMap handles collisions internally; two
    requests updating the same database record at the same time and how
    to handle the race condition; debugging an API when latency suddenly
    increases from 200ms to 5 seconds.

-   SQL and Database: finding the second-highest salary from an Employee
    table; optimizing a slow query running on millions of records;
    indexing, composite indexes, and query execution plans.

-   Microservices: what happens when a downstream service becomes slow
    or unavailable; Retry vs Timeout vs Circuit Breaker; how to handle
    duplicate requests.

-   System Design: designing a scalable payment/transaction backend
    APIs, database design, Kafka, Redis, idempotency, retries, and
    failure handling; what happens if a payment succeeds but the service
    crashes before sending the response.

## Experience C --- #BNY_Melon Client, Senior Consultant

-   Spring Boot Features: main features used day-to-day.

-   High CPU usage but low traffic --- what could be the reason.

-   Kafka Consumer Lag: consumer is slower than producer and the lag
    keeps increasing --- how to troubleshoot it.

-   Application slows down after running for a few hours --- what to
    check.

-   Frequent GC pauses --- how to optimize.

-   A HashMap causing performance issues under heavy load --- why.

-   Multiple threads updating shared data incorrectly --- how to fix it.

-   API works fine locally but fails in production --- what to
    investigate.

-   Suspected memory leak --- how to confirm it.

-   A service becomes unresponsive randomly --- what could be happening.

-   Deadlock in the system --- how to detect and resolve it.

-   Logs show inconsistent behavior across requests --- why.

-   Application crashes without any clear error --- how to debug it.

-   A database call is slowing down a Java service --- how to optimize
    it.

-   Thread pool gets exhausted under load --- approach.

-   Handling high concurrency safely --- what to use.

-   System processes duplicate requests --- how to handle it.

-   A cache is giving stale data --- how to fix it.

-   A thread stuck in BLOCKED state --- how to identify and fix it.

-   Tracing a request across multiple layers --- how to do it.

-   Application is not scaling even after adding instances --- why.

-   *Candidate Selected.*

## Experience D --- Technical Interview \| Spring Boot \| Microservices \| AWS \| Kafka

-   Java: difference between HashMap and ConcurrentHashMap; Java Memory
    Management and different JVM memory areas; what is the PC Register;
    String Pool vs Heap memory; what happens when we use \`new
    String(\"hello\")\` if \"hello\" already exists in the String Pool;
    == vs .equals() for String; ExecutorService vs CompletableFuture;
    does CompletableFuture create its own thread pool, and which pool
    does it use by default.

-   System Design / Microservices: design a fund transfer system capable
    of handling millions of transactions; what happens if Account A is
    debited but Account B\\'s credit fails; preventing duplicate
    transactions if the user clicks multiple times; ensuring
    transactions for an account are processed in order; how Kafka
    partitions help with ordering; which design avoids duplicate
    transactions and ensures correct processing; how the Saga pattern
    can be used for fund transfer, including the architecture and flow
    of a Saga-based fund transfer system; which database to choose and
    why; how microservices communicate with each other; REST vs Kafka;
    handling cascading failures in microservices; processing 1 lakh
    messages per second in Kafka.

-   Spring: flow of Spring MVC from request to response; the N+1 problem
    in Hibernate/JPA and how to solve it.

-   Security: difference and relationship between OAuth 2.0, JWT and
    OpenID Connect; JWT structure and the JWT authentication/validation
    flow.

-   AWS: difference between EC2, ECS and Lambda; monitoring applications
    in AWS; deploying microservices on AWS and explaining the deployment
    flow.

-   Database & Troubleshooting: identifying and resolving a database
    bottleneck; debugging a 401 Unauthorized error; debugging a 404 Not
    Found error.

-   Coding: given a list of account transactions such as
    A:+100,B:+200,A:-50,C:+300 --- calculate the final balance for each
    account.


[⬆ Back to Index](#index)

<a id="airtel"></a>
# Airtel

*Second Round --- Java & Microservices Interview Questions*

## Round 2

-   AOP Programming.

-   Managing distributed transactions.

-   Design patterns for microservices.

-   Securing APIs.

-   Transactional annotations.

-   SAGA Pattern.

-   HashMap enhancements in Java 8.

-   final vs finally vs finalize.

-   Handling memory leaks.

-   SOLID Principles.

-   Thread-safe Singleton code.

-   Microservices principles.

-   Circuit Breaker.

-   Constructor overloading.


[⬆ Back to Index](#index)

<a id="mastercard"></a>
# Mastercard

*Shared interview stories --- coding & design rounds*

## Coding --- Maximum Binary Tree

-   Build a Maximum Binary Tree from an array where every parent node is
    the maximum element of its subarray. (LeetCode 654: given an integer
    array nums with no duplicates, build the tree recursively ---
    root\'s value is the maximum in nums; left subtree is built from the
    subarray prefix left of the max; right subtree from the subarray
    suffix right of the max.)

-   *Hint / Answer given: What Mastercard was really testing: Divide &
    Conquer, Binary Trees, and Monotonic Stack optimization. Key
    insight: apply divide & conquer recursively; as a follow-up, the
    solution can be optimized to O(n) using a Monotonic Stack.*

## SDE-2 (Java Backend) --- 30-Question Set

## DSA

-   Longest Subarray with Sum = K --- Prefix Sum + HashMap, O(n).

-   Merge Overlapping Intervals --- Sorting + Merging, O(n log n).

-   Design LRU Cache --- HashMap + Doubly Linked List, O(1) per
    operation.

## Core Java & Object-Oriented Concepts

-   Difference between interface and abstract class (with Java 8
    default/static methods).

-   Explain the Java Memory Model, Heap vs Stack, and the Garbage
    Collection process.

-   Difference between String, StringBuilder, and StringBuffer.

-   Explain the equals() and hashCode() contract with a real-world
    example.

-   What are functional interfaces and lambdas in Java 8?

-   Difference between Runnable, Callable, and Future in multithreading.

-   Explain how synchronization and locks work in Java.

## Spring Boot / Microservices

-   What is the Spring Boot autoconfiguration mechanism?

-   Difference between \@Component, \@Service, \@Repository, and
    \@Controller.

-   How do you secure REST APIs in Spring (JWT/OAuth2)?

-   Explain Spring Boot Actuator and its use in monitoring.

-   How do you implement exception handling in REST APIs using
    \@ControllerAdvice?

-   Explain the Bean lifecycle in Spring and the role of
    \@PostConstruct/@PreDestroy.

-   How does Spring Dependency Injection work under the hood?

-   How do you handle circular dependencies in Spring Boot?

## System Design & Architecture

-   Design a Payment Processing System handling millions of transactions
    per second.

-   How do you ensure idempotency in APIs (important for payments)?

-   What\'s the difference between monolithic and microservices
    architectures?

-   Explain API Gateway and its responsibilities.

-   How do you achieve fault tolerance, retry mechanisms, and circuit
    breakers?

-   Explain event-driven architecture with Kafka or similar.

-   What is horizontal vs vertical scaling, and when to use each?

## Database & Transactions

-   Explain ACID properties in relational databases.

-   What are indexes and how do they improve query performance?

-   How do you handle concurrent updates (e.g. using
    optimistic/pessimistic locking)?

-   Difference between SQL and NoSQL databases; when to choose each.

-   How do you detect and resolve deadlocks or performance bottlenecks?

## Race Condition & Concurrency (Technical Round, September 2026)

-   What is a race condition and when does it occur?

-   What is the solution to prevent race conditions?

-   Difference between putIfAbsent and computeIfAbsent in
    ConcurrentHashMap.

-   If two methods are synchronized but you still get a race condition,
    why does that happen, and how would you investigate it?

-   In which scenario would you use a big HashMap?

-   Is there any possibility of getting a deadlock situation in HashMap?

-   Does ConcurrentHashMap allow null values? Why does it not allow null
    keys or null values?

-   How would you investigate an application that normally responds in
    100 ms but is now taking 4--5 seconds, including checking the
    database side and indexes?

-   What issues arise when a HashMap is shared by multiple threads in
    production, even if it works fine in testing --- why would requests
    time out even though the application isn\\'t crashing, and why might
    all requests time out with exceptions even though the application is
    running?

-   In which scenarios will you get concurrency issues?

-   What branching strategies are available in GitHub, and how can you
    configure branch protection rules?

-   What databases have you worked with? How does PostgreSQL handle MVCC
    differently from MySQL?

-   Do you know what cascade type as well as orphan removal are?

-   Have you used any locking mechanism in your code, like database-
    level locking?


[⬆ Back to Index](#index)

<a id="fincart"></a>
# Fincart

*Java Backend Developer Interview Experience (shared via
InterviewRecap)*

## Round 1 --- Java Core

-   Self Introduction.

-   Day-to-day responsibilities in your current role.

-   Have you worked on High-Level Design (HLD) and Low-Level Design
    (LLD)?

-   Which version of Java are you using in your current project?

-   What are the major features introduced in Java 8?

-   Why do we use Functional Interfaces in Java?

-   What is the purpose of Default Methods in interfaces?

-   What is the purpose of Static Methods in interfaces?

-   Explain a real-time use case of Default Methods in Java.

-   Explain a real-time use case of Static Methods in interfaces.

-   What is the difference between Default Methods and Static Methods in
    interfaces?

-   Can we make a class private?

-   Why do we make methods private in Java?

-   Why do we use a private constructor?

-   Can we make a class static in Java?

-   How do you instantiate or call a static nested class?

-   How do you instantiate or call a non-static inner class?

-   Write the implementation of a Singleton class.

-   How does the Singleton design pattern ensure only one object is
    created?

## Round 2 --- Java Collections

-   Why do we use ConcurrentHashMap in Java?

-   How is ConcurrentHashMap different from HashMap?

-   Where have you used ConcurrentHashMap in your project?

## Round 3 --- Spring Boot

-   (Details not fully captured in the shared post.)


[⬆ Back to Index](#index)

<a id="nagarro"></a>
# Nagarro

*Software Developer \| 1--2 YOE \| Technical Interview (after OA)*

## Core Java, Collections, Concurrency

-   HashMap with a custom object --- put(s, \"A\") then get(s1) with an
    equal-but-different Student instance. What will be the output? What
    happens if equals() is overridden?

-   String as a HashMap key --- put(s,\"A\") with s=\"ABC\", then
    get(s1) with s1=\"ABC\" --- why does this return \"A\"?

-   *Hint / Answer given: Because of the String pool / String
    immutability and equal hashCode/equals for identical literal
    content.*

-   HashMap internal working and implementation.

-   equals() vs == vs ===.

-   Can Wrapper Classes be extended?

-   Synchronized HashMap vs ConcurrentHashMap.

-   Handling two users updating the same data simultaneously
    (concurrency).

-   Java Stream API --- Intermediate vs Terminal Operations.

-   N+1 Problem --- what is it and how would you handle it?

-   Cascade in JPA.

## Spring Boot & Other

-   \@Autowired in Spring.

-   Circular Dependency.

-   CORS.

-   PostgreSQL vs MySQL.

-   Git Rebase vs Merge-base.

-   Mockito when() and Assertions.

## Experience B --- Java Developer, 5--7 Years

-   Round 1 --- Online Assessment: MCQ-based assessment consisting of
    aptitude questions, logical reasoning, general technical questions,
    and Core Java fundamentals.

-   Round 2 --- Technical Coding Interview: given an array containing
    only 0s and 1s, move all 1s to the left and all 0s to the right,
    solved specifically using the Two Pointer approach, with a
    discussion of the time and space complexity; given an integer, find
    the sum of all its digits using Java Stream API; predict the output
    of a Java exception-handling code with try, multiple catch blocks
    (ordered NullPointerException → Exception → ArithmeticException),
    and finally --- including whether the program compiles with the
    catch blocks in that order, and what the correct ordering of catch
    blocks should be for parent and child exception classes.

-   Key areas to prepare (per candidate): array coding problems, the Two
    Pointer pattern, time & space complexity, Java 8 Stream API,
    IntStream and chars(), Core Java fundamentals, exception handling
    and exception hierarchy, multiple catch block ordering, and
    output-based Java questions.

## Experience C --- Java Technical L1, Nagarro Saudi Arabia

-   Write Java 8 code to find employees earning more than 5000.

-   Explain the Saga Pattern. What are its different types?

-   Explain the Circuit Breaker Pattern. When would you use it?

-   What are the different Spring Bean Scopes? Where have you used them?

-   What is Lazy Loading, and how does it work? If the database is
    initially empty, how would you populate/refresh the data?

-   What is \@SpringBootApplication?

-   What is the JPA Criteria API and when would you use it?

-   Interface vs Abstract Class --- when would you use each, especially
    after Java 8 introduced default methods?

-   A table grows from thousands to millions of records --- how would
    you maintain performance? Would database indexing help, and how does
    an index work internally?

-   What are you using for Authentication & Authorization? How would you
    implement Role-Based Authorization?

-   How do you perform an effective Code Review?

-   What is your experience with CI/CD? Which tools have you used?

-   Maven or Gradle --- what do you use and why?

-   Do you know Kubernetes? How have you used it?

-   What Cloud services/features have you worked with?


[⬆ Back to Index](#index)

<a id="slice"></a>
# Slice

*SDE 2 Interview Experience (shared from LeetCode Discuss)*

## Round 1 --- Machine Coding (90 mins)

-   Build a Wallet System: credit money to wallet, debit money from
    wallet, transfer money between users, API to fetch wallet balance,
    API to fetch user transactions.

## Round 2 --- HLD (1 hour)

-   Design a Ticket Booking System similar to BookMyShow: system
    architecture, database design, API design, concurrent booking
    requests, preventing double booking, concurrency and race
    conditions, scalability and failure scenarios.

## Round 3 --- Hiring Manager (1 hour)

-   Past project experience; deep dive into projects and
    responsibilities.

-   Scaling or expanding a project.

-   Engineering and behavioral questions.

-   Situational and HR questions.


[⬆ Back to Index](#index)

<a id="jpmorganchase"></a>
# JPMorganChase

*30 LPA-level preparation checklist for a Java Backend role*

## Core Java

-   How do you sort a Map?

-   Write a Singleton class.

-   Comparable vs Comparator.

-   Features of Java 7, 8, 11, and 17.

-   What is try-with-resources?

-   What is a multi-catch statement?

-   Runnable vs Callable.

-   Types of exceptions and the Java exception hierarchy.

-   Different Design Patterns in Java.

-   OOP concepts.

-   ConcurrentHashMap internals.

-   How does Java Garbage Collection work?

## DSA & Coding

-   Find duplicate strings in a list.

-   Check whether a string is a palindrome.

-   Combination Sum II.

-   Using Java Streams: remove odd numbers, multiply remaining numbers
    by a constant, and calculate the sum.

-   Find the missing integer in a consecutive array.

-   Move all zeroes to the end of an array.

-   Check whether two strings are anagrams.

-   Find the longest common prefix.

-   Longest Increasing Subsequence.

-   Best Time to Buy and Sell Stock.

-   Dijkstra\'s Algorithm.

-   Coin Change Problem --- minimum coins.

-   Reverse-Add Palindrome problem.

## Database & SQL

-   How do you find the number of tables and their columns in a
    database?

-   What is the purpose of a database index?

-   How do you identify duplicate rows in SQL?

-   How would you design a schema for a ride-sharing application?

## Web & Frameworks

-   REST vs SOAP.

-   What is Spring? Why do we use Spring?

-   Spring vs Spring Boot.

-   How does autowiring work internally?

-   What is Spring Security?

-   What is a RESTful API?

-   HTTP vs HTTPS.

## System Design

-   Explain the architecture of one of your recent projects.

-   Design a fraud detection system for transactions.

-   Design the database for a ride-sharing application.

-   Design a data warehouse for an online retailer.

-   Design a news aggregator.

## Server & JVM Troubleshooting

-   How do you find the reason behind a server crash?

-   How do you check server memory usage?

-   How do you debug high CPU or memory issues in a JVM application?

-   How do you capture and analyze heap dumps and thread dumps?

## High-Frequency Checklist --- Java Spring Boot, 3--5+ Years

-   Core Java: HashMap vs ConcurrentHashMap; how does HashMap work
    internally; String vs StringBuilder vs StringBuffer; Comparable vs
    Comparator; Runnable vs Callable; volatile vs Synchronized; wait()
    vs sleep() vs join(); explain the Java Memory Model; how to avoid
    deadlocks; Exception vs RuntimeException.

-   Spring Boot: how does Spring Boot auto-configuration work; Bean
    lifecycle in Spring; \@Component vs \@Service vs \@Repository;
    \@Controller vs \@RestController; Constructor Injection vs Setter
    Injection; circular dependency in Spring; \@Transactional and
    propagation levels; Spring Security authentication vs authorization;
    profiles in Spring Boot; global exception handling using
    \@ControllerAdvice.

-   Hibernate/JPA: Lazy vs Eager loading; the N+1 problem and solutions;
    first-level vs second-level cache; optimistic vs pessimistic
    locking; pagination in JPA; persist() vs merge().

-   Microservices: monolith vs microservices; Service Discovery; API
    Gateway; Circuit Breaker pattern; distributed transactions;
    synchronous vs asynchronous communication; Kafka consumer lag
    handling; idempotency in APIs.

-   SQL: find the 2nd/Nth highest salary; joins (inner, left, right);
    indexing; ACID properties; query optimization; departments with zero
    employees query.

-   System Design: design a Payment System; design an Order Management
    System; scale a Spring Boot service for 1M users; rate limiting;
    caching with Redis; high availability and failover.


[⬆ Back to Index](#index)

<a id="barclays-capgemini-citi-infosys-jpmorganchase-tcs"></a>
# Barclays / Capgemini / Citi / Infosys / JPMorganChase / TCS

*Combined set of Java Backend Developer interview questions asked across
these organizations*

## Core Java & Language Features

-   What are the key features of Java 17?

-   What is the var keyword? Can it be used with generics?

-   What is an effectively final variable in Java?

-   What is a Record class? How do you map query results to Records and
    fetch the first record?

-   What is Optional? When should it be used?

-   What are checked exceptions? Their pros and cons?

-   How does exception handling work in Spring Boot? What is
    \@ControllerAdvice?

-   How do you reverse a String without using built-in methods?

-   Can mutable objects be used as HashMap keys? What issues can arise?

## Interfaces & OOP

-   Can interfaces have private methods? Why are they used?

-   What types of methods are allowed in an interface?

-   What if two interfaces have the same default method? How do you
    resolve it?

-   What are SOLID principles?

-   What is LSP (Liskov Substitution Principle)?

-   What is Factory Design Pattern?

## Concurrency & Multithreading

-   synchronized vs ReentrantLock.

-   What is volatile in Java?

-   volatile vs Atomic classes (AtomicInteger, AtomicLong)?

-   What is CompletableFuture in Java?

## Streams & Functional Programming

-   Difference between sequential and parallel Streams? When to use
    Parallel Streams in production?

-   Difference between map() and flatMap() with example.

## Spring Framework & Spring Boot

-   How does the Spring Container work?

-   What is Bean Lifecycle in Spring?

-   What is \@EnableAutoConfiguration?

-   Difference between \@Qualifier and \@Primary?

-   What is IoC vs DI?

-   What is Dependency Injection? Types?

-   What are Spring Profiles?

-   \@PathVariable vs \@RequestParam?

-   How does \@Transactional work?

-   Lazy vs Eager Loading?

## Spring Security & JWT

-   What is Spring Security?

-   How do you implement authentication & authorization in Spring
    Security?

-   How is Spring Security handled in microservices architecture?

-   What is JWT and how is it used for security in your projects?


[⬆ Back to Index](#index)

<a id="hughes-systique-corporation-hsc"></a>
# Hughes Systique Corporation (HSC)

*Java Backend Developer --- Round 2 Interview Experience, Gurugram*

## Java & Multithreading

-   Two-thread problem: one thread prints odd numbers and another prints
    even numbers sequentially from 1--10.

-   synchronized, wait(), and notify().

## Microservices

-   Microservices design patterns.

-   How to implement Service Discovery using Eureka.

-   API Gateway and inter-service communication.

## Apache Kafka

-   Kafka partitions and round-robin partitioning.

-   Message ordering across multiple partitions.

-   How to maintain ordering using Kafka message keys.

## Database and SQL Performance

-   How to analyze a slow-running SQL query.

-   EXPLAIN vs EXPLAIN ANALYZE.

-   Understanding execution plans.

-   Index usage, full table scans, rows examined, and query
    optimization.


[⬆ Back to Index](#index)

<a id="epam"></a>
# EPAM

*Two candidate experiences*

## Experience A --- Java Developer Interview

-   Coding & Java Streams: arrange elements around a given threshold ---
    smaller elements first, larger elements after; convert \"I love
    Java\" → \"#ILoveJava\" using Streams; find anagrams using Java
    Streams.

-   Core Java: HashMap & HashSet internals; treeification; wait() vs
    sleep() vs yield(); Thread vs Runnable; thread & multithreading;
    ReentrantLock vs synchronized; Java Memory Model (JMM); garbage
    collection basics; design a Thread Pool.

-   Spring Boot & Security: \@Configuration, \@Service,
    \@SpringBootApplication --- output-based questions; how do Spring
    Boot annotations work internally; Global Exception Handling; Spring
    Security; Authentication vs Authorization.

-   Microservices: microservices design; designing scalable and reliable
    services.

## Experience B --- SDET / Automation Round (Java + Selenium)

-   Round 1: find the last non-repeating character from a string; use
    of Comparable and Comparator (write Java code); Selenium 4 new
    features; Page Object vs Page Factory; meaning of :: in Java
    streams; where Java streams have been used in the framework;
    handling elements inside shadow DOM; how to throw a custom exception
    in Java; write Selenium code to handle multiple windows; implement
    serialization and deserialization (live-coded on the editor provided
    during the interview); implement OOP concepts for a Savings Bank;
    Page Factory design pattern; Singleton design pattern; where to use
    LinkedList concepts in automation; exception hierarchy in Java;
    checked exception examples; Fail-Safe vs Fail-Fast iterators; when
    to use ConcurrentHashMap; ObjectMapper in REST Assured; meaning of a
    405 status code in Postman, what it means, and how to resolve it.

-   Round 2: how to execute only regression-related tests in TestNG; how
    to read data from the Examples table section in Cucumber without
    Scenario Outline (answer: DataTable); how to perform parallel
    execution in a Cucumber framework; conditional hooks vs directional
    hooks in Cucumber; CI/CD Jenkins process --- how to run Selenium
    tests; Continuous Integration vs Deployment difference; custom
    reporting (Extent Report and Allure) --- write code; why use logging
    (Log4j2) in a Selenium framework; different log levels in Log4j2;
    use of Appenders in logging (Log4j2); root cause of flakiness in
    Selenium scripts --- how to fix, how many ways; explain Root Cause
    Analysis techniques; use of the git cherry-pick command; branching
    strategy in Git; estimation techniques in Agile; metrics to track
    project progress in Agile; explain a Test Plan document; difference
    between Test Plan and Test Strategy; prepare manual test cases &
    explain test design techniques; how to calculate defect leakage;
    explain defect leakage meaning; explain the Test Pyramid concept;
    DDL vs DML explain; use of Subqueries in SQL; given an array of
    cars, capitalize all car names with length \> 3 using Java 8
    concepts (streams) --- write code; write a method of the
    IAnnotationTransformer interface in TestNG; explain SQL Joins.


[⬆ Back to Index](#index)

<a id="kotak-mahindra-bank"></a>
# Kotak Mahindra Bank

*SDE-2, Backend (Java) \| Under 4 Years Experience \| Compensation 25--30
LPA \| 3 Rounds + HR*

## Round 1 --- Bar Raiser (90 min)

-   DSA (40 min): three travel passes --- 1 day, 7 day, 30 day, each
    with its own cost. Given an array of travel days, find the minimum
    spend that covers every day.

-   Kafka (20 min, as a discussion): producers, consumers, partitions,
    offsets; what a consumer group does with an offset; rebalance.

-   LLD (30 min): class responsibilities, and how the design extends
    without breaking existing callers.

## Round 2 --- DSA + Project

-   Given a JSON string, count the nested objects --- {{}} returns 2.

-   For n up to 10\^9: compute 2\^n, then keep summing the digits until
    one digit is left; brute force fails before n = 100.

-   Current project in depth --- why that design, what broke, what would
    be changed.

## Round 3 --- LLD in an IDE

-   Build a music player: play, next, previous, repeat one song, repeat
    the playlist. Working code, not diagrams.

-   Follow-up: too many if-else branches --- refactor with a design
    pattern. (Strategy was the answer given. State was the answer
    wanted.)

## HR

-   All three rounds cleared. Under 4 years of experience, so the offer
    was revised to SDE-1. Status: Declined.


[⬆ Back to Index](#index)

<a id="wissen"></a>
# Wissen

*Java Backend \| 13 YOE*

## Key Questions

-   How would you design an immutable class in Java?

-   Implement a custom Iterator and a custom Exception.

-   How do you decide the number of Kafka partitions?

-   Design a system that guarantees per-user event ordering while
    maintaining high throughput.

-   Using Java Streams, process transaction data and find the Top 3
    clients by transaction amount.

-   Analyze a multithreading scenario involving synchronized instance vs
    static methods.

-   Production API latency increased from 200ms → 5 seconds --- how
    would you troubleshoot it?

-   How would you handle Kafka poison messages, retries, DLT/DLQ and
    offsets?

-   Explain Saga, Transactional Outbox, CDC and Distributed Locking.

-   How would you design Kafka-based credit/debit processing with
    ordering and high throughput?

-   Explain HashMap vs ConcurrentHashMap vs LinkedHashMap.

-   Design a Distributed Log Aggregation System.


[⬆ Back to Index](#index)

<a id="iris-software-group"></a>
# IRIS Software Group

*Senior Java Developer Interview*

## Java 8 / 17 / 21

-   What improvements were introduced in Java 17 for Garbage Collection?
    What were the GC improvements in Java 8?

-   What is Stream API? How does Stream API work internally?

-   How many times can a Stream be consumed? What happens if we try to
    reuse a consumed Stream?

-   How can a non-thread-safe collection be used with Stream API?

-   What are the different types of thread pools in Java?

## Spring / Spring Boot

-   Where would you use \@Async?

-   What is Spring Cache?

-   Difference between \@Autowired and \@Qualifier.

-   What happens when multiple beans of the same type are available?
    Can two beans have the same name?

-   Difference between BeanFactory and ApplicationContext.

-   How would you handle application errors in production? How would
    you handle an OutOfMemoryError?

## Microservices / Kafka

-   What are the different communication mechanisms between
    microservices?

-   How would you implement asynchronous communication between Service
    A and Service B? What code is required to make service-to-service
    communication asynchronous?

-   Where are Kafka partitions used? How many partitions can a Kafka
    topic have?

-   What would you do if a Kafka consumer gets stuck in production? How
    would you handle consumer lag or a consumer that isn\\'t processing
    messages?

-   How do you trace a request across Service A → Service B → Service
    C? How is the Trace ID propagated between services?

## PostgreSQL

-   How does PostgreSQL handle concurrency?

-   What are transaction isolation levels?

## AWS / Kubernetes / DevOps

-   What is the maximum size of a single S3 object?

-   What would you do if a production deployment pipeline fails midway?

-   Where would you check CI/CD pipeline logs? Where would you check
    Kubernetes pod logs?

-   Which tools can be used for centralized production logging?

## Testing

-   What is the difference between Mockito \@Mock and \@Spy?


[⬆ Back to Index](#index)

<a id="visa"></a>
# Visa

*Software Engineer Role --- 3-Round In-Person Interview*

## Round 1 --- System Design (\~1.5 hours)

-   Task: Design a payment gateway system. Applied the Strategy Pattern
    --- designed a common interface, explained how it avoids tightly
    coupled code, and added support for multiple payment options (UPI,
    credit card).

-   Follow-up: explained the flow using Spring Boot; discussed
    challenges of integrating with Amazon Payment Gateway.

## Round 2 --- DSA (1 hour, with Senior Engineering Lead)

-   Next Greater Element → similar pattern to LeetCode 739: Daily
    Temperatures --- first brute force approach, then optimized with
    stacks.

-   Rotten Oranges → LeetCode 994: Rotting Oranges --- solved on
    whiteboard, explained BFS approach, then executed code.

## Round 3 --- Managerial (Director of Business Unit)

-   Work done in past projects and follow-up questions on design
    principles.

-   Frameworks used versus better design decisions.

-   Database exposure and relevant tools used for optimisation.

-   Implementation of Factory Design Pattern and other patterns.

-   General questions: location, background.

-   Role alignment --- focused more on database migration support than
    full stack development.

-   *Outcome: each round was elimination-based with quick feedback; the
    candidate didn\\'t move forward with the role.*


[⬆ Back to Index](#index)

<a id="lt"></a>
# L&T

*A Tricky DSA Question --- Topological Sort*

## Question

-   Given a list of project dependencies where each task depends on
    other tasks, find a valid order to complete all tasks. If it\\'s
    impossible because of a dependency cycle, return an empty list.

-   Example: numTasks = 4, dependencies = \[\[1,0\], \[2,0\], \[3,1\],
    \[3,2\]\] → output \[0,1,2,3\].

-   *Hint / Answer given: Optimal approach is Topological Sort + BFS ---
    build the dependency graph, calculate the in-degree of every task,
    start with tasks having in-degree = 0, process them using a Queue,
    reduce the in-degree of dependent tasks; if all tasks are processed
    it\\'s a valid ordering, otherwise a cycle exists. Time complexity
    O(V + E), space complexity O(V + E).*

-   Follow-up questions: how would you detect a cycle; can you solve it
    using DFS instead of BFS; what if there are multiple valid orders;
    how would you handle 10\^5+ tasks; what if dependencies change
    dynamically; how would you design this for a real-world task
    scheduling system.


[⬆ Back to Index](#index)

<a id="hotfoot"></a>
# Hotfoot

*Java Backend Developer \| 5+ Years Experience*

## Technical Interview Questions

-   How do you handle a production issue? Explain your complete approach
    step by step.

-   How do you identify whether a production issue is coming from the
    application, database, Kafka, or a downstream service?

-   After resolving a production issue, how do you perform Root Cause
    Analysis and prevent the same issue from happening again?

-   How many types of Garbage Collectors are there in Java?

-   Suppose you receive a requirement to create a new REST API endpoint.
    How would you proceed from requirement gathering to production
    deployment? Explain the complete process step by step.

-   What functional and non-functional requirements would you clarify
    before developing a new REST API?

-   Suppose there is an existing REST endpoint. Explain the complete
    request flow from the client to the database.

-   Explain what happens internally when a request reaches a Spring
    Boot application and how it travels through Controller, Service,
    Repository, Hibernate, JDBC, and the database.

-   How have you used multithreading in your project? Explain a real use
    case.

-   Suppose a REST API receives a payload containing 100 records and
    your ThreadPoolExecutor has only 8 threads. How would you process
    all 100 records and ensure that the final response is returned only
    after all records are processed? What happens internally when 100
    tasks are submitted to a ThreadPoolExecutor with only 8 worker
    threads?

-   What is the difference between Future.get() and CompletableFuture?
    Is Future.get() synchronous or asynchronous?

-   Why would you use a custom ExecutorService with CompletableFuture?

-   What makes the FinTech domain technically different from other
    domains?

-   How would you prevent duplicate financial transactions if the client
    times out but the server successfully completes the transaction and
    the client retries the same request?

-   How do you maintain data consistency in your application? How do you
    maintain consistency within a single database transaction? How do
    you maintain data consistency across multiple microservices?

-   How do you achieve exactly-once business processing when the
    messaging system guarantees at-least-once delivery?

-   Which Design Patterns have you used in your project? Explain with
    real project examples. Where have you used the Singleton Design
    Pattern in your project? Should a database Connection object be
    Singleton? Why or why not?


[⬆ Back to Index](#index)

<a id="part-1b-interview-experiences-where-the-company-was-not-named"></a>
# Part 1b --- Interview Experiences Where the Company Was Not Named


[⬆ Back to Index](#index)

<a id="unnamed-company-12-lpa-backend-developer-offer"></a>
# Unnamed Company --- \"₹12 LPA Backend Developer Offer\"

*Friend\'s interview --- 15 production-scenario questions, no theory
recitation*

## Production Scenario Questions

-   Your API suddenly becomes 5x slower. How do you debug it?

-   CPU usage is normal, but response time is high. What could be the
    reason?

-   Database connection pool is exhausted. What would you check first?

-   Two users update the same record at the same time. How do you handle
    it?

-   The same payment request arrives twice. How do you prevent duplicate
    payments?

-   A downstream service takes 10 seconds to respond. What would you do?

-   Retries are creating a traffic spike. How would you prevent a retry
    storm?

-   Kafka consumer lag keeps increasing. How would you investigate?

-   A Kafka message fails after consumption. How do you handle it?

-   Your application suddenly throws OutOfMemoryError. What would you
    check?

-   The database query is fast, but the API is still slow. Where would
    you look?

-   One microservice goes down during a critical transaction. What
    should happen?

-   A scheduled job executes twice. How would you make it safe?

-   You need to change an API without breaking existing clients. How?

-   Traffic suddenly increases by 10x. How would you prepare the system?


[⬆ Back to Index](#index)

<a id="unnamed-service-based-company-1st-technical-round"></a>
# Unnamed Service-Based Company --- 1st Technical Round

*36-question set --- covers \~80--90% of a typical first round for
Java/Spring Boot roles at service companies (Infosys, TCS, Wipro, HCL,
Cognizant, Accenture, Capgemini-type)*

## Java

-   Difference between String, StringBuilder, and StringBuffer.

-   What is the difference between HashMap and ConcurrentHashMap?

-   Explain OOP concepts with examples.

-   What is the difference between ArrayList and LinkedList?

-   What is Exception Handling? Checked vs Unchecked exceptions.

-   What is Multithreading? How does synchronization work?

## Spring Boot

-   What are Spring Boot starters?

-   Difference between \@Component, \@Service, and \@Repository.

-   What is Dependency Injection?

-   Explain Spring Boot application flow.

-   What is Actuator?

-   How do you handle exceptions globally?

## Database / SQL

-   Difference between DELETE, TRUNCATE, and DROP.

-   What are indexes and why are they used?

-   Difference between COUNT(\*), COUNT(1), and COUNT(column)?

-   Write a query to find the 2nd highest salary.

-   What are joins? Explain all types.

-   What is normalization?

## Microservices

-   What is Service Discovery?

-   How do microservices communicate?

-   What is an API Gateway?

-   What is Circuit Breaker?

-   What challenges have you faced in microservices?

## REST API

-   Difference between GET, POST, PUT, and DELETE.

-   What is JWT authentication?

-   What is CORS?

-   What are HTTP status codes?

## Scenario-Based Questions

-   Your API suddenly becomes slow. What will you check first?

-   A production issue occurs at midnight. How will you investigate?

-   Database CPU is low but API latency is high. What could be the
    reason?

-   How would you handle 100K requests per minute?

-   A critical service goes down during business hours. What steps would
    you take?

## Project-Based Questions

-   Explain your current project architecture.

-   What was the most challenging bug you fixed?

-   How did you improve application performance?

-   What is your role in deployment and production support?


[⬆ Back to Index](#index)

<a id="unnamed-company-position-java-backend-developer"></a>
# Unnamed Company --- \"Position: Java Backend Developer\"

*Experience Band: 4--8 years \| 25 questions in Round 1 alone*

## Round 1 --- A. Java Core and Collections

-   Explain HashMap internal working.

-   How does hashing work internally?

-   How are collisions handled?

-   What happens during resizing (rehashing)?

-   Default initial capacity and load factor?

## Round 1 --- B. Exception Handling

-   What are custom exceptions in Java?

-   How do you handle exceptions in Spring Boot?

-   Predict the output of a snippet using Collections and exception
    handling.

-   Explain the output and identify the issue in the code.

## Round 1 --- C. Iteration and Concurrency

-   What happens if a collection is modified while iterating?

-   How does ConcurrentModificationException occur?

-   Write code demonstrating both fail-fast and fail-safe behaviour.

## Round 1 --- D. Design Principles and Data

-   Explain all five SOLID principles.

-   Which ones have you actually implemented in your project?

-   Explain ACID properties with a banking transaction example.

-   How does rollback work?

## Round 1 --- E. Resilience

-   What are the different states of a circuit breaker?

-   Which library did you use to implement it?

## Round 1 --- F. Docker, CI/CD and Kafka

-   Explain your CI/CD pipeline.

-   What are the stages in your deployment pipeline?

-   Which deployment strategy do you follow in your project?

-   Explain blue-green deployment.

-   Why are offsets important?

-   How do you avoid duplicate message processing?

-   What happens if a consumer crashes before committing the offset?

## Round 2 --- Managerial Discussion (all project & production, no theory)

-   Walk me through your current project.

-   Explain your roles and responsibilities.

-   Which modules are you responsible for?

-   What challenges have you faced in production?

-   Tell me about a production issue you resolved.

-   How do you perform RCA?

-   How do you communicate with clients during a critical incident?

-   Have you handled production deployments?

-   How do you prioritise when multiple production issues hit at once?

-   Tell me about a difficult client interaction.

-   Teamwork, ownership, stakeholder communication.


[⬆ Back to Index](#index)

<a id="unnamed-company-java-developer-dubai-l2-round"></a>
# Unnamed Company --- Java Developer, Dubai (L2 Round)

*5+ years experience --- following up on an earlier L1 round*

## L2 Round Topics

-   Self introduction.

-   Deep dive into your project.

-   Spring Boot code snippet --- find the mistakes.

-   SQL coding questions.

-   What is Docker?

-   ConcurrentHashMap.

-   Payments & transaction-related scenarios.

-   Race conditions.

-   Caching concepts.

-   Spring Boot annotations.

-   \@Lazy loading.

-   \@Transactional annotation.

-   Database indexes & how indexing works internally.

-   Stored procedures.

-   Views in databases.

-   POS (Point of Sale) system concepts.

-   SAF (Store and Forward) --- real-time scenario-based discussion.

-   3D Secure flow.

-   End-to-end payment flow.


[⬆ Back to Index](#index)

<a id="unnamed-company-todays-first-round-interview-experience"></a>
# Unnamed Company --- \"Today\'s First-Round Interview Experience\"

*Java Backend Developer --- PostgreSQL & Docker-focused, 30 questions*

## Questions Asked

-   Tell me about yourself.

-   What is your current role and responsibility?

-   What is your role in your current project?

-   Explain your current project and its architecture.

-   What technologies are you currently working with?

-   How did you use Spring Boot in your project?

-   How did you integrate PostgreSQL with Spring Boot?

-   How does Spring Data JPA work with PostgreSQL?

-   Explain the flow from REST API → Controller → Service → Repository →
    Database.

-   How do you create and integrate REST APIs?

-   How do you test REST APIs?

-   How do you check whether data is correctly inserted/updated in
    PostgreSQL?

-   What is a transaction?

-   What is rollback?

-   How do you handle transactions in Spring Boot?

-   What is \@Transactional?

-   Give a real-time example where rollback is required.

-   How do you use Docker in your project?

-   What is a Dockerfile?

-   What is Docker Compose?

-   Why do we use Docker Compose?

-   How do you run Spring Boot and PostgreSQL together using Docker
    Compose?

-   How do you deploy your application?

-   How do you check whether your deployed application is working
    correctly?

-   What happens when an API request comes to your Spring Boot
    application?

-   How do you handle exceptions in Spring Boot?

-   How do you manage your code using Git/GitHub?

-   Why are you looking for a change?

-   Why should we hire you?


[⬆ Back to Index](#index)

<a id="unnamed-company-multi-round-scenario-system-design-process"></a>
# Unnamed Company --- Multi-Round Scenario & System-Design Process

*5-round process: Scenario-Based Technical → Coding → Data Structure →
Code Design → System Design → Managerial*

## Round 1 --- Scenario Based Technical Question

-   What are the potential issues with consistent hashing for music
    streaming servers?

-   What are the pros and cons of using pre-loaded hints versus
    server-loaded hints in an application?

-   How would you process a file larger than the available RAM on a
    single system?

-   You are tasked with building a sports news classification service
    that downloads articles and applies machine learning to detect bias.
    What information would you require to estimate the resources needed
    for this system?

-   When expanding a production-ready application to multiple countries,
    what backend changes and considerations must be taken into account?

## Round 1 --- Coding

-   Given a list of words Words = \[\"baby\",\"cat\",\"dada\",\"dog\"\]
    and a random jumbled string like \'ctay\', write a function
    find(words, word1) that returns the word if it can be formed from
    the characters of the given string. Example: find(words,\"ctay\") →
    \"cat\"; find(words,\"dad\") → \"-\" (not found).

-   Given a 2D matrix of characters, determine if a given word exists in
    the matrix by moving only right or down.

## Round 2 --- Data Structure Round

-   Each file has a collectionId attached. How would you generate a
    report to show the total size of all files, and the top N
    collections ranked by total file size?

-   How would you modify the system if multiple collections can be
    associated with a single file?

-   How would you design and optimize this solution for a multithreaded
    environment to ensure correctness and efficiency?

## Round 3 --- Code Design Round

-   How would you design a Rate Limiter?

-   How would you scale this system to include a credit-based model,
    where unused requests are carried over as credits?

-   How would you implement and manage this system in a multithreaded
    environment?

## Round 4 --- System Design Round

-   Design a Web Scraper system that: starts with an initial set of
    URLs, scrapes all nested URLs recursively, extracts and returns all
    image links mapped against their respective parent URLs, handles
    depth limits, optimizes scraping for speed and server load, and
    ensures fault tolerance and retries.

## Round 5 --- Managerial Round

-   Describe a situation where you successfully handled a project with
    vague requirements.

-   Explain how you helped a team member grow technically.


[⬆ Back to Index](#index)

<a id="unnamed-company-java-full-stack-developer-first-technical-round"></a>
# Unnamed Company --- \"Java Full Stack Developer\" First Technical Round

*4+ Years Experience --- interview covered Java, Spring Boot,
Microservices, Angular, Security and Banking scenarios (25 questions)*

## Java

-   How do you use Optional and what are its pitfalls?

-   How does CompletableFuture support asynchronous programming?

-   How would you improve service scalability with multiple
    API/repository calls?

-   Difference between Future and CompletableFuture?

-   Explain try-with-resources.

-   How does try-with-resources automatically close resources?

## Angular

-   How do you manage shared state using Services and RxJS?

-   When would you choose NgRx over a shared service?

-   Difference between pure and impure pipes?

-   How do you implement lazy loading?

-   How do you securely validate user input?

-   How do you prevent XSS in Angular?

-   Can untrusted data be bound to innerHTML?

-   What is Angular interpolation syntax?

## Spring Boot & Microservices

-   How do you handle partial failures in microservices?

-   How do you manage sensitive configuration in Spring Boot?

-   How does Spring load externalized properties at runtime?

-   How do you implement global exception handling?

-   How do you handle multiple exceptions separately?

-   How do you map custom exceptions to different handlers?

-   How can Tomcat support REST and SOAP together?

-   How do you secure REST and SOAP APIs using JWT?

## Banking & Coding

-   How do you implement idempotency to prevent duplicate fund
    transfers?

-   Write a POST /account/transfer API with validation and response
    handling.

-   How do you validate complex nested JSON requests?


[⬆ Back to Index](#index)

<a id="unnamed-company-senior-java-backend-developer-spring-boot-microservices-virtual-technical-round"></a>
# Unnamed Company --- \"Senior Java Backend Developer -- Spring Boot & Microservices\" (Virtual Technical Round)

## Java

-   What are the benefits of sealed classes introduced in Java 17?

-   How do virtual threads in Java 21 improve request handling in
    high-load systems?

-   What is the difference between Optional.map() and
    Optional.flatMap()?

-   How does garbage collection differ in Serial, Parallel, and G1
    collectors?

-   How does var help with type inference in Java?

-   Explain the difference between CopyOnWriteArrayList and ArrayList.

-   What is the purpose of CompletableFuture in asynchronous
    programming?

-   How does pattern matching for switch improve readability in Java
    17+?

-   What are text blocks, and how do they simplify working with
    JSON/XML?

-   Explain immutability and how to enforce it in Java.

## Spring + Spring Boot

-   How does dependency injection work in Spring Framework?

-   What is the role of \@Configuration and \@Bean annotations?

-   How do you configure application properties for multiple
    environments in Spring Boot?

-   How do you secure Spring Boot REST APIs with JWT authentication?

-   Explain how Spring Boot manages embedded servers like Tomcat/Jetty.

-   How does Spring Boot Actuator help with monitoring and metrics?

-   How do you manage transaction propagation in Spring?

-   What is the difference between \@ControllerAdvice and
    \@ExceptionHandler?

-   How do you enable asynchronous method execution in Spring Boot?

-   How does Spring Boot integrate with cloud services (e.g., AWS S3,
    RDS)?

## Microservices

-   How do you implement inter-service communication in microservices?

-   What is the difference between API Gateway and Service Mesh?

-   How do you handle eventual consistency in distributed systems?

-   How do you implement rate limiting in a microservices ecosystem?

## Coding Questions

-   Write a program to check if a number is a palindrome without
    converting it to a string.

-   Implement a function to sort a stack using another stack.

-   Write a program to find the maximum sum subarray (Kadane\\'s
    algorithm).

## Others

-   What is the difference between Kafka topics and partitions?

-   How do StatefulSets in Kubernetes differ from Deployments?

-   Write a SQL query to get the 2nd highest salary in a department.

-   What are readiness probes in Kubernetes, and how do they differ from
    liveness probes?

-   How do you implement request tracing across multiple microservices?

-   What is the difference between Docker volumes and bind mounts?

-   How do you implement rolling updates in Kubernetes?


[⬆ Back to Index](#index)

<a id="unnamed-company-java-full-stack-angular-developer-2nd-round-technical"></a>
# Unnamed Company --- Java Full Stack (Angular) Developer, 2nd Round Technical

## Java & Spring Boot

-   Tell me about your experience and current project.

-   How does Angular integrate with a Java backend API?

-   Java 17 features and where you used them.

-   Spring vs Spring Boot --- why use Spring Boot?

-   The \"final\" keyword.

-   Different ways to create objects in Java.

-   \"new String()\" vs \"==\" vs \".equals()\".

-   Hibernate and the N+1 problem.

-   \@Autowired vs constructor injection.

## AWS & Docker

-   How do you deploy a Java application to AWS Elastic Beanstalk?

-   How do you create a Docker image? Explain the key Docker commands.

-   What AWS and development tools/versions are you using?

## Git & Development

-   Explain your Git workflow.

-   What happens if you get merge conflicts after pulling the latest
    code?

-   How do you resolve Git conflicts?

-   How do you perform code reviews and debugging?

## SQL & Database

-   LEFT JOIN vs RIGHT JOIN.

-   Given Source and Destination tables, write a query using \"code\"
    and \"source\" to get the required destination details.

-   What is pagination? What is an index?

-   How do you implement pagination using Hibernate/Spring Data JPA?

## Angular

-   Which Angular version are you using?

-   What is MVVM?

-   What is scope in Angular?

-   What are Pipes?

-   What is a Component?

-   Explain the Angular lifecycle and lifecycle hooks.

## Production & Performance

-   What is production support?

-   What production challenges have you faced?

-   How do you optimize an API?

-   If an API is fast locally but slow in production, how would you
    debug and fix it?

## Questions the Candidate Asked

-   If I am selected, what would my responsibilities be during the
    first 30 days?

-   How is the team currently utilizing AI in development and day-to-day
    work?


[⬆ Back to Index](#index)

<a id="part-2-general-topic-wise-question-banks-not-tied-to-one-company"></a>
# Part 2 --- General / Topic-Wise Question Banks (Not Tied to One Company)


[⬆ Back to Index](#index)

<a id="top-java-and-spring-boot-interview-questions-general-topic-list-not-tied-to-a-company"></a>
# Top Java and Spring Boot Interview Questions (general topic list, not tied to a company)

## Java Core

-   How is Java platform independent (JVM vs JRE)?

-   What is a ClassLoader?

-   Difference: ClassNotFoundException vs NoClassDefFoundError.

-   String Pool, intern(), == vs equals().

## OOP Concepts

-   What is Encapsulation?

-   The Serializable interface.

-   Can a class be static, final, or private?

-   OOP vs scripting languages.

## Exceptions

-   Superclass of all exceptions.

-   Error vs Exception.

-   Checked vs Unchecked.

-   Try-with-resources.

-   Exception propagation.

## Java Language Features

-   AutoBoxing.

-   Key Java 8 features (Streams, Lambdas, Functional interfaces).

-   Lambda functions and why they are useful.

-   Why interfaces cannot have constructors.

-   Performance: for-loop vs for-each.

## Concurrency

-   Code using 5 threads printing in sequence.

-   Volatile vs Atomic.

## Design Patterns

-   Builder pattern (usage + example).

-   Singleton implementation.

-   Factory pattern concept.

-   Why design patterns are needed.

-   Patterns you know and how to invent one.

## Spring and Spring Boot

-   What is Spring Boot and why it\'s popular.

-   Spring annotations: \@Component, \@Bean, \@Qualifier, \@Value.

-   \@Controller vs \@RestController.

-   \@Mock vs \@InjectMocks.

-   OAuth basics and role-based access.

-   Spring Data JPA main interfaces.

-   Exception handling in controllers.

-   Transaction propagation.

-   application.properties purpose.

-   How Spring/Hibernate generates SQL.

-   MVC: Controller vs Service vs Repository.

-   Pagination and unique constraints.

## Collections

-   How HashMap works internally.

-   HashMap vs Hashtable vs ConcurrentHashMap.

-   List vs Set vs Map (when to use which).

-   What is LinkedList.

-   Comparable interface.

-   How hashCode is generated.

-   Iterable interface.


[⬆ Back to Index](#index)

<a id="production-scenario-question-banks-multiple-compiled-lists-not-tied-to-a-company"></a>
# Production-Scenario Question Banks (multiple compiled lists, not tied to a company)

*Recurring theme across 2026 postings: interviewers are moving from
definitions to \'what would you do when it breaks\'*

## Set A --- 15 real production scenarios (Spring Boot/Java)

-   Your API suddenly becomes 5× slower. How do you debug it?

-   CPU usage is normal, but response time is high. What could be the
    reason?

-   Database connection pool is exhausted. What would you check first?

-   Two users update the same record at the same time. How do you handle
    it?

-   The same payment request arrives twice. How do you prevent duplicate
    payments?

-   A downstream service takes 10 seconds to respond. What would you do?

-   Retries are creating a traffic spike. How would you prevent a retry
    storm?

-   Kafka consumer lag keeps increasing. How would you investigate?

-   A Kafka message fails after consumption. How do you handle it?

-   Your application suddenly throws OutOfMemoryError. What would you
    check?

-   The database query is fast, but the API is still slow. Where would
    you look?

-   One microservice goes down during a critical transaction. What
    should happen?

-   A scheduled job executes twice. How would you make it safe?

-   You need to change an API version without breaking existing clients.
    How?

-   Traffic suddenly increases by 10×. How would you prepare the system?

## Set B --- 20 backend scenarios (\"What is \@Transactional?\" → \"Production is down. What do you do?\")

-   Your Spring Boot API suddenly becomes 5× slower in production. How
    would you find the root cause?

-   Your application is running out of database connections. How would
    you troubleshoot it?

-   Two requests update the same account balance at exactly the same
    time. How would you prevent incorrect results?

-   A customer clicks \'Pay\' twice because the first request timed out.
    How would you prevent duplicate payment?

-   Your API works perfectly locally but returns 500 errors in
    production. What would you investigate?

-   One external service takes 15 seconds to respond. How would you
    prevent your application threads from getting blocked?

-   Your retry mechanism starts sending thousands of requests to an
    already-failing service. How would you handle it?

-   Your Spring Boot application slowly consumes more memory and
    eventually crashes. How would you investigate the memory leak?

-   A scheduled job runs once on your laptop but runs 5 times in
    production. Why could this happen?

-   Your Kafka consumer is processing messages slower than the producer.
    How would you reduce the growing lag?

-   Your database query takes 50ms, but the API response takes 2
    seconds. Where would you look?

-   A microservice becomes unavailable while processing an order. How
    should your system handle the failure?

-   Your application suddenly creates thousands of threads. What could
    cause this and how would you investigate it?

-   You need to deploy a new API version without breaking existing
    clients. How would you design it?

-   Your application has high GC activity and users are experiencing
    slow responses. What would you check?

-   Multiple instances of your Spring Boot application are processing
    the same event. How would you prevent duplicate processing?

-   Your API receives 10× normal traffic unexpectedly. What would you do
    to keep the system available?

-   Production logs show errors, but you cannot trace a request across
    microservices. How would you improve debugging?

-   A transaction updates three tables and fails after the second
    update. How should the database state be handled?

-   Your API works fine with 1,000 users but starts failing at 100,000
    users. How would you identify the scalability bottleneck?

## Set C --- 20 more Spring Boot production scenarios (candidate rejected for not being \'basic enough\')

-   Your Spring Boot API suddenly becomes slow. How would you debug it?

-   CPU and memory are normal, but API latency is high. What would you
    check?

-   Your database connection pool is exhausted. What could be the
    reason?

-   A downstream service is taking 10 seconds to respond. How would you
    handle it?

-   Your API is receiving duplicate requests. How would you make the
    operation idempotent?

-   A \@Transactional method fails halfway through. What happens to the
    transaction?

-   Your application has frequent database deadlocks. How would you
    investigate them?

-   One API returns 500 errors only in production. Where would you start
    debugging?

-   Your Spring Boot application suddenly starts consuming too much
    memory. What would you check?

-   How would you handle exceptions consistently across all REST APIs?

-   Your API needs to handle 10,000 concurrent requests. What would you
    optimize first?

-   A scheduled Spring Boot job runs twice after deploying multiple
    instances. How would you prevent it?

-   Your API works in local but fails in production. What configuration
    differences would you check?

-   A third-party API is unreliable. How would you prevent it from
    affecting your service?

-   Your application starts timing out under heavy traffic. What could
    be causing it?

-   How would you prevent a slow database query from affecting the
    entire application?

-   Your cache contains stale data. How would you handle cache
    consistency?

-   A Spring Boot service keeps retrying a failed request. How could
    this make the problem worse?

-   How would you trace a request across multiple Spring Boot
    microservices?

-   A production issue occurs but the logs don\'t show enough
    information. What would you do next?

## Set D --- \"20 production scenarios\" companion list (2026 first-round shift)

-   HashMap has 1 million entries. Performance is degrading. Why, and
    what do you do?

-   Your code uses synchronized everywhere. The application is slow.
    What is wrong?

-   Double-checked locking in a singleton. Why do you need volatile?

-   A ThreadLocal variable is causing a memory leak. How?

-   wait vs sleep. Where would each one cause a deadlock?

-   Ran fine for 30 days. Crashed with OutOfMemoryError. No code
    changes. What happened?

-   GC runs every 30 seconds and pauses the app for 2 seconds each time.
    Fix it.

-   When would you choose G1GC over ZGC in production?

-   \@Transactional on a private method. Why does it silently do
    nothing?

-   Two beans of the same type. How does Spring decide which to inject?

-   Your \@Async method throws. Nobody catches it. What happens?

-   Startup takes 45 seconds. Diagnose and fix it.

-   LazyInitializationException in production but not in dev. Why?

-   A JPA query is fine at 100 rows. 45 seconds at 1 million. What is
    wrong?

-   Two transactions update the same row at once. One silently
    overwrites the other. Prevent it.

-   Your payment API gets the same request twice on a network retry.
    Stop the double charge.

-   Service A calls B. B is down. A keeps retrying. B never recovers.
    What pattern fixes this?

-   Redis cache is working. Database load is still at 100 percent. What
    is wrong?

-   Response time is 20ms normally, 8 seconds under load. No errors in
    logs. Where do you start?

-   Feature deployed Friday evening. Monday morning everything is slow.
    Nobody touched the code. What happened?

## Set E --- \"90% of candidates can\'t answer\" (senior-level, with answers given)

-   What actually breaks when two threads write to the same HashMap?

-   *Hint / Answer given: \'It is not thread safe\' is not enough ---
    they want the failure: lost entries, a corrupted table, a get()
    returning null for a key you just inserted.*

-   Your counter is volatile and still gives the wrong total. Why?

-   *Hint / Answer given: Volatile provides visibility, not atomicity.
    count++ is read-modify-write --- three operations, no
    synchronization. Fix with AtomicInteger or LongAdder under high
    contention.*

-   You put \@Transactional on a method and call it from another method
    in the same class. What happens?

-   *Hint / Answer given: Nothing --- no transaction is created.
    Self-invocation bypasses the Spring proxy entirely; many candidates
    have shipped this bug without realizing it.*

-   Your service runs 200 threads on a pool of 10 connections. Traffic
    triples. CPU sits at 20%. What is going on?

-   *Hint / Answer given: Threads are waiting for HikariCP connections,
    not executing work. The bottleneck is the connection pool, not CPU
    --- adding more servers can make it worse.*

-   Why is double-checked locking broken without volatile?

-   *Hint / Answer given: Another thread can observe a non-null
    reference to a partially constructed object due to instruction
    reordering --- rare, real, and extremely difficult to debug.*

-   Your query has an index, but the database still performs a full
    scan. Why?

-   *Hint / Answer given: A function applied to the indexed column, an
    incorrect leading column in a composite index, or implicit type
    conversion --- all three can prevent effective index usage.*

-   Where does a memory leak hide in a language with garbage collection?

-   *Hint / Answer given: Static collections that grow indefinitely,
    ThreadLocals never removed inside thread pools, listeners never
    unregistered --- GC cannot reclaim objects that are still
    referenced.*

-   What happens to your in-flight requests when Kubernetes sends
    SIGTERM?

-   *Hint / Answer given: Without graceful shutdown, they can terminate
    mid-write. One part is the shutdown hook; the other is a preStop
    delay so traffic stops reaching the instance first.*

-   Your Kafka consumer commits the offset before processing the
    message. What did you just choose?

-   *Hint / Answer given: At-most-once delivery --- a crash after the
    commit means the message is lost. Commit after processing to get
    at-least-once delivery, which then requires idempotency.*

## Set F --- Scenario-based Java questions (no memorized definitions)

-   Your Singleton is getting broken. What are all the possible ways it
    can happen, and how would you prevent it?

-   Multiple threads are updating the same object. How would you make it
    thread-safe without hurting performance?

-   Your application suddenly starts throwing OutOfMemoryError. How
    would you debug it?

-   Your API is making multiple independent service calls. When would
    you use CompletableFuture vs Virtual Threads?

-   A HashMap suddenly performs very slowly. What could be the reasons?

-   You need to process 10 million records. Would you use Streams,
    Parallel Streams, or traditional loops? Why?

-   You need to cache frequently accessed data. Which cache would you
    choose and what eviction strategy would you use?

-   Your application startup time is very high. Where would you start
    investigating?

-   How would you design JWT authentication with Refresh Tokens
    securely?

-   Your database is slowing down because of too many queries. How would
    you identify and fix the N+1 query problem?

-   You need to process messages reliably from Kafka without duplicates.
    How would you design it?

-   One downstream service is failing intermittently. How would you
    prevent cascading failures?

## Set G --- Production readiness mindset (not Q&A, framing questions to ask yourself)

-   What happens when a dependency becomes slow? (Think: timeouts,
    connection pools, circuit breakers, fallbacks.)

-   What happens when the same request comes twice? (Network retries,
    client retries, queue redelivery --- is the operation idempotent?)

-   What happens when the response contains 1 million records?
    (Pagination, sorting, filtering, proper indexing.)

-   How will you know when something goes wrong? (Structured logs,
    metrics, health checks, distributed tracing.)

-   What happens when traffic becomes 10×? (Is the service stateless?
    Can you add more instances? Is the database the bottleneck? Can
    frequently-read data be cached? Do you need asynchronous
    processing?)


[⬆ Back to Index](#index)

<a id="core-java-concurrency-jvm-deep-dive-question-banks"></a>
# Core Java, Concurrency & JVM --- Deep-Dive Question Banks

*General guides, not tied to a specific interview*

## Java Concurrency & Multithreading --- Top 40

-   What is the difference between a Process and a Thread?

-   Platform Thread vs Virtual Thread?

-   How does the Java Memory Model work?

-   What is the happens-before relationship?

-   synchronized vs ReentrantLock?

-   volatile vs Atomic variables?

-   What is CAS and how does it work?

-   How does ConcurrentHashMap achieve thread safety?

-   What is a Race Condition?

-   How do you prevent Race Conditions?

-   What is Deadlock?

-   How do you detect and prevent Deadlocks?

-   Deadlock vs Livelock vs Starvation?

-   What is Thread Starvation?

-   ExecutorService vs ForkJoinPool?

-   How does ThreadPoolExecutor work?

-   How do you choose Core Pool Size and Maximum Pool Size?

-   What happens when a ThreadPoolExecutor queue becomes full?

-   CompletableFuture vs Future?

-   How does CompletableFuture work internally?

-   thenApply vs thenCompose vs thenCombine?

-   What is CountDownLatch?

-   CountDownLatch vs CyclicBarrier?

-   What is Semaphore and where would you use it?

-   What is Phaser?

-   What is ThreadLocal?

-   What problems can ThreadLocal cause?

-   What is False Sharing?

-   What is ForkJoinPool?

-   How do parallel streams use threads?

-   Why can parallel streams hurt performance?

-   How do Virtual Threads work internally?

-   When should you NOT use Virtual Threads?

-   Virtual Threads vs CompletableFuture?

-   How do you handle blocking operations with Virtual Threads?

-   How would you debug a production thread issue?

-   How would you investigate high CPU caused by Java threads?

-   How would you identify thread pool exhaustion?

-   How would you design a thread-safe cache?

-   Describe a real-world concurrency problem you solved.

## JVM Internals

-   What is the difference between Heap and Stack memory?

-   How does the JVM memory model work?

-   Explain the Class Loading lifecycle.

-   What are Bootstrap, Platform, and Application ClassLoaders?

-   How does Garbage Collection work internally?

-   G1 GC vs ZGC vs Serial GC?

-   What triggers a Full GC?

-   How do you identify memory leaks in production?

-   What is a Heap Dump and when would you analyze it?

-   How does the JIT Compiler improve performance?

-   What is Escape Analysis?

-   What is Metaspace and how is it different from PermGen?

-   How do you troubleshoot OutOfMemoryError?

-   How do you troubleshoot high CPU usage in JVM applications?

-   What tools do you use for JVM performance analysis?

## Java Design Patterns

-   What problem does the Factory Pattern solve?

-   Factory Pattern vs Abstract Factory Pattern?

-   When should you use the Builder Pattern?

-   Why is Builder preferred for immutable objects?

-   How do you implement a thread-safe Singleton?

-   What are the drawbacks of Singleton?

-   Explain the Strategy Pattern with a real-world example.

-   When would you use the Observer Pattern?

-   How is Observer used in event-driven systems?

-   Adapter Pattern vs Decorator Pattern?

-   What problem does the Proxy Pattern solve?

-   How is the Template Method Pattern different from Strategy?

-   What is Dependency Injection and which pattern does it use?

-   Which design patterns are commonly used in the Spring Framework?

-   Which design patterns are most commonly used in Microservices?

## Spring \@Transactional Deep-Dive (30 questions)

-   How does \@Transactional work internally?

-   Why does \@Transactional sometimes not work?

-   What exceptions trigger transaction rollback?

-   What is the difference between REQUIRED and REQUIRES_NEW?

-   Explain all transaction propagation types.

-   What is transaction isolation?

-   Explain all isolation levels.

-   What causes dirty reads?

-   What causes phantom reads?

-   Why should API calls be avoided inside transactions?

-   What happens during nested transactions?

-   What is transaction synchronization?

-   How do you debug transaction issues?

-   What is the self-invocation problem?

-   How does Spring use proxies for transaction management?

## Java Backend Interview Questions --- SDE-2 (₹25--30 LPA level)

-   How does ConcurrentHashMap achieve thread safety?

-   wait() vs sleep() vs join()?

-   JVM Class Loading Mechanism.

-   equals() vs == and why hashCode() matters.

-   How Garbage Collection works.

-   volatile vs synchronized.

-   Deep Copy vs Shallow Copy.

-   Checked vs Unchecked Exceptions.

-   ThreadLocal with real-world use cases.

-   Scenario: You\'re building a payment gateway where thousands of
    transactions run in parallel. How would you ensure thread-safe
    balance updates?

-   \@Bean vs \@Component.

-   Spring AOP with real production examples.

-   Circular Dependencies.

-   JWT vs OAuth2.

-   Spring Boot Auto Configuration.

-   application.properties vs application.yml.

-   ApplicationContext vs BeanFactory.

-   Scenario: Design an order processing workflow where inventory,
    payment, and invoice generation execute independently without tight
    coupling.

-   get() vs load() (Hibernate).

-   First-Level vs Second-Level Cache.

-   Optimistic vs Pessimistic Locking.

-   Dirty Checking.

-   Entity Lifecycle States.

-   Scenario: Multiple users update the same product stock
    simultaneously. How would you prevent inconsistent data?

-   Design an E-commerce Order Tracking System.

-   Design a Rate Limiter.

-   Design a URL Shortener.

-   Design a Distributed Key-Value Store (Redis).

-   Design a User Authentication System.

## Advanced Java Questions (\"only revise these before an interview\")

-   How does HashMap work internally in Java 8+?

-   What happens when two keys have the same hash code?

-   How does ConcurrentHashMap achieve thread safety?

-   What is the difference between volatile, synchronized, and
    AtomicInteger?

-   How does the Java Memory Model work?

-   What happens when an object becomes eligible for Garbage Collection?

-   What causes OutOfMemoryError vs StackOverflowError?

-   How does the JVM ClassLoader work?

-   What is the difference between String, StringBuilder, and
    StringBuffer?

-   How would you identify and fix a memory leak in Java?

-   What problems can ThreadLocal cause in a thread pool?

-   How does CompletableFuture work internally?

-   What is ForkJoinPool and how does work-stealing work?

-   How would you detect and troubleshoot a deadlock?

-   What happens when you create too many threads in a Java application?

-   What are Weak, Soft, and Phantom References?

-   What is false sharing and how can it affect performance?

-   How would you investigate high GC pauses in production?

-   What happens internally when a Java thread calls wait()?

-   How would you troubleshoot high thread contention in a Java
    application?

## Java 8 Stream Questions --- \"Last Minute Series\"

-   Given a list of integers, find all the even numbers using Stream
    functions.

-   Given a list of integers, find all the numbers starting with 1 using
    Stream functions.

-   How to find duplicate elements in a given integers list using Stream
    functions.

-   Given the list of integers, find the first element of the list using
    Stream functions.

-   Given a list of integers, find the total number of elements using
    Stream functions.

-   Given a list of integers, find the maximum value element using
    Stream functions.

-   Given a String, find the first non-repeated character in it using
    Stream functions.

-   Given a String, find the first repeated character in it using Stream
    functions.

-   Given a list of integers, sort all the values in it using Stream
    functions.

-   Given a list of integers, sort all the values in descending order
    using Stream functions.

-   Given an integer array nums, return true if any value appears at
    least twice, and false if every element is distinct.

-   How will you get the current date and time using the Java 8 Date and
    Time API?

-   Write a Java 8 program to concatenate two Streams.

-   Java 8 program to cube list elements and filter numbers greater than
    50.

-   Write a Java 8 program to sort an array and then convert the sorted
    array into a Stream.

-   How to use map to convert an object into uppercase in Java 8?

-   How to convert a List of objects into a Map by considering duplicate
    keys and store them in sorted order?

-   How to count each element/word from a String ArrayList in Java 8?

-   How to find only duplicate elements with their count from a String
    ArrayList in Java 8?

-   How to check if a list is empty in Java 8 using Optional, and if
    not, iterate through the list and print the objects?

-   Write a program to find the maximum element in an array.

-   Write a program to print the count of each character in a String.


[⬆ Back to Index](#index)

<a id="microservices-system-design-api-performance-question-banks"></a>
# Microservices, System Design & API Performance --- Question Banks

*General guides, not tied to a specific interview*

## 20 Microservices Questions to Revise

-   Why would you choose Microservices over a Monolith?

-   How do you decide whether two services should be separate?

-   REST vs Kafka --- when would you choose one over the other?

-   What happens when one service becomes unavailable?

-   How does an API Gateway help in a Microservices architecture?

-   What problem does Service Discovery solve?

-   How does a Circuit Breaker prevent cascading failures?

-   Retry vs Timeout --- when should you use each?

-   How would you make a Microservice API idempotent?

-   How would you handle distributed transactions?

-   What is the Saga Pattern, and when would you use it?

-   How do you maintain data consistency between services?

-   What happens if Kafka delivers the same event twice?

-   How would you handle Kafka consumer lag?

-   How would you trace a request across multiple Microservices?

-   What metrics would you monitor for a production Microservice?

-   How would you secure communication between Microservices?

-   Why might adding more instances not improve performance?

-   How would you handle backward compatibility when changing an API?

-   One Microservice suddenly becomes slow in production. What\'s your
    step-by-step debugging approach?

## Java Microservices --- Latest Interview Questions (2026 Trend, 3--5 years experience)

-   Difference between Fail-Fast and Fail-Safe?

-   How does ConcurrentHashMap work internally?

-   What is a Daemon Thread?

-   Comparable vs Comparator?

-   SOLID principles with real-time example?

-   Optional: isPresent() vs ifPresent()?

-   How does JVM handle memory management?

-   How do you implement JWT authentication?

-   OAuth2 vs JWT?

-   RestTemplate vs WebClient?

-   How do microservices communicate?

-   What is API Gateway and why is it required?

-   How to implement Global Exception Handling?

-   How does \@Transactional work internally?

-   Why Kafka instead of RabbitMQ?

-   How do partitions work?

-   What happens if a consumer crashes?

-   How do you maintain message ordering?

-   What is idempotency in distributed systems?

-   First-level vs Second-level cache?

-   What is the N+1 problem?

-   How to optimize slow queries?

-   How do you handle concurrent updates?

-   Design a Payment / NEFT Processing System.

-   How to handle 1M+ transactions daily?

-   Saga Pattern vs 2PC?

-   How to ensure data consistency across services?

-   How to implement distributed locking?

-   How do you deploy microservices using Docker?

-   What is Circuit Breaker?

-   How do you monitor logs?

-   What is Rate Limiting?

## Senior Java + Spring Boot + Microservices (8+ Years Experience) --- Focus Areas

-   Java: HashMap internals & ConcurrentHashMap; CompletableFuture &
    multithreading; JVM, Heap, Stack & GC; Streams & parallel streams.

-   Spring Boot: auto-configuration; bean lifecycle; \@Transactional
    internals; Spring Security; performance optimization.

-   Microservices: Saga & distributed transactions; Circuit Breaker,
    Retry & Timeout; idempotency; API versioning; resilience & fault
    tolerance.

-   Kafka: consumer failures & offset management; message ordering;
    duplicate messages; partition & consumer strategy.

-   System Design: designing scalable microservices; 10K+ TPS handling;
    Redis caching; distributed tracing; production troubleshooting.

-   Note: at this level, expect follow-ups like \'Why did you choose
    this approach?\', \'What happens internally?\', \'What happens under
    high load?\', \'What can fail in production?\', \'How did you solve
    it in your project?\'

## Java API Performance Optimization --- Top 40 Senior Interview Questions

-   How would you identify the root cause of a slow Java REST API?

-   How do you measure API latency and throughput?

-   What is the difference between average, p95, p99, and p999 latency?

-   How would you optimize a REST API that suddenly becomes slow?

-   How do you identify whether the bottleneck is CPU, memory, database,
    network, or external services?

-   How can excessive object creation affect API performance?

-   How can JSON serialization and deserialization impact latency?

-   How would you optimize large JSON responses?

-   How would you implement pagination for large datasets?

-   Offset pagination vs cursor-based pagination?

-   How would you optimize database access from a Java API?

-   How do you identify slow SQL queries?

-   What is the N+1 query problem?

-   How would you reduce unnecessary database calls?

-   How does database connection pool exhaustion affect an API?

-   How would you tune HikariCP for a high-traffic application?

-   When should you use caching to improve API performance?

-   Caffeine vs Redis for API caching?

-   How would you handle cache invalidation?

-   What is a Cache Stampede and how do you prevent it?

-   How do thread pools affect API performance?

-   How would you identify thread pool exhaustion?

-   CPU-bound vs I/O-bound API workloads?

-   How can blocking operations reduce API throughput?

-   When can Virtual Threads improve API performance?

-   When should you avoid Virtual Threads?

-   How can connection timeouts affect API performance?

-   How would you configure timeouts for downstream REST calls?

-   How can retries negatively impact API performance?

-   How would Circuit Breaker and Bulkhead patterns improve API
    reliability?

-   How would you optimize a Java API running in Kubernetes?

-   How do CPU and memory limits affect Java API performance?

-   How would you investigate CPU throttling?

-   How would you investigate increasing JVM memory usage?

-   How does Garbage Collection affect API latency?

-   How would you identify GC-related performance issues?

-   How would Micrometer help monitor API performance?

-   How would distributed tracing help identify a slow API dependency?

-   How would you optimize an API handling thousands of concurrent
    requests?

-   A Java API normally responds in 100ms but suddenly takes 5 seconds.
    How would you troubleshoot it end-to-end?

## \"How Do You Make a Java API Handle 10K+ Requests?\" --- Design Checklist

-   Connection Pooling --- use HikariCP and tune the pool size based on
    actual workload (more connections ≠ more performance).

-   Database Optimization --- proper indexing, efficient queries, query
    optimization, avoid N+1 problems.

-   Caching --- don\'t hit the database for data that doesn\'t change
    frequently; use Redis for frequently accessed data.

-   Asynchronous Processing --- move non-critical or time-consuming work
    to Kafka / message queues.

-   Thread Management --- understand your concurrency model; tune thread
    pools and consider Virtual Threads for high-concurrency workloads.

-   Load Balancing --- don\'t depend on a single application instance;
    distribute traffic across multiple Spring Boot instances.

-   Stateless APIs --- keep services stateless to make horizontal
    scaling easier.

-   Rate Limiting --- protect APIs from traffic spikes and abusive
    clients.

-   Monitoring & Observability --- monitor latency, throughput, error
    rates, CPU & memory, GC activity, database performance.

-   Horizontal Scaling --- add more instances behind the load balancer
    instead of continuously growing one machine.

-   Caveat: the right architecture depends on request complexity,
    database workload, payload size, read/write ratio, external API
    dependencies, latency requirements, and traffic patterns --- don\'t
    add Kafka, Redis, Kubernetes, or caching just because they sound
    scalable.


[⬆ Back to Index](#index)

<a id="part-3-reference-topic-lists-not-qa"></a>
# Part 3 --- Reference / Topic Lists (Not Q&A)


[⬆ Back to Index](#index)

<a id="kafka-topics-reference-playlist"></a>
# Kafka Topics --- Reference Playlist

*Topic checklist (not Q&A) shared for backend engineers going deep on
Kafka*

## Topics

-   Event Driven Architecture

-   Why Kafka?

-   Kafka Architecture

-   Topics, Partitions & Offsets

-   Producers Internals

-   Consumers Internals

-   Consumer Groups & Rebalancing

-   Delivery Guarantees

-   Transactions

-   Exactly Once Semantics

-   Schema Registry

-   Avro

-   Kafka Connect

-   CDC with Debezium

-   Kafka Streams Architecture

-   Topology, Tasks & Threads

-   KStream

-   KTable

-   GlobalKTable

-   KStream vs KTable vs GlobalKTable

-   SerDes

-   Default vs Explicit SerDes

-   Custom SerDes

-   Serialization & Deserialization Exception Handling


[⬆ Back to Index](#index)

<a id="low-level-design-50-problem-master-list"></a>
# Low Level Design --- 50-Problem Master List

*Reference list of common LLD interview problems (not company-specific)*

## Core OOP & Real-World Systems

-   Design Parking Lot

-   Design Vending Machine

-   Design ATM

-   Design Library Management System

-   Design Elevator System

-   Design Traffic Light System

-   Design Meeting Room Scheduler

## Platform & Marketplace Apps

-   Design URL Shortener

-   Design BookMyShow

-   Design BookMyShow Seat Locking

-   Design Uber

-   Design Food Delivery Application

-   Design Online Hotel Booking System

-   Design Airline Management System

-   Design Restaurant Management System

-   Design Car Rental System

-   Design Amazon Order Management System

-   Design CricBuzz

-   Design Truecaller

-   Design Stock Exchange System

-   Design Learning Management System

## Games & Simulations

-   Design Snake and Ladder Game

-   Design Tic-Tac-Toe Game

-   Design Chess Game

## Social & Communication Apps

-   Design Splitwise

-   Design Chat Application (WhatsApp-like)

-   Design Community Discussion Platform (Reddit-like)

-   Design LinkedIn (LLD focus)

-   Design Calendar Application

-   Design Online Voting System

## Infrastructure & Core Systems

-   Design Cache System

-   Design Rate Limiter

-   Design Logging Framework

-   Design Notification System

-   Design Payment System (LLD)

-   Design File System

-   Design Task Scheduler

-   Design Search Autocomplete System

-   Design API Throttling System

-   Design Inventory Management System

## Design Patterns & Advanced LLD

-   Design Feature Flag System

-   Design Distributed ID Generator (Snowflake-like)

-   Design Circuit Breaker

-   Design Retry with Backoff Mechanism

-   Design Metrics and Monitoring System

-   Design Authentication System

-   Design Role-Based Access Control (RBAC)

-   Design Web Crawler (LLD focus)

-   Design Recommendation Engine (LLD focus)

-   Design Event-Driven Producer-Consumer System


[⬆ Back to Index](#index)

<a id="complete-java-topic-syllabus-as-per-developer"></a>
# Complete Java Topic Syllabus (\"As per Developer\")

*Reference topic map for a Java backend developer, shared as a study
checklist*

## Topics

-   1\. Java Basics --- syntax, structure, comments; keywords,
    identifiers; data types (primitive & non-primitive); variables &
    constants; type casting & promotion; operators; input/output;
    control flow; methods (overloading, varargs); exception handling.

-   2\. Object Oriented Programming --- class & object; constructors;
    this keyword; inheritance; method overriding vs overloading;
    polymorphism; abstraction; encapsulation; packages & access
    modifiers.

-   3\. Collections Framework --- collection hierarchy; List, Set, Map,
    Queue implementations; Iterator/ListIterator; Comparable vs
    Comparator.

-   4\. Multithreading & Concurrency --- process vs thread; thread
    lifecycle; creating threads; synchronization; inter-thread
    communication; deadlock; executor framework; concurrent collections.

-   5\. Java I/O & NIO --- File class; byte/character/buffered streams;
    object streams; Scanner class; NIO (channel, buffer, selector,
    paths, files).

-   6\. Java 8 Features --- lambda expressions; functional interfaces;
    method references; Stream API; Optional class; default & static
    methods in interfaces; Date & Time API.

-   7\. JDBC --- architecture; connecting to a database;
    Statement/PreparedStatement/CallableStatement; executing CRUD
    queries; ResultSet; transactions; batch processing; connection
    pooling.

-   8\. Java EE / Jakarta EE --- Servlets; JSP; filters & listeners; MVC
    architecture.

-   9\. Spring & Spring Boot --- Spring Core (IoC, DI); Spring Beans &
    configuration; Spring MVC; Spring Boot (starters,
    auto-configuration); RESTful web services; Spring Data JPA; Spring
    Security basics.

-   10\. Hibernate (ORM) --- architecture; configuration;
    session/session factory; CRUD operations; HQL; entity relationships;
    annotations vs XML mapping.

-   11\. Build Tools --- Maven (pom.xml, lifecycle, goals, plugins);
    Gradle; dependency management; build & release process.

-   12\. JVM Internals --- architecture; class loader subsystem; runtime
    data areas; execution engine; garbage collection; JVM types; JVM
    parameters; memory management & tuning.

-   13\. Testing --- JUnit (assertions, annotations); test cases & test
    suites; Mockito (mocking, stubbing, verifications); integration
    testing.

-   14\. Advanced Java Concepts --- generics; annotations (built-in &
    custom); reflection API; enums; records (Java 14+); modules (Java
    9+).

-   15\. Networking --- URL & URLConnection; TCP/IP sockets;
    ServerSocket & Socket.

-   16\. Tools & IDE --- IntelliJ IDEA / Eclipse; Git & GitHub; Postman
    (API testing).

-   17\. DSA (must for interviews) --- arrays & strings; linked lists;
    stack & queue; trees (binary tree, BST); graphs; searching &
    sorting; recursion & backtracking; dynamic programming; hashing.

[⬆ Back to Index](#index)
