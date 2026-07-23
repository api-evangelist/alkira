---
name: Check tenant network health
description: Pull a health and status snapshot of an Alkira tenant network — summary, connector/service health, route summary, and open alerts.
api: mcp/alkira-mcp.yml
operations: [tenant_network_summary, tenant_resource_usages, health_connector_get_by_id, health_service_get_by_id, route_get_summary, notification_alerts, notification_jobs]
---

# Check tenant network health

Read-only health/monitoring flow over the official Alkira MCP server or Portal REST API.

## Auth
- Per-user API key from Portal `Settings` -> `User Management`.
- REST: `Authorization: api-key <base64(API_KEY)>`.

## Steps
1. Get the network snapshot with `tenant_network_summary` and capacity with `tenant_resource_usages`.
2. Check per-connector health with `health_connector_get_by_id` (and instance-level via
   `health_connector_instance_get_by_id`).
3. Check integrated-service health with `health_service_get_by_id` (instance detail via
   `health_service_instance_get_by_id`).
4. Summarize routing with `route_get_summary` (and `route_get_count`).
5. Surface operational signals with `notification_alerts`, `notification_jobs`, and
   `notification_auditLogs`.
6. Report a red/amber/green rollup per connector and service.

## Conventions
- Correlate requests with the `x-ak-request-id` header.
- Retries: honor `Retry-After` on 429. See conventions/alkira-conventions.yml.
