---
id: troubleshooting
title: Troubleshooting Guide
sidebar_position: 9
---

This guide helps you diagnose and resolve common issues with the Daytona SDK.

## Diagnostic Tools

### Health Check

#### Python
```python
from daytona_sdk import Daytona

daytona = Daytona()
try:
    # Check server connection
    workspace = daytona.create()
    print("✅ Server connection successful")
    
    # Check workspace operations
    workspace.process.process_execute_command("echo 'test'")
    print("✅ Workspace operations working")
    
    # Cleanup
    daytona.remove(workspace)
except Exception as e:
    print(f"❌ Health check failed: {e}")
```

#### TypeScript
```typescript
import { Daytona } from '@daytona/sdk'

async function healthCheck() {
    const daytona = new Daytona()
    try {
        // Check server connection
        const workspace = await daytona.create()
        console.log("✅ Server connection successful")
        
        // Check workspace operations
        await workspace.process.processExecuteCommand("echo 'test'")
        console.log("✅ Workspace operations working")
        
        // Cleanup
        await daytona.remove(workspace)
    } catch (e) {
        console.error("❌ Health check failed:", e)
    }
}
```

## Common Issues

### Connection Issues

#### Problem: Unable to Connect to Server
```
Error: Failed to connect to server at https://your-server-url
```

**Solutions:**
1. Check server URL
   ```bash
   curl -v https://your-server-url/health
   ```
2. Verify API key
   ```python
   # Python
   import requests
   headers = {"Authorization": f"Bearer {api_key}"}
   response = requests.get("https://your-server-url/health", headers=headers)
   ```
   ```typescript
   // TypeScript
   const response = await fetch("https://your-server-url/health", {
       headers: { Authorization: `Bearer ${apiKey}` }
   })
   ```
3. Check network configuration
   - Verify firewall settings
   - Check proxy configuration
   - Ensure DNS resolution works

### Workspace Issues

#### Problem: Workspace Creation Fails
```
Error: Failed to create workspace
```

**Solutions:**
1. Check resource availability
   ```python
   # Python
   workspace = daytona.create(CreateWorkspaceParams(
       language="python",
       id="test-workspace",
       image="python:3.9-slim"  # Use minimal image
   ))
   ```
   ```typescript
   // TypeScript
   const workspace = await daytona.create({
       language: 'typescript',
       id: 'test-workspace',
       image: 'node:16-slim'  // Use minimal image
   })
   ```

2. Verify target environment
   ```python
   # Python - Try different target
   config = DaytonaConfig(target="docker")
   daytona = Daytona(config)
   ```
   ```typescript
   // TypeScript - Try different target
   const config: DaytonaConfig = { target: "docker" }
   const daytona = new Daytona(config)
   ```

### Process Execution Issues

#### Problem: Command Execution Timeout
```
Error: Command execution timed out after 30 seconds
```

**Solutions:**
1. Increase timeout
   ```python
   # Python
   workspace.process.process_execute_command(
       "long-running-command",
       timeout=120  # Increase timeout to 120 seconds
   )
   ```
   ```typescript
   // TypeScript
   await workspace.process.processExecuteCommand(
       "long-running-command",
       { timeout: 120000 }  // 120 seconds in milliseconds
   )
   ```

2. Use background execution
   ```python
   # Python
   workspace.process.process_execute_command("long-running-command &")
   ```
   ```typescript
   // TypeScript
   await workspace.process.processExecuteCommand("long-running-command &")
   ```

### File System Issues

#### Problem: Permission Denied
```
Error: Permission denied when accessing /path/to/file
```

**Solutions:**
1. Check file permissions
   ```python
   # Python
   workspace.fs.set_file_permissions(
       path="/path/to/file",
       permissions={"mode": "644"}
   )
   ```
   ```typescript
   // TypeScript
   await workspace.fs.setFilePermissions(
       "/path/to/file",
       { mode: "644" }
   )
   ```

2. Use sudo when necessary
   ```python
   # Python
   workspace.process.process_execute_command("sudo chmod 644 /path/to/file")
   ```
   ```typescript
   // TypeScript
   await workspace.process.processExecuteCommand("sudo chmod 644 /path/to/file")
   ```

### Git Issues

#### Problem: Authentication Failed
```
Error: Authentication failed for repository
```

**Solutions:**
1. Use personal access token
   ```python
   # Python
   workspace.git.clone(
       url="https://github.com/user/repo.git",
       path="/workspace/repo",
       username="git",
       password=os.getenv("GITHUB_TOKEN")
   )
   ```
   ```typescript
   // TypeScript
   await workspace.git.clone(
       "https://github.com/user/repo.git",
       "/workspace/repo",
       undefined,
       undefined,
       "git",
       process.env.GITHUB_TOKEN
   )
   ```

2. Check token permissions
   ```bash
   # Verify token has required scopes
   curl -H "Authorization: token YOUR_TOKEN" https://api.github.com/user
   ```

## Performance Issues

### Problem: Slow Operations

**Solutions:**
1. Use appropriate batch operations
   ```python
   # Python - Instead of multiple single operations
   workspace.fs.replace_in_files(
       files=["/path/file1.txt", "/path/file2.txt"],
       pattern="old_text",
       new_value="new_text"
   )
   ```
   ```typescript
   // TypeScript - Instead of multiple single operations
   await workspace.fs.replaceInFiles(
       ["/path/file1.txt", "/path/file2.txt"],
       "old_text",
       "new_text"
   )
   ```

2. Optimize resource usage
   ```python
   # Python - Use minimal image
   workspace = daytona.create(CreateWorkspaceParams(
       language="python",
       image="python:3.9-alpine"
   ))
   ```
   ```typescript
   // TypeScript - Use minimal image
   const workspace = await daytona.create({
       language: 'typescript',
       image: 'node:16-alpine'
   })
   ```

## Debugging

### Enable Debug Logging

#### Python
```python
import logging

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger("daytona_sdk")
logger.setLevel(logging.DEBUG)
```

#### TypeScript
```typescript
import { setLogLevel } from '@daytona/sdk'

setLogLevel('debug')
```

### Collect Debug Information

```python
# Python
def collect_debug_info(workspace):
    debug_info = {
        "workspace_id": workspace.id,
        "root_dir": workspace.get_workspace_root_dir(),
        "git_status": workspace.git.status("/workspace"),
        "process_list": workspace.process.process_execute_command("ps aux").output
    }
    return debug_info
```

```typescript
// TypeScript
async function collectDebugInfo(workspace) {
    const debugInfo = {
        workspace_id: workspace.id,
        root_dir: await workspace.getWorkspaceRootDir(),
        git_status: await workspace.git.status("/workspace"),
        process_list: (await workspace.process.processExecuteCommand("ps aux")).output
    }
    return debugInfo
}
```

## Getting Help

1. Check the documentation
2. Search existing GitHub issues
3. Enable debug logging
4. Collect relevant error messages and logs
5. Create a minimal reproduction case
6. Open a new GitHub issue with detailed information    