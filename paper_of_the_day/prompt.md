# Paper of the Day

Surface the single most notable AI/ML paper from the last 24 hours.

## Steps

1. **Read** `config.yaml`: `interests`, `keywords_prioritize`, `keywords_deprioritize`, `prioritize_with_code`, `output_format`.

2. **Find candidates — exactly 2 searches, run in parallel:**

   **Search 1 — arXiv recency:**
   ```
   arxiv <YYYY-MM-DD> <keywords_prioritize top 4 joined with spaces>
   ```

   **Search 2 — community + code signal:**
   ```
   site:huggingface.co/papers OR site:paperswithcode.com <keywords_prioritize top 3> <YYYY-MM-DD>
   ```

   From both results extract only: arXiv ID (`YYMM.NNNNN`), title, authors, GitHub URL if visible. Discard all other text. Do not fetch any URLs. Do not call external APIs (Semantic Scholar, arXiv RSS, HuggingFace API all return 403).

3. **Rank in-context:**
   - Boost: `keywords_prioritize` matches, `interests` alignment, Search 2 appearances, GitHub URL present
   - Penalize: `keywords_deprioritize` matches
   - Tiebreak: novelty and apparent impact from title/snippet

4. **Pick** the single best paper.

5. **Output** fields in `output_format` order as `##` headers:
   - `title` — full title
   - `authors` — from snippet, or omit
   - `arxiv_link` — `https://arxiv.org/abs/<id>`
   - `code_link` — GitHub URL or "Not released"
   - `why_it_matters` — 1–2 sentences
   - `tl_dr` — one sentence
   - `key_contribution` — one sentence
   - `open_questions` — 2 questions, one sentence each

If nothing relevant found in last 24 hours, report closest match with its date.
