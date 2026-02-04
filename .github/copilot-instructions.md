# Wild Light Agency — Copilot Instructions

This is a **static exhibition website** for Wild Light Arts Agency (currently featuring Jessica Lee's "Impressions" exhibition). The site is built with vanilla HTML/CSS and minimal JavaScript—no build tools, frameworks, or package managers.

## Architecture & Key Patterns

### Project Structure
- **Single-page exhibition site**: `impressions.html` is the main deliverable (paired with `impressions.css`)
- **Placeholder files**: `exhibitions.html` and `landingpage.html` are empty—reserved for future expansion
- **Static assets**: Images stored in `images/` folder; `.ics` calendar files in `calendar/`
- **Domain**: Deployed via GitHub Pages (CNAME file points to custom domain)

### Design System (CSS)
The site uses a **cohesive dark theme** with CSS custom properties defined in `:root` of `impressions.css`:
```css
--bg: #0b0b0c;              /* Main background */
--panel: rgba(255,255,255,0.06);  /* Card backgrounds */
--text: rgba(255,255,255,0.90);   /* Primary text */
--muted: rgba(255,255,255,0.68);  /* Secondary/metadata text */
--line: rgba(255,255,255,0.14);   /* Dividers */
```

**Responsive spacing**: Uses `clamp()` for fluid typography and padding (e.g., `clamp(18px, 3vw, 28px)`). This scales smoothly across device sizes without media queries for most elements.

### HTML Structure Conventions
- **Semantic layout**: Sections use `.section > .section-inner` pattern for consistent padding
- **Content hierarchy**: Sections include `.kicker` (uppercase label), `h2` (title), descriptive text
- **Accessibility**: Proper `aria-label` and `role` attributes on interactive groups (e.g., calendar buttons)
- **External links**: Always include `target="_blank" rel="noopener"` for security and UX

### JavaScript Philosophy
- **Minimal**: Single scroll-based animation (`hero` image dissolves as you scroll down)
- **Accessibility-first**: Uses `prefers-reduced-motion` media query; disables animation if user prefers
- **Self-contained**: All logic in a single IIFE in the HTML file (no external dependencies)

## Key Developer Workflows

### Deploying Changes
1. Commit changes locally: `git add . && git commit -m "description"`
2. Push to GitHub: `git push` (GitHub Pages auto-publishes on main branch)
3. **No build step required**—changes are live on the custom domain immediately

### Updating Exhibition Content
When creating new exhibition pages:
1. **Copy `impressions.html` as template** (same structure, same CSS file)
2. **Replace placeholder content**: Hero image, exhibition dates, artist statement, artist name
3. **Update meta tags** at the top (title, og:description, og:image) for social previews
4. **Calendar files**: Create new `.ics` files in `calendar/` folder matching the naming convention
5. **Reference in HTML**: Use relative paths (`./images/`, `./calendar/`)

### Common Modifications
- **Update footer copyright**: Change the year dynamically via JavaScript (`#year` span auto-updates)
- **Replace hero image**: Change `src` in `.hero-media img` tag (currently `./images/Birch Charcoal on Butchers Paper 2.jpg`)
- **Adjust event dates**: Modify `.hero-meta` chips and section content; regenerate Google Calendar links
- **Add new sections**: Follow the `.section` > `.section-inner` pattern; reuse `.kicker`, `h2`, `p` styles

## Critical Implementation Details

### Hero Image Scroll Animation
The hero fades out and translates upward as users scroll. Logic:
```javascript
const fadeDistance = heroH * 0.60;  // Fade completes over 60% of hero height
heroMedia.style.opacity = String(1 - t);  // Fades to 0
heroMedia.style.transform = `translateY(${t * 10}px)`;  // Subtle parallax
```
Uses `clamp()` to prevent negative opacity or excessive translation. Respects `prefers-reduced-motion`.

### Calendar Integration
- Two `.ics` files (opening night + exhibition dates) for Apple Calendar downloads
- Google Calendar links constructed via URL parameters (properly encoded)
- Responsive button grid: 2 columns on desktop, 1 on mobile (<700px breakpoint)

### Typography & Spacing
- **Font stack**: System fonts (ui-sans-serif, system-ui, Segoe UI, etc.)—no web fonts to load
- **Responsive sizing**: `font-size: clamp(34px, 5vw, 56px)` scales h1 from 34px to 56px fluidly
- **Max content width**: `--max: 980px`; all content constrained via `.wrap` class
- **Padding/gap**: `--pad` and `--gap` variables use clamp for mobile-first scaling

### Accessibility Notes
- Dark theme with sufficient contrast ratios (white text on dark backgrounds)
- Video embeds use `youtube-nocookie.com` domain for privacy
- Footer includes acknowledgment of Indigenous sovereignty (Yuin Nation, Walbanga People)
- BRAG venue note mentions wheelchair accessibility

## Patterns to Follow

1. **Color references**: Always use CSS custom properties (`var(--bg)`, `var(--text)`), never hardcode colors
2. **Responsive design**: Prefer `clamp()` over media queries for fluid scaling
3. **External links**: Use `rel="noopener"` when opening in new tab (security best practice)
4. **Metadata**: Keep meta tags (og:title, og:description, og:image) updated for social sharing
5. **Hover states**: `.btn:hover` adds light background; maintain consistent interaction feedback

## No Build Tools or Dependencies
This is **intentionally simple**:
- No npm, no bundler, no framework
- No CSS preprocessor (SCSS/Less)—pure CSS with custom properties
- No JavaScript libraries—vanilla DOM APIs only
- Deploy-ready: just commit and push to GitHub

---

**Last Updated**: February 2026  
**Relevant Files**: [impressions.html](impressions.html), [impressions.css](impressions.css)
