# Product Copilot

**Exploring how AI works, one question at a time.**

My curiosity about AI started with a simple question: what actually happens behind the scenes when we ask an LLM a question?

Rather than relying only on ready made AI tools, I wanted to understand how the pieces work together. I started exploring retrieval augmented generation (RAG) and built a small Product Management Copilot from scratch to learn by doing.

## The idea

I wanted to create an assistant that could answer Product Management interview questions using my own collection of PDF documents.

Instead of asking an LLM to answer from its general knowledge, I wanted to see whether it could find relevant information in a defined set of documents, use that information to generate an answer, and recognise when the answer was not supported by the available material.

## How it works

The prototype follows a simple RAG workflow.

1. **Ingest:** Extract text from PDF documents and split it into smaller chunks.
2. **Embed:** Convert the chunks into vector representations for semantic search.
3. **Retrieve:** Find the five most relevant chunks for a question.
4. **Generate:** Pass the retrieved content to a local LLM to produce an answer.
5. **Evaluate:** Test different questions and review the answers, citations, and refusal behaviour.

## What I built

The project includes a Python pipeline for document ingestion, retrieval, answer generation, and evaluation, along with a Streamlit interface for asking questions.

I used ChromaDB for vector storage, Sentence Transformers for embeddings, and Llama 3.2 through Ollama for local answer generation.

## What I learned

Building the pipeline helped me understand that retrieving relevant information and generating a reliable answer are two different challenges.

My initial tests revealed inconsistent citations, occasional incorrect refusals, and answers that were not always fully grounded in the source documents.

These results gave me a clearer understanding of why RAG systems need evaluation beyond simply checking whether they produce an answer.

## What I want to explore next

Improve citation accuracy, reduce incorrect refusals, compare retrieval settings, and understand how different models affect answer quality.

For me, this project is part of learning AI by building, testing, and questioning how things work under the hood.

## Technology

Python, ChromaDB, Sentence Transformers, Ollama, Llama 3.2, and Streamlit.
