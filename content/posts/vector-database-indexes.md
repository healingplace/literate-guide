---
date: '2026-03-31T14:28:50-04:00'
draft: true
title: 'Vector Database Indexes'
categories:
  - VectorDatabase, RAG, AI
tags:
  - AI
  - RAG
  - Vector Database
author: Manish
description: "Brief intro for vector database indexes"
---


## Beyond Brute Force: A Guide to Vector Database Indexing

In the world of generative AI and Retrieval-Augmented Generation (RAG), **vector databases** are the unsung heroes. They allow us to store and query high-dimensional embeddings at scale. However, as your dataset grows from a few thousand to millions of vectors, a standard "search" becomes a massive computational bottleneck.

This is where **indexing** comes in. To keep searches lightning-fast, vector databases trade a tiny bit of mathematical perfection for massive gains in speed a concept known as **Approximate Nearest Neighbor (ANN)** search.

Here is a breakdown of the primary index types you’ll encounter when architecting AI systems.

### 1. The Gold Standard: HNSW (Hierarchical Navigable Small World)

If you are looking for the best balance of speed and accuracy, **HNSW** is likely your starting point. It organizes data into a multi-layered graph.

- **How it works:** Think of it like a skip list or a highway system. The top layers have fewer points (the "exits" are far apart) for fast traversal across the dataset. As you move down the layers, the graph becomes denser until you reach the bottom, which contains every data point.
    
- **The Trade-off:** It is incredibly fast and accurate, but it is a "memory hog." Because it stores all those graph connections, it requires more RAM than other methods.

### 2. The Space Saver: Product Quantization (PQ)

When you have billions of vectors and a limited budget for memory, **Product Quantization** is the go-to compression technique.

- **How it works:** PQ breaks down high-dimensional vectors into smaller sub-vectors and "rounds" them to the nearest representative value (a centroid). It essentially compresses 32-bit floats into small codes.
    
- **The Trade-off:** You save up to 95% on memory, but you lose some precision. This is often paired with an **IVF (Inverted File)** index to narrow down the search area before the compressed vectors are compared.

### 3. The Specialist: Locality Sensitive Hashing (HSH)

LSH is an older but clever technique used primarily when speed is the absolute priority over everything else.

- **How it works:** It uses mathematical hashing functions to group similar vectors into "buckets." If two vectors are close together in high-dimensional space, they are likely to end up in the same bucket.
    
- **The Trade-off:** It's very fast, but its accuracy can be hit-or-miss compared to graph-based methods like HNSW.

### 4. The Purist: Flat Indexing

A **Flat Index** isn't actually "approximate", it is a brute-force search.

- **How it works:** The database calculates the distance between your query and _every single vector_ in the store.
    
- **The Trade-off:** It provides 100% accuracy (perfect recall). However, because it has $O(n)$ complexity, it will eventually crawl to a halt as your database grows. It’s perfect for small-scale testing but rarely used in large-scale production.

### Which Index Should You Choose?

Selecting the right index depends entirely on your specific constraints:

| **If your priority is...**   | **Use this Index**  |
| ---------------------------- | ------------------- |
| **High Accuracy & Speed**    | **HNSW**            |
| **Minimum Memory Footprint** | **IVF-PQ**          |
| **Small Datasets (<10k)**    | **Flat**            |
| **Massive Scale / Low Cost** | **SCANN or IVF-SQ** |


As we move toward more complex **Agentic AI** systems and larger RAG implementations, understanding these indexes is crucial. For most enterprise applications, starting with **HNSW** provides the most reliable performance, while **Quantization** techniques offer a vital path for scaling without breaking the bank on infrastructure.