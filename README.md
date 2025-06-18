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
│   ├── .claude.json       # Claude configuration
│   ├── .gitconfig         # Git configuration (optional)
│   ├── .npmrc             # npm configuration (optional)
│   └── ...                # Any other home directory files
├── Dockerfile             # Container image definition
└── README.md              # Documentation
```

#### Docker Directory

The `docker/` directory is mounted as the home directory (`/home/node`) inside the container. This allows you to:

- **Persistent Git Configuration**: Place your `.gitconfig` file to maintain git settings
- **SSH Keys**: Store `.ssh/` directory for git authentication
- **NPM Configuration**: Add `.npmrc` for npm registry settings
- **Shell Configuration**: Include `.bashrc`, `.zshrc` for personalized shell environment
- **Tool Configurations**: Store configuration files for various development tools

**Example configurations you can add:**

```bash
# Git credentials and configuration
docker/.gitconfig
docker/.ssh/id_rsa
docker/.ssh/id_rsa.pub
docker/.ssh/config

# Development tool configurations
docker/.npmrc
docker/.zshrc
docker/.bashrc
docker/.vimrc

# Application-specific settings
docker/.aws/credentials
docker/.docker/config.json
```

All files placed in the `docker/` directory will be available in the container and persist between runs.

**Practical Usage Examples:**

```bash
# Set up git credentials
echo '[user]
    name = Your Name
    email = your.email@example.com' > docker/.gitconfig

# Copy your SSH key for git authentication
cp ~/.ssh/id_rsa docker/.ssh/
cp ~/.ssh/id_rsa.pub docker/.ssh/
chmod 600 docker/.ssh/id_rsa

# Configure npm for private registries
echo 'registry=https://your-private-registry.com/' > docker/.npmrc

# Add custom shell aliases
echo 'alias ll="ls -la"
alias g="git"' >> docker/.zshrc
```

This makes the container environment feel like your local development setup!

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
