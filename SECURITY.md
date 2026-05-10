# Security policy

## Reporting a vulnerability

Email <support@shinobidata.com> with subject `Security: <short summary>`. Please don't open a public GitHub issue for security stuff.

We aim to acknowledge in 48 hours and ship a fix or mitigation in 30 days for valid issues. We'll credit you in the disclosure note unless you'd rather stay anonymous.

## Scope

In scope:

- The hosted MCP service at `https://mcp.shinobidata.com` (OAuth flow, tool endpoints, well-known metadata).
- The OAuth consent screen at `https://mcp.shinobidata.com/oauth/authorize`.
- Anything reachable from the public well-known endpoints.

Out of scope (please don't test these):

- DoS or volumetric attacks.
- Social-engineering attacks against the maintainer or any users.
- Issues in third-party services we depend on (Cloudflare, Anthropic, OpenAI, etc.). Report to those providers directly.

## Coordinated disclosure

1. You email us with the report.
2. We confirm receipt and start investigating.
3. We work with you on a timeline. Default is 30 days for moderate, sooner for critical (RCE, auth bypass).
4. After the fix ships, we publish a brief disclosure note crediting the reporter.

Please don't disclose publicly before the coordinated date. We're far more responsive when reports don't surprise us.

## Past advisories

None yet. When we ship one, it'll appear here with a date and summary.
