---
date: '2026-03-15T00:56:08-04:00'
draft: false
title: 'Intro to Vectordb'
categories:
  - AI
tags:
  - AI
  - VectorDB
author: Manish
description: "Introduction to vector database"
---


## What is a “vector” in this context?

A vector database is a type of database designed to store and search data represented as vectors (arrays of numbers). These vectors usually come from machine learning models that convert text, images, audio, or other data into numerical embeddings so computers can measure similarity between them.

A vector is simply a list of numbers representing the meaning or features of something.

Example:  
The sentence “I love pizza” might be converted by an embedding model into something like:

```
[0.12, -0.84, 0.33, 0.91, ...]
```

Another sentence like “Pizza is my favorite food” will produce a similar vector, meaning the database can detect that they are semantically related.

This relies on the idea of Vector Embedding.

## What a vector database actually does

A vector DB stores these embeddings and lets you quickly find **similar vectors**.

Main operations:
1. **Store embeddings**
2. **Index vectors for fast search**
3. **Run similarity searches**

Example query:

> Find documents most similar to:  
> “How to train a dog”

The DB returns documents with similar meaning, even if the words are different.

## How similarity search works

Vector databases use mathematical distance measures like:

- Cosine Similarity
- Euclidean Distance
- Dot Product

These help determine how close two vectors are in high-dimensional space.

## Why vector databases became popular

They are essential for modern AI applications, especially:
- Retrieval-augmented generation (RAG)
- Semantic search
- Recommendation systems
- Image similarity search
- Chatbots with memory

Example:  
When you ask an AI a question, it may search a vector DB for relevant documents before generating the answer.

## Popular vector databases

Some commonly used vector DB systems include:
- Pinecone
- Milvus
- Weaviate
- Chroma
- FAISS

## Example workflow

A typical AI system using a vector DB:
1. Convert documents to embeddings using a model like OpenAI Embeddings API.
2. Store vectors in a vector database.
3. User asks a question.
4. Convert the question into a vector.
5. Find closest vectors in the database.
6. Send the retrieved data to an LLM to generate the answer.
This approach is commonly called Retrieval-Augmented Generation.

A vector database is a database optimized for searching meaning instead of exact words.