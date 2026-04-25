---
name: devops
description: >
  Deploy and manage cloud infrastructure on Cloudflare (Workers, R2, D1, KV), Docker containers, and Google Cloud Platform (GKE, Cloud Run, Compute Engine). Use when deploying serverless functions, configuring CI/CD pipelines, containerizing apps, or managing cloud databases.
---

# DevOps Skill

Comprehensive guide for deploying and managing cloud infrastructure.

---

## Platform Selection

| Platform | Best For |
| :--- | :--- |
| **Cloudflare** | Edge-first, serverless, low latency |
| **Docker** | Local dev, containers, microservices |
| **Google Cloud** | Enterprise-scale, managed services |

---

## Cloudflare Workers

### Quick Start

```bash
# Install Wrangler
npm install -g wrangler

# Create and deploy
wrangler init my-worker
cd my-worker
wrangler deploy
```

### Key Products

| Product | Use |
| :--- | :--- |
| **Workers** | Serverless functions |
| **R2** | Object storage (S3-compatible, no egress fees) |
| **D1** | SQLite database |
| **KV** | Key-value store |
| **Durable Objects** | Stateful edge compute |

---

## Docker

### Dockerfile Pattern

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

### Multi-stage Build

```dockerfile
# Build stage
FROM node:20-alpine AS builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build

# Runtime stage
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --production
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

### Commands

```bash
docker build -t myapp .
docker run -p 3000:3000 myapp
docker-compose up -d
docker-compose logs -f
```

---

## Google Cloud

### Common Services

| Service | Use |
| :--- | :--- |
| **Compute Engine** | VMs |
| **GKE** | Kubernetes |
| **Cloud Run** | Containerized serverless |
| **Cloud Storage** | Object storage |
| **Cloud SQL** | Managed databases |

### Deploy to Cloud Run

```bash
gcloud run deploy my-service \
  --image gcr.io/project/image \
  --region us-central1 \
  --platform managed
```

---

## CI/CD Pipeline

### GitHub Actions Example

```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
      - run: go test ./...
      - run: go build ./...
      - run: wrangler deploy
        env:
          CF_API_TOKEN: ${{ secrets.CF_API_TOKEN }}
```

---

## Infrastructure as Code

| Tool | Use |
| :--- | :--- |
| **Terraform** | Cloud resources |
| **Pulumi** | IaC in code |
| **CDK** | Cloud Development Kit |

---

## Observability

### Logs & Metrics

| Platform | Tools |
| :--- | :--- |
| **Cloudflare** | Workers Analytics |
| **GCP** | Cloud Logging, Monitoring |
| **Docker** | `docker logs`, prometheus |

### Structured Logging

```go
logger := slog.NewJSONHandler(os.Stdout)
logger.Log(ctx, "info", "request completed",
    "method", r.Method,
    "path", r.URL.Path,
    "duration_ms", duration.Milliseconds())
```