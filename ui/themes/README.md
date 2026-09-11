# 工作台风格包（主题）

工作台界面由"风格包"定义——不是换色，而是一整套排版、字体阶梯、组件气质的复刻。本目录下每个 `.json` 就是一套完整风格。

## 内置风格包

| 文件 | 风格 | 复刻自 |
|---|---|---|
| `日光手记.json` | 明亮天蓝画布、粉彩卡片、超大紧排标题 | Geniestudio |
| `星图夜幕.json` | 纯黑星夜、紫色微光、居中巨字 | Dala |
| `深渊终端.json` | 青水深渊、动力学超大数字、仪器面板标签 | Auros |
| `黑曜蓝图.json` | 黑曜石底、发丝线网格、零圆角图纸感 | Hyperstudio |
| `暖纸笔记.json` | 暖纸底色、白卡细线、衬线引用的杂志感 | Notion |

另有一个内置保底主题「暗金 · 默认」（写在 `ui/index.html` 里，不依赖本目录）。

## 选择机制

- **测试期**：工作台右上角下拉随时切换；URL 加 `?theme=<文件名去掉 .json>` 可直达，例如 `?theme=星图夜幕`。
- **选定后**：把选择写进 `ui/config.json`：`{"theme": "星图夜幕"}`，之后打开工作台默认就是它（下拉仍可临时切换，优先级：URL 参数 > 上次选择 > config.json > 默认）。

## 新增一套风格

对 Agent（Kimi Code / Claude Code）说：

> 参考 https://styles.refero.design/style/<某个风格id> 给 self-mentor 做一套风格包

Agent 会抓取该风格页的完整设计规范（DESIGN.md：色板、字体阶梯、圆角、阴影、布局、组件），在本目录生成一个中文名 `.json`，并把它加进 `index.json` 清单。也可以直接给任意网站 URL 让 Agent 提取设计语言。

参考规范原文存档在 `references/styles/`（每套风格一份 `.md`）。

## 手动方式：自己写 JSON

在本目录新建 `我的风格.json`：

```json
{
  "name": "我的风格",
  "mode": "dark",
  "source": "https://参考网站（可选，仅作记录）",
  "vars": {
    "--bg": "#101014",
    "--card": "#1b1b22",
    "--line": "#2c2c36",
    "--text": "#eceae4",
    "--accent": "#7ec8a9",
    "--accent-dim": "rgba(126,200,169,.12)",
    "--accent-text": "#101014",
    "--radius": "10px",
    "--font-display": "-apple-system, \"PingFang SC\", sans-serif"
  },
  "css": ".hero-title { font-weight: 400; } .stat { border-radius: 0; }"
}
```

规则：

- `mode` 只能是 `"dark"` 或 `"light"`，决定浏览器原生控件（滚动条等）的明暗
- `vars` 只写要覆盖的设计令牌，缺省回落到「暗金 · 默认」
- `css` 是整段样式覆盖——排版、布局、组件气质都在这里改；参考现有五个风格包的写法
- 新文件要加进 `index.json` 的 `themes` 数组（HTTP/演示模式靠它发现主题；文件夹模式会遍历整个目录，不加也能读到）
- 改完在工作台点「重新读取」，或刷新页面后重新选择文件夹

## 签名视觉（signature）

每套风格包至少要有一个标志性视觉，不只是排版差异。JSON 里加 `signature` 字段：

```json
"signature": {"type": "globe", "shape": "tri", "count": 1500, "colors": ["#8952ff", "#ffb829"], "speed": 0.003}
```

- `globe`：canvas 粒子星球（`shape`: `tri` 三角粒子 / `dot` 点阵），由主题 css 用 `.sig` 选择器定位和定尺寸（默认 `display:none`，启用时要写 `.sig { display:block; ... }`）
- `stars`：星座图——亮星闪烁 + 近邻连线 + 星芒，参数 `count`、`colors`
- `earth`：点阵地球——大陆团块轮廓 + 自转，参数 `count`、`colors`（[陆地, 海洋, 高光]）、`speed`
- `adam`：创世纪·两只将近相触的手（点绘，触点辉光脉动），横向构图，适合摆成全宽短横幅
- `brain`：大脑神经网络——皮质褶皱轮廓 + 突触节点 + 游走脉冲
- `network`：生长的知识网络——节点渐次点亮 + 连线 + 呼吸漂移，参数 `count`、`colors`
- `kinetic`：动力学大字跑马灯，内容由数据自动生成（热门标签 + 卡片/素材数），样式由主题 css 里的 `.kinetic-track` 定义
- `ambient`（可选子字段）：全屏漂移粒子氛围层，渲染在画布 `#sig-ambient`（z-index -2，页面最底层），写法 `"ambient": {"count": 70, "colors": [...], "shape": "tri"}`
- 缺省或 `"none"`：无签名视觉（克制型风格也可以不启用）

现有签名（每套主题一个，互不重复）：日光手记 = 大脑神经网络（hero 右列）、星图夜幕 = 全宽星空带（hero 顶部）+ 氛围粒子、深渊终端 = 点阵地球（hero 右侧仪器视窗）、黑曜蓝图 = 创世纪双手点绘（hero 下方全宽横幅）、暖纸笔记 = 知识网络（文首插图）。
