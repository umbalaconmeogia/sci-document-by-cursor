# Hướng dẫn Setup môi trường hoàn chỉnh

## Phần 1: Setup Cursor IDE

### 1.1 Cài đặt Extensions cần thiết

Mở Cursor IDE → Extensions → Tìm và cài đặt:

#### Essential Extensions:
1. **Markdown All in One** (Yu Zhang)
   - Auto numbering headings
   - Table of contents generation
   - Keyboard shortcuts

2. **Markdown PDF** (yzane)
   - Export basic PDF
   - Custom CSS support

3. **Citation Picker** (mblode)
   - Zotero integration
   - Bibliography management

4. **LaTeX Workshop** (James Yu)
   - LaTeX template support
   - PDF preview

#### Optional but helpful:
5. **Pandoc Citer** - Advanced citation
6. **Markdown Preview Enhanced** - Rich preview
7. **Word Count** - Track writing progress

### 1.2 Configure Workspace Settings

Tạo file `.vscode/settings.json` trong project:

```json
{
  // Markdown All in One settings (basic)
  "markdown.extension.toc.levels": "1..4",
  "markdown.extension.toc.orderedList": true,
  "markdown.extension.toc.updateOnSave": true,
  "markdown.extension.list.indentationSize": "adaptive",
  
  // Custom numbering (optional - for advanced users)
  "markdown.extension.toc.numberedList": true,
  
  // Preview settings
  "markdown.extension.preview.autoShowPreviewToSide": true,
  "markdown.preview.scrollPreviewWithEditor": true,
  "markdown.preview.scrollEditorWithPreview": true,
  
  // PDF export settings
  "markdown-pdf.format": "A4",
  "markdown-pdf.margin": {
    "top": "2.5cm",
    "bottom": "2.5cm", 
    "left": "2cm",
    "right": "2cm"
  },
  "markdown-pdf.displayHeaderFooter": true,
  "markdown-pdf.headerTemplate": "<div style='font-size: 9px; margin-left: 1cm;'></div>",
  "markdown-pdf.footerTemplate": "<div style='font-size: 9px; margin: 0 auto;'><span class='pageNumber'></span> / <span class='totalPages'></span></div>",
  
  // LaTeX settings
  "latex-workshop.latex.autoBuild.run": "onSave",
  "latex-workshop.view.pdf.viewer": "tab",
  
  // File associations
  "files.associations": {
    "*.md": "markdown"
  }
}
```

---

## Phần 2: Setup Pandoc Pipeline

### 2.1 Cài đặt Pandoc

#### Windows:
```powershell
# Using Chocolatey
choco install pandoc

# Or download từ: https://pandoc.org/installing.html
```

#### macOS:
```bash
# Using Homebrew
brew install pandoc
```

#### Linux:
```bash
# Ubuntu/Debian
sudo apt-get install pandoc

# Fedora
sudo dnf install pandoc
```

### 2.2 Cài đặt LaTeX Engine

#### TeX Live (Recommended):
```bash
# Download từ: https://www.tug.org/texlive/
# Hoặc sử dụng package manager

# Windows
choco install texlive

# macOS
brew install --cask mactex

# Linux
sudo apt-get install texlive-full
```

### 2.3 Test Installation

```bash
# Kiểm tra Pandoc
pandoc --version

# Kiểm tra LaTeX
pdflatex --version
```

---

## Phần 3: Project Structure Setup

### 3.1 Tạo cấu trúc thư mục

```
your-project/
├── .vscode/
│   └── settings.json
├── content/
│   ├── chapters/
│   │   ├── 01-introduction.md
│   │   ├── 02-literature-review.md
│   │   ├── 03-methodology.md
│   │   ├── 04-results.md
│   │   ├── 05-discussion.md
│   │   └── 06-conclusion.md
│   ├── main.md
│   └── metadata.yaml
├── templates/
│   ├── academic-paper.latex
│   ├── book.latex
│   └── styles.css
├── bibliography/
│   ├── references.bib
│   └── citation-styles/
│       ├── apa.csl
│       └── ieee.csl
├── scripts/
│   ├── build-pdf.bat (Windows)
│   ├── build-pdf.sh (Linux/Mac)
│   └── combine-chapters.py
├── output/
└── temp/
```

### 3.2 Template files

#### Simple Markdown structure (Recommended):
```markdown
# Chapter 1: Giới thiệu

## 1. Bối cảnh nghiên cứu
## 2. Mục tiêu nghiên cứu

# Chapter 2: Tổng quan tài liệu

## 1. Cơ sở lý thuyết
## 2. Các nghiên cứu liên quan
```

*→ Xem [`custom-numbering-guide.md`](custom-numbering-guide.md) để biết chi tiết*

#### metadata.yaml:
```yaml
---
title: "Tiêu đề nghiên cứu"
author: 
  - name: "Tên tác giả"
    affiliation: "Tên trường"
    email: "email@domain.com"
date: "`r format(Sys.time(), '%B %d, %Y')`"
abstract: |
  Tóm tắt nghiên cứu của bạn ở đây.
keywords: ["từ khóa 1", "từ khóa 2", "từ khóa 3"]
bibliography: bibliography/references.bib
csl: bibliography/citation-styles/apa.csl
---
```

#### academic-paper.latex:
```latex
\documentclass[12pt,a4paper]{article}

% Vietnamese support
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[vietnamese]{babel}

% Essential packages
\usepackage{amsmath,amssymb}
\usepackage{graphicx}
\usepackage{longtable,booktabs}
\usepackage{hyperref}
\usepackage{geometry}
\usepackage{fancyhdr}
\usepackage{setspace}

% Page setup
\geometry{margin=2.5cm}
\onehalfspacing
\pagestyle{fancy}
\fancyhf{}
\rhead{\thepage}
\lhead{\leftmark}

% Title page
$if(title)$
\title{$title$}
$endif$
$if(author)$
\author{$for(author)$$author.name$$sep$ \and $endfor$}
$endif$
\date{$date$}

\begin{document}

$if(title)$
\maketitle
$endif$

$if(abstract)$
\begin{abstract}
$abstract$
\end{abstract}
$endif$

$if(toc)$
\tableofcontents
\newpage
$endif$

$body$

\end{document}
```

---

## Phần 4: Build Scripts

### 4.1 Windows Script (build-pdf.bat):
```batch
@echo off
echo Building academic document...

REM Combine chapters
type content\chapters\*.md > temp\full-document.md

REM Generate PDF
pandoc temp\full-document.md ^
  --from markdown ^
  --to pdf ^
  --template=templates\academic-paper.latex ^
  --bibliography=bibliography\references.bib ^
  --csl=bibliography\citation-styles\apa.csl ^
  --number-sections ^
  --toc ^
  --pdf-engine=xelatex ^
  --output=output\document.pdf

REM Cleanup
del temp\full-document.md

echo PDF generated successfully: output\document.pdf
pause
```

### 4.2 Linux/Mac Script (build-pdf.sh):
```bash
#!/bin/bash

echo "Building academic document..."

# Combine chapters
cat content/chapters/*.md > temp/full-document.md

# Generate PDF
pandoc temp/full-document.md \
  --from markdown \
  --to pdf \
  --template=templates/academic-paper.latex \
  --bibliography=bibliography/references.bib \
  --csl=bibliography/citation-styles/apa.csl \
  --number-sections \
  --toc \
  --pdf-engine=xelatex \
  --output=output/document.pdf

# Cleanup
rm temp/full-document.md

echo "PDF generated successfully: output/document.pdf"
```

### 4.3 Python Helper Script (combine-chapters.py):
```python
#!/usr/bin/env python3
import os
import glob

def combine_chapters():
    """Combine all chapter files in order"""
    chapter_files = sorted(glob.glob('content/chapters/*.md'))
    
    with open('temp/full-document.md', 'w', encoding='utf-8') as outfile:
        # Add metadata
        with open('content/metadata.yaml', 'r', encoding='utf-8') as meta:
            outfile.write(meta.read())
            outfile.write('\n\n')
        
        # Add each chapter
        for chapter_file in chapter_files:
            with open(chapter_file, 'r', encoding='utf-8') as infile:
                outfile.write(f'\n\n<!-- {os.path.basename(chapter_file)} -->\n\n')
                outfile.write(infile.read())
                outfile.write('\n\n\\newpage\n\n')
    
    print(f"Combined {len(chapter_files)} chapters")

if __name__ == "__main__":
    combine_chapters()
```

---

## Phần 5: Workflow Instructions

### 5.1 Daily Writing Workflow

1. **Mở Cursor IDE**, navigate to project folder
2. **Edit chapters** trong `content/chapters/` 
3. **Real-time preview** với Markdown All in One
4. **AI assistance** cho content generation/improvement
5. **Save regularly** - auto TOC update

### 5.2 Export Workflow

1. **Quick preview**: Sử dụng "Markdown PDF" extension
2. **Professional output**: Run build script
   ```bash
   # Linux/Mac
   ./scripts/build-pdf.sh
   
   # Windows
   scripts\build-pdf.bat
   ```

### 5.3 Bibliography Management

1. **Manage references** trong Zotero
2. **Export to BibTeX**: `bibliography/references.bib`
3. **Insert citations** trong Markdown:
   ```markdown
   Theo nghiên cứu của Smith [-@smith2023], việc sử dụng AI...
   ```

### 5.4 Version Control

```bash
# Initialize git
git init
git add .
git commit -m "Initial project setup"

# Daily commits
git add content/
git commit -m "Update chapter 3: methodology"
```

---

## Phần 6: Troubleshooting

### Common Issues:

#### 1. Pandoc not found
```bash
# Add to PATH or use full path
export PATH="/usr/local/bin:$PATH"
```

#### 2. LaTeX compilation errors
```bash
# Check LaTeX installation
pdflatex --version

# Install missing packages
tlmgr install [package-name]
```

#### 3. Vietnamese characters issues
- Ensure UTF-8 encoding
- Use XeLaTeX engine
- Install proper fonts

#### 4. Citations not working
- Check .bib file format
- Verify CSL file path
- Ensure proper citation syntax

---

## Phần 7: Next Steps

1. **Test the setup** với sample content
2. **Customize templates** theo requirements
3. **Setup CI/CD** cho automatic builds (GitHub Actions)
4. **Explore advanced features**: cross-references, index generation
5. **Create style guides** cho consistent formatting

Với setup này, bạn sẽ có một environment hoàn chỉnh cho academic writing với Cursor IDE!
