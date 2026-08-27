# Social Media Data Tools

用于整理、导出社交媒体公开数据的 Skill 集合。

目前包含：

- `douyin-transcript-exporter`：导出抖音视频的标题、文案、互动数据与完整逐字稿；可选写入飞书多维表格，或保存到本地。

## 使用前提

`douyin-transcript-exporter` 必须在豆包工作中执行，因为逐字稿获取依赖豆包工作内置的相关能力。

采集抖音主页的公开视频信息时，Skill 会打开豆包工作内置浏览器，需要登录一个抖音账号。建议使用抖音小号：该账号仅用于读取公开主页的视频信息，不涉及视频下载；逐字稿获取也不依赖该账号。

请仅处理你有权访问和使用的内容，并遵守适用的平台规则与法律要求。

## 本地输出

用户明确选择不写入飞书多维表格时，结果会保存到本地：

```text
douyin_data/
└── {博主昵称}_{采集日期}_{视频数量}条/
    ├── 01_{视频ID}.md
    ├── 02_{视频ID}.md
    └── _all.json  # 可选汇总文件
```

每个 Markdown 文件包含视频基本信息、介绍或文案、选题方向、主题总结和逐字稿。

## 使用方式

将 [skills/douyin-transcript-exporter](skills/douyin-transcript-exporter) 导入豆包工作的 Skill 环境后调用，并提供抖音博主主页链接、单条视频链接或分享短链。需要写入飞书多维表格时，再提供目标表格链接；明确选择本地保存时无需提供。

安装时可直接使用以下提示词：

```text
请打开下面的 GitHub 仓库，找到其中的 douyin-transcript-exporter Skill，阅读相关文件并完成安装：
[https://github.com/jinchenma94/social-media-data-tools](https://github.com/jinchenma94/social-media-data-tools)
安装完成后，请告诉我安装结果。
```
