# Singleton Design Pattern in Java

The **Singleton Pattern** ensures that a class has **only one instance** throughout the application lifecycle and provides a global access point to that instance.

---

## ✅ Key Points

* Only **one instance** of the class exists.
* Constructor is **private** → prevents direct object creation.
* A **static method** (usually `getInstance()`) returns the instance.
* Used for shared resources like **logging, configuration, thread pools, cache, DB connections**.

---

## 📝 Basic Example (Thread-Unsafe)

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

❌ Problem → Not thread-safe (multiple threads can create multiple instances).

---

## 📝 Thread-Safe Singleton with Double-Check Locking

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

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
```

✅ Features:

* `volatile` prevents instruction reordering issues.
* Double-check locking improves performance by synchronizing only on the first initialization.

---

## 📝 Eager Initialization (Simpler but less flexible)

```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

✅ Instance created at class loading.
❌ May waste memory if instance is never used.

---

## 📝 Bill Pugh Singleton (Best Practice)

```java
public class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```

✅ Thread-safe, efficient, no synchronization overhead.

---

## 🔄 Example Usage

```java
public class Main {
    public static void main(String[] args) {
        Singleton obj1 = Singleton.getInstance();
        Singleton obj2 = Singleton.getInstance();

        System.out.println(obj1 == obj2); // true
    }
}
```

---

## ✅ Advantages

* Controlled access to a single instance.
* Saves memory (one instance only).
* Useful for shared resources (DB connections, logging).

## ❌ Disadvantages

* Difficult to unit test (global state).
* Hidden dependencies.
* Breaks **Single Responsibility Principle** (class controls its lifecycle).
* If misused, can become **global variable with side effects**.

---

## 💡 Best Practices

1. Use **Bill Pugh Singleton** (recommended).
2. Avoid reflection attack (can break Singleton) → handle in constructor.
3. For serialization, override `readResolve()` to return the same instance.

---

## 📌 Interview Tips

* Be ready to implement **Thread-safe Singleton**.
* Know about **Double-checked locking** and why `volatile` is needed.
* Explain difference between **Eager vs Lazy initialization**.
* Be prepared to discuss **pros/cons in real-world systems**.

---

## ✅ Summary

* **Singleton = One instance + Global access point.**
* Different implementations: **Lazy, Eager, Double-Check, Bill Pugh**.
* Widely used but should be applied carefully to avoid tight coupling.
