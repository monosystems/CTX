# CTX - Todo List Overview

## Phases

| Phase | Status | Description |
|-------|--------|-------------|
| [Phase 0: GitHub Open Source Foundation](./phase-0-github-foundation.md) | Pending | Repository setup, CI/CD, templates (DO FIRST) |
| [Phase 1: Project Initialization & Layout](./phase-1-scaffold-layout.md) | Pending | Scaffold Tauri app, FS bridging, basic layout |
| [Phase 2: Editor Implementation](./phase-2-editor.md) | Pending | Tiptap editor with Markdown serialization |
| [Phase 3: Terminal Integration](./phase-3-terminal.md) | Pending | portable-pty and xterm.js integration |
| [Phase 4: Smart Linking](./phase-4-smart-linking.md) | Pending | Doc-to-File anchor system |
| [Phase 5: AI Chat Integration](./phase-5-ai-chat.md) | Pending | Basic AI completion streaming |

---

## Quick Navigation

### Phase 0 - GitHub Foundation (DO FIRST)
- [ ] Initialize git repo and create GitHub repository
- [ ] Create essential files (LICENSE, README, CONTRIBUTING, CODE_OF_CONDUCT)
- [ ] Set up Issue & PR templates
- [ ] Configure CI/CD (GitHub Actions: build, test, release)
- [ ] Configure Dependabot for dependencies
- [ ] Set up security policy and CodeQL scanning
- [ ] Create release workflow and CHANGELOG
- [ ] Document development process and user guides

### Phase 1 - Initialization
- [ ] Initialize Tauri v2 project with React + TypeScript
- [ ] Configure project structure (src/, src-tauri/)
- [ ] Set up Tailwind CSS + shadcn/ui
- [ ] Implement basic 3-pane layout
- [ ] Implement File System bridging (fs_list_dir, fs_read_file)

### Phase 2 - Editor
- [ ] Install and configure Tiptap
- [ ] Implement block-based editing
- [ ] Add slash commands (/todo, /h1, /code)
- [ ] Markdown serialization/deserialization
- [ ] YAML frontmatter handling

### Phase 3 - Terminal
- [ ] Implement portable-pty backend commands
- [ ] Create pty_spawn, pty_write, pty_resize commands
- [ ] Integrate xterm.js frontend
- [ ] Implement xterm-addon-fit for resize handling
- [ ] Connect frontend terminal to backend PTY

### Phase 4 - Smart Linking
- [ ] Implement [[path/to/file]] parsing
- [ ] Create clickable anchors for linked files
- [ ] Handle file opening events
- [ ] Update Document interface with linked_files

### Phase 5 - AI Chat
- [ ] Set up async-openai integration
- [ ] Implement ai_completion_stream command
- [ ] Create AI chat UI panel
- [ ] Implement BYOK (Bring Your Own Key) input
- [ ] Connect AI stream to terminal/chat panel
