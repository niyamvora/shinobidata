# Contributing

Thanks for being here. This repo is the public docs / examples / assets surface for ShinobiData. The MCP server itself runs at `https://mcp.shinobidata.com` from a separate (currently closed) codebase.

## What's welcome

| Type | What helps | Where |
|---|---|---|
| Bug reports | OAuth that doesn't work, a tool returning wrong data, consent screen issues | [Bug template](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=bug.yml) |
| Tool requests | "I'd love a tool that does X" | Discuss first in [Discussions → Ideas](https://github.com/dark-horse-stocks/shinobidata/discussions/categories/ideas), then file once the shape is clear |
| Doc fixes | Typos, unclear instructions, missing screenshot, broken link | PR direct |
| Translations | `README.{ja,ko,vi}.md` and other localized docs | PR direct, see notes below |
| Examples | New prompt patterns for `examples/prompts.md`, sample integrations | PR direct |
| Comparison-table updates | New finance MCPs to compare against | PR direct, factual not snarky |
| Architecture diagrams | Improvements to mermaid diagrams in `docs/architecture.md` | PR direct |

## What's out of scope

- Code changes to the MCP server. The server source isn't here. If you've found a tool-level bug or want to propose a behavior change, [Discussions → Q&A](https://github.com/dark-horse-stocks/shinobidata/discussions/categories/q-a) or <support@shinobidata.com>.
- Production credentials, internal infra details, anything beyond what's already public on `mcp.shinobidata.com`'s well-known endpoints.

## Filing a good bug report

The [bug template](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=bug.yml) asks for the fields we actually use:

- AI client + version (Claude Desktop x.y.z, ChatGPT Plus, etc.).
- Tool you called + the prompt.
- Expected vs actual.
- Console / logs if you have them. For OAuth issues, the contents of `~/.mcp-auth/` (with secrets redacted) help a lot.

## Filing a good tool request

The [tool request template](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=tool-request.yml) asks for:

- The user-facing question the AI should be able to answer. Phrase it as a question, not a tool name.
- What data sources it would need. Rough is fine ("earnings revisions over time").
- Single-company, multi-company, or portfolio-level.
- Cursor-paginated or constant-size.

Most tools that ship come from real questions someone tried to ask Claude or ChatGPT and couldn't answer.

## Translations

The English `README.md` is canonical. Translations live as `README.<lang>.md` (e.g., `README.ja.md`, `README.ko.md`, `README.vi.md`). Each one:

1. Mirrors the English structure section by section. Skip a section you can't translate accurately rather than guess.
2. Keeps technical terms in English where there's no widely-used native equivalent (OAuth, MCP, PKCE).
3. Includes the language switcher at the top:

   ```markdown
   [English](README.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md)
   ```

4. Gets reviewed by at least one native speaker. Note in the PR if you'd like a review before merge.

For new languages, copy `README.md` to `README.<lang>.md` and start. Open a PR with whatever you have. Bar is "useful for native speakers", not "publication ready".

## PR guidelines

- One concern per PR. Doc rewrites + image updates + new examples = three PRs, not one.
- Reference any related issue in the PR body (`Refs #N` for partial work, `Closes #N` for full).
- For doc edits, eyeball that mermaid diagrams still render on GitHub.
- For code in `examples/`, keep it minimal. These are reference snippets, not full apps.

## Code of conduct

[Contributor Covenant](CODE_OF_CONDUCT.md). Be kind. Disagreements happen; ad hominem doesn't.

## License

By contributing you agree your contributions are MIT (see [`LICENSE`](LICENSE)).
