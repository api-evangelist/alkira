---
name: Inventory multi-cloud connectors
description: Enumerate every cloud and site connector across an Alkira tenant network so an agent can report the multi-cloud footprint (AWS, Azure, GCP, OCI, internet).
api: mcp/alkira-mcp.yml
operations: [cxp_get_all, connector_aws_vpc_get_all, connector_aws_tgw_get_all, connector_azure_vnet_get_all, connector_gcp_vpc_get_all, connector_oci_vcn_get_all, connector_internet_get_all]
---

# Inventory multi-cloud connectors

Use the official Alkira MCP server (`alkiranet/mcp-alkira`) or the Portal REST API
(`https://<tenant>.portal.alkira.com/api`). All tools here are read-only.

## Auth
- Generate a per-user API key in the Portal: `Settings` -> `User Management`.
- REST calls send `Authorization: api-key <base64(API_KEY)>`.
- The MCP server is launched with `--portal <tenant>.portal.alkira.com --key <API_KEY>`.

## Steps
1. List the Cloud Exchange Points with `cxp_get_all` to establish where connectors land.
2. Enumerate cloud connectors per provider:
   - AWS: `connector_aws_vpc_get_all` and `connector_aws_tgw_get_all`
   - Azure: `connector_azure_vnet_get_all`
   - GCP: `connector_gcp_vpc_get_all`
   - OCI: `connector_oci_vcn_get_all`
   - Internet egress: `connector_internet_get_all`
3. For any connector of interest, fetch detail with the matching `*_get_by_id` or
   `*_get_by_name` tool (e.g. `connector_aws_vpc_get_by_name`).
4. Aggregate by CXP and segment to report the multi-cloud footprint.

## Conventions
- List responses use offset/limit pagination with a `{data, pagination:{Offset,limit,hits}}` envelope.
- Resources are nested under the tenant network id resolved at session start.
- Honor `Retry-After` on HTTP 429. See conventions/alkira-conventions.yml.
