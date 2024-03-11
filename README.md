# slidev-theme-envek

[![NPM version](https://img.shields.io/npm/v/slidev-theme-envek?color=3AB9D4&label=)](https://www.npmjs.com/package/slidev-theme-envek)

A personal theme for [Slidev](https://github.com/slidevjs/slidev).

## Install

Add the following frontmatter to your `slides.md`. Start Slidev then it will prompt you to install the theme automatically.

<pre><code>---
theme: <b>envek</b>
---</code></pre>

Learn more about [how to use a theme](https://sli.dev/themes/use).

## Layouts

This theme provides the following layouts:


### Footnote

```
---
layout: footnote
footnote-class: text-sm
---

Main slide content

::footnote::

Footnote
```

## Components

This theme provides the following components:

### QRCode

```html
<qr-code
  url="https://github.com/Envek/slidev-theme-envek" 
  caption="Envek's theme for Sli.dev"
  class="w-36 absolute bottom-10px right-10px"
/>
```

## Contributing

- `npm install`
- `npm run dev` to start theme preview of `example.md`
- Edit the `example.md` and style to see the changes
- `npm run export` to generate the preview PDF
- `npm run screenshot` to generate the preview PNG
