# Sử dụng Cursor IDE để ứng dụng AI viết tài liệu khoa học

## Giới thiệu

Dự án này hướng dẫn cách sử dụng Cursor IDE để viết **nghiên cứu khoa học và sách** một cách hiệu quả, tận dụng khả năng AI tích hợp để chỉnh sửa trực tiếp các file văn bản.

## Nội dung

- [`docs/guide-cursor-scientific-writing.md`](docs/guide-cursor-scientific-writing.md) - Hướng dẫn chi tiết về workflow và best practices
- [`docs/template-research-paper.md`](docs/template-research-paper.md) - Template Markdown cho nghiên cứu khoa học
- [`docs/template-research-paper.tex`](docs/template-research-paper.tex) - Template LaTeX cho nghiên cứu khoa học
- [`docs/template-book.md`](docs/template-book.md) - Template Markdown cho viết sách
- [`docs/issue.md`](docs/issue.md) - Các thách thức kỹ thuật và status giải pháp
- [`docs/solutions-analysis.md`](docs/solutions-analysis.md) - Phân tích chi tiết các giải pháp
- [`docs/pandoc-analysis.md`](docs/pandoc-analysis.md) - Phân tích khả năng Pandoc DOCX vs LaTeX
- [`docs/setup-instructions.md`](docs/setup-instructions.md) - Hướng dẫn setup môi trường hoàn chỉnh
- [`docs/custom-numbering-guide.md`](docs/custom-numbering-guide.md) - Tùy chỉnh format numbering (Chapter 1, 2...)
- [`docs/markdown-as-content-layer.md`](docs/markdown-as-content-layer.md) - Markdown làm content layer, tools làm formatting
- [`docs/wysiwyg-markdown-editors.md`](docs/wysiwyg-markdown-editors.md) - WYSIWYG editors cho table editing (Typora, Mark Text)
- [`docs/zettlr-analysis.md`](docs/zettlr-analysis.md) - Phân tích Zettlr cho academic writing
- [`docs/latex-wysiwyg-editors.md`](docs/latex-wysiwyg-editors.md) - LaTeX WYSIWYG editors (LyX, Overleaf)

## Khuyến nghị chính

**Cursor IDE > ChatGPT/Gemini Web** cho academic writing vì:
- ✅ Chỉnh sửa trực tiếp file (không cần copy-paste)
- ✅ Hiểu ngữ cảnh toàn bộ tài liệu  
- ✅ Workflow tích hợp hoàn chỉnh
- ✅ Hỗ trợ version control

**Markdown + Pandoc** approach vì:
- ✅ **Content-focused** - Markdown chỉ để tạo nội dung
- ✅ **AI-optimized** - Markdown đơn giản, AI hiểu tốt
- ✅ **Formatting flexibility** - Pandoc convert sang mọi format
- ✅ **Professional output** - DOCX/PDF chất lượng cao
- ✅ **Workflow separation** - content vs formatting tách biệt

## Tại sao Cursor IDE tốt hơn ChatGPT/Gemini Web?

### Kịch bản thực tế cho nhà khoa học:

**Với ChatGPT/Gemini Web** (cách cũ):
```
1. Viết 5 trang trong Word/Google Docs
2. Muốn AI cải thiện phần Introduction
3. Copy toàn bộ 5 trang → paste vào ChatGPT
4. ChatGPT trả về Introduction mới
5. Copy kết quả → paste về Word
6. Muốn sửa Method thì lại phải copy-paste toàn bộ
7. Lặp lại quy trình này hàng chục lần...
```

**Với Cursor IDE** (cách mới):
```
1. Viết trong file .md
2. Highlight đoạn Introduction → "Cải thiện phần này"
3. AI đọc cả file, hiểu context, sửa trực tiếp
4. Muốn sửa Method → highlight → "Viết lại rõ ràng hơn"
5. AI tự động sửa, không cần copy-paste gì cả
```

### Ưu điểm cụ thể:

**🚀 Tiết kiệm thời gian khổng lồ**
- Không cần copy-paste liên tục
- AI hiểu ngữ cảnh ngay, không cần giải thích lại

**🧠 AI thông minh hơn** 
- Đọc được toàn bộ paper của bạn
- Hiểu methodology để viết discussion phù hợp
- Giữ được tone và style nhất quán

**📝 Làm việc tự nhiên**
- Viết và chỉnh sửa ngay trong một môi trường
- Track changes với Git như code
- Collaborate dễ dàng với đồng nghiệp

**💡 Ví dụ thực tế**:
- "Viết abstract 200 từ cho paper này" → AI đọc cả paper và tóm tắt chính xác
- "Literature review này còn thiếu gì?" → AI phân tích và gợi ý gaps
- "Cải thiện academic tone cho section này" → AI sửa style mà giữ nguyên ý nghĩa

## Bắt đầu nhanh

1. Copy template phù hợp từ folder `docs/`
2. Đọc hướng dẫn trong `guide-cursor-scientific-writing.md`  
3. **Focus vào nội dung** - viết Markdown đơn giản với AI assistance
4. **Table editing** - dùng WYSIWYG editor (Zettlr/Typora/Mark Text) cho tables
5. **Formatting instructions** - thêm comments cho page breaks, layout
6. **Convert với Pandoc** - sang DOCX/PDF professional

*→ Chi tiết workflow: xem [`docs/markdown-as-content-layer.md`](docs/markdown-as-content-layer.md)*

