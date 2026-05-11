# Spring-Bank-Backend

> Backend system for a banking application built with Spring Boot and a microservices architecture.

---

## Overview

Spring-Bank-Backend includes separate services for:
- Authentication
- Account management
- Transactions
- Notifications
- Service discovery
- API gateway routing
- Centralized configuration

**Tech stack:** PostgreSQL, RabbitMQ, Liquibase, Eureka, Spring Cloud Config, JWT/OAuth security

---

## Repository Structure

- **auth-service** — authentication and user-related logic
- **account-service** — account management
- **transaction-service** — transaction handling
- **transaction-orchestrator** — transaction coordination/orchestration
- **notification-service** — notifications
- **gateway** — API gateway
- **eureka** — service discovery
- **config-server** — centralized config
- **common** — shared code
 



### 1. Install Requirements
- Java 17
- Maven
- Docker
- Docker Compose
 

```

### 3. Start Infrastructure with Docker Compose
The repository contains a `docker-compose.yaml` that starts:
- 3 PostgreSQL databases
- RabbitMQ
 
This starts:
- PostgreSQL for auth/user DB on port **2345**
- PostgreSQL for account DB on port **3214**
- PostgreSQL for transaction DB on port **4123**
- RabbitMQ on ports **5672** and **15672**

### 4. Start the Spring Services
You will likely need to run the microservices separately from their own module directories.

**Recommended startup order:**
1. config-server
2. eureka
3. auth-service
4. account-service
5. transaction-service
6. gateway
7. (optional) notification-service, transaction-orchestrator

**Example:**

To start each service, open a new terminal for each and run:

```bash
cd <service-directory>
mvn spring-boot:run
```

For example, to start the config server:
```bash
cd config-server
mvn spring-boot:run
```
Then, in a new terminal, start Eureka:
```bash
cd eureka
mvn spring-boot:run
```
Repeat for each service in the recommended order.

---

## Ports
| Service                | Port   |
|------------------------|--------|
| auth-service           | 8080   |
| transaction-service    | 8081   |
| account-service        | 8082   |
| gateway                | 9090   |
| config-server          | 8012   |
| eureka                 | 8761   |
| Keycloak/JWK endpoint  | 1111   |

---

## Important Notes
- The project references additional services that may need to exist for full functionality:
	- Config Server: [http://localhost:8012](http://localhost:8012)
	- Eureka Server: [http://localhost:8761/eureka/](http://localhost:8761/eureka/)
	- Keycloak or another identity provider: [http://localhost:1111](http://localhost:1111)
- Docker Compose alone is **not enough**. It only starts databases and RabbitMQ.
- You still need to run the Spring services manually.
- Some features may require a working Keycloak setup.
