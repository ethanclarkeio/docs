# DGrid AI Docs

This repository contains the Mintlify documentation site for **DGrid AI**, a decentralized AI network that connects AI model supply, developer demand, routing, verification, and on-chain settlement.

The docs cover DGrid AI Gateway, model API usage, AI Arena, Dori, DClaw, the `$DGAI` token, Genesis Premium, x402 payments, and ecosystem resources.

## Documentation Languages

| Locale | Path | Notes |
| --- | --- | --- |
| English | `/` | Default source language |
| 繁體中文（台灣） | `/zh-Hant` | Traditional Chinese localization |
| 한국어 | `/ko` | Korean localization |
| Français | `/fr` | French localization |
| Español | `/es` | Spanish localization |

Navigation, localized labels, and page groups are configured in [`docs.json`](./docs.json).

## Project Structure

```text
.
├── docs.json                 # Mintlify site configuration and navigation
├── *.mdx                     # English documentation pages
├── ai-gateway/               # AI Gateway guides
├── api-reference/            # Model API reference pages and OpenAPI config
├── ai-arena/                 # AI Arena documentation
├── premium/                  # Genesis Premium documentation
├── x402/                     # x402 payment documentation
├── zh-Hant/                  # Traditional Chinese docs
├── ko/                       # Korean docs
├── fr/                       # French docs
└── es/                       # Spanish docs
```

Each page is an MDX file with YAML frontmatter. Most pages should include `title`, `sidebarTitle`, and `description`.

## Local Development

Install the Mintlify CLI:

```bash
npm install -g mint
```

Run the local preview from the repository root:

```bash
mint dev
```

Open `http://localhost:3000` to preview the docs.

If the preview behaves unexpectedly, update the CLI:

```bash
mint update
```

## Contribution Guidelines

- Keep English pages as the source of truth unless a change is localization-only.
- When adding or renaming a page, update `docs.json` for every affected locale.
- Keep localized page paths aligned with the English source path.
- Preserve Mintlify components such as `<Info>`, `<Frame>`, `<Card>`, `<Tabs>`, and API playground blocks.
- Use concise, active documentation language.
- Bold UI labels with Markdown or `<strong>`, and use backticks for file names, commands, paths, parameters, and code.
- In CJK and Korean prose, prefer `<strong>...</strong>` when bold text touches surrounding characters. This avoids raw `**` being rendered in some MDX contexts.

## Pull Request Checklist

Before opening a PR:

- Run `mint dev` and spot-check changed pages locally.
- Check that navigation labels and sidebar titles render correctly in each changed locale.
- Confirm no local AI assistant files, caches, secrets, or preview output are included.
- For localization changes, compare the translated page against the English source for missing sections, broken links, and untranslated navigation text.

## 中文快速指南

這是 DGrid AI 的 Mintlify 文件倉庫，包含英文預設文件，以及繁體中文（台灣）、韓語、法語和西班牙語本地化內容。

常用指令：

```bash
npm install -g mint
mint dev
```

本機預覽網址是 `http://localhost:3000`。提交 PR 前，請確認 `docs.json` 中的導覽、各語言目錄下的頁面、頁首和側邊欄標題都已同步更新。
