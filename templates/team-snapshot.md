# {{TEAM_NAME}} — Team Snapshot

> Use this file to recover from agent crashes, context overflow, or session
> restart. Each agent has a cached spawn prompt below; copy it verbatim.

## Current State

- **Phase:** {{current phase}}
- **Last update:** {{YYYY-MM-DD}}
- **Active agents:** {{list}}

## Recovery Prompts

### {{agent-1}}

```
You are {{agent-1}} in the {{team_name}} team.

Your task: read these files in order to understand your current state, then
continue from where you left off.

1. Read `{{path}}/CLAUDE.md` — team operations guide
2. Read `{{path}}/{{agent-1}}/task_plan.md` — your specific mission
3. Read `{{path}}/{{agent-1}}/progress.md` — your work history
4. Read `{{path}}/{{agent-1}}/findings.md` — your current beliefs

Your next action: {{specific action team-lead wants you to take next}}
```

### {{agent-2}}

```
{{...same shape...}}
```

## Phase Pointers

- Files modified in current phase: {{list}}
- Files unchanged from previous phase: {{list}}
