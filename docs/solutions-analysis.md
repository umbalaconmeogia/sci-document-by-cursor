# Phân tích giải pháp cho các vấn đề Markdown

## Tổng quan 2 hướng tiếp cận

### Hướng 1: Plugin/Extension cho Cursor IDE
- **Ưu điểm**: Giữ được workflow trong Cursor, real-time preview
- **Nhược điểm**: Phụ thuộc vào ecosystem plugins, có thể thiếu tính năng

### Hướng 2: Markdown → LaTeX/Advanced Tools
- **Ưu điểm**: Professional formatting, đầy đủ tính năng academic
- **Nhược điểm**: Cần học thêm tools, workflow phức tạp hơn

---

## Giải pháp chi tiết cho từng vấn đề

### 1. Numbering Heading ✅ PLUGIN SOLUTION

**Hướng 1 - Cursor Extensions:**

**a) Markdown All in One Extension**
- ✅ Có sẵn trong VSCode marketplace
- ✅ Auto-numbering headings (1., 1.1, 1.1.1)
- ✅ Update tự động khi thêm/xóa sections
- ✅ Customizable format

**Setup:**
```json
// settings.json
{
  "markdown.extension.toc.levels": "1..6",
  "markdown.extension.toc.orderedList": true,
  "markdown.extension.toc.updateOnSave": true
}
```

**b) Auto Markdown TOC Extension**
- ✅ Tạo TOC với numbering
- ✅ Sync real-time

**Hướng 2 - Pandoc preprocessing:**
```bash
# Script tự động add numbering
pandoc --number-sections input.md -o output.pdf
```

**KHUYẾN NGHỊ**: Dùng **Markdown All in One** - giải quyết được 90% nhu cầu

---

### 2. Phân trang & Layout ⚠️ HYBRID SOLUTION

**Vấn đề**: Markdown thuần không handle được advanced layout

**Hướng 1 - Cursor + Preview Extensions:**

**a) Markdown PDF Extension**
- ✅ Export PDF với basic page breaks
- ❌ Limited control over layout

**b) Custom CSS cho preview:**
```css
/* Custom styles */
@page {
  margin: 2.5cm;
  @bottom-center {
    content: counter(page);
  }
}

.page-break {
  page-break-before: always;
}

.landscape {
  page-break-before: always;
  page-break-after: always;
  /* Landscape orientation */
}
```

**Hướng 2 - Markdown → LaTeX Pipeline:**

**a) Pandoc với LaTeX templates:**
```bash
# Advanced PDF generation
pandoc input.md \
  --template=academic-template.latex \
  --pdf-engine=xelatex \
  -o output.pdf
```

**b) Custom LaTeX template cho academic papers:**
- ✅ Full control over page layout
- ✅ Professional formatting
- ✅ Complex tables/figures handling

**KHUYẾN NGHỊ**: **Hybrid approach**
- Viết content trong Markdown (Cursor IDE)
- Advanced formatting với Pandoc + LaTeX templates

---

### 3. Tạo mục lục ✅ PLUGIN + TOOL SOLUTION

**Hướng 1 - Built-in Extensions:**

**Markdown All in One:**
```markdown
<!-- TOC được generate tự động -->
- [1. Introduction](#1-introduction)
- [2. Methodology](#2-methodology)
  - [2.1 Data Collection](#21-data-collection)
```

**Hướng 2 - Pandoc generation:**
```bash
# Auto-generate TOC with page numbers
pandoc --toc --toc-depth=3 input.md -o output.pdf
```

**KHUYẾN NGHỊ**: **Plugin cho editing, Pandoc cho final output**

---

### 4. Tính năng nâng cao ⚠️ TOOL-DEPENDENT

#### a) Cross-references

**Hướng 1 - Markdown extensions:**
```markdown
See [Section 2.1](#methodology-data-collection)
As shown in [Figure 1](#fig-flowchart)
```
- ✅ Basic linking
- ❌ No automatic numbering

**Hướng 2 - Pandoc filters:**
```markdown
See [@sec:methodology]
As shown in [@fig:flowchart]
```
- ✅ Automatic numbering
- ✅ Professional citation style

#### b) Bibliography Management

**Hướng 1 - Citation extensions:**
- **Zotero Connector**: Export to BibTeX
- **Citation Picker**: Insert citations

**Hướng 2 - Pandoc + BibTeX:**
```bash
pandoc input.md --bibliography=refs.bib --csl=apa.csl -o output.pdf
```

#### c) Index Generation

**Hướng 1**: ❌ Không có plugin hiệu quả
**Hướng 2**: ✅ LaTeX với makeidx package

**KHUYẾN NGHỊ**: **LaTeX pipeline cho advanced features**

---

## Recommended Workflow Architecture

### **SIMPLIFIED APPROACH: Pandoc DOCX** ⭐

### Phase 1: Content Creation (Cursor IDE)
```
Markdown files + Extensions:
├── Markdown All in One (numbering, TOC)
├── Citation Picker (bibliography)
└── Live Preview (real-time view)
```

### Phase 2: Professional Output (Pandoc Pipeline)
```
Markdown → Pandoc → DOCX
├── Reference template (academic-template.docx)
├── Bibliography processing (BibTeX)
├── Citation styles (CSL)
└── Professional formatting
```

### **ADVANCED APPROACH: LaTeX (khi cần)**

### Phase 2b: Advanced Output (LaTeX Pipeline)
```
Markdown → Pandoc → LaTeX → PDF
├── Custom LaTeX templates
├── Advanced formatting rules
├── Complex mathematical typesetting
└── Professional publishing
```

---

## Implementation Plan

### Bước 1: Setup Cursor IDE Environment
1. **Install essential extensions:**
   - Markdown All in One
   - Markdown PDF  
   - Citation Picker
   - LaTeX Workshop (cho LaTeX preview)

2. **Configure workspace:**
```json
{
  "markdown.extension.toc.levels": "1..4",
  "markdown.extension.preview.autoShowPreviewToSide": true,
  "latex-workshop.latex.autoBuild.run": "onSave"
}
```

### Bước 2: Create Template Structure
```
project/
├── content/
│   ├── chapters/
│   │   ├── 01-introduction.md
│   │   ├── 02-literature.md
│   │   └── 03-methodology.md
│   └── main.md (combines all)
├── templates/
│   ├── academic-paper.latex
│   └── book.latex  
├── bibliography/
│   └── references.bib
└── scripts/
    ├── build-pdf.sh
    └── build-docx.sh
```

### Bước 3: Automated Build Scripts
```bash
#!/bin/bash
# build-pdf.sh

# Combine chapters
cat content/chapters/*.md > temp/full-document.md

# Generate PDF with advanced formatting
pandoc temp/full-document.md \
  --template=templates/academic-paper.latex \
  --bibliography=bibliography/references.bib \
  --csl=styles/apa.csl \
  --number-sections \
  --toc \
  --pdf-engine=xelatex \
  -o output/document.pdf

# Cleanup
rm temp/full-document.md
```

---

## Kết luận & Khuyến nghị

### **UPDATED: Simplified Best Practice Workflow** ⭐

1. **Content Writing**: 100% trong Cursor IDE với Markdown
   - Focus vào nội dung, không lo formatting
   - Real-time collaboration với AI
   - Version control với Git

2. **Professional Output**: **Pandoc DOCX** (Primary)
   - 90% quality của LaTeX
   - 50% complexity reduction
   - Easy collaboration
   - Journal-friendly

3. **Advanced Output**: LaTeX PDF (When needed)
   - Complex mathematical papers
   - Advanced typography requirements
   - Journal-specific formatting

### **Simplified Extensions cần thiết**:
- ✅ **Markdown All in One** (essential)
- ✅ **Citation Picker** (bibliography)
- ✅ **Pandoc** (external tool)
- ⚠️ **LaTeX Workshop** (optional, chỉ khi cần LaTeX)

### **Updated Tỷ lệ giải quyết**:
- **Numbering**: 100% solved với plugins
- **TOC**: 100% solved với plugins + Pandoc  
- **Page layout**: 90% solved với Pandoc DOCX
- **Advanced features**: 85% solved với Pandoc DOCX

### **Final Recommendation**:

**🎯 Start với Pandoc DOCX approach:**
- Đơn giản hơn LaTeX
- Chất lượng đủ tốt cho 80-90% academic papers
- Collaboration-friendly
- Easy setup

**🔄 Upgrade to LaTeX khi cần:**
- Complex mathematical content
- Advanced formatting requirements
- Journal-specific needs

**Kết luận**: **Pandoc DOCX là sweet spot** - professional quality với minimal complexity!
