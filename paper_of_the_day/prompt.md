# Paper of the Day

Surface the single most notable AI/ML paper from the last 24 hours.

## Steps

1. **Read** `config.yaml`: `arxiv_categories`, `interests`, `keywords_prioritize`, `keywords_deprioritize`, `max_candidates`, `prioritize_with_code`, `output_format`.

2. **Fetch papers — two requests total:**

   **Request 1 — candidates:** Call Semantic Scholar with `keywords_prioritize` joined by spaces, date-bounded to the last 3 days:
   ```
   GET https://api.semanticscholar.org/graph/v1/paper/search
     ?query=<keywords_prioritize joined with spaces>
     &fields=title,authors,abstract,externalIds,openAccessPdf
     &limit=<max_candidates>
     &publicationDateOrYear=<TODAY-3d>:<TODAY>
   ```
   If this call fails, fetch ONE arXiv RSS feed instead: `https://rss.arxiv.org/rss/<first_category>`.

   **Request 2 — trending signal:** WebSearch `site:huggingface.co/papers trending <TODAY>`. Note which arXiv IDs appear — boost those papers in ranking. (HuggingFace's website and API are blocked by Cloudflare bot protection and cannot be fetched directly.)

3. **Rank**: boost `keywords_prioritize` matches and `interests` alignment; penalize `keywords_deprioritize`; boost code links if `prioritize_with_code`; then rank by novelty, result clarity, and impact.

4. **Pick** the single best paper.

5. **Output** the fields in `output_format` order as Markdown `##` headers:
   - `title` — full title
   - `authors` — full list
   - `arxiv_link` — `https://arxiv.org/abs/<id>`
   - `code_link` — repo URL or "Not released"
   - `why_it_matters` — 2–3 sentences on significance
   - `tl_dr` — one plain-English sentence
   - `key_contribution` — single most important advance
   - `open_questions` — 2–3 questions the paper raises

If nothing is found in the last 24 hours, report the most recent paper found (with date) and suggest broadening categories or keywords.
