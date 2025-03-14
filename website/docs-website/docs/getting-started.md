---
id: getting-started
title: Getting Started with Daytona SDK
sidebar_position: 1
slug: /
---

The Daytona SDK provides official Python and TypeScript interfaces for interacting with Daytona, enabling you to programmatically manage development environments and execute code.

## Prerequisites

Before you begin, ensure you have:

- Python 3.7+ or Node.js 14+ installed
- A Daytona server instance
- API key for authentication (if required)

## Installation

### Python Installation

```bash
pip install daytona-sdk
```

### TypeScript/JavaScript Installation

```bash
# Using npm
npm install @daytona/sdk

# Using yarn
yarn add @daytona/sdk

# Using pnpm
pnpm add @daytona/sdk
```

## Quick Start

### Python Example

```python
from daytona_sdk import Daytona, DaytonaConfig

# Initialize with default configuration
daytona = Daytona()

# Or with custom configuration
config = DaytonaConfig(
    api_key="your-api-key",
    server_url="https://your-server-url",
    target="local"
)
daytona = Daytona(config)

# Create a workspace
workspace = daytona.create()

try:
    # Run some code
    response = workspace.process.code_run('''
        print("Hello from Daytona!")
    ''')
    print(response.result)
finally:
    # Clean up
    daytona.remove(workspace)
```

### TypeScript Example

```typescript
import { Daytona, DaytonaConfig } from '@daytona/sdk';

// Initialize with default configuration
const daytona = new Daytona();

// Or with custom configuration
const config: DaytonaConfig = {
    apiKey: "your-api-key",
    serverUrl: "https://your-server-url",
    target: "local"
};
const daytona = new Daytona(config);

async function main() {
    // Create a workspace
    const workspace = await daytona.create({
        language: 'typescript'
    });

    try {
        // Run some code
        const response = await workspace.process.codeRun(`
            console.log("Hello from Daytona!");
        `);
        console.log(response.result);
    } finally {
        // Clean up
        await daytona.remove(workspace);
    }
}

main().catch(console.error);
```

## Environment Variables

The SDK looks for these environment variables:

- `DAYTONA_API_KEY` - Your Daytona API key
- `DAYTONA_SERVER_URL` - URL of your Daytona server
- `DAYTONA_TARGET` - Target environment (defaults to "local")

You can set these in a `.env` file:

```bash
DAYTONA_API_KEY=your-api-key
DAYTONA_SERVER_URL=https://your-server-url
DAYTONA_TARGET=local
```

## Core Features

The SDK provides several key features:

- 🚀 **Workspace Management**: Create, manage, and remove development environments
- 📂 **File System Operations**: Full file system access and management
- 🔄 **Git Integration**: Built-in Git operations support
- ⚡ **Process Execution**: Run commands and code in isolated environments
- 🔧 **LSP Support**: Language Server Protocol integration

## Next Steps

- Learn about [Configuration](configuration) options
- Explore [Workspace Management](workspace)
- See [File System Operations](filesystem)
- Check out [Git Operations](git)
- Understand [Process & Code Execution](process)
- Learn about [LSP Support](lsp)

## Common Issues

### Authentication Failed
- Verify your API key is correct
- Check if the server URL is accessible
- Ensure environment variables are properly set

### Workspace Creation Failed
- Check if you have sufficient permissions
- Verify the target environment is available
- Ensure network connectivity to the Daytona server

## Getting Help

If you encounter any issues:

1. Check the [Troubleshooting Guide](troubleshooting)
2. Review the [Security Guidelines](security)
3. Search existing GitHub issues
4. Join our community Discord server
5. Open a new GitHub issue with detailed information      