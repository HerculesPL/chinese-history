# Chinese Civilization Timeline — Project Log

> Living document tracking implemented features, design decisions, and planned work.

---

## Implemented

### v1.0 — Foundation

- **Horizontal scroll timeline** (desktop) with GSAP ScrollTrigger pinning and snap
- **Vertical scroll** fallback for mobile (< 900px)
- **13 dynasty panels**, each containing: era metadata, tagline, description, artifact image, territory SVG map, notable people cards with hover previews
- **Hero section** with stamp reveal animation and layered CSS mountain/grid background
- **Progress bar** with dynasty counter, clickable ticks, and fill animation
- **Era navigation** grouped into 6 periods (Ancient through Modern)
- **Keyboard navigation** — arrow keys step between panels
- **Responsive typography** using `clamp()` for fluid sizing across breakpoints
- **Staggered entrance animations** — panel elements cascade in on `.in-view`
- **Noise texture overlay** — subtle SVG turbulence filter for analog feel
- **Person cards** — hover reveals role, quote, and 3D peek effect
- **Artifact images** — sepia filter default, color on hover
- **All assets remote** — Wikimedia Commons portraits and artifacts, Google Fonts

---

## In Progress

### Parallax Depth Layers

**Goal:** Create a 2.5D diorama effect within each dynasty panel by scrolling layered elements at different rates during horizontal navigation.

**Three layers:**

| Layer | Element | Speed | Purpose |
|-------|---------|-------|---------|
| Back | `.panel-bg-char` (dynasty character) | Slowest | Anchors the scene, feels like a distant wall |
| Mid | `.panel-artifact-bg` (ghost artifact) | Medium | Adds atmospheric depth between background and content |
| Front | `.panel-content` (text, cards, maps) | Normal (1:1) | Interactive foreground, scrolls with the user |

**Approach:**
- Use GSAP `containerAnimation` to tie parallax tweens to the existing horizontal scroll
- Background character drifts ~80px counter to scroll direction
- Mid-layer artifact ghost drifts ~40px, rendered at `opacity: 0.03` with `grayscale(1)`
- Desktop only — skip on mobile for performance and because vertical scroll gains less from the effect

**Design rationale:** The site already positions a large background character per panel. Adding a second atmospheric layer (faint artifact) and animating both at offset speeds turns each panel into a layered scene without adding new visual weight. Subtlety is key — the effect should be felt, not noticed.

---

## Ideas & Backlog

### Visual & Animation
- **Panel transition wipes** — ink wash or brush-stroke mask between dynasty panels instead of a hard cut
- **Ambient particle system** — faint floating ink dots or dust motes, density tied to dynasty era (sparse for ancient, denser for modern)
- **Territory map animation** — regions draw in with a calligraphic stroke effect when a panel enters view
- **Scroll-driven color theming** — shift the panel's accent color subtly per dynasty (vermillion for warlike eras, jade for prosperous ones)
- **Portrait parallax** — person card images shift slightly on mouse move for a living portrait effect

### Content & Interaction
- **Audio layer** — ambient traditional instrument clips per era (guqin for ancient, erhu for later), opt-in with a mute toggle
- **Dynasty comparison mode** — select two dynasties to view stats side-by-side
- **Deep-dive overlay** — click a person card to expand into a full biography modal with timeline of their life
- **Search / filter** — find a person or innovation across all dynasties
- **Shareable links** — URL hash updates per dynasty so users can link to a specific era

### Performance & Technical
- **Image preloading strategy** — preload the next 2 panels' assets based on scroll direction
- **Reduced motion support** — respect `prefers-reduced-motion` and disable parallax/animations
- **Offline support** — service worker to cache the HTML and critical assets
- **Build step** — extract CSS/JS into separate files, minify, add cache-busting hashes

---

## Design Principles

1. **Subtlety over spectacle** — effects should enhance the historical content, not compete with it
2. **Performance is a feature** — no animation is worth a dropped frame; test on mid-range devices
3. **Progressive enhancement** — the site works as a static document; JS adds layers of interaction
4. **Respect the content** — 5,000 years of history carries its own weight; the design serves it
