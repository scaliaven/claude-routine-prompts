# Paper of the Day

Surface the single most notable AI/ML paper from the last 24 hours.

## Steps

1. **Read** `config.yaml`: `arxiv_categories`, `interests`, `keywords_prioritize`, `keywords_deprioritize`, `max_candidates`, `prioritize_with_code`, `output_format`.

2. **Find candidates — exactly 3 searches, one per source, run in parallel:**

   **Search 1 — arXiv recency** (primary candidates):
   ```
   arxiv <YYYY-MM-DD> <keywords_prioritize top 4 joined with spaces>
   ```

   **Search 2 — HuggingFace trending** (community signal):
   ```
   site:huggingface.co/papers <keywords_prioritize top 3> <YYYY-MM-DD>
   ```

   **Search 3 — Papers With Code** (code-release signal, only if `prioritize_with_code` is true):
   ```
   site:paperswithcode.com <keywords_prioritize top 3> <YYYY-MM-DD>
   ```

   From all three results: extract arXiv IDs (`YYMM.NNNNN`), titles, author snippets, and any GitHub URLs. Do not fetch individual paper pages. Do not call any external APIs — Semantic Scholar, arXiv RSS, and HuggingFace API all return 403 in this environment and must not be attempted.

3. **Rank in-context** using what the search snippets already tell you:
   - Boost papers matching `keywords_prioritize` or `interests`
   - Penalize `keywords_deprioritize` matches
   - Boost papers appearing in Search 2 (HuggingFace community signal)
   - Boost papers appearing in Search 3 (code available)
   - Boost papers with a GitHub URL visible in the snippet
   - Rank remaining by apparent novelty and impact

4. **Pick** the single best paper.

5. **Output** the fields in `output_format` order as `##` headers:
   - `title` — full title
   - `authors` — full list (from snippet; omit if not available)
   - `arxiv_link` — `https://arxiv.org/abs/<id>`
   - `code_link` — GitHub URL from snippet, or "Not released"
   - `why_it_matters` — 2–3 sentences on significance
   - `tl_dr` — one plain-English sentence
   - `key_contribution` — single most important advance
   - `open_questions` — 2–3 questions the paper raises

If the snippets don't contain enough detail for a field, note "Details not available in search results" rather than hallucinating.

If nothing relevant is found in the last 24 hours, report the closest match with its date.
