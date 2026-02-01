# VPC Compose - Orchestration Context

This document provides an overview of the Docker Compose orchestration for the VPC stack, environment management, and deployment strategies.

## Overview

The `vpc-compose` directory contains the configuration for orchestrating the multiple services that make up the Virtual Pinball Chat (VPC) stack.

### Key Components

- **Orchestration:** Docker Compose
- **Network:** A shared bridge network named `vpc-network` for inter-service communication.
- **Environment Management:** Use of `.env` files for service-specific configuration.

## Compose Files

The project uses multiple compose files tailored for different environments:

- `docker-compose.yml`: The standard production configuration. Uses pre-built images from Docker Hub.
- `docker-compose-local.yml`: Used for local development. Builds images from local source code and mounts volumes for development convenience.
- `docker-compose-refactor.yml`: Used for testing new architectural changes (tagged as `:refactor`).
- `docker-compose-aws.yml`: A specific configuration for AWS-based deployments.

## Common Service Configurations

- **Logging:** All services use the `json-file` driver with rotation (max-size: 10m, max-file: 14).
- **Restart Policy:** Most services use `unless-stopped` to ensure high availability.
- **Resource Limits:** Services have defined memory limits (e.g., 64MB for Nginx, 512MB for Next.js) to prevent resource exhaustion.

## Service Dependencies

The stack follows a specific dependency order:

1. **APIs:** `vpc-data`, `vps-data`, `vpw-data` (no dependencies).
2. **Frontend & Bots:** `vpc-next` and `vpc-bot` depend on the data APIs.
3. **Gateway:** `nginx` depends on the frontend and data APIs to ensure it has backends to proxy to.

## SSL & Certbot

In production-like environments, a `certbot` service (using `dns-cloudflare`) is used to manage SSL certificates. These certificates are shared with the `nginx` service via a volume mount.

## Local Development Workflow

To start the entire stack locally:

```bash
docker compose -f docker-compose-local.yml up -d --build
```

To view logs for a specific service:

```bash
docker compose -f docker-compose-local.yml logs -f <service-name>
```

## Environment Variable Templates

Templates for all required environment variables are stored in this directory (e.g., `vpc-bot.env`, `vpc-data.env`). Ensure these are populated correctly before starting the stack.
