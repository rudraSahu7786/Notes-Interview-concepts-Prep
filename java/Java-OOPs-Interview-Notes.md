# Java OOPs Interview Notes

## 1. OOPs Concepts in Java

### 1.1 Class & Object
- **Class:** Blueprint for objects, defines properties (fields) & behaviors (methods).
- **Object:** Instance of a class.

```java
class Car {
    String color;
    void drive() { System.out.println("Car is driving"); }
}
Car myCar = new Car();
myCar.drive();
```

### 1.2 Inheritance
- Mechanism for one class (child) to acquire properties/methods of another (parent).
- Promotes code reuse.

```java
class Animal { void eat() { System.out.println("eating"); }}
class Dog extends Animal { void bark() { System.out.println("barking"); }}
Dog d = new Dog();
d.eat(); // Inherited
```

### 1.3 Polymorphism
- **Compile-time (Method Overloading):** Same method name, different parameters.
- **Run-time (Method Overriding):** Subclass redefines parent’s method.

```java
class MathOp {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
}
class Animal { void sound() { System.out.println("Generic sound"); } }
class Dog extends Animal { void sound() { System.out.println("Bark"); } }
```

### 1.4 Encapsulation
- Wrapping data (fields) and code (methods) into a single unit (class).
- Achieved by making fields private & providing public getters/setters.

```java
class Person {
    private String name;
    public String getName() { return name; }
    public void setName(String n) { name = n; }
}
```

### 1.5 Abstraction
- Hiding internal details, showing only essential features.
- Achieved using abstract classes & interfaces.

```java
abstract class Shape {
    abstract void draw();
}
class Circle extends Shape {
    void draw() { System.out.println("Drawing Circle"); }
}
```

---

## 2. Java Code Architecture (How Java Works)

1. **Write Code:** In .java files.
2. **Compilation:** `javac` compiles .java files to .class (bytecode).
3. **Class Loader:** Loads .class files into JVM.
4. **JVM Execution:** JVM interprets bytecode and executes on the host machine.
5. **JRE & JDK:** JRE = JVM + Libraries; JDK = JRE + Development Tools.

**Simple Flow:**
```
.java (source code) --[javac]--> .class (bytecode) --[JVM]--> Output
```

---

## 3. Key Interview Points

- **Constructor:** Special method for object creation, same name as class, no return type.
- **this & super:** `this` refers to current object; `super` refers to parent class object.
- **Access Modifiers:** `private`, `default`, `protected`, `public`
- **Static Keyword:** Belongs to class, not instance.
- **Interface vs Abstract Class:** Interface: all methods public & abstract (Java 8+: default/static allowed); Abstract class can have concrete methods.

---

## 4. Quick Reference Table

| Concept        | Description                                 | Example/Keyword        |
|----------------|---------------------------------------------|------------------------|
| Class/Object   | Blueprint/Instance                          | `class`, `new`         |
| Inheritance    | IS-A relationship                           | `extends`              |
| Polymorphism   | Many forms (overload/override)              | `@Override`            |
| Encapsulation  | Data hiding with accessors                  | `private`, getters     |
| Abstraction    | Hiding implementation, showing interface    | `abstract`, `interface`|

---

## 5. Sample Java Program

```java
class Animal {
    void sound() { System.out.println("Animal sound"); }
}

class Dog extends Animal {
    void sound() { System.out.println("Bark"); }
}

public class TestOOPs {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.sound(); // Output: Bark
    }
}
```

---

> **Tip:** Always mention real-world examples and explain concepts with code in interviews!