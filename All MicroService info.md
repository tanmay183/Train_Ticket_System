
## ✅ 1. **EUREKA SERVER (Service Registry)**

### 📌 **Purpose:**

* It's like a **central address book** for all your microservices.
* Other services register themselves here so that they can find and talk to each other by **name**, not by IP or port.

### 💡 Example:

* API Gateway doesn’t need to know User Service is on port 8101.
* It just says: "I want to talk to `USER` service", and Eureka handles the rest.

### 🛠️ What it does:

* Keeps track of all services (User, Booking, Admin, Gateway).
* Doesn’t process any user request directly.

---

## ✅ 2. **API GATEWAY (Single Entry Point)**

### 📌 **Purpose:**

* It acts as a **gatekeeper or main door** to the entire system.
* All external API calls go through here.

### 💡 Example:

* If the frontend calls `/user/getUserById/1`, it goes to API Gateway.
* Gateway will **forward** that request to the correct microservice (e.g., `USER`).

### 🛠️ What it does:

* Manages routing to services like USER, BOOKING, ADMIN.
* Handles CORS, logging, security (can be added later).
* Acts like a **traffic controller**.

---

## ✅ 3. **USER MICROSERVICE (User Management)**

### 📌 **Purpose:**

* Manages **user-related tasks** like:

  * Registering a user
  * Logging in
  * Storing user details (name, email, contact, address)

### 💡 Example:

* When a new user signs up, the request goes to this service.
* When Booking Service wants to get user info, it calls this service.

### 🛠️ What it does:

* Provides APIs like `/register`, `/login`, `/getUserById/{id}`.
* Stores user info in H2 database.

---

## ✅ 4. **ADMIN MICROSERVICE (Train Management)**

### 📌 **Purpose:**

* It is used by **Admin** to manage **Train information**:

  * Add a train
  * View all trains
  * Update or delete a train

### 💡 Example:

* When admin adds a new train from Angular dashboard → this service handles it.
* Booking Service also calls this service to check train seat availability.

### 🛠️ What it does:

* Stores train details like:

  * Train number, name
  * Source/Destination
  * Seat count
  * Ticket price, timings

---

## ✅ 5. **BOOKING MICROSERVICE (Ticket Booking)**

### 📌 **Purpose:**

* This is where **actual ticket booking happens**.
* Handles passengers, booking logic, seat assignment, and fare calculation.

### 💡 Example:

* When a user books 2 AC tickets:

  * Booking service checks seat availability in **Admin** service
  * Stores booking and passenger info
  * Decreases seat count in Admin service

### 🛠️ What it does:

* Calculates fare based on distance (e.g., AC ₹2.5/km, Sleeper ₹1.5/km)
* Assigns seat numbers (e.g., AC1, AC2, SL1)
* Stores passenger details and booking status
* Can cancel booking

---

## 🧩 How They Work Together (Simple Flow)

```
Frontend (Angular)
     ↓
API Gateway (9001)
     ↓
↓--------------↓--------------↓
USER       BOOKING       ADMIN
↓             ↓              ↓
DBs          ↓              DB
           ↓
        EUREKA
```

---

### 🔗 Example Journey:

> **User books a ticket for train 1101 from Mumbai to Pune**

1. Angular → Gateway → `/booking/bookTicket`
2. Booking microservice:

   * Gets user from `USER` microservice
   * Gets train from `ADMIN` microservice
   * Checks seat availability
   * Books ticket, saves passenger info
   * Updates train seat count in Admin

---

## ✅ Final Summary Table

| Microservice    | Port | Responsibility                    | Talks To                    |
| --------------- | ---- | --------------------------------- | --------------------------- |
| **Eureka**      | 8761 | Service registry                  | All other services register |
| **API Gateway** | 9001 | Entry point, routing, CORS        | Routes to other services    |
| **User**        | 8101 | Login, Register, Get user info    | Used by Booking             |
| **Admin**       | 8102 | Manage Train Info (CRUD)          | Used by Booking, Frontend   |
| **Booking**     | 8100 | Book Ticket, Cancel, Assign seats | Talks to User + Admin       |

---

