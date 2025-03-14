---
id: lsp
title: Language Server Protocol (LSP) Support
sidebar_position: 7
---

The Daytona SDK provides Language Server Protocol (LSP) support through workspace instances. This enables advanced language features like code completion, diagnostics, and more.

## Creating LSP Servers

### Python
```python
from daytona_sdk import Daytona, LspLanguageId

# Create workspace
daytona = Daytona()
workspace = daytona.create()

# Create LSP server for Python
lsp_server = workspace.create_lsp_server(
    language_id=LspLanguageId.PYTHON,
    path_to_project="/workspace/project"
)
```

### TypeScript
```typescript
import { Daytona, LspLanguageId } from '@daytona/sdk'

// Create workspace
const daytona = new Daytona()
const workspace = await daytona.create({
    language: 'typescript'
})

// Create LSP server for TypeScript
const lspServer = workspace.createLspServer(
    LspLanguageId.TYPESCRIPT,
    "/workspace/project"
)
```

## Supported Languages

The SDK supports LSP for various languages through the `LspLanguageId` enum:

### Python
```python
from daytona_sdk import LspLanguageId

# Available language IDs
LspLanguageId.PYTHON      # Python language server
LspLanguageId.TYPESCRIPT  # TypeScript/JavaScript language server
LspLanguageId.GO          # Go language server
LspLanguageId.JAVA        # Java language server
LspLanguageId.RUST        # Rust language server
```

### TypeScript
```typescript
import { LspLanguageId } from '@daytona/sdk'

// Available language IDs
LspLanguageId.PYTHON      // Python language server
LspLanguageId.TYPESCRIPT  // TypeScript/JavaScript language server
LspLanguageId.GO          // Go language server
LspLanguageId.JAVA        // Java language server
LspLanguageId.RUST        // Rust language server
```

## LSP Features

### Code Completion

Get code completion suggestions:

#### Python
```python
completions = lsp_server.get_completions(
    file_path="/workspace/project/main.py",
    position={"line": 10, "character": 15}
)
for completion in completions:
    print(f"Label: {completion.label}")
    print(f"Kind: {completion.kind}")
    print(f"Detail: {completion.detail}")
```

#### TypeScript
```typescript
const completions = await lspServer.getCompletions({
    filePath: "/workspace/project/main.ts",
    position: { line: 10, character: 15 }
})
completions.forEach(completion => {
    console.log(`Label: ${completion.label}`)
    console.log(`Kind: ${completion.kind}`)
    console.log(`Detail: ${completion.detail}`)
})
```

### Diagnostics

Get diagnostic information:

#### Python
```python
diagnostics = lsp_server.get_diagnostics(
    file_path="/workspace/project/main.py"
)
for diagnostic in diagnostics:
    print(f"Message: {diagnostic.message}")
    print(f"Severity: {diagnostic.severity}")
    print(f"Range: {diagnostic.range}")
```

#### TypeScript
```typescript
const diagnostics = await lspServer.getDiagnostics({
    filePath: "/workspace/project/main.ts"
})
diagnostics.forEach(diagnostic => {
    console.log(`Message: ${diagnostic.message}`)
    console.log(`Severity: ${diagnostic.severity}`)
    console.log(`Range: ${diagnostic.range}`)
})
```

### Go to Definition

Navigate to symbol definitions:

#### Python
```python
definition = lsp_server.get_definition(
    file_path="/workspace/project/main.py",
    position={"line": 15, "character": 20}
)
print(f"Definition location: {definition.uri}:{definition.range}")
```

#### TypeScript
```typescript
const definition = await lspServer.getDefinition({
    filePath: "/workspace/project/main.ts",
    position: { line: 15, character: 20 }
})
console.log(`Definition location: ${definition.uri}:${definition.range}`)
```

### Hover Information

Get hover information for symbols:

#### Python
```python
hover = lsp_server.get_hover(
    file_path="/workspace/project/main.py",
    position={"line": 12, "character": 25}
)
print(f"Hover content: {hover.contents}")
```

#### TypeScript
```typescript
const hover = await lspServer.getHover({
    filePath: "/workspace/project/main.ts",
    position: { line: 12, character: 25 }
})
console.log(`Hover content: ${hover.contents}`)
```

## Best Practices

1. **Server Lifecycle**
   ```python
   # Python
   try:
       lsp_server = workspace.create_lsp_server(
           LspLanguageId.PYTHON,
           "/workspace/project"
       )
       # Use LSP server...
   finally:
       lsp_server.dispose()  # Clean up server
   ```
   ```typescript
   // TypeScript
   try {
       const lspServer = workspace.createLspServer(
           LspLanguageId.TYPESCRIPT,
           "/workspace/project"
       )
       // Use LSP server...
   } finally {
       await lspServer.dispose()  // Clean up server
   }
   ```

2. **Error Handling**
   ```python
   # Python
   try:
       completions = lsp_server.get_completions(file_path, position)
   except Exception as e:
       print(f"LSP error: {e}")
   ```
   ```typescript
   // TypeScript
   try {
       const completions = await lspServer.getCompletions(params)
   } catch (e) {
       console.error("LSP error:", e)
   }
   ```

3. **Performance**
   - Cache LSP results when appropriate
   - Batch LSP requests when possible
   - Dispose of unused servers

## Common Issues

### Server Initialization
- Ensure project path exists
- Check language server installation
- Verify file permissions

### Request Timeouts
- Check network connectivity
- Verify server health
- Consider request complexity

### Memory Usage
- Monitor server memory
- Dispose unused servers
- Limit concurrent servers

## Advanced Usage

### Custom Language Servers

Configure custom language servers:

#### Python
```python
lsp_server = workspace.create_lsp_server(
    language_id=LspLanguageId.CUSTOM,
    path_to_project="/workspace/project",
    server_path="/path/to/custom/server",
    server_args=["--custom-option"]
)
```

#### TypeScript
```typescript
const lspServer = workspace.createLspServer(
    LspLanguageId.CUSTOM,
    "/workspace/project",
    {
        serverPath: "/path/to/custom/server",
        serverArgs: ["--custom-option"]
    }
)
```

### Server Configuration

Set language server configuration:

#### Python
```python
lsp_server.configure({
    "python.analysis.typeCheckingMode": "basic",
    "python.formatting.provider": "black"
})
```

#### TypeScript
```typescript
await lspServer.configure({
    "typescript.suggest.completeFunctionCalls": true,
    "typescript.format.insertSpaceAfterFunctionKeywordForAnonymousFunctions": true
})
```

## Security Considerations

1. **File Access**
   - Restrict server file access
   - Validate file paths
   - Monitor file operations

2. **Resource Usage**
   - Set memory limits
   - Monitor CPU usage
   - Implement request throttling

3. **Server Security**
   - Use secure connections
   - Validate server binaries
   - Update servers regularly   