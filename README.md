# AI Spider

AI Spider is a professional AI retrievability crawler and technical SEO intelligence platform. It evaluates how AI systems discover, extract, and cite content from your website, and surfaces everything Screaming Frog does plus the AI-specific signals they don't.

Currently in **private beta**.

---

## What AI Spider Does

AI Spider crawls your site and produces:

- **AI Retrievability Score** per page (0-100, six sub-dimensions)
- **Page Intelligence Panel** - click any URL anywhere in the app to open a full-detail drawer with scores, signals, inlinks/outlinks, and per-page fixes
- **Internal Link Explorer** - inlinks, outlinks, and anchor text distribution for every page
- **Redirect Intelligence** - chains, loops, mixed 301/302, redirects to noindex
- **Canonical Intelligence** - chains, loops, canonical pointing to 404/noindex/redirect
- **Architecture Intelligence** - directory tree with avg AI score, issue rate, and crawl depth per folder. Orphan pages, weakly-linked pages, buried authority detection, excessive outlinks, and dead-end page analysis across 4 sub-views
- **PageRank + HITS Authority** - authority distribution, buried authority pages, hub scores, leakage analysis, dead-end detection
- **Citation Readiness** - on-demand per-page scoring of statistics, dates, sources, quotes, tables, and lists
- **Section Strength** - on-demand per-section scoring of content richness across every H2 block
- **N-Gram Intelligence** - on-demand most frequent 2- and 3-word phrases across selected pages
- **Authority / HITS** - on-demand hub and authority scores via the HITS algorithm on selected pages
- **Near-Duplicate Detection** - MinHash Jaccard similarity (80%+ overlap) plus exact duplicate detection via SHA-256 hash
- **Resource Explorer** - MIME-aware JS/CSS/image/font inventory with broken resource detection
- **Site-wide Recommendations** - 22+ types prioritised by high/medium/low impact with affected page lists
- Full resource inventory: HTML, images, JavaScript, CSS, fonts, media
- Bot access analysis: GPTBot, ClaudeBot, PerplexityBot, AppleBot, CCBot
- 22+ issue types across 11 categories
- Real-time SSE updates - pages, scores, and issues stream live during crawl
- Paginated URL table supporting 13,000+ URL crawls with Load More across all tabs

---

## Why It Exists

Search is no longer just blue links.

AI systems crawl, chunk, score, and cite content differently from search engines. A page can rank well on Google and still be invisible to AI models because its content is JavaScript-dependent, poorly structured, missing author signals, or blocked by robots.txt directives.

AI Spider measures retrievability the way AI crawlers measure it, and gives you an intelligence platform to act on what you find.

---

## Architecture Overview

### Frontend
- Vite 7 + React 19 + TypeScript 5.9
- Tailwind CSS v4 with CSS custom properties
- Server-Sent Events for live crawl updates
- react-window virtualised URL table
- 17 intelligence views across tabs
- Paginated data loading (200 rows per fetch) with Load More across all tabs

### Backend
- Node.js + Express
- SQLite via better-sqlite3 (per-session database)
- Raw SQL - no ORM
- Session-scoped architecture: each crawl gets its own isolated database
- Server-Sent Events for real-time page and issue streaming

### Processing Pipeline (no LLM API key required)
- Axios with retry logic and redirect chain capture
- Cheerio for HTML parsing and signal extraction
- MinHash Jaccard similarity (128 hash functions) for near-duplicate detection
- SHA-256 hash for exact duplicate detection
- Power-iteration PageRank on the internal link graph
- HITS algorithm for hub and authority scoring
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
7. Issue generation (22+ rules across 11 categories, streamed via SSE in real-time)
8. Post-crawl: redirect and canonical analysis
9. Post-crawl: duplicate detection and PageRank + HITS computation
10. On-demand: N-Gram extraction, Citation Readiness, Section Strength, Authority/HITS on selected URLs
11. Recommendations engine (22+ site-wide and per-page types, prioritised by impact)

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

## Post-Crawl Intelligence Engines

Six engines run automatically after every crawl:

- **Redirect intelligence** - chains, loops, mixed 301/302, redirect to noindex
- **Canonical intelligence** - chains, loops, canonical to redirect/noindex/404
- **Duplicate detection** - SHA-256 exact match + MinHash Jaccard near-duplicates (80%+ threshold)
- **PageRank** - power-iteration on the internal do-follow link graph
- **HITS authority** - hub and authority score propagation via power iteration
- **External link check** - HEAD requests to all discovered external URLs

Four engines run on-demand on user-selected URLs:

- **N-gram extraction** - frequent 2- and 3-word phrase mining across selected pages
- **Citation readiness** - deterministic scoring of citable signals per page
- **Section strength** - per-chunk content richness scoring across H2 sections
- **Authority / HITS** - hub and authority scores on a selected subset of pages

---

## Issue Detection

**Errors** - AI access blocked, JS-dependent content, redirect loops, canonical to 404, missing H1, very thin content

**Warnings** - Long redirect chains, canonical chains, high boilerplate, no author signal, missing meta description, exact duplicates, orphan pages, readability issues, missing structured data, images without alt text

**Notices** - llms.txt missing, buried authority pages, URL uppercase, near-duplicate content, temporary redirects, pages missing from sitemap

---

## Export

Every crawl exports to five formats:

- **CSV** - 70 columns covering all page signals, AI scores, content metrics, link counts, duplicate data, redirect info, and bot access flags. Excel-compatible with BOM encoding.
- **HTML Report** - standalone visual report with sections for pages, all issues, broken internal links, broken external links, image issues, redirects, canonical issues, structured data types, and all four on-demand analysis results if run before export.
- **Markdown ZIP** - per-page .md files with front-matter URL, ready for AI workflows and RAG pipelines.
- **llms.txt** - structured site index per the llmstxt.org spec, grouped by directory with AI score summary.
- **Sitemap XML** - standard sitemap with AI-score-derived priority values.

---

## Reliability Design

- Slot-based concurrency with try/finally leak prevention
- Per-phase timeout enforcement
- Non-fatal post-crawl analysis (one failure does not stop others)
- All new-column queries guarded with try/catch for compatibility with older session databases
- Session isolation: each crawl writes to its own SQLite database
- SSE connection management with graceful client disconnect handling
- Link insertion chunked at 100 rows to avoid SQLite parameter limits on pages with 140+ links

---

## Advanced Intelligence (Coming Soon)

The next tier of AI Spider will add LLM-powered analysis for deep retrieval intelligence. No API key is required for any current features - these will be an optional add-on.

**Content Intelligence**
- Information Gain - unique insights your content contributes vs competing pages
- Coverage Score - how comprehensively a page addresses a topic
- Coverage vs Confidence - breadth of coverage balanced against evidence quality

**Entity & Knowledge Analysis**
- Entity Expansion - related entities and concepts connected to your content
- Multi-Hop Relationship Analysis - connections between entities across content
- Retrieval Drift Analysis - gaps between query intent and information surfaced

**Query & Retrieval Intelligence**
- Query Expansion Intelligence - related searches and topic variations for a target query
- Retrieval Compression - how effectively a page distils into key facts and claims
- Complex Query Reformulation - breaking sophisticated queries into underlying concepts

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
