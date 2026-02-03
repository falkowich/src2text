# src2text

Concatenate source files from a project into a single text file for easy sharing and review.

## Use Cases

- Share code context without repository access
- Create documentation snapshots
- Code review preparation
- Project analysis
- Backup of source structure

## Features

- Auto-detects project language
- Excludes build artifacts and dependencies
- Token estimation for context planning
- Optional line limiting per file
- Platform independent (bash + standard Unix tools)

## Requirements

- bash
- find
- awk
- wc
- Standard Unix utilities

Works on Linux, macOS, and Windows (via WSL or Git Bash).

## Installation
```bash
# Download
curl -O https://raw.githubusercontent.com/falkowich/src2text/main/src2text

# Make executable
chmod +x src2text

# Optional: Move to PATH
mv src2text ~/bin/
```

## Usage
```bash
# Auto-detect language and output to file
src2text > output.txt

# Explicit language
src2text python > python-code.txt
src2text rust > rust-code.txt
src2text go > go-code.txt

# Limit lines per file (useful for large codebases)
src2text python 500 > limited-output.txt

# From specific subdirectory
cd src/
src2text > ../project-src.txt
```

## Supported Languages

- **Python** - .py files
- **Go** - .go files
- **Rust** - .rs files
- **JavaScript** - .js, .jsx, .svelte files
- **TypeScript** - .ts, .tsx files

## What Gets Excluded

### Python
- venv/, .venv/
- __pycache__/
- migrations/
- site-packages/
- .pytest_cache/

### Go
- vendor/
- testdata/

### Rust
- target/
- vendor/

### JavaScript/TypeScript
- node_modules/
- dist/, build/
- .svelte-kit/, .next/

### Common (all languages)
- .git/
- .idea/, .vscode/
- .DS_Store

## Output Format
```
===== ./src/main.py =====

[file contents]

===== ./src/utils.py =====

[file contents]
```

Each file is prefixed with its relative path for easy identification.

## Examples

### Basic usage
```bash
cd my-project
src2text > my-project.txt
```

### Share Python project
```bash
cd django-app
src2text python > django-app-src.txt
# Share django-app-src.txt via email, chat, etc.
```

### Large codebase with limiting
```bash
cd large-monorepo
src2text rust 500 > partial-rust-code.txt
```

## Token Estimation

The script estimates tokens using the approximation: 1 token ≈ 4 characters.

This is useful for planning context windows when sharing with text analysis tools.

## Privacy Note

This tool does NOT:
- Upload anything to external services
- Require network access (except for installation)
- Store or cache any data
- Depend on external APIs

Output goes to stdout. You control what happens with the result.

## Why Not Use X?

**Why not just tar/zip?**
- Text format is easier to review
- No extraction needed
- Works in environments without archive tools

**Why not use git archive?**
- Works on non-git projects
- Excludes build artifacts automatically
- Provides token estimation

**Why not copy-paste?**
- Consistent formatting
- Handles many files efficiently
- Maintains file structure information

## Philosophy

- Simple Unix tools over complex dependencies
- Stdout over file manipulation
- Auto-detection over configuration
- Platform independence

## Contributing

This is a personal tool. Fork it and make it yours.

## License

Public domain. Use it, modify it, distribute it.
```

## LICENSE (valfri)
```
This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <https://unlicense.org>
