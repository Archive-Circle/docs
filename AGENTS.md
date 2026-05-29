# AI agent instructions for Archive Circle Docs

This repository contains the Mintlify documentation site for Archive Circle products.

Treat this as a **multi-product documentation repository**. Your job is to preserve clear product separation, factual documentation, and valid Mintlify navigation.

## Project facts

- Docs platform: Mintlify
- Config file: `docs.json`
- Page format: `.mdx` with YAML frontmatter
- Site name: `Archive Circle`
- Top-level navigation should be product-level.
- Current product tab: `Eternum`
- Local CLI command used in this repo: `mintlify`

Do not assume the newer `mint` command is available. Prefer:

```bash
mintlify dev --no-open
```

## Core rules

1. Inspect `docs.json` before adding, moving, or deleting pages.
2. Keep product docs separated by product tab and product folder.
3. Do not add a new product under another product's navigation tab.
4. Do not mix unrelated product pages into generic tabs such as `Guides`, `Security`, or `Operators`.
5. Keep global site shell company-level, not product-specific.
6. Add or update `docs.json` navigation in the same change as new pages.
7. Every page referenced in `docs.json` must exist as a `.mdx` file.
8. Keep diffs small and reviewable.
9. Do not commit build artifacts, dependency folders, caches, or local preview output.
10. Do not modify logos, favicon, or branding assets unless explicitly asked.

## Product structure

Existing Eternum docs currently use root-level and section folders:

```text
index.mdx
quickstart.mdx
guides/
recovery/
security/
operators/
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

Preferred `docs.json` shape for products:

```json
{
  "navigation": {
    "tabs": [
      {
        "tab": "Eternum",
        "groups": []
      },
      {
        "tab": "ChainLens",
        "groups": []
      }
    ]
  }
}
```

## Content standards

Write docs that are accurate, concise, and implementation-aligned.

- Use active voice and second person.
- Use sentence case for headings.
- Use code formatting for commands, files, paths, env vars, endpoints, and code symbols.
- Use bold for UI labels.
- Include examples when they help users complete a task.
- State assumptions, limitations, and prerequisites explicitly.
- Prefer concrete steps over vague descriptions.

Do not:

- Invent product features, API endpoints, environment variables, supported chains, or guarantees.
- Include real secrets, API keys, private RPC URLs, private keys, credentials, or production `.env` values.
- Present planned behavior as current behavior.
- Copy large source files into docs.
- Add marketing claims that are not backed by product behavior.

## Mintlify/MDX rules

- Pages should usually have frontmatter with `title`, `description`, and optionally `icon`.
- Check JSX-like components for matching open/close tags.
- Keep code fences valid and language-tagged where useful.
- Escape content that MDX might parse incorrectly.
- Use absolute docs links carefully; prefer repo-consistent paths.
- If a page is removed or renamed, update all references in `docs.json` and nearby links.

## Validation commands

Run this from the repository root after navigation changes:

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

Preview locally when possible:

```bash
mintlify dev --no-open
```

If the command hangs because it starts a long-running server, that is expected. Check the server output or stop it after confirming it starts.

## PR review policy for agents

Request changes for PRs that:

- Add an unrelated product without a product-level tab.
- Add product docs outside a product-prefixed folder.
- Rebrand the whole site as one product.
- Leave global navbar/anchors/CTA misleading for a multi-product site.
- Break `docs.json` parsing or navigation references.
- Include secrets or production values.
- Claim behavior not supported by the source implementation.
- Mix unrelated structural changes with content changes without explanation.

Comment but do not necessarily block for:

- Minor wording improvements.
- Small style inconsistencies.
- Missing examples when the docs are still correct.
- Non-critical IA improvements that can be follow-up work.

## Expected final response from agents

When finishing a docs task, summarize:

- Files changed.
- Navigation changes.
- Validation performed.
- Any remaining risks or follow-up work.

Keep the summary concise and operational.
