---
name: Audit segmentation and traffic policy
description: Review an Alkira tenant network's segmentation and the traffic/NAT policy that governs east-west and egress flows, for a security/compliance posture check.
api: mcp/alkira-mcp.yml
operations: [segment_get_all, segment_resource_get_all, segment_resource_share_get_all, policy_traffic_get_all, policy_traffic_rule_get_all, policy_nat_get_all, list_prefix_get_by_name]
---

# Audit segmentation and traffic policy

Read-only governance flow over the official Alkira MCP server or Portal REST API.

## Auth
- Per-user API key from Portal `Settings` -> `User Management`.
- REST: `Authorization: api-key <base64(API_KEY)>`.

## Steps
1. List segments with `segment_get_all` to establish the isolation boundaries.
2. Map shared resources with `segment_resource_get_all` and `segment_resource_share_get_all`
   to see what crosses segments.
3. Pull traffic policy with `policy_traffic_get_all`, then rule detail with
   `policy_traffic_rule_get_all` / `policy_traffic_rule_list_get_all`.
4. Pull NAT policy with `policy_nat_get_all` and rules with `policy_nat_rule_get_all`.
5. Resolve prefix/CIDR scope referenced by rules with `list_prefix_get_by_name`,
   `list_global_cidr_get_by_name`, and BGP community lists as needed.
6. Flag any segment reachable without an explicit allow rule, and any overly broad
   prefix in an allow policy.

## Conventions
- List responses paginate via offset/limit; iterate on `pagination.hits`.
- Resources are scoped to the tenant network id. See conventions/alkira-conventions.yml.
