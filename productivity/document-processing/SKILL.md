---
name: document-processing
description: 文件處理工具技能。當使用者說「處理 PDF」、「讀取 Word」、「轉換文件格式」時，執行文件處理指令。
---

# 文件處理工具

Hermes Agent 內建的文件處理能力。

## 觸發時機

- 「處理 PDF」
- 「讀取 Word 文件」
- 「轉換文件格式」
- 「讀取 PDF」
- 「建立 Word 文件」

## PDF 處理工具

### 讀取 PDF（建議工具）

使用 `read_file` 工具可直接讀取 PDF 內容（.pdf 會自動轉換為文字）。

```bash
# 直接讀取 PDF
read_file(path="~/文件/合約.pdf")
```

### PyMuPDF（需安裝）

```bash
pip3 install PyMuPDF
```

```python
import fitz  # PyMuPDF

# 開啟 PDF
doc = fitz.open("document.pdf")

# 讀取文字
for page in doc:
    print(page.get_text())

# 擷取圖片
for page_num, page in enumerate(doc):
    images = page.get_images()
    for img_index, img in enumerate(images):
        xref = img[0]
        pix = fitz.Pixmap(doc, xref)
        pix.save(f"page_{page_num}_img_{img_index}.png")

doc.close()
```

### pypdf

```bash
pip3 install pypdf
```

```python
from pypdf import PdfReader

reader = PdfReader("document.pdf")
for page in reader.pages:
    print(page.extract_text())
```

### pdfplumber（表格專用）

```bash
pip3 install pdfplumber
```

```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    for page in pdf.pages:
        # 提取文字
        text = page.extract_text()
        # 提取表格
        tables = page.extract_tables()
        for table in tables:
            for row in table:
                print(row)
```

### pdf2image（PDF 轉圖片）

```bash
pip3 install pdf2image
# 還需要安裝 poppler
# macOS: brew install poppler
# Windows: 下載 poppler for Windows
```

```python
from pdf2image import convert_from_path

images = convert_from_path("document.pdf", dpi=200)
for i, image in enumerate(images):
    image.save(f"page_{i+1}.png", "PNG")
```

### reportlab（建立 PDF）

```bash
pip3 install reportlab
```

```python
from reportlab.lib.pagesizes import A4
from reportlab.pdfgen import canvas

c = canvas.Canvas("output.pdf", pagesize=A4)
c.drawString(100, 750, "Hello World")
c.save()
```

## Word 文件處理

### 讀取 Word（python-docx）

```bash
pip3 install python-docx
```

```python
from docx import Document

doc = Document("document.docx")
for para in doc.paragraphs:
    print(para.text)

# 讀取表格
for table in doc.tables:
    for row in table.rows:
        for cell in row.cells:
            print(cell.text)
```

### 建立 Word

```python
from docx import Document
from docx.shared import Pt, RGBColor

doc = Document()

# 添加標題
doc.add_heading("文件標題", 0)

# 添加段落
doc.add_paragraph("這是正文內容")

# 添加格式化的文字
run = doc.add_paragraph().add_run("這是粗體")
run.bold = True

doc.save("output.docx")
```

## 電子表格處理

### openpyxl（Excel）

```bash
pip3 install openpyxl
```

```python
from openpyxl import load_workbook

wb = load_workbook("data.xlsx")
for sheet_name in wb.sheetnames:
    sheet = wb[sheet_name]
    for row in sheet.iter_rows(values_only=True):
        print(row)
```

## 踩坑筆記

| 狀況 | 解法 |
|------|------|
| PDF 中文亂碼 | 使用 `pdfplumber` 而非 `pypdf`，或指定編碼 |
| PDF 表格讀取失敗 | 使用 `pdfplumber.extract_tables()` |
| DOCX 讀取錯誤 | 檢查檔案是否損壞或加密 |
| 圖片解析度太低 | 提高 `dpi` 參數（建議 300+）|

## 安全規則

- ❌ 不處理來路不明的 PDF（可能有惡意程式碼）
- ❌ 不在網路上傳機密文件（除非使用可信的加密服務）
- ✅ 處理前先做 Security Scan