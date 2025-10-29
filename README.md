# Claude Power User

A comprehensive skills library of proven techniques, patterns, and workflows for AI coding assistants.

Based on [Superpowers](https://github.com/obra/superpowers) by Jesse Vincent.

## What You Get

- **Testing Skills** - TDD, async testing, anti-patterns
- **Debugging Skills** - Systematic debugging, root cause tracing, verification
- **Collaboration Skills** - Brainstorming, planning, code review, parallel agents
- **Development Skills** - Git worktrees, finishing branches, subagent workflows
- **Meta Skills** - Creating, testing, and sharing skills

Plus:
- **Slash Commands** - `/poweruser:brainstorm`, `/poweruser:write-plan`, `/poweruser:execute-plan`
- **Automatic Integration** - Skills activate automatically when relevant
- **Consistent Workflows** - Systematic approaches to common engineering tasks

## Learn More

Read the introduction: [Superpowers for Claude Code](https://blog.fsck.com/2025/10/09/superpowers/)

## Installation

### Claude Code (via Plugin Marketplace)

```bash
# In Claude Code
/plugin marketplace add Hacker0x01/claude-power-user
/plugin install claude-power-user@claude-power-user
```

### Verify Installation

```bash
# Check that commands appear
/help

# Should see:
# /poweruser:brainstorm - Interactive design refinement
# /poweruser:write-plan - Create implementation plan
# /poweruser:execute-plan - Execute plan in batches
```

### Codex (Experimental)

**Note:** Codex support is experimental and may require refinement based on user feedback.

Tell Codex to fetch https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md and follow the instructions.

## Quick Start

### Using Slash Commands

**Brainstorm a design:**
```
/poweruser:brainstorm
```

**Create an implementation plan:**
```
/poweruser:write-plan
```

**Execute the plan:**
```
/poweruser:execute-plan
```

### Automatic Skill Activation

Skills activate automatically when relevant. For example:
- `test-driven-development` activates when implementing features
- `systematic-debugging` activates when debugging issues
- `verification-before-completion` activates before claiming work is done

## What's Inside

### Skills Library

**Testing** (`skills/testing/`)
- **test-driven-development** - RED-GREEN-REFACTOR cycle
- **condition-based-waiting** - Async test patterns
- **testing-anti-patterns** - Common pitfalls to avoid

**Debugging** (`skills/debugging/`)
- **systematic-debugging** - 4-phase root cause process
- **root-cause-tracing** - Find the real problem
- **verification-before-completion** - Ensure it's actually fixed
- **defense-in-depth** - Multiple validation layers

**Collaboration** (`skills/collaboration/`)
- **brainstorming** - Socratic design refinement
- **writing-plans** - Detailed implementation plans
- **executing-plans** - Batch execution with checkpoints
- **dispatching-parallel-agents** - Concurrent subagent workflows
- **requesting-code-review** - Pre-review checklist
- **receiving-code-review** - Responding to feedback
- **using-git-worktrees** - Parallel development branches
- **finishing-a-development-branch** - Merge/PR decision workflow
- **subagent-driven-development** - Fast iteration with quality gates

**Meta** (`skills/meta/`)
- **writing-skills** - Create new skills following best practices
- **sharing-skills** - Contribute skills back via branch and PR
- **testing-skills-with-subagents** - Validate skill quality
- **using-superpowers** - Introduction to the skills system

### Commands

All commands are thin wrappers that activate the corresponding skill:

- **brainstorm.md** - Activates the `brainstorming` skill
- **write-plan.md** - Activates the `writing-plans` skill
- **execute-plan.md** - Activates the `executing-plans` skill

## How It Works

1. **SessionStart Hook** - Loads the `using-superpowers` skill at session start
2. **Skills System** - Uses Claude Code's first-party skills system
3. **Automatic Discovery** - Claude finds and uses relevant skills for your task
4. **Mandatory Workflows** - When a skill exists for your task, using it becomes required

## Philosophy

- **Test-Driven Development** - Write tests first, always
- **Systematic over ad-hoc** - Process over guessing
- **Complexity reduction** - Simplicity as primary goal
- **Evidence over claims** - Verify before declaring success
- **Domain over implementation** - Work at problem level, not solution level

## Contributing

Skills live directly in this repository. To contribute:

1. Fork the repository
2. Create a branch for your skill
3. Follow the `writing-skills` skill for creating new skills
4. Use the `testing-skills-with-subagents` skill to validate quality
5. Submit a PR

See `skills/meta/writing-skills/SKILL.md` for the complete guide.

## Updating

Skills update automatically when you update the plugin:

```bash
/plugin update claude-power-user
```

## License

MIT License - see LICENSE file for details

## Support

- **Issues**: https://github.com/Hacker0x01/claude-power-user/issues
- **Original Superpowers**: https://github.com/obra/superpowers (by Jesse Vincent)
