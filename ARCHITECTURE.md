# Architecture and implemented boundaries

Adapted from canonical `architecture/ARCHITECTURE_V1.md` and the project source/release documentation; this is a newly authored summary.

React issues commands to a Fastify modular monolith. PostgreSQL transactions/constraints protect reservations and custody; local private storage holds restricted evidence. A same-codebase worker consumes durable jobs/outbox and produces notifications, SSE and signed webhook deliveries. REST snapshots remain authoritative. Planned reservations and actual custody are separate entities. Runtime roles use BYPASSRLS; direct RLS tests are narrower than deployed authorization. External effects can duplicate; no broker, Redis, microservice fleet, Kubernetes or S3 is implied.

The public-friendly Mermaid diagram is in the [README](README.md). It depicts implemented components, not proposed future infrastructure.

[Return to the case study](README.md).
