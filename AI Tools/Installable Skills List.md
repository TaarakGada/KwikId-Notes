# Installable Cursor Skills — Quick List

> Skills like Caveman that you can install with one command.
> Install via: `npx skills add <owner/repo> -a cursor`

---

## 🔥 Most Popular / Trending

| Skill | What It Does | Install |
|-------|-------------|---------|
| **caveman** | Ultra-terse replies. No fluff. ~65% fewer tokens. Toggle with `/caveman full` | `npx skills add JuliusBrussee/caveman -a cursor` |
| **superpowers** | Full agentic dev methodology — planning, tasks, memory, code style all-in-one | `npx skills add obra/superpowers -a cursor` |
| **ECC** | Agent performance booster — memory, security, research-first dev workflow | `npx skills add affaan-m/ECC -a cursor` |
| **hermes-agent** | Adaptive agent that "grows with you" — learns your preferences over time | `npx skills add NousResearch/hermes-agent -a cursor` |

---

## 🛠️ Developer Productivity

| Skill | What It Does | Install |
|-------|-------------|---------|
| **agent-skills** | Vercel's official skills collection — Next.js, deployments, Edge functions | `npx skills add vercel-labs/agent-skills -a cursor` |
| **cursor-skills** | Community curated Cursor-specific skill set | `npx skills add aussiegingersnap/cursor-skills -a cursor` |
| **code-review** | Adds structured code review workflow to Cursor | Search on `skillsllm.com` |
| **test-writer** | Automatically writes unit tests when you implement a function | Search on `skillsllm.com` |
| **git-helper** | Smart git commit messages, PR descriptions, branch naming | Search on `skillsllm.com` |

---

## 🧠 Behavior / Communication Style

| Skill | What It Does | How to Get |
|-------|-------------|------------|
| **caveman** ⭐ | Short, dense, zero-filler responses | `npx skills add JuliusBrussee/caveman -a cursor` |
| **no-yapping** | Similar to Caveman — stops AI from over-explaining | Add to `.cursorrules`: `"Don't yap. Answer directly."` |
| **terse-mode** | Brief, engineering-style communication | Add to global rules manually |

---

## 🔍 Research / Code Understanding

| Skill | What It Does | Install |
|-------|-------------|---------|
| **superpowers** | Includes research-first dev mode — reads docs before coding | `npx skills add obra/superpowers -a cursor` |
| **ECC** | Research-first approach + memory of past decisions | `npx skills add affaan-m/ECC -a cursor` |

---

## 📦 Bulk Install Options

### Install Everything from the Awesome-Skills Library
```bash
# 1,900+ skills from the community
npx skills add sickn33/agentic-awesome-skills -a cursor
```

### Use the upskill CLI (alternative installer)
```bash
# Install the upskill CLI first
npm install -g upskill

# Search for skills
upskill search "code review"
upskill search "testing"

# Install a skill
upskill install <skill-name>
```

---

## 🌐 Where to Browse More Skills

| Site | What You'll Find |
|------|-----------------|
| [skillsllm.com](https://skillsllm.com) | 3,700+ security-vetted skills, searchable |
| [skills-hub.ai](https://skills-hub.ai) | Open skill registry, one-command installs |
| [cursor.directory](https://cursor.directory) | Cursor rules + community favorites |
| [github.com/PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | Curated `.cursorrules` for every stack |

---

## 🚀 Quick Start — Recommended Stack

Install these 3 skills and you're set up well:

```bash
# 1. Caveman — make Cursor shut up and just code
npx skills add JuliusBrussee/caveman -a cursor

# 2. Superpowers — structured dev methodology
npx skills add obra/superpowers -a cursor

# 3. Vercel's official agent skills (great for web dev)
npx skills add vercel-labs/agent-skills -a cursor
```

Then add this to **Cursor → Settings → Rules**:
```
Use caveman skill full
```

---

## 💡 How to Find New Skills via CLI

```bash
# Search the registry directly
npx skills find "docker"
npx skills find "python testing"
npx skills find "code review"

# List what you have installed
npx skills list

# Remove a skill
npx skills remove caveman
```

---

> **Note:** Skill availability changes as the ecosystem grows fast.
> Check [skillsllm.com](https://skillsllm.com) for the latest vetted list.
