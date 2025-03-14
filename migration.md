# Migration Guide

This guide helps you migrate between different versions of the Daytona SDK.

## Migrating to v1.0

### Breaking Changes

1. **Configuration Changes**

   #### Before (v0.x)
   ```python
   # Python
   daytona = Daytona(
       api_key="your-key",
       server_url="your-url"
   )
   ```
   ```typescript
   // TypeScript
   const daytona = new Daytona({
       apiKey: "your-key",
       serverUrl: "your-url"
   })
   ```

   #### After (v1.0)
   ```python
   # Python
   config = DaytonaConfig(
       api_key="your-key",
       server_url="your-url"
   )
   daytona = Daytona(config)
   ```
   ```typescript
   // TypeScript
   const config: DaytonaConfig = {
       apiKey: "your-key",
       serverUrl: "your-url"
   }
   const daytona = new Daytona(config)
   ```

2. **Workspace Creation**

   #### Before (v0.x)
   ```python
   # Python
   workspace = daytona.create_workspace(
       language="python",
       image="python:3.9"
   )
   ```
   ```typescript
   // TypeScript
   const workspace = await daytona.createWorkspace({
       language: "typescript",
       image: "node:16"
   })
   ```

   #### After (v1.0)
   ```python
   # Python
   workspace = daytona.create(CreateWorkspaceParams(
       language="python",
       image="python:3.9"
   ))
   ```
   ```typescript
   // TypeScript
   const workspace = await daytona.create({
       language: "typescript",
       image: "node:16"
   })
   ```

3. **Process Execution**

   #### Before (v0.x)
   ```python
   # Python
   result = workspace.execute_command("ls -la")
   ```
   ```typescript
   // TypeScript
   const result = await workspace.executeCommand("ls -la")
   ```

   #### After (v1.0)
   ```python
   # Python
   result = workspace.process.process_execute_command("ls -la")
   ```
   ```typescript
   // TypeScript
   const result = await workspace.process.processExecuteCommand("ls -la")
   ```

### Deprecated Features

The following features are deprecated in v1.0:

1. **Old Configuration Format**
   ```python
   # Python - Deprecated
   daytona = Daytona(api_key="key")  # Use DaytonaConfig instead
   ```
   ```typescript
   // TypeScript - Deprecated
   const daytona = new Daytona({ apiKey: "key" })  // Use DaytonaConfig instead
   ```

2. **Direct Command Execution**
   ```python
   # Python - Deprecated
   workspace.execute_command()  # Use workspace.process.process_execute_command()
   ```
   ```typescript
   // TypeScript - Deprecated
   workspace.executeCommand()  // Use workspace.process.processExecuteCommand()
   ```

### New Features in v1.0

1. **Resource Limits**
   ```python
   # Python
   workspace = daytona.create(CreateWorkspaceParams(
       language="python",
       resource_limits={
           "memory": "512Mi",
           "cpu": "1"
       }
   ))
   ```
   ```typescript
   // TypeScript
   const workspace = await daytona.create({
       language: "typescript",
       resourceLimits: {
           memory: "512Mi",
           cpu: "1"
       }
   })
   ```

2. **Enhanced LSP Support**
   ```python
   # Python
   lsp = workspace.create_lsp_server(
       language_id=LspLanguageId.PYTHON,
       path_to_project="/workspace/project",
       configuration={
           "python.analysis.typeCheckingMode": "basic"
       }
   )
   ```
   ```typescript
   // TypeScript
   const lsp = workspace.createLspServer(
       LspLanguageId.TYPESCRIPT,
       "/workspace/project",
       {
           configuration: {
               "typescript.suggest.completeFunctionCalls": true
           }
       }
   )
   ```

## Migration Steps

1. **Update Dependencies**
   ```bash
   # Python
   pip install --upgrade daytona-sdk>=1.0.0
   
   # TypeScript
   npm install @daytona/sdk@^1.0.0
   # or
   yarn upgrade @daytona/sdk@^1.0.0
   ```

2. **Update Imports**
   ```python
   # Python
   from daytona_sdk import Daytona, DaytonaConfig, CreateWorkspaceParams
   ```
   ```typescript
   // TypeScript
   import { Daytona, DaytonaConfig, CreateWorkspaceParams } from '@daytona/sdk'
   ```

3. **Update Configuration**
   ```python
   # Python
   config = DaytonaConfig(
       api_key=os.getenv("DAYTONA_API_KEY"),
       server_url=os.getenv("DAYTONA_SERVER_URL")
   )
   daytona = Daytona(config)
   ```
   ```typescript
   // TypeScript
   const config: DaytonaConfig = {
       apiKey: process.env.DAYTONA_API_KEY,
       serverUrl: process.env.DAYTONA_SERVER_URL
   }
   const daytona = new Daytona(config)
   ```

4. **Update Method Calls**
   ```python
   # Python
   # Old
   workspace = daytona.create_workspace()
   result = workspace.execute_command("ls")
   
   # New
   workspace = daytona.create()
   result = workspace.process.process_execute_command("ls")
   ```
   ```typescript
   // TypeScript
   // Old
   const workspace = await daytona.createWorkspace()
   const result = await workspace.executeCommand("ls")
   
   // New
   const workspace = await daytona.create()
   const result = await workspace.process.processExecuteCommand("ls")
   ```

## Troubleshooting Migration Issues

### Common Issues

1. **Configuration Errors**
   ```python
   # Python
   try:
       config = DaytonaConfig(api_key="key")
       daytona = Daytona(config)
   except ValueError as e:
       print(f"Configuration error: {e}")
   ```
   ```typescript
   // TypeScript
   try {
       const config: DaytonaConfig = { apiKey: "key" }
       const daytona = new Daytona(config)
   } catch (e) {
       console.error("Configuration error:", e)
   }
   ```

2. **Method Not Found**
   ```python
   # Python - If you see AttributeError
   # Old: workspace.execute_command()
   # New: workspace.process.process_execute_command()
   ```
   ```typescript
   // TypeScript - If you see Property does not exist
   // Old: workspace.executeCommand()
   // New: workspace.process.processExecuteCommand()
   ```

### Migration Checklist

1. **Pre-Migration**
   - [ ] Backup existing code
   - [ ] Document current configuration
   - [ ] List all SDK method calls

2. **During Migration**
   - [ ] Update dependencies
   - [ ] Update configuration format
   - [ ] Update method calls
   - [ ] Test each component

3. **Post-Migration**
   - [ ] Remove deprecated code
   - [ ] Update documentation
   - [ ] Test full workflow
   - [ ] Monitor for issues

## Getting Help

If you encounter issues during migration:

1. Check the [troubleshooting guide](troubleshooting.md)
2. Search existing GitHub issues
3. Open a new issue with:
   - SDK versions (old and new)
   - Migration step where issue occurred
   - Error messages
   - Minimal reproduction code 