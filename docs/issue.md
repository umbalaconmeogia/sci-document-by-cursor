# Các vấn đề cần tìm cách giải quyết với Markdown

Các vấn đề kỹ thuật cần giải quyết khi viết tài liệu dài (sách):

### 1. Numbering heading ✅ SOLVED
- ✅ Tự động đánh số chương, mục (1.1, 1.2, Chương 1, etc.)
- ✅ Cập nhật số tự động khi thêm/xóa sections
- **Giải pháp**: Extension "Markdown All in One"

### 2. Phân trang và formatting ⚠️ HYBRID SOLUTION  
- ✅ Tạo page breaks khi xuất PDF
- ✅ Quay ngang một số trang (tables, figures)
- ✅ Control layout cho in ấn
- **Giải pháp**: Pandoc + LaTeX pipeline

### 3. Tạo mục lục ✅ SOLVED
- ✅ Auto-generate table of contents
- ✅ Liên kết clickable trong PDF
- ✅ Cập nhật số trang tự động
- **Giải pháp**: Extension + Pandoc `--toc`

### 4. Tính năng nâng cao ⚠️ TOOL DEPENDENT
- ✅ Cross-references trong tài liệu (Pandoc filters)
- ✅ Bibliography management (Zotero + BibTeX)
- ⚠️ Index generation (cần LaTeX)

---

## Status tổng quan

**Giải quyết được**: 85-90% vấn đề
- ✅ **Plugin solutions**: Numbering, TOC  
- ✅ **Hybrid solutions**: Page layout, citations
- ⚠️ **Tool-dependent**: Advanced features

**Chi tiết giải pháp**: Xem [`solutions-analysis.md`](solutions-analysis.md)
**Hướng dẫn setup**: Xem [`setup-instructions.md`](setup-instructions.md)