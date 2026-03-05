# Example: SaaS Microservices Architecture

**Input:** A JSON file describing a SaaS backend architecture

**Topology:** hub-and-spoke

**Central node:** API Gateway (Kong / AWS) — 3,200 req/s

**Spoke nodes:**
1. Auth Service — JWT + OAuth2, sessions — CRITICAL
2. Billing — Stripe, invoicing, metering — $$$
3. Notifications — SES, FCM, Twilio — ASYNC
4. Analytics — ClickHouse, event tracking — READ-HEAVY
5. User Service — CRUD, profiles, RBAC — POSTGRESQL
6. Storage — S3, file upload, CDN — BLOB
7. Search — Elasticsearch, faceted — INDEXED
8. WebSocket — Real-time, presence, collab — WS

**Info panels:**
- Infrastructure: Kubernetes (EKS), Terraform, GitHub Actions, RabbitMQ, Kafka
- Observability: Datadog, ELK Stack, Sentry, OpenTelemetry

**Callout:** "Services never call each other directly. Sync via gateway, async via message queue."

**Legend:** blue=Gateway, cyan=Core services, green=Data services, orange=External integrations, red=Critical path
