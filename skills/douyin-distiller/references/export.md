# 导出格式说明

本文档说明各种导出格式的生成方法。所有导出文件默认保存到当前工作目录下的 `douyin_data/{博主昵称}_{采集日期YYYYMMDD}_{视频数量}条/` 目录。

## 通用数据结构

所有导出格式基于统一的数据结构：

```python
videos = [
    {
        "id": "7673116537092830577",
        "url": "https://www.douyin.com/video/7673116537092830577",
        "账号名称": "发疯姥子",
        "标题": "一人公司设计产品的正确姿势",
        "介绍": "视频描述或文案全文",
        "逐字稿": "完整口播逐字稿...",
        "点赞": 3073,
        "评论": 120,
        "转发": 45,
        "发表日期": "2026-08-12 17:04",
        "视频时长": "17:04",
        "是否置顶": True,
        "选题方向": ["自媒体运营", "个人成长"],  # 可选
        "主题总结": "一句话概括核心观点",  # 可选
    },
    # ... 更多视频
]
```

---

## 1. Markdown（默认格式）

### 目录结构

```
douyin_data/
└── {博主昵称}_{日期}_{数量}条/
    ├── 01_{视频ID}.md
    ├── 02_{视频ID}.md
    ├── ...
    └── _all.json  # 可选汇总
```

### 单个 Markdown 文件格式

```markdown
# {视频标题}

## 基本信息
- **账号名称**：{博主昵称}
- **发表日期**：{YYYY-MM-DD HH:MM}
- **视频时长**：{时长}
- **视频链接**：{url}
- **点赞**：{点赞数}
- **评论**：{评论数}
- **转发**：{转发数}

## 介绍/文案
{视频介绍或文案全文}

## 选题方向
{标签1}、{标签2}、{标签3}

## 主题总结
{一句话主题总结}

## 逐字稿
{完整口播逐字稿全文}
```

### Python 生成示例

```python
import os
import json

def export_markdown(videos, author_name, output_dir=None):
    if output_dir is None:
        date_str = datetime.now().strftime("%Y%m%d")
        output_dir = f"douyin_data/{author_name}_{date_str}_{len(videos)}条"
    os.makedirs(output_dir, exist_ok=True)

    pad = len(str(len(videos))) if len(videos) > 9 else 2
    for i, v in enumerate(videos, 1):
        seq = str(i).zfill(pad)
        md_path = os.path.join(output_dir, f"{seq}_{v['id']}.md")

        topics = v.get("选题方向", [])
        topics_str = "、".join(topics) if topics else "无"

        md_content = f"""# {v['标题']}

## 基本信息
- **账号名称**：{v['账号名称']}
- **发表日期**：{v.get('发表日期', '未知')}
- **视频时长**：{v.get('视频时长', '未知')}
- **视频链接**：{v['url']}
- **点赞**：{v.get('点赞', 0)}
- **评论**：{v.get('评论', 0)}
- **转发**：{v.get('转发', 0)}

## 介绍/文案
{v.get('介绍', '无')}

## 选题方向
{topics_str}

## 主题总结
{v.get('主题总结', '无')}

## 逐字稿
{v.get('逐字稿', '无逐字稿')}
"""
        with open(md_path, "w", encoding="utf-8") as f:
            f.write(md_content)

    # 可选汇总 JSON
    json_path = os.path.join(output_dir, "_all.json")
    with open(json_path, "w", encoding="utf-8") as f:
        json.dump(videos, f, ensure_ascii=False, indent=2)

    return output_dir
```

---

## 2. Excel

### 格式说明

- 所有视频一个 `.xlsx` 文件
- 第一行为表头
- 每行一条视频
- 列：序号、账号名称、标题、介绍、点赞、评论、转发、发表日期、视频时长、是否置顶、视频链接、选题方向、主题总结、逐字稿

### Python 生成示例（使用 openpyxl）

```python
from openpyxl import Workbook
from openpyxl.styles import Font, Alignment, PatternFill
from openpyxl.utils import get_column_letter

def export_excel(videos, output_path):
    wb = Workbook()
    ws = wb.active
    ws.title = "抖音视频数据"

    # 表头
    headers = ["序号", "账号名称", "标题", "介绍", "点赞", "评论", "转发",
               "发表日期", "视频时长", "是否置顶", "视频链接", "选题方向", "主题总结", "逐字稿"]
    for col, header in enumerate(headers, 1):
        cell = ws.cell(row=1, column=col, value=header)
        cell.font = Font(bold=True)
        cell.fill = PatternFill(start_color="E8F0FE", end_color="E8F0FE", fill_type="solid")

    # 数据行
    for row, v in enumerate(videos, 2):
        topics = v.get("选题方向", [])
        topics_str = "、".join(topics) if topics else ""
        ws.cell(row=row, column=1, value=row-1)
        ws.cell(row=row, column=2, value=v.get("账号名称", ""))
        ws.cell(row=row, column=3, value=v.get("标题", ""))
        ws.cell(row=row, column=4, value=v.get("介绍", ""))
        ws.cell(row=row, column=5, value=v.get("点赞", 0))
        ws.cell(row=row, column=6, value=v.get("评论", 0))
        ws.cell(row=row, column=7, value=v.get("转发", 0))
        ws.cell(row=row, column=8, value=v.get("发表日期", ""))
        ws.cell(row=row, column=9, value=v.get("视频时长", ""))
        ws.cell(row=row, column=10, value="是" if v.get("是否置顶") else "否")
        ws.cell(row=row, column=11, value=v.get("url", ""))
        ws.cell(row=row, column=12, value=topics_str)
        ws.cell(row=row, column=13, value=v.get("主题总结", ""))
        ws.cell(row=row, column=14, value=v.get("逐字稿", ""))

    # 列宽设置
    col_widths = [6, 12, 30, 40, 8, 8, 8, 18, 10, 8, 40, 20, 30, 60]
    for col, width in enumerate(col_widths, 1):
        ws.column_dimensions[get_column_letter(col)].width = width

    # 自动换行
    for row in ws.iter_rows(min_row=2):
        for cell in row:
            cell.alignment = Alignment(wrap_text=True, vertical="top")

    wb.save(output_path)
    return output_path
```

---

## 3. Word

### 格式说明

- 每个视频一个 `.docx` 文件
- 排版美观，含标题、基本信息表格、逐字稿正文
- 文件名：`{序号}_{视频ID}.docx`

### Python 生成示例（使用 python-docx）

```python
from docx import Document
from docx.shared import Pt, Inches, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
import os

def export_word(videos, output_dir):
    os.makedirs(output_dir, exist_ok=True)
    pad = len(str(len(videos))) if len(videos) > 9 else 2

    for i, v in enumerate(videos, 1):
        seq = str(i).zfill(pad)
        doc = Document()

        # 标题
        title = doc.add_heading(v.get("标题", "无标题"), level=1)
        title.alignment = WD_ALIGN_PARAGRAPH.LEFT

        # 基本信息表格
        doc.add_heading("基本信息", level=2)
        table = doc.add_table(rows=5, cols=4, style="Light Grid Accent 1")
        info = [
            ("账号名称", v.get("账号名称", ""), "发表日期", v.get("发表日期", "")),
            ("视频时长", v.get("视频时长", ""), "是否置顶", "是" if v.get("是否置顶") else "否"),
            ("点赞", str(v.get("点赞", 0)), "评论", str(v.get("评论", 0))),
            ("转发", str(v.get("转发", 0)), "视频链接", v.get("url", "")),
            ("选题方向", "、".join(v.get("选题方向", [])) or "无", "", ""),
        ]
        for row_idx, (k1, v1, k2, v2) in enumerate(info):
            table.cell(row_idx, 0).text = k1
            table.cell(row_idx, 1).text = v1
            table.cell(row_idx, 2).text = k2
            table.cell(row_idx, 3).text = v2

        # 介绍/文案
        if v.get("介绍"):
            doc.add_heading("介绍/文案", level=2)
            doc.add_paragraph(v["介绍"])

        # 主题总结
        if v.get("主题总结"):
            doc.add_heading("主题总结", level=2)
            doc.add_paragraph(v["主题总结"])

        # 逐字稿
        doc.add_heading("逐字稿", level=2)
        transcript = v.get("逐字稿", "无逐字稿")
        for para_text in transcript.split("\n"):
            if para_text.strip():
                p = doc.add_paragraph(para_text)
                p.paragraph_format.line_spacing = 1.5

        # 保存
        doc_path = os.path.join(output_dir, f"{seq}_{v['id']}.docx")
        doc.save(doc_path)

    return output_dir
```

---

## 4. PDF

### 格式说明

- 与 Word 内容一致，导出为 PDF
- 可以先生成 Word 再转 PDF，或直接生成 PDF

### 方法一：Word 转 PDF（推荐，Windows）

```python
import os
import win32com.client as win32

def word_to_pdf(word_dir, pdf_dir):
    os.makedirs(pdf_dir, exist_ok=True)
    word = win32.gencache.EnsureDispatch("Word.Application")
    word.Visible = False

    for filename in os.listdir(word_dir):
        if filename.endswith(".docx"):
            word_path = os.path.abspath(os.path.join(word_dir, filename))
            pdf_path = os.path.abspath(os.path.join(pdf_dir, filename.replace(".docx", ".pdf")))
            doc = word.Documents.Open(word_path)
            doc.SaveAs(pdf_path, FileFormat=17)  # 17 = PDF
            doc.Close()

    word.Quit()
    return pdf_dir
```

### 方法二：直接生成 PDF（使用 reportlab）

```python
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.units import cm
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont
import os

def export_pdf(videos, output_dir):
    os.makedirs(output_dir, exist_ok=True)
    # 注册中文字体（Windows 自带）
    pdfmetrics.registerFont(TTFont("SimSun", "C:/Windows/Fonts/simsun.ttc"))

    styles = getSampleStyleSheet()
    title_style = ParagraphStyle("ChineseTitle", parent=styles["Heading1"], fontName="SimSun", fontSize=16, leading=24)
    heading_style = ParagraphStyle("ChineseHeading", parent=styles["Heading2"], fontName="SimSun", fontSize=13, leading=20)
    body_style = ParagraphStyle("ChineseBody", parent=styles["Normal"], fontName="SimSun", fontSize=11, leading=18)

    pad = len(str(len(videos))) if len(videos) > 9 else 2
    for i, v in enumerate(videos, 1):
        seq = str(i).zfill(pad)
        pdf_path = os.path.join(output_dir, f"{seq}_{v['id']}.pdf")
        doc = SimpleDocTemplate(pdf_path, pagesize=A4, topMargin=2*cm, bottomMargin=2*cm)
        story = []

        story.append(Paragraph(v.get("标题", "无标题"), title_style))
        story.append(Spacer(1, 0.5*cm))

        story.append(Paragraph("基本信息", heading_style))
        info_data = [
            ["账号名称", v.get("账号名称", ""), "发表日期", v.get("发表日期", "")],
            ["点赞", str(v.get("点赞", 0)), "评论", str(v.get("评论", 0))],
            ["视频链接", v.get("url", ""), "", ""],
        ]
        table = Table(info_data, colWidths=[3*cm, 5*cm, 3*cm, 5*cm])
        story.append(table)
        story.append(Spacer(1, 0.5*cm))

        if v.get("逐字稿"):
            story.append(Paragraph("逐字稿", heading_style))
            for para in v["逐字稿"].split("\n"):
                if para.strip():
                    story.append(Paragraph(para, body_style))

        doc.build(story)

    return output_dir
```

---

## 5. JSON

### 格式说明

- 所有视频一个 `.json` 文件
- 结构化数据，便于程序处理和二次开发

### Python 生成示例

```python
import json

def export_json(videos, output_path):
    with open(output_path, "w", encoding="utf-8") as f:
        json.dump(videos, f, ensure_ascii=False, indent=2)
    return output_path
```

---

## 导出后验证

无论哪种格式，导出后必须执行验证：

1. **文件数量检查**：导出的文件数量与预期采集数量一致。
2. **抽样内容检查**：随机抽取 2-3 个文件，检查：
   - 逐字稿部分不包含"完整内容"、"web.fetch"、"已通过"等占位符文字
   - 逐字稿长度合理（非纯音乐视频应 > 50 字）
   - 标题、介绍、视频链接、发表日期、账号名称不为空
3. **格式检查**：
   - 点赞、评论、转发为整数
   - 发表日期格式为 `YYYY-MM-DD HH:MM`
   - 选题方向为 1-3 个标签
4. **可打开性检查**：确认生成的文件可以被对应程序正常打开（Excel 用 Excel 打开，Word 用 Word 打开，PDF 用 PDF 阅读器打开）。

验证不通过时，重新生成对应文件，直到全部通过。
