# 360plusLC26 — Leadership Collective 2026 Interactive Globe

A self-contained, responsive **rotatable globe** for the 360plus Leadership
Collective 2026 page. It highlights **only the countries the students come
from**, and tapping/clicking a highlighted country reveals the students from
there (name + photo). Built to drop straight into a WordPress page, and tuned
for both desktop (drag to spin, hover tooltips) and mobile (drag to spin, tap
to select, bottom sheet panel).

Palette is taken from the 360plus logo — **shades of green on black**.

![colour: green + black](https://img.shields.io/badge/palette-green%20%2B%20black-4caf50)

---

## What's in here

| File | Purpose |
|------|---------|
| `index.html` | The entire widget — inline CSS + JS, self-contained. Open it directly to preview, or paste it into WordPress. |

The globe uses [D3](https://d3js.org/) (orthographic projection) + TopoJSON +
the `world-atlas` country map, all loaded from a public CDN. No build step.

---

## Add it to WordPress

**Option A — Custom HTML block (simplest)**

1. Edit the *Leadership Collective 2026* page.
2. Add a **Custom HTML** block.
3. Open `index.html`, copy **everything** from `<style>` down through the
   final `</script>` (or just paste the whole file — the `<!doctype>`/`<head>`
   wrapper is harmless inside a block, but copying from `<style>` onward is
   cleanest), and paste it into the block.
4. **Preview**, then **Publish**. The widget fills the block's width and is
   responsive.

**Option B — iframe (keeps it fully isolated from theme CSS)**

1. Upload `index.html` to your site (e.g. `/wp-content/uploads/lc-globe/index.html`)
   via SFTP or the Media/File manager.
2. In a Custom HTML block:
   ```html
   <iframe src="/wp-content/uploads/lc-globe/index.html"
           style="width:100%;height:680px;border:0;border-radius:18px;"
           loading="lazy" title="Leadership Collective 2026 Globe"></iframe>
   ```
   Option B is recommended if your theme's CSS interferes with the widget.

> **Networking note:** the widget loads D3 and the world map from
> `cdn.jsdelivr.net`. That's fine on a normal live website. (During
> development in this sandbox, outbound access to some hosts — including
> `360plus.org` — was blocked by the environment's egress policy, which is why
> the real student roster couldn't be auto-scraped; see below.)

---

## Add the real 50 students

Open `index.html` and find the block marked:

```js
/* ▼▼▼  STUDENT DATA — EDIT THIS  ▼▼▼ */
const STUDENTS = [ … ];
```

Each student is one line:

```js
{ name: "Full Name", country: "India", image: "https://360plus.org/.../photo.jpg" },
```

- **`country`** must be a real country name. If a country doesn't light up,
  open the browser console — any unmatched country is logged with the exact
  fix (add an entry to `COUNTRY_ALIASES`, e.g. `"USA": "United States of America"`).
- **`image`** is any photo URL (e.g. the student photos already in the 360plus
  media library). Leave it `""` to show the student's initials instead.
- Multiple students from the same country are grouped automatically — the
  country's panel lists all of them.

> The names currently in the file are **placeholders** spread across the
> program's known participating countries (India, South Africa, Tanzania,
> Madagascar, USA, Lebanon, Nepal, etc.). Replace them with the real roster.

### Getting the exact data automatically

The live page `https://360plus.org/leadership-collective-2026/` could not be
fetched from the build sandbox because `360plus.org` is not in the
environment's network allowlist. To have the real names/countries/photo URLs
pulled in automatically, either:

- add `360plus.org` to the environment's network egress allowlist, **or**
- paste the 50 students (name + country + photo URL) and they'll be wired in.

---

## Customising the look

All colours are CSS variables at the top of `index.html` under `:root`:

```css
--lc-green:        #4caf50;   /* highlighted student country  */
--lc-green-bright: #8dc63f;   /* hover / selected             */
--lc-green-deep:   #2f7d32;   /* shading / borders            */
--lc-black:        #0a0d0b;   /* backdrop                     */
--lc-ocean:        #0e1512;   /* the sphere                   */
```

Swap in the exact 360plus brand hex values once you have them — everything
else (tooltips, chips, panel) inherits from these.

---

## Features

- 🌍 True rotatable globe (drag to spin; idle auto-rotation; momentum-free,
  smooth on mobile).
- ✨ Only student countries are highlighted in 360plus green.
- 👆 Tap/click a country → side panel (desktop) / bottom sheet (mobile) with
  every student from that country: photo + name.
- 🏷️ Country "chips" below the globe — click one to fly the globe to that
  country and open it.
- 📊 Live counts of total students + countries.
- 📱 Fully responsive, no build step, single file.
