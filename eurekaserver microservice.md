## ✅ Microservice Name: `eurekaserver`

This microservice acts as the **Service Registry** for your microservices architecture using **Spring Cloud Netflix Eureka**.

---

## ✅ Project Structure (as shown in your image):

```
eurekaserver
│
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.tcs.eurekaserver
│   │   │       └── EurekaserverApplication.java
│   │   └── resources
│   │       └── application.properties
├── pom.xml
```

---

# 🔍 File-by-File Detailed Analysis

---

## 1. ✅ **File Name: `EurekaserverApplication.java`**

📄 **Location**: `src/main/java/com/tcs/eurekaserver/EurekaserverApplication.java`

### 🔧 Code:

```java
package com.tcs.eurekaserver;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaserverApplication {

	public static void main(String[] args) {
		SpringApplication.run(EurekaserverApplication.class, args);
	}

}
```

---

### 🧠 **Explanation (Line-by-Line):**

* `package com.tcs.eurekaserver;`
  → Declares the package name for this main class.

* `import ...`
  → Imports required Spring Boot and Eureka Server classes.

* `@SpringBootApplication`
  → This is a standard Spring Boot annotation that:

  * Enables auto-configuration
  * Component scanning
  * Configures Spring application context

* `@EnableEurekaServer`
  → This is the **key annotation**.

  * It tells Spring Boot to start the **Eureka Server**.
  * This application will now behave as a **service registry**.

* `public static void main(...)`
  → Entry point for the Spring Boot application.

---

### ✅ What It Does:

* Starts up a Eureka server on the configured port (default 8761).
* Other microservices can register with this Eureka server using their `application.name`.

---

## 2. ✅ **File Name: `application.properties`**

📄 **Location**: `src/main/resources/application.properties`

### 🔧 Content:

```properties
spring.application.name=eurekaserver

server.port=8761

eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

---

### 🧠 **Explanation:**

* `spring.application.name=eurekaserver`
  → Gives this app a name (`eurekaserver`) for service registry & discovery.

* `server.port=8761`
  → Starts the Eureka Server on port `8761` (the default port for Eureka).

* `eureka.client.register-with-eureka=false`
  → Tells this service **NOT to register itself** as a client to Eureka.

  * Eureka Server should not register itself as a client.

* `eureka.client.fetch-registry=false`
  → Tells this service **NOT to fetch** the service registry.

  * It's the server, not a client.

---

### ✅ What It Does:

* Configures Eureka Server correctly.
* Sets it to act only as a **registry**, not as a service consumer.

---

## 3. ✅ **File Name: `pom.xml`**

📄 **Location**: Project Root

You didn’t share this file, but for Eureka to work correctly, it typically contains:

### 🔧 Required dependencies:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

And in `<dependencyManagement>`:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>2023.0.1</version> <!-- or latest -->
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

---

## ✅ Overall Summary of Eureka Server

| Feature                           | Description                                                                             |
| --------------------------------- | --------------------------------------------------------------------------------------- |
| 📂 `EurekaserverApplication.java` | Bootstraps the Eureka server with `@EnableEurekaServer`                                 |
| 🛠️ `application.properties`      | Sets name, port, and disables client-side registry/fetch                                |
| ⚙️ Purpose                        | Provides a central **directory** for all microservices to **register** and **discover** |

