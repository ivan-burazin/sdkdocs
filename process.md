# Process & Code Execution

The Daytona SDK provides powerful process and code execution capabilities through the `process` module in workspaces. This guide covers all available process operations and best practices.

## Code Execution

### Running Code

#### Python
```python
# Run Python code
response = workspace.process.code_run('''
def greet(name):
    return f"Hello, {name}!"

print(greet("Daytona"))
''')
print(response.result)

# Run code with input
response = workspace.process.code_run(
    'name = input("Enter name: ")\nprint(f"Hello, {name}!")',
    input="Daytona"
)
print(response.result)

# Run code with timeout
response = workspace.process.code_run(
    'import time\ntime.sleep(2)\nprint("Done")',
    timeout=5
)
print(response.result)
```

#### TypeScript
```typescript
// Run TypeScript code
const response = await workspace.process.codeRun(`
function greet(name: string): string {
    return \`Hello, \${name}!\`;
}

console.log(greet("Daytona"));
`);
console.log(response.result);

// Run code with input
const response = await workspace.process.codeRun(
    'const name = prompt("Enter name: ");\nconsole.log(`Hello, ${name}!`);',
    { input: "Daytona" }
);
console.log(response.result);

// Run code with timeout
const response = await workspace.process.codeRun(
    'setTimeout(() => console.log("Done"), 2000);',
    { timeout: 5000 }
);
console.log(response.result);
```

### Code Execution Options

#### Python
```python
# Run code with environment variables
response = workspace.process.code_run(
    'import os\nprint(os.getenv("MY_VAR"))',
    env={"MY_VAR": "value"}
)

# Run code in specific working directory
response = workspace.process.code_run(
    'import os\nprint(os.getcwd())',
    cwd="/workspace/project"
)

# Run code with specific language
response = workspace.process.code_run(
    'console.log("Hello!");',
    language="javascript"
)
```

#### TypeScript
```typescript
// Run code with environment variables
const response = await workspace.process.codeRun(
    'console.log(process.env.MY_VAR);',
    { env: { MY_VAR: "value" } }
);

// Run code in specific working directory
const response = await workspace.process.codeRun(
    'console.log(process.cwd());',
    { cwd: "/workspace/project" }
);

// Run code with specific language
const response = await workspace.process.codeRun(
    'print("Hello!")',
    { language: "python" }
);
```

## Process Execution

### Running Commands

#### Python
```python
# Execute shell command
response = workspace.process.process_execute_command("ls -la")
print(response.output)

# Execute command with input
response = workspace.process.process_execute_command(
    "read -p 'Enter name: ' name && echo Hello, $name",
    input="Daytona"
)
print(response.output)

# Execute command with timeout
response = workspace.process.process_execute_command(
    "sleep 2 && echo Done",
    timeout=5
)
print(response.output)
```

#### TypeScript
```typescript
// Execute shell command
const response = await workspace.process.processExecuteCommand("ls -la");
console.log(response.output);

// Execute command with input
const response = await workspace.process.processExecuteCommand(
    "read -p 'Enter name: ' name && echo Hello, $name",
    { input: "Daytona" }
);
console.log(response.output);

// Execute command with timeout
const response = await workspace.process.processExecuteCommand(
    "sleep 2 && echo Done",
    { timeout: 5000 }
);
console.log(response.output);
```

### Process Options

#### Python
```python
# Execute command with environment variables
response = workspace.process.process_execute_command(
    "echo $MY_VAR",
    env={"MY_VAR": "value"}
)

# Execute command in specific working directory
response = workspace.process.process_execute_command(
    "pwd",
    cwd="/workspace/project"
)

# Execute command with shell
response = workspace.process.process_execute_command(
    "echo $SHELL && echo $PATH",
    shell=True
)
```

#### TypeScript
```typescript
// Execute command with environment variables
const response = await workspace.process.processExecuteCommand(
    "echo $MY_VAR",
    { env: { MY_VAR: "value" } }
);

// Execute command in specific working directory
const response = await workspace.process.processExecuteCommand(
    "pwd",
    { cwd: "/workspace/project" }
);

// Execute command with shell
const response = await workspace.process.processExecuteCommand(
    "echo $SHELL && echo $PATH",
    { shell: true }
);
```

## Background Processes

### Managing Long-Running Processes

#### Python
```python
# Start background process
process_id = workspace.process.start_background_process(
    "python -m http.server 8000"
)

# Check process status
status = workspace.process.get_process_status(process_id)
print(f"Process {process_id} status: {status}")

# Stop background process
workspace.process.stop_process(process_id)

# List all running processes
processes = workspace.process.list_processes()
for process in processes:
    print(f"PID: {process.id}, Command: {process.command}")
```

#### TypeScript
```typescript
// Start background process
const processId = await workspace.process.startBackgroundProcess(
    "python -m http.server 8000"
);

// Check process status
const status = await workspace.process.getProcessStatus(processId);
console.log(`Process ${processId} status: ${status}`);

// Stop background process
await workspace.process.stopProcess(processId);

// List all running processes
const processes = await workspace.process.listProcesses();
processes.forEach(process => {
    console.log(`PID: ${process.id}, Command: ${process.command}`);
});
```

## Best Practices

1. **Resource Management**
   ```python
   # Python - Clean up processes
   try:
       process_id = workspace.process.start_background_process("long-running-cmd")
       # Do work...
   finally:
       workspace.process.stop_process(process_id)
   ```

   ```typescript
   // TypeScript - Clean up processes
   try {
       const processId = await workspace.process.startBackgroundProcess("long-running-cmd");
       // Do work...
   } finally {
       await workspace.process.stopProcess(processId);
   }
   ```

2. **Error Handling**
   ```python
   # Python
   try:
       response = workspace.process.code_run("invalid python code")
   except ProcessExecutionError as e:
       print(f"Execution failed: {e}")
       print(f"Exit code: {e.exit_code}")
       print(f"Error output: {e.stderr}")
   ```

   ```typescript
   // TypeScript
   try {
       const response = await workspace.process.codeRun("invalid typescript code");
   } catch (e) {
       if (e instanceof ProcessExecutionError) {
           console.error("Execution failed:", e);
           console.error("Exit code:", e.exitCode);
           console.error("Error output:", e.stderr);
       }
   }
   ```

## Common Issues

### Process Execution Failed
- Check command syntax
- Verify required dependencies
- Ensure sufficient permissions

### Process Timeout
- Adjust timeout settings
- Optimize long-running operations
- Consider using background processes

### Resource Limits
- Monitor process memory usage
- Handle process cleanup properly
- Use appropriate resource constraints 