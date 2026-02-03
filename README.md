# src2text

Package your codebase into Claude-friendly formats for efficient AI-assisted development.

## What is src2text?

**src2text v2** is a smart packaging tool that prepares your entire codebase for Large Language Models like Claude. Instead of manually copying files or overwhelming the AI with pasted code, src2text creates an optimized tar.gz package that enables token-efficient, iterative development workflows.

## Why src2text v2?

### The Problem
When working with AI assistants like Claude:
- **Pasting code** = Every message includes entire history = token budget exhausted in hours
- **Manual file selection** = Tedious and error-prone
- **No context** = AI doesn't understand project structure

### The Solution
src2text v2 creates a smart package that:
- ✅ **Saves 6x tokens** - Files loaded only when needed via `view` commands
- ✅ **Auto-excludes junk** - No `node_modules`, `venv`, `__pycache__`, etc.
- ✅ **Provides context** - Generated `STRUCTURE.md` with file tree and token estimates
- ✅ **Language-aware** - Detects Python, Go, Rust, TypeScript/JavaScript automatically
- ✅ **Includes configs** - Grabs `pyproject.toml`, `Cargo.toml`, `package.json`, `Makefile`, etc.

## Version History

- **v1.x**: Generated single text file (good for ChatGPT free tier without file upload)
- **v2.0**: Creates tar.gz packages optimized for Claude's file operations

## Installation
```bash
# Download
curl -O https://raw.githubusercontent.com/falkowich/src2text/main/src2text

# Make executable
chmod +x src2text

# Optional: Move to PATH
sudo mv src2text /usr/local/bin/
```

### Requirements

- bash
- rsync
- tar
- Standard Unix utilities (find, wc, date)
- Optional: `tree` (for prettier file trees)

Works on Linux, macOS, and Windows (via WSL or Git Bash).

## Usage

### Basic Usage
```bash
# Auto-detect language
cd my-project
src2text

# Output: /tmp/20240203-1430-my-project.tar.gz
```

### Specify Language
```bash
src2text python
src2text go
src2text rust
src2text typescript
```

### Custom Output Name
```bash
src2text rust myapp
# Output: /tmp/20240203-1430-myapp.tar.gz
```

### Help & Version
```bash
src2text --help
src2text --version
```

## Workflow with Claude

### 1. Package Your Project
```bash
cd ~/projects/my-app
src2text
# ✅ Package created: /tmp/20240203-1430-my-app.tar.gz
```

### 2. Upload to Claude

- Open Claude.ai or Claude desktop app
- Click the attach button (📎)
- Upload the generated `.tar.gz` file

### 3. Start Working
```
I've uploaded my Python project.
Please read STRUCTURE.md first to get an overview.
```

### 4. Iterate Efficiently

Claude will:
- Use `view` to read files as needed (token-efficient)
- Use `str_replace` to modify code
- Provide updated files via `present_files`

**Result:** 6x longer sessions compared to pasting code!

## Supported Languages

| Language | Auto-detection | Config Files Included |
|----------|---------------|----------------------|
| **Python** | `*.py`, `pyproject.toml`, `setup.py` | pyproject.toml, requirements.txt, setup.py, pytest.ini, tox.ini |
| **Go** | `*.go`, `go.mod` | go.mod, go.sum, Makefile |
| **Rust** | `*.rs`, `Cargo.toml` | Cargo.toml, Cargo.lock, build.rs, rust-toolchain.toml |
| **TypeScript/JavaScript** | `*.ts`, `*.js`, `package.json` | package.json, tsconfig.json, webpack/vite/next configs |

## What Gets Excluded

### Python
```
venv/ .venv/ __pycache__/ *.pyc *.pyo
.pytest_cache/ .mypy_cache/ .tox/
dist/ build/ *.egg-info/ site-packages/
```

### Go
```
vendor/ testdata/
```

### Rust
```
target/ vendor/
```

### TypeScript/JavaScript
```
node_modules/ dist/ build/
.next/ .svelte-kit/ out/
.cache/ .parcel-cache/ coverage/
```

### Always Excluded
```
.git/ .DS_Store
.idea/ .vscode/
```

## Package Contents

Each package contains:
```
YYYYMMDD-HHMM-projectname/
├── src/                    # Your source files
│   ├── main.py
│   ├── lib/
│   └── ...
├── STRUCTURE.md           # File tree + token estimates
└── PACKAGE_README.md      # Usage instructions
```

### STRUCTURE.md Example
```markdown
# Project Structure: my-app

**Language:** python
**Generated:** 2024-02-03 14:30:00

## File Tree
[complete tree here]

## Token Estimates
### Source Files
- main.py: ~245 tokens
- lib/parser.py: ~512 tokens

### Configuration Files
- pyproject.toml: ~89 tokens
- README.md: ~156 tokens

### Totals
- Source files: ~4,230 tokens
- Config/docs: ~245 tokens
- TOTAL: ~4,475 tokens (2.8% of Claude's 160k budget)

## Recommended Workflow
1. Upload this tar.gz to Claude
2. Ask Claude to read STRUCTURE.md first
3. Work iteratively with view/str_replace
```

## Token Economy

### Without src2text (pasting code):
```
Message 1: 30k (system) + 2k (code) + 1k (your text) = 33k
Message 2: 30k + 2k + 1k + 2k (previous) = 35k
Message 3: 30k + 2k + 1k + 2k + 2k + 1k = 38k
...
After 15 messages: ~60-80k tokens = Budget exhausted
```

### With src2text (file operations):
```
Message 1: 30k (system) + 1k (your text) = 31k
Message 2: 30k + 1k + 200 (view command) = 31.2k
Message 3: 30k + 1k + 200 + 200 = 31.4k
...
After 15 messages: ~35k tokens = Budget healthy
```

**Result:** ~6x more messages possible!

## Examples

### Python Project
```bash
cd ~/projects/django-app
src2text python
# Includes: pyproject.toml, requirements.txt, all .py files
# Excludes: venv/, __pycache__/, migrations/
```

### Go Service
```bash
cd ~/projects/microservice
src2text go
# Includes: go.mod, go.sum, Makefile, all .go files
# Excludes: vendor/, testdata/
```

### Rust Application
```bash
cd ~/projects/cli-tool
src2text rust
# Includes: Cargo.toml, Cargo.lock, all .rs files
# Excludes: target/
```

### TypeScript/React App
```bash
cd ~/projects/webapp
src2text typescript
# Includes: package.json, tsconfig.json, all .ts/.tsx files
# Excludes: node_modules/, dist/, .next/
```

## Philosophy

- **Simple Unix tools** over complex dependencies
- **Auto-detection** over configuration
- **Platform independence** - Works everywhere bash works
- **Token efficiency** - Designed for LLM context windows

## Comparison to Alternatives

| Tool | Output | Best For |
|------|--------|----------|
| **src2text v2** | tar.gz with structure | Claude (file operations) |
| **Repomix** | XML text file | General LLM sharing |
| **Copy-paste** | Manual | Quick questions |

## Tips

### For Large Projects
```bash
# Focus Claude on specific subdirectories
"Focus on the src/api/ directory for now"
```

### For Long Sessions
```bash
# Start fresh when needed
"Let's start a new chat and continue with X"
```

### For Iterative Development
```bash
# Let Claude work incrementally
"Fix the parser first, then we'll move to the renderer"
```

## Troubleshooting

### "Could not detect project language"
```bash
# Explicitly specify language
src2text python
```

### Package too large
```bash
# Check what's included
tar -tzf /tmp/20240203-1430-project.tar.gz | head -20

# Manually exclude more
# Edit the script's get_exclude_patterns function
```

### Missing tree command
```bash
# Install tree (optional, not required)
# macOS
brew install tree

# Ubuntu/Debian
sudo apt install tree

# src2text works without tree, just uses find instead
```

## Contributing

This is a personal tool that evolved from practical needs. Feel free to:
- Fork and adapt for your workflow
- Submit issues for bugs
- Suggest improvements via PRs

Keep it simple, keep it bash.

## License

Public domain (Unlicense). Use it, modify it, distribute it.

## Links

- **Repository:** https://github.com/falkowich/src2text
- **Issues:** https://github.com/falkowich/src2text/issues
- **Related:** [Repomix](https://github.com/yamadashy/repomix), [Context Tools](https://github.com/context-hub/generator)

---

**Remember:** Upload the tar.gz, ask Claude to read STRUCTURE.md first, then work iteratively. No more token budget surprises! 🚀
