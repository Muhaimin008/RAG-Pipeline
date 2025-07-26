# RAG-Pipeline
This repository contains a Retrieval-Augmented Generation (RAG) pipeline designed to automatically extract and answer multiple-choice questions (MCQs) from Enlish & Bangla-language educational PDFs. It’s built to run seamlessly on **Google Colab**, ensuring ease of use in resource-constrained settings and supporting future extensibility.

The system integrates OCR, semantic chunking, vector similarity search, and OpenAI’s GPT-based generation to provide context-aware answers. Its modular design encourages adaptation for a broader range of educational and low-resource NLP applications.

---

## 🚀 Features

### ✅ Google Colab Ready

Runs entirely in Google Colab, so there is no setup hassle. Ideal for rapid prototyping and sharing with educators or researchers.

### 📄 OCR with `pytesseract`

Most Bangla textbooks are scanned images. We use Tesseract OCR to convert images to text, enabling downstream NLP tasks.

### 🖼 PDF Conversion with `pdf2image`

Converts each PDF page to an image for OCR. Crucial for handling non-digital (scanned) documents.

### ✂️ Smart Chunking

The text is chunked into overlapping semantic units using sentence-based splitting. This preserves context during retrieval.

### 🔍 Embedding + FAISS Retrieval

Text chunks are embedded using OpenAI's `text-embedding-3-small` model, and similarity search is powered by FAISS. This combination balances performance, accuracy, and cost.

### 🧠 GPT-4 Answer Generation

Once relevant chunks are retrieved, GPT-4 generates grounded answers in Bangla, handling both inline and tabular MCQ formats.

---

## 📦 Installation & Dependencies

If running locally (not on Colab), install the following:

```bash
pip install openai faiss-cpu pytesseract pdf2image python-dotenv numpy
```

You will also need to install Tesseract OCR manually:

* Ubuntu: `sudo apt install tesseract-ocr`
* Windows: [Tesseract Download](https://github.com/tesseract-ocr/tesseract)

For Google Colab, just ensure these libraries are installed via code cells.

---

## 🛠 How to Use (Colab Instructions)

1. Open the notebook in Colab.
2. Upload a scanned Bangla PDF (e.g., textbook).
3. Set your OpenAI API key:

   ```python
   import os
   os.environ["OPENAI_API_KEY"] = "your-key-here"
   ```
4. Run the processing pipeline:

   ```python
   chunks, emb_model, all_indices, mcq_answers, full_text = initialize_rag("yourfile.pdf", os.getenv("OPENAI_API_KEY"))
   comparative_query_loop(chunks, all_indices, emb_model, os.getenv("OPENAI_API_KEY"), mcq_answers, full_text)
   ```
5. Ask questions in Bangla or English. Type `exit` to end.

---

## 🧪 Evaluation

* ✅ Handles inline and tabular MCQ formats
* ✅ Effective retrieval with FAISS + embeddings
* ⚠️ OCR sensitivity: scan quality impacts results
* ⚠️ Ambiguous queries may reduce grounding accuracy

---

## 🔮 Future Extensions

This project was designed with future upgrades in mind:

* Integrate Layout-aware OCR (`layoutparser`) for better MCQ structure detection
* Replace embedding model with multilingual or Bangla-specialized versions
* Build a front-end interface for teachers/students
* Deploy as a REST API or add a Flask/FastAPI wrapper
* Add analytics for performance evaluation (e.g., accuracy, F1-score)

---

## 💡 Design Highlights

* **OCR first**: Enables processing of non-digital textbooks
* **Semantic chunking**: Improves retrieval by preserving logical flow
* **Embeddings**: Efficient context matching across large text volumes
* **LLM grounding**: Ensures answers are drawn from source content

---

## 🔎 Example Query

**Input:**

```
প্রশ্ন: ভাষার সংজ্ঞা কী?
```

**Output:**

```
উত্তর: মানুষের ভাব বিনিময়ের প্রধান মাধ্যম
```

---

## 🧰 Modules Overview

| Module        | Role                                                   |
| ------------- | ------------------------------------------------------ |
| `pytesseract` | Extracts text from scanned PDF images                  |
| `pdf2image`   | Converts PDF pages to image format                     |
| `re` (regex)  | Identifies MCQ and patterns in raw OCR text            |
| `faiss`       | Fast similarity search for embedded text chunks        |
| `openai`      | Embedding + GPT-4 question answering                   |
| `numpy`       | Numerical processing for vectors and similarity scores |

---
