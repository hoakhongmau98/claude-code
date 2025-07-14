# Claude Code - Project Overview & Developer Onboarding

## Project Skeleton Structure

```
claude-code/
├── .devcontainer/                     # Development container configuration
│   ├── devcontainer.json             # VS Code devcontainer configuration
│   ├── Dockerfile                    # Development environment Docker image
│   └── init-firewall.sh              # Firewall initialization script
├── .github/                          # GitHub configuration
│   ├── workflows/                    # GitHub Actions workflows
│   │   ├── claude.yml                # Claude Code GitHub integration
│   │   └── claude-issue-triage.yml   # Issue triage automation
│   └── ISSUE_TEMPLATE/               # Issue templates
├── .vscode/                          # VS Code configuration
├── Script/                           # Utility scripts
│   └── run_devcontainer_claude_code.ps1  # PowerShell script for devcontainer setup
├── examples/                         # Example configurations and hooks
│   └── hooks/                        # Hook examples
│       └── bash_command_validator_example.py  # Bash command validation hook
├── .git/                             # Git repository data
├── CHANGELOG.md                      # Release notes and version history
├── LICENSE.md                        # Project license
├── README.md                         # Project documentation
├── SECURITY.md                       # Security policy
├── demo.gif                          # Demo animation
├── .gitattributes                    # Git attributes configuration
└── PROJECT_OVERVIEW.md               # This file
```

## What is Claude Code?

Claude Code is an agentic coding tool developed by Anthropic that lives in your terminal, understands your codebase, and helps you code faster by executing routine tasks, explaining complex code, and handling git workflows — all through natural language commands.

### Key Features
- **Natural Language Commands**: Interact with your codebase using plain English
- **Terminal Integration**: Works directly in your terminal environment
- **Codebase Understanding**: Analyzes and understands your project structure
- **Git Workflow Support**: Handles version control operations
- **IDE Integration**: Can be used in terminal, IDE, or tagged on GitHub
- **Hook System**: Extensible through custom hooks for command validation and modification

## Developer Onboarding

### Prerequisites

1. **Node.js 18+** - Required for running Claude Code
2. **Git** - For version control operations
3. **Docker or Podman** - For development container (optional but recommended)
4. **PowerShell** - For Windows development setup (Windows only)

### Installation

#### Global Installation
```bash
npm install -g @anthropic-ai/claude-code
```

#### Development Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/anthropics/claude-code.git
   cd claude-code
   ```

2. **Option A: Using DevContainer (Recommended)**
   
   **For Docker:**
   ```bash
   # From project root
   ./Script/run_devcontainer_claude_code.ps1 -Backend docker
   ```
   
   **For Podman:**
   ```bash
   # From project root
   ./Script/run_devcontainer_claude_code.ps1 -Backend podman
   ```

3. **Option B: Manual Setup**
   ```bash
   # Install dependencies
   npm install
   
   # Run Claude Code
   claude
   ```

### Development Environment

#### DevContainer Configuration
The project uses a sophisticated devcontainer setup that includes:

- **Base Image**: Node.js 20
- **Shell**: zsh (default)
- **Tools**: git, fzf, gh (GitHub CLI), iptables, ipset, jq
- **VS Code Extensions**: ESLint, Prettier, GitLens
- **Volumes**: 
  - Command history persistence
  - Claude configuration persistence
- **Network**: Enhanced networking capabilities with NET_ADMIN and NET_RAW caps

#### Key Environment Variables
- `NODE_OPTIONS`: Set to `--max-old-space-size=4096` for memory optimization
- `CLAUDE_CONFIG_DIR`: Points to `/home/node/.claude` for configuration storage
- `POWERLEVEL9K_DISABLE_GITSTATUS`: Disabled for performance

### Project Structure Details

#### Core Components

1. **DevContainer Setup** (`.devcontainer/`)
   - Provides consistent development environment
   - Includes firewall initialization for network features
   - Configured for both Docker and Podman backends

2. **Automation Scripts** (`Script/`)
   - PowerShell script for automated devcontainer setup
   - Handles backend selection (Docker/Podman)
   - Manages container lifecycle and connection

3. **Hook System** (`examples/hooks/`)
   - Extensible system for command validation and modification
   - Example: Bash command validator that suggests `rg` over `grep`
   - Supports PreToolUse, PreCompact, and Stop hooks

4. **GitHub Integration** (`.github/`)
   - Automated Claude Code responses on GitHub issues/PRs
   - Trigger with `@claude` mentions
   - Issue triage automation

### Development Workflow

#### Getting Started
1. Navigate to your project directory
2. Run `claude` to start the interactive session
3. Use natural language commands to interact with your codebase

#### Common Commands
- `/bug` - Report issues directly within Claude Code
- `/doctor` - Diagnose and fix configuration issues
- `/export` - Export conversations for sharing
- `/add-dir` - Add directories to context (supports `~` expansion)
- `/mcp` - View MCP (Model Context Protocol) resources

#### Hook Development
Create custom hooks by:
1. Writing a hook script (Python example provided)
2. Configuring the hook in your Claude configuration
3. Testing with the PreToolUse, PreCompact, or Stop hook types

### Configuration

#### Claude Configuration Directory
- Location: `~/.claude/` (or `$CLAUDE_CONFIG_DIR`)
- Contains user settings, command history, and custom configurations
- Persisted across devcontainer sessions

#### Custom Slash Commands
- Store in `.claude/commands/` directory
- Supports subdirectory namespacing (e.g., `.claude/commands/frontend/component.md` becomes `/frontend:component`)
- Markdown format for command definitions

### Recent Updates (Latest Version: 1.0.51)

#### New Features
- Native Windows support (requires Git for Windows)
- Bedrock API key support via `AWS_BEARER_TOKEN_BEDROCK`
- Enhanced `/doctor` command for configuration validation
- Interactive mode support for `--append-system-prompt`
- Expanding variables in MCP server configuration

#### Performance Improvements
- Increased auto-compact warning threshold to 80%
- Shell snapshots moved to `~/.claude` for reliability
- Progress messages for long-running Bash commands

#### Bug Fixes
- Fixed user directory handling with spaces
- Resolved config file corruption issues
- Improved IDE extension path handling in WSL

### Security

#### Data Collection and Privacy
- Collects usage data and feedback for improvement
- **Does not train generative models** using your feedback
- Stores feedback transcripts for only 30 days
- Restricted access to user session data

#### Security Safeguards
- Limited retention periods for sensitive information
- Clear policies against using feedback for model training
- Comprehensive privacy policy and commercial terms

### Troubleshooting

#### Common Issues
1. **Container Setup Failures**
   - Ensure Docker/Podman is running
   - Check firewall settings
   - Verify script execution permissions

2. **Configuration Issues**
   - Use `/doctor` command to diagnose
   - Check `~/.claude/` directory permissions
   - Verify environment variable settings

3. **Performance Issues**
   - Increase NODE_OPTIONS memory limit
   - Use auto-compact when prompted
   - Clear old conversation history

### Contributing

#### Bug Reports
- Use `/bug` command within Claude Code
- File GitHub issues for complex problems
- Include conversation transcripts when helpful

#### Development Process
- Use the devcontainer for consistent environment
- Follow ESLint and Prettier configurations
- Test hooks thoroughly before submission

### Resources

- **Official Documentation**: https://docs.anthropic.com/en/docs/claude-code/overview
- **npm Package**: https://www.npmjs.com/package/@anthropic-ai/claude-code
- **GitHub Repository**: https://github.com/anthropics/claude-code
- **Privacy Policy**: https://www.anthropic.com/legal/privacy
- **Commercial Terms**: https://www.anthropic.com/legal/commercial-terms

### Support

For issues and questions:
1. Use the `/bug` command within Claude Code
2. Check the official documentation
3. File a GitHub issue with detailed information
4. Review the CHANGELOG.md for recent updates

---

This overview provides a comprehensive foundation for understanding and contributing to the Claude Code project. The combination of sophisticated development tooling, extensible hook system, and natural language interaction makes it a powerful tool for modern software development.