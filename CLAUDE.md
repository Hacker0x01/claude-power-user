# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **Claude Power User** plugin for Claude Code - a comprehensive skills library that provides proven techniques, patterns, and workflows for software development. The plugin uses Claude Code's native skills system and delivers skills through a SessionStart hook.

Based on [Superpowers](https://github.com/obra/superpowers) by Jesse Vincent.

Current version: 3.2.2

## Architecture

### Plugin Structure

This is a Claude Code plugin with the following components:

- **`.claude-plugin/plugin.json`** - Plugin metadata (name, version, author)
- **`.claude-plugin/marketplace.json`** - Marketplace configuration (references this directory as source)
- **`hooks/`** - SessionStart hook that injects the `using-superpowers` skill context
- **`commands/`** - Three slash commands (thin wrappers that activate corresponding skills)
- **`skills/`** - Core skills library embedded directly in the plugin
- **`agents/`** - Custom agent definitions (currently `code-reviewer`)
- **`lib/`** - Legacy utilities (no longer actively used)

### How the Plugin Works

1. **SessionStart Hook** (`hooks/session-start.sh`):
   - Executes when Claude Code starts a session (startup|resume|clear|compact)
   - Reads `skills/using-superpowers/SKILL.md`
   - Injects content as `<EXTREMELY_IMPORTANT>` context into Claude's session
   - Checks for legacy skills directory and warns users to migrate

2. **Skills System**:
   - Skills are markdown files with frontmatter: `---\nname: skill-name\ndescription: ...\n---`
   - Located in categorized directories under `skills/`
   - Claude Code's native skills system discovers and loads them automatically
   - The `using-superpowers` skill establishes mandatory workflows for finding and using other skills

3. **Commands**:
   - Simple markdown files that activate corresponding skills
   - `brainstorm.md` → activates `brainstorming` skill
   - `write-plan.md` → activates `writing-plans` skill
   - `execute-plan.md` → activates `executing-plans` skill

4. **Agents**:
   - `code-reviewer.md` - Senior code reviewer agent for validating completed work
   - Referenced by skills as `poweruser:code-reviewer`

## Skills Organization

Skills are organized by category:

- **Core Meta Skills**:
  - `using-superpowers/` - Introduction and mandatory workflows (injected at SessionStart)
  - `writing-skills/` - Guide for creating new skills
  - `testing-skills-with-subagents/` - Validate skill quality
  - `sharing-skills/` - Contribute skills via branch and PR

- **Development Workflows**:
  - `brainstorming/` - Socratic design refinement with 4 phases
  - `writing-plans/` - Detailed implementation planning
  - `executing-plans/` - Batch execution with review checkpoints
  - `using-git-worktrees/` - Parallel development branches
  - `finishing-a-development-branch/` - Merge/PR decision workflow

- **Testing & Quality**:
  - `test-driven-development/` - RED-GREEN-REFACTOR cycle
  - `testing-anti-patterns/` - Common pitfalls to avoid
  - `condition-based-waiting/` - Async test patterns
  - `verification-before-completion/` - Ensure fixes work
  - `defense-in-depth/` - Multiple validation layers

- **Debugging**:
  - `systematic-debugging/` - 4-phase root cause process
  - `root-cause-tracing/` - Find the real problem (includes `find-polluter.sh`)

- **Collaboration**:
  - `dispatching-parallel-agents/` - Concurrent subagent workflows
  - `requesting-code-review/` - Pre-review checklist
  - `receiving-code-review/` - Responding to feedback
  - `subagent-driven-development/` - Fast iteration with quality gates

- **Commands** (special category):
  - `commands/` - Skill activation patterns and command structure guide

## Key Files

### Plugin Configuration
- `.claude-plugin/plugin.json` - Plugin definition (v3.2.2)
- `.claude-plugin/marketplace.json` - Marketplace metadata (superpowers-dev)

### Hook System
- `hooks/session-start.sh` - Bash script that injects `using-superpowers` context
- `hooks/hooks.json` - Hook registration (SessionStart matcher)

### Commands (Slash Commands)
- `commands/brainstorm.md` - `/poweruser:brainstorm`
- `commands/write-plan.md` - `/poweruser:write-plan`
- `commands/execute-plan.md` - `/poweruser:execute-plan`

### Agents
- `agents/code-reviewer.md` - Code review agent with 6-phase review protocol

### Documentation
- `README.md` - User-facing documentation and installation instructions
- `RELEASE-NOTES.md` - Version history and changelog

## Development Workflow

### No Build System

This plugin has no build process, compilation, or dependencies. All files are directly used by Claude Code:

- Edit files in place
- Changes take effect immediately on next session/hook trigger
- Test by triggering the relevant event (SessionStart, slash command, etc.)

### Testing Changes Locally

1. **Hook changes**: Start a new Claude Code conversation to trigger SessionStart
2. **Command changes**: Invoke the command (e.g., `/poweruser:brainstorm`)
3. **Skill changes**: Skills are loaded by Claude Code's native system on session start

### Releasing a New Version

1. Update version in `.claude-plugin/plugin.json`
2. Update version in `.claude-plugin/marketplace.json` (plugins array)
3. Add entry to `RELEASE-NOTES.md` with changes
4. Commit with message: `Release vX.Y.Z: <summary>`
5. Tag the commit: `git tag vX.Y.Z`
6. Push: `git push && git push --tags`

Users update with: `/plugin update superpowers`

### Creating a New Skill

1. Create directory: `skills/skill-name/`
2. Create `SKILL.md` with frontmatter:
   ```markdown
   ---
   name: skill-name
   description: Brief description for skill discovery
   ---

   # Skill content here
   ```
3. Follow the `writing-skills` skill for structure and best practices
4. Test using the `testing-skills-with-subagents` skill
5. Update version and release notes

### Modifying the SessionStart Hook

The hook is critical to the plugin's functionality:

1. Edit `hooks/session-start.sh`
2. Hook outputs JSON with `hookSpecificOutput.additionalContext`
3. Context is injected as `<EXTREMELY_IMPORTANT>` tags
4. Test by starting a new conversation (triggers SessionStart)

**Warning**: The hook uses bash string escaping for JSON. Be careful with quotes and newlines.

## Skills Philosophy

This plugin embodies these principles:

- **Mandatory workflows** - If a skill exists for your task, you must use it
- **Test-Driven Development** - Write tests first, always
- **Systematic over ad-hoc** - Process over guessing
- **Complexity reduction** - Simplicity as primary goal
- **Evidence over claims** - Verify before declaring success
- **Domain over implementation** - Work at problem level, not solution level

## Important Notes

### Legacy System

- **Old location**: Previous versions stored skills at `~/.config/superpowers/skills/`
- **Old system**: Used `lib/initialize-skills.sh` to clone/update external skills repo
- **Current system**: Skills are embedded in plugin, managed by Claude Code's native system
- **Migration**: SessionStart hook warns users if legacy directory exists

### Repository References

The plugin references multiple repositories:

- **Homepage**: `https://github.com/Hacker0x01/claude-power-user` (this plugin)
- **Original Superpowers**: `https://github.com/obra/superpowers` (by Jesse Vincent)
- **Legacy skills repo**: `https://github.com/HackerOx01/superpowers-skills` (archived pattern)

### Marketplace Distribution

This directory serves dual purpose:
1. Plugin source code (the actual plugin)
2. Development marketplace (`.claude-plugin/marketplace.json` points to `./`)

Users install via:
```
/plugin marketplace add Hacker0x01/claude-power-user
/plugin install claude-power-user@claude-power-user
```

## Common Tasks

### Update using-superpowers skill

This is the most critical skill since it's injected into every session:

1. Edit `skills/using-superpowers/SKILL.md`
2. Test by starting new Claude Code conversation
3. Verify the injected context appears correctly
4. Update version and release notes

### Add a new command

1. Create `commands/new-command.md`
2. Add frontmatter with description
3. Content should be: "Use the [skill-name] skill exactly as written"
4. Create corresponding skill in `skills/skill-name/`
5. Test with `/poweruser:new-command`

### Modify the code-reviewer agent

1. Edit `agents/code-reviewer.md`
2. Update frontmatter (name, description, model)
3. Modify review protocol and instructions
4. Test by having a skill invoke `poweruser:code-reviewer`
