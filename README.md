# A&N Style Website — Technical Documentation

**Version:** 1.0  
**Last Updated:** June 2026  
**Project Type:** Single-file static website  
**File:** `index.html`

---

## Table of Contents

1. Project Overview
2. Technology Stack
3. File Structure
4. CSS Architecture
5. Theme System (Dark / Light Mode)
6. Sections Reference
7. Third-Party Integrations
8. Gallery & Lightbox System
9. Video Showreel
10. Responsive Design System
11. JavaScript Reference
12. Deployment
13. Common Maintenance Tasks
14. Known Limitations

---

## 1. Project Overview

A&N Style is a single-page, production-ready website for a premium barbershop and hair salon located on Lower Richmond Road, SW London. The entire site lives in one `index.html` file alongside a local `images_and_videos/` folder for client-supplied media assets.

**Key characteristics:**
- No build step, no npm, no bundler — opens directly in a browser
- All styling via Tailwind CSS CDN + a custom `<style>` block
- All interactivity via vanilla JavaScript in a single `<script>` block
- CSS custom properties (`var()`) power the dark/light theme system
- Formspree handles form submissions (no backend required)

---

## 2. Technology Stack

| Layer | Technology | Version / Source |
|---|---|---|
| Markup | HTML5 | — |
| Styling | Tailwind CSS | CDN (`cdn.tailwindcss.com`) |
| Typography | Montserrat + Inter | Google Fonts CDN |
| Icons / SVGs | Inline SVG | Hand-coded |
| Form backend | Formspree Ajax | `unpkg.com/@formspree/ajax@1` |
| Maps | Google Maps Embed API | iframe embed |
| Video | HTML5 `<video>` tag | Local MP4 file |
| Hosting | Static file host | (see Deployment section) |

### Tailwind Configuration (inline)

Tailwind is configured inside a `<script>` tag in the `<head>`:

```js
tailwind.config = {
  theme: {
    extend: {
      fontFamily: {
        heading: ['Montserrat', 'sans-serif'],
        body:    ['Inter', 'sans-serif'],
      },
    },
  },
};
```

Custom colours are NOT in Tailwind config — they use CSS custom properties instead (see Theme System).

---

## 3. File Structure

```
project-root/
├── index.html                        ← The entire website
└── images_and_videos/
    ├── ladiesstyling.jpg             ← Gallery: ladies styling photo
    ├── hottowelshave.jpg             ← Gallery: hot towel shave photo
    ├── clipercut.webp                ← Gallery: clipper cut photo
    └── A&N Style.mp4                 ← Video showreel
```

All other gallery images are loaded from Unsplash CDN (placeholder images). Replace with real photos by updating the `src` attributes in the gallery section and the matching entry in the `galleryImages` JavaScript array.

---

## 4. CSS Architecture

All custom CSS is written inside a single `<style>` block in `<head>`. It is organised into labelled sections:

```
─── CSS CUSTOM PROPERTIES   (theme variables)
─── RESET & BASE            (box-sizing, body)
─── THEME TRANSITION        (smooth colour change)
─── NAV                     (sticky nav, link underlines)
─── THEME TOGGLE BUTTON     (pill toggle)
─── HERO                    (background image overlay)
─── CARDS                   (service-card, staff-card)
─── GOLD                    (accent lines, glow)
─── MOBILE MENU             (hamburger overlay)
─── FORM                    (inputs, focus states)
─── BUTTONS                 (btn-gold, btn-outline)
─── CAT SEPARATOR           (gold gradient line)
─── HOURS TABLE             (row borders)
─── MAP                     (placeholder background)
─── REVEAL                  (scroll animation)
─── SCROLLBAR               (custom webkit scrollbar)
─── GALLERY                 (grid items, overlay, tag)
─── LIGHTBOX                (full-screen image viewer)
─── VIDEO SHOWREEL          (aspect ratio wrapper)
─── WHATSAPP FAB            (fixed button + pulse ring)
─── REVIEW CARDS            (hover transitions)
─── FORMSPREE VISIBILITY    (!important overrides)
─── RESPONSIVE              (comprehensive breakpoints)
```

### Selector conventions

- Component classes: `.service-card`, `.staff-card`, `.gallery-item`, `.review-card`
- State classes: `.reveal`, `.reveal.visible`, `.active` (lightbox), `.scrolled` (nav), `.open` / `.hidden` (mobile menu)
- Layout helpers: `.gallery-grid-main`, `.gallery-grid-row2`, `.mock-slots`, `.mock-week`, `.hero-stats`, `.google-badge`, `.footer-map`
- Grid placement (desktop only): `.g-tall-left`, `.g-top-ml`, `.g-top-mr`, `.g-tall-right`, `.g-bot-ml`, `.g-bot-mr`

---

## 5. Theme System (Dark / Light Mode)

The theme is controlled by the `data-theme` attribute on the `<html>` element.

```html
<html lang="en" data-theme="dark">   ← default
<html lang="en" data-theme="light">  ← toggled state
```

### CSS Variables

Two full sets of CSS custom properties are defined:

```css
:root, [data-theme="dark"]  { --bg-base: #111111; ... }
[data-theme="light"]        { --bg-base: #FFFFFF; ... }
```

Every colour in the site references these variables — never a hardcoded hex except for the gold accent (`#C9A84C`) which is constant across both themes.

### Full variable list

| Variable | Dark | Light | Purpose |
|---|---|---|---|
| `--bg-base` | `#111111` | `#FFFFFF` | Page background |
| `--bg-surface` | `#161616` | `#F9FAFB` | Section alt background |
| `--bg-surface2` | `#1A1A1A` | `#FFFFFF` | Cards, hours panel |
| `--bg-surface3` | `#1E1E1E` | `#F3F4F6` | Form inputs, service cards |
| `--border` | `#2A2A2A` | `#E5E7EB` | Card and input borders |
| `--border-soft` | `#222222` | `#F3F4F6` | Subtle dividers |
| `--text-primary` | `#F0EDE8` | `#1F2937` | Headings, main text |
| `--text-secondary` | `#A09890` | `#4B5563` | Body text, descriptions |
| `--text-muted` | `#6B6560` | `#9CA3AF` | Labels, timestamps |
| `--nav-bg` | `rgba(17,17,17,0.96)` | `rgba(255,255,255,0.96)` | Navbar background |
| `--form-bg` | `#1A1A1A` | `#FFFFFF` | Form input background |
| `--form-border` | `#333333` | `#D1D5DB` | Form input border |
| `--staff-nabi-bg` | Gold-tinted dark gradient | Gold-tinted light gradient | Staff card header |
| `--staff-alex-bg` | Neutral dark gradient | Neutral light gradient | Staff card header |
| `--staff-sam-bg` | Warm dark gradient | Warm light gradient | Staff card header |

### Toggle function

```js
function toggleTheme() {
  const html = document.documentElement;
  const isDark = html.getAttribute('data-theme') === 'dark';
  html.setAttribute('data-theme', isDark ? 'light' : 'dark');

  // Sync mobile pill knob position
  const knob = document.getElementById('mob-knob');
  if (knob) knob.style.transform = isDark ? 'translateX(15px)' : '';

  // Sync datetime input colour scheme
  document.getElementById('f-datetime').style.colorScheme = isDark ? 'light' : 'dark';

  // Persist to localStorage
  localStorage.setItem('an-theme', isDark ? 'light' : 'dark');
}
```

Theme preference persists across page loads via `localStorage` key `an-theme`. On page load, this IIFE runs before first paint:

```js
(function() {
  const saved = localStorage.getItem('an-theme');
  if (saved && saved !== 'dark') {
    document.documentElement.setAttribute('data-theme', 'light');
  }
})();
```

---

## 6. Sections Reference

| Section ID | Background Variable | Key Content |
|---|---|---|
| *(hero)* | `hero-bg` class (CSS image) | Headline, CTAs, stat counters |
| `#services` | `--bg-surface` | 19 service + price cards in 4 category grids |
| `#team` | `--bg-base` | 3 staff profile cards (Nabi, Alex, Sam) |
| `#gallery` | `--bg-surface` | Photo grid + video showreel |
| `#reviews` | `--bg-base` | 5 Google reviews + CTA card |
| `#book-online` | `--bg-surface` | Mock Fresha calendar widget placeholder |
| `#hours` | `--bg-surface2` | Weekly hours table |
| `#booking` | `--bg-surface` | Formspree contact/booking form |
| `#contact` | `--bg-base` | Footer: contact info, hours summary, Google Maps iframe |

---

## 7. Third-Party Integrations

### Formspree (Booking Form)

- **Endpoint:** `https://formspree.io/f/mgojaorz`
- **Library:** `@formspree/ajax@1` loaded via `unpkg.com` CDN
- **Form element ID:** `#booking-form`
- **Initialisation:**
  ```js
  window.formspree = window.formspree || function () {
    (formspree.q = formspree.q || []).push(arguments);
  };
  formspree('initForm', { formElement: '#booking-form', formId: 'mgojaorz' });
  ```
- **Field names submitted:** `name`, `phone`, `service`, `stylist`, `preferred_datetime`
- **Data attributes used:**
  - `data-fs-field` — marks inputs for validation
  - `data-fs-error="fieldname"` — inline error message container
  - `data-fs-success` — shown after successful submission
  - `data-fs-error` (no attribute) — form-level error banner
  - `data-fs-submit-btn` — disables during submission

  **To change the Formspree endpoint:** update `formId: 'mgojaorz'` in the initForm call, and ensure the form `action` (if present) matches.

### Google Maps (Footer)

- **Embed type:** Street View / Map iframe
- **Coordinates:** `51.46616, -0.2145764` (Lower Richmond Road, SW London)
- **Current embed URL:** Google Maps Embed API with the client's Street View panorama ID
- **Footer link:** `https://maps.app.goo.gl/q8zmYjgaR36RgRXp7`

  To update the map, replace the `src` attribute on the `<iframe>` in the `#contact` footer section.

### WhatsApp

- **Phone number:** `+447836445764`
- **FAB link:** `https://wa.me/447836445764?text=Hi%20A%26N%20Style%2C%20I%27d%20like%20to%20book%20an%20appointment.`
- **Footer link:** `https://wa.me/447836445764`

  To change the number, update both the FAB `href` and the footer WhatsApp link. The pre-filled message text is URL-encoded in the query string.

### Fresha (Booking Widget Placeholder)

The `#book-online` section contains a styled mock calendar. To activate real-time booking:

1. Create / log into your Fresha business account at `fresha.com`
2. Go to **Dashboard → Widgets → Online Booking**
3. Copy the embed snippet (iframe or script tag)
4. In `index.html`, find the comment `⚡ INTEGRATION NOTE` inside `#book-online`
5. Replace the entire mock calendar `<div>` with your Fresha embed code

---

## 8. Gallery & Lightbox System

### Photo grid

The gallery uses two CSS grid classes:

- **`.gallery-grid-main`** — 6 images in a masonry-style layout (desktop only)
- **`.gallery-grid-row2`** — 3 equal images below

Grid item classes for desktop masonry placement:

| Class | Desktop position |
|---|---|
| `.g-tall-left` | Column 1, rows 1–2 (tall) |
| `.g-top-ml` | Column 2, row 1 |
| `.g-top-mr` | Column 3, row 1 |
| `.g-tall-right` | Column 4, rows 1–2 (tall) |
| `.g-bot-ml` | Column 2, row 2 |
| `.g-bot-mr` | Column 3, row 2 |

On mobile/tablet these classes are overridden and items stack in equal-height rows.

### Lightbox

Triggered by `onclick="openLightbox(index)"` on each `.gallery-item`.

The `galleryImages` array in the `<script>` block must be kept in sync with the HTML gallery order:

```js
const galleryImages = [
  { src: '...', caption: 'Skin Fade' },       // index 0 — g-tall-left
  { src: '...', caption: 'Gents Cut' },        // index 1 — g-top-ml
  { src: '...', caption: 'Beard Shape Up' },   // index 2 — g-top-mr
  { src: '...', caption: 'Ladies Styling' },   // index 3 — g-tall-right
  { src: '...', caption: 'Hot Towel Shave' },  // index 4 — g-bot-ml
  { src: '...', caption: 'Highlights' },       // index 5 — g-bot-mr
  { src: '...', caption: 'Clipper Cut' },      // index 6 — row 2, item 1
  { src: '...', caption: 'Wash, Cut & Blow' }, // index 7 — row 2, item 2
  { src: '...', caption: "Kids' Cut" },        // index 8 — row 2, item 3
];
```

**Lightbox keyboard controls:** `←` / `→` to navigate, `Escape` to close.

**To add a new photo:**
1. Add the `<div class="gallery-item ...">` in the HTML
2. Add a corresponding entry to `galleryImages[]` at the matching index

---

## 9. Video Showreel

The video is served as a local HTML5 `<video>` element:

```html
<video controls playsinline
  poster="https://img.youtube.com/vi/mVoCtszCYJM/maxresdefault.jpg"
  style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;border-radius:20px;">
  <source src="./images_and_videos/A&N Style.mp4" type="video/mp4">
</video>
```

- **Source file:** `./images_and_videos/A&N Style.mp4`
- **Poster:** YouTube thumbnail (used as a static preview image only)
- **Aspect ratio:** maintained by the `.video-wrapper` class (`padding-bottom: 56.25%`)

  **To replace the video:** swap the MP4 file in `images_and_videos/` and update the `src` attribute. If the filename contains spaces or special characters, ensure they are URL-encoded or rename the file.

  **Note on YouTube:** A previous implementation used a YouTube embed (`mVoCtszCYJM`). This was replaced with a local video because the YouTube video had embedding disabled (Error 153). If embedding is later enabled on YouTube, the iframe approach can be restored.

---

## 10. Responsive Design System

All breakpoints use a **mobile-first** approach via a dedicated `RESPONSIVE` block at the bottom of the `<style>` tag.

| Breakpoint | Width | Gallery behaviour |
|---|---|---|
| Mobile (default) | < 640px | 2-column equal grid, 160px tall |
| Tablet | 640px – 1023px | 3-column equal grid, 200px tall |
| Desktop | ≥ 1024px | Full masonry with spanning columns |

### Key responsive patterns

**Gallery (the most complex):**
The masonry layout uses named grid-placement classes (`.g-tall-left` etc.) which are only activated inside `@media (min-width: 1024px)`. Below that breakpoint, `grid-column: auto !important` overrides the placements.

**Hero stats:** `flex-wrap: wrap` ensures the three stat counters reflow to two rows on very narrow screens rather than overflowing.

**Mock booking calendar:** The 7-column week strip uses class `.mock-week` with `grid-template-columns: repeat(7, 1fr)` — it stays 7 columns at all sizes (each cell is just narrower), with font-size reduced to `10px` at the smallest breakpoint.

**Universal overflow guard** (last rule in the block):
```css
img, video, iframe, input, select, textarea { max-width: 100%; }
section, footer, nav { overflow-x: hidden; }
```

---

## 11. JavaScript Reference

All JavaScript is in one `<script>` block near the bottom of `<body>`, organised in this order:

### Theme toggle
`toggleTheme()` — flips `data-theme`, syncs mobile knob, updates datetime `color-scheme`, persists to `localStorage`.

### Sticky navbar
`scroll` event listener — adds/removes `.scrolled` class on `#navbar` when `scrollY > 10`.

### Hamburger menu
Click handler on `#hamburger` — toggles `open`/`hidden` classes on `#mobile-menu` and animates the three spans into an X. All `.mobile-nav-link` clicks close the menu.

### Gallery lightbox
Three functions:
- `openLightbox(idx)` — sets `#lightbox-img` src from `galleryImages[]`, adds `.active`, locks scroll
- `closeLightbox()` — removes `.active`, restores scroll
- `lightboxNav(dir)` — increments/decrements `currentIdx` with wrap-around
- `keydown` event — handles `Escape`, `ArrowLeft`, `ArrowRight`

### Formspree Ajax
```js
window.formspree = window.formspree || function () { ... };
formspree('initForm', { formElement: '#booking-form', formId: 'mgojaorz' });
```

### Scroll reveal
`IntersectionObserver` watches all `.reveal` elements. When 10% of the element enters the viewport (with a 40px bottom margin), `.visible` is added and the element is unobserved. The CSS transition then runs: `opacity 0 → 1`, `translateY(20px) → 0`.

---

## 12. Deployment

This is a static site — no server-side code. It can be deployed to any static host.

### Recommended hosts

| Host | Free tier | Notes |
|---|---|---|
| **Netlify** | Yes | Drag-and-drop deploy, custom domain, HTTPS auto |
| **Vercel** | Yes | Git-connected deploy, instant previews |
| **GitHub Pages** | Yes | Push to repo, configure Pages in settings |
| **Cloudflare Pages** | Yes | Fastest CDN, custom domain free |

### Deployment steps (Netlify — recommended)

1. Go to `app.netlify.com`
2. Click **Add new site → Deploy manually**
3. Drag the entire project folder (containing `index.html` and `images_and_videos/`) into the drop zone
4. Netlify assigns a URL immediately
5. Go to **Domain settings** to add a custom domain

### Important: file paths

The video and local images use relative paths (`./images_and_videos/...`). The `images_and_videos/` folder must be uploaded alongside `index.html` in the **same directory**. If the folder is missing, local images and the video will not load (Unsplash-sourced images will still work).

### HTTPS requirement

The Google Maps embed and Formspree both require the site to be served over HTTPS. All recommended hosts above provide HTTPS automatically. Do not serve via plain HTTP in production.

---

## 13. Common Maintenance Tasks

### Update a service price or duration

Find the service card in the `#services` section and update the price span and duration paragraph:

```html
<h4 ...>Skin Fade</h4>
<span ...>£32</span>          ← change price here
...
<p ...>45 minutes</p>         ← change duration here
```

Also update the matching `<option>` in the booking form's service dropdown:
```html
<option>Skin Fade — £32</option>
```

### Add a new gallery photo

1. Place the image in `images_and_videos/`
2. Add a new `.gallery-item` div in the appropriate grid
3. Add a matching entry to `galleryImages[]` in the JavaScript block at the correct index position

### Replace a placeholder Unsplash image with a real photo

1. Place the real photo in `images_and_videos/`
2. Find the `<img src="https://images.unsplash.com/...">` tag for that gallery slot
3. Replace the `src` with `./images_and_videos/your-photo.jpg`
4. Update the matching `src` in `galleryImages[]` to the same path

### Change the WhatsApp number

Search for `447836445764` in the file — there are 3 occurrences:
1. The FAB `href` attribute
2. The FAB `data-wa` or tooltip (if present)
3. The footer WhatsApp link

Replace all three with the new number in international format without spaces or `+` (e.g. `447700900123`).

### Change the phone number

Search for `442088745322` — update the `href="tel:+442088745322"` attribute and the visible display text `+44 20 8874 5322`.

### Update opening hours

Hours appear in two places:
1. The `#hours` section (full weekly table)
2. The compact hours grid in the `#contact` footer

Update both. Search for `9:00 AM` to find all occurrences quickly.

---

## 14. Known Limitations

- **No CMS** — all content is hardcoded in HTML. Any update requires editing the file directly and redeploying.
- **Formspree free tier limit** — 50 submissions/month on the free plan. Upgrade at formspree.io if volume increases.
- **Fresha widget** — the `#book-online` section shows a non-functional mock calendar. A real Fresha embed must be added manually once the business account is set up (see Section 7).
- **YouTube video** — the original YouTube embed (ID: `mVoCtszCYJM`) was replaced with a local MP4 because the video had embedding disabled. If the YouTube video owner enables embedding in YouTube Studio → Content → Details → "Allow embedding", the iframe approach can be restored.
- **Google Reviews** — reviews are hardcoded. They do not auto-update from Google. New reviews should be added manually to the HTML.
- **IE/Legacy browser support** — the site uses CSS custom properties, `IntersectionObserver`, and CSS Grid. It does not support Internet Explorer. All modern browsers (Chrome, Firefox, Safari, Edge) are fully supported.
