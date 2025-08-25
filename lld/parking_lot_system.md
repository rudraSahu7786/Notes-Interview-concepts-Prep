# 🅻🅻🅳: Parking Lot System (Java)

## 🎯 Requirements (Minimal)
- Multiple parking slots.
- Vehicles: Car, Bike.
- Entry → Assign nearest available slot.
- Exit → Free the slot, calculate fare.
- Display available slots.

## 📦 Main Classes

### 1. ParkingLot
- **Fields:** `List<ParkingFloor> floors`, `String name`.
- **Methods:**
    - `parkVehicle(Vehicle v)`
    - `unparkVehicle(String ticketId)`
    - `displayAvailableSlots()`

### 2. ParkingFloor
- **Fields:** `int floorNumber`, `List<ParkingSlot> slots`.
- **Methods:**
    - `getAvailableSlot(VehicleType type)`
    - `freeSlot(int slotId)`

### 3. ParkingSlot
- **Fields:** `int slotId`, `boolean isAvailable`, `VehicleType type`, `Vehicle vehicle`.
- **Methods:**
    - `assignVehicle(Vehicle v)`
    - `removeVehicle()`

### 4. Vehicle (Abstract)
- **Fields:** `String vehicleNo`, `VehicleType type`.
- **Subclasses:** Car, Bike.

### 5. Ticket
- **Fields:** `String ticketId`, `Vehicle vehicle`, `ParkingSlot slot`, `LocalDateTime entryTime`.

### 6. Payment
- **Method:** `calculateFare(Ticket t, LocalDateTime exitTime)`

## 🏷️ Enums
```java
enum VehicleType { CAR, BIKE; }
```

## 🔑 Flow
- User enters → `ParkingLot.parkVehicle()` → generate Ticket.
- Slot assigned based on nearest available.
- On exit → `unparkVehicle(ticketId)` → free slot + calculate fare.
- Admin/User can call `displayAvailableSlots()`.

## ⚡ Example (Simplified Usage)
```java
ParkingLot lot = new ParkingLot("MyLot", 2, 10);  // 2 floors, 10 slots each
Vehicle car = new Car("JH01AB1234");
Ticket t = lot.parkVehicle(car);
lot.unparkVehicle(t.getTicketId());
lot.displayAvailableSlots();
```

---

👉 This is minimal LLD. For extensions, you can add:
- Different fare strategies (Car vs Bike).
- Multiple entry/exit gates.
- Reservation system.
- Exception handling.
