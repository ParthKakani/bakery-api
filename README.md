# 🎂 Sweet Crumbs Bakery API

A full **e-commerce REST API** for a bakery shop, built with Flask. It covers the
whole backend a small online store needs: user accounts with JWT authentication,
a product catalog with categories and tags, a shopping/order system, and customer
reviews — all documented with an interactive Swagger UI and packaged to run in Docker.

A lightweight shop front-end and an admin page are included so the API can be
demoed in the browser without any extra setup.

---

## Features

- **JWT authentication** — register, login, logout, with admin-only protected routes
- **Product catalog** — full CRUD, search (by name, price range, category, availability), and a "featured / bestseller" endpoint
- **Categories** — CRUD, plus listing all products in a category
- **Tags** — many-to-many tagging of products
- **Orders** — create orders, view order history, update status, and order statistics
- **Reviews** — customers can review products, with per-product rating stats
- **Image upload** — authenticated image upload for products
- **Interactive API docs** — Swagger UI generated automatically
- **Dockerized** — one command to build and run

---

## Tech Stack

| Layer            | Technology                          |
|------------------|-------------------------------------|
| Language         | Python 3.11                         |
| Framework        | Flask + Flask-Smorest               |
| ORM / Database   | Flask-SQLAlchemy (SQLite)           |
| Auth             | Flask-JWT-Extended                  |
| Validation       | Marshmallow schemas                 |
| Passwords        | Passlib (bcrypt)                    |
| API docs         | OpenAPI 3 / Swagger UI              |
| Container        | Docker + docker-compose             |

---

## Getting Started

### Run locally
```bash
pip install -r requirements.txt
python app.py
```

### Run with Docker
```bash
docker compose up --build
```

Once running, open:

| Page       | URL                                  |
|------------|--------------------------------------|
| Shop UI    | http://localhost:5000/shop           |
| Admin UI   | http://localhost:5000/admin          |
| Swagger UI | http://localhost:5000/swagger-ui     |

> **Config:** set `JWT_SECRET_KEY` and (optionally) `DATABASE_URL` as environment
> variables. Defaults are provided for local development only — set your own secret
> before deploying anywhere real.

---

## API Overview

The API is organised into six resource groups. Full request/response schemas are
available in the Swagger UI.

**Auth & Users** — `/register`, `/login`, `/logout`, `/users`, `/users/<id>`,
`/users/<id>/orders`, `/users/<id>/password`

**Products** — `/products`, `/products/<id>`, `/products/search`,
`/products/featured`, `/products/<id>/tags`, `/products/<id>/reviews`

**Categories** — `/categories`, `/categories/<id>`, `/categories/<id>/products`

**Tags** — `/tags`, `/tags/<id>`

**Orders** — `/orders`, `/orders/<id>`, `/orders/<id>/status`, `/orders/stats`

**Reviews** — `/products/<id>/reviews`, `/reviews/<id>`,
`/products/<id>/reviews/stats`

Public endpoints (browsing, search) require no token. Creating orders/reviews
requires a valid JWT; administrative actions require an admin account.

### Example: search products
```
GET /products/search?q=chocolate&min_price=5&max_price=50&available=true
```

---

## Data Model

```
Category ──< Product >── Tag        (product <-> tag: many-to-many)
                 |
User ──< Order ──< OrderItem >── Product
  |
  └──< Review >── Product
```

- Category -> Products (one-to-many)
- User -> Orders, User -> Reviews (one-to-many)
- Order -> OrderItems (one-to-many)
- Product <-> Tags, Order <-> Products (many-to-many)

---

## Project Structure

```
bakery-api/
├── app.py              # App factory, config, UI + upload routes
├── db.py               # SQLAlchemy instance
├── schemas.py          # Marshmallow schemas (validation/serialization)
├── utils.py            # Helpers (e.g. admin_required)
├── models/             # SQLAlchemy models
├── resources/          # API endpoints (blueprints) per resource
├── static/             # Shop + admin HTML, default images
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

---

## Author

Built solo by **Parth Kakani** as a backend engineering project — designing a
realistic e-commerce REST API from scratch: resource modelling, table relationships,
JWT authentication, input validation, API documentation, and Docker packaging.
The included shop/admin pages are simple demos for exercising the API.