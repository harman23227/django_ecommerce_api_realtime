# Django eCommerce API with Redis JWT Celery Smart Search

A scalable, modular, and performant eCommerce REST API built with Django and Django REST Framework. Features include JWT authentication, Stripe integration, Redis caching, Celery tasks, real-time chat with Django Channels, personalized search, tagging, and more.

---

## 🧭 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [API Documentation](#api-documentation)
- [Environment Variables](#environment-variables)
- [Endpoints Overview](#endpoints-overview)
  - [User Management](#user-management)
  - [Authentication](#authentication)
  - [Products](#products)
  - [Categories](#categories)
  - [Cart](#cart)
  - [Orders](#orders)
  - [Coupons](#coupons)
  - [Payments](#payments)
  - [Chat](#chat)
- [Testing](#testing)


---

## ✅ Features

- 🔐 User registration, login/logout, and JWT auth  
- 🛍️ Product catalog, categories, tagging, search  
- 🧺 Cart and order management  
- 💳 Stripe payment integration  
- 🧾 Coupon and discount system  
- 📦 Order history and tracking  
- 🔄 Background task scheduling (Celery + Redis)  
- 📡 Real-time chat (Django Channels)  
- 📬 Email notifications  
- 📄 Auto-generated OpenAPI/Swagger docs  
- 🐳 Docker support (optional)

---

## 🧰 Tech Stack

- **Backend**: Django, Django REST Framework  
- **Auth**: JWT, Google OAuth2 (social-auth)  
- **Database**: PostgreSQL (configurable)  
- **Caching/Queue**: Redis  
- **Background Tasks**: Celery  
- **Payments**: Stripe API  
- **Docs**: drf-spectacular (OpenAPI/Swagger/Redoc)  
- **Containerization**: Docker & Docker Compose (optional)

---

## 📁 Project Structure

```
apps/
├── accounts/   → User auth, profiles
├── products/   → Products, categories, tags, search
├── orders/     → Order creation, history, cart
├── payments/   → Stripe integration & webhook
├── chat/       → Real-time chat via Channels
├── core/       → Shared utils, settings
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+  
- PostgreSQL  
- Redis  
- React.js (for frontend, optional)  
- `pipenv` or `pip`

### Installation

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/hypex-ecommerce-api.git
cd hypex-ecommerce-api

# 2. Create virtual env & install deps
pip install -r requirements.txt

# 3. Configure your environment variables
cp .env.example .env  # then edit with your secrets

# 4. Apply migrations
python manage.py migrate

# 5. Create superuser
python manage.py createsuperuser

# 6. Start Redis & Celery (in separate terminals)
redis-server
celery -A ecommerce_api worker -l info

# 7. Start development server
python manage.py runserver
```

---



## 🔐 Environment Variables

Set these in `.env`:

```env
SECRET_KEY=your_secret
DATABASE_URL=postgres://user:pass@localhost:5432/dbname
REDIS_HOST=localhost
REDIS_PORT=6379
EMAIL_HOST_USER=your@email.com
EMAIL_HOST_PASSWORD=password
DEFAULT_FROM_EMAIL=noreply@yourdomain.com
STRIPE_PUBLISHABLE_KEY=...
STRIPE_SECRET_KEY=...
STRIPE_WEBHOOK_SECRET=...
GOOGLE_OAUTH2_KEY=...
GOOGLE_OAUTH2_SECRET=...
DOMAIN=localhost
SITE_NAME=Hypex
```

---

## 🔀 Endpoints Overview

### 👤 User Management

| Method | Endpoint              | Description                  |
|--------|-----------------------|------------------------------|
| GET    | `/auth/me/`           | Get current user profile     |
| PUT    | `/auth/me/`           | Update user profile          |
| PATCH  | `/auth/me/`           | Partial update               |
| DELETE | `/auth/me/`           | Delete user                  |
| POST   | `/auth/reset-password/`        | Request password reset |
| POST   | `/auth/reset-password-confirm/`| Confirm reset token     |
| POST   | `/auth/set-password/`          | Set new password        |

### 🔐 Authentication

| Method | Endpoint               | Description            |
|--------|------------------------|------------------------|
| POST   | `/auth/register/`      | Register a new user    |
| POST   | `/auth/activate/`      | Activate account       |
| POST   | `/auth/token/create/`  | Login                  |
| POST   | `/auth/token/refresh/` | Refresh JWT token      |
| POST   | `/auth/token/verify/`  | Verify token           |
| POST   | `/auth/token/destroy/` | Logout                 |

### 🛒 Products

| Method | Endpoint                        | Description            |
|--------|----------------------------------|------------------------|
| GET    | `/api/v1/products/`             | List all products      |
| POST   | `/api/v1/products/`             | Create a new product   |
| GET    | `/api/v1/products/{slug}/`      | Retrieve product       |
| PUT    | `/api/v1/products/{slug}/`      | Update product         |
| PATCH  | `/api/v1/products/{slug}/`      | Partial update         |
| DELETE | `/api/v1/products/{slug}/`      | Delete product         |
| GET    | `/api/v1/products/user-products/` | User's own listings |

### 🧭 Categories

| Method | Endpoint                            |
|--------|-------------------------------------|
| GET    | `/api/v1/categories/`               |
| POST   | `/api/v1/categories/`               |
| GET    | `/api/v1/categories/{slug}/`        |
| PUT    | `/api/v1/categories/{slug}/`        |
| PATCH  | `/api/v1/categories/{slug}/`        |
| DELETE | `/api/v1/categories/{slug}/`        |

### 🧺 Cart

| Method | Endpoint                             |
|--------|--------------------------------------|
| GET    | `/api/v1/cart/`                      |
| POST   | `/api/v1/cart/{product_id}/add/`     |
| DELETE | `/api/v1/cart/{product_id}/remove/`  |

### 📦 Orders

| Method | Endpoint                          |
|--------|-----------------------------------|
| GET    | `/api/v1/orders/`                 |
| POST   | `/api/v1/orders/`                 |
| GET    | `/api/v1/orders/{order_id}/`      |
| PUT    | `/api/v1/orders/{order_id}/`      |
| PATCH  | `/api/v1/orders/{order_id}/`      |
| DELETE | `/api/v1/orders/{order_id}/`      |

### 🏷️ Coupons

| Method | Endpoint                                 |
|--------|------------------------------------------|
| GET    | `/api/v1/coupon/`                        |
| POST   | `/api/v1/coupon/`                        |
| GET    | `/api/v1/coupon/{id}/`                  |
| PUT    | `/api/v1/coupon/{id}/`                  |
| PATCH  | `/api/v1/coupon/{id}/`                  |
| DELETE | `/api/v1/coupon/{id}/`                  |
| POST   | `/api/v1/coupon/apply-coupon/`          |

### 💳 Payments

| Method | Endpoint                            |
|--------|-------------------------------------|
| POST   | `/payment/process/{order_id}/`      |
| GET    | `/payment/completed/`               |
| GET    | `/payment/canceled/`                |

### 💬 Chat

| Method | Endpoint                      |
|--------|-------------------------------|
| GET    | `/api/v1/chat/{product_id}/`  |

---

## 🧪 Running Tests

```bash
python manage.py test
```

