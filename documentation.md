# **Stacked PR Command Line Tool Documentation**

---

## **Overview**

The `stkd` CLI tool is designed to simplify the workflow of managing stacked pull requests. By automating common tasks such as creating, syncing, pushing, and converting branches into stacks, this tool helps developers focus on delivering high-quality code without worrying about the complexities of maintaining stack hierarchies.

---

## **Installation**

Install the `stkd` CLI tool using one of the following methods:

### **Using npm**
```bash
npm install -g stkd-cli
```

### **Using pip**
```bash
pip install stkd-cli
```

### **Verifying Installation**
To verify that the tool is installed, run:
```bash
stkd --version
```
This should output the installed version of stkd.

---

## **Commands**

### **1. `stkd new`**  
Creates a new stack branch based on the current branch.

#### **Usage**  
```bash  
stkd new  
```

#### **How it Works**  
- Prompts the user for a name for the new stack branch.  
- Creates a new branch and sets it as a dependent stack of the current branch.  
- Tracks the stack relationship in a `.stkd` metadata file stored in the repository.  

#### **Example Workflow**  
```bash  
git checkout feature/add-login  
stkd new  
# Prompt: Enter the name for the new stack: feature/add-logout  
# Result: Creates a new branch `feature/add-logout` stacked on `feature/add-login`.  
```

---

### **2. `stkd sync [--push]`**  
Rebases all stack branches from their respective parent branches.

#### **Usage**  
```bash  
stkd sync  
```

#### **Options**  
- `--push`: Rebases all stack branches and automatically pushes the updated branches to the remote repository.

#### **How it Works**  
- Traverses the stack hierarchy defined in the `.stkd` metadata file.  
- Rebases each branch onto its parent branch in the correct order.  
- Handles potential conflicts, allowing the user to resolve them before continuing.

#### **Example Workflow**  
```bash  
# Rebase all stacks locally:  
stkd sync  

# Rebase and push all stacks:  
stkd sync --push  
```

#### **Output Example**  
```plaintext  
Rebasing feature/add-logout onto feature/add-login...  
Rebasing feature/add-settings onto feature/add-logout...  
Sync complete!  
```

---

### **3. `stkd push`**  
Pushes all stack branches to the remote repository.

#### **Usage**  
```bash  
stkd push  
```

#### **How it Works**  
- Iterates through all stack branches and pushes each one to the configured remote repository (e.g., `origin`).  
- Automatically creates remote branches if they don’t exist.

#### **Example Workflow**  
```bash  
stkd push  
# Output: Pushing feature/add-login, feature/add-logout, and feature/add-settings to origin.  
```

---

### **4. `stkd pr`**  
Creates pull requests for all stack branches.

#### **Usage**  
```bash  
stkd pr  
```

#### **How it Works**  
- Uses your GitHub credentials (or SSH key) to create pull requests for each stack branch.  
- Ensures that each PR has the correct base branch based on the stack hierarchy.  
- Links PRs in a chain so reviewers can easily navigate the stack.

#### **Example Workflow**  
```bash  
stkd pr  
# Output:  
# Creating PR for feature/add-login...  
# Creating PR for feature/add-logout with base feature/add-login...  
# Creating PR for feature/add-settings with base feature/add-logout...  
# All PRs created successfully!  
```

---

### **5. `stkd convert`**  
Converts the current branch into a stack managed by `stkd`.

#### **Usage**  
```bash  
stkd convert  
```

#### **How it Works**  
- Prompts the user to specify the parent branch for the current branch.  
- Updates the `.stkd` metadata to include the current branch in the stack hierarchy.

#### **Example Workflow**  
```bash  
git checkout feature/refactor-auth  
stkd convert  
# Prompt: Enter the parent branch for this stack: main  
# Output: feature/refactor-auth has been converted to a stack with parent main.  
```

---

## **Configuration**

The `stkd` tool can be configured via a `.stkdconfig` file located in the root of your repository.

### **Sample `.stkdconfig`**  
```json  
{  
  "remote": "origin",  
  "defaultBranch": "main",  
  "prTemplate": ".github/pull_request_template.md",  
  "stackMetadataFile": ".stkd"  
}  
```

### **Options**  
- `remote`: The name of the Git remote to use (default: `origin`).  
- `defaultBranch`: The default branch for the repository (e.g., `main` or `master`).  
- `prTemplate`: Path to a pull request template to use for creating PRs.  
- `stackMetadataFile`: Path to the file where stack metadata is stored.

---

## **Examples**

### **Creating a New Stack**  
```bash  
git checkout feature/add-login  
stkd new  
# Prompt: Enter the name for the new stack: feature/add-logout  
# Creates branch feature/add-logout based on feature/add-login.  
```

### **Syncing Stacks**  
```bash  
stkd sync  
# Rebases all stack branches from their parent branches.  
```

### **Pushing Stacks**  
```bash  
stkd push  
# Pushes all stack branches to the remote repository.  
```

### **Creating PRs**  
```bash  
stkd pr  
# Creates a pull request for each stack branch, linking them in a hierarchical chain.  
```

---

## **Troubleshooting**

### **Common Issues**  
- **Merge conflicts during `stkd sync`:**  
  - Resolve conflicts manually, then re-run the command.  
- **PR creation fails:**  
  - Ensure you are authenticated with GitHub and have the correct permissions.  
- **Stack metadata issues:**  
  - Ensure the `.stkd` file is not corrupted or manually edited.

### **Logging**  
For verbose output, use the `--verbose` flag:  
```bash  
stkd sync --verbose  
```

---

## **Support**  
For further assistance, please contact the development team or open an issue on our GitHub repository.

---
