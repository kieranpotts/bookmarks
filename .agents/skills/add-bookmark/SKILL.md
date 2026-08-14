---
name: add-bookmark
description: >-
  Add a web resource to the bookmarks collection, filing it under the
  best-fit category page. Use when the user says something like "add
  bookmark", "add this to my bookmarks", or supplies a URL to be filed
  away. Do not use it to reorganize existing categories, or to publish the
  site.
compatibility: >-
  requires Read, Edit, Write, Glob, Grep, WebFetch
license: CC0-1.0
---

# Add bookmark

Follow a URL supplied by the user, analyze the resource, and add a link to it
under the most appropriate category of the bookmarks collection. Do not commit
the change, unless the user expressly asks you to.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **URL — REQUIRED.** A single web resource to be added to the bookmarks. Take
  it from the user's prompt, or from the resource under discussion in the
  surrounding context.

- **Label — OPTIONAL.** The link text. Derive it from the page's own title or
  product name when the user does not supply one.

- **Category — OPTIONAL.** The page the bookmark is filed under, given as a
  path below `src/modules/ROOT/pages/`. Discover it by traversal when the user
  does not name one.

## Success criteria

- Exactly one `index.adoc` file under `src/modules/ROOT/pages/` MUST carry a
  new bullet of the form `* https://host/path[Label]` with the supplied URL.

- Re-reading the modified file MUST show the URL present and the bullet
  well-formed, so the change is confirmed rather than assumed.

- Where a new category page was created, `src/modules/ROOT/nav.adoc` MUST carry
  an `xref:` entry for it at the correct nesting depth. Where no page was
  created, `nav.adoc` MUST be left untouched.

- Files outside `src/modules/ROOT/` MUST NOT be modified, and the change MUST
  be left uncommitted in the working tree — no commit, push, or pull request.

- Your final report MUST name the file path written to and the line added, so
  the user can review the placement without searching for it.

## Instructions

1.  Fetch and read the supplied URL. Extract the page title and enough of the
    content to judge what the resource is for.

2.  Choose a link label. Prefer the site or product name alone, adding a brief
    qualifier only where the name by itself is ambiguous.

3.  Find the most appropriate category for the resource. Start at the root page, 
    `src/modules/ROOT/pages/index.adoc`, and traverse down the directory tree. 
    At each index, identify the next best internal link and follow it to the 
    next directory down. Read candidate `index.adoc` files until the best fit 
    is clear.

    Prefer leaf category pages over their parents. But where the resource does 
    not fit any existing subcategory but does fit the parent category, use the
    parent page. Where it fits neither, you MAY create a new category page
    under an existing parent.

4.  Where you have low confidence in the chosen category, or two or more
    candidates are equally good, prompt the user to make the final decision
    rather than guessing.

5.  Add the bookmark as a new bullet, using AsciiDoc link syntax:

    ```adoc
    * https://example.com/[Example]
    ```

    Place the entry in a sensible position within the list — usually
    alphabetically, or grouped with similar items — following the pattern of
    the surrounding file.

6.  Update `src/modules/ROOT/nav.adoc` only if you created a new page. Add an
    `xref:` entry at the correct nesting depth, matching the surrounding
    entries. Most additions land on an existing page, in which case the
    navigation needs no change.

7.  Verify the change by reading the modified `index.adoc` back. Report the
    exact file path and the line added.

## Rules

- You MUST file each bookmark under the deepest category that matches the
  resource. The collection is organized hierarchically, and a link parked in a
  parent category is effectively lost among broader entries.

- You MUST use AsciiDoc link syntax of the form `* https://host/path[Label]`,
  because the published site renders these pages through Antora and a bare URL
  produces no label.

- You SHOULD add only a single bookmark per invocation, unless the user
  supplies several URLs at once.

- You MUST NOT reorder, reword, or reformat existing entries. Adding a
  bookmark is an insertion, and unrelated churn makes the change hard to
  review.

## Edge cases

- The URL is unreachable, or returns an error status. Stop and report the 
  failure. Do not file a bookmark for a page you could not read.

- The same URL is already listed somewhere in the collection. Stop and report 
  the existing entry and its file path, rather than duplicating it.

- The target page is divided into sub-sections. Place the bookmark in the 
  section that best matches the resource. Where no section is clearly 
  appropriate, add it to the top-level list at the start of the page.

## Examples

- The user says: "Add bookmark: https://caddyserver.com/". You fetch the page, 
  identify Caddy as a web server, and add `* https://caddyserver.com/[Caddy]` 
  to the "web servers" category page under `src/modules/ROOT/pages/`.
