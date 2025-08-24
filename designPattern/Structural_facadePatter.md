# Facade Pattern

The **Facade Pattern** is a structural design pattern that provides a simplified interface to a larger body of code, such as a complex subsystem. It hides the complexities of the system and provides an easy-to-use interface for the client.

## Intent
The Facade Pattern:
- Simplifies the interaction between the client and the subsystem.
- Reduces dependencies between the client and the subsystem.
- Promotes loose coupling.

---

## Example: Home Theater System

Imagine a home theater system with multiple components like a DVD player, projector, amplifier, and speakers. Each component has its own interface and requires specific steps to operate. The Facade Pattern can simplify this by providing a unified interface to control the entire system.

### Without Facade
The client has to interact with each component individually:

```java
public class HomeTheaterTest {
    public static void main(String[] args) {
        Amplifier amp = new Amplifier();
        DVDPlayer dvd = new DVDPlayer();
        Projector projector = new Projector();
        Screen screen = new Screen();
        Lights lights = new Lights();

        lights.dim(10);
        screen.down();
        projector.on();
        amp.on();
        amp.setVolume(5);
        dvd.on();
        dvd.play("Inception");
    }
}
```

This approach is cumbersome and tightly couples the client with the subsystem.

---

### With Facade
Using a facade, we can simplify the process:

#### Facade Class
```java
public class HomeTheaterFacade {
    private Amplifier amp;
    private DVDPlayer dvd;
    private Projector projector;
    private Screen screen;
    private Lights lights;

    public HomeTheaterFacade(Amplifier amp, DVDPlayer dvd, Projector projector, Screen screen, Lights lights) {
        this.amp = amp;
        this.dvd = dvd;
        this.projector = projector;
        this.screen = screen;
        this.lights = lights;
    }

    public void watchMovie(String movie) {
        System.out.println("Get ready to watch a movie...");
        lights.dim(10);
        screen.down();
        projector.on();
        amp.on();
        amp.setVolume(5);
        dvd.on();
        dvd.play(movie);
    }

    public void endMovie() {
        System.out.println("Shutting down the home theater...");
        dvd.stop();
        dvd.off();
        amp.off();
        projector.off();
        screen.up();
        lights.on();
    }
}
```

#### Client Code
```java
public class HomeTheaterTest {
    public static void main(String[] args) {
        Amplifier amp = new Amplifier();
        DVDPlayer dvd = new DVDPlayer();
        Projector projector = new Projector();
        Screen screen = new Screen();
        Lights lights = new Lights();

        HomeTheaterFacade homeTheater = new HomeTheaterFacade(amp, dvd, projector, screen, lights);

        homeTheater.watchMovie("Inception");
        homeTheater.endMovie();
    }
}
```

---

## Benefits of Facade Pattern
1. **Simplifies Usage**: Provides a single interface to interact with a complex subsystem.
2. **Reduces Coupling**: The client is decoupled from the subsystem's internal details.
3. **Improves Maintainability**: Changes in the subsystem do not affect the client.

---

## When to Use
- When you want to provide a simple interface to a complex subsystem.
- When you want to decouple the client from the subsystem.
- When you want to layer your system and define entry points for each layer.

---

The Facade Pattern is a great way to manage complexity and improve the usability of your system.