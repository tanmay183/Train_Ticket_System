## ✅ Microservice Name: `User`

This microservice is responsible for **user management** in the Train Ticket Management System. It provides APIs for:

* Registering a new user 👤
* Logging in a user 🔐
* Fetching user details by ID 📋
* Handling user data like name, email, password, address, contact, and user type

---

## 🔍 File-by-File Detailed Analysis of User Microservice

---

### ✅ 1. **File Name: `UserApplication.java`**

📄 **Location**: `src/main/java/com/tcs/User/UserApplication.java`

### 🔧 Code:

```java
@SpringBootApplication
@EnableEurekaClient
public class UserApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserApplication.class, args);
    }
}
```

### 🧠 **Explanation**:

* `@SpringBootApplication`: Starts the Spring Boot application.
* `@EnableEurekaClient`: Registers this service with the **Eureka Server** so it can be discovered by others (like API Gateway).

✅ **Purpose**: Boots the microservice and connects it to Eureka.

---

### ✅ 2. **Model Class: `User.java`**

📄 **Location**: `com.tcs.User.model.User`

### 🔧 Key Fields:

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private int userId;
private String userName;
private String email;
private String password;
private String address;
private String contact;
private String type;
```

### 🧠 **Explanation**:

* Represents the `user` table in H2 database.
* Includes all personal and login information.
* Includes multiple constructors and getter/setter methods.

✅ **Purpose**: Used to map user data to database rows.

---

### ✅ 3. **Repository: `UserRepository.java`**

📄 **Location**: `com.tcs.User.repository.UserRepository`

### 🔧 Code:

```java
public interface UserRepository extends JpaRepository<User, Integer> {
    Optional<User> findByEmail(String email);
}
```

### 🧠 **Explanation**:

* Provides CRUD methods for User.
* Adds a custom method `findByEmail` to find a user during login.

✅ **Purpose**: DAO interface to access user data in the database.

---

### ✅ 4. **Service: `UserService.java`**

📄 **Location**: `com.tcs.User.service.UserService`

### 🔧 Key Methods:

```java
public User createUser(User user)
public User getUserById(int id)
public List<User> getAllUsers()
public String login(String email, String password)
```

### 🧠 **Explanation**:

* `createUser`: Registers a new user.
* `getUserById`: Fetches user by ID (used by Booking microservice).
* `getAllUsers`: Returns list of all users (Admin Panel use case).
* `login`: Verifies user credentials.

✅ **Purpose**: Contains core business logic for user operations.

---

### ✅ 5. **Controller: `UserController.java`**

📄 **Location**: `com.tcs.User.controller.UserController`

### 🔧 Endpoints:

```java
@PostMapping("/register") → Register new user
@GetMapping("/getUserById/{id}") → Get user by ID
@GetMapping("/getAllUsers") → Get list of all users
@PostMapping("/login") → User login
```

### 🧠 **Explanation**:

* Exposes REST APIs to other services (like Booking) and frontend (Angular or Postman).
* Each endpoint connects to the service layer.

✅ **Purpose**: Provides HTTP access to user management functions.

---

### ✅ 6. **DTO: `UserDTO.java`**

📄 **Location**: `com.tcs.User.DTO.UserDTO`

### 🔧 Fields:

```java
private int userId;
private String userName;
private String email;
private List<Booking> bookings;
```

### 🧠 **Explanation**:

* Used to send user data along with booking info from Booking service.
* Doesn't expose sensitive fields like password.

✅ **Purpose**: Serves as a safe, frontend-friendly user info format.

---

### ✅ 7. **Configuration: `application.properties`**

### 🔧 Key Properties:

```properties
spring.application.name=USER
server.port=8101
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:UserDB

eureka.client.register-with-eureka=true
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
```

### 🧠 **Explanation**:

* Sets up port (8101) and in-memory H2 DB.
* Registers with Eureka.

✅ **Purpose**: Configures the microservice and Eureka settings.

---

## ✅ Summary: What the User Microservice Does

| Feature                 | Description                                   |
| ----------------------- | --------------------------------------------- |
| 👤 **Register User**    | Accepts user details and creates a new record |
| 🔐 **Login User**       | Validates email and password                  |
| 📋 **Get User by ID**   | Useful for Booking and Admin microservices    |
| 📊 **Get All Users**    | Admin feature or future frontend use          |
| 🧠 **Business Logic**   | All logic separated in service layer          |
| 🗃️ **Database Access** | Uses JPA to store users in H2 database        |
| 🌐 **Eureka Discovery** | Registers with Eureka for service lookup      |


