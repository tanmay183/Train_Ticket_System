### ✅ 1. `Passenger.java` (`com.tcs.Booking.model.Passenger`)

**Purpose**: Represents a single passenger’s information (name, age, gender, seat info, etc.) in a booking.

**Annotations & Key Points**:

* `@Entity(name="passenger_pbl")`: JPA will create a table named `passenger_pbl`.
* `@Id` and `@GeneratedValue`: Marks `id` as the primary key, auto-generated.
* Two constructors are provided — one with `id`, one without (used while saving new passengers).
* Standard getters/setters.
* `SeatNumber` is incorrectly capitalized; it should ideally be `seatNumber`.

---

### ✅ 2. `Train.java` (`com.tcs.Booking.model.Train`)

**Purpose**: Represents the Train details like number, name, departure/arrival, seat counts, etc.
**Note**: This is used as a *remote object* fetched using Feign from Admin service.

* Not annotated with `@Entity`, which is correct — it’s only used for data transfer.
* Constructors for both full and partial initialization.
* Contains fields for AC/sleeper seats (total and available).
* Used by BookingService to calculate seat availability and fare.

---

### ✅ 3. `User.java` (`com.tcs.Booking.model.User`)

**Purpose**: Represents the user who makes the booking.
**Note**: Also fetched remotely via Feign from the User microservice.

* Not an entity, only a DTO-like class.
* Contains user information like name, email, address, type (admin/user).
* Contains a default constructor, full constructor, getters, setters, and `toString()`.

---

### ✅ 4. `BookingRepository.java`

**Purpose**: Interface for CRUD operations on `Booking` table.

* Extends `JpaRepository<Booking, Integer>`.
* Custom method:

  * `List<Booking> findByUserId(int userId)`: Finds all bookings of a specific user.

---

### ✅ 5. `PassengerRepository.java`

**Purpose**: Repository for the `Passenger` table.

* Extends `JpaRepository<Passenger, Integer>`.
* Custom method:

  * `List<Passenger> findByBookingId(int bookingId)`: Fetches all passengers for a booking.

---

### ✅ 6. `BookingService.java`

**Purpose**: Main service logic to:

* Book tickets
* Cancel bookings
* Get booking details by user or ID
* Map passenger and train data
* Handle availability

**Main Features**:

1. **`BookTicket(...)`**:

   * Takes user ID, trainNo, date, seat counts, and passenger details.
   * Checks if user and train exist.
   * Validates seat availability.
   * Calculates fare based on distance and seat type.
   * Allocates seat numbers like `AC1`, `SL1`.
   * Saves booking and passengers.
   * Updates train seat availability via Feign client.

2. **`cancelBooking(...)`**:

   * Sets status to `CANCELLED` for a booking.

3. **`getBooking(...)`**:

   * Returns `BookingDTO` with passenger and user info.

4. **`getAllBookingsOfUser(...)`**:

   * Returns `UserDTO` with all bookings of a user.

5. **Private `mapper(...)` methods**:

   * Converts entity → DTO objects (`Booking → BookingDTO`, `User → UserDTO`).

---

### ✅ 7. `PassengerService.java`

**Purpose**: Handles saving and retrieving passengers.

* Saves all passengers with `saveAllPassenger`.
* Finds passengers by booking ID.

---

### ✅ 8. `TrainService.java` (Feign Client)

**Purpose**: Used to fetch and update train data from the **Admin microservice**.

* `getTrainByNo(int trainNo)`
* `updateTrain(Train train)`

---

### ✅ 9. `UserService.java` (Feign Client)

**Purpose**: Used to fetch user data from the **User microservice**.

* `getUser(int id)`

---

### ✅ 10. `application.properties`

**Purpose**: Spring config.

Key properties:

* `spring.application.name=Booking`
* `spring.datasource.url=jdbc:h2:mem:BookingDB` — in-memory DB for dev/test
* `server.port=8100`
* Eureka client config to register to `localhost:8761`

---

### 🔍 Summary of Booking Microservice Responsibility

| Functionality            | Description                                                               |
| ------------------------ | ------------------------------------------------------------------------- |
| **Booking Tickets**      | Receives user ID, train no, date, seats, passenger list → creates booking |
| **Cancelling Booking**   | Sets booking status to "CANCELLED"                                        |
| **Fetching Bookings**    | Retrieves bookings by user ID or booking ID                               |
| **Passenger Handling**   | Maps passengers to seat numbers, saves them                               |
| **Seat Management**      | Updates available seat counts on train via Admin service                  |
| **User/Train Retrieval** | Uses Feign clients to fetch details from User/Admin services              |


