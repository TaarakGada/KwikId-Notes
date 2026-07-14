# Cursor — AI Code Editor

## What is it?
- Cursor is a **code editor built on VS Code** with deep AI integration
- It understands your entire codebase and helps you write, edit, and debug code
- Think of it as: VS Code + a brilliant pair programmer who knows your whole project

## Key Features

### Chat (Cmd/Ctrl + L)
- Ask questions about your codebase
- "Explain how this function works"
- "Why is this test failing?"
- "What does this regex do?"

### Inline Edit (Cmd/Ctrl + K)
- Select code and ask Cursor to change it
- "Refactor this to use async/await"
- "Add error handling here"
- "Optimize this loop"

### Composer / Agent Mode
- Give a high-level task — Cursor figures out which files to change
- "Add a login page with JWT authentication"
- "Write unit tests for the UserService class"
- Can create files, run terminal commands, iterate on errors

### Codebase Context
- Cursor indexes your entire project
- Answers are specific to YOUR code, not generic
- Use `@filename` or `@codebase` to reference specific context

### Tab Autocomplete
- Predicts multi-line completions as you type
- Context-aware — knows what you're building

## Tips for Better Results
- **Be specific**: "Add input validation to the /api/login route in auth.py" > "fix the login"
- Use **`@` references**: `@user_service.py` to include a specific file in context
- **Iterate**: If the first answer isn't right, refine in the same chat
- **Rules**: Add a `.cursorrules` file to set project-wide instructions for Cursor
- Use **Notepads** to save frequently used context (architecture docs, style guides)

## .cursorrules Example
```
This is a Django + React project.
Backend: Python 3.11, Django 5, PostgreSQL
Frontend: React 18, TypeScript, Tailwind CSS
Always write type hints in Python.
Prefer functional React components.
Follow PEP 8 for Python.
Use descriptive variable names.
```

## Pricing (as of 2024)
- **Free tier**: Limited usage
- **Pro**: ~$20/month — unlimited completions, higher limits
- Supports: GPT-4, Claude, Gemini (choose the model)

## Good to Know
- Based on VS Code — all VS Code extensions work
- Privacy mode: code not stored on servers
- `Cmd+Shift+P` → access all commands like VS Code
- Works best with smaller, well-organized codebases
