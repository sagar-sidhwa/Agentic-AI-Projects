# 🚀 Hospital AI – An AI Agent for Healthcare Norms & Regulations  

## 📖 Project Overview  
**Hospital AI** is a **domain-specific AI agent** designed to assist healthcare professionals, patients, and administrators by providing **accurate, up-to-date information** on:  
- Hospital **norms & safety protocols**  
- **Healthcare regulations** and compliance standards  
- **Insurance policies** and legal guidelines  

The project implements a **Retrieval-Augmented Generation (RAG) pipeline** to combine:  
- **Semantic Search (FAISS + embeddings)**  
- **Lightweight LLMs (Flan-T5-small / other LoRA fine-tuned models)**  
- **Custom healthcare datasets (converted to JSONL)**
- **Grounded Responses:** Answers are derived from a curated dataset of official manuals and legal guidelines.

## 🎯 Objectives
* Build a **domain-specific** healthcare Q&A assistant.
* Provide accurate answers regarding **HIPAA, WHO, and CDC** standards.
* Enable fast **semantic search** across massive policy documents.
* Demonstrate **PEFT (Parameter-Efficient Fine-Tuning)** on JSONL instruction data.


## 📁 Dataset  
- The dataset for this project is located in the `Hospital AI Dataset` folder of this repository.

### Data Sources  
The dataset for Hospital AI are curated from:  
- **Hospital policy manuals & safety guidelines** (PDFs)  
- **National & international healthcare regulations** (WHO, CDC, HIPAA, etc.)  
- **Insurance rules & claim procedures**  
- **Patient safety standards & accreditation documents**  

### Data Preparation  
1. **Collected PDFs** from public healthcare portals and hospital websites.  
2. **Converted into JSONL** format with structure:  
   ```json
   {"instruction": "...", "input": "...", "output": "..."}
3. Embedded using all-MiniLM-L6-v2 on the output field.
4. Stored in FAISS vector DB for fast retrieval.


---

## 🛠️ Tech Stack
| Category | Technology |
| :--- | :--- |
| **Language** | Python |
| **Frameworks** | PyTorch, Hugging Face Transformers |
| **Embeddings** | Sentence-Transformers (`all-MiniLM-L6-v2`) |
| **Vector Database** | FAISS (Facebook AI Similarity Search) |
| **Model** | `google/flan-t5-small` (LoRA/QLoRA fine-tuned) |
| **Data Format** | JSONL |

---

## 📁 Dataset & Preparation
The dataset is located in the `Hospital AI Dataset` folder and consists of curated data from:
* Hospital policy manuals and safety guidelines.
* National/International regulations (WHO, CDC, HIPAA).
* Insurance claim procedures and medical malpractice laws.

### Data Pipeline:
1.  **PDF Extraction:** Collected official healthcare documents.
2.  **Structuring:** Converted text into a structured JSONL format:
    ```json
    {"instruction": "...", "input": "...", "output": "..."}
    ```
3.  **Vectorization:** Generated embeddings on the `output` field using `all-MiniLM-L6-v2`.
4.  **Indexing:** Stored in **FAISS** for rapid top-$k$ similarity retrieval.

---

## 🔑 Implementation Steps
1.  **Dataset Preparation:** PDF extraction and JSONL formatting.
2.  **Embedding Generation:** Creating vector representations of healthcare norms.
3.  **Vector DB Setup:** Initializing FAISS for efficient similarity search.
4.  **Fine-tuning:** Training the LLM using LoRA adapters on domain-specific instructions.
5.  **RAG Integration:** Linking the retrieval system with the fine-tuned LLM to generate grounded answers.
6.  **Evaluation:** Stress-testing the agent with complex insurance and safety queries.

---

## 📊 Example Queries
* *"What are the safety protocols for handling infectious patients?"*
* *"How does HIPAA protect patient information in hospitals?"*
* *"What steps must be followed before claiming health insurance?"*
* *"List the documents required for filing a medical insurance claim."*

---

## 👨‍💻 Author
**Sagar Sidhwa**
* **AI / ML Engineer**
* **Education:** MS in CS (AI Track) — Binghamton University
* *Focusing on ML and end-to-end real-world projects. Open to collaboration!*
