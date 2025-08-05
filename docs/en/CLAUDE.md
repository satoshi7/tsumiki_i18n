# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Tsumiki is a CLI tool that provides command templates for AI-driven development frameworks. This project is a CLI application built with TypeScript + React using Ink, which installs Claude Code command templates to the user's `.claude/commands/` directory.

## Development Commands

```bash
# Development environment
pnpm install                # Install dependencies

# Build
pnpm build                  # Build the project and copy commands directory to dist/
pnpm build:run              # Build and run CLI (for testing)

# Code quality
pnpm check                  # Code check with Biome
pnpm fix                    # Auto-fix with Biome
pnpm typecheck              # TypeScript type checking (using tsgo)
pnpm secretlint             # Check for secret information

# pre-commit hooks
pnpm prepare                # Setup simple-git-hooks
```

## Project Structure

- **`src/cli.ts`**: CLI entry point, command definitions using commander
- **`src/commands/install.tsx`**: Install command UI implementation using React + Ink
- **`commands/`**: Tsumiki's AI development framework Claude Code command templates (`.md` and `.sh` files)
- **`dist/`**: Build output, templates are copied to `dist/commands/`

## Technology Stack

- **CLI Framework**: Commander.js
- **UI Framework**: React + Ink (React rendering in CLI)
- **Build Tool**: tsup (TypeScript + ESBuild based)
- **Code Quality**: Biome (linter and formatter)
- **TypeScript**: tsgo (fast type checking)
- **Package Manager**: pnpm

## Build Process

During build (`pnpm build`), the following processes are executed:
1. Clean up the `dist` directory
2. Create the `dist/commands` directory
3. Copy `.md` and `.sh` files from `commands/` to `dist/commands/`
4. Build TypeScript code with tsup in both ESM and CJS formats

## Install Behavior

The `tsumiki install` command executes the following:
1. Create `.claude/commands/` directory in the current directory
2. Copy all `.md` and `.sh` files from the built `dist/commands/`
3. Display progress and file list with React + Ink

## Quality Management

Pre-commit hooks automatically execute the following:
- `pnpm secretlint`: Check for sensitive information
- `pnpm typecheck`: Type checking
- `pnpm fix`: Automatic code fixing

Always run `pnpm check` and `pnpm typecheck` before committing code changes.