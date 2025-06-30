## ✅ 1. `ApigatewayApplication.java`

**📁 Path**: `com.tcs.apigateway`

```java
@SpringBootApplication
public class ApigatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(ApigatewayApplication.class, args);
    }
}
```

### 📌 Explanation:

* This is the **main Spring Boot launcher class** for the API Gateway.
* `@SpringBootApplication`: Enables component scan, auto-configuration, and Spring Boot setup.
* It **starts the API Gateway** on port `9001` (configured below).

---

## ✅ 2. `application.yml`

```yaml
spring:
  application:
    name: api-gateway
  config:
    import: optional:configserver
  cloud:
    gateway:
      default-filters:
        - DedupeResponseHeader=Access-Control-Allow-Credentials Access-Control-Allow-Origin
      globalcors:
        corsConfigurations:
          '[/**]':
            allowedOrigins: "*"
            allowedHeaders: "*"
            allowedMethods:
              - GET
              - POST
              - DELETE
              - PUT
              - OPTIONS
      routes:
        - id: USER
          uri: lb://USER
          predicates:
            - Path=/user**
```

### 📌 Explanation:

#### 🛠 `spring.application.name=api-gateway`

* Registers this app with Eureka as **`api-gateway`**.

#### 🌐 `spring.cloud.gateway.routes`

* **This is the most important section**. It configures how incoming requests are routed.

```yaml
- id: USER
  uri: lb://USER
  predicates:
    - Path=/user**
```

This says:

* If any request comes to `/user**` (e.g., `/user/login`, `/user/register`, etc.),
* It should be forwarded to the **USER microservice** (discovered by Eureka using load balancing: `lb://USER`).

> 💡 This means:
> The API Gateway is acting like a reverse proxy. Instead of calling `localhost:8101/getUserById/1`, clients will call `localhost:9001/user/getUserById/1`.

#### 🌍 `globalcors`

* This configuration **enables CORS**, allowing your frontend (Angular) to access backend resources through this gateway.

---

## ✅ 3. `pom.xml`

### Key dependencies:

```xml
<dependencies>
    <!-- For routing requests using Spring WebFlux -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>

    <!-- Core Spring Cloud Gateway dependency -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId>
    </dependency>

    <!-- Eureka Client to register with Eureka server -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>

    <!-- Swagger/OpenAPI support -->
    <dependency>
        <groupId>org.springdoc</groupId>
        <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
        <version>2.0.4</version>
    </dependency>
</dependencies>
```

### 📌 Explanation:

* `spring-boot-starter-webflux`: Required for non-blocking routing.
* `spring-cloud-starter-gateway`: Provides API Gateway functionality.
* `spring-cloud-starter-netflix-eureka-client`: Enables service discovery via Eureka.
* `springdoc-openapi`: (Optional but helpful) to expose API documentation (Swagger UI) if needed.

---

## ✅ 4. Eureka Configuration (inside `application.yml`):

```yaml
eureka:
  instance:
    prefer-ip-address: false
  client:
    register-with-eureka: true
    service-url:
      defaultZone: http://localhost:8761/eureka
```

### 📌 Explanation:

* This makes the API Gateway **register itself with the Eureka Server**.
* It can then **discover other microservices** like `USER`, `ADMIN`, `BOOKING`, etc.

---

## 🔍 Summary of What API Gateway Does

| Feature                          | Description                                                                 |
| -------------------------------- | --------------------------------------------------------------------------- |
| ✅ **Single Entry Point**         | It provides one endpoint for the frontend: `http://localhost:9001/`         |
| 🚀 **Routing to Microservices**  | Forwards requests to the correct microservice (e.g., `/user/**` to USER MS) |
| 🌐 **CORS Support**              | Allows Angular or other web apps to call backend endpoints                  |
| ⚙️ **Service Discovery Enabled** | Uses Eureka to dynamically resolve service instances                        |
| 📦 **Scalable Routing Config**   | You can add more microservices under `routes:` (e.g., `ADMIN`, `BOOKING`)   |

---

## 🧩 Example: Future Route Config (Suggested)

To connect the full system, you will add routes like:

```yaml
routes:
  - id: USER
    uri: lb://USER
    predicates:
      - Path=/user/**
  - id: ADMIN
    uri: lb://ADMIN
    predicates:
      - Path=/admin/**
  - id: BOOKING
    uri: lb://BOOKING
    predicates:
      - Path=/booking/**
```

---

## ✅ Summary: API Gateway Microservice Role

| 🔧 Task               | 🎯 Responsibility                               |
| --------------------- | ----------------------------------------------- |
| Acts as Reverse Proxy | Routes frontend requests to appropriate backend |
| Central Gateway       | Hides microservice complexity from frontend     |
| Load Balanced         | Uses Eureka + Ribbon (LB) to balance load       |
| Simple CORS Setup     | Lets Angular/React clients access APIs safely   |


