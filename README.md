Spring-Bank-Backend is a backend system for a banking application built with Spring Boot and a microservices architecture.
It includes separate services for authentication, account management, transactions, notifications, service discovery, API gateway routing, and centralized configuration.
The project uses PostgreSQL databases, RabbitMQ, Liquibase, Eureka, Spring Cloud Config, and JWT/OAuth-related security components.

From the repository structure, the main parts are:

auth-service — authentication and user-related logic
account-service — account management
transaction-service — transaction handling
transaction-orchestrator — transaction coordination/orchestration
notification-service — notifications
gateway — API gateway
eureka — service discovery
config-server — centralized config
common — shared code
How to run it
Based on the repo files, the safest local setup is:

1. Install requirements
You need:

Java 17
Maven
Docker
Docker Compose
2. Clone the repository
bash
git clone https://github.com/arturnowakowski2015/Spring-Bank-Backend.git
cd Spring-Bank-Backend
3. Start infrastructure with Docker Compose
The repository contains a docker-compose.yaml that starts:

3 PostgreSQL databases
RabbitMQ
Run:

bash
docker compose up -d
This starts:

PostgreSQL for auth/user DB on port 2345
PostgreSQL for account DB on port 3214
PostgreSQL for transaction DB on port 4123
RabbitMQ on ports 5672 and 15672
4. Start the Spring services
You will likely need to run the microservices separately from their own module directories.

A reasonable startup order is:

config-server
eureka
auth-service
account-service
transaction-service
gateway
other services like notification-service and transaction-orchestrator
Example:

bash
cd config-server
mvn spring-boot:run
Then in another terminal:

bash
cd eureka
mvn spring-boot:run
Then:

bash
cd auth-service
mvn spring-boot:run
And so on for the other services.

Ports found in the configuration
From the config files:

auth-service → 8080
transaction-service → 8081
account-service → 8082
gateway → 9090
config-server → expected at 8012
eureka → expected at 8761
Keycloak/JWK endpoint is referenced at 1111
Important note
The project also references additional services that may need to exist for full functionality:

Config Server at http://localhost:8012
Eureka Server at http://localhost:8761/eureka/
Keycloak or another identity provider at http://localhost:1111
So Docker Compose alone is not enough. It only starts databases and RabbitMQ.
You still need to run the Spring services manually, and some features may require a working Keycloak setup.