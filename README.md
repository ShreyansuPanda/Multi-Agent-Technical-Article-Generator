# 📝Multi-Agent Technical Article Generator

This project implements a Multi-Agent System (MAS) designed to autonomously generate in-depth technical articles (1000–1500 words) with optional code examples, and export them in PDF and DOCX formats.

-----

## Features

- **Topic Analysis**: Parses and validates topic; refines scope and outlines subtopics
- **Content Generation**: Creates detailed, structured text (intro, body, conclusion)
- **Code Snippets**: Generates or embeds code examples (Python, JS, etc.) when requested
- **Formatting**: Applies consistent heading styles, bullets, and syntax highlighting
- **Export**: Generates downloadable PDF and DOCX documents
- **Web Interface**: Streamlit-based UI for easy interaction
  
-----

## Architecture

The system follows a sequential, agent-based pipeline architecture orchestrated by a central `OrchestratorAgent`. Each agent is a specialized module responsible for a distinct step in the article generation process. Data flows from one agent to the next, with each agent refining or adding to the output of the previous one.

This modular design makes the system easy to understand, maintain, and extend.

### Workflow

1.  **Orchestrator (`OrchestratorAgent`)**: This is the central coordinator. It receives the initial topic from the user and directs the workflow by calling each agent in the correct sequence. It manages the state and passes the output of one agent as the input to the next.

2.  **Topic Analyzer (`TopicAnalyzerAgent`)**:
    -   **Input**: Raw topic string from the user.
    -   **Process**: Uses an LLM to analyze the topic, refine its scope, and generate a structured outline including key sub-topics, related fields, and practical applications.
    -   **Output**: A structured markdown text containing the topic analysis.

3.  **Content Generator (`ContentGeneratorAgent`)**:
    -   **Input**: The structured analysis from the Topic Analyzer.
    -   **Process**: Uses the analysis as a detailed prompt for an LLM to write a full-length technical article (1000-1500 words), complete with an introduction, main body, and conclusion.
    -   **Output**: The complete article in markdown format.

4.  **Code Snippet Agent (`CodeSnippetAgent`)**: (Conditional Step)
    -   **Input**: The generated article content.
    -   **Process**: If the user requests code examples, this agent uses a code-specialized LLM (like CodeLlama) to identify relevant sections in the article and generate 2-4 illustrative code snippets.
    -   **Output**: A markdown-formatted string containing code examples.

5.  **Formatter (`FormatterAgent`)**:
    -   **Input**: The main article content and the optional code snippets.
    -   **Process**: Merges the code snippets into the article (typically before the conclusion), cleans up whitespace, and extracts the main title.
    -   **Output**: The final, clean, and fully assembled article in markdown.

6.  **Exporter (`ExporterAgent`)**:
    -   **Input**: The final formatted content and the article title.
    -   **Process**: Converts the markdown into two formats:
        -   **PDF**: Uses `WeasyPrint` to convert styled HTML to PDF. It includes a fallback mechanism to `ReportLab` or a simple HTML file if `WeasyPrint` fails.
        -   **DOCX**: Uses `python-docx` to create a well-structured Word document with proper headings and formatted code blocks.
    -   **Output**: Paths to the newly created PDF and DOCX files.

-----

## Technical Stack

- **Backend Language**: Python
- **LLM Frameworks**: LangChain, LangGraph
- **Language Models**: Ollama (Mistral, Llama3, CodeLlama)
- **PDF Generation**: WeasyPrint (primary), ReportLab (fallback), HTML (final fallback)
- **DOCX Generation**: python-docx
- **UI Framework**: Streamlit
- **Markdown Processing**: markdown2, Pygments

---------

## Prerequisites

1. **Python 3.10+** installed
2. **Ollama** installed and running ([https://ollama.com](https://ollama.com))
3. Required models pulled:
   ```bash
   ollama pull mistral:latest
   ollama pull llama3:8b
   ollama pull codellama:7b
   ```
---------

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
--------

## Usage

### Running the Web Interface

```bash
streamlit run ui/app.py
```

The application includes a command-line test script to verify the end-to-end workflow.

--------

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
├── test_llm.py
├── requirements.txt
└── README.md
```
-----

## How It Works

1. **Topic Analysis**: The Topic Analyzer Agent refines the user's input topic and identifies key sub-topics
2. **Content Generation**: The Content Generator Agent creates a structured article based on the analysis
3. **Code Enhancement**: If requested, the Code Snippet Agent adds relevant code examples
4. **Formatting**: The Formatter Agent ensures consistent styling and structure
5. **Export**: The Exporter Agent converts the final content to PDF and DOCX formats

--------

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

-------------

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

--------------

## Customization

You can customize the behavior by modifying:
- Agent prompts in each agent file
- Output formatting in the formatter agent
- Export styling in the exporter agent
- UI elements in the Streamlit app

-----------------

## License

This project is licensed under the MIT License - see the LICENSE file for details.
