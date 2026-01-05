# PROJECT KNOWLEDGE BASE

**Generated:** 2026-01-05 18:53:00
**Project:** CTX — Local-First Project Management Environment
**State:** Greenfield (empty, scaffold to begin)

## OVERVIEW

CTX is a developer-centric desktop application (Tauri v2) that bridges planning and execution. Local-first, file-system integrated, with embedded terminal. Replaces Notion/Jira workflows with "docs as code" approach.

## STRUCTURE

```
CTX/
├── .ctx/              # Hidden storage (markdown docs + frontmatter)
├── src/               # Frontend (React + TypeScript + Vite)
│   ├── components/    # UI components
│   ├── hooks/         # Custom React hooks
│   ├── stores/        # Zustand state management
│   └── lib/           # Utilities
├── src-tauri/         # Rust backend (Tauri v2)
├── docs/              # Documentation
└── MVP.md             # Technical specification
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| Specs | `./MVP.md` | Authoritative technical design |
| Frontend | `./src/` | React + TypeScript + Vite |
| Backend | `./src-tauri/` | Rust + Tauri v2 |
| Storage | `./.ctx/` | Created at runtime |
| Docs | `./docs/` | User/developer guides |

## ARCHITECTURE (Planned)

### Frontend Stack
- **Framework:** React + TypeScript + Vite
- **State:** Zustand (transient) + TanStack Query (file sync)
- **Styling:** Tailwind CSS + shadcn/ui (Radix)
- **Editor:** Tiptap (ProseMirror) for block-based markdown
- **Terminal:** xterm.js + xterm-addon-fit

### Backend Stack (Tauri v2)
- **Runtime:** Tauri v2 (generic webview)
- **FS:** `std::fs` + `tokio::fs` (async I/O)
- **Terminal:** `portable-pty` crate
- **AI:** `async-openai` (BYOK)

### Key Patterns
- **Docs as Code:** Markdown with YAML frontmatter in `.ctx/`
- **Smart Linking:** `[[path/to/file]]` syntax for doc-to-file anchors
- **IPC:** Tauri commands (fs_list_dir, fs_read_file, pty_spawn, etc.)

## CONVENTIONS

| Aspect | Convention |
|--------|------------|
| **Language** | TypeScript for frontend, Rust for backend |
| **Formatting** | Prettier (TS), rustfmt (Rust) |
| **Linting** | ESLint (TS), clippy (Rust) |
| **Testing** | Vitest (TS), cargo test (Rust) |
| **Commits** | Conventional Commits |
| **Branch** | trunk-based (main + feature branches) |

## ANTI-PATTERNS (THIS PROJECT)

- **NEVER** sync API keys or secrets to version control
- **NEVER** access files outside opened project root (sandbox escape)
- **NEVER** block async I/O on main thread
- **NEVER** store docs in proprietary formats (markdown only)

## UNIQUE STYLES

- **Local-First:** All data lives in `.ctx/` hidden directory
- **Block-Based Editor:** Tiptap with slash commands (`/`)
- **3-Pane Layout:** Sidebar (20%) + Editor + Terminal/AI (30%)
- **BYOK AI:** User provides own LLM API key (no vendor lock-in)

## COMMANDS

```bash
# Frontend dev
npm run dev          # Vite dev server
npm run build        # Production build
npm run lint         # ESLint
npm run format       # Prettier

# Backend
cargo check          # Type check Rust
cargo build          # Build Tauri binary
cargo test           # Run Rust tests
cargo clippy         # Lint Rust

# Full stack
npm run tauri dev    # Run full app
```

## NOTES

- **Greenfield:** No code exists yet. Scaffold Phase 1 from MVP.md first.
- **TypeScript LSP:** Available (configured), install for full IDE support.
- **Rust LSP:** Not installed (gopls detected), install `rust-analyzer` for backend.
- **File System:** Scope all access to opened project directory.
