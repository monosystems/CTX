# Contributing to CTX

Thank you for your interest in contributing to CTX! This document provides guidelines and instructions for contributing.

## Table of Contents

- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Coding Standards](#coding-standards)
- [Commit Message Convention](#commit-message-convention)
- [Development Setup](#development-setup)
- [Testing](#testing)
- [Pull Request Process](#pull-request-process)
- [Community](#community)

---

## Getting Started

### Prerequisites

- **Node.js** 20+
- **Rust** (stable) + Cargo
- **Git**

### Setting Up Your Development Environment

1. **Fork the repository**

   Visit https://github.com/monosystems/CTX and click "Fork"

2. **Clone your fork**

   ```bash
   git clone https://github.com/YOUR_USERNAME/CTX.git
   cd CTX
   ```

3. **Add upstream remote**

   ```bash
   git remote add upstream https://github.com/monosystems/CTX.git
   ```

4. **Install dependencies**

   ```bash
   npm install
   ```

5. **Run development server**

   ```bash
   npm run tauri dev
   ```

---

## How to Contribute

### Reporting Bugs

Before reporting a bug, please check the [issues page](https://github.com/monosystems/CTX/issues) to see if it's already reported.

When reporting bugs, include:

- Clear description of the problem
- Steps to reproduce
- Expected vs actual behavior
- Screenshots (if applicable)
- OS and version
- CTX version (if applicable)

Use the [Bug Report Template](./.github/ISSUE_TEMPLATE/bug_report.md) when filing.

### Suggesting Features

We welcome feature suggestions! Before submitting:

- Check if the feature aligns with CTX's goals
- Search existing issues for similar suggestions

Use the [Feature Request Template](./.github/ISSUE_TEMPLATE/feature_request.md) when filing.

### Submitting Pull Requests

1. **Create a feature branch**

   ```bash
   git checkout main
   git pull upstream main
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**

   Follow our coding standards (below)

3. **Test your changes**

   ```bash
   npm run test      # Frontend tests
   cargo test        # Backend tests
   npm run lint      # Linting
   ```

4. **Commit your changes**

   Follow the commit message convention (below)

5. **Push and create PR**

   ```bash
   git push origin feature/your-feature-name
   ```

   Then create a PR via GitHub

---

## Coding Standards

### TypeScript / React

- Use **TypeScript** for all new code
- Follow functional component patterns
- Use hooks for state and effects
- Prefer composition over inheritance
- Keep components small and focused

```typescript
// Good
export function Button({ children, onClick }: ButtonProps) {
  return <button onClick={onClick}>{children}</button>;
}
```

### Rust / Tauri

- Run `cargo fmt` before committing
- Run `cargo clippy` to catch common issues
- Write unit tests for new functions
- Document public APIs with doc comments

```rust
/// Spawns a new terminal process with the given command.
///
/// # Arguments
///
/// * `cmd` - The command to execute
///
/// # Returns
///
/// A Result containing the PTY pair on success.
pub fn spawn_terminal(cmd: &str) -> Result<PtyPair, Error> {
    // ...
}
```

### Styling

- Use Tailwind CSS for styling
- Follow the shadcn/ui pattern for components
- Keep styles consistent with existing components

---

## Commit Message Convention

We use [Conventional Commits](https://www.conventionalcommits.org/) for our commit messages.

```
<type>(<scope>): <subject>
```

### Types

| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `style` | Code style changes (formatting, etc.) |
| `refactor` | Code refactoring (no functional changes) |
| `test` | Adding or modifying tests |
| `chore` | Maintenance tasks |
| `perf` | Performance improvements |
| `ci` | CI/CD changes |
| `build` | Build system changes |

### Scope

Scope should reflect the area of the project:

- `ui`, `components`, `hooks` — Frontend
- `backend`, `terminal`, `fs` — Rust backend
- `docs`, `readme`, `contributing` — Documentation
- `license`, `config` — Project configuration

### Examples

```
feat(auth): add login functionality
fix(editor): resolve slash command not working
docs(readme): update installation instructions
refactor(terminal): simplify pty spawning logic
style(lint): fix formatting issues
```

---

## Testing

### Frontend Tests

```bash
# Run all tests
npm run test

# Run with coverage
npm run test:coverage
```

### Backend Tests

```bash
# Run Rust tests
cargo test

# Run with coverage
cargo tarpaulin
```

### Linting

```bash
# TypeScript linting
npm run lint

# Rust linting
cargo clippy
```

---

## Pull Request Process

1. **Ensure CI passes** — All tests and linting must pass
2. **Update documentation** — If you changed functionality, update docs
3. **Add tests** — Include tests for new functionality
4. **Request review** — Assign reviewers from the maintainer team
5. **Address feedback** — Make requested changes
6. **Merge** — Maintainers will merge once approved

### PR Checklist

- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] No new warnings
- [ ] Commit messages follow convention

---

## Community

### Communication

- **Issues** — For bug reports and feature requests
- **Discussions** — For questions and ideas
- **Pull Requests** — For code contributions

### Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

---

## Questions?

If you have questions, feel free to open a [Discussion](https://github.com/monosystems/CTX/discussions) or reach out to maintainers.
