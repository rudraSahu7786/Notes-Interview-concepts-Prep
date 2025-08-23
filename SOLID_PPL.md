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
❌ Bad Example (Breaks LSP)
class Bird { void fly() {} }
class Ostrich extends Bird { 
    void fly() { throw new UnsupportedOperationException(); } 
}


Problem: Ostrich is a Bird, but it cannot fly.

When used via Bird reference, calling fly() will break behavior.

This violates LSP.

✅ Good Example (Respects LSP)
interface Bird {}

interface FlyableBird extends Bird {
    void fly();
}

class Sparrow implements FlyableBird {
    public void fly() { System.out.println("Flying"); }
}

class Ostrich implements Bird {
    // Ostrich doesn't have fly() -> no broken contract
}


```


Separation of FlyableBird and Bird removes incorrect assumptions.

Now Ostrich can still be a Bird without being forced to fly().

Sparrow implements FlyableBird so it can fly.

This way, any substitution works correctly.

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
❌ Bad Example (without DIP)
class WiredKeyboard {
    public void type() { System.out.println("Typing with wired keyboard"); }
}

class Computer {
    private WiredKeyboard keyboard = new WiredKeyboard(); // tightly coupled

    public void use() {
        keyboard.type();
    }
}

```

Problem
--------

Computer is tightly bound to WiredKeyboard.

If we want to use a WirelessKeyboard, we must change Computer class → violates DIP.
```java 
✅ Good Example (with DIP)
// Abstraction
interface Keyboard {
    void type();
}

// Low-level modules
class WiredKeyboard implements Keyboard {
    public void type() { System.out.println("Typing with wired keyboard"); }
}

class WirelessKeyboard implements Keyboard {
    public void type() { System.out.println("Typing with wireless keyboard"); }
}

// High-level module depends on abstraction
class Computer {
    private Keyboard keyboard;

    // Inject dependency via constructor
    public Computer(Keyboard keyboard) {
        this.keyboard = keyboard;
    }

    public void use() {
        keyboard.type();
    }
}

Usage
public class Main {
    public static void main(String[] args) {
        Keyboard wired = new WiredKeyboard();
        Keyboard wireless = new WirelessKeyboard();

        Computer c1 = new Computer(wired);
        c1.use(); // Typing with wired keyboard

        Computer c2 = new Computer(wireless);
        c2.use(); // Typing with wireless keyboard
    }
}

```
“SOLID stands for five design principles:

SRP: one responsibility per class,

OCP: extend behavior without modifying existing code,

LSP: subclasses should work seamlessly in place of parent,

ISP: prefer small, specific interfaces,

DIP: depend on abstractions, not concrete classes.
