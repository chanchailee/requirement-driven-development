---
name: rdd
argument-hint: "[action] [requirement]"
description: "Requirement-driven development. Usage: /rdd action requirement, action: new|resume|list|review|rollback, requirement: filename or description"

---

ALWAYS CHECK CLAUDE.md before starting

# Requirement-Driven Development: {{action}} {{name}}

## Quick Reference
- `/rdd new oauth-integration` - Create and execute new requirement
- `/rdd resume oauth-integration` - Continue in-progress requirement
- `/rdd list` - Show all requirements with status
- `/rdd review oauth-integration` - Analyze requirement quality
- `/rdd rollback oauth-integration` - Revert failed requirement

---

## Action: {{action}}

### If action = "new"
**{{name}} is required for new action**

1. **Research** using Context7 MCP if needed
2. **Plan** tasks with acceptance criteria and estimates
3. **Create** `requirements/01_pending/{YYYYMMDD_HHMM}-{{name}}.md` with:
   - Acceptance criteria
   - Task breakdown with time estimates (CRITICAL SHOULD BE ACCURATE)
   - Dependencies between tasks
   - Risk assessment
4. **Execute** all tasks following the workflow below

### If action = "resume"
**{{name}} is required for resume action**

1. **Locate** from `requirements/02_in_progress/{{name}}.md`
2. **Review** failed tasks first
3. **Continue** execution from last checkpoint (CRITICAL SHOULD BE ACCURATE)

### If action = "list"

1. **Scan** all requirement folders (01_pending, 02_in_progress, 03_done, 04_archived)
2. **Display** requirements grouped by status with:
   - Progress percentage
   - Task summary (PENDING/IN_PROGRESS/SUCCEED/FAILED counts)
   - Highlight blocked or failed tasks
   - Total time spent vs estimated

### If action = "review"
**{{name}} is required for review action**

1. **Analyze** the requirement file `{{name}}.md`
2. **Check** for:
   - Missing tests or documentation
   - Code quality issues
   - Incomplete acceptance criteria
   - Dependency conflicts
   - Tasks stuck in IN_PROGRESS
3. **Suggest** optimizations and improvements

### If action = "rollback"
**{{name}} is required for rollback action**

1. **Revert** all code changes from the requirement
2. **Move** file to `requirements/04_archived/` with failure notes
3. **Document** lessons learned
4. **Update** CLAUDE.md with what went wrong

---

# Execution Workflow

## Status Management (CRITICAL)

### Requirement Status Flow
- **PENDING** → File in `requirements/01_pending/{YYYYMMDD_HHMM}-{name}.md`
- **IN_PROGRESS** → Move to `requirements/02_in_progress/{YYYYMMDD_HHMM}-{name}.md`
- **DONE** → Move to `requirements/03_done/{YYYYMMDD_HHMM}-{name}.md`
- **ARCHIVED** → Move to `requirements/04_archived/{YYYYMMDD_HHMM}-{name}.md`
Note: YYYYMMDD_HHMM should be accurate and UTC time

### Task Status Flow
- **Create** → PENDING
- **Working** → IN_PROGRESS
- **Success** → SUCCEED
- **Failure** → FAILED

**FINAL STATES:** Tasks can only end in SUCCEED or FAILED. Requirements move to DONE only when ALL tasks are in final states.

## Requirement File Template
```markdown
# Requirement: {name}
Status: PENDING
Created: {YYYYMMDD_HHMM}
Estimated Time: {hours}
Actual Time: {hours}

## Description
{What this requirement achieves}

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Dependencies
- {Other requirements or tasks}

## Tasks

### Task 1: {name}
Status: PENDING
Estimate: {time}
Actual: {time}
Dependencies: {tasks}

#### Implementation
- [ ] Code changes
- [ ] Tests added
- [ ] Docs updated

#### Results
{Updated after completion}

---

## Task Summary
PENDING: X tasks
IN_PROGRESS: X tasks
SUCCEED: X tasks
FAILED: X tasks
PROGRESS: XX%
```

## Pre-execution Checklist (CRITICAL)
- [ ] CLAUDE.md reviewed
- [ ] Dependencies satisfied
- [ ] Environment validated
- [ ] Tests exist for acceptance criteria
- [ ] Research completed using Context7 MCP

## Quality Gates (CRITICAL)

### 1. Code Quality
- Apply SOLID principles
- Follow clean code practices
- Run linting: `npm run lint` or equivalent
- Run prettier: `npm run format` or equivalent
- No console.log or debug code
- Proper error handling

### 2. Testing Strategy

#### User Confirmation Required (CRITICAL)
Before running any tests, ASK USER:
- "Do you need **Unit tests** for this requirement?"
- "Do you need **Integration tests** for this requirement?"
- "Do you need **UI E2E tests** for this requirement? (Playwright)"
- "Do you need **API E2E tests** for this requirement? (Supertest/Jest)"
- "Skip all tests for now?"

Wait for user confirmation before proceeding with test execution.

#### Test Types
- **Unit tests**: (ASK USER first)
  - Per task implementation
  - Test individual functions/components in isolation
- **Integration tests**: (ASK USER first)
  - Per requirement completion
  - Test component interactions
- **UI E2E tests**: (ASK USER first)
  - Critical user paths only
  - Use Playwright MCP for execution and debugging
  - Capture screenshots for documentation
- **API E2E tests**: (ASK USER first)
  - Use Supertest/Jest for automated API testing
  - Run filtered tests: `npm test -- --grep "API"` or equivalent
  - Verify request/response contracts
  - Test authentication flows
  - Test error handling and edge cases
  - Use Playwright MCP for API debugging when:
    - Need to inspect network requests in browser
    - Verify API calls triggered by UI interactions
    - Debug CORS or authentication issues

### 3. Documentation
- Update CLAUDE.md with learnings
- Generate changelog from SUCCEED tasks
- Update architecture docs if structural changes
- Add inline code comments for complex logic

## Research Workflow (CRITICAL)
- **ALWAYS USE Context7 MCP** for:
  - Library documentation and best practices
  - Design patterns and architecture
  - Framework-specific implementations
- Document research findings in requirement file
- Link to relevant documentation

## Playwright MCP (CRITICAL)
- **ASK USER first** before running any Playwright tests
- **Use Playwright MCP** for:
  - UI E2E testing (after user confirmation)
  - Visual debugging of UI issues
  - Verifying user flows and interactions
  - Capturing screenshots for documentation
  - API debugging (inspect network requests in browser context)
  - Debugging CORS or authentication issues
- **Use Playwright MCP for debugging** when:
  - UI components don't render correctly
  - User interactions fail unexpectedly
  - Need to verify visual state of application
  - Integration issues between frontend components
  - API calls from browser need inspection

## Monitoring & Metrics
- Track actual time vs estimates per task
- Flag requirements with >80% FAILED tasks
- Suggest requirement split if >10 tasks
- Calculate velocity (tasks/hour)

## Completion Criteria (CRITICAL)

Requirement moves to DONE when:
1. ALL tasks in SUCCEED or FAILED (final states)
2. All tests pass
3. Documentation updated
4. Code quality gates passed
5. Linting and prettier executed
6. CLAUDE.md updated with learnings

---

## File Naming Pattern (CRITICAL)
`requirements/{status}/{YYYYMMDD_HHMM}-{requirement-name}.md`

Example: `requirements/01_pending/20241213_1430-oauth-integration.md`