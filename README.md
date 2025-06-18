# Claude Code Container

[English](#english) | [日本語](README_ja.md)

## English

A tool for running Claude Code inside Docker containers.

### Setup

1. **Start Container System** (if using container runtime)
   ```bash
   container system start
   ```

2. **Ready to Use**
   The container image will be automatically built on first run. No manual build required!
   
   Simply run `claude-container` and the setup will complete automatically.

### Usage

```bash
claude-container [options] [workspace] [-- claude-options...]
```

#### Options

- **--rebuild-container**: Rebuild the container image
- **--remove-container**: Remove the container image

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

#### Container Management Examples

```bash
# Rebuild container image
claude-container --rebuild-container

# Remove container image
claude-container --remove-container
```

#### Using with Symbolic Links

The script supports symbolic links and can be executed from anywhere:

```bash
# Create symbolic link in PATH directory
sudo ln -s /opt/ai-agents/claude-code-container/bin/claude-container /usr/local/bin/claude-container

# Execute from anywhere
cd ~/my-project
claude-container

# Or from another location
claude-container ~/my-project
```

**Note**: Even when using symbolic links, configuration files (`.claude`, etc.) are automatically loaded from the `docker/` folder in the project root directory.

#### Project Structure

```
claude-code-container/
├── bin/
│   └── claude-container    # Main executable script
├── docker/                 # Configuration files (auto-created)
│   ├── .claude/           # Claude settings
│   └── .claude.json       # Claude configuration
├── Dockerfile             # Container image definition
└── README.md              # Documentation
```

#### Features

- **Zero Manual Setup**: Automatically builds container image on first run
- **Auto Configuration**: Automatically mount local Claude settings (`.claude` directory)
- **Timezone Detection**: Auto-detect local timezone (Windows/macOS/Linux support)
- **Symbolic Link Support**: Works regardless of script location or linking
- **Configuration Persistence**: Changes made inside container are saved locally
- **Multi-language Support**: Japanese in Japanese environments, English otherwise
- **Flexible Argument Processing**: Optional workspace specification, `--` delimiter for Claude options
- **Container Runtime Detection**: Automatically uses `container` on macOS (if available) or `docker` elsewhere
- **Container Management**: Rebuild and remove container images as needed

#### Container Runtime

The script automatically detects and uses the appropriate container runtime:

- **macOS**: Uses `container` command if available, otherwise falls back to `docker`
- **Other platforms**: Uses `docker` command

You can override the container runtime by setting the `CONTAINER_RUNTIME` environment variable:

```bash
# Force use of docker
CONTAINER_RUNTIME=docker claude-container

# Force use of container (macOS)
CONTAINER_RUNTIME=container claude-container
```

#### Reference

For detailed setup instructions, see:
https://zenn.dev/schroneko/articles/claude-code-on-apple-container
