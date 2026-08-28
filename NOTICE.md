# NOTICE · 版权与许可范围

这个仓库把两个上游项目的成果和我自己新写的部分放在了一起。
**三方来源不同、许可不同，所以逐文件列清楚。**

---

## 一、逐文件归属

| 路径 | 来源 | 状态 | 适用许可 |
|---|---|---|---|
| `skills/douyin-distiller/references/lark-write.md` | [jinchenma94/social-media-data-tools](https://github.com/jinchenma94/social-media-data-tools) | **一字未改，原件** | ⛔ 上游未声明许可证，版权归原作者。**不在本仓库 MIT 覆盖范围内** |
| `skills/douyin-distiller/SKILL.md` | 同上，衍生 | 大幅改写（新增搜索博主、多格式导出、蒸馏阶段、面板硬绑定） | ⚠️ 衍生作品，底层仍源自上游。**不在 MIT 覆盖范围内** |
| `skills/douyin-distiller/references/douyin.md` | 同上，衍生 | 改动约 410 行 | ⚠️ 同上 |
| `skills/cangjie-skill/**` | [kangarooking/cangjie-skill](https://github.com/kangarooking/cangjie-skill) | **原样收录**，只去掉了 `.github/`（理由见第三节） | ⭐ **MIT（原作者的）**，许可证全文随附在 `skills/cangjie-skill/LICENSE` |
| `skills/douyin-distiller/douyin-distiller-panel.html` | ✅ 本仓库新增 | 全新 | **MIT** |
| `skills/douyin-distiller/references/export.md` | ✅ 本仓库新增 | 全新 | **MIT** |
| `skills/douyin-distiller/references/distillation.md` | ✅ 本仓库新增 | 全新 | **MIT** |
| `docs/安装指南.md` · `docs/使用教程.md` · `docs/常见问题.md` | ✅ 本仓库新增 | 全新 | **MIT** |
| `assets/logo.svg` · `logo-small.svg` · `logo-icon.svg` | ✅ 本仓库新增 | 全新 | **MIT** |
| `README.md` · `NOTICE.md` · `LICENSE` | ✅ 本仓库新增 | 全新 | **MIT** |

---

## 二、为什么要分层

**上游 [jinchenma94/social-media-data-tools](https://github.com/jinchenma94/social-media-data-tools) 没有 LICENSE 文件。**

在 GitHub 上，公开 ≠ 开源。没有许可证的公开代码，默认适用著作权法，
即**作者保留一切权利**，未授予任何人复制、修改、再分发的权利。

本仓库之所以能存在，依据是 [GitHub 服务条款第 D.5 条](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service#5-license-grant-to-other-users)：

> By making a repository public, you grant other Users a nonexclusive, worldwide license
> to use, display, perform and reproduce (**by forking**) Your Content **through the Service**
> as permitted by GitHub's functionality.

⚠️ 注意 **"through the Service"** —— 这项授权**只在 GitHub 平台内有效**。

⇒ 因此：

- ✅ 你可以在 GitHub 上 fork 本仓库、在自己的 fork 里查看和修改
- ⛔ **继承自上游的那三个文件，不要打包带出 GitHub 再分发**（放进安装包、上传到别的平台、做成压缩包散发等）
- ✅ 标为 **MIT** 的文件不受此限，随便用
- ✅ `skills/cangjie-skill/` 是原作者以 MIT 发布的，**再分发本来就被允许**，不受这一条约束

**我无权把别人的代码授权给你。** 这不是谨慎过头，是这份 NOTICE 存在的全部理由。

---

## 三、cangjie-skill：从"运行时依赖"改成"随仓分发"（2026-08-28 改口）

⚠️ **本文件此前写的是"本仓库不含它的任何文件，只在运行时调用它"。那句话现在不成立，特此更正。**
2026-08-28 起，[kangarooking/cangjie-skill](https://github.com/kangarooking/cangjie-skill) 的全部文件
随本仓库一起分发，位置在 `skills/cangjie-skill/`。

**为什么要打包进来**：蒸馏是本项目的硬依赖，分成两个仓库让使用者自己去装，
等于把最容易卡住新手的一步留给了新手。装一次拿全套，跟这个项目"零代码、填表即用"的定位是一致的。

**这么做在许可上成立**：cangjie-skill 是 MIT。MIT 明确允许再分发，
条件是**随副本附上版权声明与许可证全文**——`skills/cangjie-skill/LICENSE` 原样保留，正是为了满足这一条。
目录里的 README、方法论文档、图片资源一并原样保留，不做删改，也不改署名。

**唯一删掉的东西**：`.github/`（里面是 `update-star-history.yml`）。
那是原仓库自己的 GitHub Actions 工作流，跟功能无关；**留在这里会在本仓库上自动执行**，
所以排除掉。除此之外一个字没动。

---

## 四、运行时依赖（这些才是真的不含代码）

| 依赖 | 项目 | 关系 |
|---|---|---|
| 逐字稿抓取环境 | 豆包工作（WorkBuddy）内置浏览器与页面解析 | 运行环境，本仓库不含其任何代码 |
| 逐字稿回退转写 | `doubao-video-extract`（豆包工作内置） | 主路径失败时才调用，本仓库不含其任何代码 |

---

## 五、如果上游作者有异议

原作者可以随时通过 [Issues](https://github.com/AppApp777/douyin-distiller/issues) 联系我。
若上游希望移除任何继承文件、撤下随仓分发的副本，或希望调整署名方式，
**我会照办，不需要走任何流程。**
