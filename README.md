# docroute

DocRoute is a simple, lightweight documentation system for static sites, built
entirely around Markdown.

- One JSON file defines the navigation.
- Markdown files are fetched and rendered at runtime.
- No build step, no framework, no dependencies to install.
- Sidebar search filters by title and page content as you type.
- Keyboard shortcuts: `/` focuses search, `Esc` clears it, `←`/`→` move between pages.
- GitHub-style callouts (`> [!NOTE]`) rendered as cards.
- Logo, favicon and footer configurable from JSON.
- Skeleton loading state while a page is fetched.
- Syntax highlighting for code fences, loaded on demand.
- Copy button on every code block.
- Dark monochrome theme by default, light theme optional, accent color
  configurable.

## Usage

```html
<link rel="stylesheet" href="docroute.css">
<div id="docroute"></div>
<script src="docroute.js"></script>
<script>
  docroute.InitDocs("docs.json");
</script>
```

Or let the script initialize itself:

```html
<div id="docroute"></div>
<script src="docroute.js" data-docroute="docs.json"></script>
```

`docroute.css` is optional: if it is not found, DocRoute injects its styles.

## Definition

```json
{
  "name": "DocRoute",
  "primary": "#7aa2f7",
  "theme": "dark",
  "base_url": "docs/",
  "sections": [
    {
      "type": "section",
      "name": "Guide",
      "pages": [
        { "type": "page", "name": "Getting started", "md_url": "getting-started.md" },
        { "type": "link", "name": "GitHub", "url": "https://github.com" }
      ]
    }
  ]
}
```

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `name` | string | `Documentation` | Brand shown in the sidebar. |
| `logo` | string | none | Logo image URL. Relative paths resolve against `base_url`. Replaces the initial letter. |
| `favicon` | string | none | Favicon URL applied at runtime. `icon` is accepted as an alias. |
| `footer` | string \| object \| `false` | Powered-by link | Sidebar footer. `false` hides it, a string sets custom text, `{ "text", "url" }` sets text plus link. |
| `footerUrl` | string | none | Link used when `footer` is a string. |
| `primary` | string | text color | Accent color for links and active items. |
| `theme` | string | `dark` | `dark` or `light`. The user toggle is remembered. |
| `base_url` | string | location of the JSON | Directory for relative `md_url` values. |
| `sections` | array | required | `items` is accepted as an alias, sections can nest. |

`InitDocs` accepts an array, an object, a JSON string or a URL, plus an options
object:

```js
docroute.InitDocs("docs.json", { target: "#docroute", primary: "#7aa2f7", theme: "light" });
```

Options win over the JSON values. `target` accepts a selector or an element and
defaults to the element with id `docroute`, then `document.body`. Pass
`highlight: false` to disable code highlighting and `copy: false` to remove the
copy buttons. `logo`, `favicon`, `footer`/`footerUrl`, `searchContent: false`
and `shortcuts: false` are also supported (same names as `data-*` attributes).

## Search

Search matches titles first. With `searchContent` enabled (default), page
Markdown is prefetched in the background and indexed, so body matches also
keep the page visible.

## Shortcuts

`/ ` focuses search, `Esc` clears it, `←`/`→` move between pages (ignored
while typing). Disable with `{ shortcuts: false }`.

## Callouts

Quotes starting with `> [!NOTE]`, `[!TIP]`, `[!IMPORTANT]`, `[!WARNING]` or
`[!CAUTION]` render as cards. See `docs/markdown.md` for a live example.

## Routing

Pages use the hash route `#/slug`, so static hosts (GitHub Pages, Netlify, S3)
need no rewrite rules. Links between Markdown files are intercepted and routed
in place. Slug defaults to `name` lowercased and hyphenated.

## API

| Member | Description |
| --- | --- |
| `docroute.InitDocs(input, options)` | Mounts the documentation. Returns a Promise. |
| `docroute.setTheme("dark" \| "light")` | Switches the theme. |
| `docroute.setPrimary(color)` | Overrides the accent color. |
| `docroute.go(slug)` | Navigates to a page. |
| `docroute.getConfig()` | Returns the normalized configuration. |
| `docroute.version` | Current version. |

## Example

`docroute-site/` is a working example:

```
docroute.js
docroute.css
docroute-site/
  index.html
  docs.json
  CNAME
  docs/
    getting-started.md
    configuration.md
    markdown.md
```

Serve the repo over HTTP (`npx serve .`) and open `/docroute-site/`. To deploy
to GitHub Pages from `main`, publish the `docroute-site/` folder (Settings →
Pages → Deploy from branch → `main` + `/docroute-site`), because every path is
relative.

## Markdown

Rendered with [marked](https://marked.js.org), loaded from jsDelivr the first
time a page renders. If you prefer to pin your own copy, include it before
`docroute.js` and DocRoute will use it. Raw HTML in Markdown is allowed, except for
script tags and event handler attributes, which are stripped.

Code fences are highlighted with [highlight.js](https://highlightjs.org),
loaded from jsDelivr only when a page contains code. Include your own copy
before `docroute.js` to pin a version, or disable highlighting with
`{ highlight: false }` / `data-highlight="false"`. Token colors adapt to the
dark and light themes.

## License

MIT
