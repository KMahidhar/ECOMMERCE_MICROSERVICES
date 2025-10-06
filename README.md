
# 🧩 Microservices Architecture Documentation

This project is built using Spring Boot and follows a microservices design. It includes an API Gateway, User Service, Product Service, and Order Service. All services are registered with Eureka for dynamic service discovery and secured using Basic Authentication. The API Gateway handles routing and integrates circuit breakers for fault tolerance.

---

## 📚 Table of Contents

- [Introduction](#introduction)  
- [Architecture Overview](#architecture-overview)  
- [Module Breakdown](#module-breakdown)  
- [System Requirements](#system-requirements)  
- [Installation Guide](#installation-guide)  
- [Launching the Services](#launching-the-services)  
- [REST API Specifications](#rest-api-specifications)  
  - [Gateway Routes](#gateway-routes)  
  - [User Service APIs](#user-service-apis)  
  - [Product Service APIs](#product-service-apis)  
  - [Order Service APIs](#order-service-apis)  
- [Security & Authentication](#security--authentication)  
- [Postman Testing Guide](#postman-testing-guide)  
  - [Environment Setup](#environment-setup)  
  - [Sample API Calls](#sample-api-calls)  
- [Common Issues & Fixes](#common-issues--fixes)  
- [H2 Database Access](#h2-database-access)  
- [Contribution Guidelines](#contribution-guidelines)  
- [License Info](#license-info)

---

## 🔍 Introduction

This microservices ecosystem includes:

- **API Gateway**: Centralized entry point for all requests. Handles routing, authentication, and resilience via circuit breakers.  
- **User Service**: Handles user-related operations (Create, Read, Update, Delete).  
- **Product Service**: Manages product inventory and details.  
- **Order Service**: Facilitates order placement and links users with products.  
- **Eureka Server**: Enables service registration and discovery.

---

## 🏗️ Architecture Overview

- `com.microservice.apigateway`: Gateway service  
- `com.microservice.user_service`: User management  
- `com.microservice.product_service`: Product catalog  
- `com.microservice.order_service`: Order processing  
- Configuration files: `application.properties` in `src/main/resources` of each module

---

## ⚙️ System Requirements

- Java JDK 17 or newer  
- Maven (for building the project)  
- Postman (for API testing)  
- Eureka Server (accessible at http://localhost:8761)  
- Recommended IDE: IntelliJ IDEA or Eclipse

---

## 🚀 Installation Guide

1. **Clone the Repository**  
   Download the project or clone it using Git.

2. **Configure Properties**  
   Ensure each service has its `application.properties` file in the correct location.  
   Update the Eureka URL if hosted elsewhere:  
   `eureka.client.service-url.defaultZone=http://localhost:8761/eureka`

3. **Install Dependencies**  
   Run `mvn clean install` in the root or individual service directories.

---

## ▶️ Launching the Services

1. **Start Eureka Server**  
   Run the Eureka application on port `8761`.

2. **Start Microservices**  
   - API Gateway → Port `8081`  
   - User Service → Port `8082`  
   - Product Service → Port `9090`  
   - Order Service → Port `9091`

3. **Verify Service Registration**  
   Visit `http://localhost:8761` to confirm all services are listed and marked as "UP".

---

## 🌐 REST API Specifications

All endpoints are accessed via the API Gateway (`http://localhost:8080`) and require Basic Authentication.

### 🔐 Gateway Routes

| Method | Endpoint         | Description                          | Role Required |
|--------|------------------|--------------------------------------|---------------|
| GET    | /admin/test      | Returns "Admin access granted"       | ADMIN         |
| GET    | /customer/test   | Returns "Customer access granted"    | CUSTOMER      |
| GET    | /fallback        | Returns fallback message             | Any           |

### 👤 User Service APIs

| Method | Endpoint         | Description               |
|--------|------------------|---------------------------|
| GET    | /users           | List all users            |
| GET    | /users/{id}      | Get user by ID            |
| POST   | /users           | Create a new user         |
| PUT    | /users/{id}      | Update user by ID         |
| DELETE | /users/{id}      | Delete user by ID         |

### 📦 Product Service APIs

| Method | Endpoint         | Description               |
|--------|------------------|---------------------------|
| GET    | /products        | List all products         |
| GET    | /products/{id}   | Get product by ID         |
| POST   | /products        | Add a new product         |
| PUT    | /products/{id}   | Modify product details    |
| DELETE | /products/{id}   | Delete a product          |

### 🛒 Order Service APIs

| Method | Endpoint         | Description               |
|--------|------------------|---------------------------|
| GET    | /orders          | List all orders           |
| GET    | /orders/{id}     | Get order by ID           |
| POST   | /orders          | Place a new order         |
| PUT    | /orders/{id}     | Update order details      |
| DELETE | /orders/{id}     | Cancel an order           |

---

## 🔑 Security & Authentication

- **Authentication Type**: Basic Auth  
- **User Credentials**:  
  - Admin → `admin:adminpass`  
  - Customer → `user:userpass`  
- **Defined In**: `SecurityConfig` of API Gateway using `MapReactiveUserDetailsService`

---

## 🧪 Postman Testing Guide

### 🛠️ Environment Setup

1. Install Postman from [postman.com](https://www.postman.com)  
2. Create a collection named **Microservices API Tests**  
3. Set up an environment (e.g., "Local") with variables:  
   - `base_url = http://localhost:8080`  
   - `admin_username = admin`  
   - `admin_password = adminpass`  
   - `user_id`, `product_id` → Set dynamically via test scripts

### 📬 Sample API Call: Create User

- **URL**: `{{base_url}}/users`  
- **Method**: POST  
- **Auth**: Basic Auth (`{{admin_username}}` / `{{admin_password}}`)  
- **Headers**: `Content-Type: application/json`  
- **Body**:
```json
{
  "username": "testuser",
  "email": "test@example.com",
  "password": "testpass",
  "role": "CUSTOMER"
}
