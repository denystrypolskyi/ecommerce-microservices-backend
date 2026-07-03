# E-commerce Microservices Backend

A Spring Boot microservices backend that simulates an e-commerce order processing system.

The system allows users to register, manage products, place orders, and receive email notifications with order details such as purchased items and total cost.


## 🚀 Features

- Multiple independent microservices with separate PostgreSQL databases  
- Inter-service communication using REST API  
- Asynchronous event-based communication with RabbitMQ (Order → Notification)  
- Centralized logging with Kafka and a dedicated `log-consumer` service  
- JWT-based user authentication  
- Unit and integration testing with JUnit and Mockito  
- Dockerized setup for easy local development


## 🧩 Microservices Overview

| Service | Description | Communication | Repository |
|----------|--------------|----------------|-------------|
| **Auth Service** | Authentication and authorization (user registration, login, JWT). | REST API | [auth-service](https://github.com/denystrypolskyi/auth-service) |
| **Product Service** | Product management (create, update, list products). | REST API | [product-service](https://github.com/denystrypolskyi/product-service) |
| **Order Service** | Order creation and management. Sends events to RabbitMQ and logs to Kafka. | REST API + RabbitMQ + Kafka | [order-service](https://github.com/denystrypolskyi/order-service) |
| **Notification Service** | Listens to RabbitMQ messages and sends order confirmation emails. | RabbitMQ | [notification-service](https://github.com/denystrypolskyi/notification-service) |
| **Log Consumer Service** | Consumes Kafka topics for centralized logging. | Kafka | [log-consumer-service](https://github.com/denystrypolskyi/log-consumer-service) |


## 📦 Tech Stack

- Java
- Spring Boot
- Docker
- PostgreSQL
- Kafka
- RabbitMQ
- REST API
- JUnit + Mockito
