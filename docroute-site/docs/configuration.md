# Configuration

The definition passed to `docroute.InitDocs` can be an object, a bare array of
sections, a JSON string or the URL of a JSON file.

## Object form

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

## Keys

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `name` | string | `Documentation` | Brand shown in the sidebar. |
| `logo` | string | none | Logo image URL. Relative paths resolve against `base_url`. Replaces the initial letter. `logoUrl` / `logo_url` are aliases. |
| `favicon` | string | none | Favicon URL applied at runtime. `icon` is accepted as an alias. |
| `footer` | string \| object \| `false` | Powered-by link | Sidebar footer. `false` hides it, a string sets custom text, `{ "text": "...", "url": "..." }` sets text plus link. |
| `footerUrl` | string | none | Link used when `footer` is a string. `footer_url` is accepted as an alias. |
| `primary` | string | theme text color | Accent color for links, active items and focus states. Any CSS color except named colors. |
| `theme` | string | `dark` | `dark` or `light`. A user toggle is stored in `localStorage` and wins over this value. |
| `base_url` | string | location of the JSON file | Directory used to resolve relative `md_url` values. |
| `sections` | array | required | Sidebar content. `items` is accepted as an alias. |

## Sections

A section is a group with a title and a list of entries:

```json
{ "type": "section", "name": "Guide", "pages": [] }
```

Sections can be nested. `pages` is the conventional key, `items` also works.

## Pages

```json
{ "type": "page", "name": "Getting started", "md_url": "getting-started.md" }
```

- `name` is used in the sidebar, the top bar and the document title.
- `md_url` is fetched at runtime. Relative paths resolve against `base_url`.
- `slug` is optional and auto-generated from `name` for the `#/slug` route.

## Links

```json
{ "type": "link", "name": "GitHub", "url": "https://github.com" }
```

Links are regular anchors and open in a new tab.

## Options

`InitDocs` accepts a second argument:

```js
docroute.InitDocs("docs.json", { target: "#docroute", primary: "#7aa2f7", theme: "light" });
```

Options win over the JSON values. `target` accepts a selector or an element and
defaults to the element with id `docroute`, then `document.body`.

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `target` | string \| element | `#docroute` | Where the documentation is mounted. |
| `primary` | string | JSON `primary` | Accent color. |
| `theme` | string | JSON `theme` | `dark` or `light`. |
| `logo` | string | JSON `logo` | Logo image URL. |
| `favicon` | string | JSON `favicon` | Favicon URL. |
| `footer` | string \| object \| `false` | JSON `footer` | Sidebar footer override. |
| `footerUrl` | string | JSON `footerUrl` | Link used when `footer` is a string. |
| `searchContent` | boolean | `true` | Index page Markdown so search matches body text, not just titles. |
| `shortcuts` | boolean | `true` | `/` focuses search, `Esc` clears it, `←`/`→` move between pages. |
| `highlight` | boolean | `true` | Syntax highlight code fences with highlight.js. |
| `copy` | boolean | `true` | Copy button on every code block. |

With the self-initializing script tag the same options are attributes
(`data-logo`, `data-favicon`, `data-footer`, `data-footer-url`,
`data-search-content="false"`, `data-shortcuts="false"`):

```html
<script src="docroute.js" data-docroute="docs.json" data-primary="#7aa2f7" data-theme="light" data-highlight="false" data-copy="false"></script>
```

## Branding

```json
{
  "name": "DocRoute",
  "logo": "logo.svg",
  "favicon": "favicon.svg",
  "footer": { "text": "MIT · Built with DocRoute", "url": "https://github.com" }
}
```

- `logo` replaces the initial letter in the sidebar brand.
- `favicon` updates `<link rel="icon">` at runtime.
- `"footer": false` hides the sidebar footer.

## Search

Search always matches section and page titles. With `searchContent` (default)
every `md_url` is fetched once in the background and normalized, so body
matches also keep the page visible while typing. The open page is indexed on
load. Use `{ searchContent: false }` for title-only search.

## Shortcuts

Available unless `{ shortcuts: false }` is passed. `←`/`→` are ignored while
typing in inputs, textareas or editable elements.
