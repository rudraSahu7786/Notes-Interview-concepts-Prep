# Java & Spring Boot Interview Questions - Complete Study Guide

## Core Java

### 1. Difference between HashMap, ConcurrentHashMap, and Hashtable

**HashMap:**
- Not thread-safe
- Allows null keys and values
- Better performance in single-threaded environments
- Introduced in Java 1.2
- Uses array of linked lists (buckets), converts to trees when collision threshold exceeded

**ConcurrentHashMap:**
- Thread-safe without synchronizing entire map
- Uses segment-based locking (Java 7) or CAS operations (Java 8+)
- Does not allow null keys or values
- Better performance than Hashtable in multi-threaded environments
- Provides atomic operations like putIfAbsent()

**Hashtable:**
- Thread-safe but synchronizes entire map
- Does not allow null keys or values
- Legacy class from Java 1.0
- Poor performance in multi-threaded environments due to full synchronization
- All methods are synchronized

### 2. Garbage Collection (CMS vs G1 GC)

**CMS (Concurrent Mark Sweep):**
- Low-latency collector
- Concurrent collection during application execution
- Does not compact memory (fragmentation issues)
- Deprecated in Java 9, removed in Java 14
- Uses mark-and-sweep algorithm
- Can suffer from concurrent mode failures

**G1 GC (Garbage First):**
- Low-latency collector with predictable pause times
- Divides heap into regions
- Performs both concurrent and parallel collection
- Compacts memory automatically
- Default collector from Java 9+
- Better for large heaps (>4GB)
- Allows setting pause time goals

### 3. Difference between synchronized, ReentrantLock, and ReadWriteLock

**synchronized:**
- Built-in Java keyword
- Automatic lock acquisition and release
- Cannot be interrupted
- No timeout mechanism
- Unfair locking by default

**ReentrantLock:**
- Explicit lock/unlock operations
- Interruptible lock acquisition
- Timeout support with tryLock()
- Fair/unfair locking options
- Condition variables support
- Must manually release in finally block

**ReadWriteLock:**
- Separate read and write locks
- Multiple readers can access simultaneously
- Exclusive write access
- Better performance for read-heavy scenarios
- ReentrantReadWriteLock is common implementation

```java
// Example usage
ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();
ReadLock readLock = rwLock.readLock();
WriteLock writeLock = rwLock.writeLock();
```

### 4. Difference between Comparable vs Comparator

**Comparable:**
- Natural ordering of objects
- Implemented by the class being sorted
- Single sorting logic
- compareTo() method
- Part of java.lang package

**Comparator:**
- External sorting logic
- Can define multiple sorting strategies
- compare() method
- Part of java.util package
- Can sort objects without modifying their class

```java
// Comparable example
class Employee implements Comparable<Employee> {
    public int compareTo(Employee other) {
        return this.id - other.id;
    }
}

// Comparator example
Comparator<Employee> nameComparator = (e1, e2) -> e1.getName().compareTo(e2.getName());
```

### 5. Java Memory Model (Heap, Stack, Metaspace)

**Heap:**
- Stores objects and instance variables
- Shared among all threads
- Divided into Young Generation (Eden, S0, S1) and Old Generation
- Garbage collected
- OutOfMemoryError when full

**Stack:**
- Stores method calls, local variables, partial results
- Thread-specific (each thread has its own stack)
- LIFO structure
- StackOverflowError when full
- Automatically managed

**Metaspace (Java 8+):**
- Stores class metadata
- Replaced PermGen space
- Uses native memory
- Auto-expands by default
- Better memory management than PermGen

### 6. How does volatile work internally?

**volatile keyword:**
- Ensures visibility of changes across threads
- Prevents instruction reordering around volatile variables
- Creates memory barriers/fences
- Reads/writes directly from/to main memory
- Does not provide atomicity for compound operations

**Internal mechanism:**
- CPU cache coherence protocols
- Memory barriers prevent reordering
- MESI protocol ensures cache consistency
- LoadLoad, LoadStore, StoreLoad, StoreStore barriers

### 7. Difference between wait(), sleep(), join()

**wait():**
- Releases monitor lock
- Must be called within synchronized block
- Thread waits until notify()/notifyAll()
- Part of Object class
- Can be awakened by interruption

**sleep():**
- Does not release locks
- Static method of Thread class
- Pauses thread for specified time
- Can be interrupted
- Thread automatically resumes after timeout

**join():**
- Waits for another thread to complete
- Current thread blocks until target thread dies
- Does not release locks
- Can specify timeout
- Used for thread coordination

### 8. How to resolve deadlock, starvation, livelock?

**Deadlock Prevention:**
- Avoid circular wait conditions
- Use timeout with tryLock()
- Order lock acquisition consistently
- Use single lock for related operations

**Starvation Prevention:**
- Use fair locking mechanisms
- Implement priority scheduling
- Avoid long-running critical sections
- Use ReentrantLock with fair=true

**Livelock Prevention:**
- Introduce randomization in retry logic
- Use exponential backoff
- Implement proper coordination mechanisms
- Avoid overly aggressive retry strategies

### 9. How do you make a class immutable?

**Steps to create immutable class:**
1. Make class final (prevent inheritance)
2. Make all fields private and final
3. No setter methods
4. Initialize via constructor only
5. Return copies of mutable objects in getters
6. Ensure no methods modify object state

```java
public final class ImmutablePerson {
    private final String name;
    private final List<String> hobbies;
    
    public ImmutablePerson(String name, List<String> hobbies) {
        this.name = name;
        this.hobbies = new ArrayList<>(hobbies); // Defensive copy
    }
    
    public String getName() { return name; }
    
    public List<String> getHobbies() {
        return new ArrayList<>(hobbies); // Return copy
    }
}
```

### 10. Best practices for checked vs unchecked exceptions

**Checked Exceptions:**
- Use for recoverable conditions
- Client must handle or declare
- Examples: IOException, SQLException
- Good for API boundaries
- Document expected exceptions

**Unchecked Exceptions:**
- Use for programming errors
- Runtime exceptions
- Examples: NullPointerException, IllegalArgumentException
- Don't catch unless you can meaningfully recover
- Fail fast principle

**Best Practices:**
- Prefer unchecked exceptions for most cases
- Use checked exceptions sparingly
- Create custom exception hierarchies
- Include meaningful error messages
- Consider exception translation at boundaries

## Spring & Spring Boot

### 11. Spring Bean Lifecycle

**Bean Lifecycle Phases:**
1. **Instantiation** - Constructor called
2. **Population** - Dependencies injected
3. **BeanNameAware** - setBeanName() called
4. **BeanFactoryAware** - setBeanFactory() called
5. **ApplicationContextAware** - setApplicationContext() called
6. **PreInitialization** - BeanPostProcessor.postProcessBeforeInitialization()
7. **InitializingBean** - afterPropertiesSet() called
8. **Custom Init** - @PostConstruct or init-method
9. **PostInitialization** - BeanPostProcessor.postProcessAfterInitialization()
10. **Ready for use**
11. **PreDestroy** - @PreDestroy called
12. **DisposableBean** - destroy() called
13. **Custom Destroy** - destroy-method called

```java
@Component
public class LifecycleBean implements BeanNameAware, InitializingBean, DisposableBean {
    
    @PostConstruct
    public void init() {
        System.out.println("PostConstruct called");
    }
    
    @Override
    public void afterPropertiesSet() {
        System.out.println("afterPropertiesSet called");
    }
    
    @PreDestroy
    public void cleanup() {
        System.out.println("PreDestroy called");
    }
}
```

### 12. Difference between @Component, @Service, @Repository, @Controller

**@Component:**
- Generic stereotype annotation
- Base annotation for all Spring-managed components
- Used for general purpose beans

**@Service:**
- Specialization of @Component
- Business logic layer
- Service facade pattern
- May have additional semantics in future

**@Repository:**
- Specialization of @Component
- Data Access Object (DAO) layer
- Automatic exception translation
- PersistenceExceptionTranslationPostProcessor support

**@Controller:**
- Specialization of @Component
- Web layer (MVC controllers)
- Request mapping support
- Typically used with @RequestMapping

### 13. Spring Boot Auto-configuration

**How it works:**
- Uses @EnableAutoConfiguration annotation
- Scans classpath for specific libraries
- Applies conditional configuration based on presence/absence of beans
- Uses @ConditionalOnClass, @ConditionalOnMissingBean annotations
- Configuration classes in spring.factories or auto-configuration imports

**Key Components:**
- AutoConfigurationImportSelector
- SpringFactoriesLoader
- Conditional annotations
- Configuration properties

**Customization:**
- exclude specific auto-configurations
- Override with custom beans
- Use application.properties/yml
- Create custom auto-configuration

### 14. Role of Spring Boot Starter Dependencies

**Purpose:**
- Opinionated dependency management
- Reduce boilerplate configuration
- Version compatibility guaranteed
- Transititive dependency resolution

**Common Starters:**
- spring-boot-starter-web (Web applications)
- spring-boot-starter-data-jpa (JPA support)
- spring-boot-starter-security (Security)
- spring-boot-starter-test (Testing)
- spring-boot-starter-actuator (Monitoring)

**Benefits:**
- Simplified dependency management
- Consistent versions
- Best practice configurations
- Faster project setup

### 15. Spring Boot Profiles Configuration

**Profile Definition:**
```properties
# application-dev.properties
server.port=8080
logging.level.com.example=DEBUG

# application-prod.properties
server.port=80
logging.level.com.example=INFO
```

**Activation Methods:**
- Command line: `--spring.profiles.active=dev`
- Environment variable: `SPRING_PROFILES_ACTIVE=dev`
- application.properties: `spring.profiles.active=dev`
- Programmatically: `SpringApplication.setAdditionalProfiles()`

**Profile-specific Beans:**
```java
@Configuration
@Profile("dev")
public class DevConfiguration {
    // Development-specific beans
}

@Component
@Profile("!prod")
public class NonProdComponent {
    // Active in all profiles except prod
}
```

### 16. Spring Security (Authentication vs Authorization)

**Authentication:**
- Verifies "who you are"
- Login process
- Credentials validation
- Principal identification

**Authorization:**
- Verifies "what you can do"
- Access control
- Permissions and roles
- Resource protection

**Implementation:**
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin(Customizer.withDefaults())
            .build();
    }
}
```

**JWT Authentication:**
```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
            HttpServletResponse response, FilterChain filterChain) {
        String token = extractToken(request);
        if (token != null && jwtUtil.validateToken(token)) {
            Authentication auth = jwtUtil.getAuthentication(token);
            SecurityContextHolder.getContext().setAuthentication(auth);
        }
        filterChain.doFilter(request, response);
    }
}
```

### 17. JWT Authentication Implementation

**JWT Structure:**
- Header: Algorithm and token type
- Payload: Claims (user info, expiration)
- Signature: Verification hash

**Implementation Steps:**
1. Generate JWT on successful login
2. Include JWT in Authorization header
3. Validate JWT on each request
4. Extract user information from claims

```java
@Service
public class JwtService {
    
    private String SECRET_KEY = "mySecretKey";
    
    public String generateToken(UserDetails userDetails) {
        return Jwts.builder()
            .setSubject(userDetails.getUsername())
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 24))
            .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
            .compact();
    }
    
    public Boolean validateToken(String token, UserDetails userDetails) {
        String username = extractUsername(token);
        return username.equals(userDetails.getUsername()) && !isTokenExpired(token);
    }
}
```

### 18. Difference between @RestController vs @Controller

**@Controller:**
- Returns view names (String)
- Used with template engines (Thymeleaf, JSP)
- Needs @ResponseBody for JSON responses
- Traditional MVC pattern

**@RestController:**
- Combination of @Controller + @ResponseBody
- Returns data directly (JSON/XML)
- RESTful web services
- No view resolution

```java
@Controller
public class WebController {
    @RequestMapping("/page")
    public String getPage(Model model) {
        return "homepage"; // Returns view name
    }
    
    @RequestMapping("/api/data")
    @ResponseBody
    public User getData() {
        return new User(); // Returns JSON
    }
}

@RestController
public class ApiController {
    @RequestMapping("/api/users")
    public List<User> getUsers() {
        return userService.getAllUsers(); // Automatically converts to JSON
    }
}
```

### 19. Circular Dependencies Resolution in Spring

**Detection:**
Spring detects circular dependencies during bean creation and throws BeanCurrentlyInCreationException.

**Resolution Strategies:**

1. **Constructor Injection with @Lazy:**
```java
@Service
public class ServiceA {
    private final ServiceB serviceB;
    
    public ServiceA(@Lazy ServiceB serviceB) {
        this.serviceB = serviceB;
    }
}
```

2. **Setter/Field Injection:**
```java
@Service
public class ServiceA {
    @Autowired
    private ServiceB serviceB;
}
```

3. **ApplicationContextAware:**
```java
@Service
public class ServiceA implements ApplicationContextAware {
    private ApplicationContext context;
    
    public ServiceB getServiceB() {
        return context.getBean(ServiceB.class);
    }
}
```

4. **Redesign Architecture:**
- Extract common functionality
- Use events/messaging
- Apply dependency inversion

### 20. @Transactional Usage and Propagation

**Transaction Propagation Types:**

- **REQUIRED** (Default): Join existing or create new
- **REQUIRES_NEW**: Always create new transaction
- **SUPPORTS**: Join if exists, non-transactional otherwise
- **NOT_SUPPORTED**: Execute non-transactionally
- **MANDATORY**: Must have existing transaction
- **NEVER**: Must not have transaction
- **NESTED**: Nested transaction (savepoints)

**Usage Examples:**
```java
@Service
@Transactional
public class UserService {
    
    @Transactional(propagation = Propagation.REQUIRED)
    public void updateUser(User user) {
        userRepository.save(user);
        auditService.logUpdate(user); // Participates in same transaction
    }
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void logCriticalEvent(String event) {
        eventRepository.save(new Event(event));
        // Always commits, even if calling transaction rolls back
    }
    
    @Transactional(readOnly = true)
    public User findUser(Long id) {
        return userRepository.findById(id);
    }
}
```

**Isolation Levels:**
- **READ_UNCOMMITTED**: Dirty reads possible
- **READ_COMMITTED**: Prevents dirty reads
- **REPEATABLE_READ**: Prevents dirty and non-repeatable reads
- **SERIALIZABLE**: Highest isolation, prevents all phenomena

**Best Practices:**
- Use at service layer, not repository layer
- Keep transactions short
- Use readOnly=true for read operations
- Handle exceptions properly
- Avoid long-running transactions

## Advanced Topics

### Exception Handling Best Practices

**Global Exception Handling:**
```java
@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(UserNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ErrorResponse handleUserNotFound(UserNotFoundException ex) {
        return new ErrorResponse("USER_NOT_FOUND", ex.getMessage());
    }
    
    @ExceptionHandler(ValidationException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ErrorResponse handleValidation(ValidationException ex) {
        return new ErrorResponse("VALIDATION_ERROR", ex.getMessage());
    }
}
```

### Performance Optimization

**JVM Tuning:**
- Heap sizing: -Xms and -Xmx
- GC selection: -XX:+UseG1GC
- GC logging: -Xlog:gc
- Profiling with tools like JProfiler, VisualVM

**Spring Boot Optimization:**
- Lazy initialization: spring.main.lazy-initialization=true
- Exclude unused auto-configurations
- Use @ConditionalOnProperty for optional features
- Connection pool tuning
- Caching with @Cacheable

### Microservices Patterns

**Common Patterns:**
- Service Discovery (Eureka, Consul)
- Circuit Breaker (Hystrix, Resilience4j)
- API Gateway (Zuul, Spring Cloud Gateway)
- Configuration Management (Spring Cloud Config)
- Distributed Tracing (Zipkin, Jaeger)

### Testing Strategies

**Unit Testing:**
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    void shouldCreateUser() {
        // Given
        User user = new User("John");
        when(userRepository.save(any(User.class))).thenReturn(user);
        
        // When
        User result = userService.createUser(user);
        
        // Then
        assertThat(result.getName()).isEqualTo("John");
    }
}
```

**Integration Testing:**
```java
@SpringBootTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
@Testcontainers
class UserControllerIntegrationTest {
    
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:13")
            .withDatabaseName("testdb")
            .withUsername("test")
            .withPassword("test");
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Test
    void shouldCreateUserEndToEnd() {
        User user = new User("John");
        ResponseEntity<User> response = restTemplate.postForEntity("/api/users", user, User.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.CREATED);
    }
}
```

## Additional Key Concepts

### Spring AOP (Aspect-Oriented Programming)

**Core Concepts:**
- **Aspect**: Cross-cutting concern implementation
- **Join Point**: Point where aspect can be applied
- **Pointcut**: Expression that matches join points
- **Advice**: Action taken at join point
- **Weaving**: Process of applying aspects

**Example:**
```java
@Aspect
@Component
public class LoggingAspect {
    
    @Around("@annotation(Loggable)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long startTime = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long endTime = System.currentTimeMillis();
        
        logger.info("Method {} executed in {} ms", 
            joinPoint.getSignature().getName(), 
            endTime - startTime);
        
        return result;
    }
}
```

### Database Integration

**JPA/Hibernate Best Practices:**
- Use appropriate fetch types (LAZY vs EAGER)
- Implement proper equals() and hashCode()
- Use @Query for complex queries
- Leverage native queries when needed
- Proper transaction boundaries

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY, cascade = CascadeType.ALL)
    private List<Order> orders = new ArrayList<>();
    
    // equals() and hashCode() based on business key
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof User)) return false;
        User user = (User) o;
        return Objects.equals(email, user.email);
    }
}
```

## Summary

This guide covers essential Java and Spring Boot concepts frequently asked in interviews. Key areas to focus on:

1. **Concurrency**: Understanding thread safety, synchronization mechanisms
2. **Memory Management**: JVM memory areas and garbage collection
3. **Spring Framework**: Bean lifecycle, dependency injection, AOP
4. **Spring Boot**: Auto-configuration, starters, profiles
5. **Spring Security**: Authentication/authorization patterns
6. **Best Practices**: Exception handling, testing, performance optimization

Regular practice with coding examples and understanding the underlying principles will help in technical interviews. Focus on explaining not just "what" but "why" and "how" these concepts work.

---

*Study Guide prepared for Java Developer interviews. Last updated: August 2025*