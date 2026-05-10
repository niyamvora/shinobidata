# Security policy

## Reporting a vulnerability

Please report security issues **privately** — don't open a public GitHub issue.

📧 **Email**: [support@shinobidata.com](mailto:support@shinobidata.com)
**Subject**: `Security: <short summary>`

We aim to acknowledge within 48 hours and to ship a fix or mitigation within 30 days for valid issues. We'll credit you in the disclosure note unless you'd rather stay anonymous.

## Scope

In scope:

- The hosted MCP service at `https://mcp.shinobidata.com` (OAuth flow, tool endpoints, well-known metadata).
- The OAuth consent screen at `https://mcp.shinobidata.com/oauth/authorize`.
- Anything reachable from the public well-known endpoints.

Not in scope (please don't test these):

- Denial-of-service or volumetric attacks against the public endpoints.
- Social-engineering attacks against the maintainer or any users.
- Issues in third-party services we depend on (Cloudflare, Anthropic, OpenAI, etc.) — please report to those providers directly.

## Coordinated disclosure

We follow a coordinated-disclosure model:

1. You email us with the report.
2. We confirm receipt + start investigating.
3. We work with you on a timeline; default is 30 days for moderate issues, sooner for critical (e.g., RCE, auth bypass).
4. After the fix ships, we publish a brief disclosure note crediting the reporter.

Please don't publicly disclose before the coordinated date — we'll be far more responsive if we know the report won't surprise us.

## Past advisories

None yet. (When we ship one, it'll appear here with a date and summary.)
