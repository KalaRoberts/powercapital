# Power Capital — Fractional CFO Website

A fast, accessible, fully responsive multi-page marketing site for Power Capital, the fractional-CFO practice of John Roberts. Built as a hand-authored static site — no framework, no build step, no runtime dependencies — and deployed on GitHub Pages behind a custom domain.

**Live site:** [https://powercapital.cloud](https://powercapital.cloud)

**Stack:** Semantic HTML5 · modern CSS (custom properties, Grid, Flexbox) · vanilla JavaScript (ES5-safe) · GitHub Pages

---

## Table of contents

- [Overview](#overview)
- [Highlights](#highlights)
- [Tech stack & rationale](#tech-stack--rationale)
- [Architecture](#architecture)
- [Project structure](#project-structure)
- [Design system](#design-system)
- [Accessibility](#accessibility)
- [Performance](#performance)
- [Search engine optimization](#search-engine-optimization)
- [Contact form](#contact-form)
- [Analytics](#analytics)
- [Local development](#local-development)
- [Deployment](#deployment)
- [Browser support](#browser-support)

---

## Overview

Power Capital provides senior, part-time CFO leadership to capital-intensive businesses — data centers, construction, and real estate. The site introduces potential clients to Power Capital as a fractional-CFO business and routes interest through a contact form or LinkedIn.

It is a **six-page static site**:

| Page | Purpose |
| --- | --- |
| `index.html` | Hero positioning, pain points, services overview, process, closing CTA |
| `about.html` | Founder bio and a vertical career timeline (Amazon/AWS → fractional CFO) |
| `credentials.html` | Certifications, education, and client recommendations |
| `services.html` | Full service catalog, leadership pillars, and tiered engagement options |
| `industries.html` | Target verticals and "is it time for a CFO?" signals |
| `contact.html` | Working contact form (AJAX) plus direct-contact details |

---

## Highlights

- **Zero-dependency front end.** No React, no bundler, no `node_modules`. The entire site is three source languages and a handful of optimized assets — it loads instantly and will still build and run unchanged in a decade.
- **Single shared design system.** One `styles.css` drives all six pages from a set of CSS custom properties (color, type, spacing, radius, shadow tokens), so the brand stays perfectly consistent and a theme change is a one-line edit.
- **Responsive, mobile-first.** Fluid typography via `clamp()`, CSS Grid/Flexbox layouts, and three breakpoints (≤900px, ≤720px, ≤480px) including a hamburger nav and full-width touch targets.
- **Progressive enhancement.** Content and navigation work with JavaScript disabled; JS only *adds* polish (scroll reveals, sticky-header shadow, mobile menu, async form submit).
- **Accessible by default.** Semantic landmarks, keyboard-operable controls, `aria-live` form status, and full `prefers-reduced-motion` support.
- **Production-ready assets.** Favicons generated at every standard size (16–512 + `.ico`), a web app manifest for installability, and images sized to their display dimensions.
- **Working contact form** via Web3Forms — submitted asynchronously with inline success/error states and no page reload.

---

## Tech stack & rationale

| Choice | Why |
| --- | --- |
| **Hand-written HTML/CSS/JS** | A marketing site of this size has no need for a framework. Avoiding one eliminates build tooling, dependency maintenance, and shipped-JS weight — and keeps the output trivially hostable on any static host. |
| **CSS custom properties** | Centralizes the design system. Colors, spacing, and typography are defined once in `:root` and referenced everywhere, so the site is themeable and consistent. |
| **CSS Grid + Flexbox** | Modern, semantic layout without float hacks or layout frameworks. Grids reflow to single-column on mobile with a few media-query overrides. |
| **Vanilla JS (ES5-safe)** | ~100 lines, no transpilation required, no polyfills. Uses `IntersectionObserver` with a graceful fallback. |
| **GitHub Pages** | Free, fast, CDN-backed static hosting with first-class custom-domain + HTTPS support — the right fit for a static site. |
| **Web3Forms** | Lets a static site accept form submissions without a backend or server-side code. |

---

## Architecture

The site is intentionally simple and explicit:

```
Browser
  │
  ├── HTML (6 pages, each self-contained, semantic)
  │      └── shared <head>: fonts, favicons, manifest, stylesheet, SEO tags (canonical, Open Graph, JSON-LD)
  │
  ├── assets/css/styles.css   ← one design system, loaded by every page
  ├── assets/js/main.js       ← one behavior layer, loaded by every page
  └── assets/img/*            ← logo, favicons, portrait (display-sized)

```

**Shared chrome, repeated intentionally.** The header and footer markup is duplicated across pages rather than injected at runtime. For a six-page static site this is the correct tradeoff: it keeps every page a complete, crawlable, instantly-rendering document with no JS dependency for navigation or layout — which is better for SEO, resilience, and first paint than client-side includes.

**One stylesheet, one script.** Both are cached after the first page load, so subsequent navigations are effectively instant.

---

## Project structure

```
powercapital/
├── index.html              # Home
├── about.html              # Founder bio + career timeline
├── credentials.html        # Certifications, education, recommendations
├── services.html           # Services, pillars, engagement tiers
├── industries.html         # Verticals served
├── contact.html            # Contact form + details
├── site.webmanifest        # PWA/installability metadata
├── sitemap.xml             # Search-engine sitemap (final-domain URLs)
├── robots.txt              # Crawler directives + sitemap pointer
├── README.md
├── .gitignore
└── assets/
    ├── css/
    │   └── styles.css       # Full design system + components + responsive rules
    ├── js/
    │   └── main.js          # Nav toggle, sticky header, scroll reveals, form
    └── img/
        ├── logo-rich.png    # Dimensional brand mark (used in header/footer)
        ├── favicon.ico
        ├── favicon-16.png · favicon-32.png · favicon-180.png
        ├── favicon-192.png · favicon-512.png
        ├── john-roberts.jpg       # Portrait (hero/about)
        └── john-roberts-sq.jpg    # Square crop (contact card)
        └── og-cover.png           # 1200×630 social-share preview image

```

---

## Design system

All visual decisions are tokenized in `:root` and consumed via `var(--token)`:

- **Color** — a navy + forest-green palette with a burnt-orange accent drawn from the brand mark. Separate tokens for surfaces, borders, text tiers, and status.
- **Typography** — `Lora` (serif) for headings and `Inter` (sans) for body, loaded from Google Fonts with `preconnect` for faster fetch. Headings scale fluidly with `clamp()`.
- **Spacing & shape** — consistent spacing scale plus radius and layered shadow tokens for depth.
- **Components** — buttons (primary / dark / ghost / light / brand), cards, a vertical timeline, pricing-style engagement tiers, quote/recommendation cards, pain-point callouts, process steps, and a dark positioning banner — all composed from the same tokens.

Changing the entire site's accent color, corner radius, or type scale is a **single-line edit** in the token block.

---

## Accessibility

- Semantic HTML landmarks (`<header>`, `<nav>`, `<main>`-level sections, `<footer>`) and a logical heading hierarchy on every page.
- All interactive controls are real `<button>`/`<a>` elements — fully keyboard operable.
- The contact form uses associated `<label>`s and an `aria-live="polite"` status region so submit results are announced to screen readers.
- Color pairings target WCAG AA contrast.
- `prefers-reduced-motion` is honored — scroll-reveal and entrance animations collapse to a simple fade (or none) for users who request reduced motion.
- External links carry `rel="noopener"` and open predictably.

---

## Performance

- **No render-blocking JavaScript** — the single script is deferred to the end of `<body>` and only enhances an already-complete page.
- **Total page weight ≈ a few hundred KB**, most of it cached across navigations (one shared CSS + one JS file).
- **Right-sized images** — the brand mark was reduced from ~340 KB to ~20 KB (it renders at ~40px but was authored much larger); the portrait is compressed and dimensioned to use.
- `preconnect` to the font origins to shorten the font-fetch critical path.
- `IntersectionObserver` drives scroll animations (no scroll-event thrashing), with a no-JS fallback that simply shows all content.

---

## Search engine optimization

On-page SEO is built into the files and targets the final domain (`powercapital.cloud`), so it carries over unchanged when the domain is mapped:

- **Unique title tags and meta descriptions** per page, written around the data-center / infrastructure-finance niche.
- **Canonical URLs** on every page pointing at the final `https://powercapital.cloud/...` address — so search engines never split ranking signals between the GitHub Pages path and the custom domain.
- **Open Graph + Twitter Card tags** for rich link previews when the URL is shared (iMessage, LinkedIn, social). Each page sets its own preview title (e.g. home → *"Power Capital: John Roberts, Founder"*, interior pages → *"Power Capital: About/Services/…"*), the home preview description is *"Capital strategy for heavy infrastructure, without the C-suite overhead,"* and all are backed by a branded 1200×630 share image (`assets/img/og-cover.png`).
- **Structured data (JSON-LD)** — a `ProfessionalService` schema on the home page (services, expertise, founder) and a `Person` schema on the About page, so search engines understand the business and who runs it.
- **`sitemap.xml`** listing all pages (final-domain URLs, prioritized) and **`robots.txt`** allowing crawling and pointing to the sitemap.
- Semantic structure, a single `<h1>` per page, and descriptive image `alt` text.

**Deferred until the domain is live (intentionally):** submitting the sitemap to Google Search Console and requesting indexing. Doing this once, at the real `powercapital.cloud` URL, avoids getting the interim GitHub Pages path indexed as duplicate content.

---

## Contact form

The contact form posts asynchronously to **Web3Forms** — a static-site form backend — so there is no server to run or maintain:

- Submission is intercepted in JS (`fetch` → JSON POST); the page never reloads.
- Required fields (name, email, service, and message) are validated client-side before sending; company is optional.
- The submit button enters a "Sending…" state; on success an inline, animated **"Message sent!"** confirmation appears and the form resets.
- Network/API errors surface a graceful inline fallback pointing to LinkedIn.
- The email subject is built dynamically from the submitted name (e.g. *"New Power Capital form submission from Jane Smith"*).
- **Spam protection:** a hidden honeypot field plus an **hCaptcha** challenge. The submit handler requires a valid hCaptcha token before sending and resets the widget after each attempt. (hCaptcha must also be enabled for the access key in the Web3Forms dashboard for server-side enforcement.)

> The Web3Forms access key is a **public, write-only** submission key — it is safe to commit in a static site and cannot be used to read submissions.

---

## Analytics

Traffic is measured with **Google Analytics 4**, delivered via a **Google Tag Manager** container (`GTM-MM6WQ4KT`):

- The GTM container snippet is installed on every page — the loader script high in `<head>` and the `<noscript>` iframe fallback immediately after `<body>`.
- GTM is the delivery mechanism; the GA4 Configuration tag (with the GA4 `G-` Measurement ID) is created and published **inside the GTM dashboard** and fires on all pages. GTM loads but collects nothing until that tag is published.
- Because GA4 sets analytics cookies, add a privacy/cookie notice if required for your audience (e.g. EU/UK visitors).

---

## Local development

No build step. Any static file server works — for example:

```bash
# Python 3
python3 -m http.server 8000

# or Node
npx serve .

```

Then open `http://localhost:8000/`. Editing HTML/CSS/JS and refreshing is the full dev loop.

> Serving over HTTP (rather than opening the files via `file://`) is recommended so root-relative paths and the contact form behave exactly as they do in production.

---

## Deployment

Hosted on **GitHub Pages** as a project site:

1. Push the site to a GitHub repository.
2. In **Settings → Pages**, set the source to the `main` branch (root).
3. Add the custom domain (`powercapital.cloud`) — a `CNAME` file and the appropriate DNS records point the domain at GitHub Pages.
4. Enable **Enforce HTTPS**.

Because the site is fully static with relative asset paths, it deploys unchanged to any static host (GitHub Pages, Netlify, Cloudflare Pages, S3 + CloudFront, etc.).

---

## Browser support

Targets all current evergreen browsers (Chrome, Edge, Firefox, Safari) on desktop and mobile. Uses only widely-supported platform features — CSS custom properties, Grid, Flexbox, `clamp()`, and `IntersectionObserver` — each with sensible fallbacks where relevant.

---

## Author

Designed and built by **[Kala Roberts](https://kalaroberts.github.io/)**.

