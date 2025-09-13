# Markdown as Content Layer - Best Practice Approach

## Triết lý: Content vs Formatting Separation

### **Markdown = Content Layer**
- ✅ **Focus 100% vào nội dung**
- ✅ **AI-friendly** - dễ AI hiểu và chỉnh sửa
- ✅ **Human-readable** - dễ review và collaborate
- ✅ **Version control friendly** - Git diff rõ ràng

### **Tools = Formatting Layer**
- ✅ **Pandoc** - convert sang DOCX/PDF
- ✅ **LaTeX** - advanced typography
- ✅ **Custom scripts** - specialized formatting

---

## Markdown Content Structure

### **Basic Structure (No Complex Numbering)**
```markdown
# Giới thiệu

## Bối cảnh nghiên cứu
Nội dung...

## Mục tiêu nghiên cứu
Nội dung...

# Tổng quan tài liệu

## Cơ sở lý thuyết
Nội dung...

## Các nghiên cứu liên quan
Nội dung...
```

### **Formatting Instructions trong Markdown**

#### 1. **Page Breaks**
```markdown
# Giới thiệu

Nội dung chương...

<!-- PAGE_BREAK -->

# Tổng quan tài liệu
```

#### 2. **New Sections**
```markdown
## Phương pháp nghiên cứu

Nội dung...

<!-- NEW_SECTION -->

## Kết quả nghiên cứu
```

#### 3. **Landscape Pages**
```markdown
## Bảng kết quả

<!-- LANDSCAPE_PAGE -->

| Cột 1 | Cột 2 | Cột 3 |
|-------|-------|-------|
| Data  | Data  | Data  |

<!-- PORTRAIT_PAGE -->

## Phân tích kết quả
```

#### 4. **Figure Placement**
```markdown
## Kết quả thí nghiệm

<!-- FIGURE: experiment-setup.png -->
*Hình 1: Sơ đồ thí nghiệm*

Nội dung phân tích...
```

#### 5. **Table Formatting**
```markdown
## Thống kê mô tả

<!-- TABLE: wide-format -->
| Biến số | N | Mean | SD | Min | Max |
|---------|---|------|----|----|-----|
| Age     |100| 25.3 |4.2 |18  |45   |
| Score   |100| 78.5 |12.1|45  |98   |
```

#### 6. **Citation Instructions**
```markdown
## Literature Review

Theo nghiên cứu của Smith (2023), việc sử dụng AI...

<!-- CITATION: smith2023 -->
<!-- BIBLIOGRAPHY: references.bib -->
```

---

## Tool Conversion Pipeline

### **Pandoc với Custom Filters**

#### 1. **Page Break Filter**
```python
# filter-page-breaks.py
import pandocfilters as pf

def page_break(key, value, format, meta):
    if key == 'RawBlock' and value[0] == 'html' and value[1] == '<!-- PAGE_BREAK -->':
        return pf.RawBlock('latex', '\\newpage')

if __name__ == '__main__':
    pf.toJSONFilter(page_break)
```

#### 2. **Landscape Filter**
```python
# filter-landscape.py
def landscape(key, value, format, meta):
    if key == 'RawBlock' and value[0] == 'html' and value[1] == '<!-- LANDSCAPE_PAGE -->':
        return pf.RawBlock('latex', '\\begin{landscape}')
    elif key == 'RawBlock' and value[0] == 'html' and value[1] == '<!-- PORTRAIT_PAGE -->':
        return pf.RawBlock('latex', '\\end{landscape}')
```

### **Pandoc Commands với Filters**

#### **DOCX Output**
```bash
pandoc input.md \
  --filter=filter-page-breaks.py \
  --filter=filter-landscape.py \
  --reference-doc=academic-template.docx \
  --bibliography=references.bib \
  --csl=apa.csl \
  -o output.docx
```

#### **PDF Output (LaTeX)**
```bash
pandoc input.md \
  --filter=filter-page-breaks.py \
  --filter=filter-landscape.py \
  --template=academic-template.tex \
  --bibliography=references.bib \
  --csl=apa.csl \
  --pdf-engine=xelatex \
  -o output.pdf
```

---

## Advanced Formatting Instructions

### **Custom Markdown Extensions**

#### 1. **Chapter Numbering**
```markdown
# {chapter:1} Giới thiệu
# {chapter:2} Tổng quan tài liệu
```

#### 2. **Section Numbering**
```markdown
## {section:1.1} Bối cảnh nghiên cứu
## {section:1.2} Mục tiêu nghiên cứu
```

#### 3. **Figure References**
```markdown
Như thể hiện trong {fig:experiment-setup}, quá trình thí nghiệm...

<!-- FIGURE: experiment-setup.png -->
*Hình {fig:experiment-setup}: Sơ đồ thí nghiệm*
```

#### 4. **Table References**
```markdown
Kết quả thống kê được trình bày trong {tab:descriptive-stats}.

<!-- TABLE: descriptive-stats -->
| Biến số | N | Mean | SD |
|---------|---|------|----|
| Age     |100| 25.3 |4.2 |

*Bảng {tab:descriptive-stats}: Thống kê mô tả*
```

### **Processing Script**
```python
# process-markdown.py
import re

def process_markdown(content):
    # Process chapter numbering
    content = re.sub(r'# \{chapter:(\d+)\} (.+)', r'# Chương \1: \2', content)
    
    # Process section numbering
    content = re.sub(r'## \{section:([\d.]+)\} (.+)', r'## \1. \2', content)
    
    # Process figure references
    content = re.sub(r'\{fig:(\w+)\}', r'Hình \1', content)
    
    # Process table references
    content = re.sub(r'\{tab:(\w+)\}', r'Bảng \1', content)
    
    return content
```

---

## Workflow Integration

### **Cursor IDE Setup**

#### 1. **Extensions cần thiết**
- **Markdown All in One** - basic functionality
- **Pandoc** - external tool
- **Custom snippets** - formatting instructions

#### 2. **Custom Snippets**
```json
{
  "Page Break": {
    "prefix": "pagebreak",
    "body": "<!-- PAGE_BREAK -->",
    "description": "Insert page break"
  },
  "Landscape Page": {
    "prefix": "landscape",
    "body": "<!-- LANDSCAPE_PAGE -->\n$1\n<!-- PORTRAIT_PAGE -->",
    "description": "Insert landscape page"
  },
  "Figure": {
    "prefix": "fig",
    "body": "<!-- FIGURE: $1 -->\n*Hình $2: $3*",
    "description": "Insert figure"
  }
}
```

### **Build Scripts**

#### **Simple Build Script**
```bash
#!/bin/bash
# build.sh

echo "Processing Markdown..."

# Process custom instructions
python process-markdown.py input.md > temp/processed.md

# Convert to DOCX
pandoc temp/processed.md \
  --filter=filter-page-breaks.py \
  --filter=filter-landscape.py \
  --reference-doc=academic-template.docx \
  --bibliography=references.bib \
  --csl=apa.csl \
  -o output/document.docx

# Convert to PDF
pandoc temp/processed.md \
  --filter=filter-page-breaks.py \
  --filter=filter-landscape.py \
  --template=academic-template.tex \
  --bibliography=references.bib \
  --csl=apa.csl \
  --pdf-engine=xelatex \
  -o output/document.pdf

echo "Documents generated successfully!"
```

---

## Benefits của Approach này

### **1. Content Focus**
- ✅ **AI-friendly** - Markdown đơn giản, AI hiểu tốt
- ✅ **Human-readable** - dễ review và edit
- ✅ **Version control** - Git diff rõ ràng

### **2. Formatting Flexibility**
- ✅ **Multiple outputs** - DOCX, PDF, HTML
- ✅ **Custom formatting** - full control
- ✅ **Professional quality** - publication-ready

### **3. Workflow Efficiency**
- ✅ **Separation of concerns** - content vs formatting
- ✅ **Automation** - build scripts
- ✅ **Collaboration** - Markdown dễ share

### **4. Scalability**
- ✅ **Large documents** - books, theses
- ✅ **Complex formatting** - academic papers
- ✅ **Multiple formats** - different requirements

---

## Kết luận

**Markdown as Content Layer** là approach optimal cho academic writing:

1. **Focus 100% vào nội dung** trong Cursor IDE
2. **AI assistance** hiệu quả với Markdown đơn giản
3. **Formatting instructions** embedded trong content
4. **Professional output** thông qua conversion tools
5. **Flexible workflow** - dễ adapt cho different requirements

**→ Đây chính là sweet spot cho nhu cầu của bạn!**
