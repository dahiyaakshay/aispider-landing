# AI Spider

AI Spider is a professional AI retrievability crawler and technical SEO intelligence platform. It evaluates how AI systems discover, extract, and cite content from your website — and surfaces everything Screaming Frog does, plus the AI-specific signals they don't.

Currently in **private beta**.

---

## What AI Spider Does

AI Spider crawls your site and produces:

- **Executive Dashboard** — site health, critical issues, top opportunities, authority distribution, and resource health at a glance
- **AI Retrievability Score** per page (0–100, six sub-dimensions)
- **Page Intelligence Panel** — click any URL anywhere in the app to open a full-detail drawer with scores, signals, inlinks/outlinks, and per-page fixes
- **Internal Link Explorer** — inlinks, outlinks, and anchor text distribution for every page
- **Redirect Intelligence** — chains, loops, mixed 301/302, redirects to noindex
- **Canonical Intelligence** — chains, loops, canonical pointing to 404/noindex/redirect
- **Architecture Intelligence** — directory-level scores, worst directories, deepest paths, authority distribution
- **Directory Explorer** — click any directory to see its pages inline with AI score, PageRank, and issues
- **PageRank Intelligence** — authority distribution, trapped authority, high-value low-link pages
- **Content Similarity** — TF-IDF cosine similarity detecting high overlap and topic competition
- **Resource Explorer** — MIME-aware JS/CSS/image/font inventory with broken resource detection
- **Site-wide Recommendations** — prioritised by high/medium/low impact with affected page lists
- Full resource inventory: HTML, images, JavaScript, CSS, fonts, media
- Bot access analysis: GPTBot, ClaudeBot, PerplexityBot, AppleBot, CCBot
- Near-duplicate detection via MinHash Jaccard similarity
- Hreflang validation
- 25+ issue types across 7 categories
- Real-time SSE updates — pages, scores, and issues stream live during crawl

---

## Why It Exists

Search is no longer just blue links.

AI systems crawl, chunk, score, and cite content differently from search engines. A page can rank well on Google and still be invisible to AI models because its content is JavaScript-dependent, poorly structured, missing author signals, or blocked by robots.txt directives.

AI Spider measures retrievability the way AI crawlers measure it — and gives you an intelligence platform to act on what you find.

---

## Architecture Overview

### Frontend

- Vite 7 + React 19 + TypeScript 5.9
- React Router v7
- Tailwind CSS v4 with CSS custom properties
- Server-Sent Events for live crawl updates
- 18 intelligence views across tabs

### Backend

- Node.js + Express
- SQLite via better-sqlite3 (per-session database)
- Raw SQL — no ORM
- Session-scoped architecture: each crawl gets its own isolated database
- Server-Sent Events for real-time page and issue streaming

### Processing Pipeline (no LLM API key required)

- Axios with retry logic and redirect chain capture
- Playwright for JavaScript-rendered page detection
- Cheerio for HTML parsing and signal extraction
- MinHash Jaccard similarity for near-duplicate detection
- TF-IDF cosine similarity for content overlap detection
- Power-iteration PageRank on the internal link graph
- Flesch readability scoring
- MIME-aware resource classification
- All analysis is purely algorithmic. No external AI API calls.

---

## Crawl Pipeline

Every crawled page passes through sequential stages:

1. URL discovery and normalisation
2. HTTP fetch with retry and redirect chain capture
3. Signal extraction (title, meta, h1/h2, canonical, OG, robots, hreflang, HTTP version)
4. AI signal detection (bot access, JS dependency, llms.txt, author, publish date, semantic HTML)
5. Content processing (HTML to Markdown, readability scoring, chunking by heading boundaries)
6. AI Retrievability Scoring (six sub-dimensions)
7. Issue generation (25+ rules across 7 categories, streamed via SSE in real-time)
8. Post-crawl: redirect and canonical analysis
9. Post-crawl: duplicate detection, content similarity, PageRank computation
10. Recommendations engine (site-wide and per-page, prioritised by impact)

Resource URLs are discovered via HTML parsing and HEAD-checked in batches after the main crawl.

---

## Scoring Model

Each page receives six sub-scores aggregated into a single AI Retrievability Score:

| Dimension | What it measures |
|---|---|
| Extractability | How cleanly AI can pull readable content (boilerplate ratio, semantic HTML, word count) |
| Citability | Author attribution, publish date, structured data signals |
| Structure | Title, H1, meta description, canonical, OG tags |
| AI Crawlability | Bot access, JS dependency, nosnippet, llms.txt |
| Chunk Quality | Heading structure quality, orphan chunk ratio |
| Link Authority | Internal PageRank score |

---

## Issue Detection

**Errors** — AI access blocked, JS-dependent content, redirect loops, canonical to 404, missing H1, very thin content

**Warnings** — Long redirect chains, canonical chains, high boilerplate, no author signal, missing meta description, exact duplicates, orphan pages, readability issues

**Notices** — llms.txt missing, buried authority pages, URL uppercase, near-duplicate content, temporary redirects

---

## Post-Crawl Intelligence Engines

Eight engines run automatically after every crawl:

- **Redirect intelligence** — chains, loops, mixed 301/302, redirect to noindex
- **Canonical intelligence** — chains, loops, canonical to redirect/noindex/404
- **Duplicate detection** — MinHash Jaccard (exact and near-exact copies)
- **Content similarity** — TF-IDF cosine (high overlap and topic competition)
- **PageRank** — power-iteration on the internal do-follow link graph
- **Link intelligence** — orphan pages, buried authority, dead ends, excessive outlinks
- **Architecture analysis** — directory-level aggregations, depth distribution
- **Hreflang validation** — BCP-47 format, x-default presence, reciprocal links

---

## Reliability Design

- Slot-based concurrency with try/finally leak prevention
- Per-phase timeout enforcement
- Non-fatal post-crawl analysis (one failure does not stop others)
- Session isolation: each crawl writes to its own SQLite database
- SSE connection management with graceful client disconnect handling

---

## Repository Contents

This repository contains the portfolio landing page for AI Spider.

The full application (backend, frontend, crawler) is private and currently in beta testing.

---

## Developer

Built and designed by **Akshay Dahiya**

Portfolio: [akshaydahiya.site](https://akshaydahiya.site)
LinkedIn: [linkedin.com/in/akshay-dahiya](https://www.linkedin.com/in/akshay-dahiya/)

Other tools: [MarAI](https://marai.akshaydahiya.site) · [RetrieveAI](https://retrieveai.akshaydahiya.site)

---

## Positioning

AI Spider operates at the intersection of:

- AI Retrievability Auditing
- AI Crawl Intelligence
- Generative Engine Optimisation (GEO) Infrastructure
- Technical SEO for the AI Era

Not just a crawler. A full intelligence platform.
