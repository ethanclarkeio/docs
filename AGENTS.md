# Agent Instructions

These instructions apply to AI coding assistants and automated contributors working in this repository.

## Project Overview

- This is the Mintlify documentation site for **DGrid AI Docs**.
- The site documents DGrid AI Gateway, Model API, AI Arena, Dori, DClaw, `$DGAI`, Genesis Premium, x402 payments, and ecosystem resources.
- `docs.json` controls the site theme, navigation, language configuration, redirects, API playground settings, navbar links, and tabs.
- Documentation pages are MDX files with YAML frontmatter.

## Language Structure

- English is the default source language at the repository root.
- Localized documentation lives in:
  - `zh-Hant/` for Traditional Chinese (Taiwan)
  - `ko/` for Korean
  - `fr/` for French
  - `es/` for Spanish
- Keep localized paths aligned with the English source paths whenever possible.
- When adding, moving, or renaming a page, update `docs.json` for every affected language.

## Content Style

- Use active voice and concise sentences.
- Prefer second person when instructing readers.
- Use sentence case for headings unless product naming requires otherwise.
- Bold UI labels: Click **Settings**.
- Use backticks for file names, commands, paths, parameters, endpoints, and code.
- Keep product names consistent: `DGrid AI`, `DGrid AI Gateway`, `DGrid AI Arena`, `Dori`, `DClaw`, `$DGAI`, `Genesis Premium`, `Proof of Quality (PoQ)`, and `x402`.
- Do not invent product behavior. If source material is unclear, leave a concise note or ask for confirmation.

## MDX and Mintlify Rules

- Preserve YAML frontmatter keys such as `title`, `sidebarTitle`, and `description`.
- Preserve Mintlify components and their props, including `<Info>`, `<Frame>`, `<Card>`, `<Tabs>`, `<Accordion>`, and API reference blocks.
- Do not translate code identifiers, endpoint paths, model names, JSON keys, or command-line flags.
- Do not wrap code blocks in translated punctuation that changes copy-paste behavior.
- In CJK and Korean prose, avoid Markdown bold that directly touches surrounding characters, such as `中文**粗體**文字` or `한국어**굵게**문장`. Use `<strong>粗體</strong>` / `<strong>굵게</strong>` in those cases.
- Keep links locale-aware. Localized pages should link to localized paths when the target exists.

## Localization Rules

- Translate reader-facing prose, headings, descriptions, sidebar titles, navigation labels, and UI explanations.
- Preserve technical terms when translation would reduce clarity. Add a localized explanation if helpful.
- Keep numeric values, token symbols, endpoint names, and protocol names unchanged.
- For Traditional Chinese, use Taiwan-style Traditional Chinese wording. Avoid Simplified Chinese characters.
- For Korean, use polite documentation style and keep product names in English.
- For French and Spanish, use clear professional documentation tone rather than overly literal translation.

## Local Development

Use the Mintlify CLI from the repository root:

```bash
mint dev
```

Preview the site at `http://localhost:3000`.

If the CLI is missing, install it:

```bash
npm install -g mint
```

## Verification Before PRs

- Run or reuse a local Mintlify preview and spot-check changed pages.
- Verify `docs.json` parses as valid JSON.
- Check changed localized pages for missing frontmatter, broken local links, untranslated navigation text, and raw Markdown artifacts such as visible `**`.
- Do not commit local AI assistant state, local settings, caches, `.env` files, `node_modules`, `.mintlify`, or OS files.

## Repository Hygiene

- Use `.gitignore` for local tooling output and secrets.
- Do not delete or overwrite user changes unless explicitly requested.
- Keep PRs focused. Documentation content changes, navigation changes, and tooling cleanup should be easy to review.
