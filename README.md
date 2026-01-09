# Kouhei's Claude Code Plugins

A curated collection of useful plugins for Claude Code to enhance your development workflow.

## 🚀 Quick Start

### Install the Marketplace

Install all plugins from this marketplace:

```
/plugin install https://github.com/KouheiOkazaki/claude-code-plugin
```

This will make all plugins in this marketplace available in your Claude Code environment.

### Install Individual Plugins

You can also install specific plugins:

```
/plugin install https://github.com/KouheiOkazaki/claude-code-plugin/plugins/git-worktree
```

## 📦 Available Plugins

### Git Worktree Manager

**Name:** `git-worktree`
**Commands:** `/git-worktree:add-git-worktree`, `/git-worktree:remove-worktree`

Easily manage git worktrees for parallel development workflows.

**Features:**
- Create worktrees with automatic navigation
- Remove worktrees with safety checks
- Standardized directory structure (`../worktrees/<name>`)
- Comprehensive error handling

**[View Plugin Documentation →](./plugins/git-worktree/README.md)**

**Required Permissions:**
```json
{
  "permissions": {
    "allow": [
      "Bash(git worktree:*)",
      "Bash(git rev-parse:*)",
      "Bash(mkdir -p ../worktrees:*)"
    ]
  }
}
```

## 🔮 Coming Soon

More plugins will be added to this marketplace:
- Code review helpers
- Project scaffolding tools
- Development workflow automation
- And more!

## 📖 Documentation

Each plugin has its own detailed documentation in its respective directory:

- [Git Worktree Manager](./plugins/git-worktree/README.md) - Git worktree management

## 🛠️ For Plugin Developers

### Adding a New Plugin

To add a new plugin to this marketplace:

1. Create a new directory in `plugins/`
2. Add your plugin files following the Claude Code plugin structure
3. Update `marketplace.json` to include your plugin
4. Add documentation to the main README

### Plugin Structure

```
plugins/
└── your-plugin/
    ├── .claude-plugin/
    │   └── plugin.json
    ├── skills/
    │   └── your-skill/
    │       └── SKILL.md
    └── README.md
```

## 📄 License

Individual plugins may have their own licenses. See each plugin's directory for details.

- Git Worktree Manager: MIT License

## 🤝 Contributing

Contributions are welcome! Please feel free to:

1. Submit issues for bugs or feature requests
2. Create pull requests for improvements
3. Suggest new plugins to add to the marketplace

## 📞 Support

- **Issues**: https://github.com/KouheiOkazaki/claude-code-plugin/issues
- **Discussions**: https://github.com/KouheiOkazaki/claude-code-plugin/discussions

## 🔗 Links

- [Claude Code Documentation](https://code.claude.com/docs)
- [Plugin Development Guide](https://code.claude.com/docs/ja/plugins)
- [GitHub Repository](https://github.com/KouheiOkazaki/claude-code-plugin)

---

Built with ❤️ for the Claude Code community
