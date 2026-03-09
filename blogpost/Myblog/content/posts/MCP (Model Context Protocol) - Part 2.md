---
date: '2026-03-09T12:30:56-04:00'
draft: false
title: 'MCP (Model Context Protocol)   Part 2'
categories:
  - AI
tags:
  - MCP
  - AI
author: Manish
description: "Understanding the Model Context Protocol (MCP) and its applications in AI systems"
---

# Architecture and the Concepts of MCP

The **Model Context Protocol (MCP)** follows a clean, modular **client–server architecture** designed to connect AI applications to external systems in a structured and scalable way.

At its core, MCP separates responsibilities between the AI application, connection managers, and context providers.

## Participants in MCP

MCP introduces three key participants:
- **MCP Host** – The AI application that coordinates everything
- **MCP Client** – The connection manager that talks to a specific MCP server
- **MCP Server** – The program that provides context (tools, resources, prompts)

### 1️⃣ MCP Host (AI Application)

The **MCP Host** is the AI-powered application that users interact with.
Examples include:
- Claude Desktop
- Claude Code
- Visual Studio Code (when extended with AI capabilities)

The host is responsible for:
- Managing one or more MCP clients
- Coordinating requests to MCP servers
- Aggregating context for the AI model

Think of the MCP Host as the **orchestrator**.

### 2️⃣ MCP Client

An **MCP Client** is instantiated by the host.
Its job is simple but critical:
- Maintain a **dedicated connection** to one MCP server
- Send requests
- Receive context
- Relay information back to the host

Each MCP server connection gets its **own MCP client instance**.
This means:
- If the host connects to 4 servers → it creates 4 MCP clients
- Each client maintains its own isolated connection

This isolation improves reliability and modularity.

### 3️⃣ MCP Server

An **MCP Server** is a program that provides structured context to MCP clients.
It may expose:
- Tools (e.g., execute query, run search)
- Resources (e.g., files, records)
- Prompts (predefined workflows)
- Notifications (events)

Importantly:

> An MCP server refers to the program serving context regardless of where it runs.

It can be:
- **Local** (running on the same machine)
- **Remote** (running on an external platform)

## Local vs Remote MCP Servers

### 🖥 Local MCP Servers (STDIO Transport)
Local servers typically:
- Run on the same machine as the host
- Use the **STDIO transport**
- Serve a single MCP client

Example:
- A local filesystem server launched by Claude Desktop

These are commonly used for:
- File access
- Local database interaction
- Development workflows

### 🌐 Remote MCP Servers (Streamable HTTP Transport)

Remote servers:
- Run on external infrastructure
- Use **Streamable HTTP transport**
- Often serve multiple MCP clients

Example:
- The official Sentry MCP server running on Sentry

These are typically used for:
- SaaS integrations
- Monitoring systems
- Cloud-based tools

```
                MCP Host (AI Application)
                         │
      ┌──────────────────┼──────────────────┐
      │                  │                  │
  MCP Client 1      MCP Client 2      MCP Client 3
      │                  │                  │
  MCP Server A      MCP Server B      MCP Server C
   (Local)            (Local)           (Remote)
```

Each MCP Client maintains a **dedicated connection** to exactly one MCP Server.

## The Two Layers of MCP

MCP is structured into two logical layers:

### 1️⃣ Data Layer

The **Data Layer** defines the protocol itself.

It is based on **JSON-RPC** and specifies:

- Client-server communication patterns
- Lifecycle management: MCP is a stateful protocol that requires lifecycle management.
- Core primitives such as:
    - Tools
    - Resources
    - Prompts
    - Notifications

This layer defines **what is communicated** and **how it is structured**.
It ensures interoperability between hosts and servers.

### 2️⃣ Transport Layer

The **Transport Layer** defines **how messages move between client and server**.
It handles:
- Connection establishment
- Message framing
- Streaming
- Authorization
- Transport-specific behavior

Common transports include:
- **STDIO** (for local processes)
- **Streamable HTTP** (for remote servers)

If the Data Layer defines the _language_, the Transport Layer defines the _wire_.

Examples:

Data Layer