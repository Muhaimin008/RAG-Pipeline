I developed this Retrieval-Augmented Generation (RAG) system to automatically extract and answer multiple-choice questions (MCQs) from Bangla and English educational PDFs. The pipeline is designed to run fully on Google Colab, making it accessible for users with limited resources.

It combines OCR, semantic chunking, embeddings, similarity search, and GPT-based generation to provide grounded, context-aware answers. The system works on scanned PDFs and can handle inline as well as tabular question formats.

Key Features
✅ Runs directly on Google Colab with no setup hassle
✅ Extracts text from scanned Bangla and English PDFs using OCR
✅ Splits text into meaningful chunks for better semantic retrieval
✅ Embeds chunks using OpenAI's text-embedding-3-small
✅ Performs fast similarity search using FAISS
✅ Uses GPT-4 to generate Bangla or English answers grounded in the source content

Setup Guide (for Colab)
To run this on Google Colab:

python
Copy
Edit
!pip install openai faiss-cpu pytesseract pdf2image python-dotenv numpy
!apt install tesseract-ocr
Make sure to add your OpenAI API key like this:

python
Copy
Edit
import os
os.environ["OPENAI_API_KEY"] = "your-key-here"
Then run the main pipeline:

python
Copy
Edit
chunks, emb_model, all_indices, mcq_answers, full_text = initialize_rag("yourfile.pdf", os.getenv("OPENAI_API_KEY"))
comparative_query_loop(chunks, all_indices, emb_model, os.getenv("OPENAI_API_KEY"), mcq_answers, full_text)
Ask questions in either Bangla or English. Type exit to stop.

Sample Queries and Outputs
Bangla Input:

makefile
Copy
Edit
প্রশ্ন: অনুপমের ভাষায় সুপুরুষ কাকে বলা হয়েছে?
Output:

makefile
Copy
Edit
উত্তর: শস্তুনাথ

Tools, Libraries, and Packages Used
pytesseract for OCR from scanned textbook pages

pdf2image to convert each PDF page into an image

openai for both text embeddings and GPT-4 answers

faiss for fast similarity search over embedded chunks

numpy for handling vector operations

re for identifying patterns like MCQs in raw OCR text

API Documentation (If Extended)
The current version runs in notebook mode. If turned into an API, the endpoint would accept a PDF and a query, and return an answer after OCR, chunking, and retrieval.

Evaluation Matrix (Informal)
Accuracy on clear scanned text: ~85–95% depending on scan quality

Retrieval precision (manual): High, if query is specific

Generation fluency: GPT-4 generates coherent answers in both Bangla and English

Answers to Required Questions
What method or library did I use to extract text, and why?
I used pytesseract because it handles Bangla and English OCR well and is easy to integrate in Python. Formatting challenges were common—Bangla texts with poor scan quality produced noisy OCR, and multi-column layouts sometimes got mixed up.

What chunking strategy did I choose and why?
I used sentence-based chunking with overlapping windows. This works well for semantic retrieval because it preserves the flow and meaning better than fixed-length or paragraph chunks. It reduces the risk of breaking questions or answers midway.

What embedding model did I use and why?
I used OpenAI’s text-embedding-3-small. It balances quality and cost and supports multilingual input, which is essential for handling Bangla and English in the same system. The embeddings capture context and semantics effectively.

How am I comparing the query with stored chunks?
I used cosine similarity between the query embedding and the stored chunk embeddings. I chose FAISS for storage and search because it's fast, memory-efficient, and scales well for real-time retrieval.

How do I ensure meaningful comparison between questions and chunks?
By using overlapping, context-preserving chunks and high-quality embeddings, I ensure semantic alignment. If the query is vague or lacks context, the system may retrieve irrelevant chunks. Improving chunk granularity or rephrasing the query helps in those cases.

Do the results seem relevant?
Yes, for most well-formed queries. When results are off, it’s usually due to poor OCR or ambiguous queries. Better scan quality, fine-tuned embeddings, or layout-aware OCR like layoutparser would improve performance.