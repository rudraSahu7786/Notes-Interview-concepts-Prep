SOLID Principles in Java;
============================================
# 1. S – Single Responsibility Principle (SRP)

👉 A class should have only one reason to change (do one thing well).
```java 
❌ Bad Example:

class UserService {
    public void registerUser(String name) { /* save user */ }
    public void sendEmail(String message) { /* email logic */ } // extra responsibility
}


✅ Good Example:

class UserService {
    public void registerUser(String name) { /* save user */ }
}

class EmailService {
    public void sendEmail(String message) { /* email logic */ }
}
```
# 2. O – Open/Closed Principle (OCP)

👉 Classes should be open for extension, but closed for modification.
```java
❌ Bad Example:

class Shape {
    String type;
}

class AreaCalculator {
    public double calculate(Shape shape) {
        if (shape.type.equals("circle")) { /* logic */ }
        else if (shape.type.equals("square")) { /* logic */ }
        return 0;
    }
}


✅ Good Example (using polymorphism):

interface Shape { double area(); }

class Circle implements Shape {
    double r;
    Circle(double r) { this.r = r; }
    public double area() { return Math.PI * r * r; }
}

class Square implements Shape {
    double s;
    Square(double s) { this.s = s; }
    public double area() { return s * s; }
}
```
# 3. L – Liskov Substitution Principle (LSP)

👉 Subclasses should be usable via base class reference without breaking behavior.
```java
❌ Bad Example:

class Bird { void fly() {} }
class Ostrich extends Bird { void fly() { throw new UnsupportedOperationException(); } }


✅ Good Example:

interface Bird {}
interface FlyableBird extends Bird { void fly(); }

class Sparrow implements FlyableBird {
    public void fly() { System.out.println("Flying"); }
}
class Ostrich implements Bird { /* no fly method */ }
```
# 4. I – Interface Segregation Principle (ISP)

👉 Don’t force classes to implement unused methods.
```java
❌ Bad Example:

interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() {}
    public void eat() {} // unnecessary
}


✅ Good Example:

interface Workable { void work(); }
interface Eatable { void eat(); }

class Human implements Workable, Eatable {
    public void work() {}
    public void eat() {}
}

class Robot implements Workable {
    public void work() {}
}
```
# 5. D – Dependency Inversion Principle (DIP)

👉 Depend on abstractions, not concrete implementations.
👉 Definition:
High-level modules should not depend on low-level modules.
Both should depend on abstractions (interfaces).
```java
// Abstraction
interface Database {
    void connect();
}

// Low-level implementations
class MySQLDatabase implements Database {
    public void connect() { System.out.println("Connected to MySQL"); }
}

class PostgreSQLDatabase implements Database {
    public void connect() { System.out.println("Connected to PostgreSQL"); }
}

// High-level module
class UserRepository {
    private final Database db;

    // Depend on abstraction
    UserRepository(Database db) {
        this.db = db;
    }

    public void saveUser(String user) {
        db.connect();
        System.out.println("User saved: " + user);
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        Database db1 = new PostgreSQLDatabase(); // can swap with MySQLDatabase
        Database db2 = new MySQLDatabase(); // can swap with MySQLDatabase
        UserRepository repo = new UserRepository(db);
        repo.saveUser("John");
    }
}

```
“SOLID stands for five design principles:

SRP: one responsibility per class,

OCP: extend behavior without modifying existing code,

LSP: subclasses should work seamlessly in place of parent,

ISP: prefer small, specific interfaces,

DIP: depend on abstractions, not concrete classes.
