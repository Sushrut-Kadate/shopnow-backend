# 🛒 ShopNow — E-Commerce Backend

> A production-ready, full-featured e-commerce REST API built with Spring Boot, featuring JWT authentication, role-based access control, PostgreSQL database, Stripe payment integration, and Swagger API documentation.

---

## 📌 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Architecture & Flow](#architecture--flow)
- [API Endpoints](#api-endpoints)
- [Getting Started](#getting-started)
- [Environment Configuration](#environment-configuration)
- [Authentication Flow](#authentication-flow)
- [Database Schema](#database-schema)
- [API Documentation](#api-documentation)

---

## 📖 Overview

ShopNow Backend is a RESTful API service that powers the ShopNow e-commerce platform. It handles user authentication, product management, cart operations, order processing, and payment integration. The backend follows a clean layered architecture with proper separation of concerns.

**Live API Docs:** `http://localhost:8080/swagger-ui/index.html`  
**Base URL:** `http://localhost:8080`

---

## 🛠 Tech Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 21 | Programming Language |
| Spring Boot | 3.5.3 | Backend Framework |
| Spring Security | 6.x | Authentication & Authorization |
| Spring Data JPA | 3.x | Database ORM |
| Hibernate | 7.x | ORM Implementation |
| PostgreSQL | 18 | Relational Database |
| JWT (jjwt) | 0.12.5 | Token Based Authentication |
| Stripe Java SDK | 29.3.0 | Payment Processing |
| SpringDoc OpenAPI | 2.8.9 | API Documentation (Swagger) |
| ModelMapper | 3.0.0 | DTO Mapping |
| Lombok | Latest | Boilerplate Reduction |
| Maven | 3.x | Build Tool |

---

## ✨ Features

- **JWT Authentication** — Secure cookie-based JWT token authentication
- **Role Based Access Control** — Three roles: ADMIN, SELLER, USER
- **Product Management** — Full CRUD operations for products with image upload
- **Category Management** — Organize products into categories
- **Cart Management** — Add, update, remove items from cart
- **Order Processing** — Complete order workflow with status tracking
- **Payment Integration** — Stripe payment gateway integration
- **Address Management** — Multiple delivery addresses per user
- **Swagger Documentation** — Complete interactive API documentation
- **CORS Support** — Configured for frontend integration

---

## 📁 Project Structure

```
shopnow-backend/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/ecommerce/project/
│   │   │       │
│   │   │       ├── config/                         # Configuration Classes
│   │   │       │   ├── AppConfig.java              # App beans (ModelMapper etc)
│   │   │       │   ├── SwaggerConfig.java          # Swagger/OpenAPI config
│   │   │       │   └── WebSecurityConfig.java      # Spring Security config
│   │   │       │
│   │   │       ├── controller/                     # REST Controllers
│   │   │       │   ├── AuthController.java         # Login, Signup, Logout
│   │   │       │   ├── ProductController.java      # Product APIs
│   │   │       │   ├── CategoryController.java     # Category APIs
│   │   │       │   ├── CartController.java         # Cart APIs
│   │   │       │   ├── OrderController.java        # Order APIs
│   │   │       │   └── AddressController.java      # Address APIs
│   │   │       │
│   │   │       ├── model/                          # JPA Entity Classes
│   │   │       │   ├── User.java                   # User entity
│   │   │       │   ├── Product.java                # Product entity
│   │   │       │   ├── Category.java               # Category entity
│   │   │       │   ├── Cart.java                   # Cart entity
│   │   │       │   ├── CartItem.java               # Cart item entity
│   │   │       │   ├── Order.java                  # Order entity
│   │   │       │   ├── OrderItem.java              # Order item entity
│   │   │       │   ├── Address.java                # Address entity
│   │   │       │   ├── Role.java                   # Role entity
│   │   │       │   └── Payment.java                # Payment entity
│   │   │       │
│   │   │       ├── repository/                     # JPA Repositories
│   │   │       │   ├── UserRepository.java
│   │   │       │   ├── ProductRepository.java
│   │   │       │   ├── CategoryRepository.java
│   │   │       │   ├── CartRepository.java
│   │   │       │   ├── OrderRepository.java
│   │   │       │   └── AddressRepository.java
│   │   │       │
│   │   │       ├── service/                        # Business Logic
│   │   │       │   ├── ProductService.java
│   │   │       │   ├── CategoryService.java
│   │   │       │   ├── CartService.java
│   │   │       │   ├── OrderService.java
│   │   │       │   └── AddressService.java
│   │   │       │
│   │   │       ├── payload/                        # DTOs (Request/Response)
│   │   │       │   ├── ProductDTO.java
│   │   │       │   ├── CategoryDTO.java
│   │   │       │   ├── CartDTO.java
│   │   │       │   ├── OrderDTO.java
│   │   │       │   └── APIResponse.java
│   │   │       │
│   │   │       ├── security/                       # Security Layer
│   │   │       │   ├── jwt/
│   │   │       │   │   ├── JwtUtils.java           # JWT token utility
│   │   │       │   │   ├── AuthTokenFilter.java    # JWT filter
│   │   │       │   │   └── AuthEntryPointJwt.java  # Unauthorized handler
│   │   │       │   └── services/
│   │   │       │       ├── UserDetailsImpl.java
│   │   │       │       └── UserDetailsServiceImpl.java
│   │   │       │
│   │   │       └── SbEcomApplication.java          # Main Application Class
│   │   │
│   │   └── resources/
│   │       ├── application.properties              # App configuration
│   │       └── images/                             # Product images storage
│   │
│   └── test/
│       └── java/
│           └── com/ecommerce/project/
│               └── SbEcomApplicationTests.java
│
├── .gitignore
├── pom.xml                                         # Maven dependencies
└── README.md
```

---

## 🏗 Architecture & Flow

```
┌─────────────────────────────────────────────────────────┐
│                    CLIENT (React Frontend)               │
│                    localhost:5173                        │
└──────────────────────────┬──────────────────────────────┘
                           │ HTTP Requests
                           ▼
┌─────────────────────────────────────────────────────────┐
│                  SPRING BOOT BACKEND                     │
│                    localhost:8080                        │
│                                                         │
│  ┌─────────────┐    ┌──────────────┐    ┌───────────┐  │
│  │  Controller │───▶│   Service    │───▶│Repository │  │
│  │   Layer     │    │    Layer     │    │   Layer   │  │
│  └─────────────┘    └──────────────┘    └─────┬─────┘  │
│         │                                      │        │
│  ┌─────────────┐                        ┌─────▼─────┐  │
│  │  Security   │                        │PostgreSQL │  │
│  │JWT + Spring │                        │ Database  │  │
│  │  Security   │                        │port: 5432 │  │
│  └─────────────┘                        └───────────┘  │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
              ┌────────────────────┐
              │   Stripe Payment   │
              │   Gateway (Cloud)  │
              └────────────────────┘
```

### Request Flow:
1. Client sends HTTP request to REST API
2. `AuthTokenFilter` intercepts and validates JWT token
3. If valid, request reaches Controller
4. Controller calls Service layer for business logic
5. Service calls Repository for database operations
6. Repository communicates with PostgreSQL via Hibernate JPA
7. Response flows back to client

---

## 🔗 API Endpoints

### 🔐 Authentication APIs (Public)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/signup` | Register new user |
| POST | `/api/auth/signin` | Login user |
| POST | `/api/auth/signout` | Logout user |

### 📦 Product APIs
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | `/api/public/products` | Public | Get all products |
| GET | `/api/public/products/{id}` | Public | Get product by ID |
| GET | `/api/public/categories/{id}/products` | Public | Get products by category |
| POST | `/api/seller/products/category/{categoryId}` | Seller | Add new product |
| PUT | `/api/seller/products/{productId}` | Seller | Update product |
| DELETE | `/api/seller/products/{productId}` | Seller | Delete product |
| PUT | `/api/seller/products/{productId}/image` | Seller | Update product image |

### 🗂 Category APIs
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | `/api/public/categories` | Public | Get all categories |
| POST | `/api/admin/categories` | Admin | Add new category |
| PUT | `/api/admin/categories/{id}` | Admin | Update category |
| DELETE | `/api/admin/categories/{id}` | Admin | Delete category |

### 🛒 Cart APIs
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | `/api/carts/users/cart` | User | Get user cart |
| POST | `/api/carts/products/{productId}/quantity/{qty}` | User | Add to cart |
| PUT | `/api/cart/products/{productId}/quantity/{op}` | User | Update cart item |
| DELETE | `/api/carts/{cartId}/product/{productId}` | User | Remove from cart |

### 📋 Order APIs
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/order/users/payments/{paymentMethod}` | User | Place order |
| GET | `/api/admin/orders` | Admin | Get all orders |
| PUT | `/api/admin/orders/{id}/orderStatus/{status}` | Admin | Update order status |

### 📍 Address APIs
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | `/api/addresses` | User | Get all addresses |
| POST | `/api/addresses` | User | Add new address |
| PUT | `/api/addresses/{id}` | User | Update address |
| DELETE | `/api/addresses/{id}` | User | Delete address |

---

## 🚀 Getting Started

### Prerequisites
- Java 21
- Maven 3.x
- PostgreSQL 18
- IntelliJ IDEA (recommended)

### Step 1 — Clone the Repository
```bash
git clone https://github.com/Sushrut-Kadate/shopnow-backend.git
cd shopnow-backend
```

### Step 2 — Setup PostgreSQL Database
```sql
CREATE DATABASE ecommerce;
```

### Step 3 — Configure application.properties
```properties
spring.application.name=sb-ecom

spring.datasource.url=jdbc:postgresql://localhost:5432/ecommerce
spring.datasource.username=YOUR_POSTGRES_USERNAME
spring.datasource.password=YOUR_POSTGRES_PASSWORD

spring.jpa.hibernate.ddl-auto=update
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect

project.image=images/

spring.app.jwtSecret=YOUR_JWT_SECRET_KEY
spring.app.jwtExpirationMs=300000000
spring.ecom.app.jwtCookieName=springBootEcom

frontend.url=http://localhost:5173/
image.base.url=http://localhost:8080/images

stripe.secret.key=YOUR_STRIPE_SECRET_KEY
```

### Step 4 — Run the Application
Open `SbEcomApplication.java` in IntelliJ and click **Run**

OR via Maven:
```bash
./mvnw spring-boot:run
```

### Step 5 — Verify
Backend running at: **http://localhost:8080**
Swagger UI at: **http://localhost:8080/swagger-ui/index.html**

---

## 🔐 Authentication Flow

```
User                    Backend                    Database
 │                         │                          │
 │──── POST /signin ───────▶│                          │
 │                         │──── Find User ───────────▶│
 │                         │◀─── User Found ───────────│
 │                         │                          │
 │                         │── Generate JWT Token ──▶  │
 │                         │                          │
 │◀─── JWT in Cookie ──────│                          │
 │                         │                          │
 │──── API Request ─────────▶│                          │
 │      + JWT Cookie        │                          │
 │                         │── Validate Token ──────▶  │
 │                         │── Check Role ──────────▶  │
 │◀─── Response ───────────│                          │
```

### Roles & Permissions:
| Role | Access Level |
|------|-------------|
| `ROLE_USER` | Browse products, manage cart, place orders |
| `ROLE_SELLER` | Add/update/delete their own products |
| `ROLE_ADMIN` | Full access — manage categories, all orders, all users |

---

## 🗄 Database Schema

```
users                    products
├── user_id (PK)         ├── product_id (PK)
├── username             ├── product_name
├── email                ├── description
├── password             ├── price
└── roles                ├── discount
                         ├── special_price
categories               ├── quantity
├── category_id (PK)     ├── image
└── category_name        ├── category_id (FK)
                         └── seller_id (FK)

carts                    orders
├── cart_id (PK)         ├── order_id (PK)
├── user_id (FK)         ├── user_id (FK)
└── total_price          ├── order_date
                         ├── total_amount
cart_items               ├── order_status
├── cart_item_id (PK)    └── payment_id (FK)
├── cart_id (FK)
├── product_id (FK)      payments
├── quantity             ├── payment_id (PK)
└── price                ├── payment_method
                         └── stripe_payment_intent
addresses
├── address_id (PK)
├── user_id (FK)
├── street
├── city
├── state
├── country
└── pincode
```

---

## 📚 API Documentation

Swagger UI is integrated for complete interactive API documentation.

**Access:** `http://localhost:8080/swagger-ui/index.html`

To test protected APIs via Postman:
1. Login via `POST /api/auth/signin`
2. Copy JWT token from response
3. Add header: `Authorization: Bearer <token>`

---

## 👨‍💻 Author

**Sushrut Kadate**
- GitHub: [@Sushrut-Kadate](https://github.com/Sushrut-Kadate)
