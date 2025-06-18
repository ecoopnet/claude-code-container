# Claude Code Container

[English](#english) | [日本語](README_ja.md)

## English

A tool for running Claude Code inside Docker containers.

### Setup

1. **Start Container System**
   ```bash
   container system start
   ```

2. **Build Docker Image**
   ```bash
   ./build.sh
   ```

### Usage

```bash
claude-container [workspace] [-- claude-options...]
```

#### Argument Details

- **workspace** (optional): Working directory path
  - If omitted: Use current directory
  - `.`: Explicitly specify current directory
  - `/path/to/project`: Specify a specific directory
- **--**: Delimiter between workspace and Claude options
- **claude-options**: Options passed directly to Claude Code

#### Basic Usage Examples

```bash
# Use current directory
claude-container

# Explicitly specify current directory
claude-container .

# Specify a specific directory
claude-container /path/to/project
```

#### Usage with Claude Options

```bash
# Current directory + Claude options
claude-container -- --model claude-3-opus-20240229

# Current directory + Claude options (explicit)
claude-container . -- --verbose

# Specific directory + Claude options
claude-container /path/to/project -- --model claude-3-opus

# Show Claude help
claude-container --help

# Skip dangerous permissions
claude-container -- --dangerously-skip-permissions
```

#### Using with Symbolic Links

The script supports symbolic links and can be executed from anywhere:

```bash
# Create symbolic link in PATH directory
sudo ln -s /opt/ai-agents/claude-code-container/claude-container /usr/local/bin/claude-container

# Execute from anywhere
cd ~/my-project
claude-container

# Or from another location
claude-container ~/my-project
```

**Note**: Even when using symbolic links, configuration files (`.claude`, etc.) are automatically loaded from the `docker/` folder in the original script directory.

#### Features

- **Auto Configuration**: Automatically mount local Claude settings (`.claude` directory)
- **Timezone Detection**: Auto-detect local timezone (Windows/macOS/Linux support)
- **Symbolic Link Support**: Works regardless of script location or linking
- **Configuration Persistence**: Changes made inside container are saved locally
- **Multi-language Support**: Japanese in Japanese environments, English otherwise
- **Flexible Argument Processing**: Optional workspace specification, `--` delimiter for Claude options

#### Reference

For detailed setup instructions, see:
https://zenn.dev/schroneko/articles/claude-code-on-apple-container
