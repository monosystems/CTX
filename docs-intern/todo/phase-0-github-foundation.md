# Phase 0: GitHub Open Source Foundation

## Status: Pending | Priority: Critical

**Goal:** Configure repository for open source release with proper tooling, templates, and CI/CD. **DO ALL OTHER WORK AFTER THIS PHASE.**

---

## 0.1 Repository Foundation

### 0.1.1 Initialize Git Repository
- [ ] Initialize git repo: `git init`
- [ ] Create initial commit with empty project structure
- [ ] Create `.gitignore`:
  ```
  # OS files
  .DS_Store
  Thumbs.db
  
  # Build outputs
  dist/
  src-tauri/target/
  
  # Dependencies
  node_modules/
  
  # IDE
  .vscode/
  .idea/
  
  # Secrets
  .env
  *.local
  
  # Project data
  .ctx/
  *.log
  ```
- [ ] Add remote: `git remote add origin <url>`

### 0.1.2 Configure Git Settings
- [ ] Set default branch to `main`
- [ ] Configure git user.name and user.email
- [ ] Enable git hook validation (pre-commit)
- [ ] Set branch protection rules (main branch)

### 0.1.3 Create Repository on GitHub
- [ ] Create public repository "ctx" on GitHub
- [ ] Add remote URL
- [ ] Push initial commit
- [ ] Enable Discussions
- [ ] Enable Wiki (or use docs/)
- [ ] Configure Project board

---

## 0.2 Essential Files

### 0.2.1 LICENSE
- [ ] Choose license: MIT or Apache 2.0 (recommend MIT for broad adoption)
- [ ] Create LICENSE file
- [ ] Add license header to source files
- [ ] Verify license compatibility (Tauri, Rust crates, NPM packages)

### 0.2.2 README.md
- [ ] Create comprehensive README:
  ```
  # CTX
  
  One-line description
  
  ## Features
  
  - Local-first project management
  - Embedded terminal
  - Smart linking
  - AI integration (BYOK)
  
  ## Quick Start
  
  ```bash
  git clone <repo>
  npm install
  npm run tauri dev
  ```
  
  ## Architecture
  
  Link to docs/ or ARCHITECTURE.md
  
  ## Contributing
  
  Link to CONTRIBUTING.md
  
  ## License
  
  MIT
  ```
- [ ] Add badges (build status, license, version)

### 0.2.3 CONTRIBUTING.md
- [ ] Create CONTRIBUTING.md with:
  - How to file issues
  - How to submit PRs
  - Coding standards (link to style guides)
  - Commit message convention
  - Development setup guide
  - Testing requirements
  - PR review process

### 0.2.4 CODE_OF_CONDUCT.md
- [ ] Create CODE_OF_CONDUCT.md
- [ ] Use Contributor Covenant v2.1
- [ ] Link from README and CONTRIBUTING

---

## 0.3 Issue & PR Templates

### 0.3.1 Bug Report Template
- [ ] Create `.github/ISSUE_TEMPLATE/bug_report.md`:
  ```
  ---
  name: Bug report
  about: Create a report to help us improve
  ---
  
  **Describe the bug**
  A clear description of what the bug is.
  
  **To Reproduce**
  Steps to reproduce the behavior:
  1. Go to '...'
  2. Click on '....'
  3. See error
  
  **Expected behavior**
  A clear description of what you expected to happen.
  
  **Screenshots**
  If applicable, add screenshots to help explain your problem.
  
  **Desktop (please complete the following information):**
  - OS: [e.g., macOS 14, Linux]
  - Version [e.g., 0.1.0]
  
  **Additional context**
  Add any other context about the problem here.
  ```

### 0.3.2 Feature Request Template
- [ ] Create `.github/ISSUE_TEMPLATE/feature_request.md`:
  ```
  ---
  name: Feature request
  about: Suggest an idea for this project
  ---
  
  **Is your feature request related to a problem?**
  A clear description of what the problem is.
  
  **Describe the solution you'd like**
  A clear description of what you want to happen.
  
  **Describe alternatives you've considered**
  A clear description of any alternative solutions or features you've considered.
  
  **Additional context**
  Add any other context or screenshots about the feature request here.
  ```

### 0.3.3 Pull Request Template
- [ ] Create `.github/PULL_REQUEST_TEMPLATE.md`:
  ```
  ---
  name: Pull request
  about: Create a PR to submit your changes
  ---
  
  **Description**
  Brief description of the changes.
  
  **Type of change**
  - [ ] Bug fix (non-breaking change)
  - [ ] New feature (non-breaking change)
  - [ ] Breaking change
  - [ ] Documentation update
  
  **How has this been tested?**
  - [ ] Unit tests
  - [ ] Integration tests
  - [ ] Manual testing
  
  **Checklist**
  - [ ] My code follows the style guidelines
  - [ ] I have performed a self-review
  - [ ] I have commented my code
  - [ ] I have made corresponding changes
  - [ ] My changes generate no new warnings
  - [ ] I have added tests that prove my fix works
  - [ ] New and existing tests pass
  ```

---

## 0.4 CI/CD Pipeline

### 0.4.1 GitHub Actions Workflows

#### Build & Test (main + PRs)
- [ ] Create `.github/workflows/build.yml`:
  ```yaml
  name: Build & Test
  
  on:
    push:
      branches: [main]
    pull_request:
      branches: [main]
  
  jobs:
    test:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-node@v4
          with:
            node-version: '20'
        - run: npm ci
        - run: npm run lint
        - run: npm run test
    
    build:
      runs-on: ubuntu-latest
      needs: test
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-node@v4
          with:
            node-version: '20'
        - uses: actions-rs/toolchain@v1
          with:
            toolchain: stable
        - run: npm ci
        - run: npm run tauri build
          env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        - uses: actions/upload-artifact@v4
          with:
            name: ctx-app
            path: src-tauri/target/release/bundle/
  ```

#### Release Workflow
- [ ] Create `.github/workflows/release.yml`:
  ```yaml
  name: Release
  
  on:
    release:
      types: [created]
  
  jobs:
    release:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-node@v4
          with:
            node-version: '20'
        - uses: actions-rs/toolchain@v1
          with:
            toolchain: stable
        - name: Build Release
          run: npm run tauri build
        - name: Create Release
          uses: softprops/action-gh-release@v2
          with:
            files: src-tauri/target/release/bundle/**
          env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  ```

#### Auto Label PRs
- [ ] Create `.github/workflows/label-prs.yml`:
  - Auto-label based on changed files
  - Use `actions/labeler` or custom script

### 0.4.2 Configure Dependabot
- [ ] Create `.github/dependabot.yml`:
  ```yaml
  version: 2
  updates:
    - package-ecosystem: "npm"
      directory: "/"
      schedule:
        interval: "weekly"
    - package-ecosystem: "cargo"
      directory: "/"
      schedule:
        interval: "weekly"
  ```

---

## 0.5 Security & Compliance

### 0.5.1 Security Policy
- [ ] Create `.github/SECURITY.md`:
  ```
  # Security Policy
  
  ## Reporting Security Issues
  
  Do not open public issues for security vulnerabilities.
  Email: security@example.com
  
  ## Disclosure Process
  
  1. Report privately
  2. Receive acknowledgment within 24h
  3. Receive fix timeline within 7 days
  4. Coordinated disclosure
  ```

### 0.5.2 Code Scanning
- [ ] Enable GitHub Advanced Security (if available)
- [ ] Configure CodeQL analysis:
  ```yaml
  # .github/workflows/codeql.yml
  name: CodeQL
  
  on:
    push:
      branches: [main]
    pull_request:
      branches: [main]
  
  jobs:
    analyze:
      runs-on: ubuntu-latest
      permissions:
        security-events: write
      steps:
        - uses: actions/checkout@v4
        - uses: github/codeql-action/init@v3
          with:
            languages: rust, typescript
        - uses: github/codeql-action/analyze@v3
  ```

### 0.5.3 Secret Scanning
- [ ] Enable secret scanning in repo settings
- [ ] Configure push protection
- [ ] Add custom patterns if needed

---

## 0.6 Release Process

### 0.6.1 Version Management
- [ ] Set up Conventional Commits validation
- [ ] Configure semantic-release or changesets:
  ```bash
  npm install -D @changesets/cli
  npx changeset init
  ```
- [ ] Create CHANGELOG.md:
  ```markdown
  # Changelog
  
  All notable changes to this project will be documented here.
  
  The format is based on [Keep a Changelog](https://keepachangelog.com/)
  and this project adheres to [Semantic Versioning](https://semver.org/).
  ```

### 0.6.2 Release Checklist
- [ ] Update version in package.json
- [ ] Update version in Cargo.toml
- [ ] Update CHANGELOG.md
- [ ] Create git tag: `git tag v0.1.0`
- [ ] Push tag: `git push origin v0.1.0`
- [ ] Create GitHub release
- [ ] Upload platform-specific builds:
  - [ ] .dmg (macOS)
  - [ ] .deb (Linux)
  - [ ] .msi (Windows)
  - [ ] .AppImage (Linux)

### 0.6.3 Automated Releases
- [ ] Configure release-it or similar:
  ```bash
  npm install -D release-it
  npx release-it init
  ```
- [ ] Set up GitHub Actions for automated releases

---

## 0.7 Community Management

### 0.7.1 Issue Triage
- [ ] Create triage workflow:
  - [ ] Label needs-triage
  - [ ] Auto-respond with thank you
  - [ ] Assign to maintainers
- [ ] Set up project board for triage

### 0.7.2 PR Review Process
- [ ] Define review criteria
- [ ] Set up CODEOWNERS file:
  ```
  # CODEOWNERS
  *.ts @username
  *.rs @username
  *.md @username
  ```
- [ ] Configure branch protection rules
- [ ] Set up required status checks

### 0.7.3 Roadmap & Milestones
- [ ] Create GitHub Milestones:
  - [ ] v0.1.0 (MVP)
  - [ ] v0.2.0
  - [ ] v1.0.0
- [ ] Link issues to milestones
- [ ] Set up Projects board for tracking

---

## 0.8 Documentation Hub

### 0.8.1 Developer Documentation
- [ ] Create `docs/` structure:
  ```
  docs/
  ├── architecture.md
  ├── contributing.md
  ├── development.md
  ├── deployment.md
  └── api/
  ```
- [ ] Write architecture overview
- [ ] Write development setup guide
- [ ] Document build process
- [ ] Document release process

### 0.8.2 User Documentation
- [ ] Create user-facing guides:
  ```
  docs/
  ├── getting-started.md
  ├── features/
  │   ├── editor.md
  │   ├── terminal.md
  │   └── smart-linking.md
  └── faq.md
  ```
- [ ] Add screenshots and GIFs
- [ ] Add video walkthroughs (optional)

### 0.8.3 AI Chat Documentation
- [ ] Document BYOK setup
- [ ] Document prompt best practices
- [ ] Add example prompts

---

## 0.9 Pre-Launch Checklist

- [ ] Repository is public
- [ ] All required files exist:
  - [ ] README.md
  - [ ] LICENSE
  - [ ] CONTRIBUTING.md
  - [ ] CODE_OF_CONDUCT.md
  - [ ] .gitignore
  - [ ] SECURITY.md
  - [ ] CHANGELOG.md
- [ ] CI/CD passes on main
- [ ] CodeQL scan passes
- [ ] Dependabot configured
- [ ] Issue/PR templates configured
- [ ] Branch protection enabled
- [ ] Release workflow tested
- [ ] Documentation is complete
- [ ] Security policy in place
- [ ] First release published

---

## Notes

- **Timeline:** Create Phase 6 files during/after Phase 1
- **Priority:** Security and CI/CD before first public release
- **Automation:** Automate as much as possible to reduce maintenance burden
