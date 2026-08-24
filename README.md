# Wix Developer Docs on Mintlify — POC site

A [Mintlify](https://mintlify.com) rendering of the Wix developer documentation, generated
from the public Wix docs portal.

This repository holds **only the generated site**. The generator that produces it lives
separately.

- **415 APIs**, 4,924 pages
- Every operation shows both the **REST** and **SDK** view, each built from its own schema —
  field names, argument shapes, return types and even which pages exist differ between the two
- Sections, categories, ordering and slugs all mirror the docs portal's own menu

## Wix look and feel

- API pages are two columns: the description and reference text on the left, a sticky dark
  examples card alongside them on the right, and no table of contents — the layout dev.wix.com
  uses
- The REST/SDK switch sits directly under the page title as a pair of pill buttons
- Examples are chosen from a real dropdown showing each example's name and description
- HTTP verb chips inline with the page title, and coloured letter tiles in the menu
  (purple G, green P, blue P, red D)
- Permission scopes as pill badges; the scope id is each badge's tooltip
- 260px sidebar with a per-depth indent, and a flat table of contents on article pages

## Run it locally

```bash
npx mint@latest dev        # http://localhost:3000
```

Use a current CLI: 4.2.198 has no `validate` subcommand and rejects a valid `logo.href`.

```bash
npx mint@latest validate
npx mint@latest broken-links
```

## Layout

```
docs.json                          theme + navigation
index.mdx                          landing page
style.css                          Wix Design System tokens + the two-column layout
api-reference/
├── introduction.mdx               explains the REST/SDK split to readers
└── <section>/<category>/…/<api>/   415 APIs, mirroring the portal tree
kb/authentication/                 15-page headless-docs port
```

Page order within an API is overview → articles → object → methods → events, with
Introduction first and Sample Flows second. Each page kind carries its own sidebar icon.

## Known gaps

- Mintlify's `<Tabs>` sync by title **within a page** only, so the REST/SDK choice is not
  persistent across pages the way dev.wix.com's `?apiView=SDK` selector is. This is the main
  fidelity gap.
- Clicking the REST/SDK switch scrolls the page up to it. Mintlify sets `location.hash`, so the
  browser scrolls to that element; there is no CSS hook and MDX cannot run JS.
- The top bar is 112px against Wix's 48px: Mintlify splits branding and the tab strip across
  two rows and derives the height from their content, so there is no container to set.
- No "Try It Out" playground, no per-section parameter filtering.
- 17 broken internal links, all upstream-authored relative paths written for dev.wix.com's URL
  layout, plus two relative images.
- 440 portal articles that hang off a section or category rather than an API are not generated
  yet; they need nav placement outside any API group.

All content is generated from the public Wix documentation portal.
