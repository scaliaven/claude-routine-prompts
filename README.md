# claude-routines

A collection of scheduled Claude agent routines — each one is a self-contained prompt with a config file that can be run on a cron schedule or triggered manually.

## Repository structure

```
claude-routines/
├── README.md               # This file
└── paper_of_the_day/
    ├── prompt.md           # The agent prompt
    └── config.yaml         # User-configurable settings
```

Each routine lives in its own subdirectory containing two files:

| File | Purpose |
|---|---|
| `prompt.md` | Instructions for the Claude agent. Describes what to do, how to read the config, and how to format output. |
| `config.yaml` | User-editable settings that control the routine's behavior without touching the prompt. |

---

## Routines

### `paper_of_the_day`

Searches arXiv for papers published in the last 24 hours, filters by configured categories and keywords, and surfaces the single most notable paper with a structured summary.

**Config options** (`paper_of_the_day/config.yaml`):

| Key | Type | Description |
|---|---|---|
| `interests` | string | Free-text research interests used for ranking |
| `keywords_prioritize` | list | Boost papers containing these terms; top 4 also drive the arXiv search query |
| `keywords_deprioritize` | list | Down-rank papers containing these terms |
| `prioritize_with_code` | bool | Include Papers With Code in the community signal search |
| `output_format` | list | Fields to include in output, in order |

**Token cost:** ~4,500–6,500 tokens per run (~10–15% of a Claude Pro 5-hour window). On API key plans, add `--max-budget-usd 0.05`.

**How to run:**
```bash
claude --print --effort low < paper_of_the_day/prompt.md
```

- `--effort low` — reduces model thoroughness; sufficient for a focused search-and-summarise task, and keeps token usage around 4,500–6,500 tokens (~10–15% of a Pro 5-hour window).
- `--max-budget-usd` — only relevant if you are on an **API key (pay-per-token)** plan, not Claude Pro. Pro users have a token quota per 5-hour rolling window instead.

Or schedule it via Claude Code:
```
/schedule daily at 8am paper_of_the_day
```

---

## How to add a new routine

1. Create a new subdirectory with a short, descriptive name (e.g. `weekly_digest`).

2. Add a `prompt.md` that:
   - Describes the task clearly
   - Instructs the agent to read its `config.yaml` for any user-configurable settings
   - Specifies the expected output format

3. Add a `config.yaml` with sensible defaults for all configurable parameters.

4. Document the routine in this README under the **Routines** section.

### Prompt writing tips

- Start with a one-line role statement ("You are a ...").
- Number the steps the agent should follow.
- Define every config key the prompt references in a table in `config.yaml`.
- Keep prompt logic and user preferences separate — the prompt is the *how*, the config is the *what*.
