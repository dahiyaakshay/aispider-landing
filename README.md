# AI Spider

AI Spider is a professional AI retrievability crawler and technical SEO audit platform that evaluates how AI systems discover, extract, and cite content from your website.

Unlike traditional crawlers that measure search engine signals, AI Spider scores every page across six AI-specific dimensions and surfaces issues that prevent language models from reading, understanding, and citing your content.

Currently in **private beta**.

---

## What AI Spider Does

AI Spider crawls your site and produces:

- AI Retrievability Score per page (0-100, six sub-dimensions)
- Full resource inventory: HTML, images, JavaScript, CSS, PDFs
- Bot access analysis for GPTBot, ClaudeBot, PerplexityBot, and others
- Redirect chain and loop detection
- Canonical tag intelligence (chains, loops, conflicts)
- Near-duplicate and semantic similarity detection
- Internal PageRank computation
- Orphan page and buried authority page identification
- Structured data extraction and schema validation
- Hreflang validation
- 22+ issue types across 7 categories
- Site-wide and per-page actionable recommendations
- 69-column CSV export

All results appear in real-time as the crawl progresses.

---

## Why It Exists

Search is no longer just blue links.

AI systems crawl, chunk, score, and cite content differently from search engines. A page can rank well on Google and still be invisible to AI models because its content is JavaScript-dependent, poorly structured, missing author signals, or blocked by robots.txt directives.

AI Spider measures retrievability the way AI crawlers measure it.

---

## Architecture Overview

### Frontend

- Vite 7 + React 19 + TypeScript 5.9
- React Router v7
- Tailwind CSS v4 with CSS custom properties
- react-window for virtualised URL table (handles 500+ rows without frame drops)
- Recharts for visualisations
- Server-Sent Events for live crawl updates

### Backend

- Node.js + Express
- SQLite via better-sqlite3 (per-session database)
- Raw SQL migrations (no ORM)
- Session-scoped architecture: each crawl gets its own isolated database
- Server-Sent Events for real-time page and issue streaming

### Processing Pipeline (no LLM API key required)

- Axios with retry logic and redirect chain capture
- Playwright for JavaScript-rendered page detection
- Cheerio for HTML parsing and signal extraction
- MinHash Jaccard similarity for near-duplicate detection
- TF-IDF cosine similarity for semantic overlap and content cannibalization
- Power-iteration PageRank on the internal link graph
- Flesch readability scoring
- All analysis is purely algorithmic. No external AI API calls.

---

## Crawl Pipeline

Every crawled page passes through ten sequential stages:

1. URL discovery and normalisation (strips tracking params, CMS tokens, hsLang duplicates)
2. HTTP fetch with retry and redirect chain capture
3. Signal extraction (title, meta, h1/h2, canonical, OG, robots, hreflang, cookies, HTTP version)
4. AI signal detection (bot access, JS dependency, llms.txt, author, publish date, semantic HTML)
5. Content processing (HTML to Markdown, readability scoring, chunking by heading boundaries)
6. AI Retrievability Scoring (extractability, citability, structure, crawlability, chunk quality)
7. Issue generation (22+ rules across 7 categories, streamed via SSE in real-time)
8. Post-crawl: redirect and canonical analysis (chains, loops, conflicts)
9. Post-crawl: duplicate detection, semantic similarity, PageRank computation
10. Recommendations engine (site-wide and per-page, prioritised by impact)

Resource URLs (images, JS, CSS, fonts) are discovered via HTML parsing and HEAD-checked in batches after the main crawl.

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

Issues are categorised by severity and category:

**Errors** — AI access blocked, JS-dependent content, redirect loops, canonical to 404, missing H1, very thin content

**Warnings** — Long redirect chains, canonical chains, high boilerplate, no author signal, missing meta description, exact duplicates, orphan pages

**Notices** — llms.txt missing, buried authority pages, URL uppercase, near-duplicate content, temporary redirects

---

## Post-Crawl Analysis Engines

Six analysis engines run automatically after every crawl:

- **Redirect intelligence** — chains, loops, mixed 301/302, redirect to noindex
- **Canonical intelligence** — chains, loops, canonical to redirect/noindex/404
- **Duplicate detection** — MinHash Jaccard (exact and near-exact copies)
- **Semantic similarity** — TF-IDF cosine (content cannibalization and topic overlap)
- **PageRank** — power-iteration on the internal do-follow link graph
- **Hreflang validation** — BCP-47 format, x-default presence, reciprocal links

---

## Reliability Design

- Slot-based concurrency with try/finally leak prevention
- Per-phase timeout enforcement
- Non-fatal post-crawl analysis (one failure does not stop others)
- Session isolation: each crawl writes to its own SQLite database
- SSE connection management with graceful client disconnect handling
- Resource HEAD-check batching (10 concurrent, capped at total discovered)

---

## Repository Contents

This repository contains the portfolio landing page for AI Spider.

The full application (backend, frontend, crawler) is private and currently in beta testing.

---

## Developer

Built and designed by **Akshay Dahiya**

Portfolio: [akshaydahiya.site](https://akshaydahiya.site)
Connect on LinkedIn: https://www.linkedin.com/in/akshay-dahiya/

Other tools: [MarAI](https://marai.akshaydahiya.site) · [RetrieveAI](https://retrieveai.akshaydahiya.site)

---

## Positioning

AI Spider operates in the emerging category of:

AI Retrievability Auditing
AI Crawl Intelligence
Generative Engine Optimisation (GEO) Infrastructure
Technical SEO for the AI Era

This is not just a crawler.
This is an AI-era site audit platform.
