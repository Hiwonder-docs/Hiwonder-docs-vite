# Hiwonder Docs

This repository contains the Hiwonder VitePress documentation portal.

## Local Development

Install dependencies and start the local documentation server:

```bash
npm ci
npm run docs:dev
```

Build the production site:

```bash
npm run docs:build
npm run docs:stage-main
```

The staged static files are generated in:

```text
projects/Hiwonder-docs/en/latest/
```

## GitHub Pages

For the `Hiwonder-docs-vite` repository, GitHub Pages should use:

```text
Source: Deploy from a branch
Branch: main
Folder: / (root)
```

Direct GitHub Pages URL:

```text
https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/en/latest/
```

Production URL through the Hiwonder domain:

```text
https://wiki.hiwonder.com/projects/Hiwonder-docs/en/latest/
```

Root entry URL:

```text
https://wiki.hiwonder.com/
```
