# site-c

Sandbox site repo for store `site-c-store`. Part of the multi-site theme CI/CD
sandbox — the shared code lives in
[`shopify-common`](https://github.com/chinmay-garge/shopify-common).

## What this repo is for

It is git-connected to this store's **Staging** theme, so it mirrors **content**:
anything edited in the Shopify theme editor arrives here as a commit.

It is **not** where code changes are made. `.liquid` files here are overwritten
by the next deploy from `shopify-common`. To change code, change it there.

| Lane | Owner | Path |
|---|---|---|
| CODE | `shopify-common` | `sections/`, `snippets/`, `layout/`, `assets/*.vbt.*` |
| CONTENT | editors, in the Shopify admin | `templates/*.json`, `config/settings_data.json`, `locales/*.json` |

## Drift detection

`.github/workflows/drift-detection.yml` runs on every push to `main`, diffs the
code files here against `shopify-common/theme`, and maintains a single open
`drift` issue — updating it while drift persists and closing it once resolved.

Requires:

- variable `TARGET_REMOTE_REPO` = `chinmay-garge/shopify-common`
- secret `ACCESS_PAT` — PAT with `repo` scope, to read the shared repo

## Fixtures owned by this site

**`sections/sandbox-footer-cta.liquid` — divergent copy with an extra setting.**
This site's version adds a `hide_background` schema option that the shared repo
does not have. It stands in for the real decision that the shared repo wins and
site-specific extras get dropped rather than merged back.

Two things to confirm when the next code deploy overwrites it:

1. `hide_background` disappears from the theme editor, and
2. content JSON that still sets `hide_background` keeps the now-orphaned key
   **without breaking rendering** — the thing people worry about when removing a
   setting that live content might reference.
