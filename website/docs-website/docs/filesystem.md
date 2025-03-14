---
id: filesystem
title: File System Operations
sidebar_position: 4
---

The Daytona SDK provides comprehensive file system operations through the `fs` module in workspaces. This guide covers all available file system operations and best practices.

## Basic Operations

### Listing Files and Directories

#### Python
```python
# List files in a directory
files = workspace.fs.list_files("/workspace")
for file in files:
    print(f"Name: {file.name}")
    print(f"Type: {file.type}")
    print(f"Size: {file.size}")
    print(f"Modified: {file.modified}")

# List files recursively
files = workspace.fs.list_files("/workspace", recursive=True)

# List with pattern matching
files = workspace.fs.list_files("/workspace", pattern="*.py")
```

#### TypeScript
```typescript
// List files in a directory
const files = await workspace.fs.listFiles("/workspace");
files.forEach(file => {
    console.log(`Name: ${file.name}`);
    console.log(`Type: ${file.type}`);
    console.log(`Size: ${file.size}`);
    console.log(`Modified: ${file.modified}`);
});

// List files recursively
const files = await workspace.fs.listFiles("/workspace", { recursive: true });

// List with pattern matching
const files = await workspace.fs.listFiles("/workspace", { pattern: "*.ts" });
```

### Creating Directories

#### Python
```python
# Create a directory
workspace.fs.create_folder("/workspace/new-dir")

# Create with specific permissions
workspace.fs.create_folder("/workspace/new-dir", "755")

# Create nested directories
workspace.fs.create_folder("/workspace/parent/child", create_parents=True)
```

#### TypeScript
```typescript
// Create a directory
await workspace.fs.createFolder("/workspace/new-dir");

// Create with specific permissions
await workspace.fs.createFolder("/workspace/new-dir", "755");

// Create nested directories
await workspace.fs.createFolder("/workspace/parent/child", { createParents: true });
```

### File Operations

#### Python
```python
# Read file content
content = workspace.fs.read_file("/workspace/file.txt")

# Write file content
workspace.fs.write_file("/workspace/file.txt", "Hello, World!")

# Upload a file
with open("local_file.txt", "rb") as f:
    content = f.read()
workspace.fs.upload_file("/workspace/remote_file.txt", content)

# Download a file
content = workspace.fs.download_file("/workspace/remote_file.txt")
with open("local_file.txt", "wb") as f:
    f.write(content)

# Delete a file
workspace.fs.delete_file("/workspace/file.txt")

# Delete a directory
workspace.fs.delete_folder("/workspace/dir", recursive=True)
```

#### TypeScript
```typescript
// Read file content
const content = await workspace.fs.readFile("/workspace/file.txt");

// Write file content
await workspace.fs.writeFile("/workspace/file.txt", "Hello, World!");

// Upload a file
const content = new Blob(["Hello, World!"], { type: "text/plain" });
await workspace.fs.uploadFile("/workspace/remote_file.txt", content);

// Download a file
const content = await workspace.fs.downloadFile("/workspace/remote_file.txt");
// Save content using browser's download API or File System API

// Delete a file
await workspace.fs.deleteFile("/workspace/file.txt");

// Delete a directory
await workspace.fs.deleteFolder("/workspace/dir", { recursive: true });
```

## Advanced Operations

### File Permissions

#### Python
```python
# Set file permissions
workspace.fs.set_file_permissions("/workspace/file.txt", "644")

# Set directory permissions recursively
workspace.fs.set_file_permissions("/workspace/dir", "755", recursive=True)

# Get file permissions
perms = workspace.fs.get_file_permissions("/workspace/file.txt")
print(f"Permissions: {perms.mode}")
```

#### TypeScript
```typescript
// Set file permissions
await workspace.fs.setFilePermissions("/workspace/file.txt", "644");

// Set directory permissions recursively
await workspace.fs.setFilePermissions("/workspace/dir", "755", { recursive: true });

// Get file permissions
const perms = await workspace.fs.getFilePermissions("/workspace/file.txt");
console.log(`Permissions: ${perms.mode}`);
```

### File Search and Replace

#### Python
```python
# Search for text in files
results = workspace.fs.search_in_files(
    pattern="TODO:",
    paths=["/workspace/src"],
    file_pattern="*.py"
)

# Replace text in files
workspace.fs.replace_in_files(
    files=["/workspace/file1.txt", "/workspace/file2.txt"],
    pattern="old_text",
    new_value="new_text"
)
```

#### TypeScript
```typescript
// Search for text in files
const results = await workspace.fs.searchInFiles({
    pattern: "TODO:",
    paths: ["/workspace/src"],
    filePattern: "*.ts"
});

// Replace text in files
await workspace.fs.replaceInFiles(
    ["/workspace/file1.txt", "/workspace/file2.txt"],
    "old_text",
    "new_text"
);
```

## Best Practices

1. **Error Handling**
   ```python
   # Python
   try:
       content = workspace.fs.read_file("/workspace/file.txt")
   except FileNotFoundError:
       print("File not found")
   except PermissionError:
       print("Permission denied")
   ```

   ```typescript
   // TypeScript
   try {
       const content = await workspace.fs.readFile("/workspace/file.txt");
   } catch (e) {
       if (e instanceof FileNotFoundError) {
           console.error("File not found");
       } else if (e instanceof PermissionError) {
           console.error("Permission denied");
       }
   }
   ```

2. **Path Handling**
   ```python
   # Python
   from pathlib import Path

   base_path = Path("/workspace")
   file_path = base_path / "src" / "main.py"
   workspace.fs.read_file(str(file_path))
   ```

   ```typescript
   // TypeScript
   import { join } from 'path';

   const basePath = "/workspace";
   const filePath = join(basePath, "src", "main.ts");
   await workspace.fs.readFile(filePath);
   ```

3. **Batch Operations**
   ```python
   # Python
   # Use batch operations for better performance
   workspace.fs.replace_in_files(
       files=workspace.fs.list_files("/workspace/src", pattern="*.py"),
       pattern="old_text",
       new_value="new_text"
   )
   ```

## Common Issues

### Permission Denied
- Check file permissions
- Verify workspace user permissions
- Ensure proper ownership

### File Not Found
- Verify file paths
- Check for case sensitivity
- Ensure parent directories exist

### Resource Limits
- Monitor disk usage
- Clean up temporary files
- Use appropriate file handling methods    