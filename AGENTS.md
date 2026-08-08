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

- **`src/antora.yml`** \
  Antora component descriptor for this module.

- **`src/modules/ROOT/nav.adoc`** \
  The page hierarchy. Antora uses it to compute each page's breadcrumb
  ancestry when the site is built.

- **`src/modules/ROOT/pages/`** \
  One `index.adoc` per category, nested by topic (e.g.
  `applications/ai/skills/index.adoc`). Each contains a bullet list of
  bookmark links.

- **`.agents/skills/add-bookmark/`** \
  An agent skill for adding a new bookmark: given a URL, it follows the
  link, chooses the best-fit category, and appends a properly formatted
  entry to the relevant `index.adoc` — without committing the change
  unless explicitly told to. See `.agents/skills/add-bookmark/README.md`
  for details.

## Rules

- New bookmark entries MUST be added as a valid AsciiDoc bullet
  (`* https://.../[Label]`) to exactly one `index.adoc` under
  `src/modules/ROOT/pages/`, following the
  **[add-bookmark](./.agents/skills/add-bookmark/SKILL.md)** skill when
  adding via an agent.

## References

This project follows Kieran Potts' technical standards.

- **[TS-9: Version Control](https://raw.githubusercontent.com/kieranpotts/standards/refs/heads/latest/dev/src/009/AGENTS.md)**
- **[TS-28: AsciiDoc](https://raw.githubusercontent.com/kieranpotts/standards/refs/heads/latest/dev/src/028/AGENTS.md)**
- **[TS-61: AI Tools](https://raw.githubusercontent.com/kieranpotts/standards/refs/heads/latest/dev/src/061/AGENTS.md)**
