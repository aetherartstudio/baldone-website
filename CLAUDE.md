# Baldone — Contemporary Artist Portfolio Website

## Project Overview

Single-page portfolio website for **Baldone**, a contemporary painter represented by **Aether Art Gallery** in Taipei. Built as a single HTML file (`index.html`) with inline CSS and JavaScript — no build tools, no framework, no dependencies.

- **Live site:** Hosted on GitHub Pages via [github.com/aetherartstudio/baldone-website](https://github.com/aetherartstudio/baldone-website)
- **Design:** "Ink & Paper" — high-contrast minimal with off-white background (#FAFAFA), black text, terracotta accent (#8B4513)
- **Fonts:** Cormorant Garamond (serif headings) + DM Sans (sans body) via Google Fonts

## Architecture

### Single-file structure
Everything lives in `index.html` — CSS in `<style>`, HTML in `<body>`, JavaScript in `<script>`. Pages are `<div class="page">` sections toggled via JavaScript (SPA with hash routing).

### Pages & Navigation
- **Work** (`#work`) — Hero section with animated video background + masonry painting gallery with series filtering
- **About** (`#about`) — Artist bio, mentions Aether Art Gallery representation with link to aetherart.co
- **Contact** (`#contact`) — Email contact with mailto link
- Navigation uses hash routing (`#work`, `#about`, `#contact`, `#artwork/slug`) so browser back/forward buttons work
- On mobile: same 3 text links shown inline (no hamburger menu)

### Gallery System
- Paintings defined in the `paintings` array in JavaScript (id, title, series, image path, medium, year, size)
- Series defined in `seriesInfo` object (title + description text per series)
- Current series: `colors`, `infinite`, `love`, `hooponopono`
- Masonry layout with 30px gap, responsive columns
- Filter bar at top to filter by series

### Lightbox / Artwork Page
- Opens as full-screen overlay when clicking a painting
- Off-white background (#FAFAFA) — consistent with site
- Artwork occupies 70-85% max width
- Features: prev/next navigation, pinch-to-zoom on mobile, keyboard shortcuts (arrow keys, Escape)
- **Inquiry button** — generates mailto with painting details pre-filled
- **Share button** (+share) — expands to show Facebook, Pinterest, Email, WhatsApp, X options
  - On mobile: uses Web Share API (native share sheet with actual image file)
  - On desktop: per-platform popup windows sharing artwork image URL + details
- Deep links: each artwork has a shareable URL like `#artwork/color-i`

### Hero Section
- Animated video background (`images/hero-video.mp4`) — AI-generated ink painting animation (Kling 3.0)
- Video: H.264 Constrained Baseline profile for Safari compatibility, autoplay muted loop playsinline
- 55% opacity with radial vignette overlay
- Title "Baldone" with text shadow for readability over video
- Subtle terracotta glow behind title

### Favicon
Inline SVG favicon — black square with white "B" in Georgia serif

## File Structure

```
baldone-website/
├── index.html              ← entire website (single file)
├── CLAUDE.md               ← this file
├── .gitignore              ← excludes old versions, .claude/, tmp files
├── images/
│   ├── hero-video.mp4      ← hero background animation (H.264 Baseline, ~6.5MB)
│   ├── gallery/            ← painting photos for masonry grid (TO BE ADDED)
│   ├── insitu/             ← in-situ photos for lightbox (TO BE ADDED)
│   └── about/              ← artist portrait (TO BE ADDED)
└── old versions/           ← previous design variants (gitignored)
    ├── baldone-website-v5.html
    ├── baldone-sacred-earth.html
    ├── baldone-night-gallery.html
    ├── baldone-ink-ivory.html
    ├── baldone-cool-minimalist.html
    └── baldone-dark-gallery.html
```

## Key Design Decisions

- **Off-white background (#FAFAFA)** not pure white — softer on eyes, more gallery-like
- **No hamburger menu on mobile** — 3 text links shown inline for direct access
- **Consistent background** across all pages including lightbox (no dark mode for artwork viewing, because paintings will be shown in-situ on white gallery walls)
- **Hero animation** slowed to ~26-40 seconds per loop for meditative pace
- **Vignette overlay** on hero video uses expanded radial gradient (90% transparent center) to show full painting including lower portions

## Content That Needs Replacing

⚠️ **Painting images are currently placeholder Squarespace CDN URLs** that break on Mac browsers (hotlink protection). Must be replaced with local images in `images/gallery/`.

Image sizing recommendations:
- Gallery thumbnails: 800-1200px wide, JPEG quality 80%
- In-situ lightbox: 1800-2400px wide, JPEG quality 85%
- Artist portrait: 1200px wide
- All text content (series descriptions, painting data, bio) is inline in the HTML/JS

## Git & Deployment

- **Repo:** github.com/aetherartstudio/baldone-website
- **Branch:** main
- **Hosting:** GitHub Pages (deploy from main branch, root folder)
- **Git config (local):** user.name=jfkuentz, user.email=jfk@aetherart.co
- **Custom domain:** not yet configured (instructions in commit history)

## Video Processing

FFmpeg is installed on this machine. Common operations used:
- **Reverse video:** `ffmpeg -y -i input.mp4 -vf reverse -an output.mp4`
- **Seamless loop (fwd+rev):** `ffmpeg -y -i input.mp4 -filter_complex "[0:v]split[fwd][rev];[rev]reverse,trim=start_frame=1[revtrimmed];[fwd][revtrimmed]concat=n=2:v=1:a=0[out]" -map "[out]" ...`
- **Slow motion:** add `setpts=PTS*1.75` to filter chain (1.75x = 35% slower)
- **Safari-compatible encode:** `-c:v libx264 -profile:v baseline -level 3.1 -pix_fmt yuv420p -movflags +faststart`

## Related Projects (Future)

- **Kanaputz e-commerce site** — separate website for sculpture figurines with 360° turntable viewer, PayPal/Stripe payments, owner login section with live feed from "The Crystal" sculpture
- **CRM/Email:** Gallery uses Mailchimp (email) + Artable (CRM) synced via Zapier
