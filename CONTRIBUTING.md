# Contributing to ShinobiData

Thanks for your interest. This repo is the public docs / examples / assets surface for [ShinobiData](https://github.com/niyamvora/shinobidata) — the MCP server itself is hosted at `https://mcp.shinobidata.com` and runs from a separate (currently closed) codebase.

## What contributions are welcome

| Type | What's helpful | Where to send it |
| --- | --- | --- |
| **Bug reports** | OAuth flow that doesn't work; tool returning wrong data; consent screen issues | [Issues → Bug report](https://github.com/niyamvora/shinobidata/issues/new?template=bug.yml) |
| **Tool requests** | "I'd love a tool that does X" | [Discussions → Ideas](https://github.com/niyamvora/shinobidata/discussions/categories/ideas) — discuss first, then file an [issue](https://github.com/niyamvora/shinobidata/issues/new?template=tool-request.yml) once the shape is clear |
| **Doc fixes** | Typos, unclear instructions, missing screenshot, broken link | PR direct |
| **Translations** | `README.{ja,ko,vi}.md` and other localized docs | PR direct — see [Multi-language docs](#multi-language-docs) below |
| **Examples** | New prompt patterns for `examples/prompts.md`; sample integrations | PR direct |
| **Comparison table updates** | New finance MCPs to compare against in `README.md` | PR direct — keep it factual, not snarky |
| **Architecture diagrams** | Improvements to the mermaid diagrams in `docs/architecture.md` | PR direct |

## What's out of scope here

- **Code changes to the MCP server itself.** The server source isn't in this repo; reach out via [Discussions → Q&A](https://github.com/niyamvora/shinobidata/discussions/categories/q-a) or [support@shinobidata.com](mailto:support@shinobidata.com) if you've found a tool-level bug or want to propose a behavior change.
- **Production credentials, internal infra details, or anything beyond what's already public on `mcp.shinobidata.com`'s well-known endpoints.** Don't include them in PRs.

## Filing a good bug report

Use the [bug report template](https://github.com/niyamvora/shinobidata/issues/new?template=bug.yml). The fields we actually use:

- **AI client + version** (Claude Desktop x.y.z, ChatGPT Plus, etc.).
- **Tool you called** + the prompt you gave the AI.
- **What you expected** vs **what you got**.
- **Console / logs** if available — for OAuth issues, `~/.mcp-auth/` directory contents (with secrets redacted) help a lot.

## Filing a good tool request

Use the [tool request template](https://github.com/niyamvora/shinobidata/issues/new?template=tool-request.yml). Helpful detail:

- **What's the user-facing question** the AI should be able to answer? Not the tool name yet — the question.
- **What data sources** would the tool need? (Even rough — "earnings revisions over time" is enough.)
- **Single-company, multi-company, or portfolio-level**?
- **Cursor-paginated or constant-size** (per [§6.6](docs/architecture.md#why-our-analytics-tools-dont-paginate))?

Most tools that ship come from real questions someone tried to ask Claude/ChatGPT and couldn't.

## Multi-language docs

The canonical README is in English (`README.md`). Translations live alongside as `README.<lang>.md` (e.g., `README.ja.md`, `README.ko.md`, `README.vi.md`). Each translation:

1. Mirrors the English structure section-for-section. Skip sections you can't translate accurately rather than guessing.
2. Keeps technical terms in English where there's no widely-used native equivalent (e.g., "OAuth", "MCP", "PKCE").
3. Includes the language switcher row at the top:

   ```markdown
   🌐  [English](README.md)  ·  [日本語](README.ja.md)  ·  [한국어](README.ko.md)  ·  [Tiếng Việt](README.vi.md)
   ```

4. Is reviewed by at least one native speaker — leave a note in the PR if you'd like a review before merging.

For new languages, copy `README.md` to `README.<lang>.md` and start translating. Open a PR with whatever you have; the bar is "useful for native speakers", not "publication-ready".

## Pull request guidelines

- One concern per PR. Doc rewrites + image updates + new examples → 3 PRs, not 1.
- Reference any related issue in the PR body (`Refs #N` for partial work, `Closes #N` when the issue's done).
- For doc edits, run a quick visual check that mermaid diagrams still render on GitHub.
- For code in `examples/`, keep it minimal — these are reference snippets, not full apps.

## Code of conduct

This project follows the [Contributor Covenant](CODE_OF_CONDUCT.md). Be kind. Disagreements happen; ad hominem doesn't.

## License

By contributing you agree that your contributions are licensed under MIT (see [`LICENSE`](LICENSE)).
