# AI Trackers

**English | [中文](README.zh-CN.md)**

**When the logic is a prompt, the engineering problem stops being the code and becomes the evaluation.**

Three scheduled trackers that read the AI world for me every morning. There is no application to run: each tracker is a single `SKILL.md` — a natural-language instruction file executed on a cron by an agent ([Claude Code](https://claude.com/claude-code) / a scheduled-tasks runner), which gathers live data with WebSearch and WebFetch and delivers a bilingual digest as a Google Calendar event with a popup reminder. The whole repo is three prompts — so the only thing between me and a plausible-sounding daily lie is how hard I grade the output.

---

## The product argument

**1. The tracker is a prompt file, and the trade is deliberate.**
There is no schema, no test suite, no determinism — two runs of the same `SKILL.md` give different digests. In exchange, the source list, output shape, cadence and delivery live in one artifact anyone can fork and edit in English. For a personal feed where the requirement changes weekly and a bad run costs five minutes, that trade is right. For anything with a downstream consumer, it is not.

**2. Delivery is a calendar event, not an email — because an email digest dies in an inbox.**
Each run creates an **all-day event on today's primary calendar** with `overrideReminders: [{"method":"popup","minutes":540}]`, so a popup fires around 09:00 instead of sitting in a drafts folder, and each tracker gets its own `colorId` (3 / 9 / 7). Calendar unavailable is handled rather than fatal: fall back to a Gmail draft plus a Markdown copy under `~/trackers/<name>/YYYY-MM-DD.md`, **and say in the output that it was a fallback**.

**3. Three cadences, because sources change at different speeds.**

| Tracker | Cron | Window | Covers |
|---|---|---|---|
| **Thinker** | `2 8 * * *` | ~24h | Takes from lab leaders, researchers, investors, long-form podcasts/essays |
| **Feature** | `6 8 * * 5` | 7 days | Ships from OpenAI, Anthropic, Google — models, API, pricing, rollout |
| **Trend** | `10 8 * * 1,4` | 3–4 days | Product Hunt, Hugging Face, GitHub trending, Arxiv / HF Daily Papers |

A daily Product Hunt digest is mostly noise; a weekly digest of what a thinker said is stale. The window each prompt is given matches the refresh rate of the thing it watches.

**4. Every item must carry a one-sentence 分析 — which is a filter disguised as a translation rule.**
The format forces English content, then `🇨🇳 中文:` translation, then `分析:` — one line of why it matters. That last one is the real constraint: an item you cannot write a so-what about is an item that should have been cut.

**5. WebFetch degrades, it does not abort.** Trending pages block scrapers unpredictably. Every prompt says: if WebFetch errors, fall back to WebSearch and continue the run. A tracker that fails closed is a tracker you stop trusting to fire.

---

## What a run looks like

Illustrative sample, structure only — item names redacted so nothing here reads as a claim about a real product. See [`examples/`](examples/) for full captured runs.

```markdown
# 📈 Trend Tracker — <date> (中英对照)

## 🛠 Product Hunt
- **<Product>** — <what it does> + <why notable>. <link>
  - 🇨🇳 <translation>。**分析:** <one-line so-what>。
- **<Product>** — …
📌 **本版块洞察:** <pattern across this section, when there is one>

## 🤗 Hugging Face   ## 💻 GitHub   ## 📄 Arxiv / Papers
  … 3–5 items each, same shape …

🌟 **Pick of the Day / 今日精选** — <the single most interesting item, one line EN + one 中文>
```

Thinker uses `🔥 Top Takes` (3–6), `📚 Deep Dives` (3–6), `👀 Quick Hits`. Feature is organized by lab and ends with `⭐ This Week's Biggest / 本周之最`; a lab with nothing to report must say `No major updates this week.` rather than pad.

## How I judge whether a digest is any good

An agentic system nobody grades is a system that degrades quietly. I read each morning's output against four known failure modes:

| Failure mode | What it looks like | How I check it |
|---|---|---|
| **Stale items** | Something from three weeks ago presented as new | Spot-check the oldest-feeling items against the source's publish date. Any item outside the stated window is a failed run, not a soft miss. |
| **Hallucinated releases** | A model, price or feature that does not exist, stated confidently | Every Feature item must carry a link to an official blog, changelog or release note. No link, no item — and I click one per lab per week. |
| **Arxiv / trending noise** | Papers and repos that trend on volume, not substance | The prompt asks for cross-referenced buzz (HN, X) rather than raw sort order. A boilerplate `分析:` line is the tell that the item was filler. |
| **Duplicates across days** | The same take resurfacing daily as "new" | The Markdown fallback writes `YYYY-MM-DD.md`, so consecutive days diff. Repeat rate across a week is the first number I would track. |

None of this is automated yet, and that is the honest gap: the checks above are a human read, not a harness.

## What I'd build next

- **A repeat-rate metric.** Persist item titles and URLs per run and report overlap with the previous N runs — the cheapest single number that says whether a tracker is finding news or recycling it.
- **Link liveness and date checks as a post-step**, so a stale or 404 item fails the run instead of reaching the calendar.
- **A held-out grading pass**: a second agent scores yesterday's digest for accuracy, freshness and link validity, and I compare its scores against my own reads before trusting them.

## Run one

Drop the folder into your scheduled-tasks directory (e.g. `~/.claude/scheduled-tasks/<name>/SKILL.md`) and create the task with the cron from its front-matter. Edit `TRACK` / `SOURCES` to change who is watched, the delivery block to swap connectors, `🇨🇳 中文 / 分析` out for English-only.

> Delivery details use placeholders (`your-email@example.com`, `~/trackers/...`). Replace them before running.

## License

[MIT](LICENSE)
