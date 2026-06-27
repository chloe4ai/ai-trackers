# 🛰️ AI Trackers(AI 追踪器)

**[English](README.md) | 中文**

三个由 prompt 驱动的定时"追踪器",帮我随时跟上 AI 世界的动态。
它们不是传统脚本——每个追踪器都是一份**自然语言指令文件**(`SKILL.md`),
由 AI agent([Claude Code](https://claude.com/claude-code) / 定时任务运行器)按计划执行。
agent 实时联网抓取数据,并以**中英对照简报**的形式,通过带弹窗提醒的 Google 日历事件交付。

## 三个追踪器

| 追踪器 | 频率 | 抓取内容 |
|---|---|---|
| 🧠 **Thinker Tracker(思想者)** | 每天(约 08:00) | AI 大佬观点 + 深度长文/播客/视频/访谈 |
| 🚀 **Feature Tracker(功能)** | 每周五(约 08:00) | 御三家产品功能更新——OpenAI、Anthropic、Google |
| 📈 **Trend Tracker(趋势)** | 每周一、周四(约 08:00) | Product Hunt、Hugging Face、GitHub、Arxiv 热门项目 |

每份简报都是**中英对照**:每条先写英文,然后跟一行中文翻译(`🇨🇳 中文:`)和一句分析(`分析:`)。

## 工作原理

```
定时(cron) ─► AI agent 读取 SKILL.md ─► WebSearch / WebFetch 实时抓取
                                                │
              Google 日历事件 ◄── 生成中英对照简报
              (全天 + 弹窗提醒)
```

**没有可运行的应用代码**——逻辑全在 prompt 里。
要使用某个追踪器,把它的文件夹放进定时任务目录(例如
`~/.claude/scheduled-tasks/<name>/SKILL.md`),并按文件 front-matter 注释里的 cron 设置计划即可。

## 目录结构

```
trackers/
  thinker-tracker/SKILL.md    # 每天
  feature-tracker/SKILL.md    # 每周五
  trend-tracker/SKILL.md      # 每周一/四
examples/                     # 示例输出(中英对照)
```

## 自定义

- **来源 / 人物**:编辑各 `SKILL.md` 里的 "TRACK" / "SOURCES" 段落。
- **时间**:改 front-matter 注释里的 cron,然后用该 cron 重建定时任务。
- **交付方式**:默认用 Google 日历;备选是 Gmail 草稿 + 本地 Markdown 副本。可换成你喜欢的连接器。
- **语言**:去掉 `🇨🇳 中文 / 分析` 指令即可改为纯英文输出。

> 注:交付细节用了占位符(`your-email@example.com`、`~/trackers/...`),运行前请替换成你自己的。

## 许可证

[MIT](LICENSE)
