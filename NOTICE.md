# NOTICE · 版权与许可范围

这个仓库把两个上游项目的成果和我自己新写的部分放在了一起。
**三方来源不同、许可不同，所以逐文件列清楚。**

---

## 一、逐文件归属

| 路径 | 来源 | 状态 | 适用许可 |
|---|---|---|---|
| `skills/douyin-distiller/references/lark-write.md` | [jinchenma94/social-media-data-tools](https://github.com/jinchenma94/social-media-data-tools) | **一字未改，原件** | ⛔ 上游未声明许可证，版权归原作者。**不在本仓库 MIT 覆盖范围内** |
| `skills/douyin-distiller/SKILL.md` | 同上，衍生 | 大幅改写（新增搜索博主、多格式导出、蒸馏阶段） | ⚠️ 衍生作品，底层仍源自上游。**不在 MIT 覆盖范围内** |
| `skills/douyin-distiller/references/douyin.md` | 同上，衍生 | 改动约 410 行 | ⚠️ 同上 |
| `skills/douyin-distiller/references/export.md` | ✅ 本仓库新增 | 全新 | **MIT** |
| `panel/index.html` | ✅ 本仓库新增 | 全新 | **MIT** |
| `assets/logo.svg` | ✅ 本仓库新增 | 全新 | **MIT** |
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
- ⛔ **继承自上游的那几个文件，不要打包带出 GitHub 再分发**（放进安装包、上传到别的平台、做成压缩包散发等）
- ✅ 标为 **MIT** 的文件不受此限，随便用

**我无权把别人的代码授权给你。** 这不是谨慎过头，是这份 NOTICE 存在的全部理由。

---

## 三、运行时依赖（不是代码复用）

| 依赖 | 项目 | 许可 | 关系 |
|---|---|---|---|
| 内容蒸馏（RIA-TV++ 流水线） | [kangarooking/cangjie-skill](https://github.com/kangarooking/cangjie-skill) | ⭐ **MIT** | **本仓库不含它的任何文件**，只在运行时调用它 |
| 逐字稿回退转写 | `doubao-video-extract`（豆包工作内置） | — | 同上，运行时调用 |

⭐ `cangjie-skill` 是 MIT 许可的开源项目（9000+ stars）。
本仓库**没有复制它的任何代码**，因此不触发 MIT 的"复制时须附版权声明"义务；
但它是**蒸馏功能的硬依赖**，没装它这部分跑不了，所以在这里郑重登记。

---

## 四、如果上游作者有异议

原作者可以随时通过 [Issues](https://github.com/AppApp777/douyin-distiller/issues) 联系我。
若上游希望移除任何继承文件，或希望调整署名方式，**我会照办，不需要走任何流程**。
