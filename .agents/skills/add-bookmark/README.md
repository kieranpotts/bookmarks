# Add bookmark

Files a single web resource into the bookmarks collection.

Given a URL, the agent fetches the page, works out what the resource is,
traverses the category tree under `src/modules/ROOT/pages/` to find the
best-fit page, and inserts an AsciiDoc bullet into that page's `index.adoc`.
It updates `src/modules/ROOT/nav.adoc` only in the rarer case that it had to
create a new category page. The change is left in the working tree — the skill
is expressly instructed not to commit it.

## Interactivity

The skill runs interactively. Where the agent has low confidence in the
category it has chosen, or two or more categories are equally good candidates,
it stops and asks the user to make the final call. It may also ask for the URL
itself, if that is not clear from the conversation. It is therefore not suited
to unattended, away-from-keyboard runs.

## How to invoke

> Add bookmark: https://example.com/

> Add this to my bookmarks.

You can steer the placement by naming a category in the prompt — for example,
"add https://example.com/ under infrastructure/web-servers" — in which case
the agent skips the traversal and files it there.

## Recommended models

A fast, cheap utility model is sufficient. The work is a single fetch, a
category lookup, and a one-line insertion; nothing here calls for extended
reasoning.

## References

- [AsciiDoc syntax quick reference](https://docs.asciidoctor.org/asciidoc/latest/syntax-quick-reference/) —
  the link and list syntax used by every bookmark entry.

- [Antora navigation files](https://docs.antora.org/antora/latest/navigation/files-and-lists/) —
  how `nav.adoc` builds the page hierarchy that drives breadcrumbs on the
  published site.
