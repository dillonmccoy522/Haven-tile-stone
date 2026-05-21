# Haven Tile & Stone — Full Site Redesign Spec

## Goal

Upgrade all 13 pages from the current dark-luxury layout to a more modern, high-impact visual design while keeping the existing dark/gold color palette and all SEO work intact.

## Scope

All 13 pages:
- `mockup-homepage.html`
- `about.html`
- `gallery.html`
- `bathroom-remodeling.html`
- `kitchen-remodeling.html`
- `whole-home-flooring.html`
- `ocean-isle-beach.html`
- `holden-beach.html`
- `sunset-beach.html`
- `brunswick-county.html`
- `myrtle-beach.html`
- `calabash.html`
- `leland-wilmington.html`

`thanks.html` and `outdoor-pool-decks.html` are excluded (thanks gets minor styling updates, outdoor-pool-decks is not linked).

---

## Design System

### Colors (unchanged)
```css
--dark:    #0C0C0C
--dark-2:  #111111
--dark-3:  #141414
--dark-4:  #222222
--gold:    #C8A96A
--gold-lt: #D9BF8A
--gold-dim: rgba(200,169,106,0.10)
--white:   #FAFAFA
--w70:     rgba(250,250,250,0.7)
--w40:     rgba(250,250,250,0.4)
--w15:     rgba(250,250,250,0.06)
--border:  rgba(200,169,106,0.18)
```

### Typography (unchanged)
- **Headings:** Playfair Display (italic for emphasis, gold for accents)
- **Body / UI:** DM Sans

### New CSS additions
- `.bento` grid system
- `.ticker-track` keyframe animation
- `.hero-pill` eyebrow capsule
- `.badge` floating stat card
- Centered nav grid layout

---

## Component Specifications

### 1. Navigation — Centered Logo

Three-column grid layout: links left | logo center | links + CTA right.

```
[ Services  Gallery  About ]  [ HAVEN & STONE ]  [ Areas Served  Reviews  [Free Consultation] ]
```

- Grid: `grid-template-columns: 1fr auto 1fr`
- Logo: Playfair Display, 1.15rem, centered
- Links: DM Sans, 0.72rem, 0.12em letter-spacing, uppercase, `rgba(250,250,250,0.6)` default → `#C8A96A` on hover
- CTA pill: `background: #C8A96A`, `color: #0C0C0C`, 9px 22px padding, border-radius 1px
- Scroll behavior: at `scrollY > 60`, apply `nav.scrolled` → `background: rgba(12,12,12,0.96)`, `backdrop-filter: blur(20px)`, `border-bottom: 1px solid var(--border)`
- Mobile: collapses to logo only + hamburger icon at < 768px (hamburger opens full-screen overlay menu)

### 2. Hero — Full-Bleed Cinematic

Used on: homepage (video), all other pages (full-bleed photo).

**Structure:**
- Full viewport height (`min-height: 100vh`)
- Nav overlaps hero (`margin-top: -[nav height]` on hero, nav is `position: fixed`)
- Background: `<video>` autoplay/muted/loop on homepage; `<img>` on inner pages with `object-fit: cover`
- Gradient overlay: `linear-gradient(to bottom, rgba(12,12,12,0.5) 0%, rgba(12,12,12,0.3) 50%, rgba(12,12,12,0.75) 100%)`

**Content (centered):**
1. Eyebrow pill — `border: 1px solid rgba(200,169,106,0.25)`, border-radius 20px, gold pulsing dot + location text
2. Headline — Playfair Display, 5rem desktop / 3rem mobile, `line-height: 1.0`; first line white, second line italic gold
3. Subhead — DM Sans, 0.9rem, `rgba(250,250,250,0.5)`, max-width 400px
4. Dual CTAs: primary gold button + ghost outline button
5. Floating stat badges — right side, `position: absolute`, 3 stacked cards (15+ Yrs / 5.0★ / 500+), `backdrop-filter: blur(12px)`
6. Scroll indicator — bottom center, animated ↓ arrow

**Per-page headlines:**
| Page | Line 1 | Line 2 (italic gold) |
|------|--------|----------------------|
| Homepage | Crafted for | *the Coast.* |
| About | Built on | *Craft.* |
| Gallery | The Work | *Speaks.* |
| Bathroom | Bathrooms | *Worth Showing Off.* |
| Kitchen | Kitchens | *Done Right.* |
| Whole-Home | Floors | *Worth Walking On.* |
| Ocean Isle Beach | Serving | *Ocean Isle Beach.* |
| Holden Beach | Serving | *Holden Beach.* |
| Sunset Beach | Serving | *Sunset Beach.* |
| Brunswick County | Brunswick County's | *Premier Installer.* |
| Myrtle Beach | Serving | *Myrtle Beach.* |
| Calabash | Serving | *Calabash.* |
| Leland/Wilmington | Serving | *Leland & Wilmington.* |

### 3. Gold Ticker Strip

Immediately below the hero on every page.

- `background: #C8A96A`
- Infinite scrolling marquee via CSS animation
- Content: service names + location names separated by `✦`
- Font: DM Sans 0.65rem, 0.16em letter-spacing, uppercase, `color: #0C0C0C`
- Speed: 22s linear infinite

### 4. Bento Grid — Primary Content Section

Used on all pages as the main content block below the ticker.

**Grid structure:**
```
grid-template-columns: 2fr 1fr 1fr
grid-template-rows: 260px 200px
gap: 3px
background: rgba(200,169,106,0.08)  ← creates the gap "border" color
```

**Cell types:**
- **Photo main** (`grid-row: span 2`) — full-bleed photo or video still, project caption tag bottom-left
- **Gold accent cell** — `background: #C8A96A`, `color: #0C0C0C`, Playfair heading + sub text + arrow
- **Stat cell** — large Playfair number in gold + uppercase label
- **Service/content cell** — dark background, service tag chip, Playfair heading with italic gold accent, body text, gold link with arrow
- **Photo mini** — secondary photo or dark gradient placeholder

**Per-page bento content:**

*Homepage:* Services overview (bathrooms, kitchens, flooring stat, review count)

*About:* Donovan story (portrait photo, "15+ years" stat, personal quote cell, service area count)

*Gallery:* Photo grid preview (4 large cells, filter navigation)

*Service pages (bathroom/kitchen/flooring):* Process steps, before/after, materials info, service area map cell

*Geo pages:* Local project photo, "Serving [City]" accent cell, services offered, distance/travel note, CTA cell

### 5. Testimonials Section

3-column grid, used on homepage, about, and all service pages.

- Background: `#0f0f0f`
- Each card: `background: #141414`, gold stars, Playfair italic quote, DM Sans author line
- Gap: 3px with `rgba(200,169,106,0.06)` background (same gap-as-border technique as bento)

### 6. CTA Band

Bottom of every page above the footer.

- Two-column flex: headline left, button right
- Headline: Playfair Display, 2.2rem, white + italic gold
- Button: gold primary, "Book a Free Consultation"
- Sub-label: "Response within 24 hours · No obligation"
- Background: subtle gradient `linear-gradient(135deg, #111 0%, #141a18 100%)`

### 7. Footer (unchanged structure, updated styling)

- Dark background `#0C0C0C`
- Gold border-top
- 4-column grid: brand/tagline | services | areas served | contact
- Social icons + copyright

---

## Page-Specific Notes

### Homepage (`mockup-homepage.html`)
- Hero uses `work-highlight-nosubs.mp4` (existing video)
- Bento: services overview + stat cells
- Add process section (3-step: Consult → Install → Enjoy) between bento and testimonials

### About (`about.html`)
- Hero uses existing hero image
- Bento replaces current intro-split grid
- Keep video section (`work-highlight-nosubs.mp4`) as its own full-width section below bento
- 8-area cards section stays, restyled as a bento-style grid

### Gallery (`gallery.html`)
- Hero is shorter (60vh instead of 100vh)
- Filter tabs restyled to match new design language
- Masonry grid stays, card hover overlays updated

### Service Pages
- Hero: 80vh (slightly shorter than homepage)
- Bento focuses on process/materials/scope
- Geo-targeted CTA at bottom ("Serving [closest city]")

### Geo Pages
- Hero: 80vh
- Bento: local project photo + services offered in that area
- Testimonial pulled from a client in that city where possible
- Internal links to related service pages

---

## Animations & Interactions

- **Scroll reveal:** `.reveal` + IntersectionObserver (keep existing, apply to all new sections)
- **Ticker:** CSS `animation: scroll 22s linear infinite`
- **Nav scroll state:** `nav.scrolled` toggled at `scrollY > 60`
- **Bento hover:** `transform: scale(1.01)` + `box-shadow` on photo cells
- **CTA hover:** `opacity: 0.88` on gold buttons
- **Badge pulse:** subtle `box-shadow` pulse on hero stat badges (CSS keyframe)

---

## What Does NOT Change

- All `<head>` SEO tags (canonical, OG, Twitter, schema, favicon)
- All JSON-LD schema markup
- `sitemap.xml` and `robots.txt`
- Formspree contact form endpoint
- All phone numbers, addresses, and business info
- Font imports (Playfair Display + DM Sans)
- CSS custom properties (color variables)

---

## Shared CSS Architecture

All 13 pages share the same CSS block (copy-paste). Page-specific overrides are minimal. The full shared CSS includes:
- Reset
- CSS variables
- Nav (centered grid)
- Hero (cinematic)
- Ticker
- Bento grid + cell types
- Testimonials
- CTA band
- Footer
- Reveal animations
- Mobile breakpoints (768px, 480px)

---

## Mobile Breakpoints

**768px:**
- Nav collapses to logo + hamburger
- Hero title: 3rem
- Bento: `grid-template-columns: 1fr 1fr`, photo-main no longer spans 2 rows
- CTA band stacks vertically

**480px:**
- Hero title: 2.4rem
- Bento: single column
- Testimonials: single column
- Ticker font: 0.58rem
