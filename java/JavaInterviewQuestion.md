# Java & Spring Boot Interview Guide

This guide provides a comprehensive overview of key Java and Spring Boot concepts, common interview questions, and practical tips for aspiring developers. It includes detailed explanations, code examples, and additional questions to help you prepare effectively for interviews.

---

## Table of Contents

1. [Java Basics](#java-basics)  
2. [Memory Management & Collections](#memory-management--collections)  
3. [Multithreading](#multithreading)  
4. [Web Services & Spring](#web-services--spring)  
5. [Java 8 Stream API](#java-8-stream-api)  
6. [Project & Technical Questions](#project--technical-questions)  
7. [Additional Common Interview Questions](#additional-common-interview-questions)  
8. [Tips for Aspiring Java Developers](#tips-for-aspiring-java-developers)  

---

## 1. Java Basics

### 1.1 Difference between JDK, JRE, and JVM

| Term | Description |
|------|-------------|
| JVM  | Java Virtual Machine: Executes Java bytecode, enabling platform independence. |
| JRE  | Java Runtime Environment: Includes JVM and core libraries for running Java applications. |
| JDK  | Java Development Kit: Includes JRE and tools like `javac` (compiler) and `jdb` (debugger). |

### 1.2 How does Java achieve platform independence?

Java compiles source code into **bytecode**, a platform-agnostic intermediate representation. The JVM interprets or compiles bytecode to machine code specific to the host system, allowing Java programs to run on any platform with a compatible JVM.

### 1.3 Checked vs Unchecked Exceptions

- **Checked Exceptions**: Must be declared or handled.  
  Examples: `IOException`, `SQLException`.
- **Unchecked Exceptions**: Extend `RuntimeException`; no explicit handling required.  
  Examples: `NullPointerException`, `ArithmeticException`.

```java
// Checked Exception
try {
    FileInputStream file = new FileInputStream("file.txt");
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```


// Unchecked Exception
String str = null;
System.out.println(str.length()); // Throws NullPointerException


# Java & Spring Boot Interview Guide (Part 2)

---

## 1.4 Difference between String, StringBuilder, and StringBuffer

| Class         | Mutability | Thread-Safety       | Performance                         |
|---------------|------------|-------------------|------------------------------------|
| String        | Immutable  | N/A               | Slower for concatenation           |
| StringBuilder | Mutable    | Not thread-safe   | Fast for single-threaded operations|
| StringBuffer  | Mutable    | Thread-safe       | Slightly slower than StringBuilder |

```java
String str = "Hello";
str += " World"; // Creates new String object

StringBuilder sb = new StringBuilder("Hello");
sb.append(" World"); // Modifies existing object
```

## 1.5 Difference between final, finally, and finalize()

final: Prevents modification of variables, methods, or classes.

finally: Executes regardless of exceptions in a try-catch block.

finalize(): Deprecated; called by GC before object reclamation.

final int CONSTANT = 10;
```java
try {
    // Risky operation
} catch (Exception e) {
    e.printStackTrace();
} finally {
    // Cleanup code
}
```

## 1.6 Abstract Class vs Interface

-**Abstract Class**: Can have abstract/concrete methods, instance variables, constructors; supports single inheritance.

-**Interface**: Only abstract methods (default/static allowed since Java 8); supports multiple inheritance.

```java
abstract class Animal {
    abstract void sound();
    void eat() { System.out.println("Eating"); }
}

interface Pet {
    void play();
}

class Dog extends Animal implements Pet {
    void sound() { System.out.println("Bark"); }
    public void play() { System.out.println("Playing"); }
}
```
# 2. Memory Management & Collections

## 2.1 Garbage Collection in Java

Automatically reclaims memory from unreferenced objects.
**Algorithms:** Mark & Sweep, Generational GC.
```java
Object obj = new Object();
obj = null;          // Eligible for GC
System.gc();         // Suggests GC
```

## 2.2 ArrayList vs LinkedList
-Feature	ArrayList	LinkedList
-Access Time	O(1)	O(n)
-Insert/Delete	O(n)	O(1)
-Memory	    Less overhead	More overhead

```java
List<String> arrayList = new ArrayList<>();
arrayList.add("A"); 
arrayList.get(0);

List<String> linkedList = new LinkedList<>();
linkedList.addFirst("A");
```

## 2.3 HashMap vs ConcurrentHashMap

-**HashMap:** Not thread-safe

-**ConcurrentHashMap:** Thread-safe with segment locking
```java
Map<String, Integer> hashMap = new HashMap<>();
hashMap.put("Key", 1);

ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("Key", 1);
```

## 2.4 LRU Cache using LinkedHashMap
```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LRUCache extends LinkedHashMap<Integer, String> {
    private final int MAX_CAPACITY;

    public LRUCache(int capacity) {
        super(capacity, 0.75f, true); // access-order
        this.MAX_CAPACITY = capacity;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, String> eldest) {
        return size() > MAX_CAPACITY;
    }
}
```

## 2.5 HashSet vs TreeSet

-**HashSet:** O(1) operations, unordered

-**TreeSet:** O(log n), sorted order

```java
Set<String> hashSet = new HashSet<>();
hashSet.add("B"); hashSet.add("A");

Set<String> treeSet = new TreeSet<>();
treeSet.add("B"); treeSet.add("A"); // Sorted: A, B
```

# 3. Multithreading
## 3.1 Thread Lifecycle

**States:** NEW → RUNNABLE → BLOCKED → WAITING/TIMED_WAITING → TERMINATED

```java
Thread t = new Thread(() -> System.out.println("Running"));
t.start(); // NEW -> RUNNABLE
t.join();  // Wait for termination

3.2 Creating Threads
class MyThread extends Thread {
    public void run() { System.out.println("Thread running"); }
}

class MyRunnable implements Runnable {
    public void run() { System.out.println("Runnable running"); }
}

MyThread t1 = new MyThread();
t1.start();

Thread t2 = new Thread(new MyRunnable());
t2.start();
```

## 3.3 Volatile vs Atomic

-**volatile:** Visibility across threads

-**Atomic:** Thread-safe atomic operations
```java
volatile int counter = 0;
AtomicInteger atomicCounter = new AtomicInteger(0);
atomicCounter.incrementAndGet();
```

## 3.4 ExecutorService
ExecutorService executor = Executors.newFixedThreadPool(2);
executor.submit(() -> System.out.println("Task 1"));
executor.submit(() -> System.out.println("Task 2"));
executor.shutdown();

# 4. Web Services & Spring
## 4.1 REST vs SOAP
Feature	REST	SOAP
Protocol	HTTP	Any
Data Format	JSON/XML	XML
State	Stateless	Can be stateful
Simplicity	Lightweight	Heavyweight
```java
@RestController
@RequestMapping("/api")
public class MyController {
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, REST!";
    }
}
```

## 4.2 Spring Dependency Injection (DI)
```java
@Service
public class UserService {
    private final UserRepository repository;

    @Autowired
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```
## 4.3 Spring Boot Layers

Controller/Web: Handles HTTP requests

Service: Business logic

Repository: Data access

Entity: Data models
```java
@Entity
public class User {
    @Id
    private Long id;
    private String name;
}

@Repository
public interface UserRepository extends JpaRepository<User, Long> {}

@Service
public class UserService {
    @Autowired
    private UserRepository repository;
}

@RestController
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserService service;
}
```

## 4.4 Common Annotations
Annotation	Purpose
@Component	Marks a class as Spring bean
@Service	Business logic
@Repository	Data access with exception translation
@Autowired	Dependency injection

## 4.5 Transactions in Spring Boot
```java
@Service
public class UserService {
    @Transactional
    public void saveUser(User user) {
        userRepository.save(user);
    }
}
```
## 4.6 Securing REST APIs

Spring Security, JWT, OAuth2

Enforce HTTPS, RBAC, input validation
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests()
            .requestMatchers("/api/**").authenticated()
            .and().oauth2ResourceServer().jwt();
        return http.build();
    }
}
```

## 4.7 CI/CD Pipeline

**flow-======>>>>>**
Commit to Git -> Build & test (Maven/Gradle, JUnit)-> Package into Docker-> Deploy to Kubernetes/EC2->Monitor (Prometheus/Grafana)

## 4.8 Kubernetes vs Docker

Docker: Runs containers

Kubernetes: Orchestrates containers (scaling, load balancing)

## 4.9 Spring Boot Actuator

Provides monitoring endpoints like /health, /metrics.

# 5. Java 8 Stream API
```java
List<Employee> filtered = employees.stream()
    .filter(e -> e.getAge() > 26)
    .collect(Collectors.toList());

Map<String, List<Employee>> byDept = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDept));

double maxSalary = employees.stream()
    .mapToDouble(Employee::getSalary)
    .max()
    .orElse(0);
```

# 6. Project & Technical Questions

Object methods: toString(), equals(), hashCode(), clone(), finalize(), getClass(), wait(), notify(), notifyAll()

Global Exception Handling: @ControllerAdvice, @ExceptionHandler

Swagger: API documentation

Logging: Log4j, SLF4J, Logback

JWT: Authentication & refresh tokens

Microservices: REST, gRPC, Message Queues

Service Discovery: Eureka, Consul

Fault-Tolerance: Circuit Breaker, Retry, Fallback

@RestController vs @Controller: Returns JSON vs view

# 7. Additional Common Interview Questions

== vs .equals(): Reference vs content

HashCode Contract: Equal objects must have same hashcode

Synchronized vs Lock: Implicit vs explicit locking

Optional: Avoid NullPointerException

Serializable vs Externalizable: Automatic vs custom serialization

AutoConfiguration: Spring Boot auto-configures beans

Logging: Configure via application.properties or Logback

AOP: Cross-cutting concerns

# 8. Tips for Aspiring Java Developers

Master Java 8+: Streams, Lambdas, Optional

Build projects: Spring Boot with security, logging, microservices

Write clean code: SOLID principles, testable code

Explain decisions: Justify technical choices in interviews

Practice system design: Microservices, CI/CD, fault-tolerance

