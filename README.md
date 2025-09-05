# 📝 Multi-Agent Technical Article Generator  

A **multi-agent system (MAS)** that autonomously generates **in-depth technical articles** (1000–1500 words) with optional code examples, and exports them in **PDF** and **DOCX** formats.  

This project combines **LangGraph agents**, **local open-source LLMs**, and **flexible export pipelines** to deliver professional-quality articles, complete with structured formatting and syntax-highlighted code blocks.  

---

## 🚀 Features  

- **Topic Analysis** → Refines raw user input into structured outlines with subtopics and applications  
- **Content Generation** → Produces full-length, well-structured technical articles (intro, body, conclusion)  
- **Code Snippets** → Optionally generates 2–4 illustrative code examples (Python, JS, etc.)  
- **Consistent Formatting** → Clean headings, lists, and highlighted code blocks in Markdown → DOCX/PDF  
- **Export Options** → Download articles in:  
  - **DOCX** (using `python-docx`)  
  - **PDF** (via `ReportLab` or `PDFKit` for styled exports)  
- **Web Interface** → Easy-to-use **Streamlit UI** for interaction  

---

## 🏗️ Architecture  

The system uses a **sequential agent-based pipeline** coordinated by an **OrchestratorAgent**. Each agent performs a specialized step and passes refined output forward.  

### Workflow  

1. **Orchestrator (`OrchestratorAgent`)**  
   - Central coordinator  
   - Manages workflow, passing data between agents  

2. **Topic Analyzer (`TopicAnalyzerAgent`)**  
   - Input: User-provided topic  
   - Output: Structured outline with subtopics, scope, and related fields  

3. **Content Generator (`ContentGeneratorAgent`)**  
   - Expands the outline into a **1000–1500 word article**  
   - Structured into intro, body, and conclusion  

4. **Code Snippet Agent (`CodeSnippetAgent`)** *(optional)*  
   - Adds 2–4 contextual code examples  
   - Uses code-specialized LLM (e.g., **CodeLlama**)  

5. **Formatter (`FormatterAgent`)**  
   - Merges code + text  
   - Cleans up Markdown  
   - Extracts main title  

6. **Exporter (`ExporterAgent`)**  
   - Converts final content into:  
     - **DOCX** → Proper headings, code blocks with background  
     - **PDF** → Choice between:  
       - **ReportLab** (structured, lightweight)  
       - **PDFKit** (styled, HTML-like)  
   - Provides download paths  

---

## 🛠️ Tech Stack  

- **Language** → Python  
- **LLM Frameworks** → LangGraph, LangChain  
- **Models** → Local Ollama models:  
  - `mistral:latest`  
  - `llama3:8b`  
  - `codellama:7b`  
- **Export** → DOCX (`python-docx`), PDF (`ReportLab`, `PDFKit`)  
- **UI** → Streamlit  
- **Markdown** → `markdown2`, `pygments`  

---

## 📦 Prerequisites  

- **Python 3.10+**  
- **Ollama** installed & running → [https://ollama.com](https://ollama.com)  
- Pull required models:  

```bash
ollama pull mistral:latest
ollama pull llama3:8b
ollama pull codellama:7b
```
---

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd multi-agent-article-generator
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
---

## Usage

### Running the Web Interface

```bash
streamlit run ui/app.py
```

The application includes a command-line test script to verify the end-to-end workflow.

---

## Project Structure

```
multi_agent_article_generator/
├── agents/
│   ├── topic_analyzer.py
│   ├── content_generator.py
│   ├── code_snippet.py
│   ├── formatter.py
│   └── exporter.py
├── orchestrator/
│   └── workflow.py
├── ui/
│   └── app.py
├── utils/
│   ├── llm_loader.py
│   ├── markdown_utils.py
│   └── file_utils.py
├── requirements.txt
└── README.md
```
---

## How It Works

1. **Topic Analysis**: The Topic Analyzer Agent refines the user's input topic and identifies key sub-topics
2. **Content Generation**: The Content Generator Agent creates a structured article based on the analysis
3. **Code Enhancement**: If requested, the Code Snippet Agent adds relevant code examples
4. **Formatting**: The Formatter Agent ensures consistent styling and structure
5. **Export**: The Exporter Agent converts the final content to PDF and DOCX formats

---

## Troubleshooting

### Tesseract OCR Conflict

If you encounter an error like:
```
cannot load library 'C:\Program Files\Tesseract-OCR\libgobject-2.0-0.dll': error 0x7e
```

This is a known issue on Windows systems where Tesseract OCR interferes with WeasyPrint's dependencies. 
Our system automatically falls back to ReportLab for PDF generation in this case, so functionality is preserved.

To fully resolve this issue:
1. Uninstall Tesseract OCR if you don't need it
2. Or adjust your system PATH to prioritize the correct GTK libraries
3. Or use a virtual environment with isolated dependencies

---

### Model Not Found Errors

If you encounter errors like:
```
Error calling Ollama: 404 Client Error: Not Found for url: http://localhost:11434/api/generate
```

Make sure you're using the correct model names with their tags (e.g., `codellama:7b` instead of just `codellama`).
Check available models with:
```bash
ollama list
```

---

## Customization

You can customize the behavior by modifying:
- Agent prompts in each agent file
- Output formatting in the formatter agent
- Export styling in the exporter agent
- UI elements in the Streamlit app

---

## License

This project is licensed under the MIT License - see the LICENSE file for details.
