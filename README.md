# Product Copilot

**Exploring AI through product thinking, experimentation, and prototyping.**

My curiosity about AI started with a simple question: what happens behind the scenes when we ask an LLM a question?

I wanted to go beyond using ready made AI tools and understand how AI applications work in practice. Using Cursor AI as my development assistant, I built a Product Management interview copilot to explore retrieval augmented generation (RAG), test different retrieval settings, and evaluate answer quality.

## The idea

The goal was to create an assistant that could answer Product Management interview questions using a collection of PDF documents.

Rather than relying only on an LLM's general knowledge, I wanted to explore whether retrieving relevant information from a defined document collection could produce more grounded answers.

## My approach

I approached this as a product experiment:

1. **Define the use case:** Help users explore Product Management interview questions using a focused document collection.
2. **Explore the workflow:** Understand how document ingestion, embeddings, retrieval, and answer generation fit together.
3. **Make retrieval decisions:** Compare retrieving 5 versus 10 chunks to investigate how the amount of context affects answer quality.
4. **Evaluate the output:** Test questions and review answer relevance, source citations, grounding, and refusal behaviour.
5. **Identify improvements:** Use observed failures to identify opportunities to improve reliability.

I used Cursor AI to help implement the application. My focus was on understanding the workflow, making product and experiment decisions, testing the prototype, and interpreting the results.

## How it works

The prototype follows a retrieval augmented generation workflow:

1. **Ingest:** Extract text from PDF documents and split it into smaller chunks.
2. **Embed:** Convert chunks into vector representations for semantic search.
3. **Retrieve:** Find relevant chunks for a user's question.
4. **Generate:** Pass the retrieved content to a local LLM to generate an answer.
5. **Evaluate:** Review answers, citations, grounding, and refusal behaviour.

## What I learned

The experiment reinforced an important distinction: retrieving relevant information does not automatically produce a reliable answer.

My tests exposed limitations in citation accuracy, answer grounding, and the system's ability to recognise when information was missing. These findings highlighted the importance of evaluating AI applications against defined criteria rather than judging them only by whether they produce an answer.

## Technology

Python, ChromaDB, Sentence Transformers, Ollama, Llama 3.2, Streamlit, and Cursor AI.

## Project materials

- [Project presentation](https://docs.google.com/presentation/d/1QjBAjTKasTCv3R4ATYLV3Gpx4suKrDiSL4ZI6n-rwGQ/edit?slide=id.p1#slide=id.p1)
- [LinkedIn project post](https://lnkd.in/p/gc2G3_KZ)

## Next steps

Improve citation reliability, investigate retrieval settings further, compare model behaviour, and explore how document based AI assistants could support knowledge retrieval and business workflows.

This project is part of my ongoing AI learning journey: exploring how the technology works, testing ideas, and learning from the limitations.
