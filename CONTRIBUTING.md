# Contributing

## Non-existent / example URLs in bookmark descriptions

A bookmark's description text sometimes needs to mention a URL that isn't a
real, dereferenceable destination — e.g. a local development address like
`http://localhost:4566`, or another placeholder/example URL. AsciiDoc
autolinks any bare URL it finds, including inside backtick-quoted
(monospace) text, so writing:

```asciidoc
Point your client at `http://localhost:4566`.
```

still renders as a live hyperlink (`<a href="http://localhost:4566">`).
When the site is built and link-checked, that "link" gets crawled like any
other same-site link, fails to resolve, and breaks the build.

To reference such a URL as plain/code text without it being autolinked,
wrap it in `pass:[]` inside the backticks:

```asciidoc
Point your client at `pass:[http://localhost:4566]`.
```

This still renders as inline code showing the URL, but Asciidoctor's
autolink macro no longer processes it, so it isn't treated as a real link.

Apply this whenever a bookmark description mentions a non-public,
non-existent, or otherwise unreachable URL as example text.
