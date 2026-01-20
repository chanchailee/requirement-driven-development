# Requirement-Driven Development (RDD) Command

A custom command for AI-assisted development workflow that helps you manage requirements, tasks, and quality gates systematically.

## What is RDD?

RDD (Requirement-Driven Development) is a structured approach to development that:
- Creates traceable requirements with acceptance criteria
- Breaks down work into trackable tasks with time estimates
- Enforces quality gates (testing, linting, documentation)
- Maintains status flow from PENDING → IN_PROGRESS → DONE
- Integrates with MCP tools (Context7, Playwright, Figma) for enhanced development experience

## Prerequisites

### MCP Servers

> **Note:** RDD works without any MCP servers. The core workflow (requirements tracking, task management, quality gates) functions properly without MCP. However, MCP servers enhance the development experience significantly.

| MCP Server | Status | Purpose |
|------------|--------|---------|
| **Context7 MCP** | Recommended | Library documentation lookup, best practices research, framework-specific guidance |
| **Playwright MCP** | Recommended | UI E2E testing, visual debugging, screenshot capture, network request inspection |
| **Figma MCP** | Nice to have | Design-to-code workflow, extracting design tokens, frontend development tasks |

### Without MCP

If you don't have MCP servers configured:
- ✅ Requirement tracking works normally
- ✅ Task management works normally
- ✅ Quality gates work normally
- ⚠️ Skip Context7 research steps (use manual documentation lookup)
- ⚠️ Skip Playwright debugging steps (use browser DevTools instead)
- ⚠️ Skip Figma integration (use manual design reference)

## Quick Start

### 1. Clone the Repository

```bash
git clone git@github.com:chanchailee/requirement-driven-development.git
cd requirement-driven-development/command
# rdd.md file is under requirement-driven-development/command folder
```

### 2. Choose Your Setup Method

| Tool | Custom Commands | Setup Type |
|------|-----------------|------------|
| [Claude Code](#for-claude-code) | ✅ Native | Copy to `~/.claude/commands/` |
| [Cursor](#for-cursor) | ✅ Native | Copy to `.cursor/commands/` |
| [Windsurf](#for-windsurf) | ✅ Workflows | Copy to `.windsurf/workflows/` |
| [Gemini Code Assist](#for-gemini-code-assist) | ✅ Custom Commands | Settings > Custom Commands |
| [Gemini CLI](#for-gemini-cli) | ✅ Extensions | Copy to `~/.gemini/` |
| [GitHub Copilot](#for-github-copilot) | ⚠️ Partial | Copy to `.github/prompts/` |
| [Kiro, Codex, Others](#for-other-llm-tools-kiro-codex-etc) | ❌ None | Prerequisite prompt |

---

## Setup Instructions

### Important: Eager Load for Each Session

> **For all tools except Claude Code**, you should eager load the RDD workflow at the start of each development session:
> ```
> Read file '<path-to-rdd.md>', then I will use commands as defined in that file.
> ```
> This ensures the AI understands the RDD workflow before you start using `/rdd` commands.
>
> **Claude Code** is the exception - it natively loads commands from `~/.claude/commands/` automatically.

### For Claude Code

Claude Code supports native custom slash commands.

1. **Create the commands directory** (if not exists):
   ```bash
   mkdir -p ~/.claude/commands
   ```

2. **Copy the RDD command file**:
   ```bash
   cp rdd.md ~/.claude/commands/rdd.md
   ```

3. **Start Claude Code**:
   ```bash
   claude
   ```

4. **Use the command**:
   ```
   /rdd new <requirement-name>
   ```

---

### For Cursor

Cursor supports native custom slash commands via `.cursor/commands/`.

1. **Create the commands directory** in your project:
   ```bash
   mkdir -p .cursor/commands
   ```

2. **Copy the RDD command file**:
   ```bash
   cp rdd.md .cursor/commands/rdd.md
   ```

3. **Eager load before starting development** (recommended):
   ```
   Read file '.cursor/commands/rdd.md' to load RDD workflow for this session.
   ```

4. **Use the command** in Cursor's Agent input:
   ```
   /rdd new <requirement-name>
   ```

> **Note:** Cursor commands are project-scoped. Add `.cursor/commands/` to your repository to share with your team.

---

### For Windsurf

Windsurf supports custom workflows via YAML configuration.

1. **Create the workflows directory**:
   ```bash
   mkdir -p .windsurf/workflows
   ```

2. **Copy and reference the RDD file**:
   ```bash
   cp rdd.md .windsurf/rdd.md
   ```

3. **Eager load before starting development** (required for each session):
   ```
   Read file '.windsurf/rdd.md', then I will use commands as defined in that file.
   ```

4. **Use the command**:
   ```
   /rdd new <requirement-name>
   ```

---

### For Gemini Code Assist

Gemini Code Assist supports custom commands via IDE settings.

#### VS Code

1. Open Command Palette: `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS)
2. Search for `Preferences: Open Settings (UI)`
3. Search for `Geminicodeassist: Custom Commands`
4. Click `Add Item`
5. Name: `rdd`
6. Paste the content of `rdd.md` as the prompt

#### JetBrains IDEs

1. Navigate to `Settings > Tools > Gemini > Prompt Library`
2. Click `Add`
3. Name: `rdd`
4. Paste the content of `rdd.md`
5. Check `Show in In-Editor Prompt`

---

### For Gemini CLI

Gemini CLI supports custom extensions.

1. **Create the extensions directory**:
   ```bash
   mkdir -p ~/.gemini/extensions
   ```

2. **Copy the RDD file**:
   ```bash
   cp rdd.md ~/.gemini/extensions/rdd.md
   ```

3. **Eager load before starting development** (required for each session):
   ```bash
   gemini
   ```
   ```
   Read file '~/.gemini/extensions/rdd.md', then I will use commands as defined in that file.
   ```

4. **Use the command**:
   ```
   /rdd new <requirement-name>
   ```

---

### For GitHub Copilot

GitHub Copilot has limited support for custom prompts via `.github/prompts/`.

1. **Create the prompts directory**:
   ```bash
   mkdir -p .github/prompts
   ```

2. **Copy and rename the RDD file**:
   ```bash
   cp rdd.md .github/prompts/rdd.prompt.md
   ```

3. **Eager load before starting development** (recommended):
   ```
   Read file '.github/prompts/rdd.prompt.md', then I will use commands as defined in that file.
   ```

4. **Use in Copilot Chat** (VS Code):
   - Type `/` to see available commands
   - Select `rdd` from the list (if supported)
   - Or use command directly: `/rdd new <requirement>`

> **Note:** Custom slash command support in GitHub Copilot varies by environment. If not available, the eager load prompt method above will work.

---

### For Other LLM Tools (Kiro, Codex, etc.)

Tools without native custom command support require an eager load prompt before each session.

#### Setup

1. **Copy the RDD file** to a known location:
   ```bash
   # User-level (recommended)
   mkdir -p ~/llm-prompts
   cp rdd.md ~/llm-prompts/rdd.md
   
   # Or project-level
   mkdir -p ./llm-prompts
   cp rdd.md ./llm-prompts/rdd.md
   ```

   > **Note:** `llm-prompts` is a suggested folder name. You can use any folder name you prefer (e.g., `ai-prompts`, `prompts`, etc.).

#### Usage

2. **Eager load before starting development** (required for each session):
   ```
   Read file '~/llm-prompts/rdd.md', then I will use commands as defined in that file.
   ```

3. **Use commands** as normal after eager load:
   ```
   /rdd new <requirement-name>
   ```

#### Alternative: Direct Paste

If file access is not available, copy and paste the content of `rdd.md` directly into the conversation before starting work.

---

## Usage

### Available Commands

| Command | Description | Example |
|---------|-------------|---------|
| `/rdd new <n>` | Create and execute new requirement | `/rdd new oauth-integration` |
| `/rdd resume <n>` | Continue in-progress requirement | `/rdd resume oauth-integration` |
| `/rdd list` | Show all requirements with status | `/rdd list` |
| `/rdd review <n>` | Analyze requirement quality | `/rdd review oauth-integration` |
| `/rdd rollback <n>` | Revert failed requirement | `/rdd rollback oauth-integration` |

### Real-World Examples

```bash
# Create a new feature
/rdd new create e-commerce platform name rdd-commerce looklike lazada using nextjs + using 
bun as run time and deliver with dockerize should be able to start app with docker-compose up
the database use sqlite and should have basic features like user authentication , product search, product listing shopping cart and order management

# Create with detailed description
/rdd new create survey platform backoffice using react and microsoft fluent ui make it look enterprise grade

# Fix PR comments
/rdd new fix all comments for pr [pr link here]

# Resume interrupted work
/rdd resume e-commerce platform
/rdd resume continue pending task

# Check all requirements status
/rdd list
```

---

## Folder Structure

RDD creates and manages requirements in the following structure:

```
requirements/
├── 01_pending/          # New requirements waiting to start
├── 02_in_progress/      # Currently being worked on
├── 03_done/             # Completed requirements
└── 04_archived/         # Rolled back or cancelled
```

### File Naming Convention
```
{YYYYMMDD_HHMM}-{requirement-name}.md

Example: 20241213_1430-oauth-integration.md
```

---

## Workflow Overview

```
┌─────────────┐     ┌──────────────┐     ┌────────┐     ┌──────────┐
│   PENDING   │ ──▶ │ IN_PROGRESS  │ ──▶ │  DONE  │ ──▶ │ ARCHIVED │
└─────────────┘     └──────────────┘     └────────┘     └──────────┘
      │                    │                               ▲
      │                    │         (rollback)            │
      │                    └───────────────────────────────┘
      │
      └──▶ Research (Context7 MCP - if available)
      └──▶ Plan tasks with estimates
      └──▶ Execute with quality gates
      └──▶ Test (ASK USER for test types)
```

---

## Testing Strategy

RDD will **ASK USER** before running any tests:

- **Unit tests** - Test individual functions/components
- **Integration tests** - Test component interactions  
- **UI E2E tests** - Using Playwright MCP (if available)
- **API E2E tests** - Using Supertest/Jest

You can choose which tests to run or skip all tests.

---

## Quality Gates

Before marking a requirement as DONE:

1. ✅ All tasks in SUCCEED or FAILED (final states)
2. ✅ All requested tests pass
3. ✅ Documentation updated
4. ✅ Code quality gates passed (linting, formatting)
5. ✅ CLAUDE.md updated with learnings

---

## Tips

1. **Always have a `CLAUDE.md`** in your project root - RDD checks this first
2. **Be specific** with requirement names for better tracking
3. **Use Context7 MCP** for researching libraries before implementation (if available)
4. **Use Playwright MCP** for debugging UI issues visually (if available)
5. **Use Figma MCP** for design-to-code workflow (if available)
6. **Review regularly** with `/rdd list` to track progress

---

## Troubleshooting

### Command not recognized (Claude Code)
- Ensure file is at `~/.claude/commands/rdd.md`
- Restart Claude Code
- Check file permissions: `chmod 644 ~/.claude/commands/rdd.md`

### Command not recognized (Cursor)
- Ensure file is at `.cursor/commands/rdd.md` in your project
- Restart Cursor
- Commands are project-scoped, not global

### Command not recognized (Other Tools)
- Ensure you ran the eager load prompt first
- Try re-reading the file: `Read file '~/llm-prompts/rdd.md' again`
- Or paste `rdd.md` content directly into conversation

### MCP not working
- Verify MCP servers are configured in your settings
- Check MCP server logs for errors
- RDD will continue to work without MCP - just skip MCP-specific steps

### Requirements not tracking
- Ensure `requirements/` folder exists in project root
- Check write permissions on the folder

---

## License

MIT License

Copyright (c) 2025 Chanchai Lee

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
