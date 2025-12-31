# Conductor Plugin for Claude Code

**Measure twice, code once.**

> **🔄 Fork of** [gemini-cli-extensions/conductor](https://github.com/gemini-cli-extensions/conductor) • **Converted to** [Claude Code](https://github.com/anthropics/claude-code) plugin architecture

Conductor is a **Claude Code plugin** that enables **Context-Driven Development**. It transforms Claude Code into a proactive project manager that follows a strict protocol to specify, plan, and implement software features and bug fixes.

---

## 🎯 What is Conductor?

Conductor ensures a consistent, high-quality lifecycle for every task: **Context → Spec & Plan → Implement**.

The philosophy is simple: **control your code**. By treating context as a managed artifact alongside your code, you transform your repository into a single source of truth that drives every AI interaction with deep, persistent project awareness.

---

## 🔄 From Gemini CLI to Claude Code

This project is a **fork and conversion** of the original [Conductor extension for Gemini CLI](https://github.com/gemini-cli-extensions/conductor). It has been completely re-architected to use Claude Code's plugin system with Skills and Commands.

### What Changed in the Conversion

| Aspect | Gemini CLI (Original) | Claude Code (This Fork) |
|--------|----------------------|-------------------------|
| **Architecture** | TOML-based prompt commands | Skills (auto-invoked) + Commands (user-invoked) |
| **Format** | `.toml` files | `.md` files with YAML frontmatter |
| **Invocation** | `/conductor:command` | `/conductor:command` (same syntax) |
| **Context Loading** | Manual prompts | Auto-invoked Skills |
| **Tool Permissions** | Not specified | `allowed-tools` in frontmatter |
| **Documentation** | GEMINI.md | CLAUDE.md + inline docs |
| **Standards Compliance** | Gemini CLI spec | [Anthropic Skills](https://github.com/anthropics/skills) & [Plugins](https://github.com/anthropics/claude-code) |

### Key Architectural Changes

1. **Skills System** (New)
   - `conductor-context`: Auto-loads project context when needed
   - `conductor-workflow`: Enforces TDD task lifecycle during implementation
   - `conductor-docs`: Fetches up-to-date docs via Context7 MCP

2. **Commands Enhancement**
   - Added `allowed-tools` frontmatter for security
   - Added `argument-hint` for better UX
   - Markdown-based instead of TOML

3. **Removed Features**
   - Gemini-specific configuration (`gemini-extension.json`)
   - Product guidelines (simplified to focus on essentials)

4. **Preserved Features**
   - All core workflow functionality
   - Track-based development
   - Git-aware revert
   - Status tracking

---

## Features

- **Plan before you build**: Create specs and plans that guide Claude for new and existing codebases
- **Maintain context**: Ensure AI follows style guides, tech stack choices, and product goals
- **Iterate safely**: Review plans before code is written, keeping you firmly in the loop
- **Work as a team**: Set project-level context that becomes a shared foundation
- **Build on existing projects**: Intelligent initialization for both new (Greenfield) and existing (Brownfield) projects
- **Smart revert**: Git-aware revert that understands logical units of work (tracks, phases, tasks)
- **Token-efficient**: Skills load only when needed, keeping context minimal

---

## Installation

### Prerequisites

- [Claude Code](https://github.com/anthropics/claude-code) installed
- [Context7 MCP server](https://github.com/modelcontextprotocol/servers) configured (optional, for `conductor-docs` skill)

### Quick Installation (Recommended)

The easiest way to install Conductor is directly from the GitHub repository:

```bash
# Install directly from GitHub
/plugin install https://github.com/nsafouane/ClaudeCodeConductor.git

# Restart Claude Code to load the plugin
```

### Alternative Installation Methods

#### Method 1: Manual Skills Installation

For simple plugins that only need skills:

#### Method 2: Manual Skills Installation

For simple plugins that only need skills:

```bash
# Create skills directory
mkdir -p .claude/skills/conductor

# Download SKILL.md files directly
curl -o .claude/skills/conductor/context/SKILL.md https://raw.githubusercontent.com/nsafouane/ClaudeCodeConductor/main/skills/conductor-context/SKILL.md
curl -o .claude/skills/conductor/docs/SKILL.md https://raw.githubusercontent.com/nsafouane/ClaudeCodeConductor/main/skills/conductor-docs/SKILL.md
curl -o .claude/skills/conductor/workflow/SKILL.md https://raw.githubusercontent.com/nsafouane/ClaudeCodeConductor/main/skills/conductor-workflow/SKILL.md
```

#### Method 3: Project-Level Configuration

Create `.claude/settings.json` for automatic plugin installation when team members open the project:

```json
{
  "extraKnownMarketplaces": {
    "anthropics": {
      "source": {
        "source": "github",
        "repo": "anthropics/claude-code"
      }
    },
    "community": {
      "source": {
        "source": "github",
        "repo": "jeremylongshore/claude-code-plugins-plus"
      }
    }
  },
  "plugins": {
    "auto_install": [
      "conductor"
    ]
  }
}
```

When team members trust the repository folder, Claude Code will automatically install the plugin.

### Publishing to a Marketplace

To enable the `/plugin install conductor` command (without specifying the full Git URL), you need to publish this plugin to a marketplace.

#### Option 1: Submit to Existing Marketplace

1. Fork or create a pull request to an existing marketplace:
   - [jeremylongshore/claude-code-plugins-plus](https://github.com/jeremylongshore/claude-code-plugins-plus) - Community marketplace with 185+ plugins
   - [EveryInc/every-marketplace](https://github.com/EveryInc/every-marketplace) - DevOps & productivity focus
   - [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) - Official Anthropic marketplace (requires approval)

2. Add this plugin to the marketplace's `.claude-plugin/marketplace.json`:
   ```json
   {
     "name": "conductor",
     "source": "./plugins/conductor",
     "description": "Context-driven development workflow for systematic feature implementation"
   }
   ```

3. Submit a pull request to the marketplace repository

#### Option 2: Create Your Own Marketplace

1. Create a new GitHub repository for your marketplace (e.g., `your-org/claude-plugins`)
2. Create `.claude-plugin/marketplace.json`:
   ```json
   {
     "name": "your-marketplace",
     "owner": {
       "name": "Your Team",
       "email": "team@example.com"
     },
     "plugins": [
       {
         "name": "conductor",
         "source": "./plugins/conductor",
         "description": "Context-driven development workflow for systematic feature implementation"
       }
     ]
   }
   ```
3. Commit and push to GitHub

4. Share marketplace with your team:
   ```bash
   /plugin marketplace add your-org/claude-plugins
   /plugin install conductor@your-marketplace
   ```

For complete details on creating marketplaces, see the [official marketplace documentation](https://code.claude.com/docs/en/plugins).

### Verify Installation

After installation, verify the plugin is loaded by checking available commands:

```bash
/help
```

You should see `/conductor:setup`, `/conductor:track`, `/conductor:implement`, `/conductor:status`, and `/conductor:revert` in the list.

### Plugin Management Commands

```bash
# List installed plugins
/plugin list

# Show plugin details
/plugin info conductor

# Update all plugins
/plugin update all

# Remove a plugin
/plugin remove conductor

# List available marketplaces
/plugin marketplace list
```

### Troubleshooting

#### Plugin not appearing after installation

If plugin doesn't appear in `/help` after installation:

1. **Restart Claude Code** - Plugins are loaded on startup
2. **Check plugin installation**:
   ```bash
   /plugin list
   ```
3. **Verify marketplace is added**:
   ```bash
   /plugin marketplace list
   ```
4. **Check for errors** - Review Claude Code startup logs for plugin loading errors

#### Installation fails with "plugin not found"

If you get a "plugin not found" error:

1. **Verify marketplace name** - Use correct marketplace slug:
   ```bash
   /plugin marketplace list
   ```
2. **Check plugin name** - Use exact plugin name from marketplace:
   ```bash
   /plugin marketplace show <marketplace-name>
   ```
3. **Try direct installation**:
   ```bash
   /plugin install https://github.com/nsafouane/ClaudeCodeConductor.git
   ```

#### Commands not working

If Conductor commands don't work:

1. **Verify plugin is loaded**:
   ```bash
   /plugin list
   ```
2. **Check command syntax** - Use `/conductor:command` format
3. **Restart Claude Code** after installation

#### MCP server issues (conductor-docs skill)

If `conductor-docs` skill has issues:

1. **Verify Context7 MCP is installed**:
   ```bash
   /mcp list
   ```
2. **Install Context7 MCP server**:
   ```bash
   claude mcp add context7 --scope user -- npx -y @upstash/context7-mcp
   ```
3. **Restart Claude Code** to load MCP servers

---

## Usage

### 1. Set Up the Project (Run Once)

```bash
/conductor:setup
```

Conductor will guide you through:
- Detecting if your project is new or existing
- Defining your product, tech stack, and workflow
- Selecting code style guides
- Creating your first track

**Generated Artifacts:**
- `conductor/product.md` - Project context and goals
- `conductor/tech-stack.md` - Technology stack definition
- `conductor/workflow.md` - Development workflow preferences
- `conductor/styleguides/` - Code style guides
- `conductor/tracks.md` - Registry of all tracks

### 2. Create a New Track

```bash
/conductor:track "Add user authentication"
```

This creates:
- **Spec** (`spec.md`): Detailed requirements
- **Plan** (`plan.md`): Implementation plan with phases and tasks

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

---

## Commands Reference

| Command | Description | Artifacts |
|:---|:---|:---|
| `/conductor:setup` | Initialize Conductor in your project | `conductor/` structure |
| `/conductor:track` | Create a new feature or bug fix track | `conductor/tracks/<id>/spec.md`<br>`conductor/tracks/<id>/plan.md` |
| `/conductor:implement` | Execute the implementation plan | `conductor/tracks/<id>/plan.md` |
| `/conductor:status` | Show project progress | Reads `conductor/tracks.md` |
| `/conductor:revert` | Revert work using git | Reverts git history |

---

## Architecture

### Skills (Auto-Invoked)

| Skill | Purpose |
|:---|:---|
| `conductor-context` | Loads project context when needed |
| `conductor-workflow` | Enforces TDD task lifecycle |
| `conductor-docs` | Fetches up-to-date documentation via Context7 MCP |

### Workflow

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

---

## Generated Project Structure

```
your-project/
├── conductor/
│   ├── product.md              # Project context
│   ├── tech-stack.md           # Technology stack
│   ├── workflow.md             # Development workflow
│   ├── styleguides/            # Code style guides
│   ├── tracks.md               # Track registry
│   └── tracks/
│       └── track_20241230_001/
│           ├── spec.md         # Requirements
│           ├── plan.md         # Implementation plan
│           └── metadata.json   # Track metadata
```

---

## Migration from Gemini CLI

If you're migrating from the original Gemini CLI extension:

1. **Your existing `conductor/` directory is 100% compatible** - no changes needed!
2. **Command syntax is identical** - same `/conductor:setup`, `/conductor:track`, etc.
3. **Enhanced features**:
   - Skills auto-load context (no manual prompting needed)
   - Better tool permission control
   - Improved documentation integration

---

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

When contributing, keep in mind:
- This follows Claude Code's official plugin structure
- Skills use the [Anthropic Skills format](https://github.com/anthropics/skills)
- Maintain compatibility with the original Conductor philosophy

---

## Acknowledgments

- **Original Project**: [gemini-cli-extensions/conductor](https://github.com/gemini-cli-extensions/conductor)
- **Claude Code**: [anthropics/claude-code](https://github.com/anthropics/claude-code)
- **Agent Skills**: [anthropics/skills](https://github.com/anthropics/skills)

This fork maintains the same **Apache License 2.0** as the original project.

---

## License

[Apache License 2.0](LICENSE) - Same as the original Conductor extension

---

## Resources

- [Official Claude Code Repo](https://github.com/anthropics/claude-code)
- [Official Skills Repo](https://github.com/anthropics/skills)
- [Original Conductor for Gemini CLI](https://github.com/gemini-cli-extensions/conductor)
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
