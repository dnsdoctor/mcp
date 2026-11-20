# DNS Doctor: get your domain's email working, and know it stays that way

[![mcptry](https://mcptry.com/badge/dev.dnsdoctor/dns-doctor)](https://mcptry.com/servers/dev.dnsdoctor/dns-doctor) [![DNS Doctor MCP server – quality and maintenance score on Glama](https://glama.ai/mcp/servers/dnsdoctor/mcp/badges/score.svg)](https://glama.ai/mcp/servers/dnsdoctor/mcp) [![npm](https://img.shields.io/npm/v/@dnsdoctor/mcp)](https://www.npmjs.com/package/@dnsdoctor/mcp) [![npm downloads](https://img.shields.io/npm/dm/@dnsdoctor/mcp)](https://www.npmjs.com/package/@dnsdoctor/mcp) [![DNS Doctor MCP on mcprush](https://mcprush.com/badge/dns-doctor-mcp/score.svg)](https://mcprush.com/to/dns-doctor-mcp?ref=badge)

See why mail lands in spam, get the exact DNS record to fix it, and check the change
is live everywhere.

DNS Doctor checks a domain's email setup (SPF, DKIM, DMARC), mail routing, DNS
health, blacklists and domain/SSL expiry, and tells your AI assistant what is
wrong in plain words. When a record needs fixing it returns the exact record to
copy and paste. That record comes from a validating engine, never from a model
guess, and nothing is ever changed in your DNS for you.

Scanning and fixes are free: no account, no key, no card.

## Ask your assistant

- "Why do emails from example.com land in spam?"
- "Check SPF, DKIM and DMARC for example.com and give me the record to fix what fails."
- "I just changed my MX records. Is the change live everywhere yet?"
- "Is example.com on any blacklist, and when do its domain and SSL certificate expire?"
- "Is it safe to move example.com from p=none to p=quarantine?"

## Install

This package runs on your machine and calls the public
[DNS Doctor](https://dnsdoctor.dev) API. It has the same 29 tools as the hosted
server and holds no diagnosis logic of its own: every record string is relayed
verbatim.

```json
{
  "mcpServers": {
    "dns-doctor": {
      "command": "npx",
      "args": ["-y", "@dnsdoctor/mcp@1.22.0"]
    }
  }
}
```

Node 20 or newer. Pin the version as shown, so the code you run is the code you
reviewed.

If your client supports remote servers, the hosted one needs no install at all:

```json
{ "mcpServers": { "dns-doctor": { "type": "http", "url": "https://dnsdoctor.dev/mcp" } } }
```

For Claude Code, the [DNS Doctor plugin](https://github.com/dnsdoctor/claude-plugin)
wires the hosted server and adds the skills.

## Monitoring (optional)

A scan is a snapshot. [Monitoring](https://dnsdoctor.dev/pricing) watches your
domains every day, reads your DMARC reports, emails you when something breaks
and tells you when it is safe to tighten DMARC. With an account, your assistant
can also list your monitored domains, alerts and readiness.

Add a token to use the account tools:

```json
"env": { "DNSDOCTOR_API_TOKEN": "dnsd_YOUR_TOKEN" }
```

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
