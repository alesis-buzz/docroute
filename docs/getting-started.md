# Getting started

DocRoute turns a JSON definition plus a folder of Markdown files into a static
documentation site. There is no build step, no bundler and no framework.

## Install

Add the stylesheet and the script to any HTML page:

```html
<link rel="stylesheet" href="docroute.css">
<div id="docroute"></div>
<script src="docroute.js"></script>
<script>
  docroute.InitDocs("docs.json");
</script>
```

If `docroute.css` is missing, DocRoute injects its own copy of the styles, so this
also works:

```html
<div id="docroute"></div>
<script src="docroute.js" data-docroute="docs.json"></script>
```

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
