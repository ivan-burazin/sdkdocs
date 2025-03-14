# Git Operations

The Daytona SDK provides built-in Git support through the `git` module in workspaces. This guide covers all available Git operations and best practices.

## Basic Operations

### Cloning Repositories

#### Python
```python
# Basic clone
workspace.git.clone(
    url="https://github.com/user/repo.git",
    path="/workspace/repo"
)

# Clone with authentication
workspace.git.clone(
    url="https://github.com/user/repo.git",
    path="/workspace/repo",
    username="git",
    password="personal_access_token"
)

# Clone specific branch
workspace.git.clone(
    url="https://github.com/user/repo.git",
    path="/workspace/repo",
    branch="develop"
)
```

#### TypeScript
```typescript
// Basic clone
await workspace.git.clone(
    "https://github.com/user/repo.git",
    "/workspace/repo"
);

// Clone with authentication
await workspace.git.clone(
    "https://github.com/user/repo.git",
    "/workspace/repo",
    undefined,
    undefined,
    "git",
    "personal_access_token"
);

// Clone specific branch
await workspace.git.clone(
    "https://github.com/user/repo.git",
    "/workspace/repo",
    "develop"
);
```

### Repository Status

#### Python
```python
# Get repository status
status = workspace.git.status("/workspace/repo")
print(f"Current branch: {status.current_branch}")
print(f"Modified files: {status.modified}")
print(f"Staged files: {status.staged}")
print(f"Untracked files: {status.untracked}")

# Get current branch
branch = workspace.git.current_branch("/workspace/repo")
print(f"Current branch: {branch}")

# List branches
branches = workspace.git.list_branches("/workspace/repo")
for branch in branches:
    print(f"Branch: {branch.name} {'(current)' if branch.current else ''}")
```

#### TypeScript
```typescript
// Get repository status
const status = await workspace.git.status("/workspace/repo");
console.log(`Current branch: ${status.currentBranch}`);
console.log(`Modified files: ${status.modified}`);
console.log(`Staged files: ${status.staged}`);
console.log(`Untracked files: ${status.untracked}`);

// Get current branch
const branch = await workspace.git.currentBranch("/workspace/repo");
console.log(`Current branch: ${branch}`);

// List branches
const branches = await workspace.git.listBranches("/workspace/repo");
branches.forEach(branch => {
    console.log(`Branch: ${branch.name} ${branch.current ? '(current)' : ''}`);
});
```

## Branch Operations

### Managing Branches

#### Python
```python
# Create new branch
workspace.git.create_branch("/workspace/repo", "feature/new-feature")

# Switch branch
workspace.git.checkout("/workspace/repo", "feature/new-feature")

# Create and switch to new branch
workspace.git.checkout("/workspace/repo", "feature/new-feature", create=True)

# Delete branch
workspace.git.delete_branch("/workspace/repo", "feature/old-feature")
```

#### TypeScript
```typescript
// Create new branch
await workspace.git.createBranch("/workspace/repo", "feature/new-feature");

// Switch branch
await workspace.git.checkout("/workspace/repo", "feature/new-feature");

// Create and switch to new branch
await workspace.git.checkout("/workspace/repo", "feature/new-feature", { create: true });

// Delete branch
await workspace.git.deleteBranch("/workspace/repo", "feature/old-feature");
```

## Staging and Committing

### Working with Changes

#### Python
```python
# Stage specific files
workspace.git.add("/workspace/repo", ["file1.txt", "file2.txt"])

# Stage all changes
workspace.git.add("/workspace/repo", ["."])

# Commit changes
workspace.git.commit("/workspace/repo", "feat: add new feature")

# Stage and commit in one step
workspace.git.commit("/workspace/repo", "feat: add new feature", add=True)

# Get commit history
commits = workspace.git.log("/workspace/repo")
for commit in commits:
    print(f"Commit: {commit.hash}")
    print(f"Author: {commit.author}")
    print(f"Message: {commit.message}")
```

#### TypeScript
```typescript
// Stage specific files
await workspace.git.add("/workspace/repo", ["file1.txt", "file2.txt"]);

// Stage all changes
await workspace.git.add("/workspace/repo", ["."]);

// Commit changes
await workspace.git.commit("/workspace/repo", "feat: add new feature");

// Stage and commit in one step
await workspace.git.commit("/workspace/repo", "feat: add new feature", { add: true });

// Get commit history
const commits = await workspace.git.log("/workspace/repo");
commits.forEach(commit => {
    console.log(`Commit: ${commit.hash}`);
    console.log(`Author: ${commit.author}`);
    console.log(`Message: ${commit.message}`);
});
```

## Remote Operations

### Working with Remotes

#### Python
```python
# Push changes
workspace.git.push("/workspace/repo")

# Push to specific remote and branch
workspace.git.push("/workspace/repo", remote="origin", branch="feature/new-feature")

# Pull changes
workspace.git.pull("/workspace/repo")

# Pull from specific remote and branch
workspace.git.pull("/workspace/repo", remote="origin", branch="main")

# List remotes
remotes = workspace.git.list_remotes("/workspace/repo")
for remote in remotes:
    print(f"Remote: {remote.name} URL: {remote.url}")
```

#### TypeScript
```typescript
// Push changes
await workspace.git.push("/workspace/repo");

// Push to specific remote and branch
await workspace.git.push("/workspace/repo", "origin", "feature/new-feature");

// Pull changes
await workspace.git.pull("/workspace/repo");

// Pull from specific remote and branch
await workspace.git.pull("/workspace/repo", "origin", "main");

// List remotes
const remotes = await workspace.git.listRemotes("/workspace/repo");
remotes.forEach(remote => {
    console.log(`Remote: ${remote.name} URL: ${remote.url}`);
});
```

## Best Practices

1. **Authentication**
   ```python
   # Python - Use environment variables for credentials
   workspace.git.clone(
       url="https://github.com/user/repo.git",
       path="/workspace/repo",
       username=os.getenv("GIT_USERNAME"),
       password=os.getenv("GIT_TOKEN")
   )
   ```

   ```typescript
   // TypeScript - Use environment variables for credentials
   await workspace.git.clone(
       "https://github.com/user/repo.git",
       "/workspace/repo",
       undefined,
       undefined,
       process.env.GIT_USERNAME,
       process.env.GIT_TOKEN
   );
   ```

2. **Error Handling**
   ```python
   # Python
   try:
       workspace.git.push("/workspace/repo")
   except GitError as e:
       if "rejected" in str(e):
           workspace.git.pull("/workspace/repo")
           workspace.git.push("/workspace/repo")
   ```

   ```typescript
   // TypeScript
   try {
       await workspace.git.push("/workspace/repo");
   } catch (e) {
       if (e instanceof GitError && e.message.includes("rejected")) {
           await workspace.git.pull("/workspace/repo");
           await workspace.git.push("/workspace/repo");
       }
   }
   ```

## Common Issues

### Authentication Failed
- Verify credentials are correct
- Check token permissions
- Ensure remote URL is accessible

### Merge Conflicts
- Pull latest changes before pushing
- Resolve conflicts manually
- Use appropriate merge strategy

### Repository Issues
- Check repository permissions
- Verify network connectivity
- Ensure workspace has sufficient storage