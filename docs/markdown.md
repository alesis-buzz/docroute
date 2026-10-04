# Markdown

DocRoute renders GitHub Flavored Markdown through marked. Everything below is
plain Markdown.

## Text

**Bold**, *italic*, ~~strikethrough~~, `inline code` and a
[link to another page](configuration.md).

## Lists

- Unordered item
- Second item
  - Nested item

1. Ordered item
2. Second item

- [x] Task list item
- [ ] Pending task

## Code

```js
docroute.InitDocs("docs.json").then(function (api) {
  console.log(api.version);
});
```

```bash
npx serve .
```

## Quotes

> Documentation is a love letter that you write to your future self.
> Keep it short and keep it true.

## Callouts

> [!NOTE]
> Useful context for the reader.

> [!TIP]
> Press `/` to focus search and `Esc` to clear it.

> [!IMPORTANT]
> Keep page slugs stable, they are part of the URL hash.

> [!WARNING]
> Renaming `md_url` files breaks deep links.

> [!CAUTION]
> Do not publish secrets in Markdown files.

## Tables

| Feature | Supported |
| --- | --- |
| Sidebar navigation | yes |
| Sidebar search | yes |
| Hash routing | yes |
| Dark and light theme | yes |
| Syntax highlighting | yes |
| Copy code button | yes |

## Images

![Placeholder](https://placehold.co/640x200/101013/ececef?text=DocRoute)

## Rules

---

That is the whole surface. No plugins, no extensions, no configuration files.
