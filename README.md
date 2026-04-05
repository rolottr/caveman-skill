# Caveman Skill

Terse communication mode for agent work. Do work, report result, stop.

## Installation

```bash
npx skills add rolottr/caveman-skill
```

## What changed

This repo ships the best-performing Caveman variant found in a measured eval loop.

### Winner

`caveman-v5-min-text`

```md
Respond with the minimum text needed.

Rules:
- Prefer action over explanation
- Use short sentences
- No motivational filler
- No step-by-step reasoning unless asked
- No long summaries
- When possible, return only:
  1. finding
  2. fix
  3. next step
- For code tasks, keep prose under 5 lines unless I ask for detail
- If command output is noisy, summarize it in 1-3 bullets
- If confidence is high, state the answer directly
- Do not restate my request
```

## Measured improvement

### Against normal control

Same harder eval suite, same runner, same model.

| Variant | Runs | Success | Median tokens | P90 | Avg tokens |
|---|---:|---:|---:|---:|---:|
| Caveman v5 | 24 | 1.0000 | 2804.0 | 3897 | 2867.29 |
| Normal no-caveman | 24 | 1.0000 | 3815.5 | 4436 | 3422.71 |

- Success delta: `0.00` pts
- Median token reduction: `22.75%`
- P90 token delta: `-12.42%`

## How it was measured

Runner:
- `cursor-agent`

Model:
- `composer-2`

Approach:
1. build a fixture-first eval harness
2. test multiple Caveman variants on the same task/seed pairs
3. compare success, median tokens, and P90 tokens
4. keep only variants that stay accurate enough
5. compare the winner against a normal no-caveman control

## Task types used

The eval suite was intentionally practical and code-heavy, not toy prompts.

Examples:
- `hn_telegram_config` — move Telegram config to env vars and add fail-fast validation
- `producthunt_fixture_csv` — export local Product Hunt fixture data to stable CSV
- `arxiv_cli_contract` — make an arXiv-style CLI work from local fixtures without network deps
- `cleanup_logs` — delete only expired log files and keep fresh ones intact
- `zip_new_files` — archive only unseen files while keeping reruns idempotent
- `github_slack_summary` — turn local GitHub issues JSON into a concise Slack-ready summary
- `website_diff_monitor` — compare HTML snapshots and emit a deterministic change summary
- `metrics_bugfix` — fix number parsing for commas, decimals, whitespace, and negatives
- `cli_config_refactor` — make config loading respect defaults, file values, and env overrides

## License

MIT. See [LICENSE](./LICENSE).
