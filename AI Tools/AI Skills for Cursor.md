# Useful AI Skills for Cursor

## How to Get the Most Out of Cursor

### 1. Write Good Prompts
- Be **specific and contextual**: Include the file name, function name, and what you want
  - ✅ "In `api/auth.py`, add rate limiting to the `/login` route using Redis"
  - ❌ "Add rate limiting"
- Describe the **expected behavior**: "It should return a 429 error after 5 failed attempts"
- Mention **constraints**: "Don't change the existing function signature"

### 2. Use `@` References Effectively
```
@filename.py       — Include a specific file in context
@foldername/       — Include a folder
@codebase          — Search across entire codebase
@docs              — Reference documentation you've added
@web               — Search the web for current info
```

### 3. Prompt Patterns That Work Well

#### Explain Code
```
"Explain what this function does in simple terms: [paste code]"
"What are the edge cases this code doesn't handle?"
```

#### Refactor
```
"Refactor this to be more readable without changing behavior"
"Extract this logic into a separate utility function"
"Convert this class to use dependency injection"
```

#### Debug
```
"Here's the error: [paste error]. The relevant code is @filename.py. What's causing it?"
"Write a test that would catch this bug"
```

#### Generate Tests
```
"Write unit tests for all public methods in @user_service.py"
"Add edge case tests for null inputs and empty strings"
```

#### Write Documentation
```
"Add docstrings to all functions in @utils.py following Google style"
"Generate a README for this project based on the codebase"
```

### 4. Use Composer for Multi-File Tasks
- Good for: "Add a complete feature" or "Refactor the entire authentication system"
- Composer can create/edit multiple files at once
- Review each change before accepting

### 5. Cursor Rules (`.cursorrules`)
- Create this file at the project root
- Cursor reads it as context for ALL prompts
```
# Tech Stack
- Backend: FastAPI, Python 3.11
- DB: PostgreSQL with SQLAlchemy
- Auth: JWT with refresh tokens

# Code Style
- Use type hints everywhere
- Prefer composition over inheritance
- Write docstrings for all public functions
- Follow PEP 8

# Testing
- Use pytest, aim for 80% coverage
- Mock external services
```

### 6. Workflow: Fix Bugs Fast
1. Copy the **full error traceback**
2. Open Cursor Chat (`Cmd+L`)
3. Paste error + add `@relevant_file.py`
4. Ask: "What's causing this error and how do I fix it?"
5. Apply the fix with `Apply to file`

### 7. Workflow: Understand a New Codebase
```
1. Open project in Cursor
2. Chat: "Give me an overview of this project's architecture based on @codebase"
3. Ask: "What does the @models.py file contain and how is it used?"
4. Navigate: Ask "Where is the authentication logic handled?"
```

### 8. Things Cursor Is Great At
- Boilerplate and repetitive code
- Writing tests based on implementation
- Explaining unfamiliar code/libraries
- Converting code between languages/frameworks
- Reviewing code for bugs and improvements
- Writing regex and complex queries

### 9. Things to Watch Out For
- Cursor can **hallucinate** — always review generated code
- May use outdated API versions — verify with docs
- Large codebases may exceed context limits
- Don't blindly apply suggestions to production code

### 10. Useful Keyboard Shortcuts
| Action | Shortcut |
|--------|----------|
| Open Chat | Cmd/Ctrl + L |
| Inline Edit | Cmd/Ctrl + K |
| Open Composer | Cmd/Ctrl + I |
| Accept Autocomplete | Tab |
| Reject Autocomplete | Esc |
| Toggle Sidebar | Cmd/Ctrl + B |
