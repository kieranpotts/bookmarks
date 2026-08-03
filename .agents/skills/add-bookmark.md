---
name: add-bookmark
description: >-
  Add a new bookmark. Use when the user says something like "add bookmark"
  or "add this to my bookmarks".
license: CC0
metadata:
  interactive: yes
  preferred_model: ollama/WORKFLOW_BASIC
---

# Add bookmark

Follow the URL supplied by the user, analyse the resource, and add a link to it
in the most appropriate category of the bookmarks collection.

Do not commit your changes, unless expressly instructed by the user.

## Parameters

Determine the following information from the surrounding context and
environment. You MAY prompt the user for clarification if you are unsure about
the requirements.

- **URL — REQUIRED.** A single web resource to be added to the bookmarks.

## Success criteria

- The bookmark entry MUST be added to exactly one `index.adoc` file under
  `src/modules/ROOT/pages/`.

- The target `index.adoc` file MUST contain a valid AsciiDoc bullet in the form
  `* https://.../[Label]` that includes the new URL.

- The added line MUST be verified by reading the modified file back and
  confirming the URL is present and the link syntax is well-formed.

- You MUST modify only the selected `index.adoc` and, if needed, the
  navigation file `src/modules/ROOT/nav.adoc`.

## Instructions

1.  Fetch and read the supplied URL. Extract the page title and a short 
    description.

2.  Choose a link label that matches the project's style. Prefer the site or 
    product name, with a brief qualifier only if the name alone is ambiguous.

3.  Find the most appropriate category for the resource.

    Search `src/modules/ROOT/pages/` and read candidate `index.adoc` files
    until the best fit is clear. It is RECOMMENDED you start at the root
    `src/modules/ROOT/pages/index.adoc` and traverse the directory tree from
    there. At each index, identify the next best internal link and follow to
    the next directory down.

    Prefer leaf category pages over their parent pages. If the resource does 
    not fit any existing subcategory but fits the parent category, use the 
    parent page. Alternatively, consider adding the bookmark to a new category
    under an existing parent category.

    If you have low confidence that your chosen category is the best fit, or if
    there are two or more good candidates, prompt the user to make the final
    decision.

4.  Add the bookmark.

    Insert a new bullet line using AsciiDoc link syntax, for example:

    ```adoc
    * https://example.com/[Example]
    ```

    Place the entry in a sensible position within the list — usually
    alphabetically or grouped with similar items, following the pattern of the
    surrounding file.

5.  Update `src/modules/ROOT/nav.adoc` if necessary.

    If the selected page is new or not already present in the navigation tree,
    add an `xref:` entry at the correct nesting depth. 

    Most additions will be to existing pages, in which case no nav change 
    is needed.

6.  Verify the change.

    Read the modified `index.adoc` to confirm the entry is present and the
    AsciiDoc link is well-formed. Report the exact file path and line added.

## Rules

- The repository is organised hierarchically. You MUST prefer the deepest 
  category that matches the resource.

- You MUST use AsciiDoc link syntax for every bookmark entry. Use the form
  `* https://host/path[Label]`.

- You SHOULD add only a single bookmark per invocation, unless the user supplies
  multiple URLs.

- You MUST NOT commit, push, or open a pull request.

## Edge cases

- If the URL is unreachable or returns an error, stop and emit an error 
  message.

- If an identical URL is already listed, stop and report the existing entry
  rather than duplicating it.

- If the target page contains sub-sections, place the bookmark in the section 
  that best matches the resource. If no section is clearly appropriate, add it 
  to the top-level list at the start of the page.

## Examples

- The user says: "Add bookmark: https://caddyserver.com/".

  The agent fetches the page, identifies Caddy as a web server, and adds
  `* https://caddyserver.com/[Caddy]` to
  `src/modules/ROOT/pages/infrastructure/web-servers/index.adoc`.

## Assets

There are no bundled assets for this skill.

## References

- [AsciiDoc syntax reference](https://docs.asciidoctor.org/asciidoc/latest/syntax-quick-reference/).
  Use this to confirm link and list syntax.
