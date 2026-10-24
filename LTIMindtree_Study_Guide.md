# LTIMindtree Online Test: Complete Study Guide
### DBMS · SQL · Java 8 · Spring Boot · Microservices · JWT · CORS · Aggregation · Cohesion · JUnit

---

## Contents

| Part | Topic |
|---|---|
| 1 | DBMS: keys, normalization, ACID, isolation, concurrency, recovery, indexing, ER, algebra |
| 1B | DBMS deep dive: normal-form method, joins with outputs, indexing, triggers, cursors, polling and pooling, partitioning, sharding, replication |
| 2 | SQL: execution order, NULL rules, aggregation, joins, subqueries, window functions, interview patterns |
| 3 | Java 8: lambdas, functional interfaces, streams, collectors, Optional, date-time, HashMap |
| 4 | Spring Boot: auto-configuration, IoC/DI, scopes, MVC/REST, JPA, transactions |
| 5 | Spring Boot configuration: properties, profiles, precedence, @Value, @ConfigurationProperties |
| 6 | Microservices and system design recap |
| 7 | JWT, Spring Security and CORS |
| 8 | OOP design: aggregation, composition, cohesion, coupling, SOLID |
| 9 | JUnit 5 and Mockito |
| 10 | Java stream coding problems (tested solutions) |
| 11 | **388 practice questions** with answers and explanations |

### Question count

| Section | Questions |
|---|---|
| A. DBMS concepts | 81 |
| B. Indexing and storage | 15 |
| C. SQL | 62 |
| D. Java 8 | 49 |
| E. Spring Boot and configuration | 48 |
| F. Microservices and system design | 72 |
| G. JWT, Spring Security and CORS | 25 |
| H. OOP design: aggregation, composition, cohesion, coupling | 18 |
| I. JUnit and Mockito | 18 |
| **Total** | **388** |

---

## 0. How to use this guide

1. Read **Part 1 to Part 10** (concepts). Every part ends with a **"Tricky points / traps"** list. LTIMindtree questions are usually built from these.
2. Solve the **Practice Questions** (Part 11 onwards). Each question has the answer, a short explanation and a pointer back to the concept section.
3. Re-solve only the ones you got wrong after 2 days.
4. Cover the answer lines with your hand or a sheet of paper while practising. Answers are printed directly under each question.

### What the test looks like (from public candidate reports)

| Item | Details |
|---|---|
| Platform | WeCP (We Create Problems), proctored |
| Size | about 110 to 112 questions in about 130 minutes, in about 7 sections |
| Marking | No negative marking reported. **Attempt every question.** |
| Heavy sections | The two Computer Science MCQ sections (programming, error fixing, output prediction, OS, CN, DBMS, SQL, web, OOP) |
| Topics in your screenshot | MCQs on Java 8, Spring Boot, Microservices, JWT, CORS, Spring Boot configuration, Aggregation, Cohesion, JUnit, SQL. Also about 2 coding questions (Java Streams, 1 hard). |

> Patterns change from drive to drive. Treat this as a strong guide, not a guarantee. The Java/Spring topics in your screenshot are for a Java-stack role, so Part 3 to Part 8 matter a lot.

### 60-minute-per-day plan (7 days)

| Day | Focus |
|---|---|
| 1 | DBMS (Part 1) and Q-set DBMS |
| 2 | SQL (Part 2) and Q-set SQL, tricky SQL |
| 3 | Java 8 (Part 3) and Q-set Java 8 |
| 4 | Spring Boot and configuration (Part 4, 5) |
| 5 | Microservices, JWT, CORS (Part 6, 7) |
| 6 | OOP design, Aggregation/Cohesion, JUnit (Part 8, 9) |
| 7 | Coding problems (Part 10), then a timed mixed revision |

### Exam-day tips
- Output-prediction questions: read the code **twice**. Look for lazy streams with no terminal operation, `NULL` in SQL, integer division, and `==` vs `equals`.
- If two options look right, pick the **more specific and more precise** one.
- Never leave a question blank (no negative marking).

---

# PART 1: DBMS (explained in detail)

## 1.1 What is a DBMS?
A **Database Management System** is software that stores, organises, retrieves and protects data, and lets many users and programs share it safely. It gives you: data independence, integrity constraints, concurrency control, crash recovery, security and a query language.

**File system vs DBMS:** A file system gives redundancy, inconsistency, no concurrency control, weak security and no crash recovery. A DBMS solves these.

**Three-schema architecture (ANSI/SPARC)**
- **External level:** what each user or application sees (views).
- **Conceptual level:** the whole logical design (tables, relationships, constraints).
- **Internal level:** how data is physically stored (files, indexes).
- **Physical data independence:** change the internal level without changing the conceptual level.
- **Logical data independence:** change the conceptual level without changing external views/applications. This is harder to achieve.

**Relational terms**

| Term | Meaning |
|---|---|
| Relation | A table |
| Tuple | A row |
| Attribute | A column |
| Domain | Allowed values of a column |
| **Degree** | Number of **columns** |
| **Cardinality** | Number of **rows** |
| Schema | Structure (design). Instance = the data at a moment |

## 1.2 Keys (very commonly asked)

| Key | Meaning |
|---|---|
| **Super key** | Any set of attributes that uniquely identifies a row (can have extra attributes) |
| **Candidate key** | A **minimal** super key (remove any attribute and uniqueness is lost) |
| **Primary key** | The candidate key chosen as the main identifier. Unique and NOT NULL. Only one per table |
| **Alternate key** | Candidate keys that were not chosen as primary |
| **Composite key** | A key made of 2 or more columns |
| **Foreign key** | A column referencing a primary/candidate key of another table (or the same table). Can be NULL and can repeat |
| **Surrogate key** | Artificial system-generated key (auto-increment, UUID) |
| **Natural key** | A real-world attribute used as key (email, PAN number) |

**Trap:** Every candidate key is a super key, but not every super key is a candidate key.
**Trap:** A table with `n` attributes where only one is the key has `2^(n-1)` super keys.

**Referential integrity:** every foreign key value must be NULL or match an existing referenced key. Actions on delete/update of the parent: `CASCADE`, `SET NULL`, `SET DEFAULT`, `RESTRICT/NO ACTION`.

**Entity integrity:** a primary key can never be NULL.

## 1.3 Functional dependencies and closure

`X → Y` means: if two rows have the same X, they must have the same Y.
- **Armstrong's axioms:** Reflexivity (if Y ⊆ X then X → Y), Augmentation (X → Y ⇒ XZ → YZ), Transitivity (X → Y and Y → Z ⇒ X → Z).
- Derived: Union, Decomposition, Pseudo-transitivity.

**Attribute closure X⁺** = all attributes that X determines. Algorithm: start with X, repeatedly add the right side of any FD whose left side is already in the set.

*Example:* R(A,B,C,D), FDs: A→B, B→C, C→D. A⁺ = {A,B,C,D}, so A is a key.

**Finding candidate keys quickly**
1. Attributes that never appear on the right side of any FD **must be in every key**.
2. Compute closure of that set. If it covers everything, it is the key.
3. If not, add attributes one by one.

*Example:* R(A,B,C,D), AB→C, C→D, D→A. B is never on the right. So every key contains B. AB⁺ = ABCD ✓. CB⁺: C→D, D→A, so ABCD ✓. DB⁺: D→A, AB→C ✓. Keys: **AB, BC, BD** (3 keys).

## 1.4 Normalization

**Why?** To remove redundancy and the three anomalies: **insertion** (cannot add a fact without another), **deletion** (deleting one fact loses another), **update** (must change many rows).

| Form | Rule | Removes |
|---|---|---|
| **1NF** | Atomic (single-valued) cells, no repeating groups | Multi-valued columns |
| **2NF** | 1NF + no **partial dependency** (non-key attribute depending on part of a composite key) | Partial dependency |
| **3NF** | 2NF + no **transitive dependency** (non-key → non-key). Formally: for X→A, either X is a super key or A is a prime attribute | Transitive dependency |
| **BCNF** | For every non-trivial X→Y, **X is a super key** | Remaining anomalies from overlapping candidate keys |
| **4NF** | BCNF + no non-trivial multivalued dependency | MVDs |
| **5NF** | No join dependency | Join dependencies |

**Worked example.** `Student(StudentID, CourseID, StudentName, CourseName)`, key = (StudentID, CourseID).
- StudentName depends only on StudentID, which is a **partial dependency**, so the table violates 2NF.
- Fix: `Student(StudentID, StudentName)`, `Course(CourseID, CourseName)`, `Enrollment(StudentID, CourseID)`.

**3NF example.** `Emp(EmpID, DeptID, DeptName)`: EmpID→DeptID→DeptName is a transitive dependency. Split into `Emp(EmpID, DeptID)` and `Dept(DeptID, DeptName)`.

**Prime attribute** = part of some candidate key.

**Tricky points**
- BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF. Every BCNF relation is in 3NF.
- A relation with a **single-attribute key** is automatically in 2NF if it is in 1NF.
- A relation where every attribute is prime is in 3NF.
- **3NF** can always be decomposed *lossless AND dependency-preserving*. **BCNF** is always lossless but may lose dependency preservation.
- **Lossless join** condition for R1, R2: (R1 ∩ R2) → R1 or (R1 ∩ R2) → R2.
- **Denormalization** adds controlled redundancy to speed up reads (used in data warehouses and read-heavy systems).

## 1.5 ER model

- **Entity** (rectangle), **Attribute** (oval), **Relationship** (diamond).
- **Weak entity:** no key of its own, depends on an owner (identifying) entity; double rectangle. Key = owner key + partial key.
- Attribute types: simple/composite, single/multi-valued, stored/derived.
- **Cardinality:** 1:1, 1:N, M:N. **Participation:** total (every instance participates) or partial.
- **Specialization** (top-down), **generalization** (bottom-up), **aggregation** (relationship treated as an entity).

**ER → relational mapping**
- Strong entity → table.
- 1:N → put the **PK of the "1" side as FK on the "N" side**.
- M:N → separate **junction table** with both PKs.
- Multi-valued attribute → separate table.
- 1:1 → FK on either side (prefer the side with total participation).

## 1.6 Transactions and ACID

A **transaction** is a logical unit of work (one or more operations) that must be executed completely or not at all.

| Property | Meaning | Who ensures it |
|---|---|---|
| **A**tomicity | All or nothing | Recovery manager (undo log / rollback) |
| **C**onsistency | Database moves from one valid state to another (constraints hold) | Application + constraints + the other three |
| **I**solation | Concurrent transactions behave as if run serially | Concurrency control (locks, MVCC) |
| **D**urability | Committed data survives crashes | Write-ahead log (WAL), redo |

**Transaction states:** Active → Partially committed → Committed, or Active/Partially committed → Failed → Aborted (rolled back).

**Concurrency problems**

| Problem | What happens |
|---|---|
| **Dirty read** | You read data another transaction wrote but has not committed (it may roll back) |
| **Non-repeatable read** | You read the same row twice and get different values (another transaction committed an UPDATE in between) |
| **Phantom read** | You run the same range query twice and get different **sets of rows** (another transaction INSERTed or DELETEd) |
| **Lost update** | Two transactions read the same value and both write, one overwrites the other |

**Isolation levels (SQL standard)**

| Level | Dirty read | Non-repeatable read | Phantom |
|---|---|---|---|
| Read Uncommitted | possible | possible | possible |
| Read Committed | prevented | possible | possible |
| Repeatable Read | prevented | prevented | possible (standard) |
| Serializable | prevented | prevented | prevented |

- PostgreSQL default: **Read Committed**. MySQL InnoDB default: **Repeatable Read**. Oracle default: Read Committed.
- Higher isolation means fewer anomalies but less concurrency.

## 1.7 Concurrency control

- **Schedule:** an interleaving of operations of several transactions. **Serial** schedule has no interleaving.
- **Conflict:** two operations on the same item by different transactions, at least one is a write (R-W, W-R, W-W).
- **Conflict serializable:** conflict-equivalent to some serial schedule. Test with a **precedence graph**: the schedule is conflict serializable **iff the graph is acyclic**.
- **Two-phase locking (2PL):** growing phase (only acquire locks) then shrinking phase (only release). Guarantees **conflict serializability** but **not** deadlock freedom. **Strict 2PL** holds exclusive locks until commit and also prevents cascading rollbacks.
- **Shared (S) lock** for reading, **Exclusive (X) lock** for writing. S-S compatible, everything else conflicts.
- **Deadlock:** cyclic wait. Detect with a **wait-for graph** (cycle = deadlock). Prevent with ordering/timeouts (wait-die, wound-wait). Resolve by aborting a victim.
- **Timestamp ordering**, **optimistic concurrency (validation)**.
- **MVCC (multi-version concurrency control):** keeps multiple versions of a row. Readers read a snapshot and do not block writers (PostgreSQL, Oracle, MySQL InnoDB).
- **Optimistic vs pessimistic locking:** pessimistic locks first (`SELECT ... FOR UPDATE`); optimistic checks a version column at commit (JPA `@Version`).

## 1.8 Recovery

- **Write-Ahead Logging (WAL):** log record must reach stable storage before the data page does.
- **Undo** (rollback uncommitted changes) and **redo** (reapply committed changes not yet on disk).
- **Checkpoint:** a marker where dirty pages are flushed so that recovery needs only to scan the log from the last checkpoint.
- **ARIES:** three phases in order: **Analysis → Redo → Undo**. Redo "repeats history", then undo removes losers.
- **Shadow paging:** keeps a shadow copy of page table. No log needed but fragmentation.

## 1.9 Indexing (detailed)

An **index** is an additional data structure that speeds up searching on one or more columns, at the price of extra storage and slower writes.

**Structures**
- **B-tree / B+ tree:** balanced, sorted, high fan-out (hundreds). In a **B+ tree** all data pointers are in leaf nodes and leaves are **linked**, so range scans are fast. Height is about log_fanout(N): fan-out 100 and 1,000,000 rows give about 3 levels.
- **Hash index:** O(1) equality lookup only. No range queries, no ordering.
- **Bitmap index:** one bitmap per distinct value. Excellent for **low-cardinality** columns (gender, status) and data warehouses.
- **LSM tree:** writes go to memory (memtable) then flushed as sorted immutable files (SSTables) and merged by compaction. Write-optimized (Cassandra, RocksDB).
- **Inverted index:** term → list of documents (full-text search, Elasticsearch).

**Types**

| Type | Meaning |
|---|---|
| **Clustered** | Rows are physically stored in index order. Leaf level = the data. **Only one per table** (often the PK) |
| **Non-clustered (secondary)** | Separate structure with pointers (row id or PK) to rows. Many per table |
| **Dense** | Entry for every record |
| **Sparse** | Entries for some records (e.g. one per block). Needs sorted file |
| **Composite** | On several columns. **Leftmost-prefix rule:** index (A,B,C) helps `A`, `A,B`, `A,B,C` but not `B` alone or `C` alone |
| **Covering** | Contains every column the query needs. No table access (index-only scan) |
| **Unique** | Enforces uniqueness |

**When the optimizer ignores an index**
- The query returns a large fraction of the table (full scan is cheaper).
- **Non-sargable** predicates: function on the column (`WHERE YEAR(d)=2024`), leading wildcard (`LIKE '%abc'`), implicit type conversion, arithmetic on the column (`WHERE salary+10 > 100`).
- Low selectivity columns.

Check plans with `EXPLAIN`. **Join algorithms:** nested loop (small/indexed), hash join (large, equality, unsorted), merge join (both sorted).

## 1.9b Other DBMS terms

- **View:** stored query, virtual table. **Materialized view:** stores the result, needs refresh. Views with GROUP BY, DISTINCT, aggregates, joins are usually not updatable.
- **Trigger:** runs automatically on INSERT/UPDATE/DELETE. **Stored procedure:** named reusable code, called explicitly. **Cursor:** row-by-row processing.
- **OLTP** (many short transactions, normalized) vs **OLAP** (analytics on big historical data, star schema). **Star schema:** central fact table with dimension tables.
- **Relational algebra:** σ (select rows), π (project columns), ∪, −, ∩, × (Cartesian product: rows multiply, columns add), ⋈ (join), ρ (rename), ÷ (division, for "for all" queries).
- **NoSQL types:** key-value (Redis), document (MongoDB), column-family (Cassandra), graph (Neo4j). **BASE** = Basically Available, Soft state, Eventual consistency.

### Tricky points / traps (DBMS)
1. Degree = columns, cardinality = rows. (Examiners swap them.)
2. Phantom = new/removed **rows**; non-repeatable = changed **value** of the same row.
3. 2PL does **not** prevent deadlock.
4. Serializable is the only standard level preventing phantoms (InnoDB's Repeatable Read also mostly prevents them with gap locks, but exam answers follow the standard).
5. A table can have only **one clustered** index but many non-clustered.
6. BCNF may not preserve dependencies; 3NF can.
7. ARIES order: Analysis, Redo, Undo.
8. Foreign keys can be NULL and duplicated.

---

# PART 1B: DBMS DEEP DIVE (Normal forms, Joins, Indexing, Trigger, Cursor, Polling, Pooling, Partitioning, Sharding, Replication)

## 1B.1 Normal forms: step-by-step identification method

When a question gives R(...) and a list of FDs, always do this:

1. **Find all candidate keys** (use closure; attributes never on the right side must be in every key).
2. List **prime attributes** (in some candidate key) and **non-prime** attributes.
3. Check each FD `X → A`:
   - **BCNF** passes if X is a super key for **every** FD.
   - **3NF** passes if for every FD, X is a super key **or A is prime**.
   - **2NF** fails if some non-prime attribute depends on a **proper subset of a candidate key** (partial dependency).
4. The highest form that passes is the answer.

**Example 1.** R(A,B,C,D), FDs: AB→C, C→D.
- B never on the right, so keys contain B. AB⁺ = ABCD, so key = **AB**. Prime: A, B. Non-prime: C, D.
- AB→C: AB is a key ✓. C→D: C is not a key and D is non-prime ✗ for 3NF (transitive dependency AB→C→D).
- Partial dependencies? A⁺ = A, B⁺ = B, so none. → **2NF, not 3NF.**

**Example 2 (classic BCNF vs 3NF).** R(A,B,C), FDs: AB→C, C→B.
- Keys: AB and AC (AC⁺ = ACB). Prime: A, B, C (all attributes prime).
- C→B: C is not a super key, so **BCNF fails**. But B is prime, so **3NF holds**.
- → **3NF, not BCNF.**

**Example 3.** R(A,B,C,D,E), FDs: A→B, B→C, C→D, D→E. Key: A. B→C etc. are transitive. Highest form = **2NF** (single-attribute key, no partial dependency; but transitive dependencies break 3NF).

**Quick facts for MCQs**
- If every attribute is prime, the relation is at least in 3NF.
- If the key is a single attribute, the relation is at least in 2NF.
- If the relation has only 2 attributes, it is in BCNF (and 3NF).
- If there are **no non-trivial FDs**, the relation is in BCNF.

**Decomposition**
- **Lossless join:** joining the decomposed tables gives back exactly the original rows (no spurious tuples). Test: common attributes form a key of at least one part.
- **Dependency preserving:** every FD can be checked within a single decomposed table without joining.
- **BCNF decomposition:** while some FD X→Y violates BCNF, split into (X ∪ Y) and (R − Y). Always lossless; may lose dependency preservation.
- **3NF synthesis:** compute a **minimal cover**, make one relation per FD (merge same left side), add a relation for a candidate key if none is contained. Lossless and dependency-preserving.
- **Minimal (canonical) cover:** (1) split right sides into single attributes, (2) remove extraneous left attributes, (3) remove redundant FDs.

**4NF (multivalued dependency).** `Person(Name, Phone, Hobby)`: a person's phones and hobbies are independent, so every phone combines with every hobby, giving redundancy. MVD: Name →→ Phone and Name →→ Hobby. Split into `(Name, Phone)` and `(Name, Hobby)`.

**5NF (join dependency):** a table that can only be reconstructed by joining three or more projections, not two.

**Denormalization:** deliberately duplicate data (e.g. store `customer_name` in `orders`) to avoid joins. Benefits: faster reads. Cost: update anomalies, more storage, extra code to keep copies in sync. Used in reporting tables, caches, NoSQL and read-heavy systems.

---

## 1B.2 Joins in depth (with outputs)

Tables:

```
Emp                          Dept
id | name | dept_id          id | dname
1  | A    | 10               10 | HR
2  | B    | 20               20 | IT
3  | C    | NULL             30 | Sales
4  | D    | 40
```

| Query | Result rows |
|---|---|
| `Emp INNER JOIN Dept ON Emp.dept_id = Dept.id` | (A,HR), (B,IT). Rows with NULL or no match (C, D, Sales) vanish |
| `Emp LEFT JOIN Dept ...` | (A,HR), (B,IT), (C,NULL), (D,NULL) = **4 rows** |
| `Emp RIGHT JOIN Dept ...` | (A,HR), (B,IT), (NULL,Sales) = **3 rows** |
| `Emp FULL JOIN Dept ...` | (A,HR), (B,IT), (C,NULL), (D,NULL), (NULL,Sales) = **5 rows** |
| `Emp CROSS JOIN Dept` | 4 × 3 = **12 rows** |

Note: **C with `dept_id NULL` never matches anything** (`NULL = NULL` is not TRUE).

**Join row-count rules**
- INNER ≤ min(LEFT, RIGHT) *only for 1:1 or N:1 relations*. With 1:N or N:M duplicates, joins can return **more rows** than either table.
- LEFT JOIN returns **at least** as many rows as the left table.
- FULL JOIN = LEFT ∪ RIGHT.
- m × n for cross join. Joining on a column with duplicates on both sides multiplies matches (if value 5 appears 2× on the left and 3× on the right, you get 6 rows for 5).

**Other join vocabulary**
- **Equi join:** condition uses `=`. **Theta join:** any comparison (`<`, `>`, `<>`).
- **Natural join:** automatically joins on same-named columns and returns one copy of them. Dangerous if schemas change.
- **`USING(col)`:** like ON for a same-named column; output has one `col`.
- **Semi join:** return left rows that have a match (implemented with `EXISTS`/`IN`). **Anti join:** left rows with **no** match (`NOT EXISTS`, `LEFT JOIN ... IS NULL`).
- **Self join:** same table twice with aliases (hierarchies, comparing rows, finding pairs).
- **Implicit join** (`FROM a, b WHERE a.id=b.id`) is an inner join in old syntax. Forgetting WHERE gives a Cartesian product.

**Join algorithms inside the engine**

| Algorithm | How | Best when |
|---|---|---|
| **Nested loop** | For each outer row, probe the inner (using an index) | Small outer set, indexed inner |
| **Hash join** | Build a hash table on the smaller input, probe with the larger | Large unsorted inputs, **equality** joins only |
| **Sort-merge join** | Sort both on the join key and merge | Both inputs already sorted / need ordered output, handles range conditions |

**Performance tips:** index the join columns (especially foreign keys), join on same data types (no implicit casts), filter early, select only required columns, avoid functions on join columns.

---

## 1B.3 Indexing: deeper

**Why indexes help.** Without an index the DB reads every row (full scan, O(N)). A B+ tree lookup reads only a few pages (O(log N)), e.g. 3 to 4 page reads for millions of rows.

**B+ tree behaviour**
- Nodes are disk pages. Internal nodes hold keys and child pointers; **leaf nodes hold keys + row locations/data** and are linked left-to-right.
- **Insert:** find the leaf, add the key; if the leaf is full it **splits** into two and pushes a separator key up. Splits may cascade to the root, which increases tree height by 1.
- **Delete:** remove the key; if a node falls below half full it borrows from a sibling or **merges**.
- Searching a range: find the first key, then walk along linked leaves.
- Keys that arrive in random order (UUID v4) cause many page splits and cache misses; sequential keys (auto-increment) append to the right edge.

**Clustered vs heap storage**
- **Clustered** (InnoDB primary key, SQL Server clustered): the table *is* the B+ tree sorted by PK.
- A **secondary index** in InnoDB stores the **PK value**, so a secondary lookup = secondary tree + PK tree ("double lookup"). Hence a **large PK makes every secondary index large**.
- **Heap** tables (PostgreSQL): rows are unordered; every index stores a row pointer (ctid).

**Composite index rules**
- Order of columns matters. Index (A,B,C) supports `A`, `A,B`, `A,B,C`; and `ORDER BY A, B`. It does **not** efficiently support `B` alone.
- Put **equality** columns first and the **range** column last: for `WHERE status='X' AND created > ...` use (status, created).
- Columns after a range predicate on an earlier column cannot be used for seeking.
- Higher selectivity (more distinct values) first is often good, but query patterns matter more.

**Selectivity and cardinality**
- **Cardinality** = number of distinct values. **Selectivity** = distinct values / total rows. Indexes help most when the predicate is **highly selective** (returns a small fraction of rows). An index on a boolean/gender column is usually useless (use bitmap/partial index).

**Special index types**
- **Covering index:** includes all needed columns; plan shows "Index Only Scan".
- **Partial / filtered index:** index only rows meeting a condition (`WHERE active = true`).
- **Function-based index:** index on an expression (`LOWER(email)`) so `WHERE LOWER(email)=...` is sargable.
- **Unique index:** enforces uniqueness. A UNIQUE constraint is usually implemented through one.
- **Full-text** (inverted), **spatial** (R-tree), **hash**, **bitmap**.

**What breaks index use (non-sargable)**
- Function on the column: `WHERE UPPER(name) = 'A'`, `WHERE YEAR(d) = 2024`. Rewrite as a range.
- Leading wildcard `LIKE '%abc'` (but `'abc%'` is fine).
- Implicit type conversion (comparing a VARCHAR column to a number).
- Arithmetic on the column: `WHERE salary * 2 > 100` → rewrite `salary > 50`.
- `OR` across different columns (may need index merge); `<>` and `NOT IN` often scan.
- Low selectivity or tiny tables (full scan is cheaper).

**Costs:** every INSERT/UPDATE/DELETE must update every index (write amplification), more disk and memory. Do not index every column. Remove unused and duplicate indexes (index (A) is redundant if (A,B) exists).

**EXPLAIN** shows the plan: sequential/full scan, index scan, index-only scan, bitmap scan, join type, estimated rows and cost. The **cost-based optimizer** uses statistics (row counts, histograms), so run `ANALYZE`/update statistics after bulk loads.

---

## 1B.4 Triggers

A **trigger** is a stored block of code that the database runs **automatically** when a specific event (INSERT, UPDATE, DELETE) happens on a table or view. You do not call it.

**Components:** timing + event + table + (optional) condition + body.
- **Timing:** `BEFORE` (can validate/modify `NEW` values before they are written), `AFTER` (runs once the change is made, good for audit logs and cascading actions), `INSTEAD OF` (on views, replaces the operation).
- **Level:** **row-level** (`FOR EACH ROW`, runs once per affected row) vs **statement-level** (runs once per statement; supported by PostgreSQL, Oracle, not by MySQL).
- **OLD / NEW:** in UPDATE, `OLD.col` is the value before and `NEW.col` after. INSERT has only `NEW`. DELETE has only `OLD`.

```sql
-- MySQL: audit salary changes
CREATE TRIGGER trg_emp_salary_audit
AFTER UPDATE ON Emp
FOR EACH ROW
BEGIN
  IF OLD.salary <> NEW.salary THEN
    INSERT INTO emp_audit(emp_id, old_salary, new_salary, changed_at)
    VALUES (OLD.id, OLD.salary, NEW.salary, NOW());
  END IF;
END;

-- BEFORE INSERT: stop negative salary
CREATE TRIGGER trg_check_salary BEFORE INSERT ON Emp FOR EACH ROW
BEGIN
  IF NEW.salary < 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Salary cannot be negative';
  END IF;
END;
```

**Uses:** audit trails, enforcing complex business rules, maintaining derived/summary columns, replicating changes.
**Drawbacks:** hidden logic (hard to debug), performance cost on every write, cascading/recursive triggers, harder testing and migration, order of multiple triggers can be unclear. Prefer constraints for simple rules and application logic or events for business flow.
**Notes:** Triggers run **inside the same transaction** as the triggering statement; if the trigger fails, the statement rolls back. Triggers generally cannot issue COMMIT/ROLLBACK. Oracle's "mutating table" error occurs when a row trigger reads the same table it is modifying.

**Trigger vs stored procedure vs function**

| | Trigger | Stored procedure | Function |
|---|---|---|---|
| Invoked | Automatically by an event | Explicitly (`CALL`) | In an expression/SELECT |
| Parameters | None | IN/OUT/INOUT | IN only |
| Return value | None | Optional (via OUT) | **Must** return a value |
| Transactions | Part of the triggering transaction | Can control transactions | Usually not |
| Use in SELECT | No | No | **Yes** |

---

## 1B.5 Cursors

A **cursor** lets procedural code process the rows of a query **one row at a time**, instead of the usual set-based processing.

- **Implicit cursor:** created automatically by the DBMS for every DML (INSERT/UPDATE/DELETE) and single-row SELECT INTO. In Oracle you read its attributes through `SQL%ROWCOUNT`.
- **Explicit cursor:** you define it for a multi-row SELECT.

**Lifecycle:** `DECLARE` → `OPEN` → `FETCH` (repeat in a loop until no more rows) → `CLOSE` (and `DEALLOCATE` in SQL Server).

```sql
-- T-SQL / MySQL style
DECLARE cur CURSOR FOR SELECT id, salary FROM Emp;
OPEN cur;
FETCH NEXT FROM cur INTO @id, @sal;
WHILE @@FETCH_STATUS = 0
BEGIN
   -- process one row
   FETCH NEXT FROM cur INTO @id, @sal;
END;
CLOSE cur;
DEALLOCATE cur;
```
Oracle cursor attributes: `%FOUND`, `%NOTFOUND`, `%ROWCOUNT`, `%ISOPEN`.

**Types:** forward-only, scrollable (next/prior/first/last/absolute), static (snapshot), dynamic (sees changes), keyset-driven; **ref cursor** (Oracle) returns a result set to callers.

**Why cursors are slow:** row-by-row ("RBAR" = row by agonizing row) processing, more round trips and locks, more memory. **Prefer set-based SQL** (one UPDATE with a JOIN) whenever possible. Use a cursor only when each row needs procedural logic that cannot be expressed in SQL.

**Related:** in JDBC a `ResultSet` behaves like a cursor (`fetchSize`). For paging use **keyset (seek) pagination** (`WHERE id > :last ORDER BY id LIMIT 20`) instead of large `OFFSET` (OFFSET still scans and discards the skipped rows).

---

## 1B.6 Polling vs alternatives (and connection pooling)

**Polling** = the client/consumer **repeatedly asks** "is there anything new?" at fixed intervals.

| Technique | How it works | Pros | Cons / use |
|---|---|---|---|
| **Short polling** | Client sends a request every N seconds. Server responds immediately (maybe empty) | Simple, works everywhere | Wasteful empty requests, delay up to N seconds |
| **Long polling** | Server **holds** the request open until data arrives or a timeout, then client immediately re-requests | Lower latency, fewer empty replies | Many open connections, reconnect overhead |
| **Server-Sent Events (SSE)** | One-way persistent HTTP stream, server → client | Simple, auto reconnect | One direction only |
| **WebSocket** | Persistent full-duplex connection | Lowest latency, two-way (chat, games, live trading) | Stateful, harder to scale/load balance |
| **Webhooks / push / queue** | Source pushes events to a callback or broker | No wasted polling | Needs retries and idempotency |

**Database polling:** an app regularly queries a table (`SELECT ... WHERE status='NEW'`) for new work. Problems: constant load, latency up to the interval, missed or duplicate processing, contention with multiple workers. Better: **triggers + queue**, **change data capture (CDC, e.g. Debezium reading the transaction log)**, Postgres `LISTEN/NOTIFY`, or the **transactional outbox** pattern (a poller or CDC reads an outbox table and publishes to Kafka). Use `SELECT ... FOR UPDATE SKIP LOCKED` so several pollers do not grab the same row.

**Kafka note:** consumers **poll** the broker (`consumer.poll()`); Kafka uses a pull model, so consumers control their own rate (backpressure).

**Connection pooling.** Opening a DB connection is expensive (TCP, auth, memory). A **pool** keeps a set of ready connections that threads borrow and return. Spring Boot uses **HikariCP** by default. Key settings: `maximumPoolSize` (default 10), `minimumIdle`, `connectionTimeout` (wait for a free connection, default 30 s), `idleTimeout`, `maxLifetime`. Too small a pool → threads wait ("Connection is not available" timeout). Too big → overloads the DB. Pool size is not "as large as possible". Connection **leaks** (not closing connections) exhaust the pool.

---

## 1B.7 Partitioning

**Partitioning** divides one large table into smaller pieces (partitions) **inside the same database** to improve manageability and performance. The application still sees one table.

| Type | Idea |
|---|---|
| **Horizontal partitioning** | Split by **rows**: each partition has the same columns but different rows |
| **Vertical partitioning** | Split by **columns**: rarely used or large columns (BLOB, text) move to another table; rows linked by PK |

**Horizontal partitioning methods**
- **Range:** by value ranges (e.g. one partition per month or year of `order_date`). Great for time-series; old data can be dropped by dropping a partition.
- **List:** by explicit values (region IN ('IN','US')).
- **Hash:** `hash(key) mod N` spreads rows evenly. Good for even distribution, bad for range queries.
- **Composite (sub-partitioning):** range then hash.

**Benefits:** **partition pruning** (the optimizer reads only relevant partitions), faster maintenance (drop/archive a partition), smaller indexes, parallelism.
**Pitfalls:** queries not filtering on the partition key scan **all** partitions; in MySQL the partition key must be part of every unique key/PK; too many partitions add overhead; uneven data causes skew.

```sql
-- PostgreSQL range partitioning
CREATE TABLE orders (id BIGINT, order_date DATE, amount NUMERIC)
  PARTITION BY RANGE (order_date);
CREATE TABLE orders_2025 PARTITION OF orders
  FOR VALUES FROM ('2025-01-01') TO ('2026-01-01');
```

**Partitioning vs Sharding (common question)**

| | Partitioning | Sharding |
|---|---|---|
| Where data lives | Same database server/instance | **Different servers/databases** |
| Scales | Manageability, query speed | **Storage, throughput, write load beyond one machine** |
| Transparency | Handled by the DB | Mostly handled by app/middleware (or distributed DB) |
| Joins/transactions | Normal | Cross-shard joins and transactions are hard |

Sharding is a form of horizontal partitioning across machines.

---

## 1B.8 Sharding

**Sharding** splits data across **multiple independent database nodes (shards)**, each holding a subset of the rows, so storage and traffic scale horizontally. Each shard is a full database with the same schema.

**Choosing the shard key** (the most important decision)
- High **cardinality**, **even distribution**, and used in most queries so that a query hits **one** shard.
- Bad key: `country` (a few huge values), `created_date` (all new writes hit the latest shard = **hotspot**), boolean.

**Strategies**
| Strategy | How | Pros | Cons |
|---|---|---|---|
| **Range-based** | user_id 1–1M → shard 1, etc. | Easy range queries | Hotspots, uneven growth |
| **Hash-based** | `hash(key) mod N` | Even spread | **Resharding moves almost all keys**, no range queries |
| **Consistent hashing** | Keys and nodes on a ring; a key goes to the next node clockwise (with virtual nodes) | Adding/removing a node moves only ~K/N keys | More complex |
| **Directory (lookup)** | A lookup table maps key → shard | Flexible | Lookup service is a SPOF/bottleneck |
| **Geo-based** | By region | Low latency, data residency | Uneven load |

**Challenges**
- **Cross-shard joins** and transactions (use denormalization, application-level joins, 2PC or sagas).
- **Hotspots / celebrity problem** (one key gets huge traffic).
- **Resharding / rebalancing** with minimal downtime.
- **Global unique IDs** (auto-increment no longer unique across shards → use UUID, Snowflake IDs, ticket servers).
- Operational complexity: backups, schema changes and monitoring across N shards.

**Order of scaling (interview answer):** optimise queries and **indexes** → add **cache** → **read replicas** → **vertical scaling** → **partitioning** → **sharding** (last, because of complexity).

---

## 1B.9 Replication and consistency

**Replication** keeps copies of the same data on several nodes for **availability, read scaling and durability**. (Sharding splits data; replication copies data. They are combined: each shard has replicas.)

- **Leader–follower (primary–replica):** writes go to the leader, which streams changes to followers; reads can go to followers. Failover promotes a follower.
- **Synchronous** replication: leader waits for follower ack (no data loss, higher latency). **Asynchronous:** leader does not wait (fast, but **replication lag** and possible data loss on failover). **Semi-synchronous:** wait for at least one follower.
- **Replication lag** causes stale reads and "read-your-own-writes" problems; fix by reading from the leader after a write or sticky routing.
- **Multi-leader:** several writable nodes (multi-region). Needs **conflict resolution** (last-write-wins, CRDTs).
- **Leaderless (Dynamo/Cassandra):** any node accepts writes. **Quorum:** with N replicas, write to W and read from R; if **W + R > N** reads see the latest write.
- **Backup is not replication:** a bad DELETE replicates instantly to followers; backups let you restore.

**CAP theorem:** in a distributed data store during a **P**artition you must choose between **C**onsistency and **A**vailability. CP (HBase, ZooKeeper, MongoDB by default) vs AP (Cassandra, DynamoDB default). **PACELC** extends it: Else (no partition) you trade **L**atency vs **C**onsistency.

**Consistency models:** strong (linearizable), eventual, causal, read-your-writes, monotonic reads.

---

## 1B.10 Locks, deadlocks and optimizer notes

- **Lock granularity:** row, page, table. Finer locks = more concurrency, more overhead. **Intent locks** (IS, IX) at table level signal row locks below.
- **Gap lock / next-key lock** (InnoDB): locks ranges to prevent phantoms in Repeatable Read.
- `SELECT ... FOR UPDATE` takes exclusive row locks (pessimistic). `FOR SHARE` takes shared locks. `SKIP LOCKED` skips locked rows (job queues). `NOWAIT` errors immediately.
- **Deadlock example:** T1 locks row A then waits for B; T2 locks B then waits for A. The DB detects the cycle and kills one transaction (victim). Prevent by **always locking in the same order**, keeping transactions short, and using lower isolation or optimistic locking.
- **Optimistic locking:** version column; `UPDATE ... SET ..., version = version + 1 WHERE id = ? AND version = ?`; if 0 rows updated, someone else changed it (JPA `@Version` throws `OptimisticLockException`).
- **Sequences / auto-increment / UUID:** auto-increment is compact and sequential but can leak counts and conflicts across shards; UUID v4 is globally unique but random (index fragmentation); UUID v7 / Snowflake are time-ordered.
- **Temporary tables, CTE, views:** a CTE is a named temporary result valid for one statement; a view is stored; a temp table is stored for the session.

### Tricky points / traps (Part 1B)
1. 3NF holds when the right side is **prime**, even if the left side is not a key (this is the gap between 3NF and BCNF).
2. LEFT JOIN row count ≥ left table count; joins with duplicates can **multiply** rows.
3. A NULL join key never matches (not even another NULL).
4. A composite index is useless if the query skips the **leftmost** column.
5. Trigger = automatic, no parameters, no return value. Function = must return a value and can be used in SELECT.
6. Cursor processing is row-by-row and slower than set-based SQL.
7. Long polling holds the request open; short polling returns immediately.
8. Partitioning stays inside one DB instance; sharding spreads across servers.
9. Hash-mod-N sharding makes resharding expensive; consistent hashing minimises key movement.
10. W + R > N gives quorum-based strong reads.
11. Replication ≠ backup.

---

# PART 2: SQL (explained in detail, with tricky patterns)

## 2.1 Categories of SQL commands

| Category | Commands | Notes |
|---|---|---|
| **DDL** (definition) | CREATE, ALTER, DROP, TRUNCATE, RENAME | Changes structure. Often auto-commits |
| **DML** (manipulation) | INSERT, UPDATE, DELETE, (MERGE) | Changes data. Can be rolled back |
| **DQL** (query) | SELECT | Reads data |
| **DCL** (control) | GRANT, REVOKE | Permissions |
| **TCL** (transaction) | COMMIT, ROLLBACK, SAVEPOINT, SET TRANSACTION | |

**DELETE vs TRUNCATE vs DROP**

| | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| Removes | selected/all rows | all rows | table itself |
| WHERE allowed | Yes | No | No |
| Triggers | Fire (row level) | Do not fire | n/a |
| Speed | Slower (row by row, logged) | Fast | Fast |
| Identity reset | No | Yes (usually) | n/a |
| Type | DML | DDL | DDL |

## 2.2 Logical order of execution (memorise)

```
1 FROM / JOIN      -> 2 WHERE      -> 3 GROUP BY
4 HAVING           -> 5 SELECT     -> 6 DISTINCT
7 ORDER BY         -> 8 LIMIT / OFFSET / TOP
```

Consequences:
- You **cannot use a SELECT alias in WHERE** (WHERE runs before SELECT). You *can* use it in ORDER BY (and in MySQL, in GROUP BY/HAVING).
- **WHERE filters rows before grouping. HAVING filters groups after aggregation.**
- Aggregate functions cannot appear in WHERE (`WHERE salary > AVG(salary)` is an error). Use a subquery.

## 2.3 NULL: the biggest source of tricky questions

NULL means *unknown / missing*. SQL uses **three-valued logic**: TRUE, FALSE, UNKNOWN. A WHERE clause keeps only rows where the condition is TRUE.

- `NULL = NULL` → UNKNOWN (not TRUE). `NULL <> 5` → UNKNOWN. Use `IS NULL` / `IS NOT NULL`.
- Any arithmetic with NULL gives NULL: `5 + NULL = NULL`.
- `TRUE OR UNKNOWN = TRUE`, `FALSE AND UNKNOWN = FALSE`, `UNKNOWN AND TRUE = UNKNOWN`.
- `WHERE dept = 10 OR dept <> 10` **does not return rows with NULL dept**.
- `NOT IN (list containing NULL)` returns **no rows** (every non-match is UNKNOWN). Prefer `NOT EXISTS`.
- `COUNT(*)` counts rows. `COUNT(col)` skips NULLs. `COUNT(DISTINCT col)` skips NULLs and duplicates.
- `SUM`, `AVG`, `MIN`, `MAX` ignore NULLs. On an **empty set or all-NULL column**: `COUNT` = 0, but `SUM/AVG/MIN/MAX` = **NULL**.
- `GROUP BY` puts all NULLs in **one group**. `DISTINCT` treats NULLs as equal. `UNIQUE` constraint allows NULLs (PostgreSQL/MySQL allow multiple).
- `COALESCE(a, b, c)` returns the first non-NULL. `NULLIF(a, b)` returns NULL if a = b. `IFNULL/NVL/ISNULL` are vendor versions.
- ORDER BY with NULLs: **MySQL** treats NULL as the smallest (first in ASC). **PostgreSQL/Oracle** treat NULL as the largest (last in ASC).

## 2.4 Aggregate functions and GROUP BY

`COUNT, SUM, AVG, MIN, MAX` (also `GROUP_CONCAT/STRING_AGG`).

- Every non-aggregated column in SELECT must appear in GROUP BY (strict mode, `ONLY_FULL_GROUP_BY`).
- `HAVING` can use aggregates; `WHERE` cannot.
- `HAVING` without GROUP BY treats the whole table as one group.
- Aggregating over joins can **double count**: if you join orders to order_items and then `SUM(order.total)`, each order total is repeated per item. Aggregate first, then join.

```sql
-- departments having more than 2 employees, with avg salary
SELECT dept, COUNT(*) AS cnt, AVG(salary) AS avg_sal
FROM Emp
GROUP BY dept
HAVING COUNT(*) > 2
ORDER BY avg_sal DESC;
```

## 2.5 Joins

| Join | Returns |
|---|---|
| **INNER** | Only matching rows |
| **LEFT (OUTER)** | All left rows + matching right rows (NULLs when no match) |
| **RIGHT (OUTER)** | All right rows + matching left rows |
| **FULL (OUTER)** | All rows from both (NULLs where no match). MySQL has no FULL JOIN (use UNION of left and right) |
| **CROSS** | Cartesian product (m × n rows) |
| **SELF** | Table joined with itself (aliases required), e.g. employee-manager |
| **NATURAL** | Joins on all same-named columns automatically (risky) |

*Example with A(1,2,3) and B(2,3,4):* INNER = {2,3} (2 rows), LEFT = {1,2,3} (3 rows), RIGHT = {2,3,4} (3 rows), FULL = {1,2,3,4} (4 rows).

**The ON vs WHERE trap (LEFT JOIN):**
```sql
-- keeps all customers (even without orders in 2024)
SELECT c.id, o.id FROM Customer c LEFT JOIN Orders o
  ON o.cust_id = c.id AND o.year = 2024;

-- filter in WHERE on the right table: becomes effectively an INNER JOIN
SELECT c.id, o.id FROM Customer c LEFT JOIN Orders o ON o.cust_id = c.id
WHERE o.year = 2024;
```
Rows where `o` is NULL fail `o.year = 2024`, so unmatched customers vanish.

**Anti-join (rows with no match):**
```sql
SELECT c.* FROM Customer c LEFT JOIN Orders o ON o.cust_id = c.id WHERE o.id IS NULL;
-- or
SELECT * FROM Customer c WHERE NOT EXISTS (SELECT 1 FROM Orders o WHERE o.cust_id = c.id);
```

## 2.6 Subqueries

- **Scalar** (one value), **row**, **table** (derived table in FROM), **correlated** (references the outer query, logically re-evaluated per outer row).
- `=` with a subquery returning more than one row is a **runtime error**. Use `IN`/`ANY`/`ALL`.
- `EXISTS` returns TRUE if the subquery returns at least one row. It stops at the first match and is NULL-safe.
- `IN` vs `EXISTS`: use `EXISTS` for large outer + small correlated checks, `IN` for small static lists. **NOT IN + NULL is dangerous.**
- `ANY` means "some", `ALL` means "every". `salary > ALL (subquery)` means greater than the maximum.

## 2.7 Set operations

| Operator | Result | Duplicates |
|---|---|---|
| UNION | all distinct rows of both | removed (sorts/dedups, slower) |
| UNION ALL | all rows of both | **kept** (faster) |
| INTERSECT | rows in both | removed |
| EXCEPT / MINUS | rows in first but not second | removed |

Rules: same number of columns, compatible types; column names come from the first query; ORDER BY only once at the end.

## 2.8 Window functions

Do calculations across related rows **without collapsing** them (unlike GROUP BY).

```sql
SELECT name, dept, salary,
  ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS rn,
  RANK()       OVER (PARTITION BY dept ORDER BY salary DESC) AS rnk,
  DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS drnk,
  SUM(salary)  OVER (PARTITION BY dept) AS dept_total,
  LAG(salary)  OVER (ORDER BY id) AS prev_salary
FROM Emp;
```
For salaries 8000, 7000, 6000, 6000, 5000:

| salary | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|
| 8000 | 1 | 1 | 1 |
| 7000 | 2 | 2 | 2 |
| 6000 | 3 | 3 | 3 |
| 6000 | 4 | **3** | **3** |
| 5000 | 5 | **5** (gap) | **4** (no gap) |

Other: `NTILE(n)`, `LEAD`, `FIRST_VALUE`, running total `SUM(x) OVER (ORDER BY d)`.
Window functions run **after** WHERE/GROUP BY/HAVING, so you cannot filter on them in WHERE. Wrap in a subquery/CTE.

## 2.9 Classic interview patterns (learn these by heart)

**1) Nth highest salary**
```sql
-- 2nd highest, simple
SELECT MAX(salary) FROM Emp WHERE salary < (SELECT MAX(salary) FROM Emp);
-- Nth highest distinct (works with ties)
SELECT salary FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) r FROM Emp) t
WHERE r = 3 GROUP BY salary;
-- LIMIT/OFFSET (needs DISTINCT to handle ties)
SELECT DISTINCT salary FROM Emp ORDER BY salary DESC LIMIT 1 OFFSET 1;
```
**2) Duplicates**
```sql
SELECT email, COUNT(*) FROM Users GROUP BY email HAVING COUNT(*) > 1;
-- delete duplicates, keep lowest id
DELETE FROM Users WHERE id NOT IN (SELECT MIN(id) FROM Users GROUP BY email);
-- (MySQL: wrap the subquery in a derived table)
```
**3) Top N per group**
```sql
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) rn FROM Emp) t
WHERE rn <= 3;
```
**4) Employees earning more than their manager**
```sql
SELECT e.name FROM Emp e JOIN Emp m ON e.manager_id = m.id WHERE e.salary > m.salary;
```
**5) Departments with no employees**
```sql
SELECT d.* FROM Dept d LEFT JOIN Emp e ON e.dept_id = d.id WHERE e.id IS NULL;
```
**6) Running total**
```sql
SELECT id, amount, SUM(amount) OVER (ORDER BY id) AS running FROM Sales;
```
**7) Pivot using CASE**
```sql
SELECT dept,
  SUM(CASE WHEN gender='M' THEN 1 ELSE 0 END) AS males,
  SUM(CASE WHEN gender='F' THEN 1 ELSE 0 END) AS females
FROM Emp GROUP BY dept;
```
**8) Department with the highest average salary**
```sql
SELECT dept FROM Emp GROUP BY dept ORDER BY AVG(salary) DESC LIMIT 1;
```
**9) Salary above overall average**
```sql
SELECT * FROM Emp WHERE salary > (SELECT AVG(salary) FROM Emp);
```

## 2.10 Constraints and other statements

- `PRIMARY KEY` (NOT NULL + UNIQUE, one per table), `UNIQUE` (many allowed), `NOT NULL`, `CHECK`, `DEFAULT`, `FOREIGN KEY`.
- `ALTER TABLE t ADD col INT;` (existing rows get NULL or the DEFAULT), `DROP COLUMN`, `MODIFY/ALTER COLUMN`.
- `UPDATE t SET c=1;` without WHERE updates **every** row.
- `INSERT INTO t SELECT ...` copies rows. `MERGE`/`UPSERT` insert-or-update.
- `LIKE`: `%` = any sequence (including empty), `_` = exactly one char. `'a_c%'` matches `abcd`, not `ac`.
- `BETWEEN a AND b` is **inclusive** on both ends.
- Limiting rows: MySQL/PostgreSQL `LIMIT n OFFSET m`; SQL Server `TOP n`; Oracle `FETCH FIRST n ROWS ONLY` / `ROWNUM`.
- String functions: `LENGTH`, `UPPER`, `SUBSTRING(str, start, len)` (1-based), `CONCAT`, `TRIM`. Date: `NOW()`, `DATEDIFF`, `EXTRACT`.
- Integer division: SQL Server `10/4 = 2`; MySQL/PostgreSQL numeric result may be `2.5000` (PostgreSQL integer/integer also gives 2). Check types.
- **Savepoint:** `ROLLBACK TO s1` undoes work after the savepoint but keeps the transaction open.
- **CTE:** `WITH t AS (SELECT ...) SELECT * FROM t;` Recursive CTEs handle hierarchies.

### Tricky points / traps (SQL)
1. `WHERE col = NULL` returns nothing. Use `IS NULL`.
2. `COUNT(*)` vs `COUNT(col)` differ when col has NULLs.
3. `SUM` of an empty set is NULL, not 0.
4. `NOT IN` with a NULL in the list returns zero rows.
5. A filter on the right table in WHERE turns a LEFT JOIN into an INNER JOIN.
6. `UNION` removes duplicates, `UNION ALL` keeps them.
7. `RANK` has gaps after ties, `DENSE_RANK` does not.
8. `HAVING` vs `WHERE` and evaluation order.
9. `LIMIT 1 OFFSET 1` works on **rows**, so ties break "Nth highest".
10. A subquery used with `=` that returns several rows is an error.
11. `TRUNCATE` cannot take WHERE; `DELETE` can.

---

# PART 3: JAVA 8 (explained in detail)

## 3.1 Lambda expressions

A lambda is an anonymous function: `(parameters) -> expression` or `(parameters) -> { statements; }`. It can be used wherever a **functional interface** is expected.

```java
Runnable r = () -> System.out.println("Hi");
Comparator<String> c = (a, b) -> a.length() - b.length();
```
**Rules**
- Parameter types can be inferred. A single parameter needs no parentheses: `x -> x * 2`.
- Variables captured from the enclosing method must be **final or effectively final** (never reassigned). Instance and static fields can be modified.
- Inside a lambda, `this` refers to the **enclosing class** instance. (In an anonymous inner class `this` is the anonymous object.)
- A lambda is not an object of a new class file. It is compiled with `invokedynamic`.

## 3.2 Functional interfaces (`java.util.function`)

A **functional interface** has exactly **one abstract method** (it may also have `default`, `static` and methods that override `Object` methods like `equals`). `@FunctionalInterface` is optional but makes the compiler check it.

| Interface | Abstract method | Meaning |
|---|---|---|
| `Predicate<T>` | `boolean test(T t)` | condition |
| `Function<T,R>` | `R apply(T t)` | transform |
| `Consumer<T>` | `void accept(T t)` | uses a value, returns nothing |
| `Supplier<T>` | `T get()` | produces a value |
| `BiFunction<T,U,R>` | `R apply(T,U)` | two inputs |
| `UnaryOperator<T>` | `T apply(T)` | `Function<T,T>` |
| `BinaryOperator<T>` | `T apply(T,T)` | `BiFunction<T,T,T>` |
| `Runnable` / `Callable<V>` | `void run()` / `V call() throws Exception` | |
| `Comparator<T>` | `int compare(T,T)` | ordering |

Composition: `f.andThen(g)` = g(f(x)); `f.compose(g)` = f(g(x)); `p.and(q)`, `p.or(q)`, `p.negate()`.
Primitive versions avoid boxing: `IntPredicate`, `IntFunction`, `ToIntFunction`, `IntBinaryOperator`.

## 3.3 Method references (4 kinds)

| Kind | Example | Equivalent lambda |
|---|---|---|
| Static method | `Integer::parseInt` | `s -> Integer.parseInt(s)` |
| Bound instance | `System.out::println` | `x -> System.out.println(x)` |
| Unbound instance | `String::length` | `s -> s.length()` |
| Constructor | `ArrayList::new` | `() -> new ArrayList<>()` |

## 3.4 Default and static methods in interfaces

- `default` methods give a body in the interface so old implementations do not break (e.g. `Iterable.forEach`, `Collection.stream`).
- **Diamond problem:** if a class implements two interfaces with the same default method, it **must override it** (otherwise compile error). Inside, call a specific one with `InterfaceA.super.hello()`.
- **Rule 1: a class always wins** over an interface default. **Rule 2:** a more specific interface wins over a less specific one.
- `static` interface methods belong to the interface and are called as `InterfaceName.method()`. They are **not inherited** by implementing classes.
- Interfaces still cannot have instance fields or constructors. Java 9 added `private` interface methods.

## 3.5 Streams API

A **Stream** is a sequence of elements supporting aggregate operations, built as a pipeline: **source → intermediate operations → terminal operation**.

- **Lazy:** intermediate operations (`filter`, `map`, `flatMap`, `distinct`, `sorted`, `limit`, `skip`, `peek`) do **nothing** until a terminal operation runs.
- **Terminal operations:** `forEach`, `collect`, `reduce`, `count`, `min/max`, `sum` (primitive streams), `anyMatch/allMatch/noneMatch`, `findFirst/findAny`, `toArray`.
- **Short-circuit:** `limit`, `findFirst`, `findAny`, `anyMatch`, `allMatch`, `noneMatch`. **Stateful:** `sorted`, `distinct` (need all elements).
- A stream **cannot be reused**: a second terminal operation throws `IllegalStateException: stream has already been operated upon or closed`.
- Streams **do not modify the source** collection.
- Elements are processed **one by one through the whole pipeline** (vertical processing), not stage by stage.

```java
List<Integer> nums = Arrays.asList(1,2,3,4,5,6);
int result = nums.stream()
    .filter(n -> n % 2 == 0)   // 2,4,6
    .map(n -> n * n)           // 4,16,36
    .reduce(0, Integer::sum);  // 56
```

**map vs flatMap:** `map` is one-to-one. `flatMap` maps each element to a stream and **flattens** (one-to-many).
`[[1,2],[3],[4,5]]` → `flatMap(List::stream)` gives `1,2,3,4,5` (count 5); `map(List::stream)` would give 3 streams.

**Creating streams:** `collection.stream()`, `Arrays.stream(arr)`, `Stream.of(...)`, `Stream.iterate(1, x -> x * 2).limit(5)` (infinite, must limit), `Stream.generate(supplier)`, `IntStream.range(1,5)` (1..4, end **exclusive**), `IntStream.rangeClosed(1,5)` (1..5).

**Primitive streams:** `IntStream`, `LongStream`, `DoubleStream` have `sum()`, `average()` (returns `OptionalDouble`), `max()` (`OptionalInt`), `summaryStatistics()`, `boxed()`, `mapToInt`, `mapToObj`. Sum of an empty IntStream is **0**.

**reduce:** `reduce(identity, accumulator)` returns `T`; `reduce(accumulator)` returns `Optional<T>`; the 3-arg form adds a combiner for parallel streams.

**Parallel streams:** `parallelStream()` uses the **ForkJoinPool common pool**. `forEach` order is **not guaranteed** (use `forEachOrdered`). Avoid shared mutable state and avoid for small data or blocking I/O.

## 3.6 Collectors

| Collector | Notes |
|---|---|
| `toList()`, `toSet()` | |
| `toMap(keyFn, valueFn)` | **Throws `IllegalStateException` on duplicate keys** unless a merge function is given: `toMap(k, v, (a,b)->a)` |
| `groupingBy(fn)` | `Map<K, List<T>>`; `groupingBy(fn, counting())` gives `Map<K, Long>` |
| `partitioningBy(pred)` | `Map<Boolean, List<T>>`, **always has both keys** (`true` and `false`), even if empty |
| `joining(", ", "[", "]")` | concatenates strings |
| `counting()`, `summingInt()`, `averagingDouble()`, `maxBy()`, `mapping()`, `collectingAndThen()` | downstream collectors |

```java
Map<String, List<Employee>> byDept = emps.stream().collect(Collectors.groupingBy(Employee::getDept));
Map<String, Long> countByDept   = emps.stream().collect(Collectors.groupingBy(Employee::getDept, Collectors.counting()));
Map<String, Double> avgSalary   = emps.stream().collect(Collectors.groupingBy(Employee::getDept, Collectors.averagingDouble(Employee::getSalary)));
```

**Sorting with Comparator**
```java
list.sort(Comparator.comparing(Employee::getDept)
                    .thenComparing(Employee::getSalary, Comparator.reverseOrder()));
```
**Trap:** `comparing(A).thenComparing(B).reversed()` reverses the **whole** comparator (both A and B).
`sorted()` on non-Comparable elements → `ClassCastException`. `Comparator.nullsFirst/nullsLast` handle nulls.

## 3.7 Optional

A container that may or may not hold a non-null value; it avoids returning null.
- `Optional.of(x)` → **NPE if x is null**. `Optional.ofNullable(x)` → empty if null. `Optional.empty()`.
- `get()` throws `NoSuchElementException` if empty (avoid; use `orElse...`).
- **`orElse(value)`** evaluates its argument **always** (eager). **`orElseGet(supplier)`** only when empty (lazy). `orElseThrow(Supplier)`.
- `map`, `flatMap`, `filter`, `isPresent`, `ifPresent(consumer)`.
- **Not in Java 8:** `isEmpty()` (Java 11), `ifPresentOrElse`, `or`, `stream()` (Java 9).
- Do not use Optional for fields, method parameters or collections.

## 3.8 New Date and Time API (`java.time`)

`LocalDate`, `LocalTime`, `LocalDateTime`, `ZonedDateTime`, `Instant`, `Duration` (time-based: hours/minutes/seconds), `Period` (date-based: years/months/days), `DateTimeFormatter`.
- **Immutable and thread-safe** (unlike `Date`, `Calendar`, `SimpleDateFormat`).
- Months are **1–12** (old `Calendar` months are 0–11).
- Operations return **new** objects: `date.plusDays(5)` does not change `date`.
- `LocalDate.of(2024, 2, 29).plusYears(1)` = **2025-02-28** (clamped to the last valid day).

## 3.9 Collections improvements

- `Map.getOrDefault`, `putIfAbsent`, `computeIfAbsent`, `computeIfPresent`, `merge` (word count: `map.merge(w, 1, Integer::sum)`), `forEach((k,v) -> ...)`, `replaceAll`.
- `Iterable.forEach`, `Collection.removeIf`, `List.sort`.
- `Arrays.asList(...)` returns a **fixed-size** list: `set` works, `add/remove` throw `UnsupportedOperationException`.

**HashMap internals (Java 8)**
- Array of buckets, default capacity **16**, load factor **0.75** (resize when size > 12), capacity doubles.
- Index = `(n - 1) & hash(key)`. Collisions form a linked list; **when a bucket reaches 8 nodes and the table capacity is ≥ 64 it becomes a red-black tree** (O(log n) instead of O(n)); it reverts to a list at 6. If capacity < 64 it resizes instead.
- Allows **one null key** and many null values. `Hashtable` and `ConcurrentHashMap` allow **no** null keys/values.
- **ConcurrentHashMap (Java 8):** no segments anymore; uses CAS and `synchronized` on the **first node of a bin**.
- `equals`/`hashCode` contract: equal objects **must** have equal hash codes. Override both together.

## 3.10 CompletableFuture and other Java 8 changes
- `CompletableFuture.supplyAsync(...)`, `thenApply` (transform), **`thenCompose`** (flatten, when the function returns another CompletableFuture), `thenCombine` (combine two), `allOf`, `exceptionally`, `handle`.
- **PermGen was removed**; class metadata is in **Metaspace** (native memory).
- `String.join(",", list)`, `StringJoiner`, `Base64` utility, repeating annotations, type annotations, `@FunctionalInterface`.

### Tricky points / traps (Java 8)
1. Streams are lazy. A pipeline with **no terminal operation prints/does nothing**.
2. A stream cannot be reused → `IllegalStateException`.
3. `Collectors.toMap` with duplicate keys → `IllegalStateException`.
4. `partitioningBy` always gives `true` and `false` keys. `groupingBy` only keys that occur.
5. `IntStream.range(1,5)` excludes 5.
6. `Optional.of(null)` → NPE. `orElse` is eager, `orElseGet` is lazy.
7. Lambda captured locals must be effectively final.
8. Default-method conflict must be resolved manually.
9. `peek` is for debugging, not for business logic.
10. `forEach` on a parallel stream is unordered.
11. `reversed()` on a chained comparator reverses everything before it.

---

# PART 4: SPRING BOOT (explained in detail)

## 4.1 Spring vs Spring Boot

- **Spring Framework:** IoC container, dependency injection, AOP, MVC, data access, transactions. Needs a lot of configuration.
- **Spring Boot:** opinionated layer on Spring that gives **auto-configuration**, **starter dependencies**, an **embedded server** (Tomcat by default, port 8080), Actuator, and an executable ("fat") JAR. Minimal XML-free configuration.
- **Spring Boot 3** requires **Java 17+**, uses **Jakarta EE** packages (`jakarta.*` instead of `javax.*`), Spring Framework 6.

## 4.2 `@SpringBootApplication`

Combination of three annotations:
1. `@SpringBootConfiguration` (a specialised `@Configuration`)
2. `@EnableAutoConfiguration`
3. `@ComponentScan` (scans the package of the main class and its **sub-packages**; beans outside that package are not found).

**Auto-configuration** looks at the classpath and existing beans, then configures sensible defaults using conditions: `@ConditionalOnClass`, `@ConditionalOnMissingBean` (back off if you define your own), `@ConditionalOnProperty`. Candidates are listed in `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` (Boot 2.7+/3; older: `spring.factories`). Add `debug=true` to print the conditions report. Exclude one with `@SpringBootApplication(exclude = XAutoConfiguration.class)`.

**Starters** (`spring-boot-starter-web`, `-data-jpa`, `-security`, `-test`, `-actuator`, `-validation`) bundle compatible dependencies. `spring-boot-starter-parent`/BOM manages versions. `spring-boot-maven-plugin` repackages the executable JAR. `spring-boot-devtools` gives hot restart.

## 4.3 IoC and Dependency Injection

- **IoC (Inversion of Control):** the container creates and wires objects instead of your code calling `new`.
- **DI types:** **constructor** (recommended: immutable, required dependencies, easy to test, detects circularity), **setter** (optional dependencies), **field** (`@Autowired` on a field: discouraged, hard to test).
- Since Spring 4.3, a class with **a single constructor** does not need `@Autowired`.
- Ambiguity: two beans of the same type → `NoUniqueBeanDefinitionException`. Fix with `@Primary` (default choice) or `@Qualifier("name")` (explicit choice).
- **Circular dependency** with constructor injection → `BeanCurrentlyInCreationException`. (Since Boot 2.6 circular references are **prohibited by default**, even for setters/fields, unless `spring.main.allow-circular-references=true`.) Fix by redesign, `@Lazy`, or setter injection.
- Stereotypes: `@Component`, `@Service`, `@Repository` (also translates persistence exceptions to Spring's `DataAccessException`), `@Controller`, `@RestController` (= `@Controller` + `@ResponseBody`).
- `@Configuration` + `@Bean` for third-party or custom beans. In a `@Configuration` class (CGLIB-proxied) calling one `@Bean` method from another returns the **same singleton**; in a plain `@Component` ("lite mode") it creates a **new** object each call.

## 4.4 Bean scopes and lifecycle

| Scope | Meaning |
|---|---|
| **singleton** (default) | One instance **per container** (per bean definition), not per JVM. Not thread-safe by itself |
| **prototype** | New instance on every request for the bean. Spring does not call destroy callbacks |
| request / session / application / websocket | Web scopes |

**Trap:** a **prototype bean injected into a singleton** is created **once** (at injection time). Use `ObjectProvider<T>`, `@Lookup` or a scoped proxy to get a fresh one each time.

**Lifecycle (order):** instantiate (constructor) → inject dependencies → `*Aware` callbacks → `BeanPostProcessor.postProcessBeforeInitialization` → **`@PostConstruct`** → `InitializingBean.afterPropertiesSet()` → custom `initMethod` → `postProcessAfterInitialization` → **bean ready** → on shutdown `@PreDestroy` → `DisposableBean.destroy()`.

## 4.5 Spring MVC / REST

**Request flow:** Client → **DispatcherServlet** (front controller) → `HandlerMapping` (find controller method) → `HandlerAdapter` → Controller → return value converted by `HttpMessageConverter` (Jackson → JSON) for `@ResponseBody`/`@RestController`, or a `ViewResolver` for views.

Annotations: `@RestController`, `@RequestMapping`, `@GetMapping/@PostMapping/@PutMapping/@PatchMapping/@DeleteMapping`, `@PathVariable` (`/users/5`), `@RequestParam` (`/users?id=5`), `@RequestBody` (JSON body → object), `@RequestHeader`, `@ResponseStatus`, `ResponseEntity<T>` (control status + headers + body).

**HTTP semantics:** GET (safe, idempotent), POST (create, **not** idempotent), PUT (replace whole resource, idempotent), PATCH (partial update), DELETE (idempotent). Status codes: 200 OK, 201 Created, 204 No Content, 400 Bad Request, 401 Unauthorized (not authenticated), 403 Forbidden (authenticated but not allowed), 404 Not Found, 409 Conflict, 415 Unsupported Media Type, 429 Too Many Requests, 500 Internal Server Error, 503 Service Unavailable.

**Validation:** `spring-boot-starter-validation`; `@Valid @RequestBody Dto dto` with `@NotNull`, `@NotBlank`, `@Size`, `@Email`, `@Min`. Failure → `MethodArgumentNotValidException` → **400**.
**Global exception handling:** `@RestControllerAdvice` / `@ControllerAdvice` + `@ExceptionHandler(XException.class)`.

## 4.6 Spring Data JPA

- `JpaRepository<Entity, ID>` gives CRUD, paging, sorting. `findById` returns `Optional<T>`. `save` inserts or updates and returns the entity.
- **Derived queries:** `findByLastNameAndAgeGreaterThan(String l, int a)`. **`@Query`** for JPQL/native SQL.
- Entity: `@Entity`, `@Id`, `@GeneratedValue(strategy = IDENTITY)`, `@OneToMany(mappedBy=...)`, `@ManyToOne`, `@Version`.
- **Lazy vs eager:** collections are LAZY by default. Accessing a lazy association after the session closed → `LazyInitializationException`.
- **N+1 select problem:** 1 query for N parents + N queries for children. Fix with `JOIN FETCH`, `@EntityGraph`, or batch fetching.
- `spring.jpa.hibernate.ddl-auto`: `none`, `validate` (check only), `update`, `create`, `create-drop`. Use `validate` or `none` in production (never `create` there).
- **`@Transactional`**
  - Works through **proxies**: a call from **within the same class** (`this.method()`) **bypasses the proxy**, so no transaction (self-invocation trap). The method should be `public`.
  - **Default rollback only for unchecked exceptions** (`RuntimeException`, `Error`). Checked exceptions do **not** roll back unless `rollbackFor = Exception.class`.
  - Default propagation **REQUIRED** (join existing or start new). `REQUIRES_NEW` suspends the current and starts a new one. Default isolation = the database default.
  - `readOnly = true` is a hint for optimisation.

## 4.7 Other essentials

- **AOP:** cross-cutting concerns (logging, transactions, security). Proxies: JDK dynamic proxy (interface-based) or CGLIB (class-based; Boot default).
- **Actuator:** `/actuator/health`, `/info`, `/metrics`, `/env`. Over the web only `health` is exposed by default; expose more with `management.endpoints.web.exposure.include=health,info,metrics`.
- **`CommandLineRunner` / `ApplicationRunner`:** run code after the context starts.
- **Logging:** Logback by default (via SLF4J). `logging.level.com.myapp=DEBUG`.
- **Embedded server swap:** exclude `spring-boot-starter-tomcat`, add `spring-boot-starter-jetty`/`undertow`.
- **Caching:** `@EnableCaching`, `@Cacheable`, `@CacheEvict`. **Scheduling:** `@EnableScheduling`, `@Scheduled`. **Async:** `@EnableAsync`, `@Async`.
- **Spring Security:** see Part 7.

### Tricky points / traps (Spring Boot)
1. `@ComponentScan` covers only the main class's package and sub-packages.
2. Prototype in singleton is injected once.
3. `@Transactional` self-invocation does nothing.
4. `@Transactional` does not roll back checked exceptions by default.
5. `@Repository` adds exception translation.
6. Constructor injection → circular dependency fails fast.
7. `ddl-auto=validate` does not change the schema.
8. 401 = not authenticated, 403 = not authorised.
9. `@RestController` = `@Controller` + `@ResponseBody`.
10. Boot 3 → Java 17+, `jakarta.*`.

---

# PART 5: SPRING BOOT CONFIGURATION

## 5.1 Configuration files
- `application.properties` or `application.yml` (YAML, hierarchical, **indentation-sensitive with spaces, never tabs**) in `src/main/resources`.
- If both exist in the same location, **`.properties` wins** over `.yml`.
- Profile-specific files: `application-dev.properties`, `application-prod.yml`. Activate with `spring.profiles.active=dev` (property, env var `SPRING_PROFILES_ACTIVE`, or `--spring.profiles.active=dev`). **Profile-specific values override** the default file. Multiple profiles: `dev,h2`.
- `@Profile("dev")` on a bean/class registers it only when that profile is active.

## 5.2 Property sources and precedence (highest wins)
1. **Command-line arguments** (`--server.port=9090`)
2. `SPRING_APPLICATION_JSON`
3. Java system properties (`-Dserver.port=9090`)
4. **OS environment variables** (`SERVER_PORT=9090`)
5. **Profile-specific** files outside then inside the JAR
6. **`application.properties/yml`** outside then inside the JAR
7. `@PropertySource` on `@Configuration` classes
8. Default properties (`SpringApplication.setDefaultProperties`)

**Location order:** `file:./config/` > `file:./` > `classpath:/config/` > `classpath:/`. So a file in a `config/` folder next to the JAR **overrides** the one packaged inside it, without rebuilding.

**Env var mapping:** upper-case, dots/hyphens → underscores: `spring.datasource.url` → `SPRING_DATASOURCE_URL`; `my.first-name` → `MY_FIRSTNAME`.

## 5.3 Reading properties
- `@Value("${app.name}")` injects one property. Default: `@Value("${app.timeout:30}")`. A missing property **without** a default → startup failure (`Could not resolve placeholder`). SpEL: `@Value("#{2 * 3}")`.
- **`@ConfigurationProperties(prefix = "app")`** binds a group of properties to a POJO: **type-safe**, supports **relaxed binding** (`my-first-name`, `myFirstName`, `MY_FIRST_NAME` all bind to `myFirstName`), nested objects, lists, validation (`@Validated`), metadata. Enable with `@EnableConfigurationProperties` or `@ConfigurationPropertiesScan` (or annotate the class with `@Component`).
- `Environment` bean: `env.getProperty("x")`.

## 5.4 Common properties
```properties
server.port=8081                       # 0 = random port
server.servlet.context-path=/api
spring.datasource.url=jdbc:mysql://localhost:3306/db
spring.datasource.username=root
spring.datasource.password=secret
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=true
logging.level.root=INFO
logging.level.com.myapp=DEBUG
spring.application.name=orders-service
management.endpoints.web.exposure.include=health,info
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```
`Failed to configure a DataSource: 'url' attribute is not specified` appears when a DB starter is on the classpath but no URL is configured (or exclude `DataSourceAutoConfiguration`).

**Externalising secrets:** do not commit passwords. Use environment variables, Kubernetes secrets, Vault, or Spring Cloud Config Server (with encryption).

### Tricky points / traps (configuration)
1. Command-line > env vars > profile files > application.properties.
2. `.properties` beats `.yml` in the same location.
3. Profile file overrides the default one.
4. `@Value` without default fails if the property is absent.
5. Relaxed binding works for `@ConfigurationProperties`; `@Value` is stricter (use the exact kebab form).
6. YAML needs spaces, not tabs.

---

# PART 6: MICROSERVICES (explained in detail) + SYSTEM DESIGN RECAP

## 6.1 What are microservices?
An architectural style where an application is built as a set of **small, independently deployable services**, each around one business capability, owning its own data, communicating over the network (REST/gRPC/messaging).

| | Monolith | Microservices |
|---|---|---|
| Deployment | One unit | Each service separately |
| Scaling | Whole app | Per service |
| Tech stack | One | Polyglot possible |
| Failure | One bug can take everything down | Isolated (with resilience patterns) |
| Complexity | In the code | In the **network, data consistency, operations** |
| Best for | Small team, simple domain, early stage | Large teams, independent scaling and release |

**Challenges:** distributed transactions, data consistency, network latency and failures, service discovery, monitoring/tracing, testing, versioning, operational overhead.

## 6.2 Core building blocks (Spring Cloud and friends)

| Concern | Pattern / tool | Notes |
|---|---|---|
| Single entry point | **API Gateway** (Spring Cloud Gateway; Zuul is legacy) | Routing, authentication, rate limiting, TLS termination, aggregation |
| Finding services | **Service discovery** (Eureka, Consul, Kubernetes DNS) | Services register themselves; clients look up by **name**. Client-side discovery + load balancing (Spring Cloud LoadBalancer; Ribbon is retired) |
| Central config | **Config Server** (Git-backed), Vault, Kubernetes ConfigMaps | `@RefreshScope` + `/actuator/refresh` to reload |
| Calling services | **OpenFeign** (declarative HTTP client), `RestTemplate` (legacy), `WebClient` (reactive), gRPC | |
| Resilience | **Resilience4j** (circuit breaker, retry, rate limiter, bulkhead, time limiter). **Hystrix is in maintenance mode** | |
| Messaging | Kafka, RabbitMQ, SQS | Async, decoupling |
| Tracing and logs | **Micrometer Tracing / Sleuth** + **Zipkin/Jaeger**, ELK/Loki, Prometheus + Grafana | **Trace ID / correlation ID** passed through all services |
| Packaging/running | Docker, Kubernetes, CI/CD | |
| Service mesh | **Istio / Linkerd** sidecar proxies | mTLS, retries, traffic splitting outside app code |

## 6.3 Key patterns

**Database per service.** Each service owns its schema; others access data only via its API/events. Gives loose coupling and independent scaling. **Sharing one database between services is an anti-pattern** (tight coupling).

**Circuit breaker.** Stops calling a failing dependency to avoid cascading failures and thread exhaustion. States: **CLOSED** (normal, counting failures) → **OPEN** (calls fail fast or return a **fallback**) → after a wait, **HALF-OPEN** (allows a few trial calls; success closes it, failure re-opens it).

**Retry / timeout / bulkhead / rate limiter.** Always set **timeouts**. Retry only **idempotent** operations (with backoff + jitter), otherwise duplicates occur. **Bulkhead** isolates resources (thread pools) per dependency so one slow service cannot exhaust all threads.

**Saga (distributed transaction).** A sequence of local transactions; if one fails, run **compensating transactions** to undo earlier steps.
- **Choreography:** services react to each other's events (no central controller, simple but hard to follow).
- **Orchestration:** a central orchestrator tells each service what to do (clear flow, one more component).
Why not 2PC? Blocking, poor availability, not supported by NoSQL/message brokers.

**Outbox pattern.** Write the business change and an "event" row in the **same local transaction**, then a poller/CDC publishes it to the broker. Solves "update DB and publish a message atomically". Consumers must be **idempotent** (at-least-once delivery).

**CQRS** (separate write and read models) and **Event sourcing** (store events, not state). **API composition** (gateway/BFF aggregates calls). **Strangler fig** (migrate a monolith gradually by routing pieces to new services). **Sidecar**, **Backend-for-Frontend (BFF)**.

**Idempotency.** Repeating a request has the same effect as doing it once (idempotency key, upsert, dedupe table).
**Dead-letter queue (DLQ):** messages that repeatedly fail are parked for inspection.
**Eventual consistency:** services converge after events propagate. Strong consistency across services is costly.

**Health and deployment.** Kubernetes **liveness probe** (is it alive? failing → restart) vs **readiness probe** (is it ready for traffic? failing → removed from load balancer). Deployment strategies: rolling, **blue-green**, **canary**. API versioning: URL (`/v1/`), header, media type. **12-factor app:** config in environment, stateless processes, logs as streams, disposability, dev/prod parity.

## 6.4 Synchronous vs asynchronous communication
- **Synchronous (REST/gRPC):** simple, immediate response, but caller depends on callee being up (**temporal coupling**), latency adds up.
- **Asynchronous (Kafka/RabbitMQ):** decoupled, buffers spikes, retries, but eventual consistency and more complexity. Use async for notifications, analytics, long workflows.

## 6.5 System design recap (quick reference)

- **Scaling:** vertical (bigger machine) vs horizontal (more machines). Stateless services scale easily; keep sessions in Redis.
- **Load balancer:** round robin, least connections, IP hash, weighted. **L4** (IP/port) vs **L7** (HTTP-aware, path/header routing). Health checks. Sticky sessions are a drawback (uneven load).
- **Caching:** cache-aside (lazy), write-through (sync write to cache+DB), write-back/behind (async, risk of loss), refresh-ahead. **Eviction:** LRU, LFU, FIFO, TTL. **Problems:** **cache stampede** (hot key expires, many DB hits → mitigate with locking/request coalescing/jittered TTL), **penetration** (queries for non-existent keys → cache nulls / Bloom filter), **avalanche** (many keys expire together → random TTL). **CDN** caches static content at the edge.
- **Rate limiting:** token bucket (allows bursts), leaky bucket (smooth), fixed window, sliding window. Return **429**.
- **Message queues:** decouple, buffer, retry. Kafka: topics → partitions (ordering **per partition**), consumer groups, offsets, at-least-once by default.
- **Consistent hashing:** hash ring (+ virtual nodes); add/remove a node moves only ~K/N keys.
- **Bloom filter:** probabilistic set; false positives possible, **no false negatives**.
- **Availability numbers:** 99.9% ≈ 8.76 h downtime/year; 99.99% ≈ 52.6 min/year.
- **Estimation:** 1M requests/day ≈ 12 requests/second. 10M users × 1 KB = 10 GB. Powers of two and "read:write ratio" drive design.
- **Latency vs throughput:** time per request vs requests per second.
- **Failure handling:** redundancy (no single point of failure), replication, health checks, failover, graceful degradation, retries with backoff, circuit breakers.
- **Real-time:** WebSocket (two-way), SSE (server → client), long polling.
- **Storage choice:** relational for transactions/joins; key-value/cache for speed; document for flexible schema; wide-column for huge writes; search engine for full text; object storage (S3) for images/videos with a CDN in front.
- **URL shortener:** base62 of a unique ID (62⁷ ≈ 3.5 trillion), key-value store, cache hot URLs.
- **News feed:** fan-out on write (precompute feeds; bad for celebrities) vs fan-out on read; use a hybrid.
- **Proximity search:** geohash / quadtree.
- **Observability pillars:** logs, metrics, traces.

### Tricky points / traps (microservices)
1. Database per service; shared DB is an anti-pattern.
2. Hystrix is deprecated; Resilience4j is the current choice. Ribbon → Spring Cloud LoadBalancer. Zuul → Spring Cloud Gateway.
3. Circuit breaker states: closed, open, half-open.
4. Retries need idempotency.
5. Saga = local transactions + compensation. 2PC blocks.
6. Liveness restarts a container; readiness controls traffic.
7. Choreography = events, no central brain; orchestration = central coordinator.

---

# PART 7: SECURITY: JWT, SPRING SECURITY, CORS

## 7.1 Authentication vs authorization
- **Authentication:** who are you? (login, token). Failure → **401 Unauthorized**.
- **Authorization:** what may you do? (roles/permissions). Failure → **403 Forbidden**.

## 7.2 JWT (JSON Web Token)

A compact, URL-safe token with three **Base64URL**-encoded parts separated by dots:
```
HEADER . PAYLOAD . SIGNATURE
{"alg":"HS256","typ":"JWT"} . {"sub":"123","name":"Asha","role":"ADMIN","iat":1735689600,"exp":1735693200} . HMAC_SHA256(base64(header)+"."+base64(payload), secret)
```
- **Header:** algorithm and type. **Payload:** claims. **Signature:** protects **integrity/authenticity**.
- **Base64URL is encoding, NOT encryption.** Anyone can decode the payload, so **never put passwords or secrets in it**. (Encrypted tokens are JWE; typical JWTs are JWS, signed only.)
- **Registered claims:** `iss` (issuer), `sub` (subject/user), `aud` (audience), `exp` (expiry time), `nbf` (not before), `iat` (issued at), `jti` (token id).
- **Signing algorithms:**
  - **HS256 (HMAC-SHA256):** symmetric. The same secret signs and verifies. Both parties must share the secret.
  - **RS256 (RSA-SHA256):** asymmetric. A **private key signs**, the **public key verifies**. Better when many services only need to verify (public key can be shared, e.g. via a JWKS endpoint).
- **Stateless authentication:** the server stores **no session**; it validates the signature and `exp` on each request. This scales horizontally and suits microservices.

**Flow:** client logs in with credentials → server verifies and returns a JWT → client sends `Authorization: Bearer <token>` on each request → a filter validates the signature and expiry and sets the security context.

**Access token vs refresh token:** access token is **short-lived** (minutes); refresh token is **long-lived**, kept more safely and used only to obtain new access tokens. On logout or compromise, revoke the refresh token.

**Weaknesses and best practices**
- **Revocation is hard:** a stateless JWT stays valid until `exp`. Use short expiry, a denylist, or rotate refresh tokens.
- **`alg: none` attack and algorithm confusion:** always enforce the expected algorithm on the server.
- **Storage:** `localStorage` is readable by JavaScript (**XSS** risk). An **HttpOnly, Secure, SameSite cookie** is safer against XSS but then needs **CSRF** protection. 
- Use HTTPS only, strong secrets (256-bit+ for HS256), validate `iss`, `aud`, `exp`.
- Large payloads increase every request's size.

## 7.3 Spring Security essentials
- Servlet **filter chain**; the central `SecurityFilterChain`. In Spring Security 5.7+/6 you define a **`SecurityFilterChain` bean**; **`WebSecurityConfigurerAdapter` is removed**.
- Typical JWT setup: custom **`OncePerRequestFilter`** that runs **before `UsernamePasswordAuthenticationFilter`**, reads the Bearer token, validates it, and sets `SecurityContextHolder`'s `Authentication`.
- `sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)`.
- `UserDetailsService` loads users; `AuthenticationManager` authenticates credentials; **`PasswordEncoder` (BCrypt)** hashes passwords: **one-way, salted, adaptive**; it cannot be "decrypted", you compare with `matches(raw, hash)`.
- Authorization: `.requestMatchers("/admin/**").hasRole("ADMIN")`, `@PreAuthorize("hasRole('ADMIN')")` (needs `@EnableMethodSecurity`). `hasRole("ADMIN")` checks authority `ROLE_ADMIN`.
- **CSRF:** needed for cookie/session-based browser apps; usually **disabled for stateless token-in-header APIs** (the browser does not auto-attach the header).
- **OAuth2 / OIDC:** OAuth2 is for delegated **authorization** (authorization-code flow with PKCE for apps); OpenID Connect adds **authentication** with an **ID token (a JWT)**.
- Unauthenticated request to a secured URL → 401; authenticated but insufficient role → 403.

## 7.4 CORS (Cross-Origin Resource Sharing)

**Origin = scheme + host + port.** `https://example.com` and `https://example.com:8443` are different origins; `http` vs `https` differ; `api.example.com` vs `example.com` differ. Path does not matter.

**Same-Origin Policy (SOP):** browsers stop JavaScript on one origin from reading responses from another. **CORS** is the mechanism by which a server **relaxes** SOP by sending permission headers.
- CORS is enforced **by the browser**. Postman, curl and server-to-server calls ignore it. "Works in Postman but fails in browser" is the classic CORS symptom.
- For a **simple request** the request is **still sent**; the browser just blocks the response if headers are missing. CORS is **not** a server-side security feature and does not stop CSRF.

**Simple vs preflighted requests**
- **Simple:** GET/HEAD/POST with only safe headers and `Content-Type` of `text/plain`, `application/x-www-form-urlencoded` or `multipart/form-data`. No preflight.
- **Preflighted:** methods like PUT/DELETE/PATCH, **custom headers** (`Authorization`), or `Content-Type: application/json`. The browser first sends an **`OPTIONS`** request with `Origin`, `Access-Control-Request-Method`, `Access-Control-Request-Headers`. The server must reply with allow headers, then the real request is sent. `Access-Control-Max-Age` caches the preflight.

**Response headers**
| Header | Meaning |
|---|---|
| `Access-Control-Allow-Origin` | Allowed origin (`*` or a specific origin) |
| `Access-Control-Allow-Methods` | Allowed HTTP methods |
| `Access-Control-Allow-Headers` | Allowed request headers |
| `Access-Control-Allow-Credentials` | `true` to allow cookies/auth. **Cannot be combined with `Allow-Origin: *`** |
| `Access-Control-Expose-Headers` | Response headers JS may read |
| `Access-Control-Max-Age` | Preflight cache time (seconds) |

**Spring Boot configuration**
```java
// 1) Per controller/method
@CrossOrigin(origins = "http://localhost:3000")
@RestController class UserController { ... }

// 2) Global (MVC)
@Configuration
public class WebConfig implements WebMvcConfigurer {
  @Override public void addCorsMappings(CorsRegistry r) {
    r.addMapping("/api/**")
     .allowedOrigins("http://localhost:3000")
     .allowedMethods("GET","POST","PUT","DELETE")
     .allowedHeaders("*")
     .allowCredentials(true)
     .maxAge(3600);
  }
}

// 3) With Spring Security: enable CORS in the filter chain
http.cors(Customizer.withDefaults());   // uses a CorsConfigurationSource bean
```
- With **Spring Security, CORS must be handled by the security filter chain** (`http.cors()`), otherwise the **preflight (which has no credentials) is rejected with 401/403**.
- With `allowCredentials(true)`, `allowedOrigins("*")` is rejected; use explicit origins or `allowedOriginPatterns`.
- Error "Invalid CORS request" (403) from Spring = origin not allowed.

### Tricky points / traps (JWT, CORS)
1. JWT payload is readable by anyone; signature only prevents tampering.
2. HS256 = shared secret; RS256 = private signs, public verifies.
3. JWT cannot be easily revoked before `exp`.
4. 401 = unauthenticated; 403 = forbidden.
5. BCrypt is hashing, not encryption.
6. CORS is enforced by browsers, not servers or Postman.
7. `Allow-Credentials: true` + `*` origin is invalid.
8. Preflight is an `OPTIONS` request, triggered by custom headers or JSON content type.
9. Different port = different origin.

---

# PART 8: OOP DESIGN: ASSOCIATION, AGGREGATION, COMPOSITION, COHESION, COUPLING, SOLID

## 8.1 Relationships between classes

| Relationship | Meaning | Lifecycle | Example | UML |
|---|---|---|---|---|
| **Dependency** | One class uses another temporarily (parameter/local var) | none | `OrderService` uses `Logger` | dashed arrow |
| **Association** | "Uses-a/knows-a"; objects have **independent** lifecycles | independent | Teacher ↔ Student | line |
| **Aggregation** | **"Has-a" (weak, whole–part)**; part can exist **without** the whole | part survives | Department has Teachers; Team has Players | **hollow** diamond |
| **Composition** | **"Part-of" (strong whole–part)**; the part **cannot exist without** the whole and is destroyed with it | part dies with whole | House has Rooms; Order has OrderLines | **filled** diamond |
| **Inheritance** | "Is-a" | | Dog is-an Animal | hollow triangle |

**Memory trick:** Composition = *death together* (delete the House, the Rooms are gone). Aggregation = *shared/independent* (delete the Department, Teachers still exist).

```java
class Engine {}
class Car {                       // COMPOSITION: Car creates and owns Engine
  private final Engine engine = new Engine();
}
class Department {                // AGGREGATION: teachers are created elsewhere and passed in
  private List<Teacher> teachers;
  Department(List<Teacher> t) { this.teachers = t; }
}
```
In Java, both are implemented with fields; the difference is **ownership and lifetime** (who creates the part, and whether it can outlive the whole).

## 8.2 Cohesion and coupling

**Cohesion** = how strongly the responsibilities inside **one module/class** belong together. **High cohesion is good** (focused, readable, reusable).
**Coupling** = how strongly **different modules depend on each other**. **Low coupling is good** (change one module without breaking others).
Goal: **high cohesion, low (loose) coupling.**

**Types of cohesion (best → worst)**
1. **Functional** (all parts contribute to one well-defined task) ← best
2. Sequential (output of one part is input of next)
3. Communicational (parts operate on the same data)
4. Procedural (parts follow an order of execution)
5. Temporal (grouped because they run at the same time, e.g. "initialization")
6. Logical (grouped by category, e.g. one "Utils" class doing unrelated things selected by a flag)
7. **Coincidental** (random grouping) ← worst

**Types of coupling (worst → best)**
1. **Content** (one module modifies/relies on another's internals) ← worst
2. Common (shared global data)
3. External (shared external format/protocol)
4. Control (one passes a flag that controls the other's logic)
5. Stamp (share a whole data structure but use part)
6. **Data** (share only needed primitive data via parameters) ← best desirable
7. Message (communicate through messages/events; loosest)

**How to reduce coupling:** program to interfaces, **dependency injection**, events/queues, avoid global state, Law of Demeter. **How to raise cohesion:** Single Responsibility, split "God classes".

*Example of low cohesion:* a class `UserManager` that validates input, queries the DB, sends emails and prints reports.

## 8.3 SOLID

| Principle | Statement | Violation example |
|---|---|---|
| **S**ingle Responsibility | A class should have **one reason to change** | `Invoice` that calculates, prints and saves to DB |
| **O**pen/Closed | Open for extension, closed for modification (add new behaviour via new classes/strategies, not editing `if-else` chains) | Adding `else if (type == "NEW")` for each new payment type |
| **L**iskov Substitution | Subtypes must be usable wherever the base type is expected without breaking behaviour | `Square extends Rectangle` where setting width changes height; `Penguin extends Bird` with `fly()` throwing |
| **I**nterface Segregation | Many small client-specific interfaces rather than one fat interface | `Worker` interface forcing a `Robot` to implement `eat()` |
| **D**ependency Inversion | High-level modules depend on **abstractions**, not concrete classes | `OrderService` doing `new MySqlRepository()`. Fix: inject a `Repository` interface |

**IoC** is the principle (the framework controls creation/flow), **DI** is the technique (dependencies are supplied from outside), **DIP** is the design guideline (depend on abstractions).

## 8.4 Core OOP and Java basics frequently tested

- **Four pillars:** Encapsulation (hide data, expose methods), Abstraction (hide complexity), Inheritance (reuse), Polymorphism (many forms).
- **Overloading** = same name, different parameters, resolved at **compile time** (static polymorphism). **Overriding** = subclass redefines a method, resolved at **runtime** (dynamic polymorphism). Fields and static methods are **hidden**, not overridden.
- **Abstract class vs interface:** abstract class can have state (fields), constructors, any access modifier, single inheritance. Interface: multiple inheritance of type, (Java 8) default/static methods, no instance state.
- **`==` vs `equals`:** `==` compares references (for primitives, values). `equals` compares content when overridden. `new String("a") == new String("a")` is **false**; `"a" == "a"` is **true** (string pool).
- **String** is immutable; `s.concat("b")` returns a new String and `s` is unchanged. Use `StringBuilder` for mutation (`StringBuffer` is synchronized).
- **`final`** (constant/no override/no subclass), **`finally`** (always runs, even after `return`; a `return` in finally overrides), **`finalize()`** (deprecated).
- **Checked** exceptions (must handle: `IOException`) vs **unchecked** (`RuntimeException`, `Error`).
- **Fail-fast iterators** throw `ConcurrentModificationException` if the collection is structurally modified during iteration. `CopyOnWriteArrayList` is fail-safe.
- **Singleton**, **Factory**, **Builder**, **Observer**, **Strategy**, **Decorator**, **Proxy**, **Adapter** are the common patterns. Spring's beans are singletons by default; `@Transactional` uses the **Proxy** pattern; `JdbcTemplate` uses **Template Method**.
- **Immutability:** `final` class, `private final` fields, no setters, defensive copies.
- **Static vs instance:** static members belong to the class; static methods cannot use `this`.

### Tricky points / traps (OOP)
1. Aggregation = part can live alone; composition = part dies with the whole.
2. High cohesion + low coupling is the goal. Best cohesion = functional; worst = coincidental. Worst coupling = content.
3. LSP violation: subclass strengthens preconditions or throws unexpected exceptions.
4. A `return` in `finally` overrides the `try` return.
5. `String` is immutable; operations return new strings.

---

# PART 9: JUNIT 5 AND MOCKITO (explained in detail)

## 9.1 JUnit 5 architecture
**JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage** (runs JUnit 3/4 tests). Spring Boot 2.2+ uses JUnit 5 by default (`spring-boot-starter-test`, which also includes Mockito, AssertJ, Hamcrest, JSONassert, MockMvc support).

| JUnit 5 | JUnit 4 equivalent |
|---|---|
| `@Test` (`org.junit.jupiter.api`) | `@Test` (`org.junit`) |
| `@BeforeEach` / `@AfterEach` | `@Before` / `@After` |
| `@BeforeAll` / `@AfterAll` (**static** by default) | `@BeforeClass` / `@AfterClass` |
| `@Disabled` | `@Ignore` |
| `@ExtendWith` | `@RunWith` |
| `@Tag` | `@Category` |

Others: `@DisplayName`, `@Nested` (group tests), `@RepeatedTest(5)`, `@ParameterizedTest` with `@ValueSource`, `@CsvSource`, `@MethodSource`, `@EnumSource`; `@TestInstance(Lifecycle.PER_CLASS)` (then `@BeforeAll` need not be static); `@Timeout`.
**By default JUnit creates a new test-class instance for each test method** (so instance fields are not shared between tests).

## 9.2 Assertions
```java
assertEquals(expected, actual);        // order: EXPECTED first
assertNotEquals, assertTrue, assertFalse, assertNull, assertNotNull, assertSame
assertThrows(IllegalArgumentException.class, () -> service.find(-1));
assertAll(() -> assertEquals(1, a), () -> assertEquals(2, b));   // reports all failures
assertTimeout(Duration.ofSeconds(1), () -> ...);
assertArrayEquals(expectedArr, actualArr);
```
`assertEquals(double, double, delta)` for floating point. **Assumptions** (`assumeTrue`) *abort* (skip) a test instead of failing it.

## 9.3 Mockito
A mocking framework that creates **fake dependencies** so a class is tested in isolation.

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {
  @Mock  PaymentClient paymentClient;      // a fake
  @InjectMocks OrderService orderService;  // real object with mocks injected

  @Test void placeOrder_ok() {
    when(paymentClient.charge(100)).thenReturn(true);        // stub
    assertTrue(orderService.place(100));
    verify(paymentClient, times(1)).charge(100);             // interaction check
    verify(paymentClient, never()).refund(anyInt());
  }
}
```
- **Mock vs Spy:** a **mock** is fully fake (all methods return defaults unless stubbed). A **spy** wraps a **real** object (real methods run unless stubbed). For spies and void methods use `doReturn(x).when(spy).method()`, `doThrow(...)`, `doNothing()`.
- **Unstubbed defaults:** `null` for objects, `0`, `false`, **empty collections** (`List` → empty), `Optional.empty()`.
- **Matchers:** `any()`, `anyInt()`, `eq()`. **Rule: if one argument uses a matcher, all must use matchers** (`verify(m).f(anyInt(), "x")` is wrong → use `eq("x")`), otherwise `InvalidUseOfMatchersException`.
- `ArgumentCaptor<T>` captures the argument passed to a mock for inspection.
- `BDDMockito`: `given(...).willReturn(...)`, `then(...).should()`.
- Mockito cannot mock `final` classes/methods or `static` methods with the old default mock maker (Mockito 5 uses the inline mock maker by default; `mockStatic` is available).
- Do not mock the class under test or value objects/DTOs; mock **collaborators** (repositories, HTTP clients).

## 9.4 Spring Boot testing

| Annotation | Loads | Use |
|---|---|---|
| `@SpringBootTest` | **Full application context** (slow). `webEnvironment = RANDOM_PORT` starts a server | Integration tests |
| **`@WebMvcTest(UserController.class)`** | **Only the web layer** (controllers, advice, filters, Jackson). Use **`MockMvc`**. Services must be provided with **`@MockBean`** | Controller unit tests |
| **`@DataJpaTest`** | **Only JPA layer** (entities, repositories) with an **embedded DB** (H2) by default; each test **rolls back** its transaction | Repository tests |
| `@JsonTest`, `@RestClientTest` | Slices | |
| `@MockBean` | Puts a Mockito mock in the **Spring context** (replaces a bean). (From Spring Boot 3.4: `@MockitoBean`) | |
| `@TestPropertySource`, `@ActiveProfiles("test")` | Test config | |
| `@Sql`, `@Transactional` | DB setup, rollback | |

```java
@WebMvcTest(UserController.class)
class UserControllerTest {
  @Autowired MockMvc mockMvc;
  @MockBean  UserService service;

  @Test void getUser() throws Exception {
    when(service.find(1L)).thenReturn(new User(1L, "Asha"));
    mockMvc.perform(get("/users/1"))
           .andExpect(status().isOk())
           .andExpect(jsonPath("$.name").value("Asha"));
  }
}
```
**`@Mock`** (Mockito only, no Spring) vs **`@MockBean`** (registers in the Spring context).

## 9.5 Testing concepts
- **Unit test:** one class in isolation, fast, mocks collaborators. **Integration test:** several components/real DB or HTTP. **End-to-end:** whole system. **Test pyramid:** many unit, fewer integration, very few E2E.
- **TDD:** Red (write a failing test) → Green (make it pass) → Refactor.
- **F.I.R.S.T.:** Fast, Independent, Repeatable, Self-validating, Timely.
- **Arrange–Act–Assert** (Given–When–Then) structure.
- **Coverage tools:** JaCoCo (line/branch coverage). 100% coverage does **not** mean bug-free.
- A test that depends on execution order, current time, or randomness is flaky.

### Tricky points / traps (JUnit)
1. `@BeforeAll` methods must be `static` (unless `PER_CLASS` lifecycle).
2. `assertEquals(expected, actual)`: expected goes first.
3. Mixing raw values and matchers in a Mockito call is an error.
4. `@WebMvcTest` does not load services; use `@MockBean`.
5. `@DataJpaTest` rolls back by default and uses an embedded DB.
6. Unstubbed mock methods return defaults, not exceptions.
7. JUnit 5 = Platform + Jupiter + Vintage.

---

# PART 10: JAVA STREAM CODING PRACTICE (all solutions tested on Java 21)

Your screenshot mentions about 2 coding questions (Java streams, 1 hard). Practise these without looking, then compare. All outputs below were verified by running the code.

**Common setup**
```java
class Employee {
  private final int id; private final String name, dept; private final double salary;
  // constructor + getters: getId(), getName(), getDept(), getSalary()
}
List<Employee> emps = Arrays.asList(
  new Employee(1,"Asha","IT",90000),  new Employee(2,"Ravi","IT",70000),
  new Employee(3,"Meena","HR",60000), new Employee(4,"Karan","HR",65000),
  new Employee(5,"Divya","Sales",80000), new Employee(6,"Omar","IT",90000));
```

### Easy and medium

**1. Second highest distinct salary**
```java
Optional<Double> second = emps.stream().map(Employee::getSalary).distinct()
    .sorted(Comparator.reverseOrder()).skip(1).findFirst();      // 80000.0
```
`distinct()` is essential: two employees earn 90000.

**2. Count employees per department**
```java
Map<String, Long> cnt = emps.stream().collect(
    Collectors.groupingBy(Employee::getDept, TreeMap::new, Collectors.counting()));
// {HR=2, IT=3, Sales=1}
```

**3. Names per department**
```java
Map<String, List<String>> names = emps.stream().collect(
    Collectors.groupingBy(Employee::getDept, TreeMap::new,
        Collectors.mapping(Employee::getName, Collectors.toList())));
// {HR=[Meena, Karan], IT=[Asha, Ravi, Omar], Sales=[Divya]}
```

**4. Average salary per department**
```java
Map<String, Double> avg = emps.stream().collect(
    Collectors.groupingBy(Employee::getDept, Collectors.averagingDouble(Employee::getSalary)));
// HR=62500.0, IT=83333.33, Sales=80000.0
```

**5. Employees earning more than the overall average**
```java
double avgAll = emps.stream().mapToDouble(Employee::getSalary).average().orElse(0);
List<String> above = emps.stream().filter(e -> e.getSalary() > avgAll)
    .map(Employee::getName).collect(Collectors.toList());     // [Asha, Divya, Omar]
```

**6. Sort by department asc, salary desc, then name**
```java
emps.stream().sorted(Comparator.comparing(Employee::getDept)
    .thenComparing(Employee::getSalary, Comparator.reverseOrder())
    .thenComparing(Employee::getName)).collect(Collectors.toList());
// [Karan, Meena, Asha, Omar, Ravi, Divya]
```

**7. First non-repeated character**
```java
String s = "swiss";
Character ch = s.chars().mapToObj(c -> (char) c)
    .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting()))
    .entrySet().stream().filter(e -> e.getValue() == 1)
    .map(Map.Entry::getKey).findFirst().orElse(null);          // 'w'
```
`LinkedHashMap` keeps encounter order, which is what makes "first" correct.

**8. Find duplicates in a list**
```java
List<Integer> nums = Arrays.asList(1,2,3,2,4,3,5);
Set<Integer> seen = new HashSet<>();
Set<Integer> dups = nums.stream().filter(n -> !seen.add(n))
    .collect(Collectors.toSet());                              // [2, 3]
// alternative: groupingBy(identity, counting()) then filter count > 1
```

**9. Word frequency (sorted by count desc, then word)**
```java
String text = "the cat and the dog and the bird";
Arrays.stream(text.split(" "))
  .collect(Collectors.groupingBy(w -> w, Collectors.counting()))
  .entrySet().stream()
  .sorted(Map.Entry.<String, Long>comparingByValue().reversed()
          .thenComparing(Map.Entry.comparingByKey()))
  .map(e -> e.getKey() + "=" + e.getValue()).collect(Collectors.toList());
// [the=3, and=2, bird=1, cat=1, dog=1]
```

**10. Flatten nested lists and double each**
```java
List<List<Integer>> nested = Arrays.asList(Arrays.asList(1,2), Arrays.asList(3), Arrays.asList(4,5));
nested.stream().flatMap(List::stream).map(x -> x * 2).collect(Collectors.toList());  // [2,4,6,8,10]
```

**11. Partition numbers 2..15 into primes and non-primes**
```java
Map<Boolean, List<Integer>> parts = IntStream.rangeClosed(2, 15).boxed()
  .collect(Collectors.partitioningBy(n ->
      IntStream.rangeClosed(2, (int) Math.sqrt(n)).noneMatch(i -> n % i == 0)));
// {false=[4, 6, 8, 9, 10, 12, 14, 15], true=[2, 3, 5, 7, 11, 13]}
```

**12. Longest string / sum of squares of odd numbers / joining**
```java
Stream.of("a","abcd","abc","wxyz").max(Comparator.comparingInt(String::length)).get(); // abcd
IntStream.rangeClosed(1,10).filter(n -> n % 2 == 1).map(n -> n * n).sum();            // 165
Stream.of("java","spring","boot").map(String::toUpperCase)
      .collect(Collectors.joining(", ", "[", "]"));                                    // [JAVA, SPRING, BOOT]
```
(Here `abcd` and `wxyz` tie on length 4. `max` returned `abcd`, the first one. Always state your tie rule.)

**13. Palindrome and anagram checks**
```java
String p = "madam";
boolean pal = IntStream.range(0, p.length() / 2).allMatch(i -> p.charAt(i) == p.charAt(p.length() - 1 - i)); // true
boolean ana = Arrays.equals("listen".chars().sorted().toArray(), "silent".chars().sorted().toArray());        // true
```

**14. Character frequency of a string**
```java
"banana".chars().mapToObj(c -> (char) c)
  .collect(Collectors.groupingBy(Function.identity(), TreeMap::new, Collectors.counting()));  // {a=3, b=1, n=2}
```

**15. Merge two lists, remove duplicates, sort**
```java
Stream.concat(Arrays.asList(1,3,5).stream(), Arrays.asList(2,3,6).stream())
      .distinct().sorted().collect(Collectors.toList());   // [1, 2, 3, 5, 6]
```

**16. Sort a Map by value descending (keeping order)**
```java
Map<String,Integer> m = new HashMap<>(); m.put("a",3); m.put("b",1); m.put("c",2);
LinkedHashMap<String,Integer> sorted = m.entrySet().stream()
  .sorted(Map.Entry.<String,Integer>comparingByValue().reversed())
  .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (x, y) -> x, LinkedHashMap::new));
// {a=3, c=2, b=1}
```
A plain `toMap` would give a `HashMap` and lose the order, so supply `LinkedHashMap::new`.

**17. First 10 Fibonacci numbers**
```java
Stream.iterate(new long[]{0, 1}, f -> new long[]{f[1], f[0] + f[1]})
      .limit(10).map(f -> f[0]).collect(Collectors.toList());   // [0,1,1,2,3,5,8,13,21,34]
```

### Hard (the "1 hard" type)

**H1. Highest-paid employee in each department**
```java
Map<String, Optional<Employee>> top = emps.stream().collect(
  Collectors.groupingBy(Employee::getDept,
      Collectors.maxBy(Comparator.comparingDouble(Employee::getSalary))));
// HR=Karan, IT=Asha (first of the two 90000 earners), Sales=Divya

// Without Optional in the result:
Map<String, Employee> top2 = emps.stream().collect(Collectors.toMap(
    Employee::getDept, Function.identity(),
    BinaryOperator.maxBy(Comparator.comparingDouble(Employee::getSalary))));
```

**H2. Nth highest salary employee(s) (here N = 3, distinct salaries)**
```java
double third = emps.stream().map(Employee::getSalary).distinct()
    .sorted(Comparator.reverseOrder()).skip(2).findFirst().get();   // 70000.0
List<String> who = emps.stream().filter(e -> e.getSalary() == third)
    .map(Employee::getName).collect(Collectors.toList());           // [Ravi]
```

**H3. Second highest salary in EACH department**
```java
Map<String, Double> secondPer = emps.stream().collect(
  Collectors.groupingBy(Employee::getDept, TreeMap::new,
    Collectors.collectingAndThen(Collectors.toList(), list -> list.stream()
        .map(Employee::getSalary).distinct()
        .sorted(Comparator.reverseOrder()).skip(1).findFirst().orElse(null))));
// {HR=60000.0, IT=70000.0, Sales=null}
```
Sales has only one salary, so the result is `null` for it. State how you handle that case (return `Optional`, skip the department, or use a default).

**H4. Group, then pick top 2 earners per department**
```java
Map<String, List<String>> top2 = emps.stream().collect(Collectors.groupingBy(
    Employee::getDept, TreeMap::new,
    Collectors.collectingAndThen(Collectors.toList(), l -> l.stream()
        .sorted(Comparator.comparingDouble(Employee::getSalary).reversed())
        .limit(2).map(Employee::getName).collect(Collectors.toList()))));
```

**H5. Employees whose salary equals the maximum of their department (ties included)**
```java
Map<String, Double> maxByDept = emps.stream().collect(
    Collectors.groupingBy(Employee::getDept, Collectors.collectingAndThen(
        Collectors.maxBy(Comparator.comparingDouble(Employee::getSalary)), o -> o.get().getSalary())));
List<String> topEarners = emps.stream()
    .filter(e -> e.getSalary() == maxByDept.get(e.getDept()))
    .map(Employee::getName).collect(Collectors.toList());   // [Asha, Karan, Divya, Omar]
```

### Interview tips for stream coding questions
1. State the **tie/duplicate policy** (distinct or not) before you code.
2. Use `LinkedHashMap::new`/`TreeMap::new` when order matters.
3. Always provide a **merge function** to `toMap` if keys can repeat.
4. Handle empty inputs (`Optional`, `orElse`).
5. Write a small `Employee` class and a `main` with sample data; explain complexity (sorting is O(n log n), grouping O(n)).

---

# PART 11: PRACTICE QUESTIONS WITH ANSWERS


## A. DBMS concepts


### DBMS concepts

**Q1.** Which constraint guarantees that a column never holds NULL and that every row is uniquely identifiable?

- **A.** FOREIGN KEY
- **B.** DEFAULT
- **C.** PRIMARY KEY
- **D.** CHECK

> **Answer: C.** A primary key combines NOT NULL with uniqueness.  
> *Revise:* Part 1 (DBMS)

**Q2.** A foreign key column is used to

- **A.** Physically sort the table
- **B.** Uniquely identify rows in its own table
- **C.** Store only NULL values
- **D.** Reference the primary or candidate key of another (or the same) table

> **Answer: D.** A foreign key enforces a link to a referenced key.  
> *Revise:* Part 1 (DBMS)

**Q3.** A minimal set of attributes that uniquely identifies a tuple is called a

- **A.** Super key
- **B.** Composite attribute
- **C.** Candidate key
- **D.** Foreign key

> **Answer: C.** Candidate key = minimal super key.  
> *Revise:* Part 1 (DBMS)

**Q4.** Which statement about keys is TRUE?

- **A.** Every candidate key is a super key
- **B.** A table can have only one super key
- **C.** A super key can have only one attribute
- **D.** Every super key is a candidate key

> **Answer: A.** A candidate key is a minimal super key, so it is always a super key. The reverse is false.  
> *Revise:* Part 1 (DBMS)

**Q5.** R(A,B,C) has FDs A→B and B→C, and A is the only candidate key. R is in

- **A.** 3NF but not BCNF
- **B.** BCNF
- **C.** 1NF only
- **D.** 2NF but not 3NF

> **Answer: D.** No partial dependency (single-attribute key) gives 2NF, but A→B→C is a transitive dependency, so 3NF fails.  
> *Revise:* Part 1 (DBMS)

**Q6.** First Normal Form (1NF) requires

- **A.** Atomic column values and no repeating groups
- **B.** Every determinant is a super key
- **C.** No partial dependencies
- **D.** No transitive dependencies

> **Answer: A.** 1NF is about atomic values.  
> *Revise:* Part 1 (DBMS)

**Q7.** A relation is in BCNF if for every non-trivial FD X→Y,

- **A.** Y is a subset of X
- **B.** X is a super key
- **C.** X is a foreign key
- **D.** Y is a prime attribute

> **Answer: B.** BCNF demands every determinant be a super key.  
> *Revise:* Part 1 (DBMS)

**Q8.** Every relation in BCNF is also in

- **A.** Only 2NF
- **B.** Only 1NF
- **C.** None of the other normal forms
- **D.** 3NF

> **Answer: D.** BCNF is strictly stronger than 3NF.  
> *Revise:* Part 1 (DBMS)

**Q9.** Which statement about decomposition is TRUE?

- **A.** 3NF decomposition can always be made both lossless and dependency-preserving
- **B.** BCNF decomposition is always lossy
- **C.** BCNF decomposition is always dependency-preserving
- **D.** 3NF removes all redundancy

> **Answer: A.** 3NF synthesis guarantees lossless + dependency preservation. BCNF may sacrifice dependency preservation.  
> *Revise:* Part 1 (DBMS)

**Q10.** Deleting one row unintentionally removes other unrelated facts. This is a

- **A.** Deletion anomaly
- **B.** Phantom anomaly
- **C.** Update anomaly
- **D.** Insertion anomaly

> **Answer: A.** Classic deletion anomaly caused by redundancy in an unnormalized table.  
> *Revise:* Part 1 (DBMS)

**Q11.** Which normal form deals with multivalued dependencies?

- **A.** 2NF
- **B.** 3NF
- **C.** BCNF
- **D.** 4NF

> **Answer: D.** 4NF removes non-trivial multivalued dependencies.  
> *Revise:* Part 1 (DBMS)

**Q12.** R(A,B,C,D) with FDs A→B, B→C, C→D. The closure A+ is

- **A.** {A,B}
- **B.** {A,B,C}
- **C.** {A,B,C,D}
- **D.** {A}

> **Answer: C.** A→B, B→C, C→D chain gives all attributes.  
> *Revise:* Part 1 (DBMS)

**Q13.** R(A,B,C,D) with FDs AB→C, C→D, D→A. How many candidate keys does R have?

- **A.** 1
- **B.** 2
- **C.** 3
- **D.** 4

> **Answer: C.** B never appears on the right, so every key contains B. AB, BC and BD each determine all attributes, giving 3 keys.  
> *Revise:* Part 1 (DBMS)

**Q14.** A decomposition of R into R1 and R2 is lossless if

- **A.** R1 and R2 have the same number of attributes
- **B.** R1 and R2 share no attributes
- **C.** R1 ∪ R2 = R only
- **D.** (R1 ∩ R2) → R1 or (R1 ∩ R2) → R2

> **Answer: D.** The common attributes must be a key of at least one of the two relations.  
> *Revise:* Part 1 (DBMS)

**Q15.** Armstrong's reflexivity rule says: if Y ⊆ X, then

- **A.** Y → X
- **B.** X → Z for any Z
- **C.** X → Y
- **D.** Nothing can be inferred

> **Answer: C.** Reflexivity: a set determines its own subsets.  
> *Revise:* Part 1 (DBMS)

**Q16.** A transfer debits account A, then the system crashes before crediting account B. Atomicity requires the system to

- **A.** Roll back the debit
- **B.** Credit B twice after restart
- **C.** Lock both accounts forever
- **D.** Keep the debit

> **Answer: A.** All-or-nothing: partial work must be undone.  
> *Revise:* Part 1 (DBMS)

**Q17.** Durability is primarily achieved through

- **A.** Write-ahead logging and recovery
- **B.** Foreign keys
- **C.** Normalization
- **D.** Query optimization

> **Answer: A.** Committed changes are logged to stable storage first (WAL) and replayed after a crash.  
> *Revise:* Part 1 (DBMS)

**Q18.** Which ACID property is mainly enforced by the concurrency control manager?

- **A.** Isolation
- **B.** Durability
- **C.** Consistency
- **D.** Atomicity

> **Answer: A.** Locks / MVCC / timestamps provide isolation.  
> *Revise:* Part 1 (DBMS)

**Q19.** A dirty read happens when a transaction

- **A.** Reads the same row twice and sees different values
- **B.** Cannot read because of a lock
- **C.** Sees new rows appear on re-running a query
- **D.** Reads data written by another uncommitted transaction

> **Answer: D.** Dirty read = reading uncommitted data.  
> *Revise:* Part 1 (DBMS)

**Q20.** A non-repeatable read means

- **A.** Reading uncommitted data
- **B.** A transaction being rolled back
- **C.** Re-reading a row in one transaction and getting a different value because another transaction committed an update
- **D.** New rows appearing in a repeated range query

> **Answer: C.** Same row, different value between two reads.  
> *Revise:* Part 1 (DBMS)

**Q21.** Which SQL-standard isolation level prevents phantom reads?

- **A.** Serializable
- **B.** Repeatable Read
- **C.** Read Uncommitted
- **D.** Read Committed

> **Answer: A.** Only Serializable prevents dirty reads, non-repeatable reads and phantoms per the standard.  
> *Revise:* Part 1 (DBMS)

**Q22.** The default isolation level in PostgreSQL is

- **A.** Read Uncommitted
- **B.** Repeatable Read
- **C.** Read Committed
- **D.** Serializable

> **Answer: C.** PostgreSQL defaults to Read Committed (MySQL InnoDB defaults to Repeatable Read).  
> *Revise:* Part 1 (DBMS)

**Q23.** Strict two-phase locking (2PL) guarantees

- **A.** Faster commits
- **B.** Freedom from deadlock
- **C.** Fewer locks
- **D.** Conflict serializability

> **Answer: D.** 2PL ensures conflict-serializable schedules but can still deadlock.  
> *Revise:* Part 1 (DBMS)

**Q24.** Deadlocks are typically detected using a

- **A.** Precedence graph
- **B.** Wait-for graph
- **C.** ER diagram
- **D.** Dependency diagram

> **Answer: B.** A cycle in the wait-for graph means deadlock.  
> *Revise:* Part 1 (DBMS)

**Q25.** A schedule is conflict-serializable if and only if its precedence graph

- **A.** Is complete
- **B.** Has no vertices
- **C.** Contains a cycle
- **D.** Is acyclic

> **Answer: D.** Acyclic precedence graph means an equivalent serial order exists.  
> *Revise:* Part 1 (DBMS)

**Q26.** Which of these is NOT a concurrency control technique?

- **A.** Optimistic validation
- **B.** Normalization
- **C.** Two-phase locking
- **D.** Timestamp ordering

> **Answer: B.** Normalization is a schema design technique.  
> *Revise:* Part 1 (DBMS)

**Q27.** MVCC (multi-version concurrency control) allows

- **A.** Readers to block writers
- **B.** Readers to proceed without blocking writers by reading a snapshot or older version
- **C.** Only one transaction at a time
- **D.** Skipping isolation

> **Answer: B.** Readers see a consistent snapshot, so reads and writes rarely block each other.  
> *Revise:* Part 1 (DBMS)

**Q28.** ARIES recovery runs its phases in the order

- **A.** Analysis, Redo, Undo
- **B.** Analysis, Undo, Redo
- **C.** Undo, Redo, Analysis
- **D.** Redo, Analysis, Undo

> **Answer: A.** Analysis finds dirty pages and active transactions, Redo repeats history, Undo rolls back losers.  
> *Revise:* Part 1 (DBMS)

**Q29.** The purpose of a checkpoint is to

- **A.** Increase isolation
- **B.** Limit how much of the log must be replayed after a crash
- **C.** Delete old data
- **D.** Back up the schema

> **Answer: B.** Checkpoints flush state so recovery starts from a recent point.  
> *Revise:* Part 1 (DBMS)

**Q30.** Logical data independence is the ability to

- **A.** Change the network
- **B.** Change the conceptual schema without changing external schemas or applications
- **C.** Change internal storage without affecting the conceptual schema
- **D.** Change hardware

> **Answer: B.** Physical independence concerns the internal level. Logical independence concerns the conceptual level.  
> *Revise:* Part 1 (DBMS)

**Q31.** The three levels of the ANSI/SPARC architecture are

- **A.** Primary, secondary, tertiary
- **B.** Client, server, database
- **C.** Internal, conceptual, external
- **D.** Table, index, view

> **Answer: C.** Internal (physical), conceptual (logical), external (view).  
> *Revise:* Part 1 (DBMS)

**Q32.** In relational algebra, the projection operator (π) selects

- **A.** All pairs of rows
- **B.** Common rows of two relations
- **C.** Rows satisfying a condition
- **D.** Specified columns

> **Answer: D.** σ selects rows, π selects columns.  
> *Revise:* Part 1 (DBMS)

**Q33.** Relation R has 3 rows and 2 columns, S has 4 rows and 3 columns. R × S has

- **A.** 7 rows, 5 columns
- **B.** 12 rows, 5 columns
- **C.** 12 rows, 6 columns
- **D.** 7 rows, 6 columns

> **Answer: B.** Rows multiply (3×4 = 12), columns add (2+3 = 5).  
> *Revise:* Part 1 (DBMS)

**Q34.** The relational algebra division operator is useful for 'for all' queries such as

- **A.** Students who took all courses
- **B.** Students who took at least one course
- **C.** Counting courses
- **D.** Students who took no course

> **Answer: A.** Division expresses universal quantification.  
> *Revise:* Part 1 (DBMS)

**Q35.** A relation has 5 attributes and 100 tuples. Its degree and cardinality are

- **A.** 100 and 5
- **B.** 5 and 100
- **C.** 5 and 5
- **D.** 100 and 100

> **Answer: B.** Degree = number of attributes. Cardinality = number of tuples.  
> *Revise:* Part 1 (DBMS)

**Q36.** Referential integrity ensures that

- **A.** Rows are physically sorted
- **B.** Every foreign key value is NULL or matches an existing referenced key
- **C.** Every table has an index
- **D.** No column contains NULL

> **Answer: B.** It prevents dangling references.  
> *Revise:* Part 1 (DBMS)

**Q37.** ON DELETE CASCADE on a foreign key means

- **A.** Nothing happens to child rows
- **B.** Child rows are set to default
- **C.** Deleting the parent is blocked
- **D.** Deleting a parent row also deletes its matching child rows

> **Answer: D.** Cascade propagates the delete to children.  
> *Revise:* Part 1 (DBMS)

**Q38.** A weak entity

- **A.** Is always stored as a view
- **B.** Has no attributes
- **C.** Always has a 1:1 relationship
- **D.** Cannot be uniquely identified by its own attributes alone and depends on an owner entity

> **Answer: D.** It needs the owner's key plus a partial key.  
> *Revise:* Part 1 (DBMS)

**Q39.** An M:N relationship is mapped to the relational model by

- **A.** Merging the two entities
- **B.** Adding a foreign key on one side
- **C.** Creating a separate relation holding the primary keys of both entities
- **D.** Ignoring it

> **Answer: C.** A junction (associative) table is required.  
> *Revise:* Part 1 (DBMS)

**Q40.** A 1:N relationship is mapped by

- **A.** Placing the PK of the 1 side as a FK on the N side
- **B.** Placing the PK of the N side as a FK on the 1 side
- **C.** Using a composite key on both
- **D.** Always creating a third table

> **Answer: A.** The 'many' side carries the foreign key.  
> *Revise:* Part 1 (DBMS)

**Q41.** A (non-materialized) view is

- **A.** An index
- **B.** A physical copy of a table
- **C.** A virtual table defined by a stored query
- **D.** A trigger

> **Answer: C.** A view stores the query, not the data.  
> *Revise:* Part 1 (DBMS)

**Q42.** A materialized view differs from a normal view because it

- **A.** Stores the result physically and must be refreshed
- **B.** Is always updatable
- **C.** Has no query behind it
- **D.** Cannot be queried

> **Answer: A.** Materialized views trade freshness for speed.  
> *Revise:* Part 1 (DBMS)

**Q43.** A trigger

- **A.** Executes automatically in response to INSERT, UPDATE or DELETE events
- **B.** Replaces all constraints
- **C.** Runs only when called manually
- **D.** Is a kind of index

> **Answer: A.** Triggers are event-driven.  
> *Revise:* Part 1 (DBMS)

**Q44.** Which is typical of OLAP workloads?

- **A.** Complex aggregate queries over large historical data
- **B.** Highly normalized schemas
- **C.** High-frequency single-row updates
- **D.** Many short row-level transactions

> **Answer: A.** OLAP = analytics, usually on a warehouse with star schema.  
> *Revise:* Part 1 (DBMS)

**Q45.** A star schema consists of

- **A.** A single flat table
- **B.** A tree of dimension tables only
- **C.** Many fact tables and no dimensions
- **D.** A central fact table linked to dimension tables

> **Answer: D.** Facts in the middle, dimensions around it.  
> *Revise:* Part 1 (DBMS)


### DBMS advanced: normal forms, joins, triggers, cursors

**Q46.** R(A,B,C) has FDs AB→C and C→B. The candidate keys are AB and AC. R is in

- **A.** 1NF only
- **B.** 2NF but not 3NF
- **C.** BCNF
- **D.** 3NF but not BCNF

> **Answer: D.** C→B violates BCNF (C is not a super key) but B is prime, so 3NF holds.  
> *Revise:* Part 1B (DBMS deep dive)

**Q47.** R(A,B,C,D) has FDs AB→C and C→D. The key is AB. The highest normal form is

- **A.** 3NF
- **B.** 1NF
- **C.** BCNF
- **D.** 2NF

> **Answer: D.** C→D is a transitive dependency (non-key determines non-key), so 3NF fails.  
> *Revise:* Part 1B (DBMS deep dive)

**Q48.** A relation has only two attributes. It is always at least in

- **A.** 4NF only
- **B.** BCNF
- **C.** Only 1NF
- **D.** Only 2NF

> **Answer: B.** With two attributes there cannot be a partial or transitive dependency violating BCNF.  
> *Revise:* Part 1B (DBMS deep dive)

**Q49.** Which decomposition is ALWAYS possible for a relation: lossless AND dependency-preserving?

- **A.** Into 3NF
- **B.** Into 5NF
- **C.** Into 4NF
- **D.** Into BCNF

> **Answer: A.** BCNF decomposition may lose dependency preservation.  
> *Revise:* Part 1B (DBMS deep dive)

**Q50.** `Person(Name, Phone, Hobby)` where phones and hobbies are independent multivalued facts of a person violates

- **A.** BCNF only
- **B.** 4NF
- **C.** 3NF
- **D.** 2NF

> **Answer: B.** Name →→ Phone and Name →→ Hobby are non-trivial multivalued dependencies.  
> *Revise:* Part 1B (DBMS deep dive)

**Q51.** Emp has 4 rows (dept\_id: 10, 20, NULL, 40) and Dept has 3 rows (id: 10, 20, 30). How many rows does `Emp LEFT JOIN Dept ON Emp.dept_id = Dept.id` return?

- **A.** 4
- **B.** 3
- **C.** 2
- **D.** 5

> **Answer: A.** All 4 Emp rows are kept. The NULL and 40 rows get NULLs for Dept.  
> *Revise:* Part 1B (DBMS deep dive)

**Q52.** Using the same tables (Emp dept\_id: 10, 20, NULL, 40; Dept id: 10, 20, 30), how many rows does a FULL OUTER JOIN return?

- **A.** 5
- **B.** 4
- **C.** 3
- **D.** 7

> **Answer: A.** Matches: 10 and 20 (2 rows). Unmatched Emp: NULL and 40 (2 rows). Unmatched Dept: 30 (1 row). Total 5.  
> *Revise:* Part 1B (DBMS deep dive)

**Q53.** Table X has the value 5 twice and table Y has the value 5 three times. `X INNER JOIN Y ON X.v = Y.v` returns how many rows for the value 5?

- **A.** 6
- **B.** 5
- **C.** 2
- **D.** 3

> **Answer: A.** Every matching pair is returned: 2 × 3 = 6.  
> *Revise:* Part 1B (DBMS deep dive)

**Q54.** Which join algorithm builds an in-memory hash table on the smaller input and probes it with the larger input?

- **A.** Cross join
- **B.** Nested loop join
- **C.** Sort-merge join
- **D.** Hash join

> **Answer: D.** Hash join works only for equality conditions.  
> *Revise:* Part 1B (DBMS deep dive)

**Q55.** An index is defined on `(last_name, first_name)`. Which query can use it efficiently?

- **A.** `WHERE first_name LIKE '%a'`
- **B.** `WHERE last_name = 'Rao'`
- **C.** `WHERE first_name = 'Asha'`
- **D.** `WHERE UPPER(last_name) = 'RAO'`

> **Answer: B.** Leftmost-prefix rule. A function on the column also prevents index use.  
> *Revise:* Part 1B (DBMS deep dive)

**Q56.** In InnoDB, a secondary index leaf node stores

- **A.** The full row
- **B.** Nothing
- **C.** The primary key value of the row
- **D.** A physical disk address only

> **Answer: C.** A secondary lookup then needs a second lookup in the clustered index. A large PK enlarges all secondary indexes.  
> *Revise:* Part 1B (DBMS deep dive)

**Q57.** Which index is created only on rows satisfying a condition, such as `WHERE active = true`?

- **A.** Covering index
- **B.** Partial (filtered) index
- **C.** Bitmap index
- **D.** Clustered index

> **Answer: B.** It is smaller and cheaper to maintain.  
> *Revise:* Part 1B (DBMS deep dive)

**Q58.** A database trigger

- **A.** Is called with CALL
- **B.** Executes automatically in response to INSERT, UPDATE or DELETE events and cannot be called explicitly
- **C.** Runs only at midnight
- **D.** Must return a value

> **Answer: B.** Triggers run inside the same transaction as the triggering statement.  
> *Revise:* Part 1B (DBMS deep dive)

**Q59.** In a row-level trigger for an UPDATE, `OLD.salary` and `NEW.salary` refer to

- **A.** Two different tables
- **B.** The first and last rows
- **C.** The value before the update and the value after the update
- **D.** The minimum and maximum

> **Answer: C.** INSERT has only NEW and DELETE has only OLD.  
> *Revise:* Part 1B (DBMS deep dive)

**Q60.** A `BEFORE INSERT` trigger is typically used to

- **A.** Create indexes
- **B.** Roll back the whole database
- **C.** Write an audit log after the commit
- **D.** Validate or modify the incoming NEW values before they are written

> **Answer: D.** AFTER triggers are common for audit logging.  
> *Revise:* Part 1B (DBMS deep dive)

**Q61.** Which is a difference between a function and a stored procedure?

- **A.** They are identical
- **B.** A function must return a value and can be used inside a SELECT; a procedure is invoked with CALL and may return results through OUT parameters
- **C.** Functions cannot take parameters
- **D.** A procedure must return a value

> **Answer: B.** Triggers take no parameters and run automatically.  
> *Revise:* Part 1B (DBMS deep dive)

**Q62.** What is a cursor in SQL?

- **A.** A type of index
- **B.** A transaction log
- **C.** A mouse pointer
- **D.** A database object that lets procedural code process the rows of a query one at a time

> **Answer: D.** Lifecycle: DECLARE, OPEN, FETCH, CLOSE (and DEALLOCATE in SQL Server).  
> *Revise:* Part 1B (DBMS deep dive)

**Q63.** Why is cursor-based row-by-row processing usually discouraged?

- **A.** Cursors are not supported
- **B.** Cursors cannot read data
- **C.** Cursors always cause deadlocks
- **D.** It is slower than a set-based SQL statement because of per-row overhead and locking

> **Answer: D.** Prefer a single UPDATE/INSERT ... SELECT whenever possible.  
> *Revise:* Part 1B (DBMS deep dive)

**Q64.** Which is the correct order of operations for an explicit cursor?

- **A.** OPEN, DECLARE, CLOSE, FETCH
- **B.** FETCH, OPEN, DECLARE, CLOSE
- **C.** DECLARE, FETCH, OPEN, CLOSE
- **D.** DECLARE, OPEN, FETCH, CLOSE

> **Answer: D.** FETCH is repeated in a loop until no more rows remain.  
> *Revise:* Part 1B (DBMS deep dive)

**Q65.** Compared with short polling, long polling

- **A.** Uses UDP
- **B.** Requires no server
- **C.** Sends requests more often
- **D.** Holds the request open until data is available or a timeout occurs, reducing empty responses

> **Answer: D.** WebSocket gives full-duplex communication and SSE gives server-to-client streaming.  
> *Revise:* Part 1B (DBMS deep dive)

**Q66.** Which technique gives two-way, low-latency real-time communication between browser and server?

- **A.** Short polling
- **B.** FTP
- **C.** SMTP
- **D.** WebSocket

> **Answer: D.** SSE is one-way (server to client).  
> *Revise:* Part 1B (DBMS deep dive)

**Q67.** What is the main problem with polling a database table every second for new work?

- **A.** Constant load, delay up to the interval and risk of several workers picking the same row
- **B.** It needs no index
- **C.** It is impossible
- **D.** It deletes the rows

> **Answer: A.** Better: queue/CDC/outbox, or SELECT ... FOR UPDATE SKIP LOCKED for safe job claiming.  
> *Revise:* Part 1B (DBMS deep dive)

**Q68.** A connection pool such as HikariCP improves performance because

- **A.** It encrypts queries
- **B.** It reuses a fixed set of established database connections instead of creating one per request
- **C.** It caches query results
- **D.** It removes the need for indexes

> **Answer: B.** A pool that is too small makes threads wait; too large overloads the DB.  
> *Revise:* Part 1B (DBMS deep dive)

**Q69.** Which is the best description of horizontal partitioning?

- **A.** Splitting a table by rows into partitions (for example by date range)
- **B.** Compressing a table
- **C.** Splitting a table by columns
- **D.** Copying a table to another server

> **Answer: A.** Splitting by columns is vertical partitioning.  
> *Revise:* Part 1B (DBMS deep dive)

**Q70.** Partition pruning means

- **A.** Merging partitions
- **B.** Deleting old partitions automatically
- **C.** The optimizer reads only the partitions that can contain matching rows
- **D.** Compressing partitions

> **Answer: C.** It works when the query filters on the partition key.  
> *Revise:* Part 1B (DBMS deep dive)

**Q71.** The main difference between partitioning and sharding is that

- **A.** They are the same
- **B.** Partitioning splits data inside one database instance, while sharding distributes data across multiple servers
- **C.** Sharding is inside one server
- **D.** Partitioning needs a load balancer

> **Answer: B.** Sharding is horizontal partitioning across machines.  
> *Revise:* Part 1B (DBMS deep dive)

**Q72.** Which is a poor shard key?

- **A.** A UUID
- **B.** user\_id with high cardinality
- **C.** A low-cardinality value such as `country` or a monotonically increasing timestamp, which cause uneven load and hotspots
- **D.** A hash of user\_id

> **Answer: C.** A good shard key has high cardinality, spreads load evenly and matches the main query pattern.  
> *Revise:* Part 1B (DBMS deep dive)

**Q73.** With `hash(key) mod N` sharding, what happens when N changes from 4 to 5?

- **A.** Only 1/5 of the keys move
- **B.** Only new keys are affected
- **C.** No data moves
- **D.** Most keys map to a different shard, so a large amount of data must move

> **Answer: D.** Consistent hashing reduces the movement to about K/N keys.  
> *Revise:* Part 1B (DBMS deep dive)

**Q74.** Which problem becomes hard after sharding?

- **A.** Single-shard queries by key
- **B.** Joins and transactions that span multiple shards
- **C.** Using primary keys
- **D.** Adding indexes

> **Answer: B.** Use denormalization, application-level joins, sagas or avoid cross-shard transactions.  
> *Revise:* Part 1B (DBMS deep dive)

**Q75.** Which is the usual order of scaling steps for a read-heavy database?

- **A.** Optimise queries/indexes, add cache, add read replicas, scale up, partition, then shard
- **B.** Shard first, then index
- **C.** Replicas, then queries, then cache
- **D.** Always shard immediately

> **Answer: A.** Sharding adds the most complexity, so it comes last.  
> *Revise:* Part 1B (DBMS deep dive)

**Q76.** Replication lag in asynchronous leader–follower replication can cause

- **A.** Faster writes only
- **B.** Data corruption always
- **C.** Deadlocks
- **D.** Stale reads and read-your-own-writes anomalies on followers

> **Answer: D.** Read from the leader after a write or use sticky routing.  
> *Revise:* Part 1B (DBMS deep dive)

**Q77.** In a quorum system with N replicas, strong consistency for reads is obtained when

- **A.** W = N and R = 0
- **B.** W + R \< N
- **C.** W = 1 and R = 1 always
- **D.** W + R \> N

> **Answer: D.** The read and write sets must overlap in at least one replica.  
> *Revise:* Part 1B (DBMS deep dive)

**Q78.** Why is replication NOT a substitute for backups?

- **A.** Backups cannot be restored
- **B.** Replication is slower
- **C.** Destructive changes such as an accidental DELETE are replicated to followers immediately
- **D.** Followers cannot store data

> **Answer: C.** Backups give point-in-time recovery.  
> *Revise:* Part 1B (DBMS deep dive)

**Q79.** `SELECT ... FOR UPDATE SKIP LOCKED` is commonly used to

- **A.** Drop locks permanently
- **B.** Speed up SELECT with indexes
- **C.** Create a savepoint
- **D.** Let multiple workers claim different queue rows without blocking each other

> **Answer: D.** Locked rows are skipped instead of waited on.  
> *Revise:* Part 1B (DBMS deep dive)

**Q80.** Optimistic locking with a version column works by

- **A.** Updating with `WHERE id = ? AND version = ?` and failing if no row is updated because someone else changed it
- **B.** Disabling concurrency
- **C.** Using table locks
- **D.** Locking the row for the entire user session

> **Answer: A.** In JPA this is the @Version annotation and OptimisticLockException.  
> *Revise:* Part 1B (DBMS deep dive)

**Q81.** Large `OFFSET` pagination is slow because

- **A.** Indexes cannot be used
- **B.** The database still reads and discards all the skipped rows
- **C.** Offsets are not allowed
- **D.** It locks the table

> **Answer: B.** Keyset pagination (WHERE id \> :last ORDER BY id LIMIT n) avoids this.  
> *Revise:* Part 1B (DBMS deep dive)


## B. Indexing and storage


### Indexing and storage

**Q82.** The default index structure in most relational databases is the

- **A.** Trie
- **B.** B+ tree
- **C.** Hash table
- **D.** Bitmap

> **Answer: B.** B+ trees support equality, range and ordered scans.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q83.** A table can have at most how many clustered indexes?

- **A.** None
- **B.** Two
- **C.** One
- **D.** Any number

> **Answer: C.** Data rows can be physically ordered in only one way.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q84.** A hash index is efficient for

- **A.** Equality lookups
- **B.** Range queries
- **C.** Sorting
- **D.** Prefix LIKE queries

> **Answer: A.** Hashing has no ordering, so only equality works well.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q85.** An index exists on (A, B, C). Which predicate alone CANNOT use it efficiently?

- **A.** WHERE A = 1 AND B = 2 AND C = 3
- **B.** WHERE B = 5
- **C.** WHERE A = 1
- **D.** WHERE A = 1 AND B = 2

> **Answer: B.** Leftmost-prefix rule: the query must start with column A.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q86.** A covering index

- **A.** Is larger than the table
- **B.** Contains every column a query needs so the table need not be visited
- **C.** Exists only on primary keys
- **D.** Exists only on foreign keys

> **Answer: B.** It enables index-only scans.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q87.** A sparse index

- **A.** Cannot be searched
- **B.** Has entries for only some search keys (e.g. one per block) and needs the file sorted
- **C.** Has an entry for every record
- **D.** Is stored only in memory

> **Answer: B.** Dense = entry per record. Sparse = entry per block.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q88.** In a clustered index, the leaf nodes

- **A.** Hold pointers to rows
- **B.** Are always empty
- **C.** Hold hash codes
- **D.** Contain the actual data rows, ordered by the index key

> **Answer: D.** The table itself is the index (e.g. InnoDB primary key).  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q89.** Why are B+ tree leaf nodes linked together?

- **A.** To avoid splits
- **B.** To save memory
- **C.** For efficient range scans
- **D.** To allow hashing

> **Answer: C.** After finding the start key, scan sideways along the leaves.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q90.** A B+ tree with fan-out about 100 indexing 1,000,000 records has roughly how many levels?

- **A.** 1
- **B.** 3
- **C.** 10
- **D.** 100

> **Answer: B.** 100³ = 10⁶, so about 3 levels.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q91.** The main cost of adding many indexes is

- **A.** Slower reads
- **B.** Loss of durability
- **C.** Invalid SQL
- **D.** Slower writes and extra storage

> **Answer: D.** Every INSERT/UPDATE/DELETE must maintain each index.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q92.** A bitmap index is best suited to

- **A.** A low-cardinality column such as status or gender
- **B.** Free text
- **C.** Floating-point measurements
- **D.** A unique ID column

> **Answer: A.** Few distinct values compress well as bitmaps.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q93.** The optimizer may prefer a full table scan over an index when

- **A.** The index is unique
- **B.** The query matches a large fraction of the table
- **C.** The table is empty
- **D.** The query filters on the primary key

> **Answer: B.** Random lookups cost more than a sequential scan when most rows qualify.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q94.** An LSM tree is write-optimized because it

- **A.** Buffers writes in memory and flushes sorted immutable files that are merged by compaction
- **B.** Has no write amplification
- **C.** Updates pages in place
- **D.** Uses only hash lookups

> **Answer: A.** Sequential writes + background compaction (Cassandra, RocksDB).  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q95.** An inverted index maps

- **A.** Row ID to column
- **B.** Terms to the documents that contain them
- **C.** A B-tree upside down
- **D.** Rows to pages

> **Answer: B.** Foundation of full-text search (Elasticsearch, Lucene).  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)

**Q96.** Which predicate on an indexed column hire\_date is NOT sargable (prevents normal index use)?

- **A.** hire\_date \>= '2024-01-01'
- **B.** hire\_date BETWEEN '2024-01-01' AND '2024-12-31'
- **C.** hire\_date = '2024-03-01'
- **D.** YEAR(hire\_date) = 2024

> **Answer: D.** Wrapping the column in a function hides it from the index. Rewrite as a range.  
> *Revise:* Part 1 §1.9 and Part 1B §1B.3 (Indexing)


## C. SQL


### SQL

**Q97.** Which statement about TRUNCATE is correct?

- **A.** It removes the table definition
- **B.** It always fires row-level triggers
- **C.** It supports a WHERE clause
- **D.** It quickly removes all rows and cannot filter rows

> **Answer: D.** TRUNCATE empties the table (often resetting identity). DELETE supports WHERE.  
> *Revise:* Part 2 (SQL)

**Q98.** Which command removes both the table definition and its data?

- **A.** DELETE
- **B.** REMOVE
- **C.** TRUNCATE
- **D.** DROP

> **Answer: D.** DROP TABLE deletes the object itself.  
> *Revise:* Part 2 (SQL)

**Q99.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COUNT(*), COUNT(dept) FROM Emp;
```

What does it return?

- **A.** 5, 5
- **B.** 5, 4
- **C.** 4, 4
- **D.** 4, 5

> **Answer: B.** COUNT(\*) counts rows. COUNT(col) skips NULLs.  
> *Revise:* Part 2 (SQL)

**Q100.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT AVG(salary) FROM Emp;
```

- **A.** 6000
- **B.** 6400
- **C.** 6500
- **D.** 7000

> **Answer: B.** (5000+7000+6000+6000+8000) / 5 = 6400.  
> *Revise:* Part 2 (SQL)

**Q101.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT dept, COUNT(*) FROM Emp GROUP BY dept;
```

How many rows are returned?

- **A.** 2
- **B.** 3
- **C.** 4
- **D.** 5

> **Answer: B.** Groups: 10, 20 and NULL (NULLs form one group).  
> *Revise:* Part 2 (SQL)

**Q102.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COUNT(DISTINCT salary) FROM Emp;
```

- **A.** 3
- **B.** 4
- **C.** 5
- **D.** 2

> **Answer: B.** Distinct salaries: 5000, 6000, 7000, 8000.  
> *Revise:* Part 2 (SQL)

**Q103.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COUNT(*) FROM Emp WHERE dept <> 10;
```

- **A.** 2
- **B.** 3
- **C.** 4
- **D.** 5

> **Answer: A.** Only C and D qualify. The NULL row evaluates to UNKNOWN and is excluded.  
> *Revise:* Part 2 (SQL)

**Q104.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT * FROM Emp WHERE dept = NULL;
```

- **A.** Returns all rows
- **B.** Returns row E
- **C.** Syntax error
- **D.** Returns no rows

> **Answer: D.** Comparison with NULL is UNKNOWN. Use IS NULL.  
> *Revise:* Part 2 (SQL)

**Q105.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT MAX(salary) FROM Emp WHERE salary < (SELECT MAX(salary) FROM Emp);
```

- **A.** 6000
- **B.** 7000
- **C.** 8000
- **D.** 5000

> **Answer: B.** This is the classic second-highest salary query.  
> *Revise:* Part 2 (SQL)

**Q106.** Which statement about WHERE and HAVING is correct?

- **A.** WHERE filters rows before grouping, HAVING filters groups after aggregation
- **B.** WHERE filters groups, HAVING filters rows
- **C.** HAVING cannot use aggregate functions
- **D.** Both are interchangeable

> **Answer: A.** WHERE runs before GROUP BY. HAVING runs after.  
> *Revise:* Part 2 (SQL)

**Q107.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT dept, SUM(salary) FROM Emp GROUP BY dept HAVING SUM(salary) > 11000;
```

How many rows are returned?

- **A.** 0
- **B.** 1
- **C.** 2
- **D.** 3

> **Answer: C.** Dept 10 = 12000, dept 20 = 12000, NULL group = 8000.  
> *Revise:* Part 2 (SQL)

**Q108.** Which clause is logically evaluated FIRST in a SELECT query?

- **A.** FROM
- **B.** ORDER BY
- **C.** `SELECT`
- **D.** WHERE

> **Answer: A.** Logical order: FROM, WHERE, GROUP BY, HAVING, SELECT, ORDER BY, LIMIT.  
> *Revise:* Part 2 (SQL)

**Q109.**

```text
A(id): 1, 2, 3     B(id): 2, 3, 4
```

```sql
SELECT COUNT(*) FROM A FULL OUTER JOIN B ON A.id = B.id;
```

- **A.** 3
- **B.** 4
- **C.** 5
- **D.** 6

> **Answer: B.** Matches: 2, 3. Unmatched: 1 (from A) and 4 (from B). Total 4.  
> *Revise:* Part 2 (SQL)

**Q110.**

```text
A(id): 1, 2, 3     B(id): 2, 3, 4
```

```sql
SELECT COUNT(*) FROM A LEFT JOIN B ON A.id = B.id WHERE B.id IS NULL;
```

- **A.** 0
- **B.** 1
- **C.** 2
- **D.** 3

> **Answer: B.** Only id 1 has no match. This is the anti-join pattern.  
> *Revise:* Part 2 (SQL)

**Q111.** Table A has 3 rows and table B has 3 rows. SELECT COUNT(\*) FROM A CROSS JOIN B;

- **A.** 6
- **B.** 9
- **C.** 3
- **D.** 12

> **Answer: B.** Cartesian product: 3 × 3 = 9.  
> *Revise:* Part 2 (SQL)

**Q112.** Which scenario needs a self join?

- **A.** Counting rows
- **B.** Listing each employee with their manager's name from the same Employee table
- **C.** Removing duplicates
- **D.** Joining two different tables on a primary key

> **Answer: B.** A self join compares rows of a table with other rows of the same table.  
> *Revise:* Part 2 (SQL)

**Q113.** Table X has values (1, 2, 2) and table Y has (2, 3). How many rows does SELECT v FROM X UNION ALL SELECT v FROM Y return?

- **A.** 3
- **B.** 4
- **C.** 5
- **D.** 2

> **Answer: C.** UNION ALL keeps duplicates: 3 + 2 = 5. UNION would return 3.  
> *Revise:* Part 2 (SQL)

**Q114.**

```text
A(id): 1, 2, 3     B(id): 2, 3, NULL
```

```sql
SELECT * FROM A WHERE id NOT IN (SELECT id FROM B);
```

- **A.** All rows of A
- **B.** Error
- **C.** Only id 1
- **D.** No rows

> **Answer: D.** NOT IN with a NULL in the subquery yields UNKNOWN for every non-matching row, so nothing is returned. Prefer NOT EXISTS.  
> *Revise:* Part 2 (SQL)

**Q115.** Which string matches the LIKE pattern 'a\_c%'?

- **A.** ac
- **B.** abd
- **C.** abcd
- **D.** bbc

> **Answer: C.** '\_' matches exactly one character and '%' matches any sequence.  
> *Revise:* Part 2 (SQL)

**Q116.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COUNT(*) FROM Emp WHERE salary BETWEEN 6000 AND 7000;
```

- **A.** 1
- **B.** 2
- **C.** 3
- **D.** 4

> **Answer: C.** BETWEEN is inclusive: B, C, D.  
> *Revise:* Part 2 (SQL)

**Q117.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT name FROM Emp ORDER BY salary DESC LIMIT 1 OFFSET 1;
```

- **A.** A
- **B.** C
- **C.** B
- **D.** E

> **Answer: C.** Sorted desc: E, B, ... Skip 1 row, take 1 = B.  
> *Revise:* Part 2 (SQL)

**Q118.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT DISTINCT dept FROM Emp; How many rows are returned?
```

- **A.** 2
- **B.** 3
- **C.** 4
- **D.** 5

> **Answer: B.** 10, 20 and NULL. DISTINCT treats NULLs as equal.  
> *Revise:* Part 2 (SQL)

**Q119.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COALESCE(dept, 0) FROM Emp WHERE id = 5;
```

- **A.** 5
- **B.** Error
- **C.** 0
- **D.** NULL

> **Answer: C.** COALESCE returns the first non-NULL argument.  
> *Revise:* Part 2 (SQL)

**Q120.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT CASE WHEN salary >= 7000 THEN 'H' ELSE 'L' END FROM Emp;
```

How many rows show 'H'?

- **A.** 1
- **B.** 2
- **C.** 3
- **D.** 4

> **Answer: B.** B (7000) and E (8000).  
> *Revise:* Part 2 (SQL)

**Q121.** Which statement about PRIMARY KEY and UNIQUE is correct?

- **A.** A table can have many primary keys
- **B.** A table can have many UNIQUE constraints but only one PRIMARY KEY
- **C.** A primary key may contain NULL
- **D.** UNIQUE columns can never be NULL in any DBMS

> **Answer: B.** One PK per table, any number of UNIQUE constraints.  
> *Revise:* Part 2 (SQL)

**Q122.** Which of these is a DCL (Data Control Language) command?

- **A.** UPDATE
- **B.** `SELECT`
- **C.** GRANT
- **D.** CREATE

> **Answer: C.** DCL = GRANT and REVOKE.  
> *Revise:* Part 2 (SQL)

**Q123.**

```sql
BEGIN; SAVEPOINT s1; DELETE FROM t; ROLLBACK TO s1;
```

What happens to the DELETE?

- **A.** It is undone and the transaction is still active
- **B.** It is committed
- **C.** It becomes permanent
- **D.** It raises an error

> **Answer: A.** ROLLBACK TO a savepoint undoes work after it without ending the transaction.  
> *Revise:* Part 2 (SQL)

**Q124.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT name, DENSE_RANK() OVER (ORDER BY salary DESC) AS r FROM Emp;
```

What is r for name 'A' (salary 5000)?

- **A.** 3
- **B.** 4
- **C.** 5
- **D.** 2

> **Answer: B.** Salaries 8000, 7000, 6000, 6000, 5000 give dense ranks 1, 2, 3, 3, 4.  
> *Revise:* Part 2 (SQL)

**Q125.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT name, RANK() OVER (ORDER BY salary DESC) AS r FROM Emp;
```

What is r for name 'A' (salary 5000)?

- **A.** 3
- **B.** 4
- **C.** 5
- **D.** 2

> **Answer: C.** RANK leaves gaps after ties: 1, 2, 3, 3, 5.  
> *Revise:* Part 2 (SQL)

**Q126.** A correlated subquery is one that

- **A.** References columns of the outer query and is logically evaluated per outer row
- **B.** Runs only once
- **C.** Cannot use WHERE
- **D.** Always returns one row

> **Answer: A.** It depends on the outer row.  
> *Revise:* Part 2 (SQL)

**Q127.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COUNT(*) FROM Emp WHERE salary > (SELECT AVG(salary) FROM Emp);
```

- **A.** 1
- **B.** 2
- **C.** 3
- **D.** 4

> **Answer: B.** Average is 6400. B and E are above it.  
> *Revise:* Part 2 (SQL)

**Q128.**

```sql
ALTER TABLE Emp ADD age INT; has what effect on existing rows?
```

- **A.** Deletes the rows
- **B.** Adds a column that is NULL for existing rows
- **C.** Sets age to 0
- **D.** Fails if the table has data

> **Answer: B.** New column defaults to NULL unless a DEFAULT is given.  
> *Revise:* Part 2 (SQL)

**Q129.** A query uses WHERE name LIKE '%son'. Regarding a standard B-tree index on name, it

- **A.** Is invalid
- **B.** Generally cannot use the index efficiently because of the leading wildcard
- **C.** Always uses a hash lookup
- **D.** Uses the index efficiently

> **Answer: B.** B-trees need a known prefix. 'son%' could use the index.  
> *Revise:* Part 2 (SQL)

**Q130.** NATURAL JOIN joins two tables on

- **A.** A cross product
- **B.** Columns listed in ON
- **C.** The primary keys only
- **D.** All columns that have the same name in both tables

> **Answer: D.** It matches same-named columns automatically.  
> *Revise:* Part 2 (SQL)

**Q131.**

```sql
SELECT SUBSTRING('database', 1, 4); (1-based indexing) returns
```

- **A.** data
- **B.** base
- **C.** dat
- **D.** atab

> **Answer: A.** Start at position 1, take 4 characters.  
> *Revise:* Part 2 (SQL)

**Q132.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT MAX(salary) - MIN(salary) FROM Emp;
```

- **A.** 2000
- **B.** 3000
- **C.** 4000
- **D.** 8000

> **Answer: B.** 8000 - 5000 = 3000.  
> *Revise:* Part 2 (SQL)

**Q133.**

```sql
UPDATE Emp SET salary = salary + 100; (no WHERE clause)
```

- **A.** Updates all rows
- **B.** Updates only the first row
- **C.** Updates no rows
- **D.** Fails with a syntax error

> **Answer: A.** Without WHERE every row is updated.  
> *Revise:* Part 2 (SQL)

**Q134.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
DELETE FROM Emp WHERE salary = (SELECT MIN(salary) FROM Emp);
```

How many rows are deleted?

- **A.** 0
- **B.** 1
- **C.** 2
- **D.** 5

> **Answer: B.** Only A (5000).  
> *Revise:* Part 2 (SQL)

**Q135.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COUNT(*) FROM Emp WHERE dept = 10 OR dept <> 10;
```

- **A.** 5
- **B.** 4
- **C.** 2
- **D.** 3

> **Answer: B.** For E, NULL OR NULL is UNKNOWN, so it is excluded. The result is 4, not 5.  
> *Revise:* Part 2 (SQL)

**Q136.**

```sql
SELECT 5 + NULL;
```

- **A.** 0
- **B.** NULL
- **C.** 5
- **D.** Error

> **Answer: B.** Any arithmetic with NULL gives NULL.  
> *Revise:* Part 2 (SQL)

**Q137.**

```sql
SELECT CASE WHEN NULL = NULL THEN 'Y' ELSE 'N' END;
```

- **A.** NULL
- **B.** Error
- **C.** N
- **D.** Y

> **Answer: C.** NULL = NULL is UNKNOWN, so the ELSE branch runs.  
> *Revise:* Part 2 (SQL)

**Q138.** Which query finds duplicate emails in Users(id, email)?

- **A.** `SELECT email FROM Users GROUP BY email HAVING COUNT(*) > 1`
- **B.** `SELECT DISTINCT email FROM Users`
- **C.** `SELECT email FROM Users WHERE COUNT(*) > 1`
- **D.** `SELECT email FROM Users ORDER BY email`

> **Answer: A.** Group, then filter groups with HAVING.  
> *Revise:* Part 2 (SQL)

**Q139.** To list customers who have NEVER placed an order, the most reliable pattern is

- **A.** CROSS JOIN Orders
- **B.** LEFT JOIN Orders ... WHERE Orders.id IS NULL
- **C.** INNER JOIN Orders
- **D.** UNION with Orders

> **Answer: B.** Anti-join: keep customers with no matching order.  
> *Revise:* Part 2 (SQL)

**Q140.** Which view is generally NOT updatable?

- **A.** A view selecting columns from one table
- **B.** A view that uses GROUP BY or aggregate functions
- **C.** A view with a simple WHERE
- **D.** A view on a single base table with all NOT NULL columns

> **Answer: B.** Aggregated rows do not map back to single base rows.  
> *Revise:* Part 2 (SQL)

**Q141.**

```sql
BEGIN; INSERT INTO t VALUES (1); ROLLBACK;
```

After this, the row with value 1

- **A.** Exists but is hidden
- **B.** Is in a temp table
- **C.** Does not exist in t
- **D.** Exists in t

> **Answer: C.** ROLLBACK discards the transaction's changes.  
> *Revise:* Part 2 (SQL)

**Q142.** Which SQL Server syntax returns the first 3 rows?

- **A.** `SELECT * FROM t WHERE ROWNUM <= 3`
- **B.** `SELECT TOP 3 * FROM t`
- **C.** `SELECT FIRST 3 * FROM t`
- **D.** `SELECT * FROM t LIMIT 3`

> **Answer: B.** SQL Server uses TOP. MySQL/PostgreSQL use LIMIT. Oracle uses ROWNUM or FETCH FIRST.  
> *Revise:* Part 2 (SQL)

**Q143.** Which approach returns the 3rd highest DISTINCT salary correctly even when salaries tie?

- **A.** ORDER BY salary DESC LIMIT 1 OFFSET 2
- **B.** ORDER BY salary ASC LIMIT 3
- **C.** `SELECT MAX(salary) FROM Emp`
- **D.** Use DENSE\_RANK() OVER (ORDER BY salary DESC) and filter rank = 3

> **Answer: D.** OFFSET works on rows, so ties break it. DENSE\_RANK works on distinct values.  
> *Revise:* Part 2 (SQL)

**Q144.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT * FROM Emp WHERE salary > AVG(salary);
```

- **A.** Invalid: aggregate functions cannot be used directly in WHERE
- **B.** Returns all rows
- **C.** Valid and returns B and E
- **D.** Returns no rows

> **Answer: A.** Use a subquery: salary \> (SELECT AVG(salary) FROM Emp).  
> *Revise:* Part 2 (SQL)

**Q145.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT salary, COUNT(*) FROM Emp GROUP BY salary HAVING COUNT(*) > 1;
```

- **A.** (5000, 1)
- **B.** (6000, 2)
- **C.** (6000, 1)
- **D.** No rows

> **Answer: B.** Only 6000 appears twice.  
> *Revise:* Part 2 (SQL)

**Q146.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT name FROM Emp WHERE dept = (SELECT dept FROM Emp WHERE salary = 6000);
```

- **A.** Runtime error: subquery returns more than one row
- **B.** Returns no rows
- **C.** Returns all rows
- **D.** Returns C and D

> **Answer: A.** The subquery returns two rows (20, 20), so '=' fails. Use IN.  
> *Revise:* Part 2 (SQL)


### SQL tricky patterns

**Q147.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COUNT(*), SUM(salary) FROM Emp WHERE salary > 100000;
```

- **A.** 0 and NULL
- **B.** No rows
- **C.** 0 and 0
- **D.** NULL and NULL

> **Answer: A.** On an empty set COUNT is 0 but SUM is NULL. An aggregate query without GROUP BY always returns one row.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q148.**

```text
Emp(id, name, dept, salary)
(1, 'A', 10, 5000)
(2, 'B', 10, 7000)
(3, 'C', 20, 6000)
(4, 'D', 20, 6000)
(5, 'E', NULL, 8000)
```

```sql
SELECT COUNT(*) FROM Emp HAVING COUNT(*) > 3;
```

- **A.** Returns no rows
- **B.** Returns 3
- **C.** Returns one row with 5
- **D.** Syntax error (HAVING needs GROUP BY)

> **Answer: C.** HAVING without GROUP BY treats the whole table as one group.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q149.** Which statement about ORDER BY and NULLs is TRUE?

- **A.** The placement of NULLs depends on the DBMS (MySQL sorts NULL first in ASC, PostgreSQL and Oracle sort it last)
- **B.** NULLs are excluded
- **C.** NULLs are always last
- **D.** NULLs are always first

> **Answer: A.** Use NULLS FIRST / NULLS LAST where supported.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q150.** `Customer c LEFT JOIN Orders o ON o.cust_id = c.id WHERE o.year = 2024` behaves like

- **A.** An INNER JOIN, because the WHERE filter on the right table removes the NULL-extended rows
- **B.** A CROSS JOIN
- **C.** A FULL JOIN
- **D.** A LEFT JOIN returning all customers

> **Answer: A.** Put the condition in ON to keep all customers.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q151.** Which query finds each employee who earns more than their manager (`Emp(id, name, salary, manager_id)`)?

- **A.** `SELECT name FROM Emp GROUP BY manager_id`
- **B.** `SELECT name FROM Emp WHERE salary > manager_id`
- **C.** `SELECT name FROM Emp CROSS JOIN Emp`
- **D.** `SELECT e.name FROM Emp e JOIN Emp m ON e.manager_id = m.id WHERE e.salary > m.salary`

> **Answer: D.** A self join compares an employee with their manager row.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q152.** To get the top 3 earners in EACH department you would use

- **A.** `GROUP BY dept HAVING 3`
- **B.** `ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC)` and filter rn \<= 3 in an outer query
- **C.** `SELECT TOP 3 dept`
- **D.** `LIMIT 3` once

> **Answer: B.** LIMIT 3 gives only 3 rows overall, not per group.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q153.** Why can't you write `WHERE ROW_NUMBER() OVER (...) = 1` directly?

- **A.** It works fine
- **B.** It needs GROUP BY
- **C.** Window functions are evaluated after WHERE, so they must be filtered in an outer query or CTE
- **D.** ROW\_NUMBER does not exist

> **Answer: C.** Wrap the query in a subquery or CTE.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q154.** A running total of `amount` ordered by `id` is computed using

- **A.** `SUM(amount) GROUP BY id`
- **B.** `TOTAL(amount)`
- **C.** `COUNT(amount) OVER ()`
- **D.** `SUM(amount) OVER (ORDER BY id)`

> **Answer: D.** The default window frame with ORDER BY accumulates from the first row.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q155.** Which statement about `COUNT(DISTINCT col)` is TRUE?

- **A.** It is the same as COUNT(\*)
- **B.** It counts NULLs
- **C.** It needs GROUP BY
- **D.** It ignores NULLs and duplicates

> **Answer: D.** COUNT(\*) counts all rows.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q156.** Which statement is TRUE about DELETE and TRUNCATE?

- **A.** Both fire triggers
- **B.** DELETE can use WHERE and fires row triggers; TRUNCATE removes all rows and cannot use WHERE
- **C.** TRUNCATE can use WHERE
- **D.** DELETE removes the table

> **Answer: B.** TRUNCATE is DDL and typically resets identity counters.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q157.** `SELECT salary FROM Emp ORDER BY salary DESC LIMIT 1 OFFSET 1` may fail to give the 2nd highest salary value when

- **A.** salary is indexed
- **B.** OFFSET is zero
- **C.** The table is empty only
- **D.** The highest salary is shared by two employees (ties)

> **Answer: D.** OFFSET works on rows. Use DISTINCT or DENSE\_RANK.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9

**Q158.** Which query returns departments from `Dept` that have no employees in `Emp`?

- **A.** `SELECT d.* FROM Dept d LEFT JOIN Emp e ON e.dept_id = d.id WHERE e.id IS NULL`
- **B.** `SELECT * FROM Emp WHERE dept_id IS NULL`
- **C.** `SELECT d.* FROM Dept d CROSS JOIN Emp e`
- **D.** `SELECT d.* FROM Dept d INNER JOIN Emp e ON e.dept_id = d.id`

> **Answer: A.** This is the anti-join pattern.  
> *Revise:* Part 2 (SQL) sections 2.3 to 2.9


## D. Java 8


### Java 8

**Q159.** Which of the following is a functional interface?

- **A.** An abstract class with one abstract method
- **B.** An interface with no methods and no annotations
- **C.** An interface with two abstract methods
- **D.** An interface with exactly one abstract method (it may also have default and static methods)

> **Answer: D.** A functional interface has exactly one abstract method. Methods inherited from Object (like equals) do not count.  
> *Revise:* Part 3 (Java 8)

**Q160.** What is the abstract method of `Predicate<T>`?

- **A.** `T get()`
- **B.** `void accept(T t)`
- **C.** `boolean test(T t)`
- **D.** `R apply(T t)`

> **Answer: C.** Predicate tests a condition and returns boolean.  
> *Revise:* Part 3 (Java 8)

**Q161.** What is the abstract method of `Supplier<T>`?

- **A.** `void accept(T t)`
- **B.** `boolean test(T t)`
- **C.** `T get()`
- **D.** `R apply(T t)`

> **Answer: C.** Supplier takes no argument and produces a value.  
> *Revise:* Part 3 (Java 8)

**Q162.**

```java
Function<Integer,Integer> f = x -> x * 2;
Function<Integer,Integer> g = x -> x + 3;
System.out.println(f.andThen(g).apply(5));
```

What is printed?

- **A.** 13
- **B.** 16
- **C.** 10
- **D.** 8

> **Answer: A.** andThen runs f first (10) then g (13). compose would run g first: (5+3)\*2 = 16.  
> *Revise:* Part 3 (Java 8)

**Q163.** With `f = x -> x * 2` and `g = x -> x + 3`, what does `f.compose(g).apply(5)` return?

- **A.** 16
- **B.** 13
- **C.** 10
- **D.** 25

> **Answer: A.** compose applies g first: 5+3 = 8, then f: 16.  
> *Revise:* Part 3 (Java 8)

**Q164.** `String::length` is which kind of method reference?

- **A.** Unbound instance method reference (equivalent to `s -> s.length()`)
- **B.** Bound instance method reference
- **C.** Static method reference
- **D.** Constructor reference

> **Answer: A.** The first lambda parameter becomes the receiver of the call.  
> *Revise:* Part 3 (Java 8)

**Q165.** Which expression is a constructor reference that creates a new list?

- **A.** `new::ArrayList`
- **B.** `ArrayList::new`
- **C.** `ArrayList::create`
- **D.** `ArrayList.new()`

> **Answer: B.** Constructor references use ClassName::new.  
> *Revise:* Part 3 (Java 8)

**Q166.**

```java
int count = 0;
Runnable r = () -> count++;
```

What happens?

- **A.** count becomes 1 after r.run() only
- **B.** Compile-time error: local variables used in a lambda must be effectively final
- **C.** Runtime exception
- **D.** Runs and increments count

> **Answer: B.** A lambda cannot modify a captured local variable. Use an AtomicInteger or a field.  
> *Revise:* Part 3 (Java 8)

**Q167.** Inside a lambda expression, the keyword `this` refers to

- **A.** The enclosing class instance
- **B.** The lambda object itself
- **C.** The thread running the lambda
- **D.** The functional interface

> **Answer: A.** Unlike an anonymous inner class, a lambda does not introduce a new `this`.  
> *Revise:* Part 3 (Java 8)

**Q168.**

```java
Stream.of(1, 2, 3).map(x -> { System.out.print(x); return x; });
```

What is printed?

- **A.** Nothing
- **B.** 123
- **C.** 321
- **D.** Compile error

> **Answer: A.** Streams are lazy. With no terminal operation the pipeline never runs.  
> *Revise:* Part 3 (Java 8)

**Q169.**

```java
Stream<Integer> s = Stream.of(1, 2, 3);
s.forEach(System.out::println);
s.forEach(System.out::println);
```

What happens on the second forEach?

- **A.** IllegalStateException: stream has already been operated upon or closed
- **B.** NullPointerException
- **C.** It prints nothing
- **D.** It prints 1 2 3 again

> **Answer: A.** A stream can be consumed only once.  
> *Revise:* Part 3 (Java 8)

**Q170.**

```java
List<Integer> l = Arrays.asList(1,2,3,4,5,6);
System.out.println(l.stream().filter(x -> x % 2 == 0).map(x -> x * x).reduce(0, Integer::sum));
```

Output?

- **A.** 56
- **B.** 12
- **C.** 91
- **D.** 20

> **Answer: A.** Evens: 2,4,6. Squares: 4,16,36. Sum = 56.  
> *Revise:* Part 3 (Java 8)

**Q171.** Given `List<List<Integer>> nested = [[1,2],[3],[4,5]]`, what does `nested.stream().flatMap(List::stream).count()` return?

- **A.** 5
- **B.** 3
- **C.** 2
- **D.** 15

> **Answer: A.** flatMap flattens all inner lists into one stream of 5 elements. With map you would get 3 streams.  
> *Revise:* Part 3 (Java 8)

**Q172.** `Collectors.toMap(keyFn, valueFn)` throws which exception when two elements produce the same key?

- **A.** IllegalArgumentException
- **B.** NullPointerException
- **C.** IllegalStateException
- **D.** DuplicateKeyException

> **Answer: C.** Provide a merge function as the third argument to resolve duplicates.  
> *Revise:* Part 3 (Java 8)

**Q173.** What is the type of the result of `words.stream().collect(Collectors.groupingBy(String::length, Collectors.counting()))`?

- **A.** `Map<Integer, Long>`
- **B.** `Map<Integer, Integer>`
- **C.** `Map<Long, Integer>`
- **D.** `Map<String, Long>`

> **Answer: A.** counting() returns Long.  
> *Revise:* Part 3 (Java 8)

**Q174.** What does `Collectors.partitioningBy(predicate)` return?

- **A.** A `Map<Boolean, List<T>>` containing only keys that have elements
- **B.** A `Set<Boolean>`
- **C.** `Map<Boolean, List<T>>` that always contains both keys true and false
- **D.** A `List<List<T>>`

> **Answer: C.** Both keys are present even when one list is empty. groupingBy only creates keys that occur.  
> *Revise:* Part 3 (Java 8)

**Q175.** `IntStream.range(1, 5).sum()` returns

- **A.** 10
- **B.** 15
- **C.** 9
- **D.** 14

> **Answer: A.** range excludes the end: 1+2+3+4 = 10. rangeClosed(1,5) gives 15.  
> *Revise:* Part 3 (Java 8)

**Q176.** What happens when you call `Optional.of(null)`?

- **A.** NullPointerException
- **B.** Returns Optional.empty()
- **C.** Returns Optional containing null
- **D.** IllegalArgumentException

> **Answer: A.** Use Optional.ofNullable for values that may be null.  
> *Revise:* Part 3 (Java 8)

**Q177.** What is the difference between `orElse(x)` and `orElseGet(supplier)`?

- **A.** `orElse` is lazy and `orElseGet` is eager
- **B.** They are identical
- **C.** `orElse` always evaluates its argument. `orElseGet` calls the supplier only when the Optional is empty
- **D.** `orElseGet` throws an exception when empty

> **Answer: C.** Prefer orElseGet when computing the default is expensive or has side effects.  
> *Revise:* Part 3 (Java 8)

**Q178.** Which method is NOT available on `Optional` in Java 8?

- **A.** `isPresent()`
- **B.** `orElseThrow(Supplier)`
- **C.** `ifPresentOrElse()`
- **D.** `ifPresent()`

> **Answer: C.** ifPresentOrElse was added in Java 9 (and isEmpty in Java 11).  
> *Revise:* Part 3 (Java 8)

**Q179.** A class implements two interfaces that both declare a `default void hello()`. What must the class do?

- **A.** Nothing, the last interface wins
- **B.** Override `hello()` (it may call `A.super.hello()`), otherwise compile error
- **C.** Nothing, the first interface wins
- **D.** Declare the class abstract only

> **Answer: B.** This is the diamond problem. It must be resolved explicitly.  
> *Revise:* Part 3 (Java 8)

**Q180.** A class defines a concrete method `show()` and implements an interface that has a default `show()`. Which is used?

- **A.** Compile error
- **B.** The class's method (class wins)
- **C.** Both are executed
- **D.** The interface default

> **Answer: B.** The class always wins over interface defaults.  
> *Revise:* Part 3 (Java 8)

**Q181.** How is a static method of an interface called?

- **A.** `obj.method()`
- **B.** `super.method()`
- **C.** `ImplementingClass.method()`
- **D.** `InterfaceName.method()` only. It is not inherited by implementing classes

> **Answer: D.** Static interface methods belong to the interface itself.  
> *Revise:* Part 3 (Java 8)

**Q182.** In Java 8 HashMap, when does a bucket's linked list turn into a red-black tree?

- **A.** When the bucket holds 8 or more nodes and the table capacity is at least 64
- **B.** When the map has more than 12 entries
- **C.** When the bucket holds 2 nodes
- **D.** When the load factor is 1.0

> **Answer: A.** TREEIFY\_THRESHOLD = 8 and MIN\_TREEIFY\_CAPACITY = 64. If capacity is smaller, the table resizes instead.  
> *Revise:* Part 3 (Java 8)

**Q183.** The default initial capacity and load factor of HashMap are

- **A.** 16 and 0.75
- **B.** 10 and 0.75
- **C.** 16 and 1.0
- **D.** 32 and 0.5

> **Answer: A.** Resize happens when size exceeds capacity × load factor (16 × 0.75 = 12).  
> *Revise:* Part 3 (Java 8)

**Q184.** Which statement about null handling is correct?

- **A.** HashMap allows no null
- **B.** Hashtable allows null values
- **C.** HashMap allows one null key, but Hashtable and ConcurrentHashMap do not allow null keys or values
- **D.** ConcurrentHashMap allows null keys

> **Answer: C.** HashMap: one null key, many null values.  
> *Revise:* Part 3 (Java 8)

**Q185.** How does ConcurrentHashMap in Java 8 achieve thread safety?

- **A.** CAS operations plus synchronized on the first node of a bin (no segments)
- **B.** Segment locks with 16 segments
- **C.** Copying the whole map on each write
- **D.** A single global lock

> **Answer: A.** The segment design was Java 7.  
> *Revise:* Part 3 (Java 8)

**Q186.**

```java
System.out.println(Stream.iterate(1, x -> x * 2).limit(5).collect(Collectors.toList()));
```

Output?

- **A.** [1, 2, 3, 4, 5]
- **B.** [2, 4, 8, 16, 32]
- **C.** Infinite loop
- **D.** [1, 2, 4, 8, 16]

> **Answer: D.** iterate creates an infinite stream, limit(5) stops it.  
> *Revise:* Part 3 (Java 8)

**Q187.** What is `peek()` mainly intended for?

- **A.** Debugging: inspecting elements as they flow through the pipeline
- **B.** Collecting results
- **C.** Terminating a stream
- **D.** Sorting

> **Answer: A.** peek is an intermediate operation. Do not rely on it for business logic.  
> *Revise:* Part 3 (Java 8)

**Q188.** Which method guarantees encounter order when iterating a parallel stream?

- **A.** `forEachOrdered`
- **B.** `peek`
- **C.** `forEach`
- **D.** `parallelOrder`

> **Answer: A.** forEach on a parallel stream is unordered.  
> *Revise:* Part 3 (Java 8)

**Q189.** `Stream.of("a","b","c").collect(Collectors.joining(", ", "[", "]"))` returns

- **A.** `[a, b, c]`
- **B.** `a, b, c`
- **C.** `[a,b,c]`
- **D.** `[a, b, c,]`

> **Answer: A.** joining(delimiter, prefix, suffix).  
> *Revise:* Part 3 (Java 8)

**Q190.** Which comparator sorts employees by department ascending and then by salary descending?

- **A.** `Comparator.comparing(Emp::getSalary).thenComparing(Emp::getDept)`
- **B.** `Comparator.comparing(Emp::getDept).thenComparing(Emp::getSalary).reversed()`
- **C.** `Comparator.comparing(Emp::getDept).reversed().thenComparing(Emp::getSalary)`
- **D.** `Comparator.comparing(Emp::getDept).thenComparing(Emp::getSalary, Comparator.reverseOrder())`

> **Answer: D.** reversed() applied after thenComparing reverses the whole chain, so department would also be descending.  
> *Revise:* Part 3 (Java 8)

**Q191.** What does `LocalDate.of(2024, 2, 29).plusYears(1)` return?

- **A.** 2025-02-28
- **B.** 2025-03-01
- **C.** Exception
- **D.** 2025-02-29

> **Answer: A.** The day is clamped to the last valid day of the month.  
> *Revise:* Part 3 (Java 8)

**Q192.** Why are the `java.time` classes preferred over `Date` and `SimpleDateFormat`?

- **A.** They are immutable and thread-safe
- **B.** They use 0-based months
- **C.** They only support UTC
- **D.** They are mutable and faster

> **Answer: A.** Old Date/Calendar/SimpleDateFormat are mutable and not thread-safe. java.time months are 1 to 12.  
> *Revise:* Part 3 (Java 8)

**Q193.** `Period` and `Duration` differ in that

- **A.** They are the same
- **B.** Duration works only with LocalDate
- **C.** Period is time-based and Duration is date-based
- **D.** Period is date-based (years, months, days) and Duration is time-based (hours, minutes, seconds)

> **Answer: D.** Use Period between two LocalDates and Duration between two times or Instants.  
> *Revise:* Part 3 (Java 8)

**Q194.** When should you use `thenCompose` instead of `thenApply` on a CompletableFuture?

- **A.** When the function itself returns a CompletableFuture and you want to flatten it
- **B.** When you want to combine two futures
- **C.** When handling exceptions
- **D.** When the function returns a plain value

> **Answer: A.** thenCompose is like flatMap. thenCombine joins two independent futures.  
> *Revise:* Part 3 (Java 8)

**Q195.**

```java
Stream.of(new Object(), new Object()).sorted().collect(Collectors.toList());
```

What happens?

- **A.** Returns the same order
- **B.** ClassCastException at runtime (elements are not Comparable)
- **C.** Sorted by hash code
- **D.** Compile error

> **Answer: B.** sorted() without a comparator needs Comparable elements.  
> *Revise:* Part 3 (Java 8)

**Q196.** `Arrays.asList(1, 2, 3).add(4)` throws

- **A.** NullPointerException
- **B.** Nothing, it adds 4
- **C.** IndexOutOfBoundsException
- **D.** UnsupportedOperationException

> **Answer: D.** Arrays.asList returns a fixed-size list backed by the array. set() works, add/remove do not.  
> *Revise:* Part 3 (Java 8)

**Q197.** What does `Stream.of(1,2,3,4).skip(1).limit(2).count()` return?

- **A.** 2
- **B.** 3
- **C.** 1
- **D.** 4

> **Answer: A.** After skipping 1: 2,3,4. limit(2) keeps 2 elements.  
> *Revise:* Part 3 (Java 8)

**Q198.** Which call is the idiomatic way to count word occurrences in a `Map<String,Integer>`?

- **A.** `map.add(word)`
- **B.** `map.increment(word)`
- **C.** `map.merge(word, 1, Integer::sum)`
- **D.** `map.put(word, map.get(word) + 1)`

> **Answer: C.** merge inserts 1 when absent, otherwise applies the function. put with get fails with NPE for absent keys.  
> *Revise:* Part 3 (Java 8)

**Q199.** Which of these is NOT a short-circuiting stream operation?

- **A.** `limit`
- **B.** `findFirst`
- **C.** `sorted`
- **D.** `anyMatch`

> **Answer: C.** sorted is a stateful operation that must see all elements.  
> *Revise:* Part 3 (Java 8)

**Q200.** `IntStream.empty().sum()` returns

- **A.** NaN
- **B.** OptionalInt.empty
- **C.** NullPointerException
- **D.** 0

> **Answer: D.** sum of an empty IntStream is 0, but max() and average() return empty Optionals.  
> *Revise:* Part 3 (Java 8)

**Q201.** What is the purpose of the `@FunctionalInterface` annotation?

- **A.** It is required for lambdas to work
- **B.** It makes the interface faster
- **C.** It generates the implementation
- **D.** Compile-time check that the interface has exactly one abstract method

> **Answer: D.** Lambdas work with any single-abstract-method interface even without the annotation.  
> *Revise:* Part 3 (Java 8)

**Q202.**

```java
Map<Integer,String> m = Stream.of("a","bb","ccc").collect(Collectors.toMap(String::length, s -> s));
System.out.println(m.get(2));
```

Output?

- **A.** ccc
- **B.** a
- **C.** bb
- **D.** null

> **Answer: C.** Key = length, value = the string.  
> *Revise:* Part 3 (Java 8)

**Q203.** `distinct()` on a stream of custom objects removes duplicates based on

- **A.** The `toString()` output
- **B.** Field order
- **C.** `equals()` and `hashCode()`
- **D.** Reference equality only

> **Answer: C.** If you do not override equals/hashCode, distinct will not remove logically equal objects.  
> *Revise:* Part 3 (Java 8)

**Q204.** Which Java 8 change replaced PermGen?

- **A.** Heap generation
- **B.** Code cache
- **C.** Metaspace (native memory)
- **D.** Stack memory

> **Answer: C.** Class metadata moved from PermGen to Metaspace.  
> *Revise:* Part 3 (Java 8)

**Q205.**

```java
List<String> l = Arrays.asList("b","a","c");
l.stream().sorted();
System.out.println(l);
```

Output?

- **A.** [b, a, c]
- **B.** Compile error
- **C.** [c, b, a]
- **D.** [a, b, c]

> **Answer: A.** No terminal operation ran, and streams never modify their source anyway.  
> *Revise:* Part 3 (Java 8)

**Q206.** Which functional interface represents an operation that takes two arguments of the same type and returns the same type?

- **A.** `UnaryOperator<T>`
- **B.** `Supplier<T>`
- **C.** `BiConsumer<T,T>`
- **D.** `BinaryOperator<T>`

> **Answer: D.** BinaryOperator\<T\> extends BiFunction\<T,T,T\>.  
> *Revise:* Part 3 (Java 8)

**Q207.** `stream.map(String::toUpperCase)` and `stream.flatMap(...)` differ because

- **A.** map is one-to-one and flatMap maps each element to a stream and flattens the results
- **B.** flatMap is always faster
- **C.** map can only handle Strings
- **D.** map is terminal and flatMap is intermediate

> **Answer: A.** flatMap is one-to-many.  
> *Revise:* Part 3 (Java 8)


## E. Spring Boot and configuration


### Spring Boot

**Q208.** `@SpringBootApplication` is a combination of which three annotations?

- **A.** `@Controller`, `@RestController`, `@RequestMapping`
- **B.** `@Configuration`, `@Bean`, `@Autowired`
- **C.** `@Component`, `@Service`, `@Repository`
- **D.** `@SpringBootConfiguration`, `@EnableAutoConfiguration`, `@ComponentScan`

> **Answer: D.** It marks a configuration class, enables auto-configuration and scans components in the package and sub-packages.  
> *Revise:* Part 4 (Spring Boot)

**Q209.** In Spring Boot 3, auto-configuration classes are registered in which file?

- **A.** `META-INF/spring.factories` only
- **B.** `application.properties`
- **C.** `pom.xml`
- **D.** `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`

> **Answer: D.** spring.factories was used for auto-configuration in older versions.  
> *Revise:* Part 4 (Spring Boot)

**Q210.** `@ConditionalOnMissingBean` on an auto-configured bean means

- **A.** The bean is removed at runtime
- **B.** The auto-configuration backs off if the application already defines a bean of that type
- **C.** The bean is created only when another bean is missing in the test
- **D.** The bean is always created

> **Answer: B.** This is how you override Boot defaults by defining your own bean.  
> *Revise:* Part 4 (Spring Boot)

**Q211.** What are the default embedded server and port of a Spring Boot web application?

- **A.** Tomcat on port 8080
- **B.** Netty on port 9090
- **C.** Undertow on port 80
- **D.** Jetty on port 8000

> **Answer: A.** Change with server.port.  
> *Revise:* Part 4 (Spring Boot)

**Q212.** Which statement about the default bean scope is correct?

- **A.** Singleton: one instance per Spring container (per bean definition), not per JVM
- **B.** Request: one instance per application
- **C.** Prototype: one instance per container
- **D.** Singleton: one instance per JVM

> **Answer: A.** Two ApplicationContexts in the same JVM have separate singletons.  
> *Revise:* Part 4 (Spring Boot)

**Q213.** A prototype-scoped bean is injected into a singleton bean using `@Autowired`. How many prototype instances will the singleton use?

- **A.** One, because injection happens once when the singleton is created
- **B.** A new one for each method call
- **C.** One per thread
- **D.** One per HTTP request

> **Answer: A.** Use ObjectProvider, @Lookup or a scoped proxy to get a fresh prototype each time.  
> *Revise:* Part 4 (Spring Boot)

**Q214.** `@RestController` is equivalent to

- **A.** `@Service` + `@ResponseBody`
- **B.** `@Controller` + `@ResponseBody`
- **C.** `@Component` + `@RequestMapping`
- **D.** `@Controller` + `@ResponseStatus`

> **Answer: B.** Return values are written directly to the response body (JSON by default).  
> *Revise:* Part 4 (Spring Boot)

**Q215.** What extra behaviour does `@Repository` provide compared to `@Component`?

- **A.** Automatic transactions
- **B.** Lazy loading
- **C.** Caching
- **D.** Translation of persistence exceptions into Spring's DataAccessException hierarchy

> **Answer: D.** Otherwise it is functionally a stereotype for the DAO layer.  
> *Revise:* Part 4 (Spring Boot)

**Q216.** Why is constructor injection preferred over field injection?

- **A.** It is faster at runtime
- **B.** Field injection is not supported
- **C.** It allows final fields, makes required dependencies explicit and makes testing without Spring easy
- **D.** It allows circular dependencies

> **Answer: C.** Constructor injection also fails fast on circular dependencies.  
> *Revise:* Part 4 (Spring Boot)

**Q217.** Two beans of type `PaymentService` exist and a class injects `PaymentService` without any qualifier. What happens?

- **A.** A random bean is chosen
- **B.** The last bean is chosen
- **C.** NoUniqueBeanDefinitionException
- **D.** The first bean is chosen

> **Answer: C.** Resolve with @Primary or @Qualifier.  
> *Revise:* Part 4 (Spring Boot)

**Q218.** What does `@Primary` do?

- **A.** Makes the bean load first
- **B.** Marks a bean as the default choice when several candidates of the same type exist
- **C.** Makes the bean a singleton
- **D.** Disables other beans

> **Answer: B.** @Qualifier at the injection point overrides @Primary.  
> *Revise:* Part 4 (Spring Boot)

**Q219.** Beans A and B inject each other through their constructors. What happens at startup?

- **A.** Both beans become prototype
- **B.** BeanCurrentlyInCreationException (circular dependency)
- **C.** A is null
- **D.** Spring resolves it silently

> **Answer: B.** Since Boot 2.6, circular references are prohibited by default even for field/setter injection.  
> *Revise:* Part 4 (Spring Boot)

**Q220.** A `@Transactional` method is called from another method of the \*\*same class\*\* (`this.save()`). What is the effect?

- **A.** An exception is thrown
- **B.** A new transaction always starts
- **C.** The call bypasses the Spring proxy, so no transactional behaviour is applied
- **D.** The transaction works normally

> **Answer: C.** Spring AOP is proxy-based, so self-invocation does not go through the proxy.  
> *Revise:* Part 4 (Spring Boot)

**Q221.** By default, `@Transactional` rolls back on

- **A.** All exceptions including checked
- **B.** Never
- **C.** Unchecked exceptions (RuntimeException and Error) only
- **D.** Only checked exceptions

> **Answer: C.** Use rollbackFor = Exception.class to roll back for checked exceptions.  
> *Revise:* Part 4 (Spring Boot)

**Q222.** What is the default propagation of `@Transactional`?

- **A.** REQUIRES\_NEW
- **B.** SUPPORTS
- **C.** REQUIRED
- **D.** MANDATORY

> **Answer: C.** REQUIRED joins an existing transaction or creates one.  
> *Revise:* Part 4 (Spring Boot)

**Q223.** What is the N+1 select problem in JPA?

- **A.** Transactions nested N+1 times
- **B.** N queries are run for 1 parent
- **C.** An index missing on N columns
- **D.** One query loads N parents and then N additional queries load each parent's lazy children

> **Answer: D.** Fix with JOIN FETCH, @EntityGraph or batch fetching.  
> *Revise:* Part 4 (Spring Boot)

**Q224.** `LazyInitializationException` occurs when

- **A.** A lazy association is accessed after the Hibernate session/transaction is closed
- **B.** An entity has no id
- **C.** The database is down
- **D.** Two sessions update the same row

> **Answer: A.** Fetch the data inside the transaction or use a fetch join/DTO projection.  
> *Revise:* Part 4 (Spring Boot)

**Q225.** `spring.jpa.hibernate.ddl-auto=validate` will

- **A.** Drop and recreate the schema
- **B.** Update columns automatically
- **C.** Check that the schema matches the entities and fail on mismatch, without changing the schema
- **D.** Create tables automatically

> **Answer: C.** Recommended for production together with migration tools such as Flyway or Liquibase.  
> *Revise:* Part 4 (Spring Boot)

**Q226.** A request body fails Bean Validation (`@Valid @RequestBody`). Which exception is thrown by default and which status is returned?

- **A.** HttpMessageNotReadableException and HTTP 404
- **B.** NullPointerException and HTTP 500
- **C.** ConstraintViolationException and HTTP 500
- **D.** MethodArgumentNotValidException and HTTP 400

> **Answer: D.** Handle it in a @RestControllerAdvice for a custom error body.  
> *Revise:* Part 4 (Spring Boot)

**Q227.** Which annotations implement global exception handling for controllers?

- **A.** `@Configuration` with `@Bean`
- **B.** `@Aspect` with `@Around`
- **C.** `@ControllerAdvice` (or `@RestControllerAdvice`) with `@ExceptionHandler`
- **D.** `@Transactional` with `@Rollback`

> **Answer: C.** The advice class applies to all controllers.  
> *Revise:* Part 4 (Spring Boot)

**Q228.** In Spring MVC, which component receives every HTTP request first?

- **A.** HandlerMapping
- **B.** DispatcherServlet (front controller)
- **C.** Controller
- **D.** ViewResolver

> **Answer: B.** It delegates to HandlerMapping, HandlerAdapter and then the controller.  
> *Revise:* Part 4 (Spring Boot)

**Q229.** What is the difference between `@PathVariable` and `@RequestParam`?

- **A.** They are identical
- **B.** `@PathVariable` reads from the URL path (`/users/5`); `@RequestParam` reads query parameters (`/users?id=5`)
- **C.** `@RequestParam` reads headers
- **D.** `@PathVariable` reads the body

> **Answer: B.** @RequestBody reads the HTTP body and @RequestHeader reads headers.  
> *Revise:* Part 4 (Spring Boot)

**Q230.** Which endpoint reports application health in Spring Boot Actuator?

- **A.** `/status`
- **B.** `/actuator/health`
- **C.** `/health/actuator`
- **D.** `/actuator/ping`

> **Answer: B.** By default only health (and info in some versions) is exposed over HTTP.  
> *Revise:* Part 4 (Spring Boot)

**Q231.** When does a `CommandLineRunner` bean run?

- **A.** Before the context starts
- **B.** After the application context is loaded, just before `SpringApplication.run()` completes
- **C.** On every HTTP request
- **D.** Only in tests

> **Answer: B.** Useful for startup tasks such as seeding data.  
> *Revise:* Part 4 (Spring Boot)

**Q232.** What does `JpaRepository.findById(id)` return?

- **A.** `List<T>`
- **B.** `T` or throws always
- **C.** `Optional<T>`
- **D.** `T` or null

> **Answer: C.** Use orElseThrow to convert to an exception.  
> *Revise:* Part 4 (Spring Boot)

**Q233.** Spring Boot 3 requires at least which Java version, and which package namespace replaced `javax.*`?

- **A.** Java 8 and `javax.*`
- **B.** Java 11 and `jakarta.*`
- **C.** Java 17 and `jakarta.*`
- **D.** Java 21 and `java.*`

> **Answer: C.** Spring Framework 6 and Boot 3 moved to Jakarta EE 9+.  
> *Revise:* Part 4 (Spring Boot)

**Q234.** Which order is correct for a Spring bean's initialisation?

- **A.** Injection, constructor, `@PreDestroy`
- **B.** Constructor, `@PreDestroy`, injection
- **C.** `@PostConstruct`, constructor, injection
- **D.** Constructor, dependency injection, `@PostConstruct`, bean ready

> **Answer: D.** @PreDestroy runs at shutdown for singleton beans.  
> *Revise:* Part 4 (Spring Boot)

**Q235.** Inside a `@Configuration` class, one `@Bean` method calls another `@Bean` method. Each call returns

- **A.** A prototype
- **B.** null
- **C.** The same singleton (the configuration class is CGLIB-proxied)
- **D.** A new object every time

> **Answer: C.** In a plain @Component ('lite mode') it would create a new instance per call.  
> *Revise:* Part 4 (Spring Boot)

**Q236.** What is the purpose of Spring Boot starters such as `spring-boot-starter-web`?

- **A.** They bundle a curated set of compatible dependencies for a feature
- **B.** They generate code
- **C.** They replace the JDK
- **D.** They start the server automatically only

> **Answer: A.** Version management comes from the Boot BOM.  
> *Revise:* Part 4 (Spring Boot)

**Q237.** Which statement about PUT and PATCH is correct?

- **A.** PUT replaces the whole resource and is idempotent; PATCH applies a partial update
- **B.** Neither is idempotent
- **C.** Both create a new resource
- **D.** PUT is partial and PATCH replaces

> **Answer: A.** POST is the common non-idempotent create.  
> *Revise:* Part 4 (Spring Boot)

**Q238.** In `findByLastNameAndAgeGreaterThan(String lastName, int age)`, Spring Data JPA

- **A.** Requires a @Query annotation
- **B.** Is invalid
- **C.** Derives the query from the method name
- **D.** Runs a native query only

> **Answer: C.** Derived query keywords include And, Or, GreaterThan, Like, OrderBy.  
> *Revise:* Part 4 (Spring Boot)

**Q239.** Which proxy mechanism does Spring Boot use by default for AOP?

- **A.** AspectJ weaving only
- **B.** CGLIB (class-based proxies)
- **C.** JDK dynamic proxies only
- **D.** No proxies

> **Answer: B.** spring.aop.proxy-target-class defaults to true in Boot.  
> *Revise:* Part 4 (Spring Boot)

**Q240.** A request to a secured endpoint without any credentials returns which status? And an authenticated user without the required role?

- **A.** 401 Unauthorized and 403 Forbidden
- **B.** 403 and 401
- **C.** 400 and 404
- **D.** 500 and 500

> **Answer: A.** 401 = not authenticated, 403 = authenticated but not authorised.  
> *Revise:* Part 4 (Spring Boot)


### Spring Boot configuration

**Q241.** If both `application.properties` and `application.yml` exist in the same location, which takes precedence?

- **A.** They are merged and yml wins
- **B.** Whichever is alphabetically first
- **C.** `application.yml`
- **D.** `application.properties`

> **Answer: D.** Properties are loaded last, so they override yml.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q242.** How do you activate the `dev` profile?

- **A.** `spring.profiles.active=dev` (property, env var or `--spring.profiles.active=dev`) with `application-dev.properties`
- **B.** `server.profile=dev`
- **C.** `spring.dev=true`
- **D.** `@Profile=dev` in the pom

> **Answer: A.** Profile-specific files override values from the default file.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q243.** `application.properties` has `server.port=8081` and you start the app with `--server.port=9090`. Which port is used?

- **A.** 8080
- **B.** Startup error
- **C.** 9090
- **D.** 8081

> **Answer: C.** Command-line arguments have higher precedence than property files.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q244.** Which environment variable overrides `spring.datasource.url`?

- **A.** `SPRING_DATASOURCE_URL`
- **B.** `SPRING-DATASOURCE-URL`
- **C.** `spring.datasource.url`
- **D.** `DATASOURCE_URL_SPRING`

> **Answer: A.** Upper-case and replace dots and hyphens with underscores.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q245.** `@Value("${app.timeout:30}")` injects

- **A.** Always 30
- **B.** The value of a bean named app
- **C.** The property `app.timeout`, or 30 if it is not defined
- **D.** The text `${app.timeout:30}`

> **Answer: C.** The part after the colon is the default.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q246.** `@Value("${app.name}")` is used but `app.name` is not defined anywhere and has no default. What happens?

- **A.** Only a warning is logged
- **B.** The field is null
- **C.** Application fails to start (could not resolve placeholder)
- **D.** The field is an empty string

> **Answer: C.** Always add a default when a property may be missing.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q247.** What is the main advantage of `@ConfigurationProperties` over `@Value`?

- **A.** It is faster
- **B.** It works without Spring
- **C.** Type-safe binding of a group of properties to a POJO with relaxed binding and validation
- **D.** It supports only strings

> **Answer: C.** Relaxed binding maps my-first-name, myFirstName and MY\_FIRST\_NAME to the same field.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q248.** To change the application's context path to `/api` you set

- **A.** `spring.path=/api`
- **B.** `server.context=/api`
- **C.** `server.root=/api`
- **D.** `server.servlet.context-path=/api`

> **Answer: D.** server.port=0 picks a random free port.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q249.** A file `config/application.properties` placed next to the executable JAR will

- **A.** Override the properties packaged inside the JAR
- **B.** Be ignored
- **C.** Cause a startup error
- **D.** Be merged with lower precedence

> **Answer: A.** Order: file:./config/ \> file:./ \> classpath:/config/ \> classpath:/.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q250.** `@Profile("dev")` on a `@Bean` or `@Component` means

- **A.** The bean is registered in all profiles except dev
- **B.** The bean is registered only in production
- **C.** The bean is always registered
- **D.** The bean is registered only when the `dev` profile is active

> **Answer: D.** Use @Profile("!prod") to exclude.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q251.** The startup error `Failed to configure a DataSource: 'url' attribute is not specified` usually means

- **A.** A database starter is on the classpath but no datasource URL is configured (or exclude DataSourceAutoConfiguration)
- **B.** Hibernate is missing
- **C.** The database password is wrong
- **D.** The port is in use

> **Answer: A.** Either configure spring.datasource.\* or exclude the auto-configuration.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q252.** How do you disable a specific auto-configuration?

- **A.** `spring.disable=X`
- **B.** `@Disable(X.class)`
- **C.** Delete it from the JAR
- **D.** `@SpringBootApplication(exclude = X.class)` or `spring.autoconfigure.exclude`

> **Answer: D.** Use debug=true to see which auto-configurations were applied.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q253.** What is the default logging framework in Spring Boot and how do you set a package's level?

- **A.** SLF4J only; `slf4j.level=DEBUG`
- **B.** Log4j 1; `log.level=DEBUG`
- **C.** java.util.logging; `logging=DEBUG`
- **D.** Logback; `logging.level.com.myapp=DEBUG`

> **Answer: D.** Logback is included via spring-boot-starter-logging.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q254.** Which source has the HIGHEST precedence?

- **A.** `@PropertySource`
- **B.** Command-line arguments
- **C.** Default properties
- **D.** `application.properties` inside the JAR

> **Answer: B.** Order: command line \> system properties \> env vars \> profile files \> application files \> @PropertySource \> defaults.  
> *Revise:* Part 5 (Spring Boot configuration)

**Q255.** Which is true about YAML configuration files?

- **A.** They cannot be profile specific
- **B.** They cannot hold lists
- **C.** Tabs are required
- **D.** They are indentation-sensitive and must use spaces, not tabs

> **Answer: D.** A tab character in YAML causes a parsing error.  
> *Revise:* Part 5 (Spring Boot configuration)


## F. Microservices and system design


### Microservices

**Q256.** The 'database per service' pattern mainly provides

- **A.** Strong ACID transactions across services
- **B.** Easier joins across services
- **C.** Loose coupling and independent evolution and scaling of services
- **D.** Lower storage cost

> **Answer: C.** Services share data via APIs or events, not via a common schema.  
> *Revise:* Part 6 (Microservices)

**Q257.** Two microservices read and write the same database tables directly. This is

- **A.** An anti-pattern because it tightly couples the services
- **B.** Required by service discovery
- **C.** The recommended pattern
- **D.** Required for sagas

> **Answer: A.** Schema changes in one service break the other.  
> *Revise:* Part 6 (Microservices)

**Q258.** An API Gateway typically handles

- **A.** Container scheduling
- **B.** Routing, authentication, rate limiting and request aggregation at one entry point
- **C.** Source code compilation
- **D.** Database indexing

> **Answer: B.** Spring Cloud Gateway is the current option; Zuul 1 is legacy.  
> *Revise:* Part 6 (Microservices)

**Q259.** In Eureka-based discovery, a service instance

- **A.** Registers itself with the registry and clients look up instances by service name
- **B.** Registers with the database
- **C.** Is discovered by the browser
- **D.** Is found through a static IP list only

> **Answer: A.** Client-side load balancing then picks one instance.  
> *Revise:* Part 6 (Microservices)

**Q260.** Which are the states of a circuit breaker?

- **A.** Active, Idle, Dead
- **B.** Closed, Open, Half-Open
- **C.** Start, Stop, Pause
- **D.** Green, Yellow, Red only

> **Answer: B.** Open fails fast; half-open allows a few trial calls.  
> *Revise:* Part 6 (Microservices)

**Q261.** Which library is the recommended replacement for Netflix Hystrix in Spring Cloud?

- **A.** Resilience4j
- **B.** Ribbon
- **C.** Zuul
- **D.** Eureka

> **Answer: A.** Hystrix is in maintenance mode.  
> *Revise:* Part 6 (Microservices)

**Q262.** In the Saga pattern, how are failures handled?

- **A.** By ignoring them
- **B.** By a global two-phase commit lock
- **C.** By running compensating transactions that undo earlier local transactions
- **D.** By restarting the whole system

> **Answer: C.** Each step has a compensation action.  
> *Revise:* Part 6 (Microservices)

**Q263.** What is the difference between choreography and orchestration in sagas?

- **A.** Choreography: services react to each other's events with no central controller. Orchestration: a central orchestrator directs each step
- **B.** They are the same
- **C.** Choreography needs a database, orchestration does not
- **D.** Orchestration uses no messaging

> **Answer: A.** Orchestration makes the flow explicit; choreography reduces central coupling.  
> *Revise:* Part 6 (Microservices)

**Q264.** What does Spring Cloud Config Server provide?

- **A.** API rate limiting
- **B.** Service discovery
- **C.** Centralised, externalised configuration (often Git-backed) for all services
- **D.** Load balancing

> **Answer: C.** Clients refresh with @RefreshScope and /actuator/refresh.  
> *Revise:* Part 6 (Microservices)

**Q265.** What is OpenFeign?

- **A.** A message broker
- **B.** A database driver
- **C.** A load balancer
- **D.** A declarative HTTP client where you define an interface and Spring generates the implementation

> **Answer: D.** Feign works with service names through discovery.  
> *Revise:* Part 6 (Microservices)

**Q266.** The Bulkhead pattern

- **A.** Isolates resources (for example thread pools) per dependency so one failing dependency cannot exhaust everything
- **B.** Caches responses
- **C.** Retries failed calls
- **D.** Encrypts traffic

> **Answer: A.** Named after ship compartments.  
> *Revise:* Part 6 (Microservices)

**Q267.** Why must retries be combined with idempotency?

- **A.** Retries are always safe
- **B.** A retried non-idempotent request (like creating a payment) may be executed twice
- **C.** Retries need transactions
- **D.** Idempotency slows retries

> **Answer: B.** Use idempotency keys and exponential backoff with jitter.  
> *Revise:* Part 6 (Microservices)

**Q268.** In distributed tracing, what is propagated between services so a request can be followed?

- **A.** The database transaction
- **B.** A trace/correlation ID in headers (for example with Micrometer Tracing and Zipkin)
- **C.** The user password
- **D.** The JVM thread

> **Answer: B.** Sleuth was the Boot 2 tool; Micrometer Tracing replaces it in Boot 3.  
> *Revise:* Part 6 (Microservices)

**Q269.** The Strangler Fig pattern is used to

- **A.** Kill idle services
- **B.** Scale a database
- **C.** Encrypt data
- **D.** Gradually migrate a monolith by routing functionality to new services over time

> **Answer: D.** The old system shrinks as new services take over.  
> *Revise:* Part 6 (Microservices)

**Q270.** The Transactional Outbox pattern solves

- **A.** Service discovery
- **B.** Atomically updating the database and publishing an event, by storing the event in the same local transaction and publishing it later
- **C.** Slow queries
- **D.** Load balancing

> **Answer: B.** A poller or CDC publishes outbox rows to the broker.  
> *Revise:* Part 6 (Microservices)

**Q271.** A key benefit of asynchronous messaging (for example Kafka) between services is

- **A.** Immediate consistency
- **B.** Reduced temporal coupling and the ability to absorb traffic spikes
- **C.** No need for idempotency
- **D.** No monitoring needed

> **Answer: B.** Cost: eventual consistency and more moving parts.  
> *Revise:* Part 6 (Microservices)

**Q272.** A service mesh sidecar (such as Istio's Envoy) typically provides

- **A.** Business logic
- **B.** mTLS, retries, traffic routing and telemetry outside application code
- **C.** Compilation
- **D.** Database backups

> **Answer: B.** The proxy runs next to each service instance.  
> *Revise:* Part 6 (Microservices)

**Q273.** What is a Dead Letter Queue (DLQ)?

- **A.** A queue for deleted users
- **B.** A faster queue
- **C.** A queue for logs only
- **D.** A queue where messages that repeatedly fail processing are parked for inspection

> **Answer: D.** It prevents poison messages from blocking the main queue.  
> *Revise:* Part 6 (Microservices)

**Q274.** In Kubernetes, a failing \*\*readiness\*\* probe causes the pod to

- **A.** Be scaled up
- **B.** Be removed from service endpoints (no traffic) without necessarily being restarted
- **C.** Be restarted
- **D.** Be deleted permanently

> **Answer: B.** A failing liveness probe triggers a restart.  
> *Revise:* Part 6 (Microservices)

**Q275.** According to the 12-factor app, configuration should be stored

- **A.** In the environment, separate from code
- **B.** In the source repository next to code
- **C.** Inside the compiled code
- **D.** In the database schema

> **Answer: A.** This allows the same artifact to run in dev, test and prod.  
> *Revise:* Part 6 (Microservices)

**Q276.** Compared with a monolith, microservices usually accept

- **A.** Simpler operations
- **B.** Eventual consistency across services in exchange for independent deployment and scaling
- **C.** Stronger ACID across the whole system
- **D.** Fewer network calls

> **Answer: B.** Distributed systems trade consistency for availability and autonomy.  
> *Revise:* Part 6 (Microservices)

**Q277.** When is a monolith often the better starting point?

- **A.** Need for independent scaling of 50 components
- **B.** Polyglot requirements
- **C.** Small team, simple or unclear domain, early-stage product
- **D.** Hundreds of teams

> **Answer: C.** Many teams start with a modular monolith and split later.  
> *Revise:* Part 6 (Microservices)


### System design

**Q278.** Adding more machines to share the load is called

- **A.** Sharding only
- **B.** Vertical scaling
- **C.** Caching
- **D.** Horizontal scaling

> **Answer: D.** Scale out = horizontal. Scale up = bigger machine.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q279.** A load balancer algorithm that sends each request to the server with the fewest active connections is

- **A.** IP hash
- **B.** Least connections
- **C.** Random
- **D.** Round robin

> **Answer: B.** Good when requests have uneven duration.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q280.** A Layer 7 (application layer) load balancer can

- **A.** Route based on URL path or HTTP headers
- **B.** Only route by IP and port
- **C.** Not terminate TLS
- **D.** Only balance UDP

> **Answer: A.** L7 understands HTTP. L4 only sees IPs and ports.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q281.** Which of these is typically classified as an AP system under CAP?

- **A.** Cassandra
- **B.** Single-node PostgreSQL
- **C.** HBase
- **D.** ZooKeeper

> **Answer: A.** Cassandra favours availability with tunable consistency. HBase and ZooKeeper favour consistency.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q282.** During a network partition, a CP system will

- **A.** Ignore the partition
- **B.** Refuse or fail some requests to preserve consistency
- **C.** Delete the minority partition's data
- **D.** Keep serving possibly stale data from every node

> **Answer: B.** CP gives up availability on the affected side.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q283.** In the cache-aside pattern, the application

- **A.** Never touches the DB
- **B.** Checks the cache first. On a miss it reads the DB and populates the cache
- **C.** Writes only to the cache
- **D.** Lets the DB update the cache automatically

> **Answer: B.** Also called lazy loading.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q284.** Write-through caching

- **A.** Writes only to the DB
- **B.** Never evicts
- **C.** Writes to the DB asynchronously later
- **D.** Writes to the cache and the database synchronously

> **Answer: D.** Consistent but adds write latency.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q285.** The main risk of write-back (write-behind) caching is

- **A.** Data loss if the cache fails before flushing to the DB
- **B.** Slow reads
- **C.** High DB load
- **D.** Stale DB reads only

> **Answer: A.** Writes are acknowledged before they are persisted.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q286.** LRU eviction removes

- **A.** A random item
- **B.** The most recently used item
- **C.** The least recently used item
- **D.** The largest item

> **Answer: C.** Least Recently Used.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q287.** A cache stampede (thundering herd) occurs when

- **A.** A hot key expires and many requests hit the DB at once
- **B.** The cache is empty at startup only
- **C.** Keys are too long
- **D.** TTL is infinite

> **Answer: A.** Mitigate with request coalescing, locks, jittered TTLs or early refresh.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q288.** A CDN primarily improves

- **A.** Latency for static content by serving it from edge locations
- **B.** Schema design
- **C.** Database write throughput
- **D.** Transaction isolation

> **Answer: A.** Content is cached geographically close to users.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q289.** With consistent hashing, when a node is added or removed

- **A.** No keys move
- **B.** Only hot keys move
- **C.** All keys must be remapped
- **D.** Only about K/N keys are remapped

> **Answer: D.** Only a small fraction of keys moves, unlike modulo hashing.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q290.** Sharding by a sequential ID range commonly causes

- **A.** A hot shard because new writes hit the latest range
- **B.** Even load
- **C.** Faster joins
- **D.** No resharding need

> **Answer: A.** Monotonic keys send all new writes to one shard. Hash the key instead.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q291.** With leader-follower replication, reading from a follower may return

- **A.** No data
- **B.** Stale data due to replication lag
- **C.** Encrypted data only
- **D.** Always the latest data

> **Answer: B.** Asynchronous replication means followers can lag.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q292.** Compared with asynchronous replication, synchronous replication

- **A.** Gives stronger durability but higher write latency
- **B.** Has lower write latency
- **C.** Cannot be used with leaders
- **D.** Loses data more often

> **Answer: A.** The leader waits for follower acknowledgement.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q293.** A key benefit of putting a message queue between services is

- **A.** Removing the need for retries
- **B.** Replacing the database
- **C.** Guaranteeing zero latency
- **D.** Decoupling producers from consumers and absorbing traffic spikes

> **Answer: D.** Asynchronous buffering and loose coupling.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q294.** In Kafka, message ordering is guaranteed

- **A.** Never
- **B.** Only with one consumer group
- **C.** Across the entire topic
- **D.** Within a single partition

> **Answer: D.** Use the same key so related events land in the same partition.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q295.** With at-least-once delivery, consumers should be

- **A.** Stateful
- **B.** Single-threaded
- **C.** Idempotent
- **D.** Synchronous

> **Answer: C.** Duplicates can occur, so processing must be safe to repeat.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q296.** Which HTTP method is NOT idempotent by definition?

- **A.** GET
- **B.** DELETE
- **C.** POST
- **D.** PUT

> **Answer: C.** Repeating a POST can create multiple resources.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q297.** Which rate limiting algorithm allows short bursts while enforcing an average rate?

- **A.** Random drop
- **B.** Fixed counter with no refill
- **C.** Token bucket
- **D.** Round robin

> **Answer: C.** Tokens accumulate up to the bucket size.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q298.** Which HTTP status code is normally returned when a client exceeds a rate limit?

- **A.** 401
- **B.** 403
- **C.** 429
- **D.** 503

> **Answer: C.** 429 Too Many Requests.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q299.** A REST API is said to be stateless because

- **A.** Each request contains everything the server needs and the server stores no client session between requests
- **B.** It uses only GET
- **C.** It cannot use cookies
- **D.** It has no database

> **Answer: A.** Statelessness makes horizontal scaling easy.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q300.** An API gateway typically handles

- **A.** Disk scheduling
- **B.** Index maintenance
- **C.** Row-level locking
- **D.** Authentication, routing and rate limiting at a single entry point

> **Answer: D.** It is the front door for microservices.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q301.** A common drawback of microservices compared with a monolith is

- **A.** Technology flexibility
- **B.** Added complexity from distributed communication, consistency and monitoring
- **C.** Team autonomy
- **D.** Independent deployability

> **Answer: B.** Network calls, partial failures and data consistency become harder.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q302.** A circuit breaker pattern

- **A.** Encrypts traffic
- **B.** Shards the DB
- **C.** Stops calling a failing dependency for a while to prevent cascading failures
- **D.** Balances load

> **Answer: C.** States: closed, open, half-open.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q303.** Back-of-envelope: 1 million requests per day is about how many requests per second on average?

- **A.** ~1
- **B.** ~12
- **C.** ~120
- **D.** ~1200

> **Answer: B.** 1,000,000 / 86,400 is about 11.6.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q304.** Back-of-envelope: 10 million users, each with about 1 KB of profile data, need about

- **A.** 1 GB
- **B.** 10 GB
- **C.** 100 GB
- **D.** 1 TB

> **Answer: B.** 10⁷ × 10³ bytes = 10¹⁰ bytes = 10 GB.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q305.** A service with 99.9% availability can be down about how long per year?

- **A.** ~53 minutes
- **B.** ~8.8 hours
- **C.** ~3.65 days
- **D.** ~88 hours

> **Answer: B.** 0.1% of 8,760 hours is about 8.76 hours.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q306.** Latency and throughput are best defined as

- **A.** Latency = time for one request. Throughput = requests handled per unit time
- **B.** Both are the same thing
- **C.** Latency = requests per second. Throughput = delay
- **D.** Latency = storage. Throughput = CPU

> **Answer: A.** A system can have low latency but low throughput, and vice versa.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q307.** The standard way to remove a single point of failure is

- **A.** Redundancy (replicas, multiple instances) with failover
- **B.** Compression
- **C.** More logging
- **D.** Bigger hardware

> **Answer: A.** Duplicate the component and fail over automatically.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q308.** For data needing multi-row ACID transactions and complex joins (e.g. banking), the usual choice is

- **A.** A relational SQL database
- **B.** A CDN
- **C.** A search index
- **D.** A pure key-value cache

> **Answer: A.** SQL databases give strong ACID guarantees.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q309.** A URL shortener uses 7 characters from base62 (a-z, A-Z, 0-9). The approximate number of unique keys is

- **A.** 3.5 million
- **B.** 3.5 billion
- **C.** 3.5 trillion
- **D.** 62 thousand

> **Answer: C.** 62⁷ ≈ 3.5 × 10¹².  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q310.** A Bloom filter can produce

- **A.** Neither
- **B.** False negatives only
- **C.** False positives but no false negatives
- **D.** Both always

> **Answer: C.** 'Possibly in set' or 'definitely not in set'.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q311.** Heartbeats between nodes are used to

- **A.** Compress logs
- **B.** Detect node failures
- **C.** Encrypt data
- **D.** Shard data

> **Answer: B.** Missing heartbeats suggest a node is down.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q312.** Which of these is a distributed consensus algorithm used for leader election and replication?

- **A.** LRU
- **B.** Round robin
- **C.** Raft
- **D.** Dijkstra

> **Answer: C.** Raft (and Paxos) achieve agreement among nodes.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q313.** Eventual consistency means

- **A.** Writes are rejected
- **B.** Reads always see the latest write
- **C.** Data is never consistent
- **D.** If no new updates occur, all replicas eventually converge to the same value

> **Answer: D.** Replicas may differ temporarily.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q314.** In a quorum system with N = 3 replicas, which setting guarantees a read sees the latest acknowledged write?

- **A.** W = 1, R = 2
- **B.** W = 2, R = 1
- **C.** W = 1, R = 1
- **D.** W = 2, R = 2

> **Answer: D.** W + R \> N (2 + 2 \> 3) ensures overlap.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q315.** A known weakness of two-phase commit (2PC) is that

- **A.** It has no prepare phase
- **B.** It can block participants if the coordinator fails
- **C.** It cannot work with SQL
- **D.** It is not atomic

> **Answer: B.** Participants can be stuck holding locks while waiting for the coordinator.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q316.** The Saga pattern handles distributed transactions by

- **A.** Using only 2PC
- **B.** Using one global lock
- **C.** A sequence of local transactions with compensating actions for rollback
- **D.** Disabling failures

> **Answer: C.** Each step has an undo (compensation) step.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q317.** CQRS stands for and means

- **A.** Central Queue Retry System
- **B.** Cache Query Read Sync
- **C.** Consistent Query Replica Sharding
- **D.** Command Query Responsibility Segregation: separate models for writes and reads

> **Answer: D.** Allows reads and writes to scale and be modelled independently.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q318.** In a publish/subscribe model, a published message is

- **A.** Delivered to exactly one consumer
- **B.** Never retried
- **C.** Stored only in the DB
- **D.** Delivered to every subscriber of the topic

> **Answer: D.** Contrast with a work queue, where each message goes to one worker.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q319.** A reverse proxy such as Nginx

- **A.** Stores session state
- **B.** Is a database
- **C.** Sits in front of servers, forwarding client requests and often handling TLS, caching and load balancing
- **D.** Sits in front of clients hiding their identity

> **Answer: C.** Forward proxy = client side. Reverse proxy = server side.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q320.** The best fit for a real-time chat application's client-server channel is

- **A.** WebSocket (persistent full-duplex connection)
- **B.** SMTP
- **C.** Plain HTTP polling every minute
- **D.** FTP

> **Answer: A.** WebSockets allow server push with low overhead.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q321.** Where should user-uploaded images and videos typically be stored?

- **A.** In object storage (e.g. S3) with a CDN in front, and metadata in the DB
- **B.** In message queues
- **C.** In the cache only
- **D.** As blobs in the primary relational DB

> **Answer: A.** Object storage is cheap, scalable and durable for large binaries.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q322.** In a news feed design, 'fan-out on write' has a problem with celebrity accounts because

- **A.** Writing a post to millions of followers' feeds is very expensive
- **B.** Reads become impossible
- **C.** Posts cannot be stored
- **D.** Followers cannot be counted

> **Answer: A.** Use a hybrid: push for normal users, pull for celebrities.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q323.** Which structures are commonly used to find nearby places (proximity search)?

- **A.** Geohash or quadtree
- **B.** Linked list
- **C.** Bloom filter
- **D.** Hash join

> **Answer: A.** Spatial indexing partitions the map into cells.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q324.** A drawback of sticky sessions on a load balancer is

- **A.** Uneven load and lost sessions if the pinned server fails
- **B.** They improve statelessness
- **C.** They remove the need for sessions
- **D.** They need no cookies

> **Answer: A.** Prefer storing sessions in a shared store such as Redis.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q325.** Splitting a table by columns, e.g. moving rarely used large columns into a separate table, is called

- **A.** Sharding by hash
- **B.** Horizontal partitioning
- **C.** Vertical partitioning
- **D.** Replication

> **Answer: C.** Horizontal = by rows. Vertical = by columns.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q326.** The three pillars of observability are

- **A.** Alerts, emails and tickets
- **B.** CPU, RAM and disk
- **C.** Cache, queue and DB
- **D.** Logs, metrics and traces

> **Answer: D.** Together they help debug distributed systems.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9

**Q327.** For a read-heavy workload with a slow primary database, the first improvements to consider are

- **A.** Disabling replication
- **B.** Caching and read replicas
- **C.** Removing all indexes
- **D.** Synchronous writes everywhere

> **Answer: B.** Offload reads from the primary.  
> *Revise:* Part 6 §6.5 (System design recap) and Part 1B §1B.7 to 1B.9


## G. JWT, Spring Security and CORS


### JWT and security

**Q328.** A JWT consists of which three parts?

- **A.** Username, password, token
- **B.** Header, body, footer
- **C.** Header, payload, signature separated by dots
- **D.** Key, value, hash

> **Answer: C.** Each part is Base64URL encoded.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q329.** The payload of a standard signed JWT (JWS) is

- **A.** Encrypted with AES
- **B.** Base64URL encoded, readable by anyone, and not encrypted
- **C.** Compressed and unreadable
- **D.** Hashed with BCrypt

> **Answer: B.** Never store passwords or secrets in a JWT payload.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q330.** What does the JWT signature guarantee?

- **A.** That the token cannot expire
- **B.** That the user is still logged in
- **C.** Confidentiality of the payload
- **D.** Integrity and authenticity: the token was not modified and was issued by the key holder

> **Answer: D.** Confidentiality would need JWE (encrypted tokens).  
> *Revise:* Part 7 (JWT and Spring Security)

**Q331.** What is the main difference between HS256 and RS256?

- **A.** No difference
- **B.** HS256 uses one shared secret (symmetric); RS256 signs with a private key and verifies with a public key (asymmetric)
- **C.** HS256 is asymmetric and RS256 is symmetric
- **D.** HS256 encrypts, RS256 hashes

> **Answer: B.** RS256 suits many services that only need to verify tokens.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q332.** Which registered claim defines when a JWT expires?

- **A.** `sub`
- **B.** `iat`
- **C.** `iss`
- **D.** `exp`

> **Answer: D.** iat = issued at, sub = subject, iss = issuer, aud = audience, nbf = not before, jti = id.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q333.** What does 'stateless authentication' with JWT mean?

- **A.** The user has no state in the DB
- **B.** The server keeps no session; it validates the token's signature and expiry on each request
- **C.** Tokens never expire
- **D.** The server stores tokens in memory

> **Answer: B.** This makes horizontal scaling easy.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q334.** What is the main drawback of stateless JWTs?

- **A.** They cannot carry claims
- **B.** They cannot be signed
- **C.** They are hard to revoke before expiry without extra mechanisms such as a denylist
- **D.** They are slower than sessions always

> **Answer: C.** Use short-lived access tokens and refresh token rotation.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q335.** Access tokens are kept short-lived while refresh tokens are long-lived because

- **A.** Access tokens cannot be signed
- **B.** Refresh tokens are smaller
- **C.** It saves bandwidth
- **D.** A leaked access token is only useful for a short time and the refresh token is used rarely and can be revoked

> **Answer: D.** The refresh token is exchanged for a new access token.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q336.** How is a JWT usually sent to a protected REST API?

- **A.** In a `X-Password` header
- **B.** In the response body
- **C.** `Authorization: Bearer <token>` header
- **D.** In the URL path

> **Answer: C.** Putting tokens in URLs leaks them into logs.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q337.** A valid JWT is sent but the user's role is insufficient for the endpoint. Which status code should be returned?

- **A.** 403 Forbidden
- **B.** 401 Unauthorized
- **C.** 400 Bad Request
- **D.** 404 Not Found

> **Answer: A.** 401 is for missing or invalid authentication.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q338.** The 'alg: none' attack on JWTs is prevented by

- **A.** Using HTTP
- **B.** Having the server enforce the expected signing algorithm instead of trusting the token header
- **C.** Using longer tokens
- **D.** Using cookies

> **Answer: B.** Also validate iss, aud and exp.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q339.** In a Spring Security JWT setup, where is the custom JWT filter (extending `OncePerRequestFilter`) normally placed?

- **A.** After the controller
- **B.** After the response is written
- **C.** Before `UsernamePasswordAuthenticationFilter`
- **D.** Inside the service layer

> **Answer: C.** It validates the token and sets the Authentication in SecurityContextHolder.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q340.** Which setting makes a Spring Security application stateless?

- **A.** `httpBasic()`
- **B.** `csrf().enable()`
- **C.** `rememberMe()`
- **D.** `sessionCreationPolicy(SessionCreationPolicy.STATELESS)`

> **Answer: D.** No HttpSession is created or used to store the security context.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q341.** BCrypt (used by `BCryptPasswordEncoder`) is

- **A.** A symmetric cipher
- **B.** A one-way, salted, adaptive hash. Passwords cannot be decrypted, only compared with `matches()`
- **C.** A reversible encryption
- **D.** An encoding like Base64

> **Answer: B.** Salt is stored inside the hash string.  
> *Revise:* Part 7 (JWT and Spring Security)

**Q342.** In Spring Security 6 how do you configure HTTP security?

- **A.** Use XML only
- **B.** Use `@EnableWebSecurityAdapter`
- **C.** Extend `WebSecurityConfigurerAdapter`
- **D.** Declare a `SecurityFilterChain` bean (WebSecurityConfigurerAdapter is removed)

> **Answer: D.** The adapter was deprecated in 5.7 and removed in 6.  
> *Revise:* Part 7 (JWT and Spring Security)


### CORS

**Q343.** Which of the following is the SAME origin as `https://example.com/app`?

- **A.** `https://example.com:8443/app`
- **B.** `https://example.com/other`
- **C.** `http://example.com/app`
- **D.** `https://api.example.com/app`

> **Answer: B.** Origin = scheme + host + port. The path does not matter.  
> *Revise:* Part 7 (CORS)

**Q344.** CORS restrictions are enforced by

- **A.** The firewall
- **B.** The database
- **C.** The browser
- **D.** The web server

> **Answer: C.** That is why Postman or curl work while the browser reports a CORS error.  
> *Revise:* Part 7 (CORS)

**Q345.** A browser sends a CORS preflight request using which HTTP method?

- **A.** TRACE
- **B.** HEAD
- **C.** GET
- **D.** OPTIONS

> **Answer: D.** It carries Origin, Access-Control-Request-Method and Access-Control-Request-Headers.  
> *Revise:* Part 7 (CORS)

**Q346.** Which response header tells the browser which origin may read the response?

- **A.** `Origin-Allow`
- **B.** `Content-Origin`
- **C.** `X-Frame-Options`
- **D.** `Access-Control-Allow-Origin`

> **Answer: D.** Other related headers are Allow-Methods, Allow-Headers, Allow-Credentials and Max-Age.  
> *Revise:* Part 7 (CORS)

**Q347.** Which combination is NOT allowed by browsers?

- **A.** Allow-Methods GET, POST
- **B.** `Access-Control-Allow-Credentials: true` together with `Access-Control-Allow-Origin: *`
- **C.** Allow-Origin `*` without credentials
- **D.** Allow-Credentials true with a specific origin

> **Answer: B.** In Spring use specific origins or allowedOriginPatterns when allowCredentials is true.  
> *Revise:* Part 7 (CORS)

**Q348.** Which Spring annotation enables CORS for a controller or a method?

- **A.** `@EnableCors`
- **B.** `@CrossOrigin`
- **C.** `@Cors`
- **D.** `@AllowOrigin`

> **Answer: B.** Globally, implement WebMvcConfigurer.addCorsMappings.  
> *Revise:* Part 7 (CORS)

**Q349.** With Spring Security enabled, a browser preflight request is rejected with 401/403 although `@CrossOrigin` is present. A likely fix is

- **A.** Disable the browser
- **B.** Enable CORS in the security filter chain (`http.cors(...)`) so the preflight is handled before authentication
- **C.** Switch to HTTP/2
- **D.** Add `@Transactional`

> **Answer: B.** A preflight carries no credentials, so security must let CORS processing answer it.  
> *Revise:* Part 7 (CORS)

**Q350.** Which request normally triggers a preflight?

- **A.** A request with `Content-Type: application/json` and an `Authorization` header
- **B.** A simple form POST
- **C.** A GET for an image
- **D.** A plain GET with no custom headers

> **Answer: A.** Custom headers and JSON content type make a request non-simple.  
> *Revise:* Part 7 (CORS)

**Q351.** A 'simple' cross-origin GET request without the required CORS headers in the response

- **A.** Is cached forever
- **B.** Is still sent to the server but the browser blocks JavaScript from reading the response
- **C.** Is never sent
- **D.** Returns 404

> **Answer: B.** CORS does not stop the request from reaching the server.  
> *Revise:* Part 7 (CORS)

**Q352.** Is CORS a defence against CSRF attacks?

- **A.** Yes, completely
- **B.** Yes for POST only
- **C.** No. CORS controls which origins may read responses; it does not stop forged requests from being sent
- **D.** Yes if Allow-Origin is \*

> **Answer: C.** Use CSRF tokens, SameSite cookies and custom headers.  
> *Revise:* Part 7 (CORS)


## H. OOP design: aggregation, composition, cohesion, coupling


### OOP: aggregation, composition, cohesion, coupling

**Q353.** What is the key difference between aggregation and composition?

- **A.** They are the same
- **B.** Composition is weaker than aggregation
- **C.** Aggregation uses inheritance and composition does not
- **D.** In composition the part cannot exist without the whole and dies with it; in aggregation the part can exist independently

> **Answer: D.** House and Rooms = composition. Department and Teachers = aggregation.  
> *Revise:* Part 8 (OOP design)

**Q354.** Which pair is the best example of \*\*composition\*\*?

- **A.** Team and Player
- **B.** Library and Book
- **C.** House and Room
- **D.** Department and Teacher

> **Answer: C.** Rooms do not exist independently of the house. Teachers, books and players can exist alone.  
> *Revise:* Part 8 (OOP design)

**Q355.** In UML, aggregation and composition are drawn with

- **A.** Dashed arrows for both
- **B.** Hollow triangles for both
- **C.** A hollow diamond for aggregation and a filled diamond for composition
- **D.** A filled diamond for aggregation and a hollow one for composition

> **Answer: C.** A hollow triangle is for inheritance.  
> *Revise:* Part 8 (OOP design)

**Q356.** Association, aggregation and composition are all examples of

- **A.** 'Has-a' style relationships (as opposed to the 'is-a' of inheritance)
- **B.** Encapsulation
- **C.** Polymorphism
- **D.** Inheritance

> **Answer: A.** Association is the most general; aggregation and composition are special cases.  
> *Revise:* Part 8 (OOP design)

**Q357.** \*\*Cohesion\*\* describes

- **A.** How closely the responsibilities within a single module or class are related
- **B.** The speed of a method
- **C.** How much modules depend on each other
- **D.** The size of a class

> **Answer: A.** Aim for high cohesion.  
> *Revise:* Part 8 (OOP design)

**Q358.** \*\*Coupling\*\* describes

- **A.** The relatedness inside one class
- **B.** Class inheritance depth
- **C.** The degree of dependency between different modules or classes
- **D.** Number of methods

> **Answer: C.** Aim for low (loose) coupling.  
> *Revise:* Part 8 (OOP design)

**Q359.** Which is the BEST (strongest, most desirable) type of cohesion?

- **A.** Logical cohesion
- **B.** Functional cohesion
- **C.** Coincidental cohesion
- **D.** Temporal cohesion

> **Answer: B.** Every element contributes to one well-defined task.  
> *Revise:* Part 8 (OOP design)

**Q360.** Which is the WORST type of cohesion?

- **A.** Sequential cohesion
- **B.** Functional cohesion
- **C.** Coincidental cohesion
- **D.** Communicational cohesion

> **Answer: C.** Unrelated elements are grouped arbitrarily, like a miscellaneous Utils class.  
> *Revise:* Part 8 (OOP design)

**Q361.** Which is the worst (tightest) type of coupling?

- **A.** Data coupling
- **B.** Content coupling (one module uses another's internals)
- **C.** Message coupling
- **D.** Stamp coupling

> **Answer: B.** Data and message coupling are the most desirable.  
> *Revise:* Part 8 (OOP design)

**Q362.** A `UserManager` class validates input, saves to the database, sends emails and prints reports. This design suffers from

- **A.** Good encapsulation
- **B.** Low cohesion (violates the Single Responsibility Principle)
- **C.** High cohesion
- **D.** Loose coupling

> **Answer: B.** Split into validator, repository, mail service and report classes.  
> *Revise:* Part 8 (OOP design)

**Q363.** The Open/Closed Principle states that classes should be

- **A.** Always final
- **B.** Always abstract
- **C.** Open for extension but closed for modification
- **D.** Open for modification but closed for extension

> **Answer: C.** Add new behaviour with new classes or strategies instead of editing if/else chains.  
> *Revise:* Part 8 (OOP design)

**Q364.** `Square extends Rectangle`, where setting the width of a Square also changes its height, violates

- **A.** Interface Segregation
- **B.** Liskov Substitution Principle
- **C.** Dependency Inversion
- **D.** Single Responsibility

> **Answer: B.** A Square cannot be substituted for a Rectangle without surprising callers.  
> *Revise:* Part 8 (OOP design)

**Q365.** The Dependency Inversion Principle says

- **A.** Classes must have no dependencies
- **B.** Low-level modules should depend on high-level modules
- **C.** High-level modules should depend on abstractions, not on concrete low-level classes
- **D.** Use static methods

> **Answer: C.** Dependency Injection is a technique to achieve it.  
> *Revise:* Part 8 (OOP design)

**Q366.** Which of the following is TRUE for an abstract class compared with an interface (Java 8)?

- **A.** A class can extend several abstract classes
- **B.** An interface can have constructors
- **C.** An abstract class can have instance fields and constructors; an interface cannot have instance state
- **D.** Interfaces cannot have any method bodies

> **Answer: C.** Interfaces can have default and static methods since Java 8.  
> *Revise:* Part 8 (OOP design)

**Q367.**

```java
String a = new String("hi");
String b = new String("hi");
System.out.println((a == b) + " " + a.equals(b));
```

Output?

- **A.** true false
- **B.** true true
- **C.** false true
- **D.** false false

> **Answer: C.** == compares references; equals compares content.  
> *Revise:* Part 8 (OOP design)

**Q368.**

```java
static int f() {
  try { return 1; }
  finally { return 2; }
}
```

What does `f()` return?

- **A.** Exception
- **B.** Compile error
- **C.** 1
- **D.** 2

> **Answer: D.** A return in finally overrides the return from try.  
> *Revise:* Part 8 (OOP design)

**Q369.**

```java
String s = "a";
s.concat("b");
System.out.println(s);
```

Output?

- **A.** Compile error
- **B.** b
- **C.** ab
- **D.** a

> **Answer: D.** Strings are immutable. concat returns a new String that was discarded.  
> *Revise:* Part 8 (OOP design)

**Q370.** Which statement about overloading and overriding is correct?

- **A.** Overloading is runtime and overriding is compile time
- **B.** Overloading is resolved at compile time; overriding is resolved at runtime
- **C.** Both are compile time
- **D.** Both are runtime

> **Answer: B.** Overriding gives dynamic (runtime) polymorphism.  
> *Revise:* Part 8 (OOP design)


## I. JUnit and Mockito


### JUnit and Mockito

**Q371.** JUnit 5 is made up of

- **A.** JUnit Runner and Matcher
- **B.** JUnit Core and JUnit Extras
- **C.** JUnit Platform, JUnit Jupiter and JUnit Vintage
- **D.** Jupiter only

> **Answer: C.** Vintage runs JUnit 3/4 tests on the JUnit 5 platform.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q372.** Which JUnit 5 annotation replaces JUnit 4's `@Before`?

- **A.** `@BeforeAll`
- **B.** `@BeforeEach`
- **C.** `@Init`
- **D.** `@Setup`

> **Answer: B.** @BeforeAll replaces @BeforeClass, @Disabled replaces @Ignore.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q373.** By default, `@BeforeAll` methods in JUnit 5 must be

- **A.** synchronized
- **B.** final
- **C.** private
- **D.** static

> **Answer: D.** Unless the class uses @TestInstance(Lifecycle.PER\_CLASS).  
> *Revise:* Part 9 (JUnit and Mockito)

**Q374.** What is the correct parameter order of `assertEquals`?

- **A.** `assertEquals(expected, actual)`
- **B.** `assertEquals(message, actual)`
- **C.** Order does not matter
- **D.** `assertEquals(actual, expected)`

> **Answer: A.** A wrong order only affects the failure message, but expected should come first.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q375.** Which assertion checks that code throws a specific exception in JUnit 5?

- **A.** `@Test(expected=...)`
- **B.** `assertException`
- **C.** `assertThrows(Exception.class, () -> code())`
- **D.** `assertError`

> **Answer: C.** @Test(expected = ...) is JUnit 4.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q376.** `@ParameterizedTest` runs the same test with different inputs. Which are valid sources?

- **A.** `@ValueSource`, `@CsvSource`, `@MethodSource`
- **B.** `@Param`, `@Data`
- **C.** `@Table`
- **D.** `@Random`, `@Seed`

> **Answer: A.** Also @EnumSource and @NullSource.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q377.** In Mockito, `@Mock` and `@InjectMocks` differ because

- **A.** They are the same
- **B.** `@Mock` creates a fake dependency; `@InjectMocks` creates the real object under test and injects the mocks into it
- **C.** `@InjectMocks` creates fakes
- **D.** `@Mock` creates the real object

> **Answer: B.** Use @ExtendWith(MockitoExtension.class) to initialise them in JUnit 5.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q378.** What does `verify(mock, times(2)).save(any())` check?

- **A.** That `save` is faster than 2 ms
- **B.** That `save` was called exactly twice on the mock
- **C.** That the mock was created twice
- **D.** That `save` returns 2

> **Answer: B.** never() = zero times, atLeastOnce(), atMost(n).  
> *Revise:* Part 9 (JUnit and Mockito)

**Q379.** Which Mockito call is INVALID?

- **A.** `when(m.get(anyLong())).thenReturn(1)`
- **B.** `verify(m).process(1, "x")`
- **C.** `verify(m).process(anyInt(), "x")` (mixing a matcher with a raw value)
- **D.** `verify(m).process(anyInt(), eq("x"))`

> **Answer: C.** If one argument uses a matcher, all must use matchers (InvalidUseOfMatchersException).  
> *Revise:* Part 9 (JUnit and Mockito)

**Q380.** What does an unstubbed method of a Mockito mock returning `List<String>` return?

- **A.** An exception
- **B.** An empty list
- **C.** A list with one null
- **D.** null

> **Answer: B.** Objects return null, numbers 0, booleans false, collections empty, Optional.empty().  
> *Revise:* Part 9 (JUnit and Mockito)

**Q381.** A `@Spy` differs from a `@Mock` because

- **A.** A spy wraps a real object and calls real methods unless stubbed
- **B.** A spy is always faster
- **C.** A spy replaces the JVM
- **D.** A spy cannot be verified

> **Answer: A.** For spies, stub with doReturn(x).when(spy).method() to avoid calling the real method.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q382.** `@WebMvcTest(UserController.class)` loads

- **A.** The full application context
- **B.** Only the web layer (controllers, advice, filters). Services must be supplied with `@MockBean`
- **C.** Nothing
- **D.** Only repositories

> **Answer: B.** Use MockMvc to perform requests.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q383.** `@SpringBootTest` is typically used for

- **A.** Database migration
- **B.** Integration tests that load the full application context
- **C.** Mocking static methods
- **D.** Tests of a single pure function

> **Answer: B.** It is slower than slice tests.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q384.** `@DataJpaTest` by default

- **A.** Uses production MySQL
- **B.** Configures only JPA components with an embedded database and rolls back each test
- **C.** Disables transactions
- **D.** Starts the full server

> **Answer: B.** Use @AutoConfigureTestDatabase to use a real database.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q385.** What is the difference between `@Mock` and `@MockBean`?

- **A.** `@MockBean` is for static classes
- **B.** They are identical
- **C.** `@Mock` is only for interfaces
- **D.** `@MockBean` registers the mock as a bean in the Spring context (replacing an existing one); `@Mock` is plain Mockito without Spring

> **Answer: D.** From Spring Boot 3.4 / Spring Framework 6.2 `@MockitoBean` is the new name.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q386.** The TDD cycle is

- **A.** Red (write a failing test), Green (make it pass), Refactor
- **B.** Design, Code, Debug
- **C.** Build, Test, Release
- **D.** Green, Red, Deploy

> **Answer: A.** Tests drive the design.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q387.** Which statement about code coverage is correct?

- **A.** 100% coverage means no bugs
- **B.** 100% line coverage does not guarantee absence of bugs
- **C.** Coverage is only for integration tests
- **D.** Coverage measures performance

> **Answer: B.** JaCoCo reports line and branch coverage. Quality of assertions matters.  
> *Revise:* Part 9 (JUnit and Mockito)

**Q388.** `ArgumentCaptor` is used to

- **A.** Capture exceptions
- **B.** Capture logs
- **C.** Capture timing
- **D.** Capture the actual argument passed to a mocked method so it can be asserted

> **Answer: D.** Typically combined with verify(mock).method(captor.capture()).  
> *Revise:* Part 9 (JUnit and Mockito)


---

## Answer key (quick check)

| Q | Ans | Q | Ans | Q | Ans | Q | Ans | Q | Ans |
|---|---|---|---|---|---|---|---|---|---|
| 1 | C | 2 | D | 3 | C | 4 | A | 5 | D |
| 6 | A | 7 | B | 8 | D | 9 | A | 10 | A |
| 11 | D | 12 | C | 13 | C | 14 | D | 15 | C |
| 16 | A | 17 | A | 18 | A | 19 | D | 20 | C |
| 21 | A | 22 | C | 23 | D | 24 | B | 25 | D |
| 26 | B | 27 | B | 28 | A | 29 | B | 30 | B |
| 31 | C | 32 | D | 33 | B | 34 | A | 35 | B |
| 36 | B | 37 | D | 38 | D | 39 | C | 40 | A |
| 41 | C | 42 | A | 43 | A | 44 | A | 45 | D |
| 46 | D | 47 | D | 48 | B | 49 | A | 50 | B |
| 51 | A | 52 | A | 53 | A | 54 | D | 55 | B |
| 56 | C | 57 | B | 58 | B | 59 | C | 60 | D |
| 61 | B | 62 | D | 63 | D | 64 | D | 65 | D |
| 66 | D | 67 | A | 68 | B | 69 | A | 70 | C |
| 71 | B | 72 | C | 73 | D | 74 | B | 75 | A |
| 76 | D | 77 | D | 78 | C | 79 | D | 80 | A |
| 81 | B | 82 | B | 83 | C | 84 | A | 85 | B |
| 86 | B | 87 | B | 88 | D | 89 | C | 90 | B |
| 91 | D | 92 | A | 93 | B | 94 | A | 95 | B |
| 96 | D | 97 | D | 98 | D | 99 | B | 100 | B |
| 101 | B | 102 | B | 103 | A | 104 | D | 105 | B |
| 106 | A | 107 | C | 108 | A | 109 | B | 110 | B |
| 111 | B | 112 | B | 113 | C | 114 | D | 115 | C |
| 116 | C | 117 | C | 118 | B | 119 | C | 120 | B |
| 121 | B | 122 | C | 123 | A | 124 | B | 125 | C |
| 126 | A | 127 | B | 128 | B | 129 | B | 130 | D |
| 131 | A | 132 | B | 133 | A | 134 | B | 135 | B |
| 136 | B | 137 | C | 138 | A | 139 | B | 140 | B |
| 141 | C | 142 | B | 143 | D | 144 | A | 145 | B |
| 146 | A | 147 | A | 148 | C | 149 | A | 150 | A |
| 151 | D | 152 | B | 153 | C | 154 | D | 155 | D |
| 156 | B | 157 | D | 158 | A | 159 | D | 160 | C |
| 161 | C | 162 | A | 163 | A | 164 | A | 165 | B |
| 166 | B | 167 | A | 168 | A | 169 | A | 170 | A |
| 171 | A | 172 | C | 173 | A | 174 | C | 175 | A |
| 176 | A | 177 | C | 178 | C | 179 | B | 180 | B |
| 181 | D | 182 | A | 183 | A | 184 | C | 185 | A |
| 186 | D | 187 | A | 188 | A | 189 | A | 190 | D |
| 191 | A | 192 | A | 193 | D | 194 | A | 195 | B |
| 196 | D | 197 | A | 198 | C | 199 | C | 200 | D |
| 201 | D | 202 | C | 203 | C | 204 | C | 205 | A |
| 206 | D | 207 | A | 208 | D | 209 | D | 210 | B |
| 211 | A | 212 | A | 213 | A | 214 | B | 215 | D |
| 216 | C | 217 | C | 218 | B | 219 | B | 220 | C |
| 221 | C | 222 | C | 223 | D | 224 | A | 225 | C |
| 226 | D | 227 | C | 228 | B | 229 | B | 230 | B |
| 231 | B | 232 | C | 233 | C | 234 | D | 235 | C |
| 236 | A | 237 | A | 238 | C | 239 | B | 240 | A |
| 241 | D | 242 | A | 243 | C | 244 | A | 245 | C |
| 246 | C | 247 | C | 248 | D | 249 | A | 250 | D |
| 251 | A | 252 | D | 253 | D | 254 | B | 255 | D |
| 256 | C | 257 | A | 258 | B | 259 | A | 260 | B |
| 261 | A | 262 | C | 263 | A | 264 | C | 265 | D |
| 266 | A | 267 | B | 268 | B | 269 | D | 270 | B |
| 271 | B | 272 | B | 273 | D | 274 | B | 275 | A |
| 276 | B | 277 | C | 278 | D | 279 | B | 280 | A |
| 281 | A | 282 | B | 283 | B | 284 | D | 285 | A |
| 286 | C | 287 | A | 288 | A | 289 | D | 290 | A |
| 291 | B | 292 | A | 293 | D | 294 | D | 295 | C |
| 296 | C | 297 | C | 298 | C | 299 | A | 300 | D |
| 301 | B | 302 | C | 303 | B | 304 | B | 305 | B |
| 306 | A | 307 | A | 308 | A | 309 | C | 310 | C |
| 311 | B | 312 | C | 313 | D | 314 | D | 315 | B |
| 316 | C | 317 | D | 318 | D | 319 | C | 320 | A |
| 321 | A | 322 | A | 323 | A | 324 | A | 325 | C |
| 326 | D | 327 | B | 328 | C | 329 | B | 330 | D |
| 331 | B | 332 | D | 333 | B | 334 | C | 335 | D |
| 336 | C | 337 | A | 338 | B | 339 | C | 340 | D |
| 341 | B | 342 | D | 343 | B | 344 | C | 345 | D |
| 346 | D | 347 | B | 348 | B | 349 | B | 350 | A |
| 351 | B | 352 | C | 353 | D | 354 | C | 355 | C |
| 356 | A | 357 | A | 358 | C | 359 | B | 360 | C |
| 361 | B | 362 | B | 363 | C | 364 | B | 365 | C |
| 366 | C | 367 | C | 368 | D | 369 | D | 370 | B |
| 371 | C | 372 | B | 373 | D | 374 | A | 375 | C |
| 376 | A | 377 | B | 378 | B | 379 | C | 380 | B |
| 381 | A | 382 | B | 383 | B | 384 | B | 385 | D |
| 386 | A | 387 | B | 388 | D | | | | |

