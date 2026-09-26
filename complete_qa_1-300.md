# Java Interview Guide – 10 Years Experience Level
## PART 1: Java Basics & Fundamentals (Q1–40)

---

### Q1. What are the main features of Java?

Java is built around several core design goals:

**Platform Independence** – Write Once, Run Anywhere (WORA). Java compiles to bytecode which runs on any JVM regardless of OS/hardware.

**Object-Oriented** – Everything (except primitives) is an object. Supports encapsulation, inheritance, polymorphism, abstraction.

**Strongly Typed** – Every variable must be declared with a type; no implicit type coercion between incompatible types.

**Automatic Memory Management** – Garbage Collector handles heap memory; developers don't call free().

**Multi-threaded** – Built-in thread support via java.lang.Thread and java.util.concurrent.

**Robust** – Exception handling, null safety (to an extent), array bounds checking prevent common bugs.

**Secure** – No pointer arithmetic, bytecode verification, SecurityManager (deprecated in Java 17+), sandboxed classloading.

**Distributed** – RMI, sockets, networking APIs built-in.

**High Performance** – JIT compilation optimizes hot code paths at runtime (C1/C2 compilers).

**Interpreted + Compiled** – Bytecode is interpreted by JVM, but JIT compiles hotspots to native code.

At senior/10-year level, you should also mention Java's **ecosystem maturity**: Spring, Maven/Gradle, vast library support, LTS releases, corporate backing (Oracle + OpenJDK community).

---

### Q2. Why is Java platform-independent?

Java source code (.java) is compiled by `javac` into **bytecode** (.class files), not into native machine code. Bytecode is a low-level, intermediate representation that is **architecture-neutral**.

The **JVM (Java Virtual Machine)** is platform-specific – there are different JVM implementations for Windows, Linux, macOS, ARM, x86. The JVM is the abstraction layer that interprets or JIT-compiles bytecode into native instructions specific to that platform.

So:
- **Source code → bytecode** (platform-independent)
- **Bytecode → native code** (done by JVM at runtime, platform-specific)

Important nuance at senior level: **The JVM itself is NOT platform-independent** – it's written in C/C++ and compiled per platform. It's the bytecode that is portable. Also, **native code** (via JNI/JNA) breaks this portability.

---

### Q3. What is the difference between JDK, JRE, and JVM?

**JVM (Java Virtual Machine)**
- Abstract computing machine that executes bytecode
- Manages memory (heap, stack, metaspace), GC, JIT compilation
- Just an execution engine; it cannot compile Java source code

**JRE (Java Runtime Environment)**
- JVM + standard class libraries (java.lang, java.util, java.io, etc.)
- Everything needed to *run* a Java application
- Does NOT include compiler (`javac`)
- In Java 9+, modular JRE concept; separate JRE distributions are largely deprecated in favor of custom runtimes with `jlink`

**JDK (Java Development Kit)**
- JRE + development tools: `javac`, `javadoc`, `jdb`, `jar`, `jshell`, profilers, etc.
- Everything needed to *develop and run* Java applications
- What developers install

Hierarchy: **JDK ⊃ JRE ⊃ JVM**

Note: In Java 9+, the JDK no longer ships a separate JRE folder. The distinction is more conceptual.

---

### Q4. Explain bytecode.

Bytecode is the **compiled output of Java source code**, stored in `.class` files. It is:

- A set of instructions for the **Java Virtual Machine** (not for any physical CPU)
- Binary format (not human-readable, but disassemblable with `javap -c`)
- **Platform-neutral**: the same `.class` file runs on Windows, Linux, macOS
- More compact and faster to execute than re-interpreting source code

When the JVM starts:
1. ClassLoader loads `.class` files
2. Bytecode verifier checks for safety (no illegal operations, type violations)
3. JVM interprets bytecode OR the **JIT compiler** compiles frequently-executed ("hot") methods to native machine code

Example: `javap -c HelloWorld.class` shows bytecode instructions like `invokevirtual`, `getstatic`, `ldc`, `iload`, `iadd`.

Bytecode also enables tools like **AspectJ** (weaving), **byte-buddy**, **ASM** to modify class behavior at load time or build time.

---

### Q5. What is the Java execution flow?

```
1. Write: HelloWorld.java
2. Compile: javac HelloWorld.java → HelloWorld.class (bytecode)
3. Classloading: JVM's ClassLoader loads .class file
4. Bytecode Verification: Verifier checks legality
5. Execution:
   a. Interpreter: JVM interprets bytecode line by line (slow at start)
   b. JIT Compilation: HotSpot detects hot methods → C1 (client) → C2 (server) compilation
   c. Optimized native code runs directly on CPU
6. GC: Manages heap objects throughout lifetime
7. JVM shutdown: Runtime.getRuntime().addShutdownHook() callbacks, finalizers (deprecated)
```

At senior level: mention **tiered compilation** (Java 8+). Methods start interpreted, promoted to C1 (fast compile, limited optimization), then C2 (slow compile, heavy optimization) based on profiling counters.

---

### Q6. Why is Java strongly typed?

Java is strongly typed because:
- Every variable, parameter, return value must have a declared type
- The compiler enforces type correctness at **compile time**
- No implicit coercions between unrelated types (e.g., you can't assign a `String` to an `int` without explicit conversion)

Benefits:
- **Early error detection**: type errors caught at compile time, not runtime
- **IDE support**: autocomplete, refactoring, find usages all rely on type info
- **Performance**: JVM can optimize knowing exact types
- **Readability**: types serve as documentation

Java is NOT perfectly type-safe – raw types (pre-generics), casts, and reflection can bypass type checking. That's why generics were added in Java 5 and `@SuppressWarnings("unchecked")` exists.

**Strong vs Static**: Java is both. Strongly typed = strict enforcement. Statically typed = types known at compile time (vs dynamically typed like Python where types are checked at runtime).

---

### Q7. What are access modifiers?

Access modifiers control **visibility** of classes, fields, methods, constructors.

| Modifier    | Same Class | Same Package | Subclass (diff pkg) | Everywhere |
|-------------|------------|--------------|---------------------|------------|
| `private`   | ✅         | ❌           | ❌                  | ❌         |
| `default`   | ✅         | ✅           | ❌                  | ❌         |
| `protected` | ✅         | ✅           | ✅                  | ❌         |
| `public`    | ✅         | ✅           | ✅                  | ✅         |

Key senior points:
- **Default (package-private)** is often the right choice for internal implementation classes
- `protected` is primarily for inheritance – use sparingly (violates encapsulation)
- Outer classes can only be `public` or default; inner classes can be any
- Java modules (Java 9+) add another layer: `exports` in `module-info.java` controls which packages are accessible even if `public`

---

### Q8. Difference between public, protected, default, and private?

See table above. Key distinctions:

**private**: Tightest scope. Used for fields to enforce encapsulation. Inner classes can access outer class private members.

**default (package-private)**: No keyword. Useful for classes/methods meant to be used only within a package. Common in well-structured libraries.

**protected**: Confusing for beginners. Allows subclass access, but only to the *subclass's own inherited members*, not to other instances of the parent class in a different package.
```java
// In different package:
class Child extends Parent {
    void method(Parent p, Child c) {
        p.protectedField; // ILLEGAL - different package, p is not 'this'
        c.protectedField; // LEGAL - c is a Child instance
        this.protectedField; // LEGAL
    }
}
```

**public**: API surface. Think carefully before making anything public – it becomes a contract.

---

### Q9. What is a package?

A package is a **namespace** that organizes related classes and interfaces. It maps to a directory structure on the filesystem.

```java
package com.company.service;
```

Packages serve to:
1. **Organize code** logically (domain, layer, feature)
2. **Prevent naming conflicts** – two classes can have the same name if in different packages
3. **Control access** – package-private (default) access
4. **Enable selective imports**

Naming convention: reverse domain name (com.google.guava, org.springframework.core).

In Java 9+, packages are further organized into **modules** (module-info.java). A module controls which packages it exports.

---

### Q10. Why use packages?

- **Modularity**: group related functionality
- **Encapsulation at package level**: package-private classes/methods hide implementation details
- **Avoid name collisions**: `com.company.util.StringUtils` vs `org.apache.commons.lang3.StringUtils`
- **Build tools**: Maven/Gradle use package structure to organize compilation units
- **Javadoc**: packages are the unit of documentation
- **Java modules (9+)**: packages are the unit of module export

Design principle: packages should be **cohesive** (related classes together) and **loosely coupled** (minimal cross-package dependencies). Tools like JDepend, ArchUnit can validate this.

---

### Q11. What is classpath?

The classpath tells the JVM **where to look for .class files and JARs** at runtime (and `javac` at compile time).

```bash
java -cp ".:./lib/*:./build/classes" com.example.Main
```

Classpath elements:
- Directories containing .class files
- JAR files
- ZIP files

In modern development, **build tools (Maven, Gradle) manage the classpath** automatically. Spring Boot's fat JAR bundles all dependencies with a custom classloader.

Issues with classpath:
- **Classpath hell**: multiple JARs with same class – first one on classpath wins
- **ClassNotFoundException**: class not found on classpath
- **NoClassDefFoundError**: class was present at compile time but missing at runtime

Java 9+ modules replace classpath with the **module path**, providing stronger isolation.

---

### Q12. What is an object?

An object is an **instance of a class** – a runtime entity that has:
- **State** (fields/instance variables)
- **Behavior** (methods)
- **Identity** (unique reference/address in heap memory)

```java
Person person = new Person("Alice", 30);
// person -> reference (on stack or as field)
// Person object -> lives on heap
```

Under the hood: `new` allocates memory on the **heap**, calls the constructor, returns a reference. The reference is stored on the stack (for local variables) or in another heap object (for fields).

Object layout in JVM (HotSpot):
- **Mark word** (8 bytes): GC info, lock state, hash code
- **Class pointer** (4-8 bytes): points to Class metadata
- **Instance fields**: actual field values
- **Padding**: alignment to 8-byte boundaries

---

### Q13. Difference between class and object?

| Class | Object |
|-------|--------|
| Blueprint/template | Instance of a class |
| Defined at compile time | Created at runtime |
| Exists once (per ClassLoader) | Can have many instances |
| Stored in Metaspace | Stored in Heap |
| `class Person {}` | `new Person()` |

A class defines *what an object is* (structure, behavior). An object *is* an actual thing with actual data.

Class itself is an object in Java: `Person.class` is a `java.lang.Class` instance, also stored in Metaspace.

---

### Q14. What is encapsulation?

Encapsulation is the **bundling of data (fields) and methods that operate on that data within a class**, while **hiding the internal state** from outside access.

Implementation:
- Fields are `private`
- Access via `public` getters/setters (with validation if needed)
- Internal implementation can change without affecting callers

```java
public class BankAccount {
    private double balance; // hidden
    
    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException();
        this.balance += amount;
    }
    
    public double getBalance() { return balance; }
}
```

Benefits:
1. **Control**: validate data before setting (no `account.balance = -1000`)
2. **Flexibility**: change internal representation without breaking API
3. **Maintainability**: localized changes

Senior note: Records (Java 16+) are immutable data carriers – they encapsulate data without traditional setters. Lombok's `@Value` achieves similar. Encapsulation doesn't always mean getter/setter – it means *controlled access*.

---

### Q15. What is inheritance?

Inheritance allows a class (**subclass/child**) to acquire properties and behaviors of another class (**superclass/parent**).

```java
public class Vehicle {
    protected String brand;
    public void start() { System.out.println("Starting " + brand); }
}

public class Car extends Vehicle {
    private int doors;
    
    @Override
    public void start() {
        super.start();
        System.out.println("Car started with " + doors + " doors");
    }
}
```

Java supports **single inheritance** for classes (unlike C++), but multiple inheritance for interfaces.

Types in Java:
- **Single**: A extends B
- **Multilevel**: A extends B, B extends C
- **Hierarchical**: B extends A, C extends A
- **No multiple class inheritance** (diamond problem avoided)

When NOT to use inheritance: "Is-A" relationship must be genuine. `Stack extends Vector` in Java is a famous mistake (a Stack is NOT a Vector from behavioral standpoint). Prefer composition.

**IS-A test**: Can you substitute subclass wherever superclass is expected? (Liskov Substitution Principle)

---

### Q16. What is polymorphism?

Polymorphism means "**many forms**" – the ability of a reference to behave differently based on the actual object it points to.

**Compile-time polymorphism (static dispatch)**: Method overloading
```java
void print(int x) {}
void print(String s) {}
// Resolved at compile time based on argument types
```

**Runtime polymorphism (dynamic dispatch)**: Method overriding
```java
Animal animal = new Dog(); // Animal reference, Dog object
animal.speak(); // calls Dog.speak(), not Animal.speak()
// Resolved at runtime via virtual method table (vtable)
```

Under the hood: JVM uses **virtual method tables (vtable)**. Each object has a pointer to its class's vtable, which maps method signatures to actual implementations.

Polymorphism enables **Open/Closed Principle**: add new behavior (new subclass) without modifying existing code that works with the base type.

**Covariant return types** (Java 5+): overriding method can return a subtype.

---

### Q17. What is abstraction?

Abstraction means **hiding implementation complexity and exposing only essential features**.

Two mechanisms in Java:
1. **Abstract classes** (0–100% abstract)
2. **Interfaces** (100% abstract pre-Java 8; can have defaults now)

```java
// Abstract class
public abstract class Shape {
    abstract double area(); // must be implemented
    
    void printArea() { // concrete method
        System.out.println("Area: " + area());
    }
}

// Interface
public interface Drawable {
    void draw(); // abstract
    default void drawWithBorder() { draw(); } // default
}
```

Abstraction at design level: programming to interfaces, not implementations. `List<String> list = new ArrayList<>()` – caller doesn't need to know it's an ArrayList.

Abstraction is about **"what" not "how"**. Encapsulation is about **hiding "how"**.

---

### Q18. Difference between method overloading and overriding?

| Aspect | Overloading | Overriding |
|--------|-------------|------------|
| Definition | Same name, different parameters | Same signature in subclass |
| Resolution | Compile time (static dispatch) | Runtime (dynamic dispatch) |
| Inheritance | Not required | Required |
| Return type | Can differ | Must be same or covariant |
| Access modifier | Can differ freely | Can't be more restrictive |
| Static methods | Can be overloaded | Cannot be overridden (hidden) |
| Exception | No restriction | Can't add new checked exceptions |

**Overloading pitfall**: Widening + autoboxing + varargs precedence rules are complex:
```java
void test(long x) {}
void test(Integer x) {}
test(5); // calls test(long) - widening takes priority over autoboxing
```

**Overriding rules**:
- `@Override` annotation is highly recommended (compile-time check)
- `private`, `static`, `final` methods cannot be overridden
- Overriding method can't throw broader checked exceptions
- Covariant return types allowed (return subtype)

---

### Q19. What is static keyword?

`static` means the member **belongs to the class**, not to any instance.

**Static fields**: Shared across all instances; one copy per class
```java
public class Counter {
    private static int count = 0; // class-level
    public Counter() { count++; }
    public static int getCount() { return count; }
}
```

**Static methods**: Can be called without instance; can only access static members directly
**Static blocks**: Run once when class is loaded
**Static inner classes**: No reference to outer class instance
**Static imports**: `import static java.lang.Math.PI;`

Memory: Static fields stored in **Metaspace** (Java 8+), not heap. (In Java 7 and earlier, in PermGen.)

Use cases: utility methods (Math, Collections), constants, factory methods, Singleton pattern, counters.

Pitfalls: Static state is shared globally → thread safety issues, harder to test (can't inject mock), memory leaks (static collections holding references).

---

### Q20. Can a static method be overridden?

**No.** Static methods cannot be overridden – they can only be **hidden**.

```java
class Parent {
    static void greet() { System.out.println("Parent"); }
}
class Child extends Parent {
    static void greet() { System.out.println("Child"); } // HIDING, not overriding
}

Parent p = new Child();
p.greet(); // Prints "Parent" -- resolved at compile time based on reference type
```

This is **method hiding**, not overriding. Static methods are resolved at **compile time** based on the reference type, not at runtime based on the object type.

`@Override` on a static method causes a compile error.

Why? Runtime polymorphism (dynamic dispatch) requires a vtable lookup based on the object's actual class. Static methods are bound to the class, not the object – there's no vtable involvement.

---

### Q21. Why is main() static?

`main()` is `static` so the JVM can invoke it **without creating an instance of the class**.

When the JVM starts, it:
1. Loads the class
2. Calls `public static void main(String[] args)` directly

If `main()` were an instance method, the JVM would need to instantiate the class first – but which constructor to call? What arguments? By making it static, this bootstrapping problem is avoided.

Note (Java 21+): With **unnamed classes** (JEP 445, preview), you can write:
```java
void main() {
    System.out.println("Hello");
}
```
The JVM handles the static entrypoint transparently. But in standard Java, `public static void main(String[] args)` is required.

---

### Q22. What is a constructor?

A constructor is a **special method** used to initialize an object when it is created with `new`.

Properties:
- Same name as the class
- No return type (not even `void`)
- Called automatically by `new`
- Can be overloaded (multiple constructors)
- Can call other constructors with `this()` or `super()` (must be first statement)

```java
public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Delegating constructor
    public Person(String name) {
        this(name, 0); // calls above constructor
    }
}
```

If no constructor is defined, the compiler adds a **default no-arg constructor**. If any constructor is defined, the default is NOT added.

Constructors are NOT inherited. Subclass constructor must call `super(...)` explicitly or the compiler inserts `super()` (which requires a no-arg parent constructor).

---

### Q23. Constructor vs method?

| Constructor | Method |
|-------------|--------|
| Same name as class | Any valid name |
| No return type | Must have return type |
| Called by `new` | Called on object/class |
| Used for initialization | Used for behavior |
| Not inherited | Inherited (if not private) |
| Not called on existing object | Called on existing object |
| `this()`/`super()` allowed | Not applicable |

Constructors ARE methods in bytecode terms (they appear as `<init>` in the class file). The JVM invokes them via `invokespecial`.

Static initializer blocks (`static {}`) run before the constructor, at class loading time.

---

### Q24. Types of constructors?

1. **Default constructor**: No-arg, inserted by compiler if none defined
```java
class Foo {} // compiler adds: Foo() { super(); }
```

2. **No-arg constructor**: Explicitly written with no parameters
```java
class Foo { public Foo() { init(); } }
```

3. **Parameterized constructor**: Takes arguments
```java
class Foo { public Foo(String name, int value) {} }
```

4. **Copy constructor**: Takes an instance of the same class
```java
class Foo {
    String data;
    public Foo(Foo other) { this.data = other.data; }
}
```
Java doesn't have built-in copy constructor support (unlike C++), but it's a common pattern.

5. **Private constructor**: Prevents instantiation (Singleton, utility classes)
```java
class MathUtils { private MathUtils() {} }
```

---

### Q25. What is this keyword?

`this` refers to the **current instance** of the class.

Uses:
1. **Disambiguate field vs parameter**:
```java
public Person(String name) { this.name = name; }
```

2. **Call another constructor** (`this()` – must be first statement):
```java
public Person() { this("Unknown", 0); }
```

3. **Pass current instance** to another method/constructor:
```java
EventBus.register(this);
```

4. **Return current instance** (method chaining / fluent API):
```java
public Builder name(String n) { this.name = n; return this; }
```

`this` is implicit in all instance method calls and field accesses. It's a reference stored in the local variable slot 0 of every instance method.

`this` cannot be used in static context (no instance exists).

---

### Q26. What is super keyword?

`super` refers to the **parent class** (immediate superclass).

Uses:
1. **Access parent class field** (when shadowed by subclass):
```java
class Parent { String name = "Parent"; }
class Child extends Parent {
    String name = "Child";
    void print() { System.out.println(super.name); } // "Parent"
}
```

2. **Call parent class method** (when overridden):
```java
@Override
public String toString() {
    return super.toString() + " [extended]";
}
```

3. **Call parent constructor** (`super()` – must be first statement in constructor):
```java
public Dog(String name) {
    super(name); // calls Animal(String name)
}
```

If subclass constructor doesn't call `super(...)`, compiler inserts `super()` (no-arg) automatically. If parent has no no-arg constructor, compile error.

`super` cannot be used in static context.

---

### Q27. What is a final variable?

A `final` variable can be **assigned only once**.

Types:
1. **final local variable**: Must be assigned before use; can be assigned in any branch as long as it's assigned exactly once
2. **final instance variable**: Must be assigned in constructor or initializer; effectively makes the reference constant (not the object it points to)
3. **final static variable (constant)**: `public static final double PI = 3.14159;`

```java
final List<String> list = new ArrayList<>();
list.add("item"); // OK - modifying the object
list = new LinkedList<>(); // COMPILE ERROR - can't reassign reference
```

Blank final: `private final int x;` – assigned in constructor. Allows different values per instance while being immutable after construction.

JVM optimization: `final` fields get special treatment in Java Memory Model – their values are guaranteed to be visible to all threads without synchronization after the constructor completes (as long as `this` doesn't escape).

---

### Q28. What happens if a class is final?

A `final class` **cannot be subclassed**.

```java
public final class String { ... }
public final class Integer { ... }
```

Uses:
1. **Security**: Prevents malicious subclassing (someone overriding sensitive methods)
2. **Immutability**: String's immutability relies on it being final (can't subclass and add mutability)
3. **Performance**: JVM can devirtualize method calls (no vtable lookup needed)
4. **API stability**: Express that the class is a leaf in the hierarchy

All methods of a final class are implicitly final (they can't be overridden since there are no subclasses).

Sealed classes (Java 17+) are a more nuanced alternative: restrict which classes can extend, rather than completely prohibiting extension.

---

### Q29. Can constructors be final?

**No.** Constructors cannot be `final`, `static`, or `abstract`.

Reasoning:
- `final` prevents overriding, but constructors aren't inherited and can't be overridden anyway
- `static` is meaningless since constructors are always tied to instance creation
- `abstract` requires implementation in subclass, but constructors aren't inherited

If you want to prevent subclassing (which is what you might think `final constructor` achieves), make the **class** `final`.

If you want to prevent instantiation, make the **constructor** `private`.

---

### Q30. What is an immutable object?

An immutable object's **state cannot change after construction**.

How to make a class immutable:
1. Declare class `final` (prevent subclass from adding mutability)
2. All fields `private final`
3. No setters
4. Constructor performs **deep copy** of mutable inputs
5. Accessors return **defensive copies** of mutable fields
6. Don't allow `this` to escape during construction

```java
public final class Money {
    private final BigDecimal amount;
    private final Currency currency;
    
    public Money(BigDecimal amount, Currency currency) {
        this.amount = Objects.requireNonNull(amount);
        this.currency = Objects.requireNonNull(currency);
    }
    
    public BigDecimal getAmount() { return amount; }
    public Currency getCurrency() { return currency; }
    
    public Money add(Money other) {
        // Return new instance instead of modifying
        return new Money(this.amount.add(other.amount), this.currency);
    }
}
```

Benefits: **inherently thread-safe**, can be freely shared, safe as HashMap key, easier to reason about.

Java examples: `String`, `Integer`, `BigDecimal`, `LocalDate`, `InetAddress`.

Records (Java 16+) make immutable data carriers concise.

---

### Q31. Why is String immutable?

String immutability is a deliberate design decision with multiple motivations:

1. **String Pool optimization**: JVM maintains a pool of String literals. If Strings were mutable, changing one reference's string would affect all references pointing to the same pool object.

2. **Thread safety**: Immutable objects are inherently thread-safe. Strings are shared across threads constantly (class names, log messages, etc.).

3. **HashMap/HashSet keys**: String's hashCode is cached. If String were mutable, changing a String key would make it un-findable in a HashMap (the bucket would be wrong).

4. **Security**: Filenames, class names, network connections – if a String passed to a security check could be mutated, the check could be bypassed.

5. **ClassLoader integrity**: Class names are Strings. Mutable class names could break the loading mechanism.

Implementation: `String` stores characters in a `private final char[]` (Java 8) or `private final byte[]` (Java 9+, compact strings). The array is private and never returned directly.

For mutable string operations: use `StringBuilder` (not thread-safe, faster) or `StringBuffer` (thread-safe, slower).

---

### Q32. Difference between == and .equals()?

**`==`**: Compares **references** (memory addresses) for objects, or **values** for primitives.
```java
String a = new String("hello");
String b = new String("hello");
a == b; // false - different objects on heap
```

**`.equals()`**: Compares **logical equality** (content), as defined by the class's `equals()` implementation.
```java
a.equals(b); // true - same content
```

**String pool trap**:
```java
String x = "hello"; // pool
String y = "hello"; // same pool object
x == y; // true (same reference, pool optimization)
x == new String("hello"); // false
```

Default `Object.equals()` is the same as `==`. Classes must override it for meaningful equality.

`null` handling: `"hello".equals(null)` returns false (safe); `null.equals("hello")` throws NPE. Use `Objects.equals(a, b)` for null-safe comparison.

---

### Q33. Why override hashCode() when overriding equals()?

The **general contract** (from `java.lang.Object` Javadoc):
- If `a.equals(b)` is true, then `a.hashCode() == b.hashCode()` MUST be true
- If `a.hashCode() != b.hashCode()`, then `a.equals(b)` MUST be false
- Equal objects must produce the same hash code

**What breaks if you don't?**

HashMap, HashSet, Hashtable use `hashCode()` to find the bucket, then `equals()` to find the exact key:
```java
Map<Person, String> map = new HashMap<>();
Person p1 = new Person("Alice", 30);
map.put(p1, "Engineer");

Person p2 = new Person("Alice", 30); // "equal" to p1
map.get(p2); // returns null if hashCode not overridden!
// p2.hashCode() != p1.hashCode() → different bucket → not found
```

Best practice for `hashCode()`:
- Use all fields used in `equals()`
- Use `Objects.hash(field1, field2, ...)` (easy but slightly slower)
- Or manual: `31 * result + field.hashCode()` (31 is a prime, reduces collisions)
- IDEs and Lombok can generate these

---

### Q34. What are wrapper classes?

Wrapper classes wrap primitive types into objects.

| Primitive | Wrapper |
|-----------|---------|
| `byte`    | `Byte`  |
| `short`   | `Short` |
| `int`     | `Integer` |
| `long`    | `Long`  |
| `float`   | `Float` |
| `double`  | `Double` |
| `char`    | `Character` |
| `boolean` | `Boolean` |

Why needed:
1. **Generics** require objects (`List<Integer>`, not `List<int>`)
2. **Null value**: primitives can't be null; wrappers can
3. **Utility methods**: `Integer.parseInt()`, `Integer.toBinaryString()`, `Double.isNaN()`
4. **Collections**: Only work with objects
5. **Reflection**: Method parameters/return types use wrapper classes

Wrapper classes are **immutable** and have a **cache**:
- `Integer.valueOf(int)` caches values from -128 to 127 (configurable via `-XX:AutoBoxCacheMax`)
- Same for `Byte`, `Short`, `Long`, `Character` (0–127)
- `Boolean.TRUE` and `Boolean.FALSE` are singletons

```java
Integer a = 127; Integer b = 127; a == b; // true (cached)
Integer c = 128; Integer d = 128; c == d; // false (not cached)
```

---

### Q35. Autoboxing vs unboxing?

**Autoboxing**: Automatic conversion from primitive to wrapper by the compiler.
```java
Integer x = 5; // compiler: Integer x = Integer.valueOf(5);
List<Integer> list = new ArrayList<>();
list.add(3); // compiler: list.add(Integer.valueOf(3));
```

**Unboxing**: Automatic conversion from wrapper to primitive.
```java
Integer y = Integer.valueOf(10);
int z = y; // compiler: int z = y.intValue();
int sum = y + 5; // y is unboxed
```

**Pitfalls**:

1. **NullPointerException**:
```java
Integer value = null;
int result = value; // NPE at runtime during unboxing
```

2. **Performance**: Autoboxing in loops creates many objects
```java
Long sum = 0L; // wrapper
for (long i = 0; i < 1_000_000; i++) sum += i; // creates ~1M Long objects!
// Use: long sum = 0L;
```

3. **== comparison confusion**:
```java
Integer a = 1000, b = 1000;
a == b; // false (not cached), use a.equals(b)
```

4. **Overload resolution surprises**: Autoboxing has lower priority than widening in overload selection.

---

### Q36. Difference between primitive and object?

| Aspect | Primitive | Object |
|--------|-----------|--------|
| Types | 8 types (int, long, etc.) | Any class |
| Memory | Stack (local vars) | Heap |
| Default value | 0, false, '\0' | null |
| Nullable | No | Yes |
| Generics | Not directly | Yes |
| Method calls | No | Yes |
| Performance | Faster | Slower (heap alloc, GC) |
| Size | Fixed (int=4 bytes) | Variable (object header overhead) |

Primitives are **value types**: when assigned/passed, the value is copied.
Objects are **reference types**: when assigned/passed, the reference is copied (both point to same object).

Java (pre-Valhalla) has no user-defined value types. **Project Valhalla** (Java 23+ preview) introduces **value classes** – user-defined types with primitive-like performance.

---

### Q37. What is pass-by-value in Java?

Java is **strictly pass-by-value**. Always. No exceptions.

For **primitives**: The value is copied.
```java
void modify(int x) { x = 99; }
int a = 5; modify(a); // a is still 5
```

For **objects**: The **reference value** (memory address) is copied. The method receives a copy of the reference, both pointing to the same object.
```java
void modify(List<String> list) { list.add("new"); }
// This DOES modify the original list - same object is modified

void reassign(List<String> list) { list = new ArrayList<>(); }
// This does NOT affect the caller - local reference is reassigned
```

The confusion: because both references point to the same object, mutations via the reference are visible. But the reference itself is passed by value – you can't make the caller's reference point to a different object.

This is different from pass-by-reference (C++, C# `ref`), where you get a reference to the caller's variable.

---

### Q38. Can Java simulate pass-by-reference?

Java cannot truly do pass-by-reference, but you can simulate effects:

1. **Return the modified value**:
```java
int increment(int x) { return x + 1; }
```

2. **Wrap in a mutable container** (array trick):
```java
void increment(int[] x) { x[0]++; }
int[] val = {5}; increment(val); // val[0] is now 6
```

3. **Use AtomicInteger or a holder class**:
```java
void increment(AtomicInteger x) { x.incrementAndGet(); }
```

4. **Return multiple values via a record/tuple**:
```java
record Pair(int a, int b) {}
Pair swap(int a, int b) { return new Pair(b, a); }
```

These are all workarounds – Java is fundamentally pass-by-value and this is by design (simpler, safer).

---

### Q39. What is heap memory?

The **heap** is the JVM's runtime memory area where **all objects and arrays** are allocated.

Structure (generational GC):
- **Young Generation**: Newly created objects
  - **Eden space**: Where objects are born
  - **Survivor spaces (S0, S1)**: Objects that survive minor GC
- **Old Generation (Tenured)**: Long-lived objects promoted from Young Gen
- **Metaspace** (Java 8+): Class metadata (not technically heap, but JVM-managed)

Heap sizing:
```bash
-Xms512m  # initial heap size
-Xmx4g    # maximum heap size
-Xmn256m  # young generation size
```

Objects on heap are managed by the **Garbage Collector**. Heap is shared across all threads.

**Heap tuning considerations**: Large heap → long GC pauses (with stop-the-world collectors). Balance between heap size and GC pause time. G1/ZGC/Shenandoah designed to minimize pauses.

---

### Q40. What is stack memory?

The **stack** holds **method execution frames**. Each thread has its own stack.

Each frame contains:
- **Local variables** (including primitives and object references)
- **Operand stack** (working area for computations)
- **Reference to runtime constant pool**
- **Return address**

Properties:
- **Thread-private**: No sharing, no synchronization needed
- **LIFO**: Frames pushed on method call, popped on return
- **Fixed size per thread**: Default typically 256KB–1MB (`-Xss` flag)
- **Fast**: Allocation/deallocation is just pointer movement

`StackOverflowError`: Stack exhausted (deep/infinite recursion).

Objects themselves don't live on the stack. Only primitive values and object references live on the stack. However, **escape analysis** (JIT optimization) can allocate objects on the stack if they don't escape the method – called **stack allocation** or **scalar replacement**.

```
Thread 1 Stack:     Thread 2 Stack:     Heap (shared):
[main frame]        [main frame]        [Object A]
[methodA frame]     [processData frame] [Object B]
```
## PART 2: Java Versions & Evolution (Q41–75)

---

### Q41. What features were introduced in Java 5?

Java 5 (2004) was a watershed release:

1. **Generics**: Type-safe collections and classes
2. **Enhanced for-loop (for-each)**: `for (String s : list)`
3. **Autoboxing/unboxing**: Automatic primitive-wrapper conversion
4. **Enums**: `enum Direction { NORTH, SOUTH, EAST, WEST }`
5. **Varargs**: `void method(String... args)`
6. **Annotations**: `@Override`, `@Deprecated`, `@SuppressWarnings`, custom annotations
7. **Static imports**: `import static java.lang.Math.PI`
8. **Concurrent utilities**: `java.util.concurrent` – Executor framework, locks, atomic classes
9. **Formatted I/O**: `printf`, `String.format`
10. **`Iterable` interface**: Enables for-each on custom classes

Java 5 generics are implemented via **type erasure** – generic type info is removed at runtime. This is a compromise for backward compatibility but has implications: can't do `new T()`, `instanceof T`, or `List<String>.class`.

---

### Q42. What is generics?

Generics enable **type-safe, reusable code** by parameterizing types.

```java
// Without generics (Java 1.4)
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0); // Unsafe cast, possible ClassCastException

// With generics (Java 5+)
List<String> list = new ArrayList<>();
list.add("hello");
String s = list.get(0); // No cast needed, type-safe
```

**Type erasure**: At compile time, `List<String>` is checked. At runtime, it becomes `List` (raw type). Generic info is mostly lost.

**Wildcards**:
- `? extends T` (upper bounded): read-only, covariant. "Producer Extends"
- `? super T` (lower bounded): write-capable, contravariant. "Consumer Super"
- **PECS rule** (Producer Extends, Consumer Super)

```java
// Copy elements from src to dest
<T> void copy(List<? super T> dest, List<? extends T> src) {
    for (T item : src) dest.add(item);
}
```

**Bounded type parameters**:
```java
<T extends Comparable<T>> T max(T a, T b) { return a.compareTo(b) > 0 ? a : b; }
```

**Generic methods vs generic classes**: Method can have its own type parameters independent of the class.

Reifiable types: Only raw types, non-generic types, unbounded wildcards are reifiable (accessible at runtime via reflection).

---

### Q43. What is enhanced for-loop?

The enhanced for-loop (for-each) iterates over arrays and `Iterable` implementations:

```java
// Array
int[] numbers = {1, 2, 3};
for (int n : numbers) { System.out.println(n); }

// Iterable
List<String> names = List.of("Alice", "Bob");
for (String name : names) { System.out.println(name); }
```

Under the hood, for `Iterable`, the compiler generates:
```java
Iterator<String> it = names.iterator();
while (it.hasNext()) {
    String name = it.next();
    // body
}
```

For arrays, it generates a traditional index-based loop.

**Limitations**:
- Can't modify the collection during iteration (ConcurrentModificationException)
- Can't access index
- Can't iterate multiple collections simultaneously
- Can't iterate in reverse

For indexed access, use `for (int i = 0; i < list.size(); i++)`. For removal during iteration, use `it.remove()` or `list.removeIf()`.

---

### Q44. Java 7 major features?

1. **try-with-resources**: Auto-closing of `AutoCloseable` resources
2. **Multi-catch**: `catch (IOException | SQLException e)`
3. **Diamond operator**: `List<String> list = new ArrayList<>()` (infer generic type)
4. **String in switch**: `switch (str) { case "value": }`
5. **Numeric literals with underscores**: `1_000_000`, `0xFF_FF`
6. **Binary literals**: `0b1010`
7. **Fork/Join framework**: `ForkJoinPool`, `RecursiveTask`
8. **NIO.2 (java.nio.file)**: `Path`, `Files`, `WatchService`, better file handling
9. **invokedynamic bytecode**: Foundation for lambdas in Java 8

Java 7 Project Coin: Small language improvements collected under "Project Coin".

---

### Q45. What is try-with-resources?

try-with-resources automatically closes resources that implement `AutoCloseable`:

```java
// Java 6 way (verbose, easy to forget closing in finally)
Connection conn = null;
try {
    conn = dataSource.getConnection();
    // use connection
} finally {
    if (conn != null) conn.close(); // can throw too!
}

// Java 7 way
try (Connection conn = dataSource.getConnection();
     PreparedStatement stmt = conn.prepareStatement(sql)) {
    // use conn and stmt
    // both auto-closed in reverse order on exit
}
```

**Close order**: Resources closed in reverse order of opening (stmt closed before conn).

**Suppressed exceptions**: If body throws AND close() throws, the close() exception is *suppressed* and attached to the primary exception:
```java
try {
    Throwable t = e.getSuppressed()[0]; // access suppressed exception
}
```

Java 9 improvement: Can reference effectively-final variables:
```java
Connection conn = getConnection();
try (conn) { // no need to redeclare
    // use conn
}
```

---

### Q46. What is multi-catch exception?

Multi-catch allows catching multiple exception types in one `catch` block:

```java
// Java 6
try { ... }
catch (IOException e) { log(e); throw new ServiceException(e); }
catch (SQLException e) { log(e); throw new ServiceException(e); } // code duplication

// Java 7
try { ... }
catch (IOException | SQLException e) {
    log(e); // DRY
    throw new ServiceException(e);
}
```

**Important rule**: In multi-catch, the variable `e` is implicitly **final** (can't reassign). This makes sense because `e` could be either type.

**Can't catch related exceptions**: `catch (Exception | IOException e)` is illegal because `IOException extends Exception` – it would be redundant.

---

### Q47. Java 8 key features?

Java 8 (2014) was the most transformative release since Java 5:

1. **Lambda expressions**: `(x, y) -> x + y`
2. **Functional interfaces**: `@FunctionalInterface`
3. **Stream API**: Declarative data processing
4. **Optional<T>**: Null-safe value containers
5. **Default and static methods in interfaces**
6. **Method references**: `String::toUpperCase`, `System.out::println`
7. **New Date/Time API (java.time)**: `LocalDate`, `LocalDateTime`, `ZonedDateTime`, `Duration`, `Period`
8. **CompletableFuture**: Async programming
9. **Nashorn JavaScript engine** (deprecated in 11)
10. **Base64 encoding**: `Base64.getEncoder()`
11. **forEach on Iterable**: `list.forEach(System.out::println)`
12. **Map.getOrDefault()`, `Map.computeIfAbsent()`, etc.**
13. **StringJoiner**
14. **Parallel array sorting**: `Arrays.parallelSort()`

Java 8 enabled functional programming patterns in Java and drastically changed how Java code is written.

---

### Q48. What is lambda expression?

A lambda is a **concise anonymous function** that can be passed around as a value.

Syntax: `(parameters) -> expression` or `(parameters) -> { statements; }`

```java
// Old way
Runnable r = new Runnable() {
    @Override
    public void run() { System.out.println("Hello"); }
};

// Lambda
Runnable r = () -> System.out.println("Hello");

// With parameters
Comparator<String> comp = (a, b) -> a.compareTo(b);

// Multi-line
Function<String, String> process = input -> {
    String trimmed = input.trim();
    return trimmed.toUpperCase();
};
```

**Target typing**: Lambda type is inferred from context. The target must be a **functional interface** (exactly one abstract method).

**Variable capture**: Lambdas can capture **effectively final** local variables (not just `final`). Effectively final = not reassigned after initialization.

```java
String prefix = "Hello"; // effectively final
Function<String, String> greet = name -> prefix + " " + name; // OK
prefix = "Hi"; // This would make lambda illegal
```

**Method references** are lambdas in disguise: `String::toUpperCase` = `s -> s.toUpperCase()`

Types of method references:
- Static: `Integer::parseInt`
- Instance (bound): `str::contains` (specific instance)
- Instance (unbound): `String::toUpperCase` (called on parameter)
- Constructor: `ArrayList::new`

---

### Q49. Functional interface?

A functional interface has **exactly one abstract method**. Lambdas can be used wherever a functional interface is expected.

`@FunctionalInterface` annotation: Optional but recommended (compile-time check).

Built-in functional interfaces (`java.util.function`):
| Interface | Signature | Use |
|-----------|-----------|-----|
| `Runnable` | `() -> void` | Run code |
| `Supplier<T>` | `() -> T` | Produce value |
| `Consumer<T>` | `T -> void` | Consume value |
| `Function<T,R>` | `T -> R` | Transform |
| `Predicate<T>` | `T -> boolean` | Test condition |
| `BiFunction<T,U,R>` | `(T,U) -> R` | Two-arg transform |
| `UnaryOperator<T>` | `T -> T` | Transform same type |
| `BinaryOperator<T>` | `(T,T) -> T` | Reduce same type |

Functional interfaces **can** have:
- Default methods
- Static methods
- Methods from `Object` (`equals`, `toString`, etc.) – don't count as abstract

---

### Q50. Predicate vs Function?

**`Predicate<T>`**: Takes T, returns `boolean`
```java
Predicate<String> isLong = s -> s.length() > 10;
isLong.test("Hello World!"); // true

// Composing predicates
Predicate<String> isLongAndUpper = isLong.and(s -> s.equals(s.toUpperCase()));
Predicate<String> isLongOrEmpty = isLong.or(String::isEmpty);
Predicate<String> isShort = isLong.negate();
```

**`Function<T, R>`**: Takes T, returns R
```java
Function<String, Integer> length = String::length;
length.apply("Hello"); // 5

// Composing functions
Function<String, String> trim = String::trim;
Function<String, String> upper = String::toUpperCase;
Function<String, String> trimThenUpper = trim.andThen(upper);
Function<String, String> upperAfterTrim = upper.compose(trim); // same result
```

Use `Predicate` for filtering/testing, `Function` for transforming.

**`BiPredicate<T, U>`**: Two arguments, returns boolean.
**`BiFunction<T, U, R>`**: Two arguments, returns R.

---

### Q51. What is Stream API?

Stream API provides a **declarative, functional approach to processing sequences of elements**.

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

// Imperative
List<String> result = new ArrayList<>();
for (String name : names) {
    if (name.length() > 4) result.add(name.toUpperCase());
}
Collections.sort(result);

// Stream (declarative)
List<String> result = names.stream()
    .filter(name -> name.length() > 4)
    .map(String::toUpperCase)
    .sorted()
    .collect(Collectors.toList());
```

**Key characteristics**:
1. **Lazy**: Intermediate operations don't execute until a terminal operation is called
2. **Single use**: Streams can't be reused
3. **Non-interfering**: Should not modify the source during processing
4. **Stateless** (mostly): Operations should be side-effect-free for parallel safety

**Operations**:
- **Intermediate** (lazy): `filter`, `map`, `flatMap`, `sorted`, `distinct`, `limit`, `skip`, `peek`
- **Terminal** (trigger execution): `collect`, `forEach`, `reduce`, `count`, `findFirst`, `anyMatch`, `allMatch`, `toList()` (Java 16)

**Specialized streams**: `IntStream`, `LongStream`, `DoubleStream` – avoid boxing overhead.

```java
IntStream.range(0, 10).sum(); // 45
IntStream.of(1, 2, 3).average(); // OptionalDouble
```

---

### Q52. How does Stream differ from Collection?

| Collection | Stream |
|-----------|--------|
| Stores data | Processes data |
| In-memory data structure | Computational pipeline |
| Can be traversed multiple times | Single-use |
| Eager: elements computed when added | Lazy: computed on demand |
| Mutate elements | No mutation (produces new stream/result) |
| Has size | May be infinite (`Stream.iterate`, `Stream.generate`) |
| `Iterable` | `BaseStream` |

Key distinction: A Collection **is** data. A Stream **processes** data.

```java
// Infinite stream (possible only with laziness)
Stream.iterate(0, n -> n + 2)
    .filter(n -> n % 6 == 0)
    .limit(5)
    .forEach(System.out::println); // 0, 6, 12, 18, 24
```

Streams can be created FROM collections (`.stream()`), arrays (`Arrays.stream()`), values (`Stream.of()`), files (`Files.lines()`), etc.

---

### Q53. What is Optional?

`Optional<T>` is a container that may or may not contain a value. It's a way to express "nullable return" explicitly in the type system.

```java
// Instead of returning null
public Optional<User> findById(Long id) {
    return users.stream()
        .filter(u -> u.getId().equals(id))
        .findFirst(); // Returns Optional<User>
}

// Usage
Optional<User> user = findById(1L);

// Bad practice (defeats the purpose)
if (user.isPresent()) user.get(); // like null check

// Good practice
user.ifPresent(u -> sendEmail(u.getEmail()));
String name = user.map(User::getName).orElse("Unknown");
User found = user.orElseThrow(() -> new UserNotFoundException(id));
```

**Methods**:
- `isPresent()`, `isEmpty()` (Java 11)
- `get()` (throws if empty – use cautiously)
- `orElse(default)` – always evaluates default
- `orElseGet(supplier)` – lazy, only evaluates if empty
- `orElseThrow(supplier)`
- `map()`, `flatMap()`, `filter()`
- `ifPresent()`, `ifPresentOrElse()` (Java 9)
- `or(supplier)` (Java 9) – return other Optional if empty
- `stream()` (Java 9) – 0 or 1 element stream

**When NOT to use Optional**:
- As method parameter (use overloads or nullability annotations instead)
- As field in a class (not serializable, overhead)
- In collections (use empty collection instead of Optional<List>)

---

### Q54. Java 9 module system (JPMS)?

**Java Platform Module System (JPMS / Project Jigsaw)** – `module-info.java`

```java
module com.company.service {
    requires java.sql;              // depend on module
    requires transitive java.logging; // transitive dependency
    exports com.company.service.api;  // make packages available
    exports com.company.service.impl to com.company.other; // qualified export
    opens com.company.service.model;  // allow reflection
    uses com.company.spi.Plugin;       // ServiceLoader
    provides com.company.spi.Plugin with com.company.service.DefaultPlugin;
}
```

**Goals**:
1. **Strong encapsulation**: Even public types in unexported packages aren't accessible
2. **Reliable configuration**: Explicit dependency graph; missing modules detected at startup
3. **Scalable platform**: Build custom JREs with only needed modules (`jlink`)

**Module types**:
- Named module: Has `module-info.java`
- Unnamed module: Classpath code (backward compatible)
- Automatic module: JAR on module path without `module-info.java`

**Impact in practice**: Most enterprise apps still use classpath (unnamed module) for backward compatibility. Libraries are adding `module-info.java`. Spring Boot has limited module support.

---

### Q55. What is jlink?

`jlink` is a tool to **create custom, minimal JRE images** containing only the modules your application needs.

```bash
jlink --module-path $JAVA_HOME/jmods:mods \
      --add-modules com.example.myapp \
      --output custom-runtime \
      --strip-debug \
      --compress=2
```

Benefits:
- **Smaller deployments**: Runtime can be 20–50MB vs 200MB+ full JDK
- **Faster startup**: Fewer classes to load
- **Security**: Reduced attack surface
- **Docker images**: Much smaller container images

Used heavily in **containerized microservices** and **GraalVM native image** scenarios.

---

### Q56. Java 10 local variable type inference?

Java 10 introduced `var` for local variable type inference:

```java
// Before
ArrayList<Map<String, List<Integer>>> map = new ArrayList<>();
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// With var
var map = new ArrayList<Map<String, List<Integer>>>();
var conn = (HttpURLConnection) url.openConnection();
```

---

### Q57. What is var keyword?

`var` is **not a type** – it's syntactic sugar for type inference in local variables. The compiler infers the type from the right-hand side; the type is still static.

**Where var can be used**:
- Local variable declarations with initializer
- for-loop index and enhanced for-loop
- try-with-resources

**Where var CANNOT be used**:
- Method parameters or return types
- Fields
- `var x = null;` (can't infer from null)
- Lambda parameters (actually CAN in Java 11: `(var x, var y) -> x + y`)

**Senior considerations**:
- Improves readability for complex generic types
- Can *harm* readability with primitives or unclear types: `var result = compute();` – what type is result?
- Good practice: use var when the type is obvious from the right side
- `var` is a reserved type name, not a keyword – `int var = 5;` is still valid (terrible style, but compiles)

---

### Q58. Java 11 features?

Java 11 (2018) – LTS release, major features:

1. **HTTP Client API** (java.net.http) – finalized from Java 9 incubator
2. **String new methods**: `isBlank()`, `lines()`, `strip()`, `stripLeading()`, `stripTrailing()`, `repeat(n)`
3. **Files.readString()`, `Files.writeString()`**
4. **`var` in lambda parameters**: `(var x, var y) -> x + y`
5. **Running single-file programs**: `java HelloWorld.java` without compiling
6. **Epsilon GC**: No-op GC (for testing/benchmarking)
7. **ZGC**: Low-latency GC (experimental)
8. **Nest-based access control**: Inner classes don't need synthetic accessor methods
9. **Removal of Java EE and CORBA modules**: `javax.xml.bind`, `javax.activation` removed (need to add as dependencies)
10. **Removed Nashorn JavaScript engine** (deprecated, finally removed in 15)

`strip()` vs `trim()`: `trim()` removes ASCII whitespace (≤ '\u0020'). `strip()` removes Unicode whitespace (uses `Character.isWhitespace()`).

---

### Q59. Difference between Java 8 and Java 11?

Key differences for a senior developer:

| Area | Java 8 | Java 11 |
|------|--------|---------|
| String API | No `isBlank()`, `lines()`, `strip()` | Added these |
| HTTP Client | HttpURLConnection (old) | java.net.http (modern, async) |
| Local var | No `var` | `var` in lambdas |
| GC | CMS (deprecated), G1 default | ZGC (experimental), Epsilon |
| Single file | Must compile first | `java File.java` directly |
| Java EE | javax.* modules included | Removed (must add as deps) |
| Optional | No `isEmpty()`, `or()`, `ifPresentOrElse()` | Added in 9/10 |
| Collection | No `List.copyOf()` etc. | Added in 10 |
| Modules | Not available | JPMS available (since 9) |

Practically: Java 8 → Java 11 migration requires: removing Java EE dependencies (add explicitly), updating deprecated APIs, checking module compatibility.

---

### Q60. What is HTTP Client API?

`java.net.http` (finalized in Java 11) – modern replacement for `HttpURLConnection`:

```java
HttpClient client = HttpClient.newBuilder()
    .version(HttpClient.Version.HTTP_2)
    .connectTimeout(Duration.ofSeconds(10))
    .build();

HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/users"))
    .header("Accept", "application/json")
    .GET()
    .build();

// Synchronous
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

// Asynchronous
CompletableFuture<HttpResponse<String>> future = client.sendAsync(request, HttpResponse.BodyHandlers.ofString());
future.thenApply(HttpResponse::body).thenAccept(System.out::println);
```

Features:
- HTTP/1.1 and HTTP/2 support
- WebSocket support
- Async (CompletableFuture)
- Reactive streams support
- Authentication, cookie management, redirect policies

---

### Q61. Java 12–13 useful features?

**Java 12**:
- `switch` expressions (preview): `yield` keyword
- `String.indent()`, `String.transform()`
- `Collectors.teeing()` – combine two collectors

**Java 13**:
- `switch` expressions (second preview)
- Text blocks (preview): Multi-line strings

---

### Q62. What are switch expressions?

Switch **expressions** (finalized in Java 14) vs switch **statements**:

```java
// Old switch statement (fall-through bug-prone)
String result;
switch (day) {
    case MONDAY: case TUESDAY: case WEDNESDAY:
        result = "Weekday"; break;
    case FRIDAY: case SATURDAY: case SUNDAY:
        result = "Weekend"; break;
    default: result = "Unknown";
}

// Java 14 switch expression (arrow form)
String result = switch (day) {
    case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> "Weekday";
    case SATURDAY, SUNDAY -> "Weekend";
};

// With blocks and yield
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY -> 7;
    default -> {
        String s = day.toString();
        yield s.length(); // yield returns value
    }
};
```

Benefits:
- **Expression** (produces a value, can be used in assignments)
- **Exhaustiveness**: Compiler checks all cases covered (for enums)
- **No fall-through**: Arrow form doesn't fall through
- **Cleaner**: No `break` statements

Pattern matching in switch (Java 21):
```java
Object obj = ...;
String result = switch (obj) {
    case Integer i -> "Integer: " + i;
    case String s when s.length() > 5 -> "Long string";
    case String s -> "Short string";
    default -> "Other";
};
```

---

### Q63. Java 14 records?

Records (finalized in Java 16) are **immutable data carrier classes** with concise syntax:

```java
// Traditional immutable class (boilerplate)
public final class Point {
    private final int x;
    private final int y;
    public Point(int x, int y) { this.x = x; this.y = y; }
    public int x() { return x; }
    public int y() { return y; }
    @Override public boolean equals(Object o) { ... }
    @Override public int hashCode() { ... }
    @Override public String toString() { ... }
}

// Record
public record Point(int x, int y) {}
// Compiler auto-generates: constructor, accessors x(), y(), equals(), hashCode(), toString()
```

Customization:
```java
public record Range(int min, int max) {
    // Compact constructor (validates)
    Range {
        if (min > max) throw new IllegalArgumentException("min > max");
    }
    
    // Custom method
    public int size() { return max - min; }
    
    // Custom accessor
    @Override public int min() { return Math.abs(min); } // override accessor
}
```

Records can:
- Implement interfaces
- Have static fields/methods
- Have instance methods

Records cannot:
- Extend classes (implicitly extend Record)
- Be extended
- Have instance fields beyond record components
- Be abstract

---

### Q64. Use cases of records?

1. **DTOs (Data Transfer Objects)**: API request/response models
```java
public record UserDTO(String name, String email, int age) {}
```

2. **Value objects** (DDD): Domain primitives
```java
public record Money(BigDecimal amount, Currency currency) {}
public record EmailAddress(String value) {
    EmailAddress { if (!value.contains("@")) throw new IllegalArgumentException(); }
}
```

3. **Multiple return values**:
```java
public record PagedResult<T>(List<T> items, int totalCount) {}
```

4. **Composite map keys**:
```java
record CacheKey(String region, Long id) {} // equals/hashCode auto-generated
Map<CacheKey, Object> cache = new HashMap<>();
```

5. **Intermediate stream results**:
```java
.map(person -> new PersonScore(person.name(), calculateScore(person)))
.filter(ps -> ps.score() > 50)
```

Records work well with **pattern matching** (Java 21):
```java
if (obj instanceof Point(int x, int y)) { // deconstruct
    System.out.println("x=" + x + " y=" + y);
}
```

---

### Q65. Java 15 sealed classes?

Sealed classes (finalized in Java 17) **restrict which classes can extend or implement** a class/interface:

```java
public sealed class Shape
    permits Circle, Rectangle, Triangle {
}

public final class Circle extends Shape {
    private final double radius;
}

public non-sealed class Rectangle extends Shape { // can be freely extended
    private final double width, height;
}

public sealed class Triangle extends Shape
    permits EquilateralTriangle, RightTriangle { // further restricted
}
```

Permitted classes must be: in the same package (or module), and must be `final`, `sealed`, or `non-sealed`.

**Benefits**:
1. **Controlled hierarchy**: Know all possible subtypes at compile time
2. **Pattern matching exhaustiveness**: Compiler knows all cases
3. **Domain modeling**: Express algebraic data types

```java
double area = switch (shape) {
    case Circle c -> Math.PI * c.radius() * c.radius();
    case Rectangle r -> r.width() * r.height();
    case Triangle t -> calculateTriangleArea(t);
    // No default needed - compiler knows all cases!
};
```

---

### Q66. Java 16 pattern matching?

Pattern matching (finalized in Java 16 for `instanceof`):

```java
// Old way
if (obj instanceof String) {
    String s = (String) obj; // redundant cast
    System.out.println(s.length());
}

// Java 16 pattern matching
if (obj instanceof String s) {
    System.out.println(s.length()); // s is in scope here
}

// With condition
if (obj instanceof String s && s.length() > 5) {
    System.out.println("Long string: " + s);
}
```

Pattern matching in switch (Java 21):
```java
static String describe(Object obj) {
    return switch (obj) {
        case Integer i when i < 0 -> "Negative integer: " + i;
        case Integer i -> "Positive integer: " + i;
        case String s -> "String of length " + s.length();
        case int[] arr -> "int array of length " + arr.length;
        case null -> "null";
        default -> "Something else";
    };
}
```

Record patterns (Java 21):
```java
if (obj instanceof Point(int x, int y) p) {
    System.out.println("Point at " + x + ", " + y);
}
```

---

### Q67. Java 17 LTS significance?

Java 17 (September 2021) is an **LTS (Long-Term Support)** release – the most significant LTS since Java 11 and Java 8.

**Why it matters**:
- Oracle provides 8+ years of support
- Most enterprises use LTS releases in production
- Spring Boot 3+ requires Java 17 minimum
- Many frameworks aligned to Java 17 as baseline

**New features finalized in Java 17**:
- Sealed classes (Java 15 preview)
- Pattern matching for instanceof (Java 14 preview)
- Records (Java 14 preview)
- Switch expressions (Java 14)
- Text blocks (Java 13 preview)

**Deprecations/Removals**:
- Applet API deprecated for removal
- Security Manager deprecated
- RMI Activation removed
- Strong encapsulation of JDK internals (Unsafe-like access restricted)

---

### Q68. Garbage collection improvements post Java 8?

| GC | Introduced | Characteristics |
|----|-----------|-----------------|
| G1 (Garbage First) | Java 7, default from 9 | Region-based, aims for predictable pause times |
| ZGC | Java 11 (experimental), 15 (production) | Sub-millisecond pauses, scalable to TBs |
| Shenandoah | Java 12 (RedHat), 15 OpenJDK | Low-pause, concurrent, scale with heap |
| Epsilon | Java 11 | No-op GC, for performance testing |

G1 improvements over time:
- Java 10: Parallel Full GC
- Java 12: Promptly return memory to OS
- Java 14: NUMA-aware memory allocation

ZGC evolution:
- Java 11: 4TB max heap, requires 8-core+
- Java 15: Production-ready
- Java 16: Thread-local handshakes, improved latency
- Java 21: Generational ZGC (default: `-XX:+UseZGC`)

---

### Q69. What are virtual threads?

Virtual threads (Java 21 – Project Loom) are **lightweight threads managed by the JVM**, not the OS.

```java
// Platform thread (expensive, OS-backed)
Thread.ofPlatform().start(() -> handleRequest());

// Virtual thread (cheap, JVM-managed)
Thread.ofVirtual().start(() -> handleRequest());

// With executor
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (Request req : requests) {
        executor.submit(() -> handle(req));
    }
}
```

**Why it matters**:
- Platform threads: ~1MB stack, OS-limited (~thousands)
- Virtual threads: ~few KB, JVM-limited (millions possible)
- Virtual threads **block without blocking OS thread** – when a virtual thread blocks (I/O, sleep), the carrier OS thread is freed to run other virtual threads

**One-thread-per-request model** becomes viable again for high-concurrency I/O workloads.

**Not a replacement for async**: Same mental model as blocking code, but with performance of reactive. You write:
```java
String response = httpClient.get(url); // looks blocking
// but virtual thread is suspended, OS thread is freed
```

Considerations:
- Don't use with `ThreadLocal` that holds expensive resources (use `ScopedValue` instead)
- `synchronized` blocks still pin virtual thread to carrier (use `ReentrantLock` for fine-grained locking)
- Not beneficial for CPU-bound tasks (no concurrency gain)

---

### Q70. Java 19+ Loom overview?

Project Loom delivered over multiple releases:
- **Java 19**: Virtual threads (preview)
- **Java 20**: Virtual threads (second preview), Structured Concurrency (incubator)
- **Java 21**: Virtual threads (GA!), Structured Concurrency (preview), Scoped Values (preview)

**Structured Concurrency** (Java 21 preview):
```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Future<User> user = scope.fork(() -> fetchUser(id));
    Future<Order> order = scope.fork(() -> fetchOrder(id));
    
    scope.join().throwIfFailed();
    
    return new Response(user.resultNow(), order.resultNow());
}
// scope.close() cancels subtasks if any fail
```

Benefits:
- Subtask lifetimes are bound to parent scope
- Error handling and cancellation are structured
- Easier to reason about than CompletableFuture chains

**Scoped Values**: Thread-local alternative designed for virtual threads.

---

### Q71. Impact of newer Java versions on Spring Boot?

- **Spring Boot 3.x** requires **Java 17 minimum** (dropped Java 8/11 support)
- Spring Boot 3.x is based on **Spring Framework 6**, which embraces Java 17 features (records as DTOs, sealed classes, etc.)
- **Virtual threads** in Spring Boot 3.2+: `spring.threads.virtual.enabled=true` enables virtual thread executor
- **GraalVM native image** support in Spring Boot 3 (ahead-of-time compilation)
- **Jakarta EE 10** (renamed from Java EE): `javax.*` → `jakarta.*` namespace migration in Spring Boot 3
- Pattern matching, records, text blocks used in Spring source and application code

---

### Q72. Backward compatibility in Java?

Java has a strong commitment to **backward compatibility**:
- Programs compiled with Java 5 generally run on Java 21 JVM
- `--release` flag: `javac --release 8 Source.java` compiles for Java 8 target
- **Deprecation process**: Features deprecated before removal (usually multiple versions)

Breaking changes DO happen:
- **Strong encapsulation**: `--add-opens` needed for deep reflection
- **Module system**: Some internal APIs (sun.*, com.sun.*) no longer accessible
- **Removed APIs**: CMS GC removed (Java 14), Nashorn (Java 15), Applet API (Java 17)
- **JEE modules removed** in Java 11

**Tooling**:
- `jdeprscan`: Find usage of deprecated APIs
- `--illegal-access=warn`: Identify reflective access that will break in future versions
- Migration guides on OpenJDK site

---

### Q73. Removed features in newer Java?

| Feature | Removed Version |
|---------|----------------|
| PermGen | Java 8 (replaced by Metaspace) |
| Java EE/CORBA modules | Java 11 |
| Nashorn JS engine | Java 15 |
| CMS GC | Java 14 |
| RMI Activation | Java 15 |
| Applet API | Java 17 (deprecated), removal pending |
| Security Manager | Java 17 (deprecated), pending |
| `finalize()` | Java 18 (deprecated), pending |

---

### Q74. How do you choose Java version for production?

Decision factors:

1. **LTS vs non-LTS**: Production → LTS (8, 11, 17, 21). Non-LTS for early adoption.
2. **Framework support**: Spring Boot 3 → Java 17+. Most modern frameworks: Java 11+
3. **Vendor support lifecycle**: Oracle, Amazon Corretto, Eclipse Temurin differ
4. **Team readiness**: Training, code review awareness of new features
5. **Library compatibility**: Third-party JARs may not support newer Java
6. **GC requirements**: If you need ZGC/Shenandoah, Java 15+
7. **Virtual threads**: Need Java 21
8. **Security patches**: Older versions may lack backports

**Current recommendation (2024)**: Java 21 for new projects (LTS, virtual threads, records, sealed classes, pattern matching all stable). Java 17 for projects on Spring Boot 3. Java 11 only if library constraints exist. Java 8 only if legacy systems force it.

---

### Q75. How to migrate legacy Java apps?

**Step-by-step migration approach**:

1. **Assess**: Run `jdeprscan` and `jlink --list-modules`. Identify deprecated API usage, illegal reflective access.

2. **Update dependencies**: Upgrade all third-party libraries to versions compatible with target Java. Add `javax.*` → `jakarta.*` if going to 11+ with Spring Boot 3.

3. **Fix compilation**: Compile with `--release <target>`. Fix compiler errors.

4. **Handle module encapsulation**: Add `--add-opens` JVM flags for necessary reflective access (temporary). Work to remove them over time.

5. **Update build tool**: Maven/Gradle plugins must support target Java.

6. **Test thoroughly**: Behavioral differences can be subtle. Especially concurrency, GC behavior.

7. **Incremental**: Go 8 → 11 → 17 → 21 stepwise rather than jumping.

8. **JVM flags audit**: Some old GC flags may be unrecognized (CMS flags removed). Use `-XX:+PrintFlagsFinal` to validate.

9. **Docker base image**: Update to Java 17/21 base images.

10. **Feature adoption**: After migration, gradually adopt new features (records, var, etc.) during refactoring.

---

## PART 3: Object-Oriented Design & SOLID (Q76–105)

---

### Q76. What are SOLID principles?

SOLID is an acronym for five OOP design principles that make software more maintainable, scalable, and testable:

- **S** – Single Responsibility Principle
- **O** – Open/Closed Principle
- **L** – Liskov Substitution Principle
- **I** – Interface Segregation Principle
- **D** – Dependency Inversion Principle

Introduced by Robert C. Martin ("Uncle Bob"). Not a strict rulebook but guidelines for managing complexity.

---

### Q77. Explain Single Responsibility Principle?

A class should have **only one reason to change** – it should have one primary responsibility.

**Violation**:
```java
class UserService {
    void createUser(User user) { ... } // business logic
    void saveUser(User user) { ... }   // data access
    String generateReport(User user) { ... } // reporting
    void sendEmail(User user) { ... }  // notification
}
```

**Correct**:
```java
class UserService { void createUser(User user) { ... } }
class UserRepository { void save(User user) { ... } }
class UserReportGenerator { String generate(User user) { ... } }
class EmailNotificationService { void sendWelcomeEmail(User user) { ... } }
```

**Senior nuance**: "Reason to change" = stakeholder/actor. Business rules change when product owners request it; DB schema changes when DBAs require it; report format changes when marketing asks. These are different reasons.

SRP doesn't mean "one method per class" – it means one cohesive responsibility. A `UserValidator` validating user fields has one responsibility even if it has many validation methods.

---

### Q78. Open/Closed Principle?

Software entities should be **open for extension, closed for modification**.

**Violation**: Adding a new shape requires modifying existing code:
```java
class AreaCalculator {
    double area(Object shape) {
        if (shape instanceof Circle) return ...; // modify this every time
        if (shape instanceof Rectangle) return ...;
    }
}
```

**Correct**: Extend by adding new implementations:
```java
interface Shape { double area(); }
class Circle implements Shape { public double area() { return Math.PI * r * r; } }
class Rectangle implements Shape { public double area() { return w * h; } }

class AreaCalculator {
    double totalArea(List<Shape> shapes) {
        return shapes.stream().mapToDouble(Shape::area).sum(); // never changes
    }
}
```

**Mechanisms**: Abstraction (interfaces, abstract classes), Strategy pattern, Template Method, Decorator.

**Senior nuance**: 100% OCP is impractical. The goal is to be strategic about which dimensions of change to "close". Use OCP for points of known variability. Getting the abstraction wrong is costly.

---

### Q79. Liskov Substitution Principle?

Objects of a subclass must be **substitutable for objects of the superclass** without altering program correctness.

Formally: If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering any of the desirable properties of the program.

**Classic violation – Rectangle/Square**:
```java
class Rectangle {
    void setWidth(int w) { this.width = w; }
    void setHeight(int h) { this.height = h; }
    int area() { return width * height; }
}

class Square extends Rectangle {
    @Override void setWidth(int w) { this.width = this.height = w; } // violation!
    @Override void setHeight(int h) { this.width = this.height = h; }
}

void test(Rectangle r) {
    r.setWidth(5); r.setHeight(4);
    assert r.area() == 20; // fails for Square!
}
```

**Rules**:
- Preconditions can only be weakened in subtype (accept more)
- Postconditions can only be strengthened in subtype (promise more)
- Invariants of supertype must be maintained
- No new exceptions (or only subtypes of parent's exceptions)

**LSP and design**: Java Stack extends Vector violates LSP – you can call `add(0, element)` on a Stack reference, breaking stack semantics.

---

### Q80. Interface Segregation Principle?

Clients should not be forced to depend on interfaces they don't use.

**Violation**:
```java
interface Worker {
    void work();
    void eat();
    void sleep();
}

class Robot implements Worker {
    void work() { ... }
    void eat() { throw new UnsupportedOperationException(); } // forced!
    void sleep() { throw new UnsupportedOperationException(); }
}
```

**Correct**:
```java
interface Workable { void work(); }
interface Eatable { void eat(); }
interface Sleepable { void sleep(); }

class Human implements Workable, Eatable, Sleepable { ... }
class Robot implements Workable { ... }
```

**Senior nuance**: Don't over-segregate. Splitting into too many tiny interfaces creates interface explosion. Balance based on client needs. ISP is about designing interfaces from the *client's perspective*, not the implementor's.

---

### Q81. Dependency Inversion Principle?

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend on details. Details should depend on abstractions.

**Violation**:
```java
class OrderService { // high-level
    private MySQLOrderRepository repo = new MySQLOrderRepository(); // depends on concrete
}
```

**Correct**:
```java
interface OrderRepository { void save(Order o); }

class MySQLOrderRepository implements OrderRepository { ... }
class MongoOrderRepository implements OrderRepository { ... }

class OrderService { // high-level
    private final OrderRepository repo; // depends on abstraction
    
    public OrderService(OrderRepository repo) { this.repo = repo; } // inject
}
```

DIP enables **Dependency Injection (DI)** – the practice of providing dependencies from outside. Spring is essentially a DI container implementing DIP.

**Inversion**: Traditionally, high-level code creates low-level dependencies. DIP *inverts* this – the framework/container provides them.

---

### Q82. Composition vs inheritance?

**Inheritance**: "IS-A" – extend a class to reuse behavior. Tightly coupled to parent.

**Composition**: "HAS-A" – hold a reference to another object and delegate. Loose coupling.

```java
// Inheritance
class LoggingList extends ArrayList<String> {
    @Override public boolean add(String s) { log(s); return super.add(s); }
    // Problem: Also inherits addAll, which calls add repeatedly
    // If super.addAll() is changed to not use add(), logging breaks silently
}

// Composition (safer)
class LoggingList<E> implements List<E> {
    private final List<E> delegate = new ArrayList<>();
    
    @Override public boolean add(E e) { log(e); return delegate.add(e); }
    @Override public boolean addAll(Collection<? extends E> c) { 
        c.forEach(this::log); return delegate.addAll(c); // explicit control
    }
    // ... other delegations
}
```

---

### Q83. Favor composition over inheritance – why?

1. **Fragile base class problem**: Superclass changes can break subclasses silently
2. **Deep hierarchies**: Hard to understand, test, maintain
3. **Single inheritance limit**: Can't extend multiple classes; composition with multiple
4. **Testing**: Easier to mock a composed dependency than override inherited behavior
5. **Runtime flexibility**: Can change composed object at runtime; inheritance is compile-time fixed
6. **Encapsulation preserved**: With inheritance, subclass may depend on parent's internal implementation details

GoF Gang of Four Design Patterns Principle: "Favor object composition over class inheritance."

When inheritance IS appropriate: genuine IS-A relationship, LSP is satisfied, the framework/API expects extension (Template Method pattern), shallow hierarchy.

---

### Q84. Tight coupling vs loose coupling?

**Tight coupling**: Classes are highly dependent on each other's concrete implementations. Changes to one force changes to others.

**Loose coupling**: Classes interact through abstractions (interfaces). Dependencies are minimized and replaceable.

```java
// Tight - depends on concrete class
class OrderController {
    private EmailService emailService = new SmtpEmailService(); // concrete!
}

// Loose - depends on abstraction
class OrderController {
    private final NotificationService notificationService; // interface
    
    public OrderController(NotificationService service) { // injected
        this.notificationService = service;
    }
}
```

Loose coupling enables:
- **Testability**: Inject mock/stub in tests
- **Flexibility**: Swap implementations without changing dependent code
- **Maintainability**: Changes are localized

Metrics: **Afferent coupling (Ca)** – how many depend on this. **Efferent coupling (Ce)** – how many this depends on. **Instability = Ce / (Ca + Ce)**. Stable components (low instability) should be abstract.

---

### Q85. Cohesion vs coupling?

**Cohesion**: How *related* the elements within a module/class are. **High cohesion = good**.

**Coupling**: How *dependent* modules/classes are on each other. **Low coupling = good**.

Goal: **High cohesion, low coupling**.

Types of cohesion (weakest to strongest):
- Coincidental (random things together – worst)
- Logical (similar logic but unrelated)
- Temporal (used at same time)
- Procedural (sequential steps)
- Communicational (work on same data)
- Sequential (output of one feeds next)
- Functional (single well-defined purpose – best)

Example of poor cohesion:
```java
class UtilityBag {
    void parseXML() {}
    void sendEmail() {}
    void calculateTax() {}
    void compressImage() {}
}
```

These are unrelated – poor cohesion. Split into `XmlParser`, `EmailService`, `TaxCalculator`, `ImageCompressor`.

---

### Q86. What is IS-A vs HAS-A?

**IS-A**: Expressed via inheritance or interface implementation.
```java
class Dog extends Animal {} // Dog IS-A Animal
class Circle implements Shape {} // Circle IS-A Shape
```

**HAS-A**: Expressed via composition (field containing another object).
```java
class Car {
    private Engine engine; // Car HAS-A Engine
    private List<Wheel> wheels; // Car HAS-A Wheels
}
```

Test for IS-A: Substitution test (LSP). Can you use a Dog wherever an Animal is expected? If yes, IS-A is valid. If the subclass breaks parent's contract → relationship is wrong, use HAS-A instead.

---

### Q87. Marker interfaces?

A marker interface has **no methods** – it "marks" a class with metadata.

```java
public interface Serializable {} // marker
public interface Cloneable {}    // marker
public interface Remote {}       // marker
```

The JVM/frameworks check `instanceof` to determine behavior:
```java
if (obj instanceof Serializable) {
    // proceed with serialization
}
```

**Modern alternative**: **Annotations** (`@Serializable`, `@Entity`). More flexible, can carry attributes, don't pollute type hierarchy.

Marker interfaces still have one advantage: **compile-time type checking**. A method `void serialize(Serializable s)` only accepts serializable objects at compile time. An annotation-based approach requires runtime checking.

---

### Q88. Why Serializable is a marker?

`Serializable` signals to the Java **Object Serialization** mechanism (ObjectOutputStream/ObjectInputStream) that a class can be serialized to a byte stream.

No methods needed because:
- The serialization mechanism is implemented in `ObjectOutputStream` using reflection
- It just needs to know "is this class serializable?" – a boolean property
- No specific method contracts are needed (the serialization logic handles everything)

If a class doesn't implement `Serializable` but you try to serialize it: `NotSerializableException` at runtime.

Pitfalls:
- `serialVersionUID` should be declared to control version compatibility
- Transient fields are excluded from serialization
- Custom serialization: implement `writeObject()` / `readObject()` (private methods found by reflection)
- Serialization security issues: constructor bypass, deserialization gadget chains (RCE)

Modern alternatives: Jackson JSON, Protobuf, Avro – all more flexible and safer than Java serialization.

---

### Q89. Abstract class vs interface?

| Aspect | Abstract Class | Interface |
|--------|---------------|-----------|
| Instantiation | No | No |
| Multiple inheritance | No (single) | Yes (multiple) |
| Instance fields | Yes | No (only static final) |
| Constructors | Yes | No |
| Method implementations | Yes (can mix) | Default/static only (Java 8+) |
| Access modifiers | Any | Methods public by default |
| `extends` / `implements` | `extends` | `implements` |
| State | Can have mutable state | Cannot have instance state |

**Use abstract class when**:
- Sharing code among closely related classes
- Non-public members needed
- Need to manage state
- Template Method pattern

**Use interface when**:
- Unrelated classes implement it (Comparable, Serializable)
- Multiple inheritance of type needed
- Defining a type that doesn't care about implementation hierarchy

**Java 8+ blurred the lines**: Default methods in interfaces allow code sharing. But interfaces still can't have instance state.

---

### Q90. Multiple inheritance in Java?

Java **doesn't support multiple class inheritance** but supports **multiple interface inheritance**.

```java
interface Flyable { default void move() { System.out.println("flying"); } }
interface Swimmable { default void move() { System.out.println("swimming"); } }

class Duck implements Flyable, Swimmable {
    @Override public void move() {
        Flyable.super.move(); // must resolve ambiguity
    }
}
```

Multiple class inheritance is avoided to prevent the **Diamond Problem** and complexity in method resolution.

For interfaces: if two interfaces have the same default method, implementing class MUST override it (compiler forces resolution).

Rule of thumb for multiple interface conflict resolution:
1. Class/override wins over interface
2. More specific interface wins
3. Must explicitly resolve ambiguity

---

### Q91. Default methods in interface?

Introduced in Java 8 to allow **adding methods to interfaces without breaking existing implementations**.

```java
interface Collection<E> {
    // New in Java 8 - without default, all existing implementations break
    default Stream<E> stream() {
        return StreamSupport.stream(spliterator(), false);
    }
    
    default void forEach(Consumer<? super E> action) {
        for (E e : this) action.accept(e);
    }
}
```

Use cases:
1. **Evolving APIs**: Add new methods without breaking existing code
2. **Mixin-like behavior**: Provide optional implementations
3. **Interface adapter pattern**: Interface with default no-ops; class overrides only needed

Pitfalls:
- Diamond problem must be resolved explicitly
- Default methods complicate interface design ("fat interface")
- They DO have implementations but NOT state – can call other interface methods and parameters

Static methods in interfaces (Java 8): `interface Comparator<T> { static <T> Comparator<T> naturalOrder() {...} }`

Private methods in interfaces (Java 9): Share code between default methods.

---

### Q92. Diamond problem in Java?

The diamond problem occurs in multiple inheritance when a class inherits from two classes that share a common ancestor with a conflicting method.

```
    A (move())
   / \
  B   C (both inherit/override move())
   \ /
    D (which move() to use?)
```

Java solves this for classes by **not allowing multiple class inheritance**.

For interfaces (Java 8+ with defaults):
```java
interface A { default void hello() { print("A"); } }
interface B extends A { default void hello() { print("B"); } }
interface C extends A { default void hello() { print("C"); } }

class D implements B, C {
    // Must override to resolve ambiguity
    @Override public void hello() {
        B.super.hello(); // explicit
    }
}
```

The rule: A **class override** always wins over interface defaults. A **more specific interface** wins over less specific. Otherwise, must manually resolve.

---

### Q93. When to use abstract class?

1. **Template Method Pattern**: Skeleton algorithm with steps for subclasses to fill in
```java
abstract class DataProcessor {
    // Template method
    final void process() { readData(); processData(); writeData(); }
    
    abstract void readData();
    abstract void processData();
    void writeData() { /* default impl */ }
}
```

2. **Common state and behavior**: Subclasses share fields and some implementations
```java
abstract class Vehicle {
    protected String brand; // shared state
    protected int year;
    
    public abstract void fuelType();
    
    public String describe() { // shared behavior
        return brand + " (" + year + ")";
    }
}
```

3. **Partial implementation**: Provide some but not all methods

4. **When you want to control object creation**: `protected` constructors, factory methods in abstract class

5. **Close hierarchy**: Abstract class hints "these are the related implementations"

---

### Q94. When to use interface?

1. **Defining a contract** for unrelated classes (Comparable, Serializable, Runnable)
2. **Multiple inheritance of type** needed
3. **Decoupling** – program to interfaces, not implementations
4. **Plugin/extension points**: `List`, `Queue`, `Map` – code works with any implementation
5. **Dependency injection** – inject any implementation at runtime
6. **Mocking in tests** – easier to mock interfaces than classes
7. **Functional interfaces** – for lambdas

Rule of thumb: **Start with interface** unless you have a clear need for abstract class (shared state, template method).

In API design: expose interfaces, hide implementations. `List<T> createList()` not `ArrayList<T> createList()`.

---

### Q95. What is factory pattern?

Factory Pattern provides a way to **create objects without specifying the exact concrete class**.

**Simple Factory** (not a GoF pattern):
```java
class ShapeFactory {
    public static Shape create(String type) {
        return switch (type) {
            case "circle" -> new Circle();
            case "rectangle" -> new Rectangle();
            default -> throw new IllegalArgumentException();
        };
    }
}
```

**Factory Method** (GoF):
```java
abstract class Application {
    abstract Button createButton(); // factory method
    
    void render() {
        Button btn = createButton(); // uses factory method
        btn.paint();
    }
}

class WindowsApp extends Application {
    @Override Button createButton() { return new WindowsButton(); }
}
```

**Abstract Factory**:
```java
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WindowsFactory implements GUIFactory { ... }
class MacFactory implements GUIFactory { ... }
```

Use cases: Spring's `BeanFactory`, `ConnectionFactory`, JDBC `DriverManager.getConnection()`, `Calendar.getInstance()`.

---

### Q96. Singleton pattern pitfalls?

```java
public class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) instance = new Singleton(); // NOT thread safe!
        return instance;
    }
}
```

**Pitfalls**:

1. **Thread safety**: Multiple threads may create multiple instances
2. **Testing**: Difficult to mock/replace; carries state between tests
3. **Global state**: Implicit dependency; hard to trace
4. **Classloader issues**: Multiple classloaders → multiple instances
5. **Serialization**: Deserializing creates new instance (override `readResolve()`)
6. **Reflection**: `getDeclaredConstructor().setAccessible(true)` can bypass private constructor

---

### Q97. Thread-safe singleton?

```java
// Approach 1: Synchronized method (slow)
public static synchronized Singleton getInstance() { ... }

// Approach 2: Double-checked locking (Java 5+, needs volatile)
public class Singleton {
    private volatile static Singleton instance;
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

// Approach 3: Initialization-on-demand holder (best - lazy, thread-safe, no sync overhead)
public class Singleton {
    private static class Holder {
        static final Singleton INSTANCE = new Singleton(); // initialized when Holder is loaded
    }
    public static Singleton getInstance() { return Holder.INSTANCE; }
}

// Approach 4: Enum (simplest, handles serialization and reflection)
public enum Singleton {
    INSTANCE;
    public void doSomething() { ... }
}
```

**Best practice**: Enum singleton (Josh Bloch recommendation) or Holder idiom. In Spring: just use `@Bean` – Spring manages singleton scope.

---

### Q98. Builder pattern use case?

Builder separates **complex object construction** from its representation.

```java
// Without builder (constructor hell)
User user = new User("Alice", "alice@email.com", 30, "New York", true, "ADMIN", null, null);

// With builder
User user = User.builder()
    .name("Alice")
    .email("alice@email.com")
    .age(30)
    .city("New York")
    .active(true)
    .role("ADMIN")
    .build();
```

Use when:
1. Many constructor parameters (especially optional ones)
2. Immutable object with many fields
3. Step-by-step construction
4. Complex validation before construction

Implementation styles:
- Classic GoF: Director + ConcreteBuilder
- Fluent/chained (most common in Java): `return this` from setters
- Lombok `@Builder`: Auto-generates builder
- Record with `with` methods pattern

---

### Q99. Prototype pattern?

Create new objects by **copying (cloning) existing objects**.

```java
interface Prototype {
    Prototype clone();
}

class ConcretePrototype implements Cloneable {
    private List<String> items;
    
    @Override protected ConcretePrototype clone() {
        try {
            ConcretePrototype clone = (ConcretePrototype) super.clone();
            clone.items = new ArrayList<>(this.items); // deep copy mutable fields
            return clone;
        } catch (CloneNotSupportedException e) { throw new RuntimeException(e); }
    }
}
```

Java's `Cloneable` + `Object.clone()` is notoriously broken:
- `clone()` is protected in Object
- `Cloneable` is a marker but `Object.clone()` must still be overridden
- Default clone is **shallow** – must manually deep copy mutable fields

Modern alternative: Copy constructor, `BeanUtils.copyProperties()`, serialization-based copy, or just `new` + manual copying.

Use cases: Creating objects when construction is expensive, creating snapshots/undo functionality.

---

### Q100. Strategy pattern?

Define a family of algorithms, encapsulate each, and make them **interchangeable**.

```java
interface SortStrategy {
    void sort(int[] data);
}

class BubbleSort implements SortStrategy {
    public void sort(int[] data) { ... }
}

class QuickSort implements SortStrategy {
    public void sort(int[] data) { ... }
}

class DataProcessor {
    private SortStrategy strategy;
    
    public DataProcessor(SortStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void setStrategy(SortStrategy s) { this.strategy = s; }
    
    public void process(int[] data) {
        strategy.sort(data);
        // ... more processing
    }
}

// Usage
DataProcessor processor = new DataProcessor(new QuickSort());
// Change at runtime:
processor.setStrategy(new BubbleSort());
```

With Java 8: Strategy = functional interface! `Function`, `Predicate`, `Comparator` are all strategies.

Used heavily in Spring: `ResourceLoader`, `TransactionManager`, `PasswordEncoder`, `CacheManager`.

---

### Q101. Observer pattern?

Defines a **one-to-many dependency**: when one object changes state, all dependents are notified automatically.

```java
interface Observer { void update(String event); }

class EventBus {
    private List<Observer> observers = new ArrayList<>();
    
    public void subscribe(Observer o) { observers.add(o); }
    public void unsubscribe(Observer o) { observers.remove(o); }
    
    public void publish(String event) {
        observers.forEach(o -> o.update(event));
    }
}
```

Java built-in: `java.util.Observable` (deprecated Java 9), `java.util.EventListener`.

Modern Java: `PropertyChangeListener`, Spring's `ApplicationEventPublisher`:
```java
@Component
class OrderService {
    @Autowired ApplicationEventPublisher publisher;
    
    public void createOrder(Order order) {
        // ...
        publisher.publishEvent(new OrderCreatedEvent(order));
    }
}

@Component
class EmailService {
    @EventListener
    public void onOrderCreated(OrderCreatedEvent event) { sendEmail(event.getOrder()); }
}
```

Reactive: Observer pattern at scale = Reactive Streams (RxJava, Project Reactor).

---

### Q102. Decorator pattern?

Dynamically **add responsibilities to objects** without modifying their class.

```java
interface Coffee { double cost(); String description(); }

class SimpleCoffee implements Coffee {
    public double cost() { return 1.0; }
    public String description() { return "Coffee"; }
}

abstract class CoffeeDecorator implements Coffee {
    protected Coffee wrapped;
    public CoffeeDecorator(Coffee c) { this.wrapped = c; }
}

class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee c) { super(c); }
    public double cost() { return wrapped.cost() + 0.5; }
    public String description() { return wrapped.description() + ", Milk"; }
}

// Usage
Coffee coffee = new MilkDecorator(new MilkDecorator(new SimpleCoffee()));
// Cost: 2.0, Description: "Coffee, Milk, Milk"
```

Java IO is decorator pattern:
```java
new BufferedInputStream(new FileInputStream("file.txt"))
new PrintWriter(new BufferedWriter(new FileWriter("out.txt")))
```

Spring's `BeanDefinitionDecorator`, cache proxies, transaction proxies.

---

### Q103. Chain of Responsibility?

Pass a request along a **chain of handlers** until one handles it.

```java
abstract class Handler {
    protected Handler next;
    
    public Handler setNext(Handler next) { this.next = next; return next; }
    
    public abstract void handle(Request request);
}

class AuthHandler extends Handler {
    public void handle(Request request) {
        if (!request.isAuthenticated()) { System.out.println("Auth failed"); return; }
        if (next != null) next.handle(request);
    }
}

class LoggingHandler extends Handler {
    public void handle(Request request) {
        log(request);
        if (next != null) next.handle(request);
    }
}

// Setup chain
Handler chain = new AuthHandler();
chain.setNext(new LoggingHandler()).setNext(new BusinessHandler());
chain.handle(request);
```

Used in: Servlet filters, Spring Security filter chain, Spring MVC interceptors, logging framework handlers.

---

### Q104. Command pattern?

Encapsulate a **request as an object**, allowing parameterization, queuing, logging, and undo.

```java
interface Command {
    void execute();
    void undo();
}

class TextEditor {
    StringBuilder text = new StringBuilder();
    
    void appendText(String s) { text.append(s); }
    void deleteText(int from, int to) { text.delete(from, to); }
}

class AppendCommand implements Command {
    private TextEditor editor;
    private String text;
    
    public AppendCommand(TextEditor e, String t) { editor = e; text = t; }
    public void execute() { editor.appendText(text); }
    public void undo() { editor.deleteText(editor.text.length() - text.length(), editor.text.length()); }
}

class CommandHistory {
    private Deque<Command> history = new ArrayDeque<>();
    
    public void executeCommand(Command cmd) { cmd.execute(); history.push(cmd); }
    public void undoLastCommand() { if (!history.isEmpty()) history.pop().undo(); }
}
```

Used in: Transaction management, Undo/Redo, Job queuing, Spring Batch steps.

---

### Q105. Anti-patterns in Java?

1. **God Class**: One class that does everything. Solution: decompose.

2. **Anemic Domain Model**: Domain objects are just data bags; all logic in services. Solution: Move logic into domain objects (rich domain model).

3. **Singleton overuse**: Using singletons where DI would be better.

4. **String concatenation in loops**: `str += value` in loop creates O(n²) objects. Use `StringBuilder`.

5. **Catching Exception/Throwable broadly**: Swallows unexpected errors. Catch specific exceptions.

6. **Returning null**: Causes NPE. Return Optional, empty collection, or throw exception.

7. **Premature optimization**: Micro-optimizing before profiling. Profile first.

8. **Magic numbers/strings**: `if (status == 3)` – use constants or enums.

9. **Long parameter lists**: `void create(String a, String b, int c, boolean d, ...)` – use builder or parameter objects.

10. **Dead code**: Unused methods/fields. Remove or it creates confusion.

11. **Incorrect synchronization**: `if (map != null) { synchronized(map) {} }` – checking null outside sync.

12. **Not closing resources**: Memory/connection leaks. Use try-with-resources.

13. **Excessive inheritance**: Deep hierarchies. Prefer composition.

14. **Static state abuse**: Static mutable state is global, untestable, thread-unsafe.
## PART 4: Collections Framework (Q106–150)

---

### Q106. Hierarchy of Collections?

```
Iterable<T>
  └── Collection<T>
        ├── List<T>
        │     ├── ArrayList
        │     ├── LinkedList
        │     ├── Vector (legacy)
        │     │     └── Stack (legacy)
        │     └── CopyOnWriteArrayList
        ├── Set<T>
        │     ├── HashSet
        │     │     └── LinkedHashSet
        │     ├── TreeSet (SortedSet → NavigableSet)
        │     ├── ConcurrentSkipListSet
        │     └── CopyOnWriteArraySet
        └── Queue<T>
              ├── LinkedList
              ├── PriorityQueue
              ├── ArrayDeque
              └── BlockingQueue (interface)
                    ├── LinkedBlockingQueue
                    ├── ArrayBlockingQueue
                    └── PriorityBlockingQueue

Map<K,V> (does NOT extend Collection)
  ├── HashMap
  │     └── LinkedHashMap
  ├── TreeMap (SortedMap → NavigableMap)
  ├── Hashtable (legacy)
  │     └── Properties
  ├── WeakHashMap
  ├── IdentityHashMap
  ├── EnumMap
  └── ConcurrentHashMap
```

---

### Q107. Difference between List, Set, Map?

| | List | Set | Map |
|-|------|-----|-----|
| Duplicates | Allowed | Not allowed | Keys unique, values allowed |
| Order | Insertion order maintained | Depends on impl | Depends on impl |
| Null | Allowed (multiple) | One null (HashSet) | One null key (HashMap) |
| Interface | `List<E>` | `Set<E>` | `Map<K,V>` |
| Access | Index (O(1) for ArrayList) | No index | By key |
| Extends | Collection | Collection | Nothing (standalone) |

---

### Q108. ArrayList vs LinkedList?

| | ArrayList | LinkedList |
|-|-----------|-----------|
| Internal | Dynamic array | Doubly-linked list |
| Random access `get(i)` | O(1) | O(n) |
| Insert/delete at middle | O(n) – shift elements | O(1) – just relink nodes |
| Insert/delete at end | O(1) amortized | O(1) |
| Insert/delete at beginning | O(n) | O(1) |
| Memory | Compact (array) | More overhead (node objects) |
| Implements | List | List, Deque, Queue |
| Cache performance | Better (contiguous memory) | Poor (scattered nodes) |

**Default choice**: `ArrayList`. Use `LinkedList` only when frequent O(1) insertions/deletions at head/middle with iteration. In practice, ArrayList is almost always faster due to CPU cache effects even for some "LinkedList scenarios".

---

### Q109. When to use LinkedList?

LinkedList excels as a **Deque (double-ended queue)**:
```java
LinkedList<String> deque = new LinkedList<>();
deque.addFirst("first"); // O(1)
deque.addLast("last");   // O(1)
deque.removeFirst();      // O(1)
deque.removeLast();       // O(1)
```

Use cases:
1. **Queue/Deque implementation**: Where you frequently add/remove from both ends
2. **LRU Cache**: Remove from tail, add to head (but `LinkedHashMap` is usually better)
3. **Stack with frequent push/pop at head**

**Prefer `ArrayDeque`** over `LinkedList` for most queue/deque scenarios – ArrayDeque is faster (array-based, better cache behavior) and uses less memory.

---

### Q110. Vector vs ArrayList?

Both are resizable arrays, but:
| | Vector | ArrayList |
|-|--------|----------|
| Thread safety | Synchronized (all methods) | Not synchronized |
| Performance | Slower (sync overhead) | Faster |
| Growth | Doubles by default | Grows by 50% |
| Legacy | Java 1.0 | Java 1.2 (Collections) |
| Recommendation | Legacy/avoid | Preferred |

**Vector is legacy** – use `ArrayList` + explicit synchronization (`Collections.synchronizedList()`) or `CopyOnWriteArrayList` for concurrent use.

---

### Q111. Why Vector is legacy?

Vector synchronizes *every* method at the object level, even when no concurrent access is happening. This is:
1. **Too coarse-grained**: Even iteration requires locking every element access
2. **Not composable**: `if (!v.isEmpty()) v.get(0)` is still NOT thread-safe (race between isEmpty and get)
3. **Performance penalty**: Even single-threaded code pays sync cost

Modern alternatives:
- `ArrayList` for non-concurrent
- `CopyOnWriteArrayList` for read-heavy concurrent
- `Collections.synchronizedList(new ArrayList<>())` for simple sync need
- `ConcurrentLinkedDeque` for lock-free concurrent deque

---

### Q112. HashMap internal working?

HashMap uses an **array of buckets** (Node[]) with **linked list** (Java 7) or **red-black tree** (Java 8+ when chain length ≥ 8).

```
Array (buckets):
[0] -> null
[1] -> Node("Alice", 30) -> Node("Bob", 25)  // collision - same bucket
[2] -> Node("Charlie", 35)
[3] -> null
...
[15] -> Node("Dave", 28)
```

**put(key, value) flow**:
1. `key.hashCode()` computed
2. Hash spread: `h ^ (h >>> 16)` (reduces clustering)
3. Bucket index: `hash & (capacity - 1)` (bitwise AND, fast when capacity is power of 2)
4. If bucket empty: insert new Node
5. If bucket has entries: iterate/compare with `equals()`
   - Key found: update value
   - Key not found: append (add to tree if chain ≥ 8)

**get(key) flow**:
1. Compute hash → bucket index
2. Traverse nodes in bucket comparing with `equals()`
3. Return value or null

**Treeification** (Java 8): When a single bucket's linked list reaches 8 nodes AND capacity ≥ 64, converts to red-black tree → O(log n) vs O(n) for long chains.

---

### Q113. HashMap capacity & load factor?

**Default capacity**: 16 (always power of 2)
**Default load factor**: 0.75

**Threshold = capacity × load factor**. When entries exceed threshold, HashMap **rehashes** (creates new array of double size, re-inserts all entries).

Example: Default map with 16 capacity, threshold = 12. When 13th entry is added, resize to 32.

**Why power of 2?** `hash & (n-1)` is faster than `hash % n` and distributes evenly.

**Why 0.75?** Balance between time (fewer collisions, bigger array) and space (fewer rehashes, smaller array). Below 0.6 = too many empty buckets. Above 0.85 = too many collisions.

**Pre-sizing**: If you know approximate size, provide it:
```java
Map<String, String> map = new HashMap<>(expectedSize / 0.75 + 1);
// Or use Guava: Maps.newHashMapWithExpectedSize(expectedSize)
```

---

### Q114. Collision handling in HashMap?

Collision: two keys hash to the same bucket.

**Java 7**: **Separate chaining with linked list**. New entries added to head (causes a subtle ABA problem in concurrent use that leads to infinite loops in multi-threaded HashMap – don't use HashMap from multiple threads!).

**Java 8**: 
- Linked list for ≤ 8 nodes per bucket
- **Red-black tree** for > 8 nodes (if capacity ≥ 64)
- Tree degrades back to list when nodes ≤ 6

**Why switch to tree?** In worst case (all keys hash to same bucket), linked list = O(n). Tree = O(log n). Mitigates hash DoS attacks where attacker crafts many keys with same hash.

String's `hashCode()` is deterministic and used to be exploitable – Java uses random hash seed (`-XX:+UseStringDeduplication`) to mitigate.

---

### Q115. HashMap vs Hashtable?

| | HashMap | Hashtable |
|-|---------|-----------|
| Thread safety | Not synchronized | All methods synchronized |
| Null keys | One null key allowed | No null keys |
| Null values | Allowed | Not allowed |
| Performance | Faster | Slower |
| Legacy | No | Yes (Java 1.0) |
| Iterator | Fail-fast | Enumerator (old API) |
| Extends | AbstractMap | Dictionary (ancient) |

**Hashtable is legacy** – use `ConcurrentHashMap` for thread-safe map (much better than Hashtable).

---

### Q116. HashMap vs ConcurrentHashMap?

| | HashMap | ConcurrentHashMap |
|-|---------|-------------------|
| Thread safety | No | Yes |
| Null keys | Yes | No |
| Locking | None | Segment-level (Java 7) / CAS + bucket-level (Java 8) |
| Performance (single-thread) | Faster | Slightly slower |
| Performance (multi-thread) | Dangerous | Designed for it |
| Concurrent reads | Unsafe (may see inconsistent state) | Safe, non-blocking |
| Iterator | Fail-fast | Weakly consistent |

**Java 8 ConcurrentHashMap internals**: No segment locks. Uses CAS for most operations. Only locks individual buckets during tree restructuring. `size()` uses `LongAdder`-like approach.

**Key methods**:
- `putIfAbsent(k, v)`: Atomic put-if-missing
- `computeIfAbsent(k, fn)`: Compute and store if missing
- `merge(k, v, fn)`: Atomic merge
- `compute(k, fn)`: Atomic compute

---

### Q117. TreeMap vs HashMap?

| | HashMap | TreeMap |
|-|---------|---------|
| Internal structure | Hash table | Red-black tree |
| Ordering | No guaranteed order | **Sorted by key** |
| Performance | O(1) average | O(log n) |
| Null keys | One allowed | No null keys (comparison) |
| Navigation | No | `floorKey`, `ceilingKey`, `subMap`, `headMap`, `tailMap` |
| Implements | Map | SortedMap, NavigableMap |

Use TreeMap when:
- Need keys in sorted order
- Need range queries: "all entries with key between 100 and 200"
- Need floor/ceiling key operations

```java
TreeMap<Integer, String> scores = new TreeMap<>();
scores.subMap(70, 100); // entries with keys 70-99
scores.floorKey(85); // highest key ≤ 85
scores.ceilingKey(70); // lowest key ≥ 70
scores.headMap(50); // keys < 50
```

---

### Q118. LinkedHashMap use cases?

`LinkedHashMap` maintains **insertion order** (or access order).

```java
// Insertion order (default)
Map<String, Integer> map = new LinkedHashMap<>();
map.put("banana", 2); map.put("apple", 1); map.put("cherry", 3);
// Iterates: banana, apple, cherry

// Access order (LRU behavior)
Map<String, Integer> lru = new LinkedHashMap<>(16, 0.75f, true) {
    @Override protected boolean removeEldestEntry(Map.Entry<String, Integer> eldest) {
        return size() > MAX_CACHE_SIZE; // evict oldest access
    }
};
```

Use cases:
1. **Preserve insertion order** in map iteration
2. **LRU Cache** (access-order mode + removeEldestEntry)
3. **Ordered JSON/config properties**
4. **Event logging** where insertion order matters

---

### Q119. Why Map doesn't extend Collection?

Conceptual mismatch:
- `Collection<E>` stores **elements**
- `Map<K,V>` stores **key-value pairs**

If `Map` extended `Collection`, what would `E` be? `Map.Entry<K,V>`? But then:
- `add(Map.Entry<K,V>)` doesn't make sense vs `put(K, V)`
- `contains(Object)` – contains key? value? entry?
- `remove(Object)` – remove by key? value?
- `size()` – conflicts in contract

The operations and semantics are fundamentally different. Map provides `entrySet()`, `keySet()`, `values()` which return Collection views.

Josh Bloch (Java Collections designer): "Maps are not proper Collections because the element concept doesn't fit the Map abstraction."

---

### Q120. Set internal uniqueness?

**HashSet** is backed by a **HashMap** (values are a dummy `PRESENT` object):
```java
public class HashSet<E> {
    private transient HashMap<E, Object> map;
    private static final Object PRESENT = new Object();
    
    public boolean add(E e) { return map.put(e, PRESENT) == null; }
    public boolean contains(Object o) { return map.containsKey(o); }
}
```

Uniqueness enforced via `hashCode()` and `equals()`:
1. Compute `hashCode()` → find bucket
2. Compare with `equals()` against each element in bucket
3. If match found: not added (duplicate)
4. If no match: added

This is why **overriding `equals()` without `hashCode()`** in Set elements causes duplicates:
```java
class Point { int x, y; }
// Without hashCode override:
Set<Point> set = new HashSet<>();
set.add(new Point(1,1));
set.add(new Point(1,1)); // Different hashCode! Different bucket! Both added!
```

---

### Q121. HashSet vs TreeSet?

| | HashSet | TreeSet |
|-|---------|---------|
| Order | No order | Sorted (natural/comparator) |
| Performance | O(1) add/remove/contains | O(log n) |
| Null | Allows one null | No null (can't compare) |
| Backed by | HashMap | TreeMap |
| Interface | Set | SortedSet, NavigableSet |
| Use case | Fast lookup, no order needed | Sorted unique elements, range queries |

```java
// TreeSet range operations
TreeSet<Integer> set = new TreeSet<>(Set.of(1, 5, 3, 7, 9));
set.headSet(5);        // [1, 3] - elements < 5
set.tailSet(5);        // [5, 7, 9] - elements >= 5
set.subSet(3, 7);      // [3, 5] - elements >= 3 and < 7
set.floor(6);          // 5 - greatest element <= 6
set.ceiling(6);        // 7 - smallest element >= 6
```

---

### Q122. Why TreeSet is slower?

TreeSet maintains a **red-black tree** (self-balancing BST). Every add/remove/contains requires:
1. Traversal from root to correct position: O(log n)
2. Potentially rebalancing the tree: O(log n) rotations

HashSet's hash-based approach achieves O(1) average case (single bucket lookup if no collisions).

For n=1,000,000: HashSet find = 1 hash + ~1 comparison; TreeSet find = ~20 comparisons (log₂(1,000,000) ≈ 20).

---

### Q123. What is Comparable?

`Comparable<T>` is an interface for defining **natural ordering** of a class.

```java
public interface Comparable<T> {
    int compareTo(T other);
}
```

- Returns negative: `this < other`
- Returns 0: `this == other`
- Returns positive: `this > other`

```java
public class Employee implements Comparable<Employee> {
    private String name;
    private double salary;
    
    @Override
    public int compareTo(Employee other) {
        return Double.compare(this.salary, other.salary); // sort by salary
    }
}
```

Used by `TreeSet`, `TreeMap`, `Collections.sort()`, `Arrays.sort()` when no explicit Comparator provided.

**Contract**: Must be consistent with equals. If `compareTo()` returns 0, `equals()` should return `true`.

---

### Q124. Comparable vs Comparator?

| | Comparable | Comparator |
|-|------------|------------|
| Package | `java.lang` | `java.util` |
| Method | `compareTo(T)` | `compare(T, T)` |
| Defined by | The class itself | External class or lambda |
| Orders | Natural/default | Multiple/custom |
| Modifies | Source class | No modification |
| Use | Sort by natural order | Sort by custom order |

```java
// Comparable - in the class
class Person implements Comparable<Person> {
    public int compareTo(Person other) { return this.age - other.age; }
}

// Comparator - external, multiple options
Comparator<Person> byName = Comparator.comparing(Person::getName);
Comparator<Person> byAgeDesc = Comparator.comparingInt(Person::getAge).reversed();
Comparator<Person> complex = Comparator.comparing(Person::getLastName)
                                        .thenComparing(Person::getFirstName)
                                        .thenComparingInt(Person::getAge);
```

---

### Q125. Custom sorting?

```java
List<Employee> employees = ...;

// By salary descending
employees.sort(Comparator.comparingDouble(Employee::getSalary).reversed());

// Multiple criteria
employees.sort(Comparator.comparing(Employee::getDepartment)
    .thenComparing(Employee::getName)
    .thenComparingDouble(Employee::getSalary));

// Null-safe
employees.sort(Comparator.comparing(Employee::getManager, 
    Comparator.nullsLast(Comparator.naturalOrder())));

// Stream
employees.stream()
    .sorted(Comparator.comparing(Employee::getHireDate))
    .collect(Collectors.toList());
```

---

### Q126. Fail-fast vs fail-safe?

**Fail-fast iterators** (ArrayList, HashMap, HashSet): Throw `ConcurrentModificationException` if collection is structurally modified during iteration (not through the iterator).

Mechanism: `modCount` counter tracks structural modifications. Iterator checks `modCount` on each `next()` call.

```java
List<String> list = new ArrayList<>(List.of("a", "b", "c"));
for (String s : list) {
    if (s.equals("b")) list.remove(s); // ConcurrentModificationException!
}
// Fix: list.removeIf(s -> s.equals("b"));
// Or: use iterator.remove()
```

**Fail-safe iterators** (CopyOnWriteArrayList, ConcurrentHashMap): Iterate over a snapshot or weakly-consistent view; no exception thrown, but may not reflect latest changes.

```java
CopyOnWriteArrayList<String> cowList = new CopyOnWriteArrayList<>(List.of("a", "b"));
for (String s : cowList) {
    cowList.add("new"); // no exception, iterator sees original snapshot
}
```

---

### Q127. Iterator vs ListIterator?

| | Iterator | ListIterator |
|-|----------|-------------|
| Direction | Forward only | Both forward and backward |
| Interfaces | `Iterator<E>` | `ListIterator<E>` extends Iterator |
| Available for | All Collections | Lists only |
| Methods | `hasNext()`, `next()`, `remove()` | + `hasPrevious()`, `previous()`, `add()`, `set()`, `nextIndex()`, `previousIndex()` |

```java
List<String> list = new ArrayList<>(List.of("a", "b", "c"));
ListIterator<String> it = list.listIterator();
while (it.hasNext()) {
    String s = it.next();
    it.set(s.toUpperCase()); // replace current element
}
while (it.hasPrevious()) {
    System.out.println(it.previous()); // C, B, A
}
```

---

### Q128. Why Iterator remove method?

Calling `list.remove()` during for-each throws `ConcurrentModificationException`. Iterator's `remove()` is the **safe way** to remove during iteration.

```java
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String s = it.next();
    if (condition(s)) it.remove(); // safe - updates modCount
}
```

`it.remove()` removes the last element returned by `next()`. Must call `next()` first. Can only call once per `next()`.

Modern alternative (Java 8+):
```java
list.removeIf(s -> condition(s)); // cleaner
```

---

### Q129. Stream vs Iterator?

| | Iterator | Stream |
|-|----------|--------|
| Paradigm | Imperative | Functional/declarative |
| Operation type | Sequential | Sequential or parallel |
| Reusability | Can reset | Single-use |
| Lazy | No | Yes (intermediate ops) |
| Modification | Supports remove | Immutable source |
| Short-circuit | Manual | Built-in (`findFirst`, `limit`) |
| Parallel | No | Yes (`parallelStream()`) |
| Chaining | No | Yes (fluent API) |

Use `Iterator` when: simple sequential traversal with possible removal. Use `Stream` when: transformations, filtering, aggregations, parallel processing.

---

### Q130. What is Spliterator?

`Spliterator` (Splittable Iterator) supports **parallel traversal** of a data source.

```java
public interface Spliterator<T> {
    boolean tryAdvance(Consumer<? super T> action); // process one element
    Spliterator<T> trySplit(); // split for parallel processing
    long estimateSize(); // rough count
    int characteristics(); // ORDERED, SORTED, SIZED, DISTINCT, IMMUTABLE, etc.
}
```

Stream API uses Spliterators internally for parallel streams:
1. `trySplit()` recursively splits the Spliterator
2. Each split processed by different threads in ForkJoinPool
3. Results combined

Custom Spliterators: Implement for custom data structures to support parallel stream processing.

---

### Q131. Queue vs Deque?

**Queue**: FIFO – insert at tail, remove from head.

```java
Queue<String> queue = new LinkedList<>();
queue.offer("a"); // add to tail
queue.poll();     // remove from head
queue.peek();     // look at head without removing
```

**Deque** (Double-Ended Queue): Insert/remove at both ends.

```java
Deque<String> deque = new ArrayDeque<>();
deque.offerFirst("a"); // add to front
deque.offerLast("b");  // add to back
deque.pollFirst();      // remove from front
deque.pollLast();       // remove from back
```

`ArrayDeque` is preferred over `LinkedList` for queue/deque use cases (faster, less memory).
`ArrayDeque` as stack: `push()` = `addFirst()`, `pop()` = `removeFirst()` – faster than `Stack`.

---

### Q132. PriorityQueue internal working?

`PriorityQueue` uses a **min-heap** (binary heap stored in array). Smallest element (by natural order or Comparator) is always at the head.

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.offer(5); pq.offer(1); pq.offer(3);
pq.poll(); // returns 1 (minimum)
```

**Internal structure**: Array where element at index `i` has children at `2i+1` and `2i+2`, parent at `(i-1)/2`.

Operations:
- `offer(e)`: Add at end, sift up – O(log n)
- `poll()`: Remove root (min), replace with last, sift down – O(log n)
- `peek()`: Return root – O(1)
- `remove(o)`: Find and remove arbitrary element – O(n) to find

Not sorted (heap order, not sorted order). Iteration is NOT in priority order; only poll() guarantees order.

**Max-heap**:
```java
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Comparator.reverseOrder());
```

---

### Q133. BlockingQueue?

`BlockingQueue` extends `Queue` with **blocking operations** for producer-consumer patterns:

```java
BlockingQueue<Task> queue = new LinkedBlockingQueue<>(100); // capacity 100

// Producer
queue.put(task);    // blocks if full
queue.offer(task, 5, TimeUnit.SECONDS); // blocks up to 5s

// Consumer
Task task = queue.take();    // blocks if empty
Task task = queue.poll(5, TimeUnit.SECONDS); // blocks up to 5s
```

Implementations:
- `LinkedBlockingQueue`: Unbounded (or capacity-limited), linked nodes
- `ArrayBlockingQueue`: Fixed capacity, array-backed, fair/unfair option
- `PriorityBlockingQueue`: Priority ordered, unbounded
- `DelayQueue`: Elements available after a delay
- `SynchronousQueue`: No storage; direct handoff between producer and consumer
- `LinkedTransferQueue`: More efficient for some producer-consumer scenarios

Used in: Thread pool's work queue (`Executors.newFixedThreadPool` uses `LinkedBlockingQueue`), message passing between threads.

---

### Q134. Concurrent collections?

| Regular | Concurrent |
|---------|-----------|
| ArrayList | CopyOnWriteArrayList |
| HashSet | ConcurrentSkipListSet |
| HashMap | ConcurrentHashMap |
| TreeMap | ConcurrentSkipListMap |
| LinkedList | ConcurrentLinkedDeque |
| PriorityQueue | PriorityBlockingQueue |

`ConcurrentSkipList*`: Lock-free, sorted, O(log n) operations. Alternative to synchronized TreeMap/TreeSet.

`ConcurrentLinkedDeque`: Lock-free, non-blocking concurrent deque. High throughput.

---

### Q135. Why CopyOnWriteArrayList?

`CopyOnWriteArrayList`: On every write (add, set, remove), creates a **new copy** of the entire array. Reads are non-blocking (use snapshot of array at time of iterator creation).

```java
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
// Read: No locking, uses current array reference
// Write: Creates new array, copies, modifies, switches reference atomically
```

**Best for**: Read-heavy, write-rare scenarios (observer lists, listener registries, routing tables).

**Terrible for**: High write frequency (copies entire array on each write – expensive for large lists).

Trade-offs:
- Reads: Very fast, no locking
- Writes: Slow (O(n) copy), expensive
- Memory: May hold multiple array versions during GC
- Iteration: Safe (snapshot), won't see concurrent modifications

---

### Q136. WeakHashMap use cases?

`WeakHashMap`: Keys are held with **weak references**. If no strong reference to a key exists elsewhere, GC can collect the key and the entry is automatically removed.

```java
WeakHashMap<Object, String> cache = new WeakHashMap<>();
Object key = new Object();
cache.put(key, "value");
key = null; // Remove strong reference
// After GC, the entry may be removed from the map
System.gc();
cache.size(); // may be 0
```

Use cases:
1. **Caches** where values should be collected when key is no longer used
2. **Metadata/attributes** attached to objects (without preventing GC)
3. **Canonical mapping**: canonicalize instances (no duplicate instances of same logical object)

**Pitfall**: If key is a string literal (interned, always strongly reachable), it's never collected. WeakHashMap with `String` keys often doesn't work as expected.

---

### Q137. IdentityHashMap?

`IdentityHashMap`: Uses `==` (reference equality) instead of `.equals()` for key comparison, and `System.identityHashCode()` instead of `hashCode()`.

```java
IdentityHashMap<String, Integer> map = new IdentityHashMap<>();
String a = new String("key");
String b = new String("key");
map.put(a, 1);
map.put(b, 2);
map.size(); // 2 - different objects, same content but different identity
```

Use cases:
1. **Object graph traversal**: Detect cycles using object identity (not equality)
2. **Serialization**: Track already-serialized objects
3. **Deep clone**: Track already-copied objects
4. **Proxy systems**: Map by object identity

---

### Q138. EnumMap?

`EnumMap<K extends Enum<K>, V>`: HashMap optimized for enum keys using a compact array internally.

```java
enum Day { MON, TUE, WED, THU, FRI, SAT, SUN }

EnumMap<Day, String> schedule = new EnumMap<>(Day.class);
schedule.put(Day.MON, "Team meeting");
schedule.put(Day.FRI, "Code review");
```

Advantages over HashMap with enum keys:
- **Faster**: Array indexed by enum ordinal – O(1) with no hash computation or collision
- **Memory-efficient**: Compact array representation
- **Ordered**: Iterates in enum declaration order
- **No null keys** (enum values can't be null)

Always prefer `EnumMap` over `HashMap` when keys are enums.

---

### Q139. Performance considerations of collections?

Key principles:

1. **Pre-size collections**: `new ArrayList<>(expectedSize)` or `new HashMap<>(capacity/0.75)`
2. **ArrayList vs LinkedList**: ArrayList almost always wins in practice (cache efficiency)
3. **HashSet for contains()**: O(1) vs ArrayList's O(n)
4. **Avoid Iterator wrapping**: Direct array iteration > stream > iterator for simple cases
5. **Primitive collections**: Use `IntStream`, or libraries like Eclipse Collections, Trove for primitive lists (avoid boxing)
6. **Immutable collections**: `List.of()` (Java 9) – more memory-efficient than ArrayList with no-modification
7. **EnumMap/EnumSet**: For enum-keyed maps and sets – very fast

---

### Q140. Time complexity of common operations?

| Operation | ArrayList | LinkedList | HashMap | TreeMap | HashSet | TreeSet |
|-----------|-----------|-----------|---------|---------|---------|---------|
| Add | O(1) amort | O(1) | O(1) avg | O(log n) | O(1) avg | O(log n) |
| Remove | O(n) | O(1)* | O(1) avg | O(log n) | O(1) avg | O(log n) |
| Get/Contains | O(1) idx / O(n) value | O(n) | O(1) avg | O(log n) | O(1) avg | O(log n) |
| Iteration | O(n) | O(n) | O(capacity) | O(n) | O(capacity) | O(n) |

*LinkedList remove is O(n) if you need to find the element first; O(1) with iterator.

HashMap worst case (all collisions): O(n). After Java 8 treeification: O(log n) worst case.

---

### Q141. Collection synchronization techniques?

1. **Use concurrent collections** (best):
```java
Map<K,V> map = new ConcurrentHashMap<>();
List<E> list = new CopyOnWriteArrayList<>();
```

2. **Collections.synchronizedXxx()** (synchronized wrapper):
```java
List<String> syncList = Collections.synchronizedList(new ArrayList<>());
Map<K,V> syncMap = Collections.synchronizedMap(new HashMap<>());
// Must synchronize on collection for iteration:
synchronized (syncList) { for (String s : syncList) {} }
```

3. **Manual synchronization**: `synchronized` block or `ReentrantLock`

4. **Immutability**: `List.of()`, `Collections.unmodifiableList()` – no sync needed for reads

---

### Q142. How to make collections thread-safe?

Choose based on use pattern:
- **Read-heavy**: `CopyOnWriteArrayList` / `Collections.unmodifiableList()`
- **Write-heavy, all operations**: `Collections.synchronizedList()` + external lock on iteration
- **Concurrent read+write**: `ConcurrentHashMap`, `ConcurrentSkipListMap`
- **Queue for producer-consumer**: `BlockingQueue` implementations
- **Immutable**: `List.of()`, `Map.of()` (Java 9+) – zero synchronization needed

---

### Q143. Collections vs Arrays utility classes?

**`java.util.Collections`**: Static methods for Collection operations:
- `sort()`, `reverse()`, `shuffle()`, `rotate()`
- `min()`, `max()`, `frequency()`, `disjoint()`
- `unmodifiableList()`, `synchronizedList()`
- `singletonList()`, `emptyList()`, `nCopies()`
- `binarySearch()` (on sorted list)
- `fill()`, `copy()`, `swap()`

**`java.util.Arrays`**: Static methods for array operations:
- `sort()`, `parallelSort()`
- `binarySearch()`
- `copyOf()`, `copyOfRange()`
- `fill()`
- `equals()`, `deepEquals()`
- `toString()`, `deepToString()`
- `asList()` – convert array to fixed-size List (backed by array, no add/remove)
- `stream()` – create stream from array

---

### Q144. Why equals/hashCode critical in collections?

Any hash-based collection (HashMap, HashSet, HashTable) uses both:
1. `hashCode()` to find the bucket
2. `equals()` to confirm the key match

Breaking the contract causes:
- **Missed lookups**: equal objects land in different buckets (not found)
- **Duplicate insertions**: Two "equal" objects in same Set
- **Memory leaks**: Keys added but never removable

```java
class BadKey {
    int value;
    @Override public boolean equals(Object o) { ... } // overrides equals
    // NO hashCode override!
}

Set<BadKey> set = new HashSet<>();
BadKey k1 = new BadKey(5);
set.add(k1);
set.contains(new BadKey(5)); // FALSE! Different hashCode → different bucket
```

---

### Q145. What causes memory leak in HashMap?

1. **Mutable key whose hashCode changes after insertion**:
```java
List<String> key = new ArrayList<>(List.of("a"));
map.put(key, "value");
key.add("b"); // hashCode changes! Entry now in wrong bucket!
// map.get(key) returns null even though key is in map
// Old entry is leaked - impossible to reach without iterating all buckets
```

2. **Keys not removed**: `static` HashMap accumulating entries indefinitely

3. **ThreadLocal HashMap**: If thread pool threads survive but application context changes, `ThreadLocal` + `HashMap` can leak class loaders in web app redeployment scenarios.

4. **Inner class in static context**: Anonymous inner class holding reference to outer instance.

---

### Q146. How iteration works internally?

**ArrayList**: Index-based, `cursor` variable incremented:
```java
// Simplified Iterator for ArrayList
int cursor = 0;
public boolean hasNext() { return cursor < size; }
public E next() { 
    checkForModification(); // modCount check
    return elementData[cursor++]; 
}
```

**HashMap**: Iterates through buckets array, then through linked list/tree nodes within each bucket:
```java
// Visits all capacity buckets even if empty (why iteration is O(capacity))
for (int i = 0; i < table.length; i++) {
    Node<K,V> node = table[i];
    while (node != null) { visit(node); node = node.next; }
}
```

**LinkedList**: Follows `next` pointers from head.

---

### Q147. What is structural modification?

A structural modification is any operation that **changes the size** of a collection: add, remove, clear.

Modifying an element's value (via `set()`) is NOT a structural modification.

Why it matters: Fail-fast iterators track structural modifications via `modCount`. Any structural modification during iteration (except via the iterator itself) causes `ConcurrentModificationException`.

```java
list.set(0, "new value"); // NOT structural, safe during iteration (via index, not iterator)
list.add("item");         // structural, throws if done during fail-fast iteration
```

---

### Q148. Immutability in collections?

**Immutable**: Cannot be modified (contents AND structure). Throw `UnsupportedOperationException`.

Java 9+ factory methods:
```java
List<String> list = List.of("a", "b", "c"); // immutable
Set<String> set = Set.of("x", "y");         // immutable
Map<String, Integer> map = Map.of("a", 1, "b", 2); // immutable
```

Properties:
- No null elements/keys
- `set()`, `add()`, `remove()` throw `UnsupportedOperationException`
- Compact memory representation (no backing array overhead)
- Iteration order: List – insertion; Set/Map – unspecified

Guava: `ImmutableList.of()`, `ImmutableMap.of()`, `ImmutableSet.of()` – similar but with richer API.

---

### Q149. Unmodifiable vs immutable?

**Unmodifiable** (`Collections.unmodifiableList()`): A **view** wrapping the original. Throws exception on mutation, but the **underlying collection can still be mutated** by direct reference.

```java
List<String> original = new ArrayList<>(List.of("a", "b"));
List<String> unmod = Collections.unmodifiableList(original);

unmod.add("c"); // UnsupportedOperationException
original.add("c"); // OK! unmod now shows "a", "b", "c"
```

**Immutable** (`List.of()`): No underlying mutable collection. Truly frozen.

```java
List<String> immutable = List.of("a", "b");
// No way to modify this - period
```

Use `List.of()` when you have control over creation. Use `Collections.unmodifiableList()` to expose a view of a mutable collection.

**Deep vs shallow immutability**: Both are *shallowly* immutable – can't add/remove elements, but the elements themselves can be mutated if they're mutable objects.

---

### Q150. Java 9 factory methods for collections?

```java
// List
List<String> list = List.of("a", "b", "c");
List<String> copy = List.copyOf(existingCollection);

// Set
Set<String> set = Set.of("x", "y", "z");
Set<String> copy = Set.copyOf(existingCollection);

// Map
Map<String, Integer> map = Map.of("a", 1, "b", 2); // up to 10 entries
Map<String, Integer> map = Map.ofEntries(
    Map.entry("a", 1),
    Map.entry("b", 2),
    Map.entry("c", 3)
    // no limit
);
Map<String, Integer> copy = Map.copyOf(existingMap);
```

Key characteristics vs old approaches:
- Throw `NullPointerException` for null elements/keys/values
- Throw `IllegalArgumentException` for duplicate elements/keys (in Set/Map)
- Unspecified iteration order (Set, Map)
- More memory-efficient than `Arrays.asList()` + Collections wrappers
- Serializable

---

## PART 5: Exception Handling (Q151–175)

---

### Q151. Checked vs unchecked exceptions?

**Checked exceptions** (must be caught or declared with `throws`):
- Extends `Exception` but NOT `RuntimeException`
- Represent recoverable, expected conditions
- Examples: `IOException`, `SQLException`, `ClassNotFoundException`

```java
public void readFile(String path) throws IOException { // must declare
    Files.readAllBytes(Path.of(path));
}
```

**Unchecked exceptions** (RuntimeException subclasses):
- No requirement to catch or declare
- Represent programming errors or unrecoverable situations
- Examples: `NullPointerException`, `IllegalArgumentException`, `ArrayIndexOutOfBoundsException`

**Controversy**: Checked exceptions are unique to Java. Many argue they clutter APIs and encourage `catch (Exception e) {}` anti-patterns. Spring and most modern libraries convert checked to unchecked. Kotlin and C# don't have checked exceptions.

---

### Q152. Error vs Exception?

```
Throwable
├── Error          (serious JVM/system problems, don't catch)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   ├── VirtualMachineError
│   └── AssertionError
└── Exception      (application-level problems)
    ├── IOException (checked)
    ├── SQLException (checked)
    └── RuntimeException (unchecked)
        ├── NullPointerException
        ├── IllegalArgumentException
        └── ...
```

**Errors**: Abnormal conditions in JVM itself. Not caused by application code. Recovery is impossible or impractical. **Don't catch Errors** (except in very specific frameworks like test runners catching AssertionError).

**Exceptions**: Problems in application logic. Checked = expected, recoverable. Unchecked = programming errors, typically unrecoverable in context.

---

### Q153. Why RuntimeException exists?

RuntimeException provides a category for **programming errors** that:
1. Can occur at any point in code
2. Would be impractical to declare on every method
3. Typically indicate bugs rather than external conditions

If all runtime errors were checked:
- `NullPointerException` would have to be declared on every method that uses an object reference
- `ClassCastException` on every cast
- This would make Java code completely unmanageable

RuntimeException says: "This is the caller's fault for misusing the API. Handle it at a high level or let the application fail."

---

### Q154. Custom exception?

```java
// Custom checked exception
public class UserNotFoundException extends Exception {
    private final Long userId;
    
    public UserNotFoundException(Long userId) {
        super("User not found with ID: " + userId);
        this.userId = userId;
    }
    
    // Exception chaining
    public UserNotFoundException(Long userId, Throwable cause) {
        super("User not found with ID: " + userId, cause);
        this.userId = userId;
    }
    
    public Long getUserId() { return userId; }
}

// Custom unchecked exception
public class InsufficientFundsException extends RuntimeException {
    private final BigDecimal required;
    private final BigDecimal available;
    
    public InsufficientFundsException(BigDecimal required, BigDecimal available) {
        super(String.format("Required: %s, Available: %s", required, available));
        this.required = required;
        this.available = available;
    }
}
```

**Best practices**:
- Provide constructors with message and cause (for chaining)
- Add relevant context fields
- Make unchecked unless caller can meaningfully recover
- Declare `serialVersionUID` for serializable exceptions

---

### Q155. Best practices for exception handling?

1. **Be specific**: Catch specific exceptions, not `Exception` or `Throwable`
2. **Don't swallow**: Never `catch (Exception e) {}` – at minimum log the exception
3. **Preserve cause**: Always use exception chaining when wrapping: `new ServiceException("msg", originalException)`
4. **Fail fast**: Throw early when preconditions fail (IllegalArgumentException, NullPointerException)
5. **Document with @throws**: In Javadoc, document what exceptions and when
6. **Don't use exceptions for control flow**: `try { parse() } catch (NumberFormatException)` as normal flow is bad
7. **Clean up in finally/try-with-resources**: Don't rely on exception path to close resources
8. **Log once**: Log at the point of handling, not at every re-throw
9. **Don't catch what you can't handle**: Let it propagate to a level that can handle it
10. **RuntimeException for API misuse**: IllegalArgument, IllegalState, NullPointer for programming errors

---

### Q156. Exception propagation?

Exceptions propagate up the **call stack** until caught or the program terminates.

```
main() → serviceMethod() → daoMethod() → throws IOException
         ↑ propagates
serviceMethod() → if not caught, propagates to main()
main() → if not caught, JVM prints stack trace and exits
```

**Checked exceptions**: Must be caught or declared at each level in the call stack.
**Unchecked**: Propagate automatically without declaration.

```java
void dao() throws IOException { throw new IOException("DB Error"); }
void service() throws IOException { dao(); } // propagates
void controller() {
    try { service(); }
    catch (IOException e) { throw new ServiceException("Failed", e); } // wrap and re-throw
}
```

---

### Q157. What is stack trace?

A stack trace is a **snapshot of the call stack** at the moment an exception was thrown:

```
java.lang.NullPointerException: Cannot invoke "String.length()" because "str" is null
    at com.example.UserService.processName(UserService.java:45)
    at com.example.UserController.handleRequest(UserController.java:23)
    at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
    at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:897)
    ...
```

Reading a stack trace:
- **First line**: Exception type and message
- **Second line**: Immediate location where exception was thrown
- **Subsequent lines**: Call chain (most recent first)

Java 14+ **Helpful NullPointerExceptions**: JVM can tell you exactly which part of an expression was null.

**Performance**: `new Exception()` captures stack trace (expensive). `new RuntimeException(message, cause, false, false)` disables stack trace collection (use in high-performance exception factories).

---

### Q158. finally block behavior?

`finally` block **always executes** after try/catch, regardless of:
- Normal completion
- Exception thrown (caught or not)
- `return` or `break` in try block

```java
int test() {
    try {
        return 1;
    } finally {
        System.out.println("finally runs"); // prints before return
        // return 2; // DON'T: this swallows the original return/exception
    }
}
```

**Exceptions**: `System.exit()`, JVM crash, thread kill – finally may NOT run.

**try-with-resources** is preferred over finally for resource cleanup (simpler, handles suppressed exceptions better).

---

### Q159. Return in try/finally?

If both `try` and `finally` have `return`:

```java
String test() {
    try {
        return "try"; // scheduled
    } finally {
        return "finally"; // overrides! "try" return is LOST
    }
}
test(); // returns "finally"
```

Similarly, if `finally` throws an exception, it overrides the try-block exception (original is lost unless captured and attached as suppressed).

**Best practice**: Never put `return` or `throw` in `finally`. It masks exceptions and makes code hard to reason about.

---

### Q160. Suppressed exceptions?

In try-with-resources, if both body and `close()` throw exceptions, the `close()` exception is **suppressed** and attached to the primary exception:

```java
try (Resource r = new Resource()) {
    throw new RuntimeException("body exception");
    // r.close() also throws: "close exception"
}
// Caught: RuntimeException("body exception")
// exception.getSuppressed()[0] = RuntimeException("close exception")
```

Manual suppression:
```java
Throwable primary = null;
try {
    doWork();
} catch (Throwable t) {
    primary = t;
} finally {
    try { close(); }
    catch (Throwable t2) {
        if (primary != null) primary.addSuppressed(t2);
        else throw t2;
    }
}
if (primary != null) throw primary;
```

try-with-resources handles this automatically – that's one reason to prefer it.

---

### Q161. What is Exception chaining?

Exception chaining preserves the **original cause** when wrapping exceptions:

```java
try {
    statement.executeQuery(sql);
} catch (SQLException e) {
    // Wrap with context, preserve cause
    throw new DataAccessException("Failed to fetch users", e);
}
```

Access the chain:
```java
catch (DataAccessException e) {
    Throwable cause = e.getCause(); // original SQLException
    cause.getCause(); // could be deeper chain
}
```

Constructors for chaining: `Exception(String message, Throwable cause)`.

**Why it matters**: Without chaining, the original stack trace is lost. With chaining, you can trace the full execution path from root cause to high-level error.

---

### Q162. try-with-resources internals?

Compiles to bytecode roughly equivalent to:

```java
// try (Resource r = acquire()) { body }
Resource r = acquire();
Throwable primaryException = null;
try {
    body;
} catch (Throwable t) {
    primaryException = t;
    throw t;
} finally {
    if (r != null) {
        if (primaryException != null) {
            try { r.close(); }
            catch (Throwable t) { primaryException.addSuppressed(t); }
        } else {
            r.close();
        }
    }
}
```

The key difference from manual try-finally:
1. Close exceptions are suppressed (not swallowed, not masking primary)
2. Resource is guaranteed closed
3. Works with multiple resources (closed in reverse order)

---

### Q163. AutoCloseable vs Closeable?

| | `Closeable` | `AutoCloseable` |
|-|-------------|----------------|
| Introduced | Java 5 | Java 7 |
| Extends | - | - |
| `Closeable` extends | `AutoCloseable` | - |
| `close()` throws | `IOException` | `Exception` |
| Multiple closes | Should be idempotent | Not required |
| Use in TWR | Yes | Yes |

`Closeable` is for I/O resources (specifically throws `IOException`).
`AutoCloseable` is more general (throws `Exception`).

All `java.io` classes implement `Closeable`, which extends `AutoCloseable`, so all work with try-with-resources.

For custom resources: implement `AutoCloseable` (or `Closeable` for I/O). Make `close()` idempotent when possible (safe to call multiple times).

---

### Q164. When not to catch exception?

1. **You can't handle it meaningfully**: Don't catch just to re-throw the same exception
2. **Let framework handle it**: Spring's `@ExceptionHandler`, JAX-RS exception mappers
3. **Test code**: Don't catch in tests – let JUnit/TestNG report failures
4. **Logging noise**: Don't catch, log, and re-throw – log once at the handling point
5. **Checked exceptions in streams/lambdas**: Wrapping is cumbersome; often let it propagate

**Anti-pattern**:
```java
try {
    return service.process(request);
} catch (Exception e) {
    log.error("Error", e);
    throw e; // re-throwing - logger should be at higher level
}
```

---

### Q165. Logging exceptions correctly?

```java
// CORRECT: Log with exception (full stack trace)
log.error("Failed to process order {}", orderId, exception);

// WRONG: Only log message (no stack trace)
log.error("Failed to process order {}: {}", orderId, exception.getMessage());

// WRONG: System.out.println
System.out.println("Error: " + exception); // no proper logging

// WRONG: Log and throw (duplicate logs up the chain)
log.error("Error", e);
throw new ServiceException("Error", e); // caller will log too
```

**Structured logging**:
```java
log.error("Failed to process order", 
    kv("orderId", orderId), 
    kv("userId", userId),
    exception);
```

**Log once**: Log at the point of final handling, not at every intermediate catch-and-rethrow.

---

### Q166. Re-throwing exceptions?

```java
// Rethrow same exception (no wrapping)
try { ... }
catch (IOException e) { 
    cleanup();
    throw e; // same exception, original stack trace preserved
}

// Wrap (exception translation)
try { ... }
catch (SQLException e) {
    throw new DataAccessException("DB error", e); // wrapped with cause
}

// Java 7 precise rethrow
try { mightThrowChecked(); }
catch (Exception e) {
    // compiler knows only declared checked exceptions can reach here
    throw e; // no need to declare Exception on method, only declared checked types
}
```

---

### Q167. Exception handling in lambdas?

Lambdas are tricky with checked exceptions – functional interfaces don't declare `throws`:

```java
// Compile error: Function doesn't declare throws IOException
Function<String, String> fn = path -> Files.readString(Path.of(path));

// Option 1: Wrap in unchecked
Function<String, String> fn = path -> {
    try { return Files.readString(Path.of(path)); }
    catch (IOException e) { throw new UncheckedIOException(e); }
};

// Option 2: Helper method that wraps checked
@FunctionalInterface
interface CheckedFunction<T, R> { R apply(T t) throws Exception; }
static <T, R> Function<T, R> wrap(CheckedFunction<T, R> fn) {
    return t -> { try { return fn.apply(t); } catch (Exception e) { throw new RuntimeException(e); } };
}

Function<String, String> fn = wrap(path -> Files.readString(Path.of(path)));
```

Libraries like Lombok (`@SneakyThrows`), Vavr, and others provide utilities for this.

---

### Q168. Global exception handling?

**In Spring Boot REST**:
```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(UserNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ErrorResponse handleNotFound(UserNotFoundException e) {
        return new ErrorResponse("USER_NOT_FOUND", e.getMessage());
    }
    
    @ExceptionHandler(ConstraintViolationException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ErrorResponse handleValidation(ConstraintViolationException e) {
        return new ErrorResponse("VALIDATION_ERROR", buildViolationMessage(e));
    }
    
    @ExceptionHandler(Exception.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public ErrorResponse handleGeneral(Exception e, HttpServletRequest request) {
        log.error("Unhandled exception for {}", request.getRequestURI(), e);
        return new ErrorResponse("INTERNAL_ERROR", "An unexpected error occurred");
    }
}
```

**Thread.UncaughtExceptionHandler**:
```java
Thread.setDefaultUncaughtExceptionHandler((thread, throwable) -> {
    log.error("Uncaught exception in thread {}", thread.getName(), throwable);
    // alerting, metrics, etc.
});
```

---

### Q169. Performance impact of exceptions?

Exceptions are expensive:
1. **Stack trace capture** (`fillInStackTrace()`): Walks the JVM stack, creates StackTraceElement objects. O(depth) cost.
2. **Object creation**: `new Exception()` involves heap allocation + GC pressure
3. **JIT deoptimization**: Paths with exception handling may be compiled less aggressively

**Benchmarks**: A thrown/caught exception is ~100-1000x slower than a normal return, depending on stack depth.

**Best practices**:
1. **Don't use exceptions for control flow** (e.g., use `Optional` instead of catching NPE)
2. **Pre-validate**: Check `isPresent()` before `get()` rather than catching `NoSuchElementException`
3. **Expensive in tight loops**: `Integer.parseInt()` + catch is slow; use `NumberUtils.isNumeric()` first
4. **Disable stack trace**: For exceptions used as signals (not bugs), override `fillInStackTrace()`:
```java
class FastException extends RuntimeException {
    @Override public synchronized Throwable fillInStackTrace() { return this; }
}
```

---

### Q170. When to use assertions?

`assert condition : "message";` – throws `AssertionError` if condition is false (when assertions enabled with `-ea`).

**Appropriate uses**:
- **Internal invariants**: Pre-conditions for private methods (you control callers)
- **Post-conditions**: Verify result after complex computation
- **Unreachable code**: `default: assert false : "Unknown state: " + state;`
- **Class/loop invariants**

**NOT appropriate**:
- Public method parameter validation (use `IllegalArgumentException`)
- Business logic checks (use exceptions)
- Production code where assertions are disabled (default)

Assertions are disabled by default in production JVMs. Use `IllegalArgumentException`/`IllegalStateException` for code-correctness checks that should always run.

---

### Q171. Assertion vs exception?

| | Assertion | Exception |
|-|-----------|-----------|
| Use | Programmer errors, invariants | Expected/unexpected runtime conditions |
| Default | Disabled in production | Always active |
| Type | AssertionError (Error) | Exception subclass |
| Public API | No (use IAE) | Yes |
| Recovery | No (programming bug) | Yes (if checked) |

Rule of thumb: **Assertions document invariants for developers. Exceptions handle conditions for callers.**

---

### Q172. Custom checked exceptions – pros/cons?

**Pros**:
- Compiler forces callers to handle/declare
- Self-documenting API: method signature shows failure modes
- Forces recovery strategy consideration
- Type-safe error handling

**Cons**:
- Clutters method signatures (`throws A, B, C, D`)
- Callers often swallow them: `catch (Exception e) { }`
- Breaks with lambdas/streams
- Spreads checked exception declarations through layers
- Controversy: C#, Kotlin, Scala, modern Java frameworks avoid them

**Senior take**: Use checked exceptions sparingly. If the caller can genuinely do something different for this exception (retry, fallback, user message), it might warrant checked. Otherwise, unchecked is cleaner. Spring's philosophy: convert all infrastructure exceptions (JDBC, JMS) to unchecked (DataAccessException hierarchy).

---

### Q173. Exception translation?

Converting low-level/infrastructure exceptions to higher-level/domain exceptions:

```java
// DAO layer
try {
    return jdbcTemplate.queryForObject(sql, params, User.class);
} catch (EmptyResultDataAccessException e) {
    throw new UserNotFoundException(id); // translate to domain exception
} catch (DataAccessException e) {
    throw new DataStoreException("Failed to fetch user " + id, e); // translate
}
```

Why:
1. **Abstraction**: Callers shouldn't need to know you use JDBC/JPA
2. **Encapsulation**: Implementation details (SQL errors) don't leak
3. **Layer responsibility**: DAL translates DB errors; service layer translates to business errors
4. **Testability**: Can test service with mock that throws domain exceptions

---

### Q174. API design and exceptions?

1. **Checked for expected failures**: `findUser()` → `throws UserNotFoundException` if not finding is a normal case callers must handle
2. **Unchecked for programming errors**: `setAge(-1)` → `throw new IllegalArgumentException("Age must be non-negative")`
3. **Document what you throw**: Javadoc `@throws`
4. **Consistent naming**: `XxxException` for checked, `XxxException extends RuntimeException` for unchecked, or just clear naming
5. **Don't throw from constructors** (leaking partially constructed objects) unless unavoidable
6. **Don't throw from finalizers**
7. **Prefer Optional over checked exception** for "not found" scenarios in functional contexts
8. **Consider http-status-friendly exceptions** for REST APIs

---

### Q175. Exception best practices in Spring?

1. **Use `@ControllerAdvice` / `@RestControllerAdvice`** for centralized exception handling
2. **Transactional rollback**: By default, Spring only rolls back on `RuntimeException` and `Error`. Use `@Transactional(rollbackFor = Exception.class)` to also rollback on checked exceptions
3. **DataAccessException**: Catch Spring's `DataAccessException` (unchecked) not raw `SQLException`
4. **Don't expose internal exceptions** in REST responses (security risk + poor UX)
5. **Consistent error response format**: `{ "error": "USER_NOT_FOUND", "message": "...", "timestamp": "..." }`
6. **Spring Boot's `ErrorController`**: Customize `/error` endpoint for HTML error pages
7. **Validation exceptions**: Handle `MethodArgumentNotValidException` for `@Valid` failures
8. **Log with correlation ID**: Include request ID in exception logs for traceability
9. **Propagation in async**: `@Async` methods lose exception context; use `AsyncUncaughtExceptionHandler`
10. **Circuit breaker**: Use Resilience4j to handle repeated failures gracefully

```java
// Getting context (rarely needed in Spring Boot apps)
ApplicationContext ctx = SpringApplication.run(MyApp.class, args);
UserService service = ctx.getBean(UserService.class);

// In a bean, inject context
@Component
class MyComponent implements ApplicationContextAware {
    private ApplicationContext context;
    
    @Override
    public void setApplicationContext(ApplicationContext ctx) {
        this.context = ctx;
    }
}
```

Hierarchy: Spring Boot creates a parent/child context. Parent context has non-web beans. Child (Web ApplicationContext) has web-specific beans (controllers, filters). Beans in parent visible to child but not vice versa.

**Environment**: ApplicationContext wraps Environment which holds properties, profiles, active environment.

---

### Q268. Bean lifecycle?

Complete bean lifecycle:

```
1. Instantiation (constructor called)
2. Property injection (setter/field injection, @Autowired)
3. BeanNameAware.setBeanName()
4. BeanFactoryAware.setBeanFactory()
5. ApplicationContextAware.setApplicationContext()
6. BeanPostProcessor.postProcessBeforeInitialization() [all beans]
7. @PostConstruct method
8. InitializingBean.afterPropertiesSet()
9. @Bean(initMethod = "init") custom init method
10. BeanPostProcessor.postProcessAfterInitialization() [all beans]
   → Bean ready for use
   ...
11. @PreDestroy method
12. DisposableBean.destroy()
13. @Bean(destroyMethod = "cleanup") custom destroy method
```

`BeanPostProcessor` is the extension point used by Spring AOP (creates proxies), @Async, @Transactional, etc.

---

### Q269. @Bean vs @Component?

**`@Component`** (and specializations `@Service`, `@Repository`, `@Controller`):
- Class-level annotation
- Detected by component scanning
- Spring instantiates the class directly
- For your own classes

**`@Bean`**:
- Method-level annotation inside `@Configuration` class
- You write the instantiation code
- For third-party classes you can't annotate
- More control over construction

```java
// @Component - Spring creates instance
@Service
public class UserService { ... }

// @Bean - you create instance (for third-party)
@Configuration
public class AppConfig {
    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper mapper = new ObjectMapper();
        mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        return mapper;
    }
    
    @Bean
    public RestTemplate restTemplate(RestTemplateBuilder builder) {
        return builder.setConnectTimeout(Duration.ofSeconds(5)).build();
    }
}
```

`@Configuration` classes use **CGLIB proxying**: `@Bean` methods called directly return the same bean instance (singleton). Use `@Configuration(proxyBeanMethods = false)` for lighter-weight config when inter-bean method calls not needed.

---

### Q270. Dependency injection types?

**Constructor injection** (recommended):
```java
@Service
public class OrderService {
    private final OrderRepository repo;
    private final PaymentService payment;
    
    // @Autowired optional if single constructor (Spring 4.3+)
    public OrderService(OrderRepository repo, PaymentService payment) {
        this.repo = repo;
        this.payment = payment;
    }
}
```

**Setter injection**:
```java
@Service
public class OrderService {
    private OrderRepository repo;
    
    @Autowired
    public void setRepo(OrderRepository repo) { this.repo = repo; }
}
```

**Field injection** (least recommended):
```java
@Service
public class OrderService {
    @Autowired private OrderRepository repo; // not testable without Spring context
}
```

---

### Q271. @Autowired vs constructor injection?

**Problems with `@Autowired` field injection**:
1. **Not testable**: Can't inject mocks without Spring context or reflection; with constructor you can `new OrderService(mockRepo)`
2. **Hides dependencies**: Looking at constructor tells you exactly what's needed
3. **Allows circular dependencies** (masks bad design); constructor injection fails fast
4. **Can't make fields `final`**: Breaks immutability
5. **Reflection-based**: Slower, bypasses normal Java visibility

**Constructor injection advantages**:
- Fields can be `final` (immutable, fail-fast if null)
- Obvious dependencies (API contract)
- Works without Spring (testable in isolation)
- Forces small, focused classes (many constructor params = code smell)
- No circular dependencies unless you use `@Lazy`

**Official recommendation**: Constructor injection is the Spring Team's preferred approach (since Spring 4.3).

---

### Q272. Profiles?

Profiles allow different beans/configurations for different environments:

```java
@Configuration
@Profile("prod")
public class ProdConfig { ... }

@Configuration
@Profile("!prod") // not prod
public class DevConfig { ... }

@Bean
@Profile({"dev", "test"})
public DataSource h2DataSource() { ... }

@Bean
@Profile("prod")
public DataSource prodDataSource() { ... }
```

Activate profiles:
```bash
# application.properties
spring.profiles.active=prod

# Command line
java -jar app.jar --spring.profiles.active=prod

# Environment variable
SPRING_PROFILES_ACTIVE=prod

# JVM property
-Dspring.profiles.active=prod

# Programmatic
SpringApplication app = new SpringApplication(MyApp.class);
app.setAdditionalProfiles("prod");
```

Profile-specific properties: `application-prod.yml`, `application-dev.yml`.

---

### Q273. Externalized configuration?

Spring Boot loads properties from many sources in priority order (later overrides earlier):

1. Default properties (`SpringApplication.setDefaultProperties`)
2. `@PropertySource` annotations
3. `application.properties` / `application.yml`
4. Profile-specific: `application-{profile}.properties`
5. OS environment variables
6. Java system properties (`-D`)
7. Command line arguments (`--property=value`) ← highest priority

```java
// Injecting properties
@Value("${app.name:MyApp}") // with default "MyApp"
private String appName;

// Typed configuration
@ConfigurationProperties(prefix = "app.mail")
@Validated
public class MailProperties {
    @NotBlank private String host;
    private int port = 25;
    private boolean ssl;
    // getters/setters
}

@SpringBootApplication
@EnableConfigurationProperties(MailProperties.class)
public class App { }
```

Spring Boot 2.4+: Config data API, `spring.config.import` to import from other files/vaults.

---

### Q274. application.yml vs properties?

Both configure the same properties:

```properties
# application.properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
server.port=8080
```

```yaml
# application.yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
server:
  port: 8080
```

**YAML advantages**:
- Less repetition of common prefixes
- Supports lists and maps more naturally
- Multiple profiles in one file using `---` document separator
- More readable for nested config

```yaml
# application.yml with multiple profiles
spring:
  profiles:
    active: dev
---
spring:
  config:
    activate:
      on-profile: dev
  datasource:
    url: jdbc:h2:mem:testdb
---
spring:
  config:
    activate:
      on-profile: prod
  datasource:
    url: jdbc:mysql://prod-server:3306/mydb
```

**YAML caveat**: YAML is sensitive to indentation. Properties is simpler and less error-prone.

---

### Q275. Actuator?

Spring Boot Actuator provides **production-ready endpoints** for monitoring and management:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Key endpoints (`/actuator/{id}`):
- `/health`: Application health (UP/DOWN + component details)
- `/info`: Application info (build, git commit, etc.)
- `/metrics`: Micrometer metrics (JVM, HTTP, custom)
- `/prometheus`: Prometheus-format metrics
- `/env`: Environment properties (sanitized)
- `/loggers`: View/change log levels at runtime
- `/threaddump`: Current thread dump
- `/heapdump`: Download heap dump
- `/beans`: All Spring beans
- `/mappings`: All URL mappings
- `/auditevents`: Security audit events
- `/refresh`: (Cloud) Reload config

```yaml
# Expose all endpoints (restrict in production!)
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      show-details: always
```

---

### Q276. Health checks?

`/actuator/health` aggregates health indicators:

Built-in indicators: DB, Redis, Kafka, Rabbit, Disk space, Elasticsearch, Mail, etc.

```java
// Custom health indicator
@Component
public class ExternalApiHealthIndicator implements HealthIndicator {
    
    @Override
    public Health health() {
        try {
            ResponseEntity<String> response = restTemplate.getForEntity(apiUrl, String.class);
            if (response.getStatusCode().is2xxSuccessful()) {
                return Health.up()
                    .withDetail("url", apiUrl)
                    .withDetail("responseTime", responseTime)
                    .build();
            }
            return Health.down().withDetail("status", response.getStatusCode()).build();
        } catch (Exception e) {
            return Health.down(e).build();
        }
    }
}
```

Kubernetes integration:
```yaml
management:
  endpoint:
    health:
      probes:
        enabled: true  # enables /actuator/health/liveness and /actuator/health/readiness
```

---

### Q277. Spring Boot logging?

Default: **Logback** with SLF4J facade.

```java
// SLF4J (facade - don't import Logback directly)
private static final Logger log = LoggerFactory.getLogger(UserService.class);
// Or with Lombok:
@Slf4j
public class UserService { }

log.debug("Processing user {}", userId);
log.info("User {} created", userId);
log.warn("Slow DB query: {}ms", elapsed);
log.error("Failed to process user {}", userId, exception);
```

Configuration in `application.yml`:
```yaml
logging:
  level:
    root: INFO
    com.example: DEBUG
    org.hibernate.SQL: DEBUG       # log SQL
    org.hibernate.type: TRACE      # log SQL parameters
  file:
    name: /var/log/myapp.log
  pattern:
    console: "%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"
```

Custom `logback-spring.xml` for advanced configuration (rolling files, async appenders, MDC).

---

### Q278. Logback vs Log4j2?

| | Logback | Log4j2 |
|-|---------|--------|
| Default in Spring Boot | Yes | No (exclude and add) |
| Performance | Good | Better (async appenders) |
| Async appenders | `AsyncAppender` | `AsyncLogger` (LMAX Disruptor) |
| JSON logging | Via logstash-logback-encoder | Built-in |
| Configuration | `logback-spring.xml` | `log4j2-spring.xml` |
| Hot reload | Yes | Yes |
| Security | Log4Shell vulnerability (2021) in Log4j2 2.x | CVE-2021-44228 (fixed in 2.17+) |

Switch to Log4j2:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

For high-throughput services: Log4j2 async loggers are significantly faster.

---

### Q279. Exception handling in REST?

**Option 1: `@RestControllerAdvice` (recommended)**:
```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ProblemDetail handleNotFound(ResourceNotFoundException ex, HttpServletRequest req) {
        ProblemDetail pd = ProblemDetail.forStatus(HttpStatus.NOT_FOUND);
        pd.setTitle("Resource Not Found");
        pd.setDetail(ex.getMessage());
        pd.setInstance(URI.create(req.getRequestURI()));
        return pd;
    }
    
    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ProblemDetail handleValidation(MethodArgumentNotValidException ex) {
        ProblemDetail pd = ProblemDetail.forStatus(HttpStatus.BAD_REQUEST);
        pd.setTitle("Validation Failed");
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(e -> 
            errors.put(e.getField(), e.getDefaultMessage()));
        pd.setProperty("errors", errors);
        return pd;
    }
}
```

**`ProblemDetail`** (RFC 7807, Spring 6 / Spring Boot 3): Standard error response format.

**Option 2: `ResponseEntityExceptionHandler`**: Extend for default Spring MVC exception handling.

---

### Q280. @ControllerAdvice?

`@ControllerAdvice` applies cross-cutting concerns to multiple controllers:

1. **Exception handling**: `@ExceptionHandler` methods
2. **Model attributes**: `@ModelAttribute` methods (adds to all model maps)
3. **Data binding**: `@InitBinder` methods (customize data binding)

`@RestControllerAdvice = @ControllerAdvice + @ResponseBody`

Scoping:
```java
@ControllerAdvice(assignableTypes = {UserController.class, OrderController.class})
@ControllerAdvice(basePackages = "com.example.api")
@ControllerAdvice(annotations = RestController.class)
```

Order: Multiple `@ControllerAdvice` can coexist; use `@Order` to prioritize.

---

### Q281. Validation mechanism?

Spring Boot uses **Bean Validation (JSR 380 / Jakarta Validation)** via Hibernate Validator:

```java
// DTO with constraints
public record CreateUserRequest(
    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 100)
    String name,
    
    @Email
    @NotNull
    String email,
    
    @Min(18) @Max(120)
    int age,
    
    @Pattern(regexp = "^\\+?[1-9]\\d{1,14}$")
    String phone
) {}

// Controller - @Valid triggers validation
@PostMapping("/users")
public UserDTO createUser(@Valid @RequestBody CreateUserRequest request) {
    return userService.create(request);
}

// Service-level validation
@Service
@Validated
public class UserService {
    public void updateEmail(@Valid @Email String email, Long userId) { ... }
}
```

Groups: Validate different constraints in different scenarios (`@Validated(CreateGroup.class)`).

Custom validator:
```java
@Target(FIELD) @Retention(RUNTIME)
@Constraint(validatedBy = UniqueEmailValidator.class)
public @interface UniqueEmail {
    String message() default "Email already exists";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

---

### Q282. Spring Data JPA basics?

Spring Data JPA eliminates boilerplate DAO code:

```java
// Repository - just declare the interface
public interface UserRepository extends JpaRepository<User, Long> {
    
    // Derived query from method name
    List<User> findByEmailAndActive(String email, boolean active);
    
    // Custom JPQL
    @Query("SELECT u FROM User u WHERE u.age > :minAge ORDER BY u.name")
    List<User> findUsersOlderThan(@Param("minAge") int minAge);
    
    // Native SQL
    @Query(value = "SELECT * FROM users WHERE region = ?1", nativeQuery = true)
    List<User> findByRegionNative(String region);
    
    // Pagination
    Page<User> findByDepartment(String dept, Pageable pageable);
    
    // Projection
    List<UserNameEmail> findAllProjectedBy();
    
    // Modifying
    @Modifying
    @Transactional
    @Query("UPDATE User u SET u.active = false WHERE u.lastLogin < :cutoff")
    int deactivateOldUsers(@Param("cutoff") LocalDateTime cutoff);
}
```

`JpaRepository` provides: `save()`, `findById()`, `findAll()`, `delete()`, `count()`, `existsById()`, pagination, sorting.

---

### Q283. Repository patterns?

Spring Data offers multiple repository interfaces:
- `CrudRepository<T, ID>`: Basic CRUD
- `PagingAndSortingRepository<T, ID>`: + pagination/sorting
- `JpaRepository<T, ID>`: + JPA-specific (flush, batch)

**Custom base repository**:
```java
@NoRepositoryBean
public interface BaseRepository<T, ID> extends JpaRepository<T, ID> {
    List<T> findByCreatedAfter(LocalDateTime date);
}
```

**Specification pattern** (for dynamic queries):
```java
public class UserSpecifications {
    public static Specification<User> hasEmail(String email) {
        return (root, query, cb) -> cb.equal(root.get("email"), email);
    }
    
    public static Specification<User> isActive() {
        return (root, query, cb) -> cb.isTrue(root.get("active"));
    }
}

// Use:
userRepository.findAll(
    UserSpecifications.hasEmail(email).and(UserSpecifications.isActive())
);
```

---

### Q284. Transaction management?

Spring's `@Transactional` declarative transaction management:

```java
@Service
@Transactional(readOnly = true) // default for all methods
public class OrderService {
    
    @Transactional // overrides class default - read-write
    public Order createOrder(CreateOrderRequest request) {
        Order order = new Order(request);
        order = orderRepository.save(order);
        inventoryService.reserve(order); // participates in same transaction
        return order;
    }
    
    @Transactional(
        propagation = Propagation.REQUIRES_NEW, // new transaction always
        isolation = Isolation.SERIALIZABLE,
        timeout = 30, // seconds
        rollbackFor = Exception.class, // rollback on all exceptions
        noRollbackFor = InformationalException.class
    )
    public void auditOrder(Long orderId) { ... }
}
```

**Propagation types**:
- `REQUIRED` (default): Join existing or create new
- `REQUIRES_NEW`: Always create new, suspend existing
- `NESTED`: Save point within existing transaction
- `SUPPORTS`: Join if exists, non-transactional if not
- `MANDATORY`: Must have existing transaction
- `NEVER`: Must NOT have existing transaction
- `NOT_SUPPORTED`: Suspend existing, run non-transactionally

---

### Q285. @Transactional pitfalls?

1. **Self-invocation**: Calling a `@Transactional` method from within the same class bypasses the proxy:
```java
@Service
public class OrderService {
    public void process() {
        createOrder(); // NO transaction! Self-call bypasses proxy
    }
    
    @Transactional
    public void createOrder() { ... }
}
// Fix: Inject self, or move to separate class, or use AspectJ mode
```

2. **Non-public methods**: `@Transactional` on private/protected/package methods is ignored (proxy limitation).

3. **Checked exceptions don't rollback by default**: Only `RuntimeException` and `Error`. Use `rollbackFor = Exception.class`.

4. **Catching exception inside transaction**: If you catch and swallow the exception, no rollback:
```java
@Transactional
void method() {
    try { riskyOperation(); }
    catch (Exception e) { log.error(e); } // COMMIT happens! No rollback
}
```

5. **readOnly misuse**: `readOnly = true` is a hint to optimize; doesn't prevent writes. Use it for query-only methods.

6. **Propagation.REQUIRES_NEW + exception handling**: Exception in outer transaction doesn't rollback inner, and vice versa.

---

### Q286. Lazy vs eager loading?

**Eager loading**: Fetch associated entities immediately when parent is loaded.
**Lazy loading**: Fetch associated entities only when accessed.

```java
@Entity
public class Order {
    @ManyToOne(fetch = FetchType.EAGER) // always loaded
    private Customer customer;
    
    @OneToMany(fetch = FetchType.LAZY) // loaded on access
    private List<OrderItem> items;
}
```

JPA defaults: `@OneToMany` and `@ManyToMany` default to LAZY. `@ManyToOne` and `@OneToOne` default to EAGER.

**LazyInitializationException**: Accessing a lazy collection outside a transaction (session is closed):
```java
Order order = orderRepository.findById(id).get();
// Transaction closed here if method wasn't @Transactional
order.getItems().size(); // LazyInitializationException!
```

Fixes:
- `@Transactional` on the service method
- `JOIN FETCH` in JPQL: `SELECT o FROM Order o JOIN FETCH o.items WHERE o.id = :id`
- `@EntityGraph`
- Projections

---

### Q287. N+1 problem?

Classic N+1: Load N entities, then execute 1 query per entity to load associations = N+1 queries total.

```java
// N+1 problem
List<Order> orders = orderRepository.findAll(); // 1 query
for (Order o : orders) {
    System.out.println(o.getCustomer().getName()); // N queries!
}
// Total: 1 + N queries
```

**Solutions**:

1. **JOIN FETCH**:
```java
@Query("SELECT o FROM Order o JOIN FETCH o.customer WHERE o.status = :status")
List<Order> findByStatusWithCustomer(@Param("status") String status);
```

2. **@EntityGraph**:
```java
@EntityGraph(attributePaths = {"customer", "items"})
List<Order> findByStatus(String status);
```

3. **Batch fetching**: `@BatchSize(size = 50)` on association – fetches 50 at a time instead of 1.

4. **Projection/DTO**: Fetch only needed data.

5. **Enable `spring.jpa.properties.hibernate.default_batch_fetch_size=50`**: Global batch fetching.

Detect N+1: Enable SQL logging, use `datasource-proxy`, Hibernate's statistics, tools like `p6spy`.

---

### Q288. Paging and sorting?

```java
// Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Page<User> findByDepartment(String dept, Pageable pageable);
    Slice<User> findByActive(boolean active, Pageable pageable); // no count query
}

// Service
Pageable pageable = PageRequest.of(
    pageNumber,      // 0-based
    pageSize,
    Sort.by("name").ascending().and(Sort.by("age").descending())
);

Page<User> page = userRepository.findByDepartment("Engineering", pageable);
page.getContent();       // List<User> for current page
page.getTotalElements(); // total count (runs COUNT query)
page.getTotalPages();
page.getNumber();        // current page
page.hasNext();

// REST controller
@GetMapping("/users")
public Page<UserDTO> getUsers(
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(defaultValue = "20") int size,
    @RequestParam(defaultValue = "name") String sort) {
    
    Pageable pageable = PageRequest.of(page, size, Sort.by(sort));
    return userRepository.findAll(pageable).map(userMapper::toDto);
}
```

**`Slice` vs `Page`**: `Slice` doesn't execute COUNT query (better performance when total count not needed – e.g., infinite scroll).

---

### Q289. REST best practices?

1. **Use proper HTTP methods**: GET (read), POST (create), PUT (full update), PATCH (partial), DELETE
2. **Meaningful URIs**: `/users/{id}/orders` not `/getUserOrders`
3. **Plural nouns**: `/users` not `/user`
4. **HTTP status codes**: 200 OK, 201 Created, 204 No Content, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Unprocessable, 500 Internal Error
5. **Versioning**: `/api/v1/users` or `Accept: application/vnd.myapp.v1+json`
6. **Pagination**: Consistent page/size parameters, include metadata
7. **Filtering/sorting**: Query params: `/users?status=active&sort=name,asc`
8. **Consistent error responses**: Problem Details (RFC 7807)
9. **Idempotency**: PUT, DELETE should be idempotent
10. **HATEOAS**: Link to related resources (see Q290)
11. **Security**: Always validate input, use HTTPS, authentication on all endpoints
12. **Documentation**: OpenAPI/Swagger
13. **Content negotiation**: Support `application/json`; potentially XML if required

---

### Q290. HATEOAS?

**HATEOAS (Hypermedia As The Engine Of Application State)**: REST responses include links to related actions/resources.

```json
{
  "id": 123,
  "name": "Alice",
  "email": "alice@example.com",
  "_links": {
    "self": { "href": "/api/users/123" },
    "orders": { "href": "/api/users/123/orders" },
    "update": { "href": "/api/users/123", "method": "PUT" },
    "delete": { "href": "/api/users/123", "method": "DELETE" }
  }
}
```

Spring HATEOAS:
```java
@GetMapping("/users/{id}")
public EntityModel<UserDTO> getUser(@PathVariable Long id) {
    UserDTO user = userService.findById(id);
    return EntityModel.of(user,
        linkTo(methodOn(UserController.class).getUser(id)).withSelfRel(),
        linkTo(methodOn(OrderController.class).getUserOrders(id)).withRel("orders")
    );
}
```

In practice: Full HATEOAS is rarely implemented in enterprise APIs. Self links and related resource links are the most common usage.

---

### Q291. Security basics?

Spring Security added via `spring-boot-starter-security`. Auto-configures:
- HTTP Basic auth
- Default in-memory user
- CSRF protection
- Secure all endpoints

Custom configuration (Spring Boot 3 / Spring Security 6):
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable()) // for REST APIs
            .sessionManagement(sm -> sm.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .requestMatchers(HttpMethod.GET, "/api/**").hasAnyRole("USER", "ADMIN")
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);
        return http.build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(12); // strength 12
    }
}
```

---

### Q292. JWT authentication?

```java
// JWT Filter
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                    HttpServletResponse response, 
                                    FilterChain chain) throws IOException, ServletException {
        String authHeader = request.getHeader("Authorization");
        
        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            chain.doFilter(request, response);
            return;
        }
        
        String token = authHeader.substring(7);
        String username = jwtService.extractUsername(token);
        
        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);
            
            if (jwtService.isTokenValid(token, userDetails)) {
                UsernamePasswordAuthenticationToken authToken = 
                    new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        chain.doFilter(request, response);
    }
}

// JWT Service
@Service
public class JwtService {
    @Value("${jwt.secret}") private String secret;
    @Value("${jwt.expiration:3600}") private long expiration;
    
    public String generateToken(UserDetails userDetails) {
        return Jwts.builder()
            .subject(userDetails.getUsername())
            .issuedAt(new Date())
            .expiration(new Date(System.currentTimeMillis() + expiration * 1000))
            .claim("roles", userDetails.getAuthorities())
            .signWith(getSigningKey())
            .compact();
    }
}
```

Key security considerations:
- Store JWT secret securely (Vault, Secrets Manager)
- Short expiration + refresh token pattern
- Token invalidation/blacklist for logout
- Validate issuer, audience, not-before claims
- Use RS256 (asymmetric) over HS256 for microservices

---

### Q293. OAuth2 with Spring?

Spring Security OAuth2 (Spring Boot 3):

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: ${GOOGLE_CLIENT_ID}
            client-secret: ${GOOGLE_CLIENT_SECRET}
            scope: openid, profile, email
      resourceserver:
        jwt:
          issuer-uri: https://accounts.google.com
```

**Resource Server** (validate JWT from auth server):
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtAuthenticationConverter(jwtAuthenticationConverter()))
            );
        return http.build();
    }
    
    @Bean
    JwtAuthenticationConverter jwtAuthenticationConverter() {
        JwtGrantedAuthoritiesConverter converter = new JwtGrantedAuthoritiesConverter();
        converter.setAuthoritiesClaimName("roles");
        converter.setAuthorityPrefix("ROLE_");
        JwtAuthenticationConverter jwtConverter = new JwtAuthenticationConverter();
        jwtConverter.setJwtGrantedAuthoritiesConverter(converter);
        return jwtConverter;
    }
}
```

Grant types: Authorization Code (web), Client Credentials (machine-to-machine), Device Flow (IoT).

---

### Q294. Caching in Spring?

```java
// Enable caching
@SpringBootApplication
@EnableCaching
public class App { }

// Use caching
@Service
public class ProductService {
    
    @Cacheable(value = "products", key = "#id", unless = "#result == null")
    public Product getProduct(Long id) {
        return repository.findById(id).orElse(null); // DB call only on cache miss
    }
    
    @CachePut(value = "products", key = "#product.id") // always execute + update cache
    public Product updateProduct(Product product) {
        return repository.save(product);
    }
    
    @CacheEvict(value = "products", key = "#id") // remove from cache
    public void deleteProduct(Long id) {
        repository.deleteById(id);
    }
    
    @CacheEvict(value = "products", allEntries = true) // clear entire cache
    @Scheduled(fixedRate = 3600000) // hourly
    public void evictAllProducts() { }
    
    @Caching(evict = {
        @CacheEvict("products"),
        @CacheEvict(value = "productsByCategory", key = "#product.category")
    })
    public void processProduct(Product product) { }
}
```

Default cache: `ConcurrentMapCacheManager` (in-memory, no TTL). Configure TTL with Caffeine, Redis, etc.

---

### Q295. Redis integration?

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
      password: ${REDIS_PASSWORD}
      lettuce:
        pool:
          max-active: 10
```

**Using as cache**:
```java
@Bean
public CacheManager cacheManager(RedisConnectionFactory connectionFactory) {
    RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
        .entryTtl(Duration.ofMinutes(30))
        .serializeValuesWith(RedisSerializationContext.SerializationPair
            .fromSerializer(new GenericJackson2JsonRedisSerializer()));
    
    return RedisCacheManager.builder(connectionFactory)
        .cacheDefaults(config)
        .withCacheConfiguration("products", 
            RedisCacheConfiguration.defaultCacheConfig().entryTtl(Duration.ofHours(1)))
        .build();
}
```

**Using `RedisTemplate` directly**:
```java
@Autowired RedisTemplate<String, Object> redisTemplate;

redisTemplate.opsForValue().set("key", value, Duration.ofMinutes(30));
Object value = redisTemplate.opsForValue().get("key");
redisTemplate.opsForHash().put("hash", "field", "value");
redisTemplate.opsForList().leftPush("list", "element");
```

**Session storage**:
```xml
<dependency>spring-session-data-redis</dependency>
```
```java
@EnableRedisHttpSession(maxInactiveIntervalInSeconds = 1800)
```

---

### Q296. Microservices with Spring Boot?

Spring Boot is a natural fit for microservices:

**Principles**:
- Single responsibility: one service per bounded context
- Independent deployability
- Own data store per service
- Communicate via HTTP/REST or messaging (Kafka, RabbitMQ)

**Common patterns**:
```java
// Service discovery (Eureka)
@EnableEurekaClient

// Load balancing
@LoadBalanced
@Bean RestTemplate restTemplate() { return new RestTemplate(); }

// Feign client (declarative HTTP)
@FeignClient(name = "order-service")
public interface OrderClient {
    @GetMapping("/orders/{id}")
    OrderDTO getOrder(@PathVariable Long id);
}

// Config server
spring.config.import=configserver:http://config-server:8888

// Distributed tracing (Micrometer Tracing + Zipkin)
management.tracing.sampling.probability=1.0
```

**Challenges**: Network latency, eventual consistency, distributed transactions (Saga pattern), service discovery, centralized logging, distributed tracing.

---

### Q297. Spring Cloud overview?

Spring Cloud provides tools for distributed systems:

| Component | Purpose |
|-----------|---------|
| Spring Cloud Config | Centralized config server |
| Netflix Eureka | Service registry/discovery |
| Spring Cloud Gateway | API gateway, routing, rate limiting |
| OpenFeign | Declarative REST client |
| Resilience4j | Circuit breaker, retry, rate limiter |
| Spring Cloud Sleuth | Distributed tracing (replaced by Micrometer Tracing) |
| Zipkin | Trace visualization |
| Spring Cloud Bus | Propagate config changes via message broker |
| Spring Cloud Stream | Message-driven microservices (Kafka/RabbitMQ) |

---

### Q298. Circuit breaker?

Circuit breaker prevents cascading failures when a dependency is down:

**States**:
- **CLOSED**: Normal operation; requests pass through
- **OPEN**: Too many failures; requests fail fast (no call to dependency)
- **HALF-OPEN**: After wait period, allow limited requests to test recovery

```java
// Resilience4j
@CircuitBreaker(name = "orderService", fallbackMethod = "fallbackOrder")
@Retry(name = "orderService")
@TimeLimiter(name = "orderService")
public CompletableFuture<Order> getOrder(Long id) {
    return CompletableFuture.supplyAsync(() -> orderClient.getOrder(id));
}

public CompletableFuture<Order> fallbackOrder(Long id, Exception ex) {
    log.warn("Circuit open for order {}: {}", id, ex.getMessage());
    return CompletableFuture.completedFuture(Order.empty(id));
}
```

```yaml
resilience4j:
  circuitbreaker:
    instances:
      orderService:
        slidingWindowSize: 10
        failureRateThreshold: 50
        waitDurationInOpenState: 30s
        permittedNumberOfCallsInHalfOpenState: 3
```

---

### Q299. Resilience4j?

Resilience4j provides multiple resilience patterns:

1. **Circuit Breaker**: See Q298
2. **Retry**:
```java
@Retry(name = "externalApi", fallbackMethod = "fallback")
public String callApi() { ... }
```
```yaml
resilience4j.retry.instances.externalApi:
  maxAttempts: 3
  waitDuration: 500ms
  enableExponentialBackoff: true
  exponentialBackoffMultiplier: 2
```

3. **Rate Limiter**:
```java
@RateLimiter(name = "apiGateway")
public String callGateway() { ... }
```

4. **Bulkhead**: Limit concurrent calls (Thread pool or Semaphore)
```java
@Bulkhead(name = "service", type = Bulkhead.Type.SEMAPHORE)
```

5. **Time Limiter**: Timeout for async calls

6. **Cache**: Simple result caching

Combining:
```java
@CircuitBreaker(name = "svc", fallbackMethod = "fb")
@Retry(name = "svc")
@RateLimiter(name = "svc")
public Result callService() { ... }
// Order: RateLimiter → CircuitBreaker → Retry
```

---

### Q300. Production readiness checklist?

A Spring Boot service is production-ready when it has:

**Observability**:
- [ ] Structured logging with correlation IDs (MDC)
- [ ] Metrics exposed (`/actuator/prometheus` or micrometer)
- [ ] Distributed tracing (Micrometer Tracing + Zipkin/Jaeger)
- [ ] Health checks for orchestration (`/actuator/health/liveness`, `readiness`)
- [ ] Alerting on error rate, latency, saturation

**Resilience**:
- [ ] Circuit breakers for external dependencies
- [ ] Retry with backoff
- [ ] Timeouts on all external calls (HTTP client, DB, etc.)
- [ ] Bulkheads (bounded thread pools)
- [ ] Graceful shutdown (`spring.lifecycle.timeout-per-shutdown-phase=30s`)

**Security**:
- [ ] Authentication + authorization
- [ ] Secrets in vault/environment, not in code
- [ ] HTTPS enforced
- [ ] Input validation
- [ ] Dependency vulnerability scan (OWASP Dependency Check, Snyk)
- [ ] Actuator endpoints secured or restricted

**Performance**:
- [ ] Connection pool tuned (HikariCP: `maximum-pool-size`)
- [ ] Database indexes for common queries
- [ ] Caching for expensive/repeated operations
- [ ] Thread pool sized appropriately
- [ ] JVM heap and GC tuned
- [ ] Load test results acceptable

**Deployment**:
- [ ] Dockerfile with non-root user, minimal base image
- [ ] Kubernetes liveness/readiness probes configured
- [ ] Resource requests/limits set
- [ ] Rolling update strategy
- [ ] `-XX:+UseContainerSupport` for JVM container awareness
- [ ] Environment-specific configuration externalized

**Testing**:
- [ ] Unit tests (target >80% coverage on business logic)
- [ ] Integration tests (Testcontainers for real DB)
- [ ] Contract tests (Pact or Spring Cloud Contract)
- [ ] Load test results meet SLA

**Documentation**:
- [ ] OpenAPI/Swagger (`springdoc-openapi`)
- [ ] README with runbook
- [ ] Architecture decision records (ADRs)

**Ops**:
- [ ] Log rotation configured
- [ ] Heap dump path set (`-XX:HeapDumpPath`)
- [ ] OOM action: `-XX:+ExitOnOutOfMemoryError`
- [ ] JFR enabled for profiling capability
- [ ] Runbook for common failure scenarios
