# Getting started

DocRoute turns a JSON definition plus a folder of Markdown files into a static
documentation site. There is no build step, no bundler and no framework.

## Requirements

- Any static host or local server (GitHub Pages, Netlify, S3, `npx serve .`).
  Markdown is fetched at runtime, so opening the page from `file://` does not
  work.
- A current browser. `marked` and `highlight.js` are loaded on demand from
  jsDelivr the first time they are needed.

## Install

DocRoute is one script plus one optional stylesheet. You can **vendor** both
files next to your site or load them straight from **jsDelivr** — the two
setups are interchangeable.

### Option A — jsDelivr (nothing to download)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/alesis-buzz/docroute@main/docroute.css">
<div id="docroute"></div>
<script src="https://cdn.jsdelivr.net/gh/alesis-buzz/docroute@main/docroute.js"></script>
<script>
  docroute.InitDocs("docs.json");
</script>
```

- CSS: `https://cdn.jsdelivr.net/gh/alesis-buzz/docroute@main/docroute.css`
- JS: `https://cdn.jsdelivr.net/gh/alesis-buzz/docroute@main/docroute.js`
- `@main` tracks the latest commit. For production, pin a tag or a full
  commit hash (`…/docroute@05f5c6e/docroute.js`) so a push cannot change your
  site.

### Option B — vendored

Download `docroute.js` and `docroute.css` from the
[repository](https://github.com/alesis-buzz/docroute) and keep them next to
your pages:

```html
<link rel="stylesheet" href="docroute.css">
<div id="docroute"></div>
<script src="docroute.js"></script>
<script>
  docroute.InitDocs("docs.json");
</script>
```

Use this when you want zero external requests, offline documentation or your
own cache policy.

### Auto-initializing tag

Either source works with the self-initializing tag, where `data-docroute`
points at the definition and the options are attributes:

```html
<div id="docroute"></div>
<script src="docroute.js" data-docroute="docs.json" data-theme="light"></script>
```

```html
<script src="https://cdn.jsdelivr.net/gh/alesis-buzz/docroute@main/docroute.js"
        data-docroute="docs.json" data-theme="light"></script>
```

`docroute.css` is optional in every case: if it is not found, DocRoute
injects its own copy of the styles.

## Layout

```
site/
  index.html          ← the snippet above
  docroute.js         ← only for the vendored option
  docroute.css        ← only for the vendored option
  docs.json           ← navigation definition
  getting-started.md
  configuration.md
```

`docs.json` and the Markdown files can live anywhere: `base_url` in the
definition decides where relative `md_url` values resolve from.

## How it works

1. `InitDocs` loads the JSON definition (a URL, a JSON string or an array).
2. A sidebar is rendered from your sections, pages and links.
3. The first page is loaded. Routing uses the URL hash, so any static host
   (GitHub Pages, Netlify, S3) works without server rewrites.
4. Markdown files are fetched with `fetch` and rendered with
   [marked](https://marked.js.org).

## Next

- [Configuration](configuration.md) explains every JSON key.
- [Markdown](markdown.md) shows what is supported.
