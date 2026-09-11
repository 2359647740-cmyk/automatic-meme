# self-mentor · 澄己

> **把自己的收藏、笔记和聊天记录，蒸馏成一个最懂你的学习提升助手。**
>
> Turn your saved articles, notes and chat logs into knowledge cards and a personal
> cognitive profile — a study companion that actually knows you.

---

## 这是什么

`self-mentor` 是一个 WorkBuddy / Claude Code 类的 **Agent Skill**。它把你散落各处的素材
（收藏的文章、自己的笔记、微信/QQ 聊天记录、日记）一步步蒸馏成两层资产：

1. **知识卡片** —— 从素材里提炼出的可复用观点、方法、框架；
2. **个人认知画像** —— 你的思维框架、决策倾向、认知盲区，以及"绝对不做的事"清单。

蒸馏结果不是塞进某个数据库，而是落成**一堆 markdown 文件**。配套的「观照台」界面
（`ui/index.html`）只是这些文件的视图 —— 文件夹即数据库，没有服务、没有外部依赖。

## 核心原则

| 原则 | 含义 |
| --- | --- |
| **文件夹即数据库** | 所有状态都是本目录下的 markdown，可读、可 diff、可版本管理 |
| **人在回路** | 你随时能改卡片、写纠错；**人的批注永远优先于 Agent 的输出** |
| **raw 只增不改** | 原始素材进 `raw/` 后不再改动，蒸馏产物与素材分离 |
| **增量不覆盖** | 再次蒸馏只处理变化部分，冲突显式标 `[⚠️ 冲突]` 而不是悄悄覆盖 |

## 目录结构

```
self-mentor/
├── raw/articles/    原始素材（只增不改）
├── inbox/           待蒸馏队列（工作台「导入」的落点）
├── wiki/cards/      知识卡片 —— 核心资产，人可随时手改
├── persona/
│   ├── SELF_PERSONA.md   当前生效的认知画像
│   ├── SELF_MEMORY.md    长期记忆：方法论与知识沉淀
│   └── snapshots/        每次深度蒸馏自动快照 = 免费版本管理
├── audit/           人工纠错收件箱
├── plans/           学习规划 / 任务拆解（对话产出落盘于此）
├── log/             操作日志，一天一个文件（驱动成长曲线与热力图）
├── prompts/         四个操作的详细 Prompt
├── tools/           聊天记录 / 社交媒体解析器
└── ui/index.html    观照台界面
```

## 四个操作

| 操作 | 触发 | 做什么 |
| --- | --- | --- |
| **ingest** | 给 URL / 粘贴文章 / 丢文件进 `inbox/` | 落盘素材 → 蒸馏成知识卡片 |
| **distill** | 新卡片攒够 5 张，或你说"更新画像" | 增量更新认知画像 + 自动快照 |
| **chat** | 任何其它提问 | 加载画像 + 检索卡片，按导师模式回答（规划必落盘到 `plans/`） |
| **audit** | 说"我不会这样想" / 直接改卡片 | 记录纠错 → 改原文 → 立即生效 |

## 观照台（界面）

`ui/index.html` 在浏览器里直接打开，选择本目录即可。界面由**风格包**定义 —— 不是换色，
而是整套排版结构、字体阶梯与签名视觉（canvas 渲染）。内置六套：

`日光手记` · `星图夜幕` · `深渊终端` · `黑曜蓝图` · `暖纸笔记` · `matcha`

## 安装

把整个目录放进 skills 目录即可：

```bash
git clone <本仓库地址> ~/.workbuddy/skills/self-mentor
```

然后照着 `SKILL.md` 的启动协议走：状态扫描 → 首运行门（先选界面风格）→ 宣告计划。

## 本仓库包含什么 / 不包含什么

**包含**：`SKILL.md`、`prompts/`、`tools/`、`ui/`（含六套主题），以及两个 MIT 许可的
参考实现。

**不包含**（已由 `.gitignore` 挡住）：

| 目录 | 原因 |
| --- | --- |
| `raw/`、`persona/`、`wiki/`、`log/`、`audit/`、`plans/`、`inbox/` | 这些是**使用者的个人数据** —— 简历、认知画像、工作日志。跑起来之后会自动生成 |
| `references/llm-wiki-skill/` | 上游未声明 LICENSE，不便随本仓库分发 |
| `references/styles/` | 抓取自第三方设计规范的参考文件 |

克隆下来是**空壳框架**，第一次运行时会自己长出那些目录 —— 这是有意为之。

## 致谢与许可

本技能的蒸馏方法论整合自三个开源项目，原作者版权归各自所有：

| 项目 | 用途 | 许可 |
| --- | --- | --- |
| [llm-wiki-skill](https://github.com/) | "文件夹即知识库 + 人在回路" | 未声明，**未随本仓库分发** |
| [yourself-skill](https://github.com/) | Persona / Memory 双层结构、增量 merge、对话纠正 | MIT，见 `references/yourself-skill/LICENSE` |
| [yupi-skill](https://github.com/) | 按问题类型切换回答模式、"绝对不做"清单 | MIT，见 `references/yupi-skill/LICENSE` |

随仓库分发的两个参考实现**完整保留其原始 LICENSE 文件**。除第三方部分外，
本仓库其余内容著作权归作者所有。
