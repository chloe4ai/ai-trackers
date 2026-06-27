# 🛰️ AI Trackers

Three prompt-driven, scheduled "trackers" that keep me on top of the AI world.
They aren't traditional scripts — each tracker is a **natural-language instruction file** (`SKILL.md`)
executed on a schedule by an AI agent ([Claude Code](https://claude.com/claude-code) /
the scheduled-tasks runner). The agent gathers the data live from the web and delivers a
**bilingual (English + 中文) digest** as a Google Calendar event with a popup reminder.

## The three trackers

| Tracker | Schedule | What it captures |
|---|---|---|
| 🧠 **Thinker Tracker** | Daily (~08:00) | Top takes from AI thought leaders + deep long-form content (essays / podcasts / videos / interviews) |
| 🚀 **Feature Tracker** | Fridays (~08:00) | Product & feature updates from the big three labs — OpenAI, Anthropic, Google |
| 📈 **Trend Tracker** | Mon & Thu (~08:00) | Trending projects on Product Hunt, Hugging Face, GitHub, and Arxiv |

Each digest is **bilingual**: every item is written in English first, then followed by a
Chinese translation (`🇨🇳 中文:`) and a one-line analysis (`分析:`).

## How it works

```
schedule (cron)  ─►  AI agent reads SKILL.md  ─►  WebSearch / WebFetch gathers live data
                                                          │
                 Google Calendar event  ◄── bilingual digest composed
                 (all-day + popup reminder)
```

There is **no application code to run** — the logic lives entirely in the prompts.
To use one, drop its folder into your scheduled-tasks directory (e.g.
`~/.claude/scheduled-tasks/<name>/SKILL.md`) and set the cron schedule shown in the file's
front-matter comment.

## Repo layout

```
trackers/
  thinker-tracker/SKILL.md    # daily
  feature-tracker/SKILL.md    # weekly (Fri)
  trend-tracker/SKILL.md      # twice-weekly (Mon/Thu)
```

## Customising

- **Sources / people**: edit the "TRACK" / "SOURCES" section of each `SKILL.md`.
- **Schedule**: change the cron line in the front-matter comment, then re-create the
  scheduled task with that cron.
- **Delivery**: the files deliver via Google Calendar; the fallback writes a Gmail draft +
  a local Markdown copy. Swap in whatever connector you prefer.
- **Language**: remove the bilingual `🇨🇳 中文 / 分析` instructions for English-only output.

> Note: delivery details use placeholders (`your-email@example.com`, `~/trackers/...`).
> Replace them with your own before running.

## License

[MIT](LICENSE)
