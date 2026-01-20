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
# rdd.md file is under requirement-driven-development/command folder
cd requirement-driven-development/command
```

### 2. Choose Your Setup Method

- [Claude Code](#for-claude-code) (Recommended)
- [Kiro](#for-kiro)
- [Other LLM Tools](#for-other-llm-tools-cursor-windsurf-etc)

---

## Setup Instructions

### For Claude Code

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

### For Kiro

1. **Copy the RDD file** to a known location:
   ```bash
   mkdir -p ~/ai
   cp rdd.md ~/ai/rdd.md
   ```

2. **Initialize in conversation**:
   ```
   read file '~/ai/rdd.md', then i will use command same as in that md file.
   ```

3. **Use commands** as normal after initialization.

> **Note:** For Kiro-specific MCP setup, consult your team lead.

### For Other LLM Tools (Cursor, Windsurf, etc.)

1. **Copy the RDD file** to your project or home directory:
   ```bash
   # Project-level
   mkdir -p ./.ai
   cp rdd.md ./.ai/rdd.md
   
   # Or user-level
   mkdir -p ~/ai
   cp rdd.md ~/ai/rdd.md
   ```

2. **Reference in conversation**:
   ```
   Please read the file at ./.ai/rdd.md and follow the RDD workflow for my requests.
   ```

3. **Or paste directly** into the conversation before starting work.

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
/rdd new create e-commerce platform name rdd-commerce looklike lazada using nextjs + using bun as run time and deliver with dockerize should be able to start app with docker-compose up

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

### MCP not working
- Verify MCP servers are configured in your Claude settings
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