---
date: '2026-03-10T11:27:15-04:00'
draft: true
title: 'Microsoft Agent Framework Tools'
categories:
  - AI
tags:
  - AgentFramework
  - AI
author: Manish
description: "Understanding the Microsoft Agent Framework and its applications in AI systems"
---



https://learn.microsoft.com/en-us/agent-framework/get-started/add-tools?pivots=programming-language-csharp

In the **Microsoft Agent Framework**, **Tools** are capabilities that allow an AI agent to **perform actions outside the LLM**, such as calling APIs, running code, searching data, or interacting with external systems.

Without tools, an agent can **only generate text**. With tools, it can **act**, fetch information, or execute tasks.

## What Are Tools in Microsoft Agent Framework?

Tools = Functions or services that an agent can call while solving a task.
Think of them like **skills or abilities** you give to the agent.

### Types of Tools in Microsoft Agent Framework

Microsoft Agent Framework supports several tool types.

#### 1. Function Tools (Custom Code)
	Developer-defined functions that the agent can call.
	
	Example:
	- GetWeather()
	- SendEmail()
	- GetCustomerOrders()

These are the **most common tools** developers create.

#### 2. Code Interpreter
	Allows the agent to **run code in a sandbox environment**.

Example tasks:
- Data analysis
- Calculations
- File processing

#### 3. File Search
	Allows agents to **search inside uploaded documents**.

Example:
- PDFs
- knowledge bases
- manuals

#### 4. Web Search
	Allows the agent to retrieve **live information from the web**.

#### 5. MCP Tools
	Tools exposed through **Model Context Protocol (MCP)** servers.
Example integrations:
- GitHub
- Databases
- enterprise APIs

## 3. How Tools Work Internally

When a user sends a prompt:

1️⃣ User asks something  
			|
2️⃣ LLM analyzes the request  
			|
3️⃣ LLM decides a **tool is required**  
			|
4️⃣ Framework calls the tool  
			|
5️⃣ Tool returns result  
			|
6️⃣ LLM generates final response

Code Sample:

```
// Tool method and class

using System.ComponentModel;

public class AgentToolWeather

{

    [Description("Gets the current weather for a specified location.")]

    public static string GetWeather(string location)

    {

        // In a real implementation, this method would call a weather API to get the current weather for the specified location.

        // For this example, we'll just return a dummy weather report.

        return $"The current weather in {location} is sunny with a temperature of 25°C.";

    }

}
```

```
// Agent Tool configuration
using Microsoft.Agents.AI;
using Microsoft.Extensions.AI;

// Initialize the local Ollama chat client
var chatClient = new OllamaChatClient(

    new Uri("http://localhost:11434"),
    modelId: "ministral-3");
  
// Create the agent using the chat client
AIAgent agent = chatClient.AsAIAgent(
    name: "LocalAssistant",
    instructions: "You are a helpful assistant running entirely on this machine.",
    tools: [AIFunctionFactory.Create(AgentToolWeather.GetWeather)]);

// Run the agent
var response = await agent.RunAsync("What is the current weather in Seattle?");
Console.WriteLine(response.Text);
```

Code repo:
https://github.com/healingplace/MicrosoftAgentFramework/tree/main/AgentTools

