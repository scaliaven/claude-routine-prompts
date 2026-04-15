# Paper of the Day

Find the single most notable AI/ML paper from the last 24 hours.

1. Read `config.yaml`: `interests`, `keywords_prioritize`, `keywords_deprioritize`, `prioritize_with_code`, `output_format`.

2. Run 2 searches in parallel. Extract only arXiv ID, title, authors, GitHub URL — discard all other text. No URL fetches, no API calls (all return 403).
   - `arxiv <TODAY> <keywords_prioritize top 4>`
   - `site:huggingface.co/papers OR site:paperswithcode.com <keywords_prioritize top 3> <TODAY>`

3. Rank and pick the best paper: boost `keywords_prioritize` + `interests` matches and Search 2 appearances; penalize `keywords_deprioritize`; tiebreak on novelty.

4. Output `output_format` fields as `##` headers: `title`, `authors` (omit if unknown), `arxiv_link` (`https://arxiv.org/abs/<id>`), `code_link` (GitHub or "Not released"), `why_it_matters` (1–2 sentences), `tl_dr` (1 sentence), `key_contribution` (1 sentence), `open_questions` (2 × 1 sentence).

If nothing found in last 24h, report closest match with its date.
