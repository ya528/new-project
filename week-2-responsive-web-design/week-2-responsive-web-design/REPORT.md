# Week 2 Documentation: Implementation of Responsive Web Design

## 1. Project Overview
This deliverable implements a fully responsive, multi-section webpage (`index.html` and `styles.css`) engineered to adapt seamlessly across smartphones, tablets, and desktop displays. Rather than retrofitting a fixed desktop layout down to small screens, the project follows a **mobile-first progressive enhancement** workflow.

### Included Files
* `index.html` — Semantic HTML5 document structure with accessible landmarks, skip links, responsive figure elements, and a cross-device testing table.
* `styles.css` — Mobile-first stylesheet leveraging CSS custom properties, fluid `clamp()` typography/spacing, intrinsic CSS Grid (`auto-fit` + `minmax()`), Flexbox, `aspect-ratio` media framing, and progressive `min-width` media queries.
* `assets/responsive-devices.svg` — Scalable vector illustration demonstrating multi-viewport layout adaptation.
* `REPORT.md` — Technical documentation detailing responsive strategies, device-level layout adjustments, and troubleshooting solutions.

---

## 2. Responsive Design Strategies Implemented

### A. Mobile-First Progressive Media Queries (`min-width`)
Base styles in `styles.css` target narrow mobile viewports (`< 640px`) by default—stacking sections into single columns and maximizing vertical readability. Complexity is progressively added across three breakpoints:
1. **`@media (min-width: 640px)` (Large Phones / Small Tablets):**
   * Converts stacked hero CTA buttons into an inline horizontal row.
   * Expands `.hero-stats` from 1 column to 3 equal columns (`repeat(3, minmax(0, 1fr))`).
   * Transitions the footer from 1 column to a 2-column grid.
2. **`@media (min-width: 768px)` (Tablets / Small Laptops):**
   * Replaces the mobile hamburger drawer with a full horizontal navigation bar.
   * Expands `.breakpoint-grid` to 2 columns and aligns `.cta-banner` horizontally (`justify-content: space-between`).
3. **`@media (min-width: 1024px)` (Desktops & Widescreens):**
   * Activates the two-column asymmetric hero grid (`1.15fr 1fr`) and two-column fluid media showcase (`1.05fr 1fr`).
   * Expands `.breakpoint-grid` to 3 columns and `.footer-layout` to 3 columns inside a `1200px` max-width container.

### B. Intrinsic Grids & Fluid Typography
* **Intrinsic Auto-Fit Grid (`.auto-grid`):** Uses `grid-template-columns: repeat(auto-fit, minmax(min(100%, 270px), 1fr))`. Including `min(100%, 270px)` ensures cards never overflow on ultra-narrow (`320px`) screens while automatically reflowing from 1 to 2 to 3 columns as space permits.
* **Fluid Typography (`clamp()`):** Headings and body copy scale continuously between minimum and maximum bounds (e.g., `--fs-h1: clamp(1.95rem, 1.45rem + 2.4vw, 3.1rem)`), preventing oversized headlines on phones and undersized text on widescreens.

### C. Fluid Images & Aspect-Ratio Stability
* Global media rules (`max-width: 100%; height: auto; display: block;`) prevent images and SVGs from exceeding their parent containers.
* Explicit `width` and `height` attributes on `<img>` and CSS `aspect-ratio: 16 / 9` on `.aspect-frame` reserve vertical space prior to asset load, eliminating Cumulative Layout Shift (CLS).

---

## 3. Device-by-Device Layout Adjustments

| Viewport Tier | Width Range | Navigation | Hero & Feature Grids | Data Table |
| :--- | :--- | :--- | :--- | :--- |
| **Smartphone** | `320px – 639px` | Pure-CSS collapsible drawer (`44px` tap target) | 1-column stack; full-width buttons | Horizontally scrollable wrapper (`overflow-x: auto`) |
| **Tablet** | `640px – 1023px` | Inline horizontal nav bar (`>= 768px`) | 2-column feature & breakpoint grids | Full-width table without scroll clipping |
| **Desktop** | `>= 1024px` | Sticky full-width header with centered `1200px` container | Asymmetric 2-column hero + 3-column grids | Full-width table with generous cell padding |

---

## 4. Challenges Encountered & Resolutions

1. **Mobile Navigation Without JavaScript Overhead:**
   * *Challenge:* Standard horizontal navbars wrap awkwardly or overflow on `375px` screens, while adding JavaScript just for a menu toggle adds unnecessary complexity to a static HTML/CSS deliverable.
   * *Resolution:* Built an accessible, pure-CSS checkbox toggle (`.nav-toggle` + `.nav-toggle-label`) with animated hamburger bars, visible `:focus-visible` outlines, and `44px × 44px` touch targets that smoothly transitions into an inline horizontal bar at `768px`.
2. **Preventing Data Table Overflow on Narrow Viewports:**
   * *Challenge:* The 5-column Viewport Testing Matrix table forced the entire page body to scroll horizontally on `375px` smartphones.
   * *Resolution:* Wrapped the table in `.table-wrapper` with `overflow-x: auto` and `tabindex="0"` so keyboard and touch users can scroll the table independently while keeping the rest of the page locked to `100%` viewport width.
3. **Grid Track Minimum Width Blowout:**
   * *Challenge:* Standard `minmax(270px, 1fr)` rules can cause horizontal overflow on devices narrower than `300px` once container padding is subtracted.
   * *Resolution:* Used `minmax(min(100%, 270px), 1fr)` so the minimum track size clamps to `100%` of the available container width on ultra-compact screens.
