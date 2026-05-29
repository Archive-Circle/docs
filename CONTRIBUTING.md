# Contributing to Archive Circle Docs

Thank you for helping improve Archive Circle documentation.

This repository is a **multi-product Mintlify docs site**. Contributions should keep the docs clear for users, maintainers, and AI coding agents.

## What belongs here

Accepted contribution types:

- Fixing typos, broken links, unclear wording, or stale screenshots.
- Updating existing product docs to match implementation changes.
- Adding new pages for an existing product.
- Adding a new product documentation section.
- Improving `docs.json` navigation, metadata, theme, or shared docs structure.
- Updating contributor or AI-agent guidance.

Do not include:

- Real secrets, API keys, private RPC URLs, private keys, credentials, or production `.env` values.
- Generated build artifacts, dependency folders, caches, or local preview output.
- Product claims that are not implemented or verified.
- Unrelated product docs inside another product's folder or navigation tab.

## Repository structure

Main files:

```text
docs.json          # Mintlify site config and navigation
README.md          # Project overview and multi-product docs rules
CONTRIBUTING.md    # Human contributor workflow
AGENTS.md          # Instructions for AI coding agents
development.mdx    # Local development page shown in the docs site
```

Current product docs:

```text
index.mdx          # Eternum landing page for now
quickstart.mdx     # Eternum quickstart for now
guides/            # Eternum user guides
recovery/          # Eternum recovery docs
security/          # Eternum security docs
operators/         # Eternum operator docs
```

New products should use product-prefixed folders:

```text
chainlens/
  overview.mdx
  quickstart.mdx
  configuration.mdx
  concepts.mdx
  workflows.mdx
  api-reference.mdx
```

## Multi-product navigation rules

When adding or reorganizing docs:

1. Use **product-level tabs** in `docs.json`.
   - Good: `Eternum`, `ChainLens`, `Future Product`
   - Avoid: mixing unrelated products into global tabs like `Guides`, `Security`, or `Operators`.

2. Use **product-prefixed page paths**.
   - Good: `chainlens/quickstart`
   - Avoid: `quickstart-chainlens` or placing ChainLens content under `guides/` for Eternum.

3. Keep the global shell company-level.
   - Site name should remain `Archive Circle` unless the whole brand changes.
   - Product-specific CTAs must be clearly labeled, for example `Eternum App`.

4. Update navigation with page additions.
   - If a page is added and should be discoverable, add it to `docs.json` in the same PR.
   - Every `docs.json` page reference must have a matching `.mdx` file.

## Local development

Install the Mintlify CLI:

```bash
npm install -g mintlify
```

Check the version:

```bash
mintlify --version
```

Run local preview from the repository root:

```bash
mintlify dev --no-open
```

Or open the browser automatically:

```bash
mintlify dev
```

The preview usually runs at:

```text
http://localhost:3000
```

Note: Some Mintlify docs refer to the newer `mint` command. This repository currently uses the `mintlify` CLI command.

## Contribution workflow

### Small edits on GitHub

For typo fixes or small wording changes:

1. Open the file on GitHub.
2. Click the pencil icon.
3. Make the edit.
4. Open a pull request.

### Local development workflow

For navigation, structural, or multi-page changes:

1. Clone the repo.
2. Create a branch from latest `main`.
3. Make your changes.
4. Validate `docs.json` and navigation references.
5. Preview with `mintlify dev --no-open` or `mintlify dev`.
6. Commit with a clear message.
7. Open a pull request.

Suggested branch names:

```text
docs/fix-broken-link
docs/add-chainlens-pages
docs/update-navigation
```

Suggested commit format:

```text
docs: add ChainLens quickstart
docs: fix Eternum recovery links
docs: update multi-product navigation
```

## Validation before opening a PR

Run this from the repository root:

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

Then preview locally:

```bash
mintlify dev --no-open
```

Before requesting review, check:

- [ ] `docs.json` parses.
- [ ] All nav references resolve to existing `.mdx` files.
- [ ] New product pages live under a product-prefixed folder.
- [ ] Navigation uses product-level tabs.
- [ ] Global navbar/anchors/CTA are not misleading for a multi-product site.
- [ ] MDX components are closed and render locally.
- [ ] Links are correct.
- [ ] Code samples are copy-pasteable.
- [ ] Product behavior matches the source implementation.
- [ ] No secrets or production values are included.

## Writing guidelines

- Use active voice: "Run the command" instead of "The command should be run".
- Address the reader directly with "you".
- Keep sentences concise.
- Lead with the goal before implementation details.
- Use consistent product terminology.
- Use sentence case for headings.
- Use code formatting for commands, file paths, environment variables, endpoints, and code identifiers.
- Use bold text for UI labels, for example **Settings**.
- Prefer examples over abstract descriptions.
- State limitations and assumptions clearly.

## PR expectations

A good docs PR should explain:

- What product or area changed.
- Whether the change is content-only, navigation-only, or structural.
- What source behavior was checked.
- How the docs were validated.
- Any assumptions, gaps, or follow-up work.

Maintainers may request changes when:

- A new product is added without product-level navigation.
- Product-specific branding leaks into the global docs shell.
- Navigation points to missing pages.
- The docs claim behavior that is not implemented.
- Secrets or production values are included.
- The PR mixes unrelated products or unrelated cleanup.

## Useful references

- Project overview: `README.md`
- AI-agent instructions: `AGENTS.md`
- Local development docs page: `development.mdx`
- Mintlify docs: https://mintlify.com/docs
- Mintlify CLI package: https://www.npmjs.com/package/mintlify
