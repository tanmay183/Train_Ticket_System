## ✅ Microservice Name: `Admin`

This microservice is responsible for performing **CRUD operations on train information**. It interacts with the database and exposes REST APIs to allow other services (like Booking and API Gateway) to:

* Add
* Update
* Delete
* Fetch trains

---

## 🔍 File-by-File Detailed Analysis of Admin Microservice

We'll go through the files based on what you’ve shared earlier.

---

### ✅ 1. **File Name: `AdminApplication.java`**

📄 **Location**: `src/main/java/com/tcs/admin/AdminApplication.java`

### 🔧 Code:

```java
@SpringBootApplication
@EnableEurekaClient
public class AdminApplication {
    public static void main(String[] args) {
        SpringApplication.run(AdminApplication.class, args);
    }
}
```

### 🧠 **Explanation**:

* `@SpringBootApplication`: Marks this as a Spring Boot application.
* `@EnableEurekaClient`: Registers this service with **Eureka Server** for service discovery.

✅ **What it does**: Bootstraps the Admin microservice and registers it with Eureka at startup.

---

### ✅ 2. **Model Class: `Train.java`**

📄 **Location**: `com.tcs.admin.model.Train`

Represents the **train entity** stored in the database.

### 🔧 Key fields:

```java
@Id
@GeneratedValue
private int trainId;
private int trainNo;
private String trainName;
private String originStation;
private String destinationStation;
private String departureTime;
private String arrivalTime;
private int totalAcSeats;
private int availableAcSeats;
private int totalSleeperSeats;
private int availableSleeperSeats;
private int distance;
```

### 🧠 **Explanation**:

* Contains all details related to a train.
* Used by JPA to create a `train` table.
* Has proper constructors, getters, setters, and a `toString()` method.

✅ **What it does**: Maps to the `train` table and is used in CRUD operations.

---

### ✅ 3. **Repository: `TrainRepository.java`**

📄 **Location**: `com.tcs.admin.repository.TrainRepository`

### 🔧 Code:

```java
public interface TrainRepository extends JpaRepository<Train, Integer> {
    Train findByTrainNo(int trainNo);
}
```

### 🧠 **Explanation**:

* Extends Spring Data JPA to allow default CRUD methods.
* `findByTrainNo(int trainNo)` is a custom finder to get train by its number.

✅ **What it does**: Acts as a DAO layer to fetch and save train data from H2 DB.

---

### ✅ 4. **Service: `TrainService.java`**

📄 **Location**: `com.tcs.admin.service.TrainService`

### 🔧 Methods:

* `addTrain(Train train)`
* `updateTrain(Train train)`
* `getAllTrains()`
* `getTrainByNo(int trainNo)`
* `deleteTrain(int trainId)`

### 🧠 **Explanation**:

This layer handles the business logic:

* Validate data before saving.
* Check if the train exists before update/delete.
* Acts as an intermediary between controller and repository.

✅ **What it does**: Implements the logic to add, retrieve, update and delete trains.

---

### ✅ 5. **Controller: `TrainController.java`**

📄 **Location**: `com.tcs.admin.controller.TrainController`

### 🔧 Endpoints:

```java
@PostMapping("/addTrain") → add a train  
@GetMapping("/getTrains") → get all trains  
@GetMapping("/getTrainByNo/{trainNo}") → get train by train number  
@PutMapping("/updateTrain") → update train details  
@DeleteMapping("/deleteTrain/{id}") → delete train by id
```

### 🧠 **Explanation**:

* Handles HTTP requests.
* Maps URLs to service layer methods.
* Used by external clients like **Booking microservice** via FeignClient.

✅ **What it does**: Exposes REST endpoints for performing CRUD operations on trains.

---

### ✅ 6. **application.properties**

```properties
spring.application.name=ADMIN
server.port=8102
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:AdminDB

eureka.client.register-with-eureka=true
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
```

### 🧠 **Explanation**:

* Names this service as `ADMIN` (used in Eureka and API Gateway routing).
* Sets port to `8102`.
* Configures in-memory H2 database.
* Registers the service with Eureka.

✅ **What it does**: Configures the admin service and enables Eureka registration.

---

## ✅ Summary: What Admin Microservice Does

| Responsibility               | Description                                                 |
| ---------------------------- | ----------------------------------------------------------- |
| 🚅 **Manage Train Data**     | Add, update, delete, fetch train details                    |
| 🔄 **Exposes APIs**          | RESTful endpoints to be used by frontend and other services |
| 🧠 **Business Logic**        | Validates train details, handles seat availability, etc.    |
| 🗃️ **Persists to DB**       | Uses H2 in-memory DB to store train records                 |
| 🌐 **Registers with Eureka** | Enables other services to discover it via Eureka            |
| 🔗 **Used by**               | Booking Service (via Feign Client), API Gateway             |

---

