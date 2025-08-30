# Java Interview Experience Notes

## Overview
This document contains detailed notes based on a recent Java Developer interview experience. The questions cover a range of topics from basic concepts to advanced technical implementations, providing a comprehensive guide for preparation.

## Interview Structure
1. **Introduction** - A brief about myself and my current role.
2. **Day-to-Day Activities** - Explained my responsibilities in the current project.

## Technical Questions

### 1. What is Abstraction? How did you implement it in your project?
- **Definition**: Abstraction is a principle in object-oriented programming that hides complex implementation details and shows only the necessary features of an object.
- **Implementation**: Used abstract classes and interfaces in a project to define common methods (e.g., a `PaymentProcessor` interface) while letting concrete classes (e.g., `CreditCardProcessor`) provide specific implementations.

### 2. How do you create a REST API? (I was asked to write a sample GET and POST API)
- **Overview**: REST APIs are created using frameworks like Spring Boot. Key steps include defining endpoints, handling HTTP methods, and using proper annotations.
- **Sample Code** (Conceptual):
  - **GET**: `@GetMapping("/users/{id}") public User getUser(@PathVariable Long id) { ... }`
  - **POST**: `@PostMapping("/users") public ResponseEntity<User> createUser(@RequestBody User user) { ... }`
- **Considerations**: Use proper HTTP status codes (e.g., 200, 201, 404) and validate input data.

### 3. How can you decrease API response time?
- Techniques:
  - Caching (e.g., using Redis).
  - Optimizing database queries (e.g., indexing).
  - Load balancing and using CDNs.
  - Reducing payload size with compression.

### 4. Explain the difference between Monolithic vs Microservices architecture.
- **Monolithic**: Single, unified application where all components (UI, business logic, database) are interconnected. Easier to develop but harder to scale.
- **Microservices**: Independent services communicating via APIs. Offers scalability and flexibility but increases complexity.

### 5. What are Microservices and the types of communication between them?
- **Definition**: Small, independent services that work together to form an application.
- **Communication Types**:
  - Synchronous (e.g., HTTP/REST, gRPC).
  - Asynchronous (e.g., Message queues like RabbitMQ, Kafka).

### 6. What is Apache Kafka and where did you implement it?
- **Definition**: A distributed streaming platform for handling large-scale data feeds.
- **Implementation**: Used Kafka for real-time log processing in a project, setting up producers and consumers to handle event streams.

### 7. How do you capture and manage application logs?
- **Tools**: Use Log4j or SLF4J with Spring Boot.
- **Approach**: Configure log levels, use centralized logging (e.g., ELK stack), and rotate logs to manage space.

### 8. How did you implement Redis in your project?
- **Usage**: In-memory data store for caching frequently accessed data.
- **Implementation**: Integrated Redis with Spring Boot using `RedisTemplate` to cache user sessions, reducing database load.

### 9. Why do you use Redis and how the data is stored?
- **Why**: High performance, support for various data structures (strings, hashes, lists).
- **Storage**: Data is stored in memory as key-value pairs, with optional persistence to disk.

### 10. What is Spring Boot Actuator?
- **Definition**: Provides production-ready features like health checks, metrics, and monitoring endpoints.
- **Usage**: Enabled endpoints like `/actuator/health` and `/actuator/metrics` for monitoring.

### 11. Difference between annotations: @Component vs @Repository vs @Service vs @Controller.
- **@Component**: Generic stereotype for any Spring-managed component.
- **@Repository**: For DAO layer, adds exception translation.
- **@Service**: For business logic layer.
- **@Controller**: For web layer, handles HTTP requests.

### 12. How do you implement Global Exception Handling in Spring Boot?
- **Approach**: Use `@ControllerAdvice` and `@ExceptionHandler` to catch and handle exceptions globally.
- **Example**: `@ExceptionHandler(value = {CustomException.class}) public ResponseEntity handleException(CustomException ex) { ... }`

### 13. What is the use of @Valid vs @Validated annotations?
- **@Valid**: JSR-303 for bean validation.
- **@Validated**: Spring’s variant, supports group validation.

### 14. How do you secure REST APIs using JWT / OAuth2?
- **JWT**: Use Spring Security with JWT for token-based authentication.
- **OAuth2**: Implement authorization server and resource server using Spring OAuth2.

### 15. Write a program to find the first non-repeating character in a string.
- **Solution**: Use a HashMap to track character counts, then iterate to find the first with count 1.
- **Code Example** (Conceptual):
  ```java
  public char findFirstNonRepeating(String str) {
      Map<Character, Integer> map = new HashMap<>();
      for (char c : str.toCharArray()) map.put(c, map.getOrDefault(c, 0) + 1);
      for (char c : str.toCharArray()) if (map.get(c) == 1) return c;
      return '\0';
  }
  ```

### 16. Write a program to find the top 5 longest strings in a list which contain 'a' or 'A'.
- **Solution**: Filter strings containing 'a' or 'A', sort by length, and take top 5.
- **Code Example** (Conceptual):
  ```java
  public List<String> findTopFiveWithA(List<String> list) {
      return list.stream()
          .filter(s -> s.toLowerCase().contains("a"))
          .sorted((s1, s2) -> s2.length() - s1.length())
          .limit(5)
          .collect(Collectors.toList());
  }
  ```

## Reflection
This interview was a great learning experience, helping me identify areas to strengthen further, such as deeper Kafka integration and advanced security implementations.