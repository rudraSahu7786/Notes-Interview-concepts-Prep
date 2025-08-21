SOLID Principles in Java;
============================================
1. S – Single Responsibility Principle (SRP)

👉 A class should have only one reason to change (do one thing well).

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

2. O – Open/Closed Principle (OCP)

👉 Classes should be open for extension, but closed for modification.

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

3. L – Liskov Substitution Principle (LSP)

👉 Subclasses should be usable via base class reference without breaking behavior.

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

4. I – Interface Segregation Principle (ISP)

👉 Don’t force classes to implement unused methods.

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

5. D – Dependency Inversion Principle (DIP)

👉 Depend on abstractions, not concrete implementations.

❌ Bad Example:

class MySQLDatabase {
    void connect() {}
}

class UserRepository {
    MySQLDatabase db = new MySQLDatabase(); // tightly coupled
}


✅ Good Example:

interface Database { void connect(); }

class MySQLDatabase implements Database {
    public void connect() {}
}

class UserRepository {
    private Database db;
    UserRepository(Database db) { this.db = db; }
}

✅ Interview-Ready Summary

“SOLID stands for five design principles:

SRP: one responsibility per class,

OCP: extend behavior without modifying existing code,

LSP: subclasses should work seamlessly in place of parent,

ISP: prefer small, specific interfaces,

DIP: depend on abstractions, not concrete classes.
