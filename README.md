# slidev-theme-envek

[![NPM version](https://img.shields.io/npm/v/slidev-theme-envek?color=3AB9D4&label=)](https://www.npmjs.com/package/slidev-theme-envek)

A personal theme for [Slidev](https://github.com/slidevjs/slidev).

## Install

Add the following frontmatter to your `slides.md`. Start Slidev then it will prompt you to install the theme automatically.

<pre><code>---
theme: <b>envek</b>
---</code></pre>

Learn more about [how to use a theme](https://sli.dev/themes/use).

## Progress bar

This theme provides a progress bar at the bottom of all slides. Martian figure is used as a progress indicator based on elapsed time since presentation started (compared to planned talk time), while human figure shows progress based on current slide number (compared to total number of slides).

Specify the planned talk time in minutes by adding `talkDurationMinutes` to the frontmatter of your `slides.md`. For example, add `talkDurationMinutes: 30` to the frontmatter to set the planned talk time to 30 minutes.

You can also specify the slide number at which the progress bar should start showing the martian figure by adding `progressBarStartSlide` to the frontmatter of your `slides.md`.

Progress bar can be hidden on a per-slide basis by adding `progressBar: false` to the slide's frontmatter.

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
