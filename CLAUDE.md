# Claude Code Container - AI Agent Documentation

This document provides comprehensive information for AI agents working on the Claude Code Container project.

## Project Overview

Claude Code Container is a containerized environment for running Claude Code CLI in isolated, reproducible environments. The project provides:

- Docker-based isolation for Claude Code sessions
- Cross-platform compatibility (Linux, macOS, Windows/WSL)
- Automatic timezone detection and configuration
- Multi-language support (Japanese/English)
- Container runtime detection (Docker/Container)

## Architecture

### Core Components

1. **`bin/claude-container`** - Main entry point script
   - Handles argument parsing and container orchestration
   - Provides bilingual user interface
   - Manages container lifecycle (build, run, cleanup)

2. **`Dockerfile`** - Container image definition
   - Based on Node.js environment
   - Includes Claude Code CLI installation
   - Configured for development workflows

3. **`docker/`** - Container configuration directory
   - Contains user environment setup
   - Mounted as `/home/node` in container

## Key Features Implemented

### Symlink Detection and User Hints

The `claude-container` script includes intelligent symlink detection that provides helpful hints to users:

```bash
# Function: show_symlink_hint()
# Purpose: Displays installation hints when script is run directly (not via symlink)
# Languages: Japanese and English support
# Triggered: Only when [ ! -L "$0" ] (not a symlink)
```

**Implementation Details:**
- Uses `[ -L "$0" ]` test to detect symlink execution
- Shows bilingual hints for adding script to PATH
- Suggests both system-wide (`/usr/local/bin`) and user-specific (`~/.local/bin`) installation
- Only displays when running directly to avoid redundant messages

### Language Detection

```bash
# Function: detect_language()
# Method: Checks $LANG environment variable for Japanese locale
# Default: English for all non-Japanese locales
# Usage: Applied to all user-facing messages and hints
```

### Container Runtime Detection

- Prioritizes macOS `container` command when available
- Falls back to Docker on other platforms
- Provides clear error messages for missing runtimes

## Development Workflow

### Testing Symlink Functionality

1. **Direct Execution Test:**
   ```bash
   ./bin/claude-container --help
   # Should display symlink installation hint
   ```

2. **Symlink Execution Test:**
   ```bash
   ln -s $(pwd)/bin/claude-container /tmp/test-symlink
   /tmp/test-symlink --help
   # Should NOT display hint (already using symlink)
   ```

3. **Language Testing:**
   ```bash
   LANG=en_US.UTF-8 ./bin/claude-container --help  # English hint
   LANG=ja_JP.UTF-8 ./bin/claude-container --help  # Japanese hint
   ```

### Code Conventions

- **Shell Scripting:** Follow POSIX compatibility where possible
- **Error Handling:** Provide bilingual error messages
- **User Experience:** Prioritize clarity and helpfulness
- **Security:** Use proper quoting and path handling

### Container Management

The script supports several container operations:

```bash
claude-container --rebuild-container  # Force rebuild image
claude-container --remove-container   # Remove image
claude-container [workspace] [-- claude-args...]  # Run container
```

## AI Agent Guidelines

### When Working on This Project

1. **Language Requirements:**
   - All user-facing messages MUST support Japanese and English
   - Use `$LANG_CODE` variable for language switching
   - Maintain consistent messaging patterns

2. **Testing Requirements:**
   - Test both direct execution and symlink execution paths
   - Verify language switching works correctly
   - Test container operations (build, run, cleanup)

3. **Code Quality:**
   - Preserve existing error handling patterns
   - Maintain POSIX shell compatibility
   - Use proper quoting for paths with spaces

4. **User Experience:**
   - Provide helpful hints without being intrusive
   - Support both novice and expert users
   - Maintain consistency with existing CLI patterns

### Key Files to Understand

- **`bin/claude-container`** - Main script (353+ lines)
  - Line 7-24: `show_symlink_hint()` function
  - Line 27-33: `detect_language()` function
  - Line 362: Symlink hint display call

### Common Operations

- **Adding new features:** Follow existing bilingual pattern
- **Modifying user messages:** Update both Japanese and English versions
- **Testing changes:** Use both direct and symlink execution methods
- **Container changes:** Test with `--rebuild-container` flag

## Recent Changes

### Symlink Detection Feature (Latest)

- Added `show_symlink_hint()` function with bilingual support
- Integrated hint display into main execution flow
- Tested across different execution methods and languages
- Maintains user experience while providing helpful guidance

This feature helps users understand how to make the script more convenient to use while avoiding redundant messages when already using symlinks.