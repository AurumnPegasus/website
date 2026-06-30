# Blog Audit Report

**Audit Date:** 2026-06-18
**Site:** aurumnpegasus.com (Hugo, hermit theme)
**Total Posts:** 20
**Average Score:** 74/100

> Calibration note: this is a *personal technical blog*, not a commercial SEO property. Scores weight clarity, technical correctness, and authentic voice, not keyword density or conversion. SEO/GEO findings are reported but should be treated as optional, not failures.

---

## Health Overview

| Metric | Count |
|--------|-------|
| Posts Scoring 90+ (Excellent) | 1 |
| Posts Scoring 70-89 (Good) | 14 |
| Posts Scoring 50-69 (Needs Work) | 5 |
| Posts Scoring <50 (Poor) | 0 |
| **Orphan pages (zero inbound internal links)** | **20 (all)** |
| **Dead-end pages (zero outbound internal links)** | **20 (all)** |
| Cannibalization issues | 0 |
| Unfinished draft published live | 1 (`hrm.md`) |
| Posts with code/math correctness bugs | 4 |
| Stale content (>180 days, by `date`) | 14 |

---

## Critical Issues (fix first)

1. **`hrm.md` will publish as a live, half-written post.** It is 432 words, cuts off right after the `slow_fast.png` figure, and never delivers the math its title ("Math behind HRM") promises. It has **no `draft: true` flag**, so a Hugo production build ships it. → Add `draft: true` to the frontmatter now, or finish it.
2. **Zero internal links across the entire site.** Every one of the 20 posts is both an orphan and a dead-end. The only `](...)` links are images and external URLs. No `relref`/`ref` shortcodes anywhere. This is the single highest-leverage structural fix (see Internal Linking section).
3. **Correctness bugs in code/math** (these mislead readers): `fast_multiplication.md` (`a^(b+c) = a^b + a^c` should be `* `; `expo` uses undefined `n`), `rref.md` (`while ... = 0` assignment instead of `==`; undefined `ic()`), `sigmoid.md` (unbalanced parens in gradient equation), `bio_inspiration.md` (`\dot` should be `\cdot`).

---

## Per-Post Scores

| Post | Score | Strongest aspect | Self-contained intro |
|------|-------|------------------|----------------------|
| discovery.md | 92 | Crisp honest thesis grounded in a real FSA anecdote; best-written | Yes |
| bio_inspiration.md | 88 | Original "geocentrism" thesis, well-sourced citations | Yes |
| chipwar.md | 84 | Excellent lightbulb→MOSFET narrative, genuine voice | Yes |
| scotland_yard_1.md | 82 | Rules→game-theory formalism, honest tone | Yes |
| networks.md | 80 | Complete top-to-bottom OSI walkthrough | Partial (weak intro) |
| gsoc_2022_pk.md | 78 | Thorough record of real OSS contributions | Yes |
| scotland_yard_2.md | 78 | Correct-feeling PSPACE proof, good worked example | Yes |
| leetcode_441.md | 76 | Clean math derivation, complexity teaching aside | Yes |
| sigmoid.md | 75 | From-scratch derivation tied to working NumPy | Yes |
| fast_multiplication.md | 74 | Three-tier progression (fast expo→Karatsuba→FFT) | Yes |
| leetcode_3.md | 74 | Two-solution progression, honest voice | Yes |
| rref.md | 74 | Strong skeleton→fill→walkthrough scaffold | Yes |
| connect_four_1.md | 72 | Progressive build-up with worked examples | Yes |
| connect_four_2.md | 70 | Systematic coverage of Allis's nine rules | Partial |
| huge_files.md | 70 | Excellent inline-commented C, strong systems explanation | Yes |
| cooc_embeddings.md | 68 | Clean teaching code for co-occurrence + SVD | Yes |
| owasp.md | 68 | Comprehensive reproducible TryHackMe walkthrough | Yes |
| pickle_rick.md | 68 | Clear friendly CTF walkthrough | Yes |
| pop.md | 60 | Honest practical first-person install guide | Yes |
| hrm.md | 55 | Strong voice + motivation framing | Yes (but unfinished) |

---

## Prioritized Action Queue

| # | Post | Score | Top issue | Recommended action |
|---|------|-------|-----------|--------------------|
| 1 | hrm.md | 55 | Unfinished, no draft flag → publishes live | Add `draft: true` or finish |
| 2 | pop.md | 60 | Broken links (`%5D`, trailing comma/paren), walls of text | Fix 2 links, split sections |
| 3 | cooc_embeddings.md | 68 | Ends abruptly, no results/conclusion | Add output + short wrap-up; fix title typo "Co-Occurance" |
| 4 | fast_multiplication.md | 74 | Math + code bug; expired Notion S3 images | Fix `a^b*a^c`, `expo` var; rehost images |
| 5 | rref.md | 74 | `=` vs `==` bug; undefined `ic()`; dup walkthrough lines | Fix snippet, add icecream import note |
| 6 | sigmoid.md | 75 | Unbalanced parens in gradient eq; "Gradiant" typo | Fix equation + typo |
| 7 | owasp.md | 68 | Repeated header typo "Insuffecient"; non-running JS | Fix typos, `window.location`, `document.cookie` |
| 8 | chipwar.md | 84 | Highest typo density (~12), "Veritasium" wrong 3 ways | Spell pass |
| 9 | connect_four_2.md | 70 | Empty `href=""` link back to Part 1 | Point link at Part 1 |
| 10 | pickle_rick.md | 68 | Typos; leaked bare IP on line 85; filename given 2 ways | Clean typos, remove stray IP |

---

## Internal Linking (top structural opportunity)

**Every post is an orphan and a dead-end.** There are natural cluster links worth adding (Hugo `{{< relref "post.md" >}}`):

| From → To | Why |
|-----------|-----|
| connect_four_1 ↔ connect_four_2 | Two-part series; Part 2's back-link is currently empty `href=""` |
| scotland_yard_1 ↔ scotland_yard_2 | Two-part series |
| bio_inspiration ↔ discovery ↔ hrm | ML/research thread (neural nets, reasoning, discovery) |
| sigmoid ↔ cooc_embeddings ↔ rref | from-scratch ML/linear-algebra cluster |
| leetcode_3 ↔ leetcode_441 ↔ fast_multiplication | algorithms/math cluster |
| owasp ↔ pickle_rick | security / TryHackMe cluster |

Adding even the series links (the 2 two-part posts) is a 4-line fix that resolves the worst of the orphan problem.

---

## Stale Content

By `date` frontmatter (no `lastUpdated` field exists on any post). For a personal blog, "stale" mostly flags link-rot and dead images, not content decay.

| Post | Date | Age | Real risk |
|------|------|-----|-----------|
| connect_four_1/2, scotland_yard_1/2 | 2020-11 | ~5.5 yr | imgur-hosted images (link-rot) |
| fast_multiplication | 2021-11 | ~4.5 yr | **Notion S3 image URLs already expired** |
| cooc_embeddings, leetcode_3/441, networks, owasp, pickle_rick, pop, rref, sigmoid, huge_files, gsoc | 2021-2022 | 4-5 yr | mostly fine; some imgur images |
| bio_inspiration, discovery, hrm, chipwar | 2026 | <3 mo | current |

**Recommended:** rehost the expired Notion images in `fast_multiplication.md` to `static/images/` (the pattern `chipwar.md` and `hrm.md` already use). Audit imgur links on the 2020 series posts.

---

## Format Consistency (low priority, noted)

Older posts (`connect_four_*`, `huge_files`, `networks`, `owasp`, `pickle_rick`, `cooc_embeddings`) store content as **pre-rendered raw HTML** (inline `<svg>` anchor blobs, `<p>` tags) instead of clean markdown. Newer posts (`discovery`, `chipwar`, `hrm`) use clean markdown + shortcodes. Not user-visible, but makes the old posts noisy to edit. No action needed unless you edit one.

---

## What's Healthy

- No topic cannibalization (each post targets a distinct topic).
- No `<50` posts; the median post is genuinely good.
- Voice is consistent and authentic across the 2026 posts (`discovery`, `bio_inspiration`, `chipwar`) — matches the documented style (no em dashes, measured, honest).
- Self-contained intros on 18/20 posts (good for AI citation / direct linking).

---

## Suggested next steps

1. **Right now:** add `draft: true` to `hrm.md` (1-line fix, prevents a half-finished post going live).
2. Add the two series internal links (`connect_four`, `scotland_yard`) — fixes the empty `href=""` and the worst orphans.
3. Run a spell + correctness pass on the 4 posts with code/math bugs (`fast_multiplication`, `rref`, `sigmoid`, `chipwar`).
4. Rehost the expired Notion images in `fast_multiplication.md`.

Files were only read, not modified. Report saved to `blog-audit-report.md`.
