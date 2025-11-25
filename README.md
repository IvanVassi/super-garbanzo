# PDF Chapter Splitter

A universal tool that splits PDF books into clean, chapter-level files.  
It can extract the structure of a book from **built-in PDF bookmarks (outline)** or, when bookmarks are missing, from an **automatically detected table of contents** using an LLM (supports English and Russian books).

Designed to produce clean chapter inputs for workflows like **Google NotebookLM**, embedding pipelines, summarization, and notebook-style LMs that require well-structured source documents.

---

## Features

### ✓ Extract chapters from PDF bookmarks
If the PDF contains outline metadata, the tool reads it directly:
- Level-1 entries → treated as top-level chapters  
- Level-2+ entries → ignored as subsection noise

This uses the PDF's real internal page numbers and requires no normalization.

---

### ✓ Extract chapters from TOC using an LLM
If a PDF has no bookmarks, the tool analyzes the first pages and asks an LLM to extract the table of contents:

The model returns structured JSON:
```json
{
  "chapters": [
    {"title": "Introduction", "page_number": 5, "level": "chapter"},
    {"title": "Chapter 1. The Basics", "page_number": 12, "level": "chapter"},
    {"title": "Predictive Methods", "page_number": 14, "level": "section"}
  ]
}
```

Supports Russian and English TOC formats.

---

### ✓ Automatic filtering of top-level chapters
The tool removes subsections and keeps only high-level chapter entries:

- Entries beginning with  
  - `Chapter N`, `Part N`, `Section N`  
  - `Глава N`, `Часть N`, `Раздел N`
- Special titles:  
  - `Introduction`, `Conclusion`, `References`  
  - `Введение`, `Заключение`, `Литература`

Everything else is ignored.

---

### ✓ Page-number offset handling
Logical numbers in the printed TOC often do not match PDF’s real page indices.  
The tool supports a page offset to align logical and actual page numbering.

- Bookmarks → offset always `0`  
- LLM-extracted TOC → offset configurable (manual or auto-assisted)

---

### ✓ Chapter-level PDF output
Each chapter is saved as a separate PDF:

```
book.pdf
book/
    01 - Introduction.pdf
    02 - Chapter 1.pdf
    03 - Chapter 2.pdf
    ...
```

Perfect for:
- Google NotebookLM  
- Summaries and distillations  
- Embedding pipelines  
- Chunked retrieval  
- Research notebooks

---

## How It Works

1. Load the PDF  
2. Attempt to read PDF bookmarks  
3. If bookmarks missing → extract TOC using an LLM  
4. Filter only top-level chapters  
5. Apply page-number offset if needed  
6. Slice the PDF into chapter PDFs

---

## Requirements

```
Python 3.9+
PyMuPDF
openai
```

If you need LLM-based TOC extraction:

```
export OPENAI_API_KEY="your_key_here"
```

---

## Usage

A full Jupyter notebook example is included in the repository.

Typical workflow:
1. Set `PDF_PATH` to your book  
2. Run the notebook  
3. The system extracts TOC (bookmarks → LLM fallback)  
4. Filters chapters  
5. Cuts and saves chapter PDFs

---

## License

MIT License.
