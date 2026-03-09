---
date: '2026-03-09T14:03:34-04:00'
draft: false
title: 'Microsoft Agent Framework'
categories:
  - AI
tags:
  - AgentFramework
  - AI
author: Manish
description: "Understanding the Microsoft Agent Framework and its applications in AI systems"
---



## What is Microsoft Agent Framework

The **Microsoft Agent Framework** is an open-source SDK and runtime designed to simplify building, orchestrating, and deploying multi-agent AI systems. It unifies two previously separate Microsoft projects—**Semantic Kernel** (known for enterprise stability) and **AutoGen** (known for multi-agent research)—into a single, production-ready platform.

### Core Capabilities:
The framework provides tools to create agents that can reason, use external tools, and collaborate with other agents or humans.

- **Multi-Agent Orchestration**: It supports various interaction patterns, including **sequential** (one after another), **concurrent** (parallel), **group chat** (collaborative brainstorming), and **handoff** (transferring tasks between agents).
- **Graph-Based Workflows**: Developers can define structured, deterministic business processes using a graph architecture with "executors" and "edges," allowing for complex logic like fan-out/fan-in and conditional routing.
- **Interoperability Protocols**: It uses open standards like the **Model Context Protocol (MCP)** for connecting tools, **Agent-to-Agent (A2A)** for cross-runtime communication, and **OpenAPI** for integrating REST APIs.
- **Enterprise Readiness**: The framework includes built-in support for **observability** (OpenTelemetry), state persistence (checkpointing), human-in-the-loop approvals, and secure cloud hosting through [Azure AI Foundry](https://azure.microsoft.com/en-us/blog/introducing-microsoft-agent-framework/).

### Technical Implementation

- **Unified Abstractions**: All agents are derived from a common base class, `AIAgent`, which ensures a consistent interface for higher-level orchestrations.
- **Model Agnostic**: Through integration with `Microsoft.Extensions.AI`, it supports various LLM providers, including **Azure OpenAI, OpenAI, Anthropic, and local models via Ollama**.
- **Multi-Language Support**: It is available for both **.NET** and **Python** developers.

Below is code example (.Net) to use the locally hosted model on ollama (make sure you have ollama installed and have running model):

git repo link: https://github.com/healingplace/MicrosoftAgentFramework/tree/main/Agent101


Install the dependencies for the code:

```
dotnet add package Microsoft.Agents.AI --prerelease
dotnet add package Microsoft.Extensions.AI.Ollama --prerelease
```

```
using Microsoft.Agents.AI;
using Microsoft.Extensions.AI;
  

// 1. Initialize the local Ollama chat client
// using ministral-3 model, you can use any model of your choice for testing

var chatClient = new OllamaChatClient(

    new Uri("http://localhost:11434"),

    modelId: "ministral-3");
    

// 2. Create the agent using the chat client

AIAgent agent = chatClient.AsAIAgent(

    name: "LocalAssistant",

    instructions: "You are a helpful assistant running entirely on this machine."

);


// 3. Run the agent

var response = await agent.RunAsync("What is the capital of Canada?");

Console.WriteLine(response.Text);

```
