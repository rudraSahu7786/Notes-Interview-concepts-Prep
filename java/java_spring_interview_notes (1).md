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

## Advanced JPA/Hibernate & Database Topics

### 12. N+1 Query Problem in JPA/Hibernate

**What is N+1 Problem?**
The N+1 query problem occurs when an application executes one query to fetch N records, then executes N additional queries to fetch related data for each record.

**Example Scenario:**
```java
@Entity
public class Department {
    @Id
    private Long id;
    private String name;
    
    @OneToMany(mappedBy = "department", fetch = FetchType.LAZY)
    private List<Employee> employees;
}

// This causes N+1 problem
List<Department> departments = departmentRepository.findAll(); // 1 query
for (Department dept : departments) {
    System.out.println(dept.getEmployees().size()); // N queries (one per department)
}
```

**Solutions:**

1. **JOIN FETCH:**
```java
@Query("SELECT d FROM Department d JOIN FETCH d.employees")
List<Department> findAllWithEmployees();
```

2. **@EntityGraph:**
```java
@EntityGraph(attributePaths = {"employees"})
List<Department> findAll();
```

3. **Batch Fetching:**
```java
@BatchSize(size = 10)
@OneToMany(mappedBy = "department")
private List<Employee> employees;
```

4. **Hibernate.initialize():**
```java
departments.forEach(dept -> Hibernate.initialize(dept.getEmployees()));
```

**Project Experience Example:**
"In our e-commerce project, we had product listings with categories causing N+1 issues. We resolved it by using @EntityGraph annotations and JOIN FETCH queries, reducing database calls from 1000+ to just 2-3 queries, improving page load time by 80%."

### 13. Spring Dependency Injection (BeanFactory vs ApplicationContext)

**BeanFactory:**
- Basic IoC container
- Lazy initialization by default
- Limited functionality
- Suitable for memory-constrained environments
- Manual bean lifecycle management

**ApplicationContext:**
- Advanced IoC container
- Extends BeanFactory
- Eager initialization by default
- Rich feature set (events, AOP, i18n)
- Automatic bean lifecycle management

**Key Differences:**

| Feature | BeanFactory | ApplicationContext |
|---------|-------------|-------------------|
| Bean Creation | Lazy | Eager |
| Event Publishing | No | Yes |
| AOP Support | Manual | Automatic |
| Annotation Processing | No | Yes |
| Environment Abstraction | No | Yes |

**Internal DI Process:**
1. **Bean Definition Registration** - Scan and register bean metadata
2. **Dependency Resolution** - Build dependency graph
3. **Circular Dependency Detection** - Check for cycles
4. **Bean Instantiation** - Create bean instances
5. **Dependency Injection** - Inject dependencies
6. **Post-Processing** - Apply BeanPostProcessors

```java
// BeanFactory usage
BeanFactory factory = new XmlBeanFactory(new FileSystemResource("beans.xml"));
MyBean bean = (MyBean) factory.getBean("myBean"); // Lazy creation

// ApplicationContext usage
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
MyBean bean = context.getBean(MyBean.class); // Eager creation
```

### 14. Lazy Loading vs Eager Loading in Microservices

**Lazy Loading:**
- Load data only when needed
- Better initial performance
- Risk of N+1 queries
- Session management required

**Eager Loading:**
- Load all related data upfront
- Fewer database roundtrips
- Higher memory consumption
- Potential for unnecessary data loading

**Microservices Considerations:**

**Prefer Lazy Loading when:**
- High network latency between services
- Large datasets with selective access
- Memory constraints
- Unpredictable access patterns

**Prefer Eager Loading when:**
- Small, frequently accessed datasets
- Predictable access patterns
- Low latency requirements
- Batch processing scenarios

**Implementation Strategies:**
```java
// API design for lazy loading
@RestController
public class UserController {
    
    @GetMapping("/users/{id}")
    public UserDto getUser(@PathVariable Long id) {
        return userService.getBasicUserInfo(id); // No related data
    }
    
    @GetMapping("/users/{id}/orders")
    public List<OrderDto> getUserOrders(@PathVariable Long id) {
        return orderService.getOrdersByUserId(id); // Load on demand
    }
}
```

### 15. Caching for Frequently Accessed APIs in Spring Boot

**Caching Strategies:**

1. **Application-Level Caching:**
```java
@Configuration
@EnableCaching
public class CacheConfig {
    
    @Bean
    public CacheManager cacheManager() {
        CaffeineCacheManager cacheManager = new CaffeineCacheManager();
        cacheManager.setCaffeine(Caffeine.newBuilder()
            .maximumSize(1000)
            .expireAfterWrite(10, TimeUnit.MINUTES)
            .recordStats());
        return cacheManager;
    }
}

@Service
public class ProductService {
    
    @Cacheable(value = "products", key = "#id")
    public Product getProduct(Long id) {
        return productRepository.findById(id);
    }
    
    @CacheEvict(value = "products", key = "#product.id")
    public Product updateProduct(Product product) {
        return productRepository.save(product);
    }
    
    @Caching(evict = {
        @CacheEvict(value = "products", key = "#product.id"),
        @CacheEvict(value = "productsByCategory", key = "#product.categoryId")
    })
    public void deleteProduct(Product product) {
        productRepository.delete(product);
    }
}
```

2. **Redis Distributed Caching:**
```java
@Configuration
public class RedisConfig {
    
    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(jedisConnectionFactory());
        template.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());
        return template;
    }
}

@Service
public class CacheService {
    
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;
    
    public void cacheApiResponse(String key, Object data, Duration ttl) {
        redisTemplate.opsForValue().set(key, data, ttl);
    }
    
    public <T> T getCachedData(String key, Class<T> type) {
        Object cached = redisTemplate.opsForValue().get(key);
        return type.cast(cached);
    }
}
```

3. **HTTP Response Caching:**
```java
@RestController
public class ApiController {
    
    @GetMapping("/api/products/{id}")
    @Cacheable(value = "products")
    public ResponseEntity<Product> getProduct(@PathVariable Long id) {
        Product product = productService.getProduct(id);
        return ResponseEntity.ok()
            .cacheControl(CacheControl.maxAge(300, TimeUnit.SECONDS))
            .body(product);
    }
}
```

## Microservices & System Design

### 16. Microservices Communication Patterns

**Communication Methods:**

**1. Synchronous Communication:**

**REST APIs:**
- Simple and widely adopted
- HTTP-based, stateless
- Easy debugging and testing
- Higher latency and coupling

```java
@FeignClient(name = "user-service", url = "${user.service.url}")
public interface UserServiceClient {
    
    @GetMapping("/users/{id}")
    UserDto getUser(@PathVariable Long id);
    
    @PostMapping("/users")
    UserDto createUser(@RequestBody CreateUserRequest request);
}
```

**gRPC:**
- High performance, binary protocol
- Strong typing with Protocol Buffers
- Streaming support
- Better for internal service communication
- Steeper learning curve

```protobuf
service UserService {
    rpc GetUser(GetUserRequest) returns (UserResponse);
    rpc ListUsers(ListUsersRequest) returns (stream UserResponse);
}
```

**2. Asynchronous Communication:**

**Message Queues (RabbitMQ, Apache Kafka):**
- Decoupled communication
- Better fault tolerance
- Event-driven architecture
- Eventual consistency

```java
@Component
public class OrderEventPublisher {
    
    @Autowired
    private RabbitTemplate rabbitTemplate;
    
    public void publishOrderCreated(OrderCreatedEvent event) {
        rabbitTemplate.convertAndSend("order.exchange", "order.created", event);
    }
}

@RabbitListener(queues = "inventory.queue")
public void handleOrderCreated(OrderCreatedEvent event) {
    inventoryService.reserveItems(event.getOrderId());
}
```

**Recommendation:**
- **REST**: For simple CRUD operations, external APIs
- **gRPC**: For high-performance internal communication
- **Messaging**: For event-driven workflows, long-running processes

### 17. API Gateway Role in Microservices

**Primary Functions:**
- **Request Routing** - Route requests to appropriate services
- **Load Balancing** - Distribute traffic across service instances
- **Authentication/Authorization** - Centralized security
- **Rate Limiting** - Protect services from overload
- **Request/Response Transformation** - Protocol translation
- **Monitoring & Logging** - Centralized observability

**Spring Cloud Gateway Example:**
```java
@Configuration
public class GatewayConfig {
    
    @Bean
    public RouteLocator customRoutes(RouteLocatorBuilder builder) {
        return builder.routes()
            .route("user-service", r -> r.path("/api/users/**")
                .filters(f -> f
                    .stripPrefix(1)
                    .addRequestHeader("X-Gateway", "Spring-Cloud-Gateway")
                    .circuitBreaker(c -> c.setName("user-service-cb")))
                .uri("lb://user-service"))
            .route("order-service", r -> r.path("/api/orders/**")
                .filters(f -> f.stripPrefix(1))
                .uri("lb://order-service"))
            .build();
    }
}
```

**Zuul vs Spring Cloud Gateway:**

| Feature | Zuul | Spring Cloud Gateway |
|---------|------|---------------------|
| Technology | Servlet-based | WebFlux (Reactive) |
| Performance | Blocking I/O | Non-blocking I/O |
| Filters | Groovy/Java | Java Predicates |
| Memory Usage | Higher | Lower |
| Throughput | Lower | Higher |
| Maintenance | Netflix (limited) | Spring Team |

### 18. Fault Tolerance and Resilience Patterns

**1. Circuit Breaker Pattern:**
```java
@Component
public class ExternalServiceClient {
    
    @CircuitBreaker(name = "payment-service", fallbackMethod = "fallbackPayment")
    @TimeLimiter(name = "payment-service")
    public CompletableFuture<PaymentResponse> processPayment(PaymentRequest request) {
        return CompletableFuture.supplyAsync(() -> paymentServiceClient.process(request));
    }
    
    public CompletableFuture<PaymentResponse> fallbackPayment(PaymentRequest request, Exception ex) {
        return CompletableFuture.completedFuture(
            PaymentResponse.builder()
                .status("PENDING")
                .message("Payment will be processed later")
                .build()
        );
    }
}
```

**2. Retry Pattern:**
```java
@Retryable(
    value = {TransientException.class},
    maxAttempts = 3,
    backoff = @Backoff(delay = 1000, multiplier = 2)
)
public String callExternalService() {
    return externalServiceClient.getData();
}

@Recover
public String recover(TransientException ex) {
    return "Default response due to: " + ex.getMessage();
}
```

**3. Bulkhead Pattern:**
```java
@Configuration
public class ThreadPoolConfig {
    
    @Bean("orderProcessingExecutor")
    public Executor orderProcessingExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("order-");
        return executor;
    }
    
    @Bean("inventoryExecutor")
    public Executor inventoryExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(3);
        executor.setMaxPoolSize(6);
        executor.setQueueCapacity(50);
        executor.setThreadNamePrefix("inventory-");
        return executor;
    }
}
```

**Resilience4j Configuration:**
```yaml
resilience4j:
  circuitbreaker:
    instances:
      payment-service:
        sliding-window-size: 10
        failure-rate-threshold: 50
        wait-duration-in-open-state: 30s
        permitted-number-of-calls-in-half-open-state: 5
  retry:
    instances:
      payment-service:
        max-attempts: 3
        wait-duration: 1s
        exponential-backoff-multiplier: 2
```

### 19. Scalable Order Management System Design

**System Requirements:**
- Handle 10K+ orders per second
- High availability and consistency
- Real-time inventory management
- Payment processing
- Order tracking and notifications

**Architecture Design:**

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   API Gateway   │────│  Load Balancer   │────│   CDN/Cache     │
└─────────────────┘    └──────────────────┘    └─────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Microservices Layer                          │
├─────────────┬─────────────┬─────────────┬─────────────────────┤
│Order Service│User Service │Inventory    │Payment Service      │
│             │             │Service      │                     │
└─────────────┴─────────────┴─────────────┴─────────────────────┘
         │              │              │              │
         ▼              ▼              ▼              ▼
┌─────────────┬─────────────┬─────────────┬─────────────────────┐
│  Order DB   │   User DB   │Inventory DB │   Payment DB        │
│(PostgreSQL) │(PostgreSQL) │(Redis +     │(PostgreSQL)         │
│             │             │PostgreSQL)  │                     │
└─────────────┴─────────────┴─────────────┴─────────────────────┘
```

**Key Components:**

1. **Order Service:**
```java
@Service
@Transactional
public class OrderService {
    
    @Autowired
    private OrderRepository orderRepository;
    
    @Autowired
    private InventoryServiceClient inventoryClient;
    
    @Autowired
    private PaymentServiceClient paymentClient;
    
    @Autowired
    private ApplicationEventPublisher eventPublisher;
    
    public OrderResponse createOrder(CreateOrderRequest request) {
        // 1. Validate order
        validateOrder(request);
        
        // 2. Reserve inventory
        ReservationResponse reservation = inventoryClient.reserveItems(request.getItems());
        
        // 3. Create order with PENDING status
        Order order = Order.builder()
            .customerId(request.getCustomerId())
            .items(request.getItems())
            .status(OrderStatus.PENDING)
            .reservationId(reservation.getId())
            .build();
        
        order = orderRepository.save(order);
        
        // 4. Publish order created event for async processing
        eventPublisher.publishEvent(new OrderCreatedEvent(order.getId()));
        
        return OrderResponse.from(order);
    }
}
```

2. **Event-Driven Processing:**
```java
@EventListener
@Async
public void handleOrderCreated(OrderCreatedEvent event) {
    try {
        // Process payment asynchronously
        PaymentResponse payment = paymentClient.processPayment(
            PaymentRequest.from(event.getOrderId())
        );
        
        if (payment.isSuccessful()) {
            orderService.updateOrderStatus(event.getOrderId(), OrderStatus.CONFIRMED);
            inventoryService.confirmReservation(event.getReservationId());
        } else {
            orderService.updateOrderStatus(event.getOrderId(), OrderStatus.FAILED);
            inventoryService.releaseReservation(event.getReservationId());
        }
    } catch (Exception e) {
        // Handle failures, implement retry logic
        orderService.updateOrderStatus(event.getOrderId(), OrderStatus.FAILED);
    }
}
```

**Scalability Strategies:**
- **Database Sharding** - Partition orders by customer ID or date
- **Read Replicas** - Separate read/write databases
- **Caching** - Redis for frequently accessed data
- **Event Sourcing** - Store order state changes as events
- **CQRS** - Separate command and query models

### 20. Securing REST APIs with OAuth2/JWT in Spring Boot

**OAuth2 + JWT Implementation:**

1. **Authorization Server Configuration:**
```java
@Configuration
@EnableAuthorizationServer
public class OAuth2AuthorizationConfig extends AuthorizationServerConfigurerAdapter {
    
    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        clients.inMemory()
            .withClient("order-service")
            .secret(passwordEncoder.encode("secret"))
            .authorizedGrantTypes("client_credentials", "authorization_code")
            .scopes("read", "write")
            .accessTokenValiditySeconds(3600);
    }
    
    @Bean
    public JwtAccessTokenConverter jwtAccessTokenConverter() {
        JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
        converter.setSigningKey("signing-key");
        return converter;
    }
}
```

2. **Resource Server Configuration:**
```java
@Configuration
@EnableResourceServer
public class OAuth2ResourceConfig extends ResourceServerConfigurerAdapter {
    
    @Override
    public void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .antMatchers("/api/public/**").permitAll()
            .antMatchers(HttpMethod.GET, "/api/orders/**").hasScope("read")
            .antMatchers(HttpMethod.POST, "/api/orders/**").hasScope("write")
            .anyRequest().authenticated();
    }
}
```

3. **JWT Token Validation:**
```java
@Component
public class JwtTokenProvider {
    
    private String secretKey = "mySecretKey";
    private long validityInMilliseconds = 3600000; // 1h
    
    public String createToken(String username, List<String> roles) {
        Claims claims = Jwts.claims().setSubject(username);
        claims.put("roles", roles);
        
        Date now = new Date();
        Date validity = new Date(now.getTime() + validityInMilliseconds);
        
        return Jwts.builder()
            .setClaims(claims)
            .setIssuedAt(now)
            .setExpiration(validity)
            .signWith(SignatureAlgorithm.HS256, secretKey)
            .compact();
    }
    
    public Authentication getAuthentication(String token) {
        UserDetails userDetails = userDetailsService.loadUserByUsername(getUsername(token));
        return new UsernamePasswordAuthenticationToken(userDetails, "", userDetails.getAuthorities());
    }
}
```

## SQL & Database

### 21. JOIN Types with Examples

**INNER JOIN:**
Returns only matching records from both tables.
```sql
SELECT u.name, o.order_date, o.total_amount
FROM users u
INNER JOIN orders o ON u.id = o.user_id;
```

**LEFT JOIN (LEFT OUTER JOIN):**
Returns all records from left table and matching records from right table.
```sql
SELECT u.name, o.order_date, o.total_amount
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
-- Users without orders will show NULL for order fields
```

**RIGHT JOIN (RIGHT OUTER JOIN):**
Returns all records from right table and matching records from left table.
```sql
SELECT u.name, o.order_date, o.total_amount
FROM users u
RIGHT JOIN orders o ON u.id = o.user_id;
-- Orders without users will show NULL for user fields
```

**FULL OUTER JOIN:**
Returns all records when there's a match in either table.
```sql
SELECT u.name, o.order_date, o.total_amount
FROM users u
FULL OUTER JOIN orders o ON u.id = o.user_id;
-- Shows all users and all orders, with NULLs where no match
```

**Use Cases:**
- **INNER JOIN**: Order reports with customer details
- **LEFT JOIN**: Customer list with their order counts (including customers with no orders)
- **RIGHT JOIN**: Rarely used, usually replaced with LEFT JOIN
- **FULL OUTER JOIN**: Data reconciliation, finding mismatches

### 22. Query Performance Troubleshooting (30-second query)

**Troubleshooting Steps:**

1. **Analyze Execution Plan:**
```sql
EXPLAIN ANALYZE SELECT * FROM orders o
JOIN order_items oi ON o.id = oi.order_id
WHERE o.created_date BETWEEN '2024-01-01' AND '2024-12-31';
```

2. **Common Performance Issues:**
- Missing indexes on WHERE, JOIN, ORDER BY columns
- Full table scans
- Inefficient JOIN orders
- Large result sets without LIMIT
- Functions in WHERE clauses preventing index usage

3. **Optimization Strategies:**

**Add Appropriate Indexes:**
```sql
-- Composite index for date range queries
CREATE INDEX idx_orders_created_date ON orders(created_date);

-- Covering index to avoid table lookup
CREATE INDEX idx_orders_covering ON orders(id, customer_id, created_date, status);
```

**Query Rewriting:**
```sql
-- Before (inefficient)
SELECT * FROM orders WHERE YEAR(created_date) = 2024;

-- After (can use index)
SELECT * FROM orders WHERE created_date >= '2024-01-01' AND created_date < '2025-01-01';
```

**Pagination for Large Results:**
```sql
-- Offset pagination (can be slow for large offsets)
SELECT * FROM orders ORDER BY id LIMIT 20 OFFSET 1000;

-- Cursor-based pagination (better performance)
SELECT * FROM orders WHERE id > 1000 ORDER BY id LIMIT 20;
```

4. **Database-Specific Optimizations:**
- Update table statistics: `ANALYZE TABLE orders;`
- Check for table locks: `SHOW PROCESSLIST;`
- Monitor buffer pool hit ratio
- Consider partitioning for very large tables

### 23. When to Avoid Using Indexes

**Scenarios to Avoid Indexes:**

1. **Small Tables (< 1000 rows):**
- Index overhead > benefit
- Full table scan is faster

2. **High INSERT/UPDATE/DELETE Frequency:**
- Index maintenance overhead
- Slower write operations
- Consider batch operations instead

3. **Low Selectivity Columns:**
```sql
-- Poor index candidate (low selectivity)
CREATE INDEX idx_gender ON users(gender); -- Only M/F values

-- Better approach: composite index
CREATE INDEX idx_user_lookup ON users(status, gender, created_date);
```

4. **Volatile Data:**
- Frequently changing columns
- Temporary tables
- ETL staging tables

5. **Memory Constraints:**
- Limited RAM for index caching
- Prioritize most critical indexes

**Index Monitoring:**
```sql
-- PostgreSQL: Check index usage
SELECT schemaname, tablename, indexname, idx_scan, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes
WHERE idx_scan = 0; -- Unused indexes

-- MySQL: Check index cardinality
SHOW INDEX FROM table_name;
```

### 24. Distributed Transactions Across Microservices

**Challenges:**
- ACID properties difficult to maintain
- Network failures and latency
- Service independence compromised
- Complexity increases exponentially

**Solutions:**

**1. Saga Pattern:**
Choreography-based saga:
```java
@Service
public class OrderSagaOrchestrator {
    
    public void processOrder(OrderRequest request) {
        try {
            // Step 1: Create order
            Order order = orderService.createOrder(request);
            
            // Step 2: Reserve inventory
            inventoryService.reserveItems(order.getItems());
            
            // Step 3: Process payment
            paymentService.processPayment(order.getPaymentInfo());
            
            // Step 4: Confirm order
            orderService.confirmOrder(order.getId());
            
        } catch (Exception e) {
            // Compensating transactions
            executeCompensation(request);
        }
    }
    
    private void executeCompensation(OrderRequest request) {
        try {
            paymentService.refundPayment(request.getPaymentId());
            inventoryService.releaseReservation(request.getReservationId());
            orderService.cancelOrder(request.getOrderId());
        } catch (Exception e) {
            // Log and handle compensation failures
        }
    }
}
```

**2. Event Sourcing with Compensation:**
```java
@EventSourcingHandler
public void on(OrderCreatedEvent event) {
    // Publish inventory reservation event
    eventBus.publish(new ReserveInventoryCommand(event.getOrderId(), event.getItems()));
}

@EventSourcingHandler
public void on(InventoryReservationFailedEvent event) {
    // Compensate by canceling order
    eventBus.publish(new CancelOrderCommand(event.getOrderId()));
}
```

**3. Two-Phase Commit (2PC) - Use Sparingly:**
```java
// Only for critical business operations
@Transactional
@TwoPhaseCommit
public void transferMoney(TransferRequest request) {
    // Phase 1: Prepare
    accountService.prepareDebit(request.getFromAccount(), request.getAmount());
    accountService.prepareCredit(request.getToAccount(), request.getAmount());
    
    // Phase 2: Commit
    accountService.commitDebit(request.getFromAccount());
    accountService.commitCredit(request.getToAccount());
}
```

### 25. ACID vs BASE Properties

**ACID Properties (Traditional RDBMS):**

- **Atomicity**: All or nothing transactions
- **Consistency**: Data remains in valid state
- **Isolation**: Concurrent transactions don't interfere
- **Durability**: Committed data survives system failures

**BASE Properties (NoSQL/Distributed Systems):**

- **Basically Available**: System remains operational
- **Soft State**: Data may change over time without input
- **Eventual Consistency**: System becomes consistent over time

**Microservices Alignment:**

**ACID Approach:**
- Single database per service
- Local ACID transactions
- Distributed transactions only when absolutely necessary
- Strong consistency within service boundaries

**BASE Approach (Preferred for Microservices):**
- Embrace eventual consistency
- Use event-driven architecture
- Implement compensation patterns
- Design for failure scenarios

**Implementation Strategy:**
```java
// BASE approach with eventual consistency
@Service
public class OrderProcessingService {
    
    @EventListener
    @Async
    public void handlePaymentConfirmed(PaymentConfirmedEvent event) {
        orderService.updateOrderStatus(event.getOrderId(), OrderStatus.PAID);
        inventoryService.confirmReservation(event.getReservationId());
        emailService.sendOrderConfirmation(event.getOrderId());
    }
    
    @EventListener
    @Async
    public void handlePaymentFailed(PaymentFailedEvent event) {
        orderService.updateOrderStatus(event.getOrderId(), OrderStatus.CANCELLED);
        inventoryService.releaseReservation(event.getReservationId());
    }
}
```

**When to Choose Each:**

**Use ACID when:**
- Financial transactions
- Critical business data
- Strong consistency requirements
- Single service operations

**Use BASE when:**
- Social media feeds
- Product catalogs
- User preferences
- Analytics data
- Cross-service operations

## Performance & Monitoring

### Database Connection Pooling

**HikariCP Configuration (Spring Boot Default):**
```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      idle-timeout: 300000
      max-lifetime: 1200000
      connection-timeout: 20000
      leak-detection-threshold: 60000
```

### Monitoring and Observability

**Application Metrics:**
```java
@RestController
public class MetricsController {
    
    private final MeterRegistry meterRegistry;
    
    @GetMapping("/api/orders")
    @Timed(name = "orders.get", description = "Time taken to fetch orders")
    public List<Order> getOrders() {
        Counter.Sample sample = Timer.start(meterRegistry);
        try {
            return orderService.getAllOrders();
        } finally {
            sample.stop(Timer.builder("orders.fetch.time").register(meterRegistry));
        }
    }
}
```

**Health Checks:**
```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    
    @Override
    public Health health() {
        try {
            // Check database connectivity
            jdbcTemplate.queryForObject("SELECT 1", Integer.class);
            return Health.up().withDetail("database", "Available").build();
        } catch (Exception e) {
            return Health.down(e).withDetail("database", "Unavailable").build();
        }
    }
}
```

## Summary

This comprehensive guide covers essential Java and Spring Boot concepts for senior developer interviews:

### Core Areas:
1. **Concurrency & Performance**: Thread safety, GC tuning, memory management
2. **Spring Framework**: Bean lifecycle, dependency injection, AOP, security
3. **Microservices Architecture**: Communication patterns, fault tolerance, scalability
4. **Database Design**: Query optimization, transaction management, distributed data
5. **System Design**: High-scale architectures, monitoring, observability

### Key Takeaways:
- **Understand trade-offs**: Every technology choice has pros/cons
- **Real-world experience**: Be ready to discuss project implementations
- **Performance focus**: Always consider scalability and optimization
- **Modern patterns**: Event-driven, reactive programming, cloud-native approaches
- **Best practices**: Code quality, testing, monitoring, security

### Interview Preparation Tips:
- Practice coding examples live
- Explain architectural decisions and trade-offs
- Discuss real project challenges and solutions
- Stay updated with latest Spring Boot features
- Understand distributed systems principles

---

*Comprehensive Study Guide for Senior Java Developer Interviews. Last updated: August 2025*