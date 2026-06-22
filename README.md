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

| Path | Purpose |
|------|---------|
| `index.html` | The full widget — inline CSS + JS. Photos referenced from `assets/headshots/`. Open directly to preview. |
| `wordpress-embed.html` | **Paste-ready Custom HTML block.** Same widget but photos are embedded as data URIs, so it works with zero file uploads. |
| `assets/headshots/` | The 50 student headshots (square JPEGs), extracted from the official LC26 Bio & Headshot PDF. |

The globe uses [D3](https://d3js.org/) (orthographic projection) + TopoJSON +
the `world-atlas` country map, all loaded from a public CDN. No build step.

**The real 2026 cohort is already loaded** — 50 participants across 15
countries (India, USA, South Africa, Egypt, Brazil, Bhutan, Ghana, Syria,
Nepal, Madagascar, Portugal, France, Philippines, Peru, Nigeria), each with
their name and photo.

---

## Add it to WordPress

**Easiest — paste the ready-made block (no uploads):**

1. Open `wordpress-embed.html` and copy its entire contents.
2. Edit the *Leadership Collective 2026* page → add a **Custom HTML** block →
   paste → **Publish**. Photos are embedded, so nothing else to host (only the
   world map loads from a CDN).

---

**Alternative — iframe + folder upload** (lighter page, isolates theme CSS):

1. Upload the whole project — `index.html` **and** the `assets/` folder — to
   your site, e.g. `/wp-content/uploads/lc-globe/`, via SFTP or a file-manager
   plugin (keep the folder structure intact).
2. Edit the *Leadership Collective 2026* page → add a **Custom HTML** block →
   paste:
   ```html
   <iframe src="/wp-content/uploads/lc-globe/index.html"
           style="width:100%;height:680px;border:0;border-radius:18px;"
           loading="lazy" title="Leadership Collective 2026 Globe"></iframe>
   ```
3. **Preview**, then **Publish**. The widget fills the iframe width and is
   responsive.

> **Alternative (inline paste):** you can paste the whole `index.html` into a
> Custom HTML block instead of using an iframe — but then the relative photo
> paths (`assets/headshots/…`) must be made absolute to wherever you uploaded
> the images (e.g. `https://360plus.org/wp-content/uploads/lc-globe/assets/headshots/…`).
> The iframe route avoids this entirely.

> **Networking note:** the widget loads D3 and the world map from
> `cdn.jsdelivr.net` — fine on a normal live website.

---

## Editing / updating the students

The roster is already populated. To tweak it, open `index.html` and find the
block marked:

```js
/* ▼▼▼  STUDENT DATA — EDIT THIS  ▼▼▼ */
const STUDENTS = [ … ];
```

Each student is one line:

```js
{ name: "Full Name", country: "India", image: "assets/headshots/07-humera-ali.jpg" },
```

- **`country`** must be a real country name. If a country doesn't light up,
  open the browser console — any unmatched country is logged with the exact
  fix (add an entry to `COUNTRY_ALIASES`, e.g. `"USA": "United States of America"`).
- **`image`** is a bundled path (`assets/headshots/…`) or any URL. Leave it
  `""` to show the student's initials instead.
- Multiple students from the same country are grouped automatically — the
  country's panel lists all of them.

> The data was auto-extracted from `LC26_Bio_and_Headshot.pdf` — **50
> participants**. (The PDF actually lists 51; Deki Lhamo, Bhutan was removed to
> match the site's count of 50.)

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
