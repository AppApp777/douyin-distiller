<div align="center">

<img src="assets/logo.svg" width="128" alt="douyin-distiller">

# douyin-distiller

**抖音逐字稿采集 · 内容蒸馏 · 零代码指令面板**

把一个博主的全部视频，变成一份你能读、能查、能复用的方法论。

[![License](https://img.shields.io/badge/license-MIT%20(部分)-blue.svg)](LICENSE)
[![Forked from](https://img.shields.io/badge/forked%20from-jinchenma94%2Fsocial--media--data--tools-lightgrey.svg)](https://github.com/jinchenma94/social-media-data-tools)
[![Powered by](https://img.shields.io/badge/蒸馏引擎-cangjie--skill-8A2BE2.svg)](https://github.com/kangarooking/cangjie-skill)
[![Runtime](https://img.shields.io/badge/运行环境-豆包工作-orange.svg)](#使用前提)

</div>

---

## 这是什么

想搞清楚一个博主到底在讲什么，最笨的办法是把他的视频一条条看完。
稍微省力一点的办法是把视频下载下来跑语音识别——但那要装环境、要等转写，一条几分钟。

**这个项目走的是第三条路：抖音的视频页本身就带完整逐字稿，直接读就行。**

于是整条链路变成：

```
说一个博主的名字
   ↓  自动搜索并进入主页
拉取全部作品（标题 / 点赞 / 时长 / 链接）
   ↓  你勾选要哪几条
批量抓取完整逐字稿
   ↓
导出 Markdown / Excel / Word / PDF / JSON / 飞书表格
   ↓  可选
蒸馏成方法论 Skill ＋ 一篇精华长文
```

全程不写一行代码，也不需要自己找链接。

---

## 它解决的三个卡点

| 卡点 | 以前 | 现在 |
|---|---|---|
| **不知道链接** | 得先手动翻到主页复制 URL | 说名字就行，自动搜索匹配 |
| **不知道怎么跟 AI 说** | 提示词写不对，AI 答非所问 | [指令面板](#指令面板)点几下，生成一条现成的指令 |
| **拿到逐字稿之后呢** | 几万字堆在那里，读不完也用不上 | 蒸馏成结构化方法论，每条自带步骤和边界 |

---

## 使用前提

- ⚠️ **必须在[豆包工作](https://www.doubao.com/)中运行**。逐字稿依赖它内置的网页解析能力，
  普通环境直接请求抖音只会拿到风控空页。
- 采集博主主页时需要在内置浏览器里登录抖音账号（建议用小号，仅用于读取公开主页信息）。
  首次运行会停下来请你扫码，**这一步无法自动化，抖音有滑块和短信验证**。
- 请仅处理你有权访问和使用的公开内容，遵守平台规则与法律要求。

---

## 安装

把本仓库 `skills/` 下的**两个**文件夹整个复制到豆包工作的 skills 目录（`.user_skills/`）：

```text
skills/douyin-distiller/   →  .user_skills/douyin-distiller/
skills/cangjie-skill/      →  .user_skills/cangjie-skill/
```

也可以直接把下面这段话发给豆包工作，让它自己去装：

```text
请打开下面的 GitHub 仓库，找到其中的 douyin-distiller Skill，阅读相关文件并完成安装：
https://github.com/AppApp777/douyin-distiller
安装完成后，请告诉我安装结果。
```

⭐ **蒸馏引擎已经随仓库一起带上了**，不用再去别处找。
`skills/cangjie-skill/` 是 [kangarooking/cangjie-skill](https://github.com/kangarooking/cangjie-skill)（MIT）的原样副本，
许可证随附。没有它，采集和导出照常工作，但"蒸馏"跑不起来。

逐字稿抓取失败时的回退路径（下载音频 → 飞书妙记转写）依赖豆包工作内置的
`doubao-video-extract`。这条路很慢（每条 1–3 分钟），只在主路径失败时才走。

👉 手把手的图文版见 [docs/安装指南.md](docs/安装指南.md)。

---

## 怎么用

**入口是面板，不是打字。而且面板不用你开。**

1. 跟豆包工作说一句要采集谁 —— 比如"帮我采集七也的抖音视频"
2. ⭐ **AI 自己就把面板拉出来了**，你只管在上面按四步填表
3. 填完说一句：**按面板执行**

AI 会自己读面板上的参数，然后开始干活。**你不需要会写提示词，也不需要去文件夹里找 html 双击。**

> 面板文件在 `.user_skills/douyin-distiller/douyin-distiller-panel.html`，
> 想自己双击打开也行，但正常流程里用不着。

支持的输入：**博主昵称 · 抖音号 · 主页链接 · 单条视频链接 · 分享短链**。

👉 完整流程见 [docs/使用教程.md](docs/使用教程.md)，卡住了看 [docs/常见问题.md](docs/常见问题.md)。

<details>
<summary>不想用面板？也可以直接说人话（不推荐，参数容易漏）</summary>

```text
帮我采集七也的抖音视频
七也的作品，帮我导出逐字稿
采集七也最新 10 条视频，要逐字稿，导出 Word
把这个主页的视频都抓下来：https://www.douyin.com/user/<主页链接>
```

</details>

---

## 指令面板

`skills/douyin-distiller/douyin-distiller-panel.html` 是一个**离线的指令生成器**——不联网、不装依赖、单文件。

⭐ **它由 Skill 自己拉起来**：只要你提到采集/逐字稿/蒸馏，AI 的第一个动作就是在内置浏览器里把它打开。
这是硬性的——SKILL.md 里写死了“禁止跳过面板直接采集”。

四步向导：

1. **确认博主** —— 昵称 / 抖音号切换，重名提示，找抖音号的引导
2. **采集参数** —— 抓多少条、按最新还是按最热排序、要不要逐字稿、要不要 AI 扩展字段
3. **导出与蒸馏** —— 选格式（Markdown / Excel / Word / PDF / JSON / 飞书表格）、选蒸馏强度
4. **确认执行** —— 指令预览，然后去对话框说"按面板执行"

⭐ **它不执行任何操作，也不联网**，只负责把你的选择翻译成一句 AI 能准确理解的话。
单文件 HTML，零外部依赖，源码可读。

⭐ 因为它是在**豆包工作的内置浏览器**里打开的，AI 能直接读到你填的参数，连复制粘贴都省了。
你要是自己在外部浏览器打开它，就用面板上的"复制指令"按钮把指令粘过去。

⛔ **全仓库只有这一份面板文件。** 之前根目录还放过一份副本，两份会各自漂移——已经删掉了。

---

## 能导出什么

| 格式 | 形态 | 适合 |
|---|---|---|
| **Markdown**（默认） | 每个视频一个 `.md` | 通用、可编辑、直接读 |
| **Excel** | 一个 `.xlsx`，每行一条 | 筛选、排序、数据分析 |
| **Word** | 每个视频一个 `.docx` | 打印、分享、正式文档 |
| **PDF** | 同 Word | 不可编辑的交付 |
| **JSON** | 一个结构化 `.json` | 二次开发 |
| **飞书多维表格** | 直接写入你指定的表 | 在线协作 |

采集字段：账号名称 · 标题 · 介绍文案 · **完整逐字稿** · 点赞 · 评论 · 转发 · 发表日期 · 视频链接 · 时长 · 是否置顶。

可选的 AI 扩展字段：选题方向（标签）· 主题总结（一句话）。

---

## 蒸馏是怎么做的

蒸馏不是摘要。它调用 [cangjie-skill](https://github.com/kangarooking/cangjie-skill) 的 RIA-TV++ 流水线，
把逐字稿拆成**可执行的方法论单元**：

1. **整稿理解** —— 主旨、骨架、核心概念
2. **并行提取** —— 框架 / 原则 / 案例 / 反例 / 术语，五类候选
3. **三重验证** —— 跨域佐证（至少 2 处独立支持）、预测力（能回答原文没明说的问题）、独特性（不是常识）
4. **六维结构化** —— 原文引用 · 重写 · 原案例 · 未来触发场景 · 可执行步骤 · **适用边界**
5. **压力测试** —— 带诱饵题，不通过的回炉
6. **交付** —— 每个方法论一份 SKILL.md ＋ 一篇 `DIGEST.md` 精华长文

⭐ **有可执行步骤和边界，才算方法论；只有观点的，会在第 3 步被淘汰。**

---

## 质量约束（继承自上游，本项目保留）

这套约束是上游项目最有价值的部分，本仓库原样保留并加强：

- ⛔ **禁止占位符**。绝不允许用"完整内容已获取"这类文字冒充逐字稿。
- ⛔ **必须分页读完**。返回被截断时继续读，直到 `end_offset >= total_length`。
- ⛔ **抓取失败就留空并写明原因**，不许含糊带过。
- ✅ **导出后必须回读验证**：占位符检查、逐字稿长度检查、关键字段非空检查。

---

## 仓库结构

```text
douyin-distiller/
├── skills/
│   ├── douyin-distiller/            ← 采集 Skill，复制到 .user_skills/
│   │   ├── SKILL.md
│   │   ├── douyin-distiller-panel.html          ← 面板（全仓唯一一份）
│   │   └── references/
│   │       ├── douyin.md            登录处理 · 选择器 · 逐字稿提取 · 常见异常
│   │       ├── export.md            六种导出格式怎么生成
│   │       ├── lark-write.md        飞书多维表格写入
│   │       └── distillation.md      蒸馏流水线与 cangjie-skill 的对接
│   └── cangjie-skill/               ← 蒸馏引擎，原样收录（MIT），也复制到 .user_skills/
├── docs/                            安装指南 · 使用教程 · 常见问题
├── assets/                          logo（标准版 / 小尺寸版 / 头像版）
├── NOTICE.md                        ⭐ 逐文件的来源与许可
└── LICENSE
```

---

## 技术来源

这个项目**不是从零写的**，它站在两个上游项目上。逐项列清楚：

| 部分 | 来源 | 许可 |
|---|---|---|
| **抖音采集与逐字稿链路** | [jinchenma94/social-media-data-tools](https://github.com/jinchenma94/social-media-data-tools) —— 本仓库 fork 自它 | ⛔ 上游未声明许可证 |
| **内容蒸馏方法论（RIA-TV++）** | [kangarooking/cangjie-skill](https://github.com/kangarooking/cangjie-skill) —— **原样收录在 `skills/cangjie-skill/`**，许可证随附 | ⭐ MIT（原作者的） |
| **多格式导出 · 指令面板 · 博主搜索 · 文档 · 图标** | 本仓库新增 | ⭐ MIT |

**逐文件的归属、状态和适用许可，全部列在 [NOTICE.md](NOTICE.md) 里。**
包括哪些文件一字未改、哪些是衍生、哪些是全新的。

⚠️ **重要**：上游 `social-media-data-tools` 没有 LICENSE 文件。公开 ≠ 开源。
本仓库依据 GitHub 服务条款的 fork 条款存在，而该授权**仅在 GitHub 平台内有效**。
继承自上游的文件请不要打包带出 GitHub 再分发。详见 [NOTICE.md](NOTICE.md)。

---

## 许可

**MIT，但只覆盖本仓库新增的部分。** 见 [LICENSE](LICENSE) 与 [NOTICE.md](NOTICE.md)。

---

## 致谢

- [@jinchenma94](https://github.com/jinchenma94) —— 抖音逐字稿采集链路的原作者。
  是他先发现了那条"不用下载视频也能拿到逐字稿"的路。
- [@kangarooking](https://github.com/kangarooking) —— cangjie-skill 作者。
  RIA-TV++ 那套蒸馏方法论是这个项目的另一半。

---

## 免责

本项目只处理**公开可见**的内容，不绕过任何登录墙或付费墙，不下载视频文件。
使用者需自行确认对所处理内容拥有合法的访问与使用权利，并遵守抖音平台规则与所在地法律。
作者不对使用本项目产生的任何后果负责。
