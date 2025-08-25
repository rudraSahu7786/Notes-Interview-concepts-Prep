# LLD: Elevator System (Java)

## 🎯 Requirements (Minimal)
- Multiple elevators
- Elevator can move up/down, stop at requested floor
- User can press up/down button from floors
- User can select floor inside elevator
- System assigns nearest available elevator

## 📦 Main Classes

### 1. ElevatorSystem
**Fields:**
- `List<Elevator> elevators`
- `int totalFloors`

**Methods:**
- `requestElevator(int floor, Direction dir)`
- `step()` → move elevators one step

### 2. Elevator
**Fields:**
- `int id`
- `int currentFloor`
- `Direction direction`
- `PriorityQueue<Integer> upRequests`
- `PriorityQueue<Integer> downRequests`

**Methods:**
- `move()`
- `addRequest(int floor)`
- `openDoor()`, `closeDoor()`

### 3. Floor
**Fields:**
- `int floorNumber`

**Methods:**
- `pressUpButton()`
- `pressDownButton()`

### 4. Request
**Fields:**
- `int floor`
- `Direction dir`

## 🏷️ Enums
```java
enum Direction { UP, DOWN, IDLE; }
```

## 🔑 Flow
- User presses floor button → `ElevatorSystem.requestElevator(floor, dir)`
- Nearest idle/optimal elevator assigned
- Inside elevator → user presses floor → `Elevator.addRequest(floor)`
- `Elevator.move()` processes requests in order (up then down)
- Doors open/close at requested floors

## ⚡ Example (Simplified Usage)
```java
ElevatorSystem system = new ElevatorSystem(3, 10); // 3 elevators, 10 floors
system.requestElevator(5, Direction.UP);  // User at 5th floor presses UP
system.step(); // move elevators one step
```

## 🚀 Extensions (Not in minimal LLD)
- Smart scheduling (look-ahead algorithm)
- Emergency stop
- Load capacity
- Multiple users queueing
