# Distributed Backend System

A Java/Spring Boot microservices demo for an e-commerce flow. The project shows REST communication, JWT authentication, PostgreSQL persistence, RabbitMQ notifications, Docker-based local infrastructure, validation, centralized exception handling, and automated tests.

## Tech Stack

- Java 21, Spring Boot
- Spring Web, Spring Security, Spring Data JPA, Bean Validation
- PostgreSQL with Flyway migrations
- RabbitMQ for order notification events
- Docker Compose
- JUnit, Mockito, MockMvc, H2 test database

## Services

| Service | Port | Responsibility | Repository |
| --- | ---: | --- | --- |
| `auth-service` | 8083 | User registration, login, JWT/refresh token generation | https://github.com/denystrypolskyi/auth-service |
| `product-service` | 8082 | Product catalog and stock updates | https://github.com/denystrypolskyi/product-service |
| `order-service` | 8084 | Order creation, stock reservation, RabbitMQ notifications | https://github.com/denystrypolskyi/order-service |
| `notification-service` | 8081 | Consumes order notification messages and sends email | https://github.com/denystrypolskyi/notification-service |

## Architecture Notes

- API errors are normalized through `@RestControllerAdvice`.
- Protected endpoints use JWT validation through Spring Security filters.
- API controllers return DTOs instead of exposing JPA entities directly.
- Product stock decrement is transactional and uses optimistic locking.
- RabbitMQ notification failures are rejected to a dead-letter queue.
- Database schema is managed with Flyway migrations.
- Tests run without local Docker infrastructure by using H2 and disabled broker listeners.

## Local Setup

Create a `.env` file from `.env.example` and set mail credentials if you want notification emails to be sent.

```env
MAIL_USERNAME=your@email.com
MAIL_PASSWORD=your-mail-app-password
POSTGRES_PASSWORD=mysecurepass123
JWT_SECRET=replace-with-at-least-32-byte-secret
RABBITMQ_USERNAME=guest
RABBITMQ_PASSWORD=guest
```

Start the full stack:

```powershell
docker compose up --build
```

RabbitMQ management UI is available at `http://localhost:15672`.

## Main API Flow

1. Register: `POST http://localhost:8083/auth/register`
2. Login: `POST http://localhost:8083/auth/login`
3. Use the returned access token as `Authorization: Bearer <token>`
4. Create products through `product-service`
5. Create orders through `order-service`
6. Order creation decrements product stock and sends a RabbitMQ notification

## Run Tests

```powershell
cd auth-service; .\mvnw.cmd test
cd ..\product-service; .\mvnw.cmd test
cd ..\order-service; .\mvnw.cmd test
cd ..\notification-service; .\mvnw.cmd test
```
