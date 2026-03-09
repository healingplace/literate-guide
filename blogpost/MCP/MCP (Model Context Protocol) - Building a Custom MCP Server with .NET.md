
In this tutorial, we’ll build a custom **Model Context Protocol (MCP)** server using the official .NET project template.

By the end, you’ll have:
- A working MCP server
- A custom tool implemented in C#
- Support for either local (stdio) or remote (HTTP) transport

Let’s get started.

# Prerequisites

Before creating the project, ensure you have:

- ✅ **.NET 10.0 SDK or later**
- ✅ Visual Studio Code
- ✅ .NET VS Code extension installed

> The `Microsoft.McpServer.ProjectTemplates` package requires .NET 10+.

## Step 1 — Install the MCP Server Template

Open a terminal and run:

```
dotnet new install Microsoft.McpServer.ProjectTemplates
```

## Step 2 — Create the Project in VS Code

1. Open **Visual Studio Code**
2. Go to **Explorer**
3. Select **Create .NET Project**

Alternatively:

- Press `Ctrl + Shift + P` (Windows/Linux)
- Or `Cmd + Shift + P` (Mac)
- Type: `.NET`
- Select: **.NET: New Project**

You’ll see a dropdown of available .NET project templates.

```
MCP Server App
```

## Step 3 — Configure the Project

You’ll be prompted for several options.
### Project Name

Enter: *MyMCPServer*

### Solution File Format

Choose either:
- `.sln`
- `.slnx`
Both work fine — `.sln` is the traditional format and .slnx is the new format for current and future version of dotnet.

### Template Options

You’ll now configure your MCP server.
#### 1️⃣ Framework

Choose your target .NET version (e.g., net10.0).
#### 2️⃣ MCP Server Transport Type

You have two options:
##### 🖥 Local Server (stdio)

- Runs as a local process
- Communicates over standard input/output
- Ideal for:
    - Local development
    - File system tools
    - IDE integrations

This configures:
```
.WithStdioServerTransport()
```

##### 🌐 Remote Server (HTTP)

- Runs as an HTTP endpoint
- Supports multiple clients
- Ideal for:
    
    - Cloud deployments
    - Shared tool servers
    - SaaS integrations

This configures:

```
.WithHttpServerTransport()
```

and includes:
```
MapMcp()
```

#### 3️⃣ Native AOT (Optional)

Enables ahead-of-time compilation:

- Faster startup
- Self-contained binary
- No runtime dependency

Great for production environments.

#### 4️⃣ Self-Contained Publish (Optional)

Bundles the .NET runtime with your app. Your server becomes a standalone executable.

After selecting your preferences, click: "*Create Project*"
VS Code will open your newly created MCP server.

## Step 4 — Update Package Identity 
(optional if you don't want to publish Nuget package)

Open the `.csproj` file and update:
```
<PackageId>YourName.SampleMcpServer</PackageId>
```

# Project Structure Overview

The template generates several important files.
Let’s walk through them.
## Program.cs

This is the entry point of your MCP server.
It:
- Configures the application as an MCP server
- Registers MCP services
- Configures transport type
- Registers tools

#### Example (stdio transport)

```
builder.Services
    .AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();
```

#### Example (HTTP transport)

```
builder.Services
    .AddMcpServer()
    .WithHttpServerTransport();

app.MapMcp();
```

This is where your MCP server becomes either local or remote.

### RandomNumberTools.cs

This file contains a sample tool implementation.

Example concept:
```
[McpTool]
public static int GetRandomNumber(int min, int max)
{
    return Random.Shared.Next(min, max);
}
```

This demonstrates:

- Tool registration via attributes
- Strongly typed arguments
- Automatic JSON schema generation
- Tool exposure via MCP protocol
When your server runs, clients can discover this tool using:
```
tools/list
```
And invoke it using:
```
tools/call
```
