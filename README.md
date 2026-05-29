# Archive Circle Docs

This repository contains the Mintlify documentation site for Archive Circle products.

The docs are intentionally structured as a **multi-product documentation site**. Each product should have its own top-level navigation area and its own folder so unrelated products do not get mixed together.

## Current structure

```text
.
├── docs.json              # Mintlify site configuration and navigation
├── index.mdx              # Eternum landing page for now
├── quickstart.mdx         # Eternum quickstart for now
├── guides/                # Eternum user guides
├── recovery/              # Eternum recovery docs
├── security/              # Eternum security docs
├── operators/             # Eternum operator docs
├── images/                # Shared images
├── logo/                  # Shared logo assets
└── snippets/              # Shared snippets
```

Current product navigation:

- `Eternum` — existing Archive Circle product docs.

Future product docs should be added in product-prefixed folders, for example:

```text
chainlens/
  overview.mdx
  quickstart.mdx
  configuration.mdx
  concepts.mdx
  workflows.mdx
  api-reference.mdx
```

## Information architecture rules

When adding a new product, keep these rules:

1. **Use product-level navigation tabs.**
   - Good: `Eternum`, `ChainLens`, `Future Product`
   - Avoid: mixing product pages into generic global tabs like `Guides`, `Security`, or `Operators`.

2. **Use product-prefixed paths.**
   - Good: `chainlens/quickstart`
   - Avoid: `quickstart-chainlens` or placing unrelated pages in existing Eternum folders.

3. **Keep product-specific links out of the global shell unless clearly labeled.**
   - Good: `Eternum App`, `Eternum GitHub`
   - Avoid: a global `Open app` button that points to only one product without saying which one.

4. **Do not rebrand the whole site as one product.**
   - The site name should remain company-level, currently `Archive Circle`.
   - Product names belong in product tabs, product pages, and product-specific CTAs.

5. **Keep docs factual and implementation-aligned.**
   - Document behavior that exists in the product source repo.
   - Do not invent features, endpoints, chains, configuration variables, or guarantees.
   - Never include real secrets, API keys, private RPC URLs, private keys, or production `.env` values.

## Mintlify configuration

Main config file:

```text
docs.json
```

Important fields:

- `name` — company-level site name. Keep as `Archive Circle` unless the whole docs brand changes.
- `colors` — global Mintlify color theme.
- `logo` and `favicon` — global site assets.
- `navigation.tabs` — top-level product navigation.
- `navigation.global.anchors` — global links shown across the docs site.
- `navbar` — top navbar links and primary CTA.

Example product tab:

```json
{
  "tab": "ChainLens",
  "groups": [
    {
      "group": "Start here",
      "pages": [
        "chainlens/overview",
        "chainlens/quickstart"
      ]
    },
    {
      "group": "Product guide",
      "pages": [
        "chainlens/concepts",
        "chainlens/configuration",
        "chainlens/workflows",
        "chainlens/api-reference"
      ]
    }
  ]
}
```

Every page listed in `docs.json` must exist as a matching `.mdx` file.

## Local development

This repo uses Mintlify.

Install the Mintlify CLI if needed:

```bash
npm install -g mintlify
```

Check the installed version:

```bash
mintlify --version
```

Run local preview from the repository root, where `docs.json` lives:

```bash
mintlify dev --no-open
```

Or open the browser automatically:

```bash
mintlify dev
```

The local preview usually runs at:

```text
http://localhost:3000
```

Note: Some Mintlify documentation refers to the newer `mint` CLI. This repository currently works with the installed `mintlify` CLI.

## Validation checklist before merging

Before merging documentation changes:

- [ ] `docs.json` parses as valid JSON.
- [ ] Every page referenced in `navigation.tabs` exists as a `.mdx` file.
- [ ] New product docs live under a product-prefixed folder.
- [ ] The global site name, navbar, anchors, and CTA are not misleading for a multi-product site.
- [ ] No real secrets, API keys, private RPC URLs, private keys, or production `.env` values are included.
- [ ] Markdown/MDX components are balanced and render locally.
- [ ] Relative links resolve.
- [ ] Code snippets are copy-pasteable and match the product source repo.
- [ ] Product-specific claims are backed by implementation or clearly marked as planned.

Quick JSON/navigation validation:

```bash
python3 - <<'PY'
import json
from pathlib import Path

cfg = json.load(open('docs.json'))
missing = []
for tab in cfg.get('navigation', {}).get('tabs', []):
    for group in tab.get('groups', []):
        for page in group.get('pages', []):
            if isinstance(page, str) and not Path(page + '.mdx').exists():
                missing.append(page)

print('site name:', cfg.get('name'))
print('tabs:', [tab.get('tab') for tab in cfg.get('navigation', {}).get('tabs', [])])
print('missing nav refs:', missing or 'none')
raise SystemExit(1 if missing else 0)
PY
```

## Guidance for AI agents

If you are an AI coding agent working in this repository:

1. Inspect `docs.json` before adding or moving pages.
2. Preserve the multi-product structure.
3. Do not add a new product under another product's tab or folder.
4. Prefer small, reviewable diffs.
5. Add or update navigation in the same change as any new page.
6. Validate navigation references before finishing.
7. If reviewing a PR, block changes that make the global docs shell product-specific again.
8. If source behavior is unclear, state the uncertainty instead of inventing documentation.
9. Do not commit generated build artifacts, local caches, or dependency folders.
10. Do not modify logo or branding assets unless explicitly asked.

## Publishing

Mintlify deploys from the default branch after changes are pushed and the Mintlify GitHub integration is configured for the repository.

For pull requests, prefer reviewing the rendered preview if available. If the Mintlify deployment check is skipped, run local validation and preview manually.

## Useful links

- Mintlify docs: https://mintlify.com/docs
- Mintlify CLI package: https://www.npmjs.com/package/mintlify
- Archive Circle GitHub organization: https://github.com/Archive-Circle
