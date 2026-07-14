# Cursor Skills & Rules Directory

> A curated list of useful `.cursorrules`, community skills, and `SKILL.md` files for Cursor AI.
> These shape how Cursor behaves — think of them as standing instructions for your AI pair programmer.

---

## 🧠 What Are Cursor Rules / Skills?

- **`.cursorrules`** — A file at the root of your project. Cursor reads it on every prompt. Sets tone, style, stack preferences.
- **`.cursor/rules/*.mdc`** — The newer modular format. Each `.mdc` file is a separate rule (can be scoped to file types).
- **`SKILL.md`** — A modular, on-demand skill file. Loaded by the agent only when relevant (not always-on).
- **`AGENTS.md`** — A vendor-neutral instruction file understood by Cursor, Claude Code, and other AI tools.

---

## ⚡ Must-Have Skill: Caveman

### What is it?
- Created by **Julius Brussee** (`JuliusBrussee/caveman`)
- Forces the AI to respond in **short, dense, technical bursts** — no fluff, no filler
- Philosophy: *"Why use many tokens when few do trick?"*
- Reduces response tokens by ~65% while keeping all technical info intact
- Strips: greetings, summaries, "of course!", "sure!", "let me explain…", hedging phrases

### Install
```bash
npx skills add JuliusBrussee/caveman -a cursor
```

### Usage
```
/caveman lite    → slightly terse
/caveman full    → no fluff at all (recommended)
/caveman ultra   → bare minimum words
stop caveman     → back to normal
```

### Add to Global Cursor Rules (Settings → Rules)
```
Use caveman skill full
```

### Example — Before vs After Caveman

**Without Caveman:**
> "Of course! I'd be happy to help you with that. Let me explain what's happening here. Basically, the issue is that you're calling the function before it's defined. This is a common mistake. Here's how you can fix it..."

**With Caveman:**
> "Hoisting issue. Move function definition above call site."

---

## 🌍 Where to Find Rules

| Source | Link | Description |
|--------|------|-------------|
| **cursor.directory** | https://cursor.directory | Official community hub. Sort by Trending/Top. |
| **awesome-cursorrules** | https://github.com/PatrickJS/awesome-cursorrules | Largest GitHub collection of rules |
| **cursorrules.org** | https://cursorrules.org | Searchable database of rules |
| **dotcursorrules.com** | https://dotcursorrules.com | More community rules |
| **skillsllm.com** | https://skillsllm.com | Skill registry (Caveman and others) |

---

## 📋 Most Popular Community Rules (by Stack)

### 🌐 Web — Frontend

| Rule | What it does |
|------|-------------|
| **Next.js + Tailwind + shadcn** | Rules for modern React stack with Tailwind & component libraries |
| **React + TypeScript** | Enforce functional components, type hints, hook best practices |
| **SvelteKit** | Rules for SvelteKit projects with Svelte conventions |
| **Vue 3 + Vite** | Composition API preferences, Vite config patterns |

### 🐍 Python / Backend

| Rule | What it does |
|------|-------------|
| **FastAPI** | REST API patterns, Pydantic models, async handlers |
| **Django** | ORM patterns, views, serializers, URLs |
| **Python General** | PEP 8, type hints, docstrings, no magic numbers |
| **Flask** | Blueprints, error handling, app factory pattern |

### ☁️ Cloud / DevOps

| Rule | What it does |
|------|-------------|
| **AWS CDK** | Infrastructure as code best practices |
| **Terraform** | Module structure, variable naming, state patterns |
| **Docker** | Dockerfile best practices, multi-stage builds |

### 📱 Mobile

| Rule | What it does |
|------|-------------|
| **React Native** | Navigation, state, platform-specific code |
| **Flutter/Dart** | Widget patterns, state management (Riverpod, Bloc) |
| **Kotlin / Jetpack Compose** | Modern Android development |

---

## 🔧 Global Rules (Work Across Any Project)

These are general behavioral rules — paste them into **Cursor Settings → Rules** (not project-specific):

```
# Communication
- Be terse. No filler words or pleasantries.
- Don't explain what you're about to do — just do it.
- Don't summarize changes after making them.
- If something is obvious, skip the explanation.

# Code Quality
- Always use early returns to reduce nesting.
- Write self-documenting code. Minimize comments that state the obvious.
- Never leave TODOs unless explicitly asked.
- Prefer explicit over implicit.

# Behavior
- If you're unsure about something, ask ONE clarifying question. Not multiple.
- Don't add features I didn't ask for.
- Preserve all existing comments and logic unless told otherwise.
- When fixing a bug, change only what's needed.
```

---

## 📁 Project-Specific `.cursorrules` Template

Create a `.cursorrules` file at the root of any project:

```
# Project: [Your Project Name]

## Stack
- Backend: Python 3.11, FastAPI, PostgreSQL
- Frontend: React 18, TypeScript, Tailwind CSS
- Infra: AWS EC2, Redis, Docker

## Code Style
- Python: PEP 8, always use type hints, Google-style docstrings
- JS/TS: Use arrow functions, prefer const over let
- No magic numbers — use named constants

## Patterns to Follow
- Backend: Repository pattern for DB access
- Error handling: Always return structured error responses
- Logging: Use structured logs (JSON format)

## Testing
- Framework: pytest (Python), Vitest (JS)
- Aim for 80% coverage
- Mock all external services

## Don'ts
- Don't use * imports
- Don't hardcode secrets — use environment variables
- Don't use any (TypeScript) unless absolutely necessary
```

---

## 🗂️ Newer Format: `.cursor/rules/*.mdc`

The modern way to organize rules — each file is scoped:

```
.cursor/
└── rules/
    ├── global.mdc          ← Always applied
    ├── python.mdc          ← Applied to *.py files only
    ├── react.mdc           ← Applied to *.tsx files only
    └── tests.mdc           ← Applied to *test* files only
```

### Example `python.mdc`
```markdown
---
globs: ["**/*.py"]
---

- Always add type hints to function arguments and return values
- Use dataclasses or Pydantic models for structured data
- Prefer list comprehensions over map/filter
- Use pathlib over os.path
```

---

## 💡 Pro Tips

- **Stack rules**: Use global rules for communication style + project rules for tech stack
- **Caveman first**: Install Caveman before anything else — saves tokens on every session
- **AGENTS.md**: Works across Cursor, Claude Code, Gemini CLI — write once, use everywhere
- **Iterate your rules**: If Cursor keeps doing something wrong, add it to `.cursorrules`
- **Share with team**: Commit `.cursorrules` to your repo so everyone gets consistent AI behavior
- **Rule conflicts**: More specific rules (file-scoped `.mdc`) override global ones

---

## 🔗 Quick Reference

```bash
# Install Caveman skill
npx skills add JuliusBrussee/caveman -a cursor

# Browse rules
open https://cursor.directory

# Awesome rules collection
open https://github.com/PatrickJS/awesome-cursorrules
```
