# Hướng dẫn tùy chỉnh Auto Numbering trong Markdown

## Yêu cầu của bạn

Bạn muốn format numbering như:
- **Chapter 1, Chapter 2, Chapter 3...** (cho chapters)
- **1, 2, 3...** (cho sections)
- Thay vì: 1, 2, 3... 1.1, 1.2, 1.3...

## Giải pháp

### **Phương pháp 1: Manual Format trong Markdown** ⭐ (Đơn giản nhất)

**Cách làm:**
```markdown
# Chapter 1: Giới thiệu

## 1. Bối cảnh nghiên cứu
## 2. Mục tiêu nghiên cứu

# Chapter 2: Tổng quan tài liệu

## 1. Cơ sở lý thuyết
## 2. Các nghiên cứu liên quan
```

**Ưu điểm:**
- ✅ Đơn giản, không cần config
- ✅ AI dễ hiểu và chỉnh sửa
- ✅ Hoạt động với mọi Markdown editor

**Nhược điểm:**
- ❌ Phải tự đánh số sections
- ❌ Không auto-update khi thêm/xóa

### **Phương pháp 2: Markdown All in One + Custom CSS** ⭐⭐

**Setup trong Cursor IDE:**

1. **Cài Markdown All in One extension**

2. **Configure settings.json:**
```json
{
  "markdown.extension.toc.levels": "1..4",
  "markdown.extension.toc.orderedList": true,
  "markdown.extension.toc.updateOnSave": true,
  
  // Custom numbering format
  "markdown.extension.toc.numberedList": true,
  "markdown.extension.toc.plaintext": false
}
```

3. **Tạo custom CSS cho preview:**
```css
/* File: styles/custom-numbering.css */
h1::before {
  content: "Chapter " counter(h1) ": ";
  counter-increment: h1;
}

h2::before {
  content: counter(h2) ". ";
  counter-increment: h2;
}

h3::before {
  content: counter(h2) "." counter(h3) ". ";
  counter-increment: h3;
}

/* Reset counters for each chapter */
h1 {
  counter-reset: h2 h3;
}
```

4. **Configure preview với custom CSS:**
```json
{
  "markdown.styles": ["styles/custom-numbering.css"]
}
```

### **Phương pháp 3: Pandoc với Custom Template** ⭐⭐⭐ (Professional)

**Tạo Word template với custom numbering:**

1. **Tạo template.docx:**
   - Mở Word
   - Format Heading 1: "Chapter 1", "Chapter 2"...
   - Format Heading 2: "1.", "2."...
   - Save as "academic-template.docx"

2. **Sử dụng template:**
```bash
pandoc input.md \
  --reference-doc=academic-template.docx \
  --number-sections \
  -o output.docx
```

**Kết quả:** Professional DOCX với format bạn muốn

### **Phương pháp 4: LaTeX Template** ⭐⭐⭐⭐ (Advanced)

**Tạo custom LaTeX template:**
```latex
\documentclass[12pt,a4paper]{book}

% Custom chapter formatting
\usepackage{titlesec}
\titleformat{\chapter}[display]
  {\normalfont\huge\bfseries}
  {Chapter \thechapter}{20pt}{\Huge}

% Custom section formatting  
\titleformat{\section}
  {\normalfont\Large\bfseries}
  {\thesection}{1em}{}

\begin{document}
$body$
\end{document}
```

**Convert với template:**
```bash
pandoc input.md \
  --template=custom-template.tex \
  --number-sections \
  -o output.pdf
```

---

## Khuyến nghị cho bạn

### **🎯 Phương pháp tối ưu: Manual Format + Pandoc**

**Lý do:**
- ✅ **Đơn giản như Google Docs** - không cần config phức tạp
- ✅ **AI-friendly** - AI hiểu và có thể chỉnh sửa
- ✅ **Professional output** - Pandoc tạo DOCX chất lượng cao
- ✅ **Flexible** - dễ thay đổi format khi cần

**Workflow:**
```
1. Viết trong Markdown với manual numbering
2. AI assist trong Cursor IDE
3. Pandoc convert sang DOCX với template
4. Final editing trong Word (nếu cần)
```

### **Setup cụ thể:**

**1. Markdown structure:**
```markdown
# Chapter 1: Giới thiệu

## 1. Bối cảnh nghiên cứu
## 2. Mục tiêu nghiên cứu  
## 3. Câu hỏi nghiên cứu

# Chapter 2: Tổng quan tài liệu

## 1. Cơ sở lý thuyết
## 2. Các nghiên cứu liên quan
## 3. Khung lý thuyết
```

**2. Pandoc command:**
```bash
pandoc input.md \
  --reference-doc=academic-template.docx \
  --bibliography=references.bib \
  --csl=apa.csl \
  -o output.docx
```

**3. Word template setup:**
- Heading 1: "Chapter 1", "Chapter 2"...
- Heading 2: "1.", "2."...
- Professional academic formatting

---

## Kết luận

**Cho nhu cầu "tương đương Google Docs":**

✅ **Manual numbering trong Markdown** + **Pandoc DOCX** là giải pháp optimal:
- Đơn giản như Google Docs
- Professional output
- AI-friendly workflow
- Easy collaboration

**Không cần:**
- ❌ Complex CSS configuration
- ❌ LaTeX knowledge
- ❌ Advanced extensions

**→ Bạn có thể bắt đầu ngay với phương pháp này!**
