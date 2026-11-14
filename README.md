# @dnsdoctor/mcp: DNS Doctor as a local stdio MCP server

[![mcptry](https://mcptry.com/badge/dev.dnsdoctor/dns-doctor)](https://mcptry.com/servers/dev.dnsdoctor/dns-doctor) [![DNS Doctor MCP server – quality and maintenance score on Glama](https://glama.ai/mcp/servers/dnsdoctor/mcp/badges/score.svg)](https://glama.ai/mcp/servers/dnsdoctor/mcp)

A thin MCP server that runs on your machine and calls the public
[DNS Doctor](https://dnsdoctor.dev) REST API. It exposes the same 29 tools as the
hosted server at `https://dnsdoctor.dev/mcp`: scan a domain's email
authentication (SPF, DMARC, DKIM), MX, DNS health, blacklists and domain/SSL
expiry, check propagation, and get the exact fix record to publish.

It holds no diagnosis logic of its own and relays every record string verbatim.
Every fix record comes from DNS Doctor's deterministic engine, never from a model
guess.

Most clients should use the hosted server instead (no install, no keys):

```json
{ "mcpServers": { "dns-doctor": { "type": "http", "url": "https://dnsdoctor.dev/mcp" } } }
```

For Claude Code, the [DNS Doctor plugin](https://github.com/dnsdoctor/claude-plugin)
wires the hosted server and adds the skills.

## Run it

```json
{
  "mcpServers": {
    "dns-doctor": {
      "command": "npx",
      "args": ["-y", "@dnsdoctor/mcp@1.21.0"],
      "env": { "DNSDOCTOR_API_TOKEN": "dnsd_YOUR_TOKEN" }
    }
  }
}
```

Node 20 or newer. Pin the version as shown, so the code you run is the code you
reviewed.

- `DNSDOCTOR_API_TOKEN` is optional. Anonymous access covers scanning and fixes;
  the account tools (monitored domains, alerts, readiness, lookalike watches,
  email tests) need a token. Create one at dnsdoctor.dev under
  **Settings > API tokens**. The client sends it only to dnsdoctor.dev, as a
  bearer token. Never commit it.
- `DNSDOCTOR_API_BASE` overrides the origin. It must serve the `/api/...` paths,
  i.e. the public site rather than a bare backend port.

## Build from source

```bash
npm ci
npm test
npm run build      # -> dist/index.js, the package's bin entrypoint
```

`tools.json` and `instructions.txt` are generated from the hosted server and
pinned by a test on the server side: never hand-edit them, and never add a tool
description to `src/`. Both ship in the npm tarball; without them the client has
no tools and none of its safety guidance.

## License

Apache-2.0
