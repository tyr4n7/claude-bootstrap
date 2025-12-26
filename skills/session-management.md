# Session Management Skill

*Load with: base.md*

For maintaining context across long development sessions and enabling seamless resume after breaks.

---

## Core Principle

**Checkpoint often, resume instantly.**

Long development sessions risk context loss. Proactively document state, decisions, and progress so any session can resume exactly where it left off - whether returning after a break or hitting context limits.

---

## Session State Structure

Create `_project_specs/session/` directory:

```
_project_specs/
└── session/
    ├── current-state.md      # Live session state (update frequently)
    ├── decisions.md          # Key decisions log (append-only)
    ├── code-landmarks.md     # Important code locations
    └── archive/              # Past session summaries
        └── 2025-01-15.md
```

---

## Current State File

**`_project_specs/session/current-state.md`** - Update every 15-20 minutes or after significant progress.

```markdown
# Current Session State

*Last updated: 2025-01-15 14:32*

## Active Task
[One sentence: what are we working on right now]

Example: Implementing user authentication flow with JWT tokens

## Current Status
- **Phase**: [exploring | planning | implementing | testing | debugging | refactoring]
- **Progress**: [X of Y steps complete, or percentage]
- **Blocking Issues**: [None, or describe blockers]

## Context Summary
[2-3 sentences summarizing the current state of work]

Example: Created auth middleware and login endpoint. JWT signing works.
Currently implementing token refresh logic. Need to add refresh token
rotation for security.

## Files Being Modified
| File | Status | Notes |
|------|--------|-------|
| src/auth/middleware.ts | Done | JWT verification |
| src/auth/refresh.ts | In Progress | Token rotation |
| src/auth/types.ts | Done | Token interfaces |

## Next Steps
1. [ ] Complete refresh token rotation in refresh.ts
2. [ ] Add token blacklist for logout
3. [ ] Write integration tests for auth flow

## Key Context to Preserve
- Using RS256 algorithm (not HS256) per security requirements
- Refresh tokens stored in HttpOnly cookies
- Access tokens: 15 min, Refresh tokens: 7 days

## Resume Instructions
To continue this work:
1. Read src/auth/refresh.ts - currently at line 45
2. The rotateRefreshToken() function needs error handling
3. Check decisions.md for why we chose RS256 over HS256
```

---

## Decision Log

**`_project_specs/session/decisions.md`** - Append-only log of architectural and implementation decisions.

```markdown
# Decision Log

Track key decisions for future reference. Never delete entries.

---

## [2025-01-15] JWT Algorithm Choice

**Decision**: Use RS256 instead of HS256 for JWT signing

**Context**: Implementing authentication system

**Options Considered**:
1. HS256 (symmetric) - Simpler, single secret
2. RS256 (asymmetric) - Public/private key pair

**Choice**: RS256

**Reasoning**:
- Allows token verification without exposing signing key
- Better for microservices (services only need public key)
- Industry standard for production systems

**Trade-offs**:
- Slightly more complex key management
- Larger token size

**References**:
- src/auth/keys/ - Key storage
- docs/security.md - Security architecture

---

## [2025-01-14] Database Schema Approach

**Decision**: Use Drizzle ORM with PostgreSQL

**Context**: Setting up data layer

**Options Considered**:
1. Prisma - Popular, good DX
2. Drizzle - Type-safe, SQL-like
3. Raw SQL - Maximum control

**Choice**: Drizzle

**Reasoning**:
- Better TypeScript inference than Prisma
- More transparent SQL generation
- Lighter weight, faster cold starts

**References**:
- src/db/schema.ts - Schema definitions
- src/db/migrations/ - Migration files
```

---

## Code Landmarks

**`_project_specs/session/code-landmarks.md`** - Important code locations for quick reference.

```markdown
# Code Landmarks

Quick reference to important parts of the codebase.

## Entry Points
| Location | Purpose |
|----------|---------|
| src/index.ts | Main application entry |
| src/api/routes.ts | API route definitions |
| src/workers/index.ts | Background job entry |

## Core Business Logic
| Location | Purpose |
|----------|---------|
| src/core/auth/ | Authentication system |
| src/core/billing/ | Payment processing |
| src/core/workflows/ | Main workflow engine |

## Configuration
| Location | Purpose |
|----------|---------|
| src/config/index.ts | Environment config |
| src/config/features.ts | Feature flags |
| drizzle.config.ts | Database config |

## Key Patterns
| Pattern | Example Location | Notes |
|---------|------------------|-------|
| Service Layer | src/services/user.ts | Business logic encapsulation |
| Repository | src/repos/user.ts | Data access abstraction |
| Middleware | src/middleware/auth.ts | Request processing |

## Testing
| Location | Purpose |
|----------|---------|
| tests/unit/ | Unit tests |
| tests/integration/ | API tests |
| tests/e2e/ | End-to-end tests |
| tests/fixtures/ | Test data |

## Gotchas & Non-Obvious Behavior
| Location | Issue | Notes |
|----------|-------|-------|
| src/utils/date.ts | Timezone handling | Always use UTC internally |
| src/api/middleware.ts:45 | Auth bypass | Skip auth for health checks |
| src/db/pool.ts | Connection limit | Max 10 connections in dev |
```

---

## CLAUDE.md Session Rules

Add this section to CLAUDE.md:

```markdown
## Session Management

### Automatic State Updates
Update `_project_specs/session/current-state.md`:
- Every 15-20 tool calls
- After completing any todo item
- Before any significant context shift
- When encountering blockers

### Decision Logging
Log to `_project_specs/session/decisions.md` when:
- Choosing between architectural approaches
- Selecting libraries or tools
- Making security-related choices
- Deviating from standard patterns

### Context Compression Triggers
When context feels heavy or after ~50 tool calls:
1. Summarize completed work in current-state.md
2. Archive verbose exploration notes
3. Keep only essential context for next steps

### Session Handoff Format
When ending a session or hitting context limits, create handoff:
```text
## Session Handoff - [timestamp]

### Completed This Session
- [bullet points of what was done]

### Current State
- [where things stand now]

### Immediate Next Steps
1. [specific next action]
2. [following action]

### Open Questions
- [any unresolved questions]

### Files to Review First
- [list of relevant files with line numbers]
```

### Resuming Work
When starting a new session:
1. Read `_project_specs/session/current-state.md`
2. Check `_project_specs/todos/active.md`
3. Review recent entries in `decisions.md` if context needed
4. Continue from "Next Steps" in current-state.md
```

---

## Compression Strategies

### When to Compress

| Trigger | Action |
|---------|--------|
| 50+ tool calls | Summarize progress, clear exploration notes |
| Task complete | Archive task details, update landmarks |
| Context shift | Summarize previous context, start fresh notes |
| End of day | Full session handoff |

### What to Keep vs Archive

**Keep in active context:**
- Current task and immediate next steps
- Active file list with status
- Blocking issues
- Key decisions affecting current work

**Archive/summarize:**
- Exploration paths that didn't work out
- Detailed debugging traces (keep conclusion only)
- Verbose error messages (keep root cause only)
- Research notes (keep recommendations only)

### Compression Template

When compressing, use this format:

```markdown
## Compressed Context - [Topic]

**Summary**: [1-2 sentences]

**Key Findings**:
- [Bullet points of important discoveries]

**Decisions Made**:
- [Reference to decisions.md entries]

**Relevant Code**:
- [File:line references]

**Archived Details**: [Link to archive file if created]
```

---

## Session Archive

After significant work or at session end, create archive:

**`_project_specs/session/archive/YYYY-MM-DD[-topic].md`**

```markdown
# Session Archive: [Date] - [Topic]

## Summary
[Paragraph summarizing what was accomplished]

## Tasks Completed
- [TODO-XXX] Description - Done
- [TODO-YYY] Description - Done

## Key Decisions
- [Reference decisions.md entries made this session]

## Code Changes
| File | Change Type | Description |
|------|-------------|-------------|
| src/auth/login.ts | Created | Login endpoint |
| src/auth/types.ts | Modified | Added RefreshToken type |

## Tests Added
- tests/auth/login.test.ts - Login flow tests
- tests/auth/refresh.test.ts - Token refresh tests

## Open Items Carried Forward
- [Anything not finished, now in active.md]

## Session Stats
- Duration: ~3 hours
- Tool calls: ~120
- Files modified: 8
- Tests added: 12
```

---

## Integration with Todo System

### Link Todos to Sessions

In active todos, reference session context:

```markdown
## [TODO-042] Implement token refresh

**Status:** in-progress
**Session Context:** See current-state.md

### Progress Notes
- 2025-01-15: Started implementation, base structure done
- 2025-01-15: Added rotation logic, need error handling
```

### Auto-Update on Todo Completion

When completing a todo:
1. Mark todo complete in active.md
2. Update current-state.md progress
3. Log any decisions made
4. Update code-landmarks.md if new patterns introduced

---

## Quick Commands

Add to project scripts or aliases:

```bash
# Show current session state
alias session-status="cat _project_specs/session/current-state.md"

# Quick edit session state
alias session-edit="$EDITOR _project_specs/session/current-state.md"

# View recent decisions
alias decisions="tail -100 _project_specs/session/decisions.md"

# Create session archive
session-archive() {
  cp _project_specs/session/current-state.md \
     "_project_specs/session/archive/$(date +%Y-%m-%d).md"
  echo "Archived to _project_specs/session/archive/$(date +%Y-%m-%d).md"
}
```

---

## Anti-Patterns

- **No state tracking** - Flying blind, can't resume
- **Overly verbose state** - Keep it scannable, not a novel
- **Stale state files** - Update regularly or they become useless
- **Missing decisions** - Future you won't remember why
- **No code landmarks** - Wastes time re-discovering the codebase
- **Never archiving** - Session files become cluttered
- **Ignoring compression signals** - Context overload degrades performance
