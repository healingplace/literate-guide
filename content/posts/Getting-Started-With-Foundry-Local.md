---
date: '2026-07-24T13:31:44-04:00'
draft: false
title: 'Getting Started With Foundry Local'
categories:
  - AI
tags:
  - Foundry
  - MicrosoftFoundry
  - C#
  - dotnet
  - .Net
  - AI
author: Manish
description: "How to setup the Microsoft Foundry Local"
---

# Getting Started with Microsoft Foundry Local in C#

If you have ever used Ollama to run a model on your own machine, Foundry Local will feel familiar right away. Microsoft's version of the same idea runs on ONNX Runtime, detects your hardware, downloads a model optimized for it, and gives you a local endpoint to talk to. The difference for us as .NET developers is that we get a native C# SDK instead of shelling out to a CLI or wrapping a REST call ourselves.

In this post I will walk through installing Foundry Local, setting up a minimal C# console project, and running a first chat completion entirely on my own machine, no Azure subscription, no API key, no cloud call.

## Why bother with a local model at all

A few reasons this is worth having in your toolbox as a .NET dev, even if most of your production work stays in the cloud:

- Zero cost experimentation. You can iterate on prompts and agent logic without burning through API quota.
- Offline development. Useful on a plane, in a client environment with locked down networking, or just when you want to work without depending on connectivity.
- Privacy by default for anything you don't want leaving your machine during development.
- A very short path from "local prototype" to "cloud deployment," since the API shape stays close to what you would use against a hosted Foundry model.

## Step 1: Install the Foundry Local runtime

On Windows, this is a single winget command:

```powershell
winget install Microsoft.FoundryLocal
```

Close and reopen your terminal so the `foundry` command lands on your PATH, then confirm it worked:

```powershell
foundry --version
foundry model list
```

That second command is worth running once just to see what is available and which variants Foundry Local considers a good match for your hardware.

## Step 2: Set up the C# project

You can setup a new dotnet Console app and use the code provided below but I had to add 
``` <RuntimeIdentifier>win-x64</RuntimeIdentifier> ``` on my csproj file, to make it run.

Here is the project file I used. Nothing exotic, just a console app targeting .NET 10 with the Foundry Local NuGet package added:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <RootNamespace>FoundryLocal101_Chat</RootNamespace>
    <ImplicitUsings>enable</ImplicitUsings>
    <RuntimeIdentifier>win-x64</RuntimeIdentifier>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AI.Foundry.Local" Version="1.2.3" />
  </ItemGroup>

</Project>
```

A couple of notes on the choices here:

- I used the plain `Microsoft.AI.Foundry.Local` package rather than the `.WinML` variant. The WinML package gives you GPU/NPU acceleration on Windows, but it needs a real DirectX 12 capable GPU and adds setup weight I did not need for a first test. Start plain, add WinML later if you want the speed.
- `RuntimeIdentifier` is pinned to `win-x64` here since I built this on Windows. Drop or change that if you are on macOS or Linux.

## Step 3: The code

This is the full program. It initializes the Foundry Local manager, pulls a model from the catalog, downloads and loads it, then sends a single chat message.

```csharp
using Microsoft.AI.Foundry.Local;
using Microsoft.Extensions.Logging.Abstractions;
using Betalgo.Ranul.OpenAI.ObjectModels.RequestModels;

await FoundryLocalManager.CreateAsync(
    new Configuration { AppName = "my-app" },
    NullLogger.Instance);

var catalog = await FoundryLocalManager.Instance.GetCatalogAsync();

var modelName = "phi-3.5-mini";

var model = await catalog.GetModelAsync(modelName)
    ?? throw new Exception("Model not found.");

await model.DownloadAsync(); 
await model.LoadAsync();

var chat = await model.GetChatClientAsync();

var response = await chat.CompleteChatAsync(new[] {
    new ChatMessage { Role = "user", Content = "Hello!" }
});
Console.WriteLine(response.Choices![0].Message.Content);
```

Walking through it piece by piece:

**Initialize the manager.** `FoundryLocalManager.CreateAsync` sets up a singleton instance for your app. Passing `AppName` mostly matters for logging and diagnostics, and `NullLogger.Instance` keeps things quiet for a quick test. Swap that for a real logger once you move past a scratch project.

**Get the catalog.** `GetCatalogAsync` gives you access to the list of models Foundry Local knows about and can fetch. This talks to the Foundry model catalog over the network the first time, so you do need connectivity for this step even though inference itself runs locally afterward.

**Look up and materialize the model.** `catalog.GetModelAsync("phi-3.5-mini")` finds the model entry by alias. I picked `phi-3.5-mini` deliberately here since it is small and fast, good for a first run where you just want to confirm the whole pipeline works before committing to a larger download. `DownloadAsync` pulls the hardware-optimized variant for your machine, and `LoadAsync` brings it into memory. The first run will take a bit depending on your connection, everything after that loads from local cache.

**Get a chat client and send a message.** `GetChatClientAsync` returns a client scoped to that specific loaded model, and from there it is a normal chat completion call, one user message in, one response out.

Run it with `dotnet run`, and on first execution expect a pause while the model downloads. Every run after that should be quick.

## What I would try next

A few natural follow ups once this is working:

- Point the same chat client pattern at a hosted Foundry model in the cloud and compare the code differences. Since the shape stays close to the same, this makes a good side by side for understanding when local versus hosted makes sense.
- Try a second model alias to compare response quality and load time against `phi-3.5-mini`.
- Wrap the download and load steps with proper logging instead of `NullLogger`, since in a real app you will want visibility into what is happening on first run versus cached runs.

That is the whole loop: install the runtime, reference one NuGet package, initialize the manager, pull a model, chat with it. No cloud resource to provision, no key to manage, just a model running on the machine in front of you.
