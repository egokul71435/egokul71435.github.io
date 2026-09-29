# egokul71435.github.io

Personal site — [egokul71435.github.io](https://egokul71435.github.io)

Static, no build step, no dependencies. One HTML file plus a `posts/` folder.
Deployed by GitHub Pages from `main` at the repo root.

## Running locally

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

Serve it over HTTP rather than opening `index.html` directly — the blog fetches
`posts/posts.json` at runtime, and `fetch` is blocked on `file://` URLs. The
other pages work either way.

Edits to `index.html` need a hard refresh (`Cmd+Shift+R`) since the browser
caches it; the post files are fetched with `cache: 'no-cache'`, so those pick up
on a normal reload.

## Layout

```
index.html                    everything — markup, CSS, JS
pfp.jpg
gokul-elangovan-resume.pdf
posts/
  posts.json                  post metadata, newest first
  moe-apple-silicon.html      post body
```

## Adding a blog post

Two files, and `index.html` never changes.

1. Write the body as `posts/<slug>.html` — just the prose, no wrapper markup.
   Available: `<p>`, `<h2>`, `<h3>`, `<ul>`, `<ol>`, `<pre><code>`,
   `<blockquote>`, `<hr>`, `<table class="data">`, `<div class="callout">`.
2. Add an entry to the top of `posts/posts.json`:

```json
{
  "slug": "my-slug",
  "title": "Post title",
  "date": "2026-10-14",
  "readTime": "6 min read",
  "category": "ML Systems",
  "tags": ["PyTorch", "RAG"],
  "excerpt": "One or two lines, shown on the blog list.",
  "dek": "Standfirst, shown under the title on the post itself.",
  "link": { "label": "View the repo ↗", "href": "https://..." }
}
```

`category` and `link` are optional. Posts route to `#blog/<slug>`.

To stub a post out before it's written, give it `"draft": true` with just a
`slug`, `title` and `excerpt`. Drafts show a `SOON` badge on the list, aren't
clickable, and need no body file. Remove the flag to publish.

## Theming

A single `--hue` variable drives the whole palette through OKLCH, so the accent
picker retints backgrounds, borders and links together rather than just swapping
a link colour. Presets live in the `PRESETS` array in the script; light mode
darkens the accent so it stays legible on white. Theme and hue persist in
`localStorage`.

Animations use native scroll-driven CSS (`animation-timeline: view()`) with an
`IntersectionObserver` fallback, and are disabled under
`prefers-reduced-motion`.
