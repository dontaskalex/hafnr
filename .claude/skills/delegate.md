---
name: delegate
description: "Split a task into 3 parallel subtasks and delegate them to 3 coder-worker agents simultaneously. Use this when you want to parallelize coding work for faster completion."
user_invocable: true
---

# Delegate Workflow

The user wants to delegate a coding task to 3 parallel coding agents for faster execution.

## Your Process

### Step 1: Analyze the Task
Look at the task provided and determine how to split it into 3 **independent** subtasks that can be worked on simultaneously without conflicts.

Good splits:
- Different files or components
- Different features or functions
- Different layers (e.g., UI component, logic, tests)
- Different sections of a larger feature

Bad splits (avoid):
- Tasks that modify the same file
- Tasks where one depends on another's output
- Tasks that would cause merge conflicts

### Step 2: Plan the Split
Create a clear plan showing:
- **Subtask 1**: [description] - Files: [files it will touch]
- **Subtask 2**: [description] - Files: [files it will touch]
- **Subtask 3**: [description] - Files: [files it will touch]

Show this plan to the user and confirm before proceeding.

### Step 3: Launch 3 Parallel Agents
Use the Task tool to launch exactly 3 `coder-worker` agents **in parallel** (all in the same message). Each agent gets:
- Clear description of their specific subtask
- List of files they should work with
- Context about the overall goal
- Instructions not to touch other agents' files

Example Task call structure:
```
Task 1: "Implement [subtask 1] - work only on [files]"
Task 2: "Implement [subtask 2] - work only on [files]"
Task 3: "Implement [subtask 3] - work only on [files]"
```

### Step 4: Collect Results
Once all 3 agents complete:
1. Summarize what each agent accomplished
2. Note any integration points that may need attention
3. Suggest any follow-up tasks if needed (like running tests)

## Important Rules

- **Always split into exactly 3 subtasks** - find a way to divide the work
- **Ensure no file conflicts** - each agent should touch different files
- **Launch all 3 in parallel** - use a single message with 3 Task tool calls
- **Use `coder-worker` as the subagent_type** for all 3 agents
- If the task is too small to split 3 ways, tell the user and offer to just do it directly

## Example Usage

User: "/delegate Add a user authentication system with login, signup, and password reset"

Split:
- Agent 1: Login page + login API endpoint (login.tsx, api/login.ts)
- Agent 2: Signup page + signup API endpoint (signup.tsx, api/signup.ts)
- Agent 3: Password reset page + reset API endpoint (reset.tsx, api/reset.ts)
