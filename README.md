# Project: PDF Chat Bot

This project implements a Retrieval-Augmented Generation (RAG) pipeline to create a chatbot that can answer questions about a PDF document. The system uses LangChain to orchestrate the process, OpenAI's language models for understanding and generation, and a vector store for efficient document retrieval. The entire workflow is structured as a graph using `langgraph`.

## How it Works

The core of the project is a RAG pipeline that allows an LLM to "read" and answer questions from a provided PDF file. Here’s a breakdown of the process:

1.  **Load and Split**: The PDF document is loaded and split into smaller, manageable chunks of text. This is done to facilitate efficient searching and to fit within the context window of the language model.
2.  **Embedding and Indexing**: Each text chunk is converted into a numerical representation (an embedding) using OpenAI's `text-embedding-3-large` model. These embeddings are then stored in an in-memory vector store for fast retrieval.
3.  **Query Analysis**: When a user asks a question, the system first uses a powerful language model (`gpt-4o-mini`) to analyze the query and extract the key information to search for.
4.  **Document Retrieval**: The system searches the vector store to find the text chunks that are most semantically similar to the user's question.
5.  **Answer Generation**: The retrieved text chunks (the "context") and the original question are passed to the language model. The model then generates a natural language answer based on the information found in the document.

This entire multi-step process is defined and executed as a stateful graph using `langgraph`, ensuring a clear and robust workflow.

## Getting Started

### Prerequisites

Before you begin, ensure you have the following:

  * Python 3.7+
  * Access to a Google Colab environment or a local machine with Python installed.
  * API keys for:
      * OpenAI
      * LangChain (for tracing with LangSmith)

### Installation

The project requires several Python packages. You can install them using pip:

```bash
pip install --upgrade langchain-text-splitters langchain-community langgraph langchain-openai pypdf
```

### Configuration

1.  **API Keys**: The script will prompt you to enter your OpenAI API key. You should also set your LangChain API key as an environment variable for tracing purposes.

    ```python
    import os
    import getpass

    os.environ["LANGCHAIN_API_KEY"] = "YOUR_LANGCHAIN_API_KEY"
    os.environ["OPENAI_API_KEY"] = getpass.getpass("Enter API key for OpenAI: ")
    ```

2.  **PDF File**: Place your PDF file in a location accessible by the script and update the `file_path` variable. In the provided notebook, it's loaded from Google Drive.

    ```python
    file_path = "/path/to/your/document.pdf"
    ```

### Running the Chatbot

To ask a question, simply call the `ask_question` function with your query.

```python
question = "What is the main topic of the document?"
answer = ask_question(question)
print(answer)
```

The system will process the PDF, retrieve relevant information, and generate an answer based on the document's content. If no relevant information is found, it may respond with "I don't know."
