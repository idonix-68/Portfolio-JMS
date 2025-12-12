# Architecture Design Review — `jamos-portfolio`

## Executive Summary

The current `jamos-portfolio` file implements a fully self-contained, single-page portfolio (HTML + CSS + minimal JavaScript). For a small, static portfolio this approach is generally acceptable and has a low operational burden (no build tooling, no dependencies, easy to host).

That said, there are a few architectural shortcomings that will impact correctness, accessibility, and maintainability as soon as the page grows:

- **HTML validity / single-document integrity is currently broken**: the file contains **two complete HTML documents back-to-back**, including a duplicated `<!DOCTYPE html>` and duplicated `<html>/<head>/<body>` blocks. Browsers will “recover”, but it is invalid HTML and can lead to inconsistent rendering and tooling issues.
- **Landmarks/navigation are minimal**: the page has good use of `<header>`, `<main>`, and `<footer>`, but lacks a conventional `<nav>` landmark (or an explicit accessible name for navigation).
- **Project cards are hard-coded**: fine for 3 projects, but becomes tedious and error-prone if projects increase or content needs frequent updates.

Overall: **the high-level sectioning (hero/projects/about/contact/footer) is appropriate**, and the “single-file” strategy is sufficient for the current scope, but the duplicated-document issue should be treated as a **correctness blocker**, and a few small structural refactors would improve scalability.

---

## 0) Document-level Architecture (File Integrity, Landmarks, Headings)

### Observations

1. **Multiple HTML documents in one file** ❌
   - The file begins with a complete HTML document, then later starts a second full HTML document.
   - This violates the core constraint that an HTML file represents **one document** with a single root `<html>` element.

2. **Landmark usage is mostly good** ✓
   - Uses `<header>`, `<main>`, and `<footer>`.
   - Content is grouped into `<section id="…">` blocks for primary page regions.

3. **Heading hierarchy is sensible** ✓
   - Hero uses `<h1>` for the page/person name.
   - Subsequent sections use `<h2>`.
   - Project cards use `<h3>`.

4. **Repository entry point / hosting is ambiguous** ⚠️
   - The repository also includes an `index.html` that appears to be a placeholder “Hello World” page, while the actual portfolio lives in a file named `jamos-portfolio` (without an `.html` extension).
   - For common static hosting (including GitHub Pages), `index.html` is the default entry point, so the deployed site may not show the portfolio at all.

### Recommendations

- **Fix the file to a single valid HTML document** (blocker)
  - Remove the duplicated/early document or merge content so there is exactly one `<!DOCTYPE html>` and one `<html>…</html>`.
  - This improves correctness, enables validation tools, and reduces unexpected browser parsing behavior.

- **Make the portfolio the default entry point for hosting**
  - Prefer renaming `jamos-portfolio` → `index.html` (or moving its content into `index.html`).
  - Alternative: keep `index.html` and have it redirect/link to the portfolio page explicitly.

- Consider adding a **skip link** (`<a href="#main" class="skip-link">Skip to content</a>`) and giving `<main>` an id (e.g., `id="main"`) for keyboard and screen reader usability.

---

## 1) Header / Hero Section

### Observations

- The hero content is placed inside `<header>` and implemented as `<section id="hero" class="hero">…`.
- CTA links function as “page navigation” (anchors to `#projects` and `#contact`), but there is no explicit `<nav>` landmark.
- Skills are visually presented as “chips” using a `<div class="chips">` with `<span>` children.

### Recommendations

- **Add a `<nav>` landmark** if CTAs are intended as page navigation
  - Example: wrap the CTA links in `<nav aria-label="Primary">…</nav>`.
  - This improves landmark discoverability for assistive tech.

- Consider representing skills as a list for semantics
  - Example: `<ul class="chips" aria-label="Skills"><li>HTML</li>…</ul>`.
  - This better reflects the content model (“a list of skills”) and makes future additions easier.

- Header placement is acceptable, but if the site grows to include a persistent site header/nav plus a hero, consider:
  - `<header>` for site-level banner + nav
  - hero as the first section inside `<main>`

---

## 2) Projects Section (Grid + Cards)

### Observations

- Uses `<section id="projects">` with a `.grid` wrapper.
- Each project is an `<article class="card">` with:
  - `h3` title
  - description `<p>`
  - tech stack in `<small>`
  - a paragraph containing “Live Demo | Source” links

This is a good semantic pattern: **a list/grid of independent pieces of content** is appropriately modeled as articles.

### Opportunities / Suggested Refactors

- **Make cards easier to update at scale**
  - For a static site with a small number of cards, hard-coding is sufficient.
  - If projects grow beyond ~5–6 entries or change frequently, consider one of these lightweight approaches:

  1) **HTML `<template>` + JavaScript rendering** (still no build tooling)
     - Store project data in an array and render cards consistently.
     - Benefits: less repetitive markup, reduces missing-field mistakes.

  2) **Data-in-HTML attributes**
     - Keep markup minimal and use `data-*` fields to populate repeated sub-elements.
     - Benefits: keeps the source of truth structured.

  3) **Server-side generation** (only if you introduce a build step)
     - Markdown/JSON driving a generator (11ty, Astro, etc.).
     - Benefits: best for long-term scalability, but increases tooling complexity.

- **Improve link semantics**
  - Replace `href="#"` placeholders with real URLs or omit links until available.
  - Consider adding `rel="noopener noreferrer"` if `target="_blank"` is introduced later.

---

## 3) About Section

### Observations

- Simple `<section id="about">` with an `h2` and a single paragraph.
- This is structurally fine and appropriately separated.

### Recommendations

- If the about content expands (multiple paragraphs, lists, timeline), consider:
  - grouping related content with subheadings (`<h3>Experience</h3>`, etc.)
  - using lists for enumerations (skills, highlights)

---

## 4) Contact Section

### Observations

- Contains a `mailto:` link and external profile links.
- Simple structure is appropriate.

### Recommendations

- Consider using a list for contact methods for consistency and easier extension:
  - e.g., `<ul>` with list items for email/GitHub/LinkedIn.
- If adding a contact form, ensure:
  - proper `<label>` elements
  - accessible error messaging
  - spam mitigation (honeypot, rate-limits, etc.)

---

## 5) Footer (Dynamic Year)

### Observations

- Footer contains `&copy; <span id="year"></span>` populated on `DOMContentLoaded`.
- This is a small, contained script and does not meaningfully affect architecture.

### Recommendations

- Optional: use a `<time>` element for semantics:
  - `<time id="year" datetime="2025">2025</time>` and update both `textContent` and `dateTime`.
- If JavaScript is removed, a manually updated year is acceptable for a small portfolio.

---

## 6) Bundling Strategy (Single File vs Modular Assets)

### Current Approach

- CSS is embedded in a `<style>` tag in the HTML.
- JavaScript is embedded in a `<script>` tag in the HTML.

### Assessment

- **Sufficient for current scope** ✓
  - One-file delivery is common and pragmatic for small portfolios.
  - It reduces operational complexity (no asset pipeline, no bundling).

- **Maintainability risks as the site grows** ⚠️
  - Inline CSS becomes harder to navigate as it grows.
  - Reusable patterns (cards, buttons, layout primitives) become harder to govern without a component or naming convention.

### Suggested Refactor Path (if/when needed)

1. **Split into external assets (no framework required)**
   - `index.html`
   - `styles.css`
   - `main.js` (or `scripts.js`)

2. **Introduce lightweight conventions**
   - CSS custom properties for theme tokens (`--color-bg`, `--color-accent`, etc.)
   - Component-ish class naming (BEM-like or utility + component)

3. **Only consider a generator/framework if requirements expand**
   - e.g., blog posts, multi-page site, i18n, many projects.

---

## Action Items (Prioritized)

1. **BLOCKER**: Ensure `jamos-portfolio` contains exactly **one** HTML document (single doctype, single `<html>`, single `<head>`, single `<body>`).
2. **SHOULD**: Make the portfolio the default entry point for hosting (rename/move it to `index.html`, or explicitly redirect/link from `index.html`).
3. **SHOULD**: Add an explicit `<nav>` landmark (or `aria-label`) around in-page navigation/CTAs.
4. **SHOULD** (if project count grows): Drive project cards from structured data via a `<template>` or a small render function.
5. **NICE TO HAVE**: Convert skill “chips” and contact links into semantic lists.

---

## Conclusion

The section-level architecture (hero → projects → about → contact → footer) follows common single-page portfolio patterns and is broadly aligned with semantic HTML and accessibility best practices. The primary architectural weakness is **document integrity** (two HTML documents in one file), which should be corrected immediately. Beyond that, the current approach is adequate for a small static site, with clear upgrade paths (external assets and data-driven cards) if maintainability and scale become priorities.
