# Filmplace

Filmplace is a marketplace for instant cameras, film, and accessories. Users can create storefronts, list products, browse and filter active listings, add items to a cart, and manage orders.

## Explore the marketplace
<img width="320" height="240" alt="1" src="https://github.com/user-attachments/assets/870fc22c-ec17-4e0e-87fd-2b3b257c2357" /><br>

## Inspect products and add them to your cart
<img width="320" height="240" alt="2" src="https://github.com/user-attachments/assets/2695104f-e4f9-4512-a02e-f964e938d419" />


## Check your cart
<img width="320" height="240" alt="3" src="https://github.com/user-attachments/assets/de0c3761-2fd8-4491-8e26-3714e54decbc" />


## Add a storefront of your own - start selling
<img width="320" height="240" alt="4" src="https://github.com/user-attachments/assets/d6f4a4cf-7897-40a6-805d-ed3d0afaa4a5" />


## Add a listing to your storefront
<img width="320" height="240" alt="5" src="https://github.com/user-attachments/assets/7d4913c1-c955-4078-9781-586fdedb4c58" />




## Repository structure

```text
frontend/       Angular application and SSR server
backend-java/   Spring Boot API, security, persistence, and image processing
postgresql/     Database schema and development seed data
compose.yaml    Docker Compose configuration
```

## Run locally

### Requirements

- Docker Desktop, or Docker Engine with Docker Compose

### Start

Create your local environment file and replace the two placeholder secrets:

```bash
cp .env.example .env
```

Generate a JWT signing secret with `openssl rand -base64 32` or another
cryptographically secure generator. The value must contain at least 32 characters.
The `.env` file is ignored by Git and must never be committed.

Then start the application:

```bash
docker compose up --build
```

Open [http://localhost:4200](http://localhost:4200). The API health endpoint is [http://localhost:8080/api/health](http://localhost:8080/api/health).

The database is populated with local development data on first start.

### Demo accounts

| Role | Email | Password |
| --- | --- | --- |
| Administrator | `admin@mail.com` | `adminpw123` |
| Seller / buyer | `jan@mail.com` | `userpw123` |
| Seller / buyer | `john@mail.com` | `userpw123` |

### Stop

```bash
docker compose down
```

To reset the seeded database:
```bash
docker compose down -v
```

## Features

- Search and filters for cameras, film, and accessories
- User registration, login, and JWT-protected routes
- Seller storefronts with configurable themes
- Catalog-backed product listings with image uploads
- Product gallery, responsive image variants, and related listings
- Cart quantity management and checkout
- Checkout split by storefront
- Transactional inventory reservation to prevent overselling
- Buyer and seller order views with status updates
- Admin catalog management and sales statistics


## SEO
- Server-side rendered public pages
- Route metadata
- robots.txt
- dynamic sitemap generation
- OpenGraph tags
- Canonical URLs
- JSON-LD structured data (including breadcrumbList)

## Tech stack

| Area | Tools |
| --- | --- |
| Frontend | Angular 21, TypeScript, SCSS, Angular SSR |
| Backend | Java 25, Spring Boot 4.1, Spring Security, Spring JDBC |
| Database | PostgreSQL 16 |
| Authentication | Stateless JWT, BCrypt |
| Images | Multipart uploads, responsive image variants |
| Environment | Docker, Docker Compose |

## Architecture

```text
Angular SSR frontend (:4200)
Spring Boot API (:8080)
PostgreSQL (:5432)
```

The API handles authentication, storefronts, listings, product images, cart state, orders, admin catalog operations, and sitemap data. The frontend renders public pages on the server and marks account and management routes as non-indexable.

Payment processing and shipping-provider integration are not included. Checkout creates orders and reserves inventory only.

## Configuration

All deploy-specific values are read from environment variables. See
[`.env.example`](.env.example) for the complete list. `POSTGRES_PASSWORD` and
`JWT_SECRET` are required; Docker Compose stops with a clear error when either is
missing. `PUBLIC_API_URL` is served to the browser at runtime, so the same frontend
image can be promoted between environments without rebuilding it.

For production, use secret storage provided by your hosting platform, set
`CORS_ORIGIN`, `PUBLIC_API_URL`, and `SITE_URL` to the public HTTPS URLs, and do not
publish PostgreSQL or the backend port unless they need to be directly reachable.
