Sure! Here's a **short and clear summary** of each microservice in your **Train Ticket Management System** project:

---

### ✅ 1. `discovery-server` (Eureka Server)

* **Purpose:** Acts as a **service registry**.
* **Role:** Keeps track of all other microservices (train, user, admin, booking, etc.).
* **Technology:** Eureka Server (Netflix OSS)
* **Port:** `8761`
* **Access URL:** `http://localhost:8761`

---

### ✅ 2. `api-gateway`

* **Purpose:** Acts as the **central entry point** for all client (frontend) requests.
* **Role:** Routes requests to appropriate microservices based on URL path.
* **Technology:** Spring Cloud Gateway
* **Port:** `8080`
* **Key Feature:** Load-balanced routing (`lb://service-name`) via Eureka

---

### ✅ 3. `train-service`

* **Purpose:** Handles **Train-related operations**
* **Role:**

  * Add, update, delete train info
  * Search trains (e.g., by name prefix)
  * Fetch all available trains
* **Port:** `8083`
* **Database:** H2 (`traindb`)

---

### ✅ 4. `user-service`

* **Purpose:** Handles **user-side interactions**
* **Role:**

  * View trains
  * Book tickets
  * See booking history (via booking-service)
* **Port:** `8081`
* **Database:** H2 (`userdb`)

---

### ✅ 5. `admin-service`

* **Purpose:** Handles **admin-side operations**
* **Role:**

  * Manage train data (calls train-service)
  * Monitor bookings
  * Generate reports (monthly, etc.)
* **Port:** `8082`
* **Database:** H2 (`admindb`)

---

### ✅ 6. `booking-service`

* **Purpose:** Handles **ticket bookings and history**
* **Role:**

  * Book tickets (user)
  * Track bookings (admin)
  * Could later send notifications or generate invoices
* **Port:** `8084`
* **Database:** H2 (`bookingdb`)

---

### 🔄 How They Interact (High-Level)

```
[Frontend: Angular (User/Admin)]
         ↓
     [API Gateway]
         ↓
 ┌───────────────┬───────────────┬──────────────┐
 │ train-service │ user-service  │ booking-svc  │
 │ admin-service communicates with all services │
 └───────────────┴───────────────┴──────────────┘
         ↓
   [Eureka (discovery-server)]
```

---

