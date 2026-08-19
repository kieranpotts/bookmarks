# Bookmarks

A curated collection of interesting destinations on the web, organized as
an AsciiDoc/Antora content module and published as part of
[kieranpotts.com](https://kieranpotts.com) (see the `website` repo, which
consumes this content and its `nav.adoc` for breadcrumb rendering).

## Tech stack

- AsciiDoc content, structured as an [Antora](https://antora.org/) module
  (`src/antora.yml`, `src/modules/ROOT/`).
- `pre-commit` with the shared `kieranpotts/pre-commit-hooks` commit-message
  validation hook.

## Project structure

- `src/antora.yml` \
  Antora component descriptor for this module.

- `src/modules/ROOT/nav.adoc` \
  The page hierarchy. Antora uses it to compute each page's breadcrumb
  ancestry when the site is built.

- `src/modules/ROOT/pages/` \
  One `index.adoc` per category, nested by topic (e.g.
  `applications/ai/skills/index.adoc`). Each contains a bullet list of
  bookmark links.

- `.agents/skills/bookmark/` \
  An agent skill for adding a new bookmark. Given a URL, it follows the
  link, chooses the best-fit category, and appends a properly formatted
  entry to the relevant `index.adoc` — without committing the change
  unless explicitly told to. See `.agents/skills/bookmark/README.md`
  for details.

## Rules

- New bookmark entries MUST be added as a valid AsciiDoc bullet
  (`* https://.../[Label]`) to exactly one `index.adoc` under
  `src/modules/ROOT/pages/`, following the
  **[bookmark](./.agents/skills/bookmark/SKILL.md)** skill when
  adding via an agent.

- If a description mentions a non-existent, non-public, or otherwise
  unreachable URL as example text (e.g. `http://localhost:4566`), wrap it
  in `` `pass:[...]` ``, not plain backticks. AsciiDoc autolinks bare URLs
  even inside monospace text, and the site's link-check build step crawls
  those live links and fails when they don't resolve. See
  **[CONTRIBUTING.md](./CONTRIBUTING.md)** for details.

## References

The following technical standards (TS) govern this project. Fetch and ingest
the relevant standards as-and-when required for the task at hand.

- [**TS-9: Version Control**](https://kieranpotts.com/standards/009) \
  Use when working with Git. Covers commits, branching, merging, integration
  strategies, cutting releases, and configuring Git/PR/CI tooling.

- [**TS-28: AsciiDoc**](https://kieranpotts.com/standards/028) \
  Use when writing or reviewing AsciiDoc documents or websites built using
  Antora.

- [**TS-61: AI Tools**](https://kieranpotts.com/standards/061) \
  Use when planning or executing coding tasks, managing your own context,
  authoring AGENTS.md files or agent skills, calling tools, or handling
  untrusted content.
