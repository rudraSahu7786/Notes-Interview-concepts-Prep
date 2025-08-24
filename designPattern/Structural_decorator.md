# Decorator Design Pattern: Coffee Shop Example

The **Decorator Design Pattern** is a structural design pattern that allows behavior to be added to individual objects, dynamically, without affecting the behavior of other objects from the same class.

## Concept

In a coffee shop, customers can order a base coffee and then add various toppings or extras like milk, sugar, or whipped cream. Each topping adds to the cost of the coffee dynamically. This is a perfect analogy for the decorator pattern.

## Implementation

### Components:
1. **Base Component**: Represents the core object (e.g., plain coffee).
2. **Decorator**: Wraps the base component and adds additional behavior (e.g., milk, sugar).

### Example

```java
// Base Component
public interface Coffee {
    String getDescription();
    double getCost();
}

// Concrete Component
public class PlainCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Plain Coffee";
    }

    @Override
    public double getCost() {
        return 2.0;
    }
}

// Abstract Decorator
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription();
    }

    @Override
    public double getCost() {
        return coffee.getCost();
    }
}

// Concrete Decorators
public class Milk extends CoffeeDecorator {
    public Milk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.5;
    }
}

public class Sugar extends CoffeeDecorator {
    public Sugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.2;
    }
}

// Usage
public class CoffeeShop {
    public static void main(String[] args) {
        Coffee coffee = new PlainCoffee();
        coffee = new Milk(coffee);
        coffee = new Sugar(coffee);

        System.out.println("Order: " + coffee.getDescription());
        System.out.println("Total Cost: $" + coffee.getCost());
    }
}
```

### Output
```
Order: Plain Coffee, Milk, Sugar
Total Cost: $2.7
```

## Advantages
- Flexibility to add/remove features at runtime.
- Promotes the Open/Closed Principle.

## Disadvantages
- Can lead to a large number of small classes.
- Complex to manage if there are many decorators.

The decorator pattern is a powerful tool for dynamically extending functionality while keeping the codebase clean and maintainable.