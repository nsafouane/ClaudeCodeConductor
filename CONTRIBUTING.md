# Contributing to Conductor for Claude Code

We'd love your contributions! This is a fork of the original [Conductor for Gemini CLI](https://github.com/gemini-cli-extensions/conductor), adapted for [Claude Code](https://github.com/anthropics/claude-code).

## How to Contribute

### Reporting Bugs

1. Check existing [GitHub Issues](https://github.com/nsafouane/ClaudeCodeConductor/issues) first
2. Create a new issue with:
   - Clear title and description
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment info (OS, Claude Code version)
   - Relevant logs or screenshots

### Suggesting Features

1. Check existing issues and discussions
2. Create a feature request with:
   - Clear use case
   - Proposed solution (if you have one)
   - How it aligns with Conductor's philosophy

### Pull Requests

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Make your changes** following the guidelines below
4. **Test thoroughly**
5. **Commit with clear messages**:
   ```
   feat(commands): Add new command for X
   fix(skills): Correct context loading logic
   docs(readme): Update installation instructions
   ```
6. **Push and create PR**

## Development Guidelines

### Code Style

- **Skills**: Follow the [Anthropic Skills format](https://github.com/anthropics/skills)
  - YAML frontmatter with `name` and `description`
  - Clear, concise instructions
  - Token-efficient (avoid verbosity)
  - Use imperative/infinitive form

- **Commands**: Follow Claude Code plugin conventions
  - YAML frontmatter with `description` (required)
  - Add `allowed-tools` for security
  - Add `argument-hint` for better UX
  - Clear step-by-step instructions

### Testing

- Test commands in real projects
- Verify Skills trigger at appropriate times
- Ensure `allowed-tools` restrictions work correctly
- Test with both greenfield and brownfield projects

### Philosophy

Conductor's core philosophy: **"Measure twice, code once"**

When contributing:
- **Preserve context-driven approach**: Context should drive development
- **Maintain simplicity**: Avoid over-engineering
- **Token efficiency**: Skills and commands should be concise
- **Backward compatibility**: Don't break existing `conductor/` directories

### Project Structure

```
ClaudeCodeConductor/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata
├── skills/                  # Auto-invoked skills
│   ├── conductor-context/
│   ├── conductor-workflow/
│   └── conductor-docs/
├── commands/                # User-invoked commands
│   ├── setup.md
│   ├── track.md
│   ├── implement.md
│   ├── status.md
│   └── revert.md
└── templates/               # Project templates
    ├── workflow.md
    └── code_styleguides/
```

## Specific Areas

### Adding New Skills

1. Create directory: `skills/your-skill/SKILL.md`
2. Add YAML frontmatter:
   ```yaml
   ---
   name: your-skill
   description: When to use this skill
   ---
   ```
3. Write concise instructions (<500 lines preferred)
4. Test auto-invocation triggers

### Adding New Commands

1. Create file: `commands/your-command.md`
2. Add YAML frontmatter:
   ```yaml
   ---
   description: What this command does
   argument-hint: Optional argument description
   allowed-tools: Tool1(*), Tool2(*)
   ---
   ```
3. Write step-by-step instructions
4. Test with and without arguments

### Updating Templates

Templates in `templates/` are copied to user projects:
- `workflow.md`: Development workflow template
- `code_styleguides/*.md`: Language-specific style guides

Changes should maintain backward compatibility.

## Documentation

- **README.md**: User-facing documentation
- **CLAUDE.md**: Plugin-specific documentation
- Code comments: Only when logic isn't self-evident

## Community

- Be respectful and constructive
- Welcome newcomers and help them learn
- Focus on what's best for the community
- Show empathy towards other community members

## Acknowledgments

This fork maintains the same **Apache License 2.0** as the original Conductor project.

By contributing, you agree that your contributions will be licensed under the Apache License 2.0.

---

## Resources

- [Original Conductor Contribution Guide](https://github.com/gemini-cli-extensions/conductor/blob/main/CONTRIBUTING.md)
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Anthropic Skills Guide](https://github.com/anthropics/skills)
