# Copilot Instructions for Not So Simple Ecommerce

## Project Overview
- **Architecture:** Microservices-based e-commerce platform with 6 backend .NET services and a React frontend. All components are containerized and orchestrated via Docker Compose.
- **Key Directories:**
  - `src/domains/`: Domain logic for Identity, Main, Order, and Shared services (C#).
  - `src/infrastructure/`: Infrastructure services (e.g., S3, SES, SNS, SQS handlers).
  - `src/services/`: API and service entrypoints (e.g., IdentityServer, Main, Order, HealthChecker).
  - `src/workers/`: Background workers (InvoiceGenerator, Notificator).
  - `src/frontend/`: React app (Vite, TypeScript, Nginx for runtime).
  - `iac/`: Infrastructure as Code (Terraform for local AWS emulation).
  - `nginx/`: Nginx reverse proxy configuration.
  - `cli/`: Utility scripts (e.g., certificate generation).

## Developer Workflows
- **Local Infrastructure:**
  - Start core infra (LocalStack, Postgres, Nginx, Terraform):
    ```bash
    docker-compose -f docker-compose.infra.yml up -d
    ```
- **App & Workers:**
  - Start backend APIs and workers:
    ```bash
    docker-compose -f docker-compose.workers.yml -f docker-compose.yml up -d
    ```
- **Frontend Build:**
  - Build and serve via Dockerfile in `src/frontend/src/Dockerfile` (multi-stage Node + Nginx).
- **Certificates:**
  - Regenerate with `cli/generate-certs.sh` (requires bash).
  - Install `root-ca.crt` to OS trust store for HTTPS.
- **Migrations:**
  - DB migrations run automatically on service startup (no manual EF CLI needed).

## Patterns & Conventions
- **Service Boundaries:**
  - Each domain/service has its own project and directory; shared logic in `NotSoSimpleEcommerce.Shared`.
- **Communication:**
  - APIs exposed via Nginx reverse proxy on `https://devopsnanuvem.internal:44300`.
  - Inter-service communication via HTTP and AWS emulated services (SQS, SNS, S3, SES) through LocalStack.
- **Environment:**
  - All services expect local AWS endpoints and Postgres; see `docker-compose.infra.yml` for details.
- **Frontend:**
  - Built with Vite, served by Nginx. Custom config in `src/frontend/nginx/nginx.conf`.

## Integration Points
- **LocalStack:** Used for AWS service emulation (SQS, SNS, S3, SES).
- **Postgres:** Main database for all services.
- **Nginx:** Central entrypoint for all HTTP traffic.
- **Terraform:** Provisions local AWS resources in LocalStack.

## Troubleshooting
- If services fail to start, check Docker Compose logs for infra containers first.
- For HTTPS issues, ensure the root CA certificate is installed and password matches `.env` variable.

---

For more details, see `README.md` and service-specific directories. Update this file if project structure or workflows change.
