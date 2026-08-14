# Add bookmark

Files a single web resource into the bookmarks collection.

Given a URL, the agent fetches the page, works out what the resource is,
traverses the category tree under `src/modules/ROOT/pages/` to find the
best-fit page, and inserts an AsciiDoc bullet into that page's `index.adoc`.

The agent updates `src/modules/ROOT/nav.adoc` only in the rarer case that it had 
to create a new category page. 

The change is left in the working tree. The agent is expressly instructed not 
to commit it.

## Interactivity

This skill is interative and not intended for away-from-keyboard use cases.

Where the agent has low confidence in the category it has chosen, or two or more 
categories are equally good candidates to place the bookmark, the agent is 
instructed to stop and ask the user to make the call. 

The agent may also ask for the URL itself, if that is not clear from the 
conversation.

## How to invoke

> Add bookmark: https://example.com/

> Add this to my bookmarks.

You can steer the placement by naming a category in the prompt, eg.
"add https://example.com/ under infrastructure/web-servers".

## Recommended models

A fast, cheap utility model is sufficient. Nothing here calls for extended
reasoning.
