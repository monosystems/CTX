# CTX Architecture Overview

This document provides a high-level overview of CTX's architecture and technical decisions.

## Overview

CTX is a developer-centric desktop application built with Tauri v2, combining a React/TypeScript frontend with a Rust backend. The application follows a "docs as code" philosophy, storing all data locally in Markdown format with YAML frontmatter.

## Tech Stack

### Frontend

| Component | Technology |
|-----------|------------|
| Framework | React + TypeScript + Vite |
| State Management | Zustand (transient state) + TanStack Query (file sync) |
| Styling | Tailwind CSS + shadcn/ui (Radix Primitives) |
| Editor | Tiptap (ProseMirror-based) |
| Terminal | xterm.js + xterm-addon-fit |

### Backend

| Component | Technology |
|-----------|------------|
| Runtime | Tauri v2 (generic system webview) |
| File System | `std::fs` + `tokio::fs` (async I/O) |
| Terminal | `portable-pty` crate |
| AI Client | `async-openai` (BYOK) |

## Project Structure

```
CTX/
├── .ctx/              # Hidden storage (Markdown + Frontmatter)
├── src/               # Frontend (React + TypeScript)
│   ├── components/    # UI components
│   ├── hooks/         # Custom React hooks
│   ├── stores/        # Zustand state management
│   └── lib/           # Utilities
├── src-tauri/         # Rust backend (Tauri v2)
├── docs/              # User documentation
└── docs-intern/       # Internal development docs
```

## Core Modules

### 1. Context Engine (Documentation)

The primary interface for project documentation.

- **Storage Strategy:** "Docs as Code"
- **Storage Location:** `./.ctx/` at project root
- **Format:** Markdown (.md) with YAML Frontmatter
- **Features:** Slash commands (`/`) for block insertion

### 2. Smart Linking

Internal linking between documents and source files.

**Syntax:** `[[path/to/file]]`

**Behavior:** Parsing this syntax creates a clickable anchor. Clicking triggers a backend event to open the file in the default editor.

### 3. Embedded Terminal

Full terminal emulation using xterm.js and portable-pty.

- Process spawning and management
- Command execution (git, language servers, etc.)
- Real-time output streaming

### 4. AI Integration (BYOK)

Bring Your Own Key (BYOK) AI integration using async-openai.

- Configurable LLM endpoint
- Prompt templates for common tasks
- Privacy-focused (no data sent to external services without user config)

## Data Flow

```
┌─────────────┐     IPC      ┌─────────────┐
│   React     │ ◄──────────► │    Rust     │
│  Frontend   │   Commands   │   Backend   │
└─────────────┘              └─────────────┘
       │                          │
       │                          │
       ▼                          ▼
┌─────────────┐            ┌─────────────┐
│   xterm.js  │            │  File System│
│  Terminal   │            │   (.ctx/)   │
└─────────────┘            └─────────────┘
```

## Key Design Decisions

1. **Local-First:** All data stored locally, no mandatory cloud sync
2. **File-System Integration:** Seamless access to project files
3. **Type Safety:** TypeScript + Rust for compile-time safety
4. **Privacy-Focused:** AGPLv3 ensures user freedom
5. **Developer UX:** Built by developers, for developers

## Future Improvements

See the [Roadmap](../docs-intern/todo/phase-5-ai-chat.md) for upcoming features.
