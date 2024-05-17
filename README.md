# Langchain-Tutorial
# LangChain Vector Store Retriever Tutorial

## Introduction

This repository demonstrates how to use a Vector Store retriever in a conversational chain with **LangChain**, comparing two popular vector stores: **LanceDB** and **Chroma**. These tools help manage and retrieve data efficiently, making them essential for AI applications.

## Setup Instructions

### 1. Create a Virtual Environment

First, create a virtual environment to manage your project dependencies.

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows use `.\venv\Scripts\activate`
```

### 2. Install Dependencies

Install the required packages using the `requirements.txt` file.

```bash
pip install -r requirements.txt
```

### 3. Set Up Environment Variables

Create a `.env` file in your project directory and add your OpenAI API key:

```
OPENAI_API_KEY=your_openai_api_key
```

### 4. Run the Script

Execute the `langchain_agent.py` script to load documents, embed them into a vector store, and perform retrieval operations.

```bash
python langchain_agent.py
```

## Script Overview

### Environment Setup

The script begins by loading environment variables from a `.env` file and setting up the OpenAI API key.

```python
import os
import openai
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv())
openai.api_key = os.getenv('OPENAI_API_KEY')
```

### Global Variables

Two global lists are used to store documents and their splits.

```python
docs = []  # List to store loaded documents
splits = []  # List to store split documents
```

### Functions

#### `load_pdf(pdf_names)`

This function loads PDF documents and splits them into smaller chunks.

#### `split_documents(docs)`

This function splits documents into smaller chunks for processing, improving manageability and retrieval relevance.

#### `embed_and_store_splits(splits)`

This function embeds and stores document splits in Chroma, a vector store optimized for small to medium-scale applications.

#### `mmr_search(question, vectordb)`

This function performs an MMR (Maximal Marginal Relevance) search and answers questions based on the vector store.

#### `question_answering(llm, compressed_docs, db, question)`

This function answers the user's question based on the compressed documents retrieved from the vector store.

#### `pretty_print_docs(docs)`

This function prints the contents of documents in a readable format.

### Main Function

The `main()` function orchestrates the loading, splitting, embedding, and querying of documents.

1. **Loading PDF Documents**: Prompts the user to enter filenames of PDFs located in the `example_materials` directory.
2. **Embedding and Storing**: Loads and splits the documents, embeds them into Chroma, and stores them.
3. **Querying**: Allows the user to ask questions about the documents and retrieves relevant information.

```python
def main():
    # Directory where example materials are located
    example_dir = "example_materials/"

    # Ask user to load PDF documents
    pdf_filenames = input("Enter the filenames of the PDF files in the 'example_materials' directory, separated by commas: ").split(',')
    pdf_filenames = [os.path.join(example_dir, filename.strip()) for filename in pdf_filenames]

    # Load and split the documents
    splits = load_pdf(pdf_filenames)
    print("Documents loaded and split into chunks.")

    # Embed and store the splits
    vectordb = embed_and_store_splits(splits)
    print("Documents embedded and stored.")

    while True:
        # Ask user a question about the documents
        question = input("Enter a question about the documents (or 'exit' to quit): ")
        if question.lower() == 'exit':
            break

        # Perform MMR search and print results
        results = mmr_search(question, vectordb)
        print(results["result"])

if __name__ == "__main__":
    main()
```

## Conclusion

This repository provides a comprehensive tutorial on using Vector Store retrievers with **LangChain**, demonstrating the capabilities of **LanceDB** and **Chroma**. Each tool has its strengths and is suited to different types of projects, making this tutorial a valuable resource for understanding and implementing vector retrieval in AI applications. 

For further reading and resources, check out the [LangChain Documentation](https://api.python.langchain.com/en/latest/) and the [DeepLearning.AI LangChain Course](https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/).
