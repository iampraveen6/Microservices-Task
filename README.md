# Copilot Instructions for Microservices-Task

## Overview
This repository implements a simple Node.js-based microservices architecture, containerized with Docker and orchestrated with Docker Compose and Kubernetes. The system consists of four main services:

- **User Service** (`/user-service`, port 3000)
- **Product Service** (`/product-service`, port 3001)
- **Order Service** (`/order-service`, port 3002)
- **Gateway Service** (`/gateway-service`, port 3003)

Each service is independently deployable and communicates via HTTP. The Gateway Service acts as an API aggregator and proxy for the other services.

## Key Patterns & Conventions
- **Service Structure:** Each service has its own directory with `app.js`, `package.json`, and a `dockerfile`.
- **Health Endpoints:** All services expose `/health` for liveness/readiness checks. Main resource endpoints are `/users`, `/products`, `/orders`.
- **Gateway Routing:** The Gateway Service exposes `/api/users`, `/api/products`, `/api/orders` and proxies requests to the respective services using service names as hostnames (e.g., `http://user-service:3000/users`).
- **Docker Compose:** Use `docker-compose.yml` at the root to build and run all services locally. Healthchecks are defined for each service.
- **Kubernetes:** Deployment and service YAMLs are in `deployment/` and `service/`. Ingress rules are in `ingress/ingress.yml`.
- **Resource Limits:** Kubernetes deployments set CPU/memory requests and limits for each container.
- **Environment:** All services run with `NODE_ENV=production` in production deployments.

## Developer Workflows
- **Build & Run Locally:**
  ```sh
  docker-compose up --build
  ```
- **Access Services:**
  - User:     http://localhost:3000/users
  - Product:  http://localhost:3001/products
  - Order:    http://localhost:3002/orders
  - Gateway:  http://localhost:3003/api/users (and similar for products/orders)
- **Kubernetes Deploy:**
  - Apply deployments: `kubectl apply -f deployment/`
  - Apply services:    `kubectl apply -f service/`
  - Apply ingress:     `kubectl apply -f ingress/ingress.yml`

## Integration & Communication
- **Service Discovery:** Services communicate using Docker/Kubernetes DNS (e.g., `user-service:3000`).
- **API Calls:** All inter-service calls use HTTP via Axios.
- **Ingress:** All `/api/*` routes are routed to the correct service via the ingress controller.

## Notable Files
- `docker-compose.yml` — Compose config for local dev
- `deployment/*.yml`   — Kubernetes Deployments
- `service/*.yml`      — Kubernetes Services
- `ingress/ingress.yml`— Ingress rules
- `gateway-service/app.js` — API aggregation logic

## Project-Specific Notes
- **No database**: All data is in-memory for demo purposes.
- **No authentication**: This is a demo/learning project.
- **Duplicate structure**: There are nested `Microservices/` folders; prefer the top-level for edits.

## Example: Adding a New Service
1. Create a new folder with `app.js`, `package.json`, and `dockerfile`.
2. Add service config to `docker-compose.yml`, `deployment/`, and `service/`.
3. Update `gateway-service/app.js` and `ingress/ingress.yml` as needed.

---

For more details, see `README.md` and the respective service folders.
