# Hiwonder Docs

This repository contains the Hiwonder VitePress documentation portal.

Production URL:

```text
https://wiki.hiwonder.com/
```

The site is built with VitePress `base: '/'`, so sidebar links use root anchors such as:

```text
https://wiki.hiwonder.com/#raspberry-pi
```

## Local Development

```bash
npm ci
npm run docs:dev
```

## Build

```bash
npm run docs:build
npm run docs:stage-main
```

The staged static files are generated in:

```text
projects/Hiwonder-docs/en/latest/
```

Nginx maps `wiki.hiwonder.com` root paths to that staged GitHub Pages directory.
