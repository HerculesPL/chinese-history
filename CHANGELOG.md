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

### v1.1 — Parallax Depth Layers

- **Three-layer parallax system** — background character, ghost artifact, and foreground content scroll at different rates during horizontal navigation
- **`.panel-bg-char` parallax** — dynasty character drifts 120px total (60px in each direction) via GSAP `containerAnimation`, creating depth against the static panel
- **`.panel-artifact-bg` mid-layer** — faint grayscale artifact image (`opacity: 0.035`, `grayscale(1)`, `contrast(0.5)`) positioned behind content, drifts 60px total
- **`will-change: transform`** on both parallax layers for GPU compositing
- **Mobile excluded** — artifact ghost hidden via `display: none` at `<= 900px`; no parallax tweens run outside desktop `matchMedia` block
- **Graceful fallback** — `onerror` hides the artifact ghost if the image fails to load

**Design rationale:** The horizontal scroll format makes parallax feel natural rather than gimmicky. Two atmospheric layers behind the content create a rice-paper diorama depth. Values tuned for perceptibility — bg char at `opacity: 0.06` drifts ±150px, artifact ghost at `opacity: 0.07` drifts ±80px.

### v1.2 — Metropolitan Museum of Art Artifact Images

- **Replaced 13 dynasty artifact images** with public domain works from The Met's Open Access collection (CC0)
- **Source:** [Metropolitan Museum of Art Collection API](https://metmuseum.github.io/) — department 6 (Asian Art), all `isPublicDomain: true`
- **Modern eras kept on Wikimedia** — Republic of China (Sun Yat-sen Memorial) and PRC (Great Hall of the People) have no Met equivalents
- **Updated attribution footer** to credit both The Met and Wikimedia Commons

**Selections by dynasty:**

| Dynasty | Object | Met ID | Medium |
|---------|--------|--------|--------|
| Xia | Jade cong ritual object | 72376 | Jade (nephrite) |
| Shang | Tripod cauldron (ding) | 60514 | Bronze |
| Zhou | Wine container (hu) with copper inlay | 53960 | Bronze inlaid with copper |
| Qin | Figure of a charioteer | 53959 | Bronze |
| Han | Female Dancer | 42178 | Earthenware with pigment |
| Three Kingdoms | Funerary urn (hunping) | 44733 | Celadon-glazed stoneware |
| Jin/South-North | Buddha Maitreya | 42733 | Gilt bronze |
| Sui | Female attendant | 49547 | Glazed earthenware |
| Tang | Horse with gilding | 49195 | Earthenware with pigment |
| Song | Vase with dragonfish handles | 42447 | Celadon porcelain |
| Yuan | Bottle with peony scroll | 49216 | Blue-and-white porcelain |
| Ming | Jar with dragon | 39666 | Blue-and-white porcelain |
| Qing | Landscape with hermits and crane | 44218 | Jade (nephrite) |

**Design rationale:** Museum-quality artifacts are more visually cohesive and historically authentic than the mixed Wikimedia sources. Each piece was chosen to be culturally representative of its dynasty and visually compelling as a small image (sculpture and ceramics over paintings or text). The Met's CC0 license means no attribution is legally required, but we credit them anyway.

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
