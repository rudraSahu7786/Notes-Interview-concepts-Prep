# Builder Design Pattern

The Builder Design Pattern is a creational design pattern that allows for the step-by-step construction of complex objects. It provides better control over the construction process and allows for creating immutable objects.

## Example: Building a `Person` Object

### Problem
Suppose we want to create a `Person` object with required fields like `firstName` and `lastName`, and optional fields like `age`, `address`, and `phoneNumber`. Using a constructor with many parameters can make the code hard to read and maintain.

### Solution
We can use the Builder Design Pattern to construct the `Person` object step by step.

---

### Implementation

```java
// Person class with Builder
public class Person {
    // Required parameters
    private final String firstName;
    private final String lastName;

    // Optional parameters
    private final Integer age;
    private final String address;
    private final String phoneNumber;

    // Private constructor
    private Person(PersonBuilder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.age = builder.age;
        this.address = builder.address;
        this.phoneNumber = builder.phoneNumber;
    }

    // Static nested Builder class
    public static class PersonBuilder {
        // Required parameters
        private final String firstName;
        private final String lastName;

        // Optional parameters
        private Integer age;
        private String address;
        private String phoneNumber;

        // Builder constructor with required parameters
        public PersonBuilder(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }

        // Setter methods for optional parameters
        public PersonBuilder setAge(Integer age) {
            this.age = age;
            return this;
        }

        public PersonBuilder setAddress(String address) {
            this.address = address;
            return this;
        }

        public PersonBuilder setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
            return this;
        }

        // Build method to create Person object
        public Person build() {
            return new Person(this);
        }
    }

    @Override
    public String toString() {
        return "Person{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                ", phoneNumber='" + phoneNumber + '\'' +
                '}';
    }
}
```

---

### Usage

```java
public class Main {
    public static void main(String[] args) {
        // Create a Person object using the Builder
        Person person = new Person.PersonBuilder("John", "Doe")
                .setAge(30)
                .setAddress("123 Main St")
                .setPhoneNumber("123-456-7890")
                .build();

        System.out.println(person);
    }
}
```

---

### Output

```
Person{firstName='John', lastName='Doe', age=30, address='123 Main St', phoneNumber='123-456-7890'}
```

---

### Advantages of Builder Pattern
1. **Readable Code**: The code is more readable and easier to understand.
2. **Immutable Objects**: The constructed object is immutable.
3. **Flexible Construction**: Allows for optional parameters without multiple constructors.

### When to Use
- When an object has many optional parameters.
- When you want to create immutable objects.
- When the construction process is complex.


