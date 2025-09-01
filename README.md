GlobalBooks SOA System

A Service-Oriented Architecture (SOA) implementation showcasing hybrid integration of SOAP and REST services with enterprise-level messaging.

Overview

The system provides a book ordering platform built on SOA principles. It integrates SOAP and REST services, orchestrated with messaging and business processes.

Services

Catalog Service (SOAP) – Port 8080

Tech: Java Spring Boot with JAX-WS

Protocol: SOAP (with WS-Security support)

Role: Manage book catalog, pricing, and availability

Orders Service (REST) – Port 8081

Tech: Java Spring Boot

Protocol: REST (secured with OAuth2)

Role: Handle order lifecycle and orchestration

Payments Service (REST) – Port 8082

Tech: Java Spring Boot

Protocol: REST microservice

Role: Manage payment processing

Shipping Service (REST) – Port 8083

Tech: Java Spring Boot

Protocol: REST microservice

Role: Shipment and delivery management

Integration

RabbitMQ – Used as an Enterprise Service Bus (ESB) for asynchronous communication

BPEL – For orchestration of order fulfillment workflows

Security – SOAP secured with WS-Security; REST services with OAuth2

Requirements

Java 11+

Maven 3.6+

Docker & Docker Compose

Quick Setup

Clone repository:

git clone <repository-url>
cd SOA


Deploy services with Docker:

cd 10-deployment/docker
docker-compose up -d --build


Validate deployment:

Catalog (SOAP): http://localhost:8080/soap/catalog?wsdl

Orders: http://localhost:8081/api/v1/orders/health

Payments: http://localhost:8082/api/v1/payments/health

Shipping: http://localhost:8083/api/v1/shippings/health

RabbitMQ: http://localhost:15672
 (admin / password123)

Service APIs
Orders Service
POST   /api/v1/orders                  # Create order
GET    /api/v1/orders                  # List all orders
GET    /api/v1/orders/{id}             # Get order by ID
PUT    /api/v1/orders/{id}/status      # Update status
DELETE /api/v1/orders/{id}             # Cancel order
GET    /api/v1/orders/health           # Health check

Payments Service
POST   /api/v1/payments/initiate       # Initiate payment
POST   /api/v1/payments/{id}/process   # Process payment
GET    /api/v1/payments/{id}           # Payment details
GET    /api/v1/payments/order/{orderId}# Payments for order
GET    /api/v1/payments/health         # Health check

Shipping Service
POST   /api/v1/shippings               # Create shipping
POST   /api/v1/shippings/{id}/process  # Process shipping
GET    /api/v1/shippings/{id}          # Shipping details
GET    /api/v1/shippings/tracking/{num}# Track shipment
GET    /api/v1/shippings/health        # Health check

Catalog Service (SOAP)

WSDL: http://localhost:8080/soap/catalog?wsdl

Operations:

searchBooks – Search books by keyword/category

getBookById – Retrieve book details

getBookPrice – Get book price

checkAvailability – Verify stock availability

Message Flow

Client creates order via Orders Service

Orders Service publishes events to RabbitMQ

Payments Service processes payment events

Shipping Service handles shipping events

BPEL orchestrates the workflow across services

Security

Current:

SOAP → Basic Auth (extendable to WS-Security)

REST → OAuth2 with JWT tokens

Keycloak for identity management

Planned:

WS-Security with UsernameToken for SOAP

Full OAuth2 integration for REST

Mutual TLS between services

Testing

Postman collections in 09-testing/postman

SOAP UI test suite in 09-testing/soap-ui

Development & Running

Build and run each service:

cd 02-catalog-service
mvn clean package spring-boot:run

cd 03-orders-service
mvn clean package spring-boot:run


Run via JAR:

java -jar target/*.jar

Monitoring

Health checks on all REST services

RabbitMQ management UI for messaging insights

Spring Boot Actuator for metrics

Deployment Options

Docker:

cd 10-deployment/docker
docker-compose up -d --build


Kubernetes: manifests available in 10-deployment/kubernetes/

Architecture Patterns

Service-Oriented Architecture (SOA)

Enterprise Service Bus (ESB)

Event-Driven Architecture

Microservices

API Gateway Pattern

Circuit Breaker Pattern

Saga Pattern (via BPEL orchestration)

Tech Stack

Java 11 with Spring Boot

Spring Web Services for SOAP

RabbitMQ for messaging

Docker for containerization

OAuth2 & WS-Security for security

BPEL for orchestration

H2 Database for development

Project Structure
├── 01-design-artifacts/       # Architecture docs
├── 02-catalog-service/        # SOAP service
├── 03-orders-service/         # REST service
├── 04-payments-service/       # REST service
├── 05-shipping-service/       # REST service
├── 06-bpel-orchestration/     # BPEL definitions
├── 07-integration/            # Integration components
├── 08-security/               # Security configs
├── 09-testing/                # Tests
├── 10-deployment/             # Deployment setups
└── 11-documentation/          # API docs

Contributing

Fork repo & create a feature branch

Implement changes with tests

Submit pull request

License

Licensed under MIT. See LICENSE file for details.