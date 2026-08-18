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

## Repository settings

This repository's GitHub configuration is defined as code in
`.github/settings.yml`. The configuration is applied by the "Apply Settings"
workflow on pushes to the default branch.

### Setting up the admin token

The workflow needs a token with admin-level access. GitHub's built-in
`GITHUB_TOKEN` is not adequate because it has no `Administration` scope, so
it cannot change repository settings.

1.  Create a fine-grained personal access token in a GitHub account associated
    with a code owner of this repository. Set the following scopes:

    - **Administration**: write

    - **Contents**: read

    - Write access to whichever sections are defined in `settings.yml`, eg.
      **Issues** (for labels, milestones), **Environments**, **Pages**,
      **Actions**, **Actions variables**, **Repository hooks**,
      **Secrets**, **Dependabot secrets**, **Codespaces secrets**,
      **Repository custom properties**, **Secret scanning alerts**. Grant
      only what the file actually uses.

2.  Add it as a repository secret named `ADMIN_TOKEN`.

3.  Trigger the workflow and confirm it applies cleanly.

Until the secret exists, the workflow runs and fails loudly.
