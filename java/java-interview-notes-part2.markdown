# Java Interview Experience Notes - Part 2

## Overview
This document contains detailed notes based on additional questions from a Java Developer interview, focusing on JPA/Hibernate, Microservices & Architecture, and System Design. These notes complement the previous set and provide a comprehensive guide for preparation.

## Interview Structure
- **JPA / Hibernate**
- **Microservices & Architecture**
- **System Design**

## Technical Questions

### JPA / Hibernate

#### 1. Difference between Lazy vs Eager fetching.
- **Lazy Fetching**: 
  - Loads related entities only when they are explicitly accessed, improving performance by reducing initial query load.
  - Example: `@OneToMany(fetch = FetchType.LAZY)` on a collection. The associated data is fetched only when the collection is accessed.
  - Use Case: When related data is not always needed, reducing memory usage.
- **Eager Fetching**: 
  - Loads all related entities immediately along with the parent entity in a single query.
  - Example: `@OneToMany(fetch = FetchType.EAGER)` ensures all related data is available upfront.
  - Use Case: When related data is small and always required (e.g., a user’s basic profile details).
- **Trade-off**: Lazy fetching can lead to the N+1 problem if not managed, while Eager fetching may load unnecessary data.

#### 2. What is the N+1 select problem and how to fix it?
- **Definition**: Occurs when an initial query (1) fetches a set of entities, and then a separate query (N) is executed for each entity to fetch its related data, leading to performance degradation.
- **Example**: Fetching 10 users with lazy-loaded orders triggers 11 queries (1 for users, 10 for orders).
- **Fixes**:
  - Use `JOIN FETCH` in JPQL/HQL to load related data in one query (e.g., `SELECT u FROM User u JOIN FETCH u.orders`).
  - Enable Eager fetching for small, always-needed relationships.
  - Use DTO projections to fetch only required fields, avoiding lazy loading overhead.

#### 3. Difference between merge(), persist(), saveOrUpdate().
- **persist()**:
  - Inserts a new entity into the database.
  - Throws an exception (e.g., `EntityExistsException`) if the entity already has an ID or exists.
  - Returns `void`, does not update existing entities.
  - Example: `entityManager.persist(new User());`
- **merge()**:
  - Merges a detached entity state into the persistence context, updating or inserting as needed.
  - Returns a managed entity instance, even if a new one is created.
  - Useful for reattaching detached entities.
  - Example: `entityManager.merge(detachedUser);`
- **saveOrUpdate()** (Legacy Hibernate):
  - Updates if the entity has an ID, inserts if it doesn’t.
  - Similar to merge but less commonly used in modern JPA.
  - Example: `session.saveOrUpdate(user);`

#### 4. Explain 1st level vs 2nd level cache.
- **1st Level Cache**:
  - Session-specific, enabled by default in Hibernate.
  - Stores entities within the scope of a single `EntityManager` or `Session`.
  - Automatically cleared when the session closes.
  - Example: Repeated calls to `find()` within one session hit the cache.
- **2nd Level Cache**:
  - Application-wide, optional, shared across sessions.
  - Configured with providers like Ehcache or Redis.
  - Stores data across all sessions, reducing database hits.
  - Example: Use `@Cacheable` with a cache provider to cache query results.

#### 5. How do you implement optimistic vs pessimistic locking?
- **Optimistic Locking**:
  - Assumes conflicts are rare, uses a version column (e.g., `@Version` with a numeric field).
  - Checks the version during updates; throws `OptimisticLockException` if it mismatches.
  - Example: `@Version @Column(name = "version") private int version;`
  - Use Case: Low contention scenarios like online forms.
- **Pessimistic Locking**:
  - Locks rows during transactions to prevent concurrent modifications.
  - Example: `entityManager.find(User.class, id, LockModeType.PESSIMISTIC_WRITE);`
  - Use Case: High contention scenarios like inventory management.

#### 6. How to handle batch inserts/updates?
- **Approach**:
  - Set `hibernate.jdbc.batch_size` (e.g., 50) to group operations.
  - Use `session.flush()` and `session.clear()` periodically to manage memory.
  - Example: Loop through entities, persist, and flush every 50 records.
  - Benefits: Reduces the number of database round-trips, improving performance.

#### 7. How to write custom queries in Spring Data JPA?
- **Methods**:
  - **@Query Annotation**: Write JPQL or native SQL.
    - Example: `@Query("SELECT u FROM User u WHERE u.age > ?1") List<User> findByAge(int age);`
  - **Method Name Derivation**: Follow naming conventions (e.g., `findByLastName`).
  - **Native Queries**: Use `@Query(value = "SQL_QUERY", nativeQuery = true)`.
  - **Parameters**: Support positional (`?1`) or named (`:name`) parameters.

### Microservices & Architecture

#### 8. What are 12-factor app principles?
- **Overview**: A set of 12 best practices for building scalable, maintainable cloud-native applications.
- **Principles**:
  1. Codebase: One codebase tracked in version control, multiple deploys.
  2. Dependencies: Explicitly declare and isolate dependencies.
  3. Config: Store config in environment variables.
  4. Backing Services: Treat services (e.g., databases) as attached resources.
  5. Build, Release, Run: Separate build and run stages.
  6. Processes: Execute the app as stateless processes.
  7. Port Binding: Export services via port binding.
  8. Concurrency: Scale out via process model.
  9. Disposability: Maximize robustness with fast startup and graceful shutdown.
  10. Dev/Prod Parity: Keep development, staging, and production as similar as possible.
  11. Logs: Treat logs as event streams.
  12. Admin Processes: Run admin/management tasks as one-off processes.

#### 9. How do microservices communicate (REST, gRPC, Messaging)?
- **REST**:
  - HTTP-based, uses JSON/XML, simple and widely supported.
  - Example: `GET /users/{id}`.
- **gRPC**:
  - High-performance RPC framework using Protocol Buffers.
  - Example: Defines services and messages in `.proto` files, supports streaming.
- **Messaging**:
  - Asynchronous using queues (e.g., Kafka, RabbitMQ).
  - Example: Publish events to a topic, consumed by subscribers.

#### 10. How to implement service discovery (Eureka, Consul)?
- **Eureka**:
  - Netflix’s service registry, part of Spring Cloud.
  - Servers register and discover services dynamically.
  - Example: Use `@EnableEurekaServer` and `@EnableDiscoveryClient`.
- **Consul**:
  - HashiCorp’s tool for service discovery and health checks.
  - Supports KV store and DNS-based discovery.
  - Example: Register services with Consul agent.

#### 11. Explain API Gateway pattern.
- **Definition**: A single entry point that routes requests to appropriate microservices, handling authentication, load balancing, and logging.
- **Benefits**: Centralizes cross-cutting concerns, improves security.
- **Example**: Use Spring Cloud Gateway or Kong to route `/users` to a user service.

#### 12. How to handle distributed transactions?
- **Approaches**:
  - **Saga Pattern**: Coordinates transactions with compensating actions.
  - **Two-Phase Commit (2PC)**: Uses a coordinator to manage commit/abort phases.
  - **Compensating Transactions**: Undo changes if a step fails (e.g., cancel an order if payment fails).

#### 13. Difference between Monolithic vs Microservices.
- **Monolithic**:
  - Single deployable unit with all components.
  - Pros: Simpler development, single database.
  - Cons: Hard to scale, tight coupling.
- **Microservices**:
  - Independent services with separate databases.
  - Pros: Scalable, independent deployment.
  - Cons: Complex management, distributed data.

#### 14. How to manage configurations (Spring Cloud Config, Vault)?
- **Spring Cloud Config**:
  - Centralized config server, supports Git backend.
  - Example: `@EnableConfigServer` to serve configs.
- **Vault**:
  - Secure storage for secrets, integrates with Spring.
  - Example: Use Vault’s API to fetch credentials dynamically.

#### 15. How to implement circuit breaker & rate limiting (Resilience4j, Hystrix)?
- **Circuit Breaker**:
  - Prevents cascading failures by opening the circuit on repeated failures.
  - Example: Resilience4j `@CircuitBreaker(name = "backendService")`.
- **Rate Limiting**:
  - Controls request rates to prevent overload.
  - Example: Resilience4j `RateLimiter(name = "default", limitForPeriod = 10, limitRefreshPeriod = 1)`.

#### 16. Difference between synchronous vs asynchronous communication.
- **Synchronous**:
  - Blocking, client waits for response (e.g., REST).
  - Pros: Simple, direct.
  - Cons: Tight coupling, latency issues.
- **Asynchronous**:
  - Non-blocking, uses queues/events (e.g., Kafka).
  - Pros: Decoupled, scalable.
  - Cons: Complex error handling.

#### 17. How do you ensure idempotency in REST APIs?
- **Approach**:
  - Use unique request IDs (e.g., `Idempotency-Key` header).
  - Implement logic to detect and skip duplicate requests.
  - Use atomic operations or database constraints.
  - Example: Check if a payment with the same ID exists before processing.

### System Design

#### 18. Design a high-availability payment system.
- **Components**:
  - Load balancers for traffic distribution.
  - Redundant databases (master-slave replication).
  - Microservices for payment processing, fraud detection.
  - Failover mechanisms (e.g., active-passive clusters).
- **Considerations**: Use circuit breakers, retry logic, and monitoring (e.g., Prometheus).

#### 19. Design a low-latency stock trading API.
- **Considerations**:
  - In-memory data store (e.g., Redis) for real-time data.
  - Event-driven architecture with WebSockets.
  - Optimized network (e.g., colocated servers).
  - Example: Use Kafka for order streams, minimize round-trips.

#### 20. How to isolate a frequently failing microservice?
- **Solution**:
  - Deploy in a separate instance or container.
  - Use circuit breakers to fail fast.
  - Monitor with tools like Prometheus and alert on failures.
  - Example: Route traffic away using an API Gateway.

#### 21. How to ensure backward compatibility in APIs?
- **Approach**:
  - Version APIs (e.g., `/v1/resource`, `/v2/resource`).
  - Deprecate old endpoints with warnings.
  - Maintain documentation (e.g., OpenAPI specs).
  - Example: Support old fields while adding new ones.

#### 22. How to scale a Spring Boot service for 1M+ users?
- **Strategies**:
  - Horizontal scaling with load balancers.
  - Caching (e.g., Redis) for frequent reads.
  - Database sharding and replication.
  - Asynchronous processing with message queues.
  - Example: Use Kubernetes for auto-scaling.

## Reflection
These questions highlighted the importance of understanding JPA/Hibernate intricacies, microservices patterns, and system design principles. Further practice in distributed systems and optimization techniques is recommended.