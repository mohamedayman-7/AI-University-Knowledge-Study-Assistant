# 🚀 [Tips Hindawi](https://www.tipshindawi.com/) Internship (August–October) 2026

> 🎓 This project was built during the [ **Tips Hindawi** ](https://www.tipshindawi.com/) **Internship (August–October) 2026**.


# 🎓 AI University Knowledge & Study Assistant

An AI-powered university study assistant that lets students upload course PDFs and interact with their study material through **grounded question answering, summaries, and multiple-choice question generation**.

The project uses a **Retrieval-Augmented Generation (RAG)** pipeline so answers are based on the uploaded university/course documents rather than general outside knowledge.

---

## 👤 Participant

| Field | Value |
|---|---|
| Full Name | |
| Project Name | AI University Knowledge & Study Assistant |
| GitHub Username | |
| Internship Batch | August–October 2026 |
| Training Program | Large Language Models (LLMs) Program |
| Organization | [**Edrak for Ai**](https://edrak4ai.com/en) |

---

# 📖 Project Overview

The **AI University Knowledge & Study Assistant** is a university-focused AI application built to help students study from their own course materials.

The user uploads one or more PDF lecture/course documents. The application processes the documents, splits them into smaller chunks, converts the chunks into vector embeddings, and stores them in a **FAISS vector database**.

When the student asks a question, the system retrieves the most relevant parts of the uploaded documents and sends them as context to a **Mistral 7B** language model through a custom **LangChain** LLM chain.

The application also provides:

- Course-material question answering
- Topic/chapter summaries
- Difficult MCQ generation
- Correct answers and explanations
- Retrieved source information for generated answers

The goal is to provide a practical study assistant that keeps its generated content grounded in the student's uploaded university materials.

---

# ✨ Features

### 💬 1. Grounded Question Answering

Ask questions about the uploaded course material.

The system:

1. Searches the FAISS vector database.
2. Retrieves the top relevant document chunks.
3. Passes the retrieved context to the LangChain RAG chain.
4. Generates an answer using Mistral.
5. Displays the retrieved document sources.

The prompt instructs the model to use **only the uploaded course context** and to state when the requested information cannot be found in the documents.

### 📝 2. Study Summaries

Enter a topic or chapter and generate a structured study summary containing:

- Main concepts
- Important definitions
- Key points
- Important relationships or steps
- Short revision checklist

The summary is generated from retrieved course material.

### 🧠 3. Difficult MCQ Generation

Generate multiple-choice questions from a selected course topic.

Each question contains:

- Question
- Four options (A–D)
- Correct answer
- Explanation

The project uses LangChain's **StructuredOutputParser** and **ResponseSchema** to request structured MCQ output.

### 📚 4. Multiple PDF Support

The Streamlit interface allows users to upload multiple PDF course/lecture files and build one knowledge base from them.

### 🔎 5. Source Retrieval

For question answering, the application displays the retrieved source documents and page information associated with the relevant chunks.

---

# 🧩 System Architecture

```text
                    ┌─────────────────────┐
                    │   University PDFs   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    PyPDFLoader      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ CharacterTextSplitter│
                    │ chunk=1000           │
                    │ overlap=100          │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ HuggingFace         │
                    │ Embeddings           │
                    │ all-MiniLM-L6-v2    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │       FAISS         │
                    │  Vector Database    │
                    └──────────┬──────────┘
                               │
                ┌──────────────┼──────────────┐
                │              │              │
                ▼              ▼              ▼
          Ask Questions     Summary         MCQs
                │              │              │
                └──────────────┼──────────────┘
                               ▼
                    ┌─────────────────────┐
                    │ LangChain / Prompt  │
                    │ + Custom HF LLM     │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Mistral 7B        │
                    │ Instruct v0.2       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Streamlit Web App   │
                    └─────────────────────┘
```

---

# 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| **Python** | Main programming language |
| **Streamlit** | Web application interface |
| **LangChain** | LLM orchestration, prompts, chains, and structured output |
| **Mistral-7B-Instruct-v0.2** | Main language generation model |
| **Hugging Face Transformers** | Loading and running the Mistral model |
| **Sentence Transformers** | Text embeddings |
| **all-MiniLM-L6-v2** | Embedding model |
| **FAISS** | Vector database and similarity search |
| **PyPDFLoader** | Loading PDF course documents |
| **CharacterTextSplitter** | Splitting documents into chunks |
| **StructuredOutputParser** | Structured MCQ generation |
| **PyTorch** | Model execution |
| **pyngrok** | Optional public access to the Streamlit application |

---

# ⚙️ Installation

## 1. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_PROJECT_FOLDER>
```

## 2. Create a virtual environment

```bash
python -m venv .venv
```

### Windows

```bash
.venv\Scripts\activate
```

### Linux / macOS

```bash
source .venv/bin/activate
```

## 3. Install dependencies

The project uses the following packages:

```bash
pip install -U streamlit faiss-cpu langchain langchain-community langchain-core langchain-huggingface langchain-classic langchain-text-splitters pypdf sentence-transformers transformers==4.52.4 accelerate torch
```

The notebook also contains a PyTorch installation step:

```bash
pip install --force-reinstall --no-deps torch==2.11.0 torchvision==0.26.0
```

> **Note:** The Mistral 7B model is relatively large. A suitable GPU is recommended for practical performance. CPU execution is supported by the model-loading logic, but it can be significantly slower.

---

# 🤗 Model Configuration

By default, the application uses:

```text
mistralai/Mistral-7B-Instruct-v0.2
```

The model can be changed using the `MODEL_NAME` environment variable.

### Windows PowerShell

```powershell
$env:MODEL_NAME="your-model-name"
```

### Linux / macOS

```bash
export MODEL_NAME="your-model-name"
```

If no environment variable is provided, the application uses the default Mistral model.

---

# 🚀 Usage

## 1. Start the application

Run:

```bash
streamlit run app.py
```

The Streamlit application will normally be available at:

```text
http://localhost:8501
```

## 2. Upload course documents

From the sidebar:

1. Upload one or more PDF lecture/course files.
2. Click **Build Knowledge Base**.
3. Wait for the documents to be loaded, split, embedded, and indexed.

## 3. Ask Questions

Open the **Ask Questions** tab.

Enter a question such as:

```text
Explain normalization based only on the uploaded lectures.
```

The application retrieves relevant course chunks and generates a grounded answer.

## 4. Generate a Summary

Open the **Summarize** tab.

Enter a topic or chapter, for example:

```text
Database normalization
```

The application generates a structured revision summary using the uploaded course material.

## 5. Generate MCQs

Open the **Generate MCQs** tab.

Enter a topic and choose the number of questions.

The application generates difficult MCQs with:

- Four answer options
- Correct answer
- Explanation

---

# 🔗 Optional: Public Streamlit Access with ngrok

The notebook includes an optional **pyngrok** workflow for exposing the Streamlit application through a public URL.

Install pyngrok:

```bash
pip install pyngrok
```

Then configure your ngrok authentication token securely using an environment variable rather than placing the token directly in source code.

Example:

```bash
export NGROK_TOKEN="YOUR_NGROK_TOKEN"
```

Then start Streamlit and create an ngrok tunnel to port `8501`.

> **Security:** Never commit your real ngrok authentication token to GitHub.

---

# 📂 Project Structure

A recommended repository structure is:

```text
AI-University-Knowledge-Study-Assistant/
│
├── app.py
├── final-project.ipynb
├── README.md
└── requirements.txt
```

### `app.py`

The main Streamlit application containing:

- Mistral model loading
- Custom LangChain LLM
- PDF processing
- FAISS vector database
- RAG question answering
- Summary generation
- MCQ generation
- Streamlit interface

### `final-project.ipynb`

The development notebook containing the project implementation and the code used to create the Streamlit application.

---

# 🧠 RAG Workflow

The question-answering system follows this process:

### Step 1 — Document Loading

PDF files are loaded using:

```python
PyPDFLoader
```

### Step 2 — Text Splitting

Documents are divided into chunks using:

```python
CharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=100
)
```

### Step 3 — Embeddings

Each chunk is converted into a vector using:

```text
sentence-transformers/all-MiniLM-L6-v2
```

### Step 4 — Vector Storage

The embeddings are stored in:

```text
FAISS
```

### Step 5 — Similarity Search

For a user question, the application retrieves the top `k=3` relevant chunks.

### Step 6 — LangChain RAG Chain

The retrieved context is passed into a LangChain prompt and custom Hugging Face LLM.

### Step 7 — Mistral Generation

Mistral generates the final answer using the retrieved course context.

---

# 🧾 Structured MCQ Generation

MCQ generation uses LangChain's:

```python
ResponseSchema
StructuredOutputParser
```

The requested output contains:

```text
questions
 ├── question
 ├── options A-D
 ├── correct_answer
 └── explanation
```

The application attempts to parse the model output into the expected structured format. If parsing fails, the raw model output is displayed instead of hiding the error.

---

# 📸 Demo

Add screenshots or a demo video here.

Suggested screenshots:

1. PDF upload / knowledge-base creation
2. Question-answering interface
3. Retrieved source documents
4. Summary generation
5. MCQ generation

Example:

```markdown
![Application Screenshot](images/app-screenshot.png)
```

---

# 📈 Results

The project demonstrates an end-to-end university study assistant that combines:

- Large Language Models
- Retrieval-Augmented Generation
- Semantic similarity search
- Document embeddings
- LangChain chains
- Structured LLM output
- Interactive Streamlit UI

The system can transform uploaded university PDFs into an interactive knowledge base and use that material for studying, revision, and self-testing.

---

# 🔮 Future Improvements

Possible future improvements include:

- Support for additional document formats such as DOCX and PPTX
- Persistent vector databases instead of rebuilding the index
- Conversation memory for multi-turn questions
- More advanced retrieval strategies
- Hybrid keyword + semantic search
- Better citation and page-level references
- User authentication and separate course workspaces
- Difficulty levels for generated MCQs
- Automatic quizzes and scoring
- Flashcard generation
- Study-plan generation
- Course-specific knowledge bases
- Deployment to a cloud platform
- More efficient quantized LLM inference

---

# 📚 About the Internship

This project was developed as part of the [**Tips Hindawi**](https://www.tipshindawi.com/) **Internship (August–October 2026)**.

The internship is associated with [**Edrak for Ai**](https://edrak4ai.com/en) and focuses on practical training and building real-world projects using Large Language Models and related AI technologies.

For more information about the internship and training programs, visit the official [Tips Hindawi](https://www.tipshindawi.com/) website.

---

# 📄 License

This project is shared for educational and portfolio purposes.
