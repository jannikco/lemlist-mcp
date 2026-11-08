# lemlist MCP server

Review campaigns and explicitly create a campaign with automatic lead review disabled.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `LEMLIST_BASIC_AUTH`



Credential help: https://developer.lemlist.com/api-reference/openapi/v2.json

## Provider setup

LEMLIST_BASIC_AUTH is the base64 encoding of a colon followed by your API key (empty username, key as password); do not include the Basic prefix. Compute it locally without logging your key. The connector adds the Basic prefix. Campaign creation does not authorize adding or launching leads.

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call lemlist_list_campaigns '{"limit":10}'
```

## Use with an MCP client

Launch `node /absolute/path/lemlist-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| lemlist_list_campaigns | Read | Read a bounded page of campaigns. |
| lemlist_create_campaign | Explicit approval required | Create a campaign with autoReview:false; no lead launch tool is exposed. |

## Concrete workflow

Review campaigns and explicitly create a campaign with automatic lead review disabled.

1. lemlist_list_campaigns
2. lemlist_create_campaign

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Activate with PipMCP](https://pipmcp.com/github/start?app=lemlist&integration=lemlist-mcp&utm_source=github&utm_campaign=github-lemlist-mcp&utm_content=readme) when you want the hosted service.  Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/lemlist/).

## Provenance and maintenance

- https://developer.lemlist.com/api-reference/openapi/v2.json

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
