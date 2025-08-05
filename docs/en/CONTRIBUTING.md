# Contribution Guide

Thank you for contributing to the Tsumiki project! This guide explains how to contribute to the project.

## Development Environment Setup

### Required Environment

- Node.js 18.0.0 or higher
- pnpm 10.13.1 or higher

### Setup Steps

1. Fork and clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/tsumiki.git
cd tsumiki
```

2. Install dependencies:

```bash
pnpm install
```

3. Set up pre-commit hooks:

```bash
pnpm prepare
```

## Development Workflow

### Branch Strategy

- `main` branch: Stable code
- Feature development: `feature/feature-name`
- Bug fixes: `bugfix/bug-name`
- Hotfixes: `hotfix/fix-content`

### Development Process

1. Create a new branch:

```bash
git checkout -b feature/your-feature-name
```

2. Make code changes

3. Run code quality checks:

```bash
# Type checking
pnpm typecheck

# Code checking
pnpm check

# Auto-fix
pnpm fix

# Secret information check
pnpm secretlint
```

4. Run build tests:

```bash
pnpm build:run
```

5. Commit changes:

```bash
git add .
git commit -m "feat: add new feature"
```

## Commit Message Conventions

Use [Conventional Commits](https://www.conventionalcommits.org/) format:

- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation changes
- `style:` Code style changes (no functional impact)
- `refactor:` Refactoring
- `test:` Test addition/modification
- `chore:` Build process or tool changes

Examples:
```
feat: add new install command for .sh files
fix: resolve path handling issue in install command
docs: update README with new command examples
```

## Code Quality Standards

### Automatic Checks

Pre-commit hooks automatically run the following:

- **secretlint**: Check for secrets (API keys, passwords, etc.)
- **typecheck**: TypeScript type checking
- **fix**: Automatic code fixing with Biome

### Manual Checks

Run the following commands before making changes:

```bash
# Run all checks
pnpm typecheck && pnpm check && pnpm secretlint

# Auto-fix code
pnpm fix
```

## Project Structure

```
tsumiki/
├── src/
│   ├── cli.ts              # CLI entry point
│   └── commands/
│       └── install.tsx     # Install command UI implementation
├── commands/               # Command templates (.md, .sh)
├── dist/                  # Build output
├── package.json
├── CLAUDE.md              # Project instructions
└── README.md
```

## Pull Requests

### Creating Pull Requests

1. Push changes:

```bash
git push origin feature/your-feature-name
```

2. Create a pull request on GitHub

3. Fill in the description following the pull request template

### Pull Request Requirements

- [ ] Changes are clearly described
- [ ] Related issues are linked (if applicable)
- [ ] Code quality checks pass
- [ ] Build succeeds
- [ ] No sensitive information is included

## Issue Reporting

Bug reports and feature requests are accepted in [Issues](https://github.com/classmethod/tsumiki/issues).

### Bug Reports

Please include the following information:

- Steps to reproduce
- Expected behavior
- Actual behavior
- Environment information (OS, Node.js version, etc.)
- Error messages (if applicable)

### Feature Requests

Please include the following information:

- Description of the proposed feature
- Use cases
- Expected benefits
- Implementation ideas (if any)

## Security

If you discover security-related issues, please report them privately rather than through public issues.

## License

This project is published under the MIT License. By contributing, you agree to this license.

## Questions & Support

- [Issues](https://github.com/classmethod/tsumiki/issues) - Bug reports, feature requests
- [Discussions](https://github.com/classmethod/tsumiki/discussions) - Questions, discussions

We look forward to your contributions!