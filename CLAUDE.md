# Conductor Plugin for Claude Code

**Measure twice, code once.**

Conductor is a Claude Code plugin that enables **Context-Driven Development**. It turns Claude Code into a proactive project manager that follows a strict protocol to specify, plan, and implement software features and bug fixes.

> **Note**: This plugin follows the official [Claude Code plugin structure](https://github.com/anthropics/claude-code) and [Agent Skills format](https://github.com/anthropics/skills).

## Features

- **Plan before you build**: Create specs and plans that guide Claude for new and existing codebases
- **Maintain context**: Ensure AI follows style guides, tech stack choices, and product goals
- **Iterate safely**: Review plans before code is written, keeping you firmly in the loop
- **Work as a team**: Set project-level context that becomes a shared foundation
- **Build on existing projects**: Intelligent initialization for both new and existing projects
- **Smart revert**: Git-aware revert that understands logical units of work

## Installation

1. **Clone or copy this plugin to your local Claude plugins directory:**

```bash
# Navigate to your Claude plugins directory
cd ~/.claude/plugins

# Clone the repository
git clone https://github.com/nsafouane/ClaudeCodeConductor.git conductor
```

2. **Restart Claude Code** to load the plugin.

## Quick Start

### 1. Set Up Your Project (Run Once)

```bash
/conductor:setup
```

This will guide you through:
- Detecting if your project is new or existing
- Defining your product, tech stack, and workflow
- Selecting code style guides
- Creating your first track

### 2. Create a New Track

```bash
/conductor:track "Add user authentication"
```

This creates:
- A detailed **specification** (spec.md)
- An implementation **plan** (plan.md) with phases and tasks

### 3. Implement the Track

```bash
/conductor:implement
```

Claude will:
- Execute tasks one-by-one following TDD workflow
- Commit changes with track metadata
- Update the plan as it progresses
- Ask for verification between phases

### 4. Check Status

```bash
/conductor:status
```

See progress on all tracks and current tasks.

### 5. Revert if Needed

```bash
/conductor:revert
```

Safely revert tracks, phases, or tasks with git awareness.

## Plugin Architecture

### File Format

This plugin uses the official Claude Code formats:

**Skills** (`SKILL.md` files):
- YAML frontmatter with `name` and `description` (required)
- Body with markdown instructions
- Format based on [anthropics/skills](https://github.com/anthropics/skills)

**Commands** (`.md` files):
- YAML frontmatter with `description` (required)
- Optional: `allowed-tools`, `argument-hint`
- Body with markdown instructions
- Format based on [anthropics/claude-code plugins](https://github.com/anthropics/claude-code)

### Skills (Auto-Invoked)

Conductor includes 3 skills that automatically activate based on context:

1. **conductor-context**: Loads project context when needed
2. **conductor-workflow**: Enforces TDD task lifecycle during implementation
3. **conductor-docs**: Fetches up-to-date documentation using Context7 MCP

### Commands (User-Invoked)

| Command | Description |
|---------|-------------|
| `/conductor:setup` | Initialize Conductor in your project |
| `/conductor:track` | Create a new feature or bug fix track |
| `/conductor:implement` | Execute the implementation plan |
| `/conductor:status` | Show project progress |
| `/conductor:revert` | Revert work using git |

## Generated Project Structure

```
your-project/
├── conductor/
│   ├── product.md              # Project context and goals
│   ├── tech-stack.md           # Technology stack definition
│   ├── workflow.md             # Development workflow preferences
│   ├── styleguides/            # Code style guides
│   ├── tracks.md               # Registry of all tracks
│   └── tracks/
│       └── track_20241230_001/
│           ├── spec.md         # Track specification
│           ├── plan.md         # Implementation plan
│           └── metadata.json   # Track metadata
```

## Workflow

Conductor follows a strict development workflow:

1. **Pre-work**: Check git, find next task, mark in-progress
2. **TDD Cycle** (if enabled):
   - Write test first (should fail)
   - Implement minimal code
   - Verify test passes
   - Refactor if needed
3. **Commit**: Commit changes with track metadata
4. **Update Plan**: Mark task complete with commit SHA
5. **Verify**: Run full test suite at phase end
6. **Checkpoint**: Get user approval between phases

## Configuration

### Workflow Settings

During setup, you can configure:
- **TDD mode**: Test-Driven Development (recommended: Yes)
- **Commit cadence**: After each task or after each phase
- **Test coverage**: Target percentage (default: 80%)

### Style Guides

Conductor includes templates for:
- Python (PEP 8, type hints)
- TypeScript (ES6+, strict mode)
- JavaScript (ES6+)
- Go
- C#
- Dart
- HTML/CSS

## Best Practices

1. **Review plans before implementing** - Always review the generated plan.md before starting implementation
2. **Commit frequently** - Each task should result in a commit with clear metadata
3. **Verify phases** - Test manually between phases to catch issues early
4. **Keep context updated** - Update product.md and tech-stack.md as your project evolves
5. **Use revert carefully** - The revert command is powerful but requires confirmation

## Token Efficiency

Conductor is designed to be token-efficient:
- Skills only load relevant files when needed
- Context is cached during conversations
- File contents are summarized, not reproduced in full

## Example Workflow

```bash
# 1. Initialize project
/conductor:setup
→ Define product, tech stack, workflow

# 2. Create first feature
/conductor:track "Add login form"
→ Generates spec.md and plan.md

# 3. Review the plan
# (Manually review conductor/tracks/track_20241230_001/plan.md)

# 4. Implement
/conductor:implement
→ Executes tasks one-by-one
→ Commits with metadata
→ Asks for verification between phases

# 5. Check progress
/conductor:status

# 6. Create next feature
/conductor:track "Add password reset"
```

## Troubleshooting

### "Conductor not initialized"
Run `/conductor:setup` first to initialize the plugin in your project.

### "Track not found"
Run `/conductor:status` to see available tracks, or create a new one with `/conductor:track`.

### Implementation stuck
- Check git status for uncommitted changes
- Read the current plan.md to see what's pending
- Use `/conductor:revert` if you need to go back

## Contributing

Contributions are welcome! Please read CONTRIBUTING.md for guidelines.

## License

Apache License 2.0 - See LICENSE file for details

## Acknowledgments

Based on the original Conductor extension for Gemini CLI.
Adapted for Claude Code with Skills and Commands architecture.
