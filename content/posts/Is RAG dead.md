---
date: '2026-03-10T12:29:39-04:00'
draft: false
title: 'Is RAG Dead'
categories:
  - AI
tags:
  - RAG
  - AI
author: Manish
description: "Is Retrieval-Augmented Generation (RAG) dead?"
---



**Is RAG dead in the era of models with massive context windows?**

As LLMs evolved, we introduced **Retrieval-Augmented Generation (RAG)** to overcome a key limitation: models have a **knowledge cutoff** and lack access to up-to-date or proprietary data. RAG helped by retrieving relevant documents and injecting them into the model’s prompt, grounding responses in real context.

But now we’re seeing models with **hundreds of thousands or even millions of tokens of context**. So the question naturally comes up:

**Do we still need RAG?**

I think **this is another classic system design trade-off.**

Large context windows reduce the friction of providing knowledge to models. In theory, you could simply load large documents, knowledge bases, or conversation history directly into the prompt.

However, several practical considerations keep RAG very relevant:

• **Cost & latency** – Passing massive context every time can be expensive and slow. Retrieval allows us to send only the most relevant information. RAG enables small cost effective models to work as effective as large models for the specific needs.
• **Signal vs. noise** – More context doesn’t always mean better answers. Retrieval helps narrow the model’s attention to the right pieces of knowledge.  
• **Dynamic knowledge** – RAG enables systems to work with continuously updated data sources without retraining or re-prompting huge corpora.  
• **Scalability** – When knowledge bases become extremely large, intelligent retrieval and evaluation become critical.


In reality, the future likely isn’t **RAG vs. large context**, but **RAG + large context** working together.

Large context windows expand what’s possible, while RAG provides **structured, efficient knowledge access**. The real challenge shifts toward **better retrieval, ranking, and knowledge evaluation pipelines**.

**RAG isn’t dead, it’s evolving.**

Curious how others are thinking about this trade-off in their AI system designs.  
#AI #LLM #RAG #SystemDesign #GenerativeAI