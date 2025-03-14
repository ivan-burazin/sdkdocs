---
id: workspace
title: Workspace Management
sidebar_position: 3
---

Workspaces are isolated development environments managed by Daytona. This guide covers how to create, manage, and remove workspaces using the SDK.

## Creating Workspaces

### Basic Workspace Creation

#### Python
```python
from daytona_sdk import Daytona

daytona = Daytona()

# Create a basic workspace
workspace = daytona.create()

# Create a workspace with specific language
workspace = daytona.create(language="python")

# Create a workspace with custom ID
workspace = daytona.create(id="my-workspace")
```

#### TypeScript
```typescript
import { Daytona } from '@daytona/sdk';

const daytona = new Daytona();

// Create a basic workspace
const workspace = await daytona.create();

// Create a workspace with specific language
const workspace = await daytona.create({ language: 'typescript' });

// Create a workspace with custom ID
const workspace = await daytona.create({ id: 'my-workspace' });
```

### Advanced Workspace Creation

#### Python
```python
from daytona_sdk import Daytona, CreateWorkspaceParams

# Create workspace with advanced options
workspace = daytona.create(
    CreateWorkspaceParams(
        language="python",
        id="custom-workspace",
        image="python:3.9-slim",
        env={
            "DEBUG": "true",
            "API_KEY": "secret"
        },
        volumes=[
            {
                "source": "/local/path",
                "target": "/workspace/data"
            }
        ],
        resources={
            "cpu": "2",
            "memory": "4Gi"
        }
    )
)
```

#### TypeScript
```typescript
import { Daytona, CreateWorkspaceParams } from '@daytona/sdk';

// Create workspace with advanced options
const workspace = await daytona.create({
    language: 'typescript',
    id: 'custom-workspace',
    image: 'node:16-slim',
    env: {
        DEBUG: 'true',
        API_KEY: 'secret'
    },
    volumes: [
        {
            source: '/local/path',
            target: '/workspace/data'
        }
    ],
    resources: {
        cpu: '2',
        memory: '4Gi'
    }
});
```

## Managing Workspaces

### Workspace Information

#### Python
```python
# Get workspace ID
workspace_id = workspace.id

# Get workspace root directory
root_dir = workspace.get_workspace_root_dir()

# Get workspace status
status = workspace.status()
print(f"Status: {status.state}")
```

#### TypeScript
```typescript
// Get workspace ID
const workspaceId = workspace.id;

// Get workspace root directory
const rootDir = await workspace.getWorkspaceRootDir();

// Get workspace status
const status = await workspace.status();
console.log(`Status: ${status.state}`);
```

### Workspace Operations

#### Python
```python
# Stop workspace
workspace.stop()

# Start workspace
workspace.start()

# Restart workspace
workspace.restart()

# Remove workspace
daytona.remove(workspace)
```

#### TypeScript
```typescript
// Stop workspace
await workspace.stop();

// Start workspace
await workspace.start();

// Restart workspace
await workspace.restart();

// Remove workspace
await daytona.remove(workspace);
```

## Workspace Environment Variables

### Setting Environment Variables

#### Python
```python
# Set environment variables during creation
workspace = daytona.create(
    env={
        "DEBUG": "true",
        "API_KEY": "secret"
    }
)

# Update environment variables
workspace.set_env({
    "NEW_VAR": "value"
})
```

#### TypeScript
```typescript
// Set environment variables during creation
const workspace = await daytona.create({
    env: {
        DEBUG: 'true',
        API_KEY: 'secret'
    }
});

// Update environment variables
await workspace.setEnv({
    NEW_VAR: 'value'
});
```

## Workspace Resources

### Resource Configuration

```python
# Python
workspace = daytona.create(
    resources={
        "cpu": "2",
        "memory": "4Gi",
        "gpu": "1"
    }
)
```

```typescript
// TypeScript
const workspace = await daytona.create({
    resources: {
        cpu: '2',
        memory: '4Gi',
        gpu: '1'
    }
});
```

## Workspace Volumes

### Volume Configuration

```python
# Python
workspace = daytona.create(
    volumes=[
        {
            "source": "/local/data",
            "target": "/workspace/data",
            "type": "bind"
        },
        {
            "name": "cache",
            "target": "/workspace/cache",
            "type": "volume"
        }
    ]
)
```

```typescript
// TypeScript
const workspace = await daytona.create({
    volumes: [
        {
            source: '/local/data',
            target: '/workspace/data',
            type: 'bind'
        },
        {
            name: 'cache',
            target: '/workspace/cache',
            type: 'volume'
        }
    ]
});
```

## Best Practices

1. **Resource Management**
   - Always clean up workspaces when done
   - Use appropriate resource limits
   - Monitor workspace status

2. **Environment Variables**
   - Use secure methods for sensitive data
   - Document required environment variables
   - Validate environment variable values

3. **Error Handling**
   ```python
   # Python
   try:
       workspace = daytona.create()
   except WorkspaceCreationError as e:
       print(f"Failed to create workspace: {e}")
   finally:
       if workspace:
           daytona.remove(workspace)
   ```

   ```typescript
   // TypeScript
   try {
       const workspace = await daytona.create();
   } catch (e) {
       console.error("Failed to create workspace:", e);
   } finally {
       if (workspace) {
           await daytona.remove(workspace);
       }
   }
   ```

## Common Issues

### Workspace Creation Failed
- Check resource availability
- Verify permissions
- Ensure valid configuration

### Workspace Not Responding
- Check workspace status
- Verify network connectivity
- Review resource usage

### Resource Limits Exceeded
- Monitor resource usage
- Adjust resource limits
- Clean up unused workspaces    