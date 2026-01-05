# CTX — Technical Design Document (MVP)

## Metadata

| Details | |
|---------|---|
| Project Name | CTX (Context) |
| Version | 0.1.0 (Alpha) |
| Type | Desktop Application (Local-First) |
| Core Stack | Rust (Tauri v2), React, TypeScript |

---

## 1. Executive Summary

CTX is a developer-centric project management environment designed to eliminate context switching. Unlike web-based tools (Jira, Notion), CTX runs locally, integrates directly with the project's file system, and embeds a fully functional terminal. It bridges the gap between planning (docs/tasks) and execution (code/terminal).

---

## 2. Architecture & Tech Stack

The application utilizes the Tauri v2 framework to minimize resource footprint and ensure type safety via the Rust backend.

### 2.1 Backend (Rust / Tauri Core)

| Component | Technology |
|-----------|------------|
| Runtime | Tauri v2 (utilizing generic system webview) |
| File System | `std::fs` and `tokio::fs` for non-blocking I/O operations |
| Terminal Emulation | `portable-pty` crate to spawn pseudo-terminal processes |
| Process Management | Spawning and managing child processes (e.g., git commands, language servers) |
| AI/LLM Client | `async-openai` (Rust crate) for handling BYOK (Bring Your Own Key) requests |

### 2.2 Frontend (Webview)

| Component | Technology |
|-----------|------------|
| Framework | React + TypeScript + Vite |
| State Management | `zustand` (for lightweight, transient state) + `tanstack-query` (for file-system syncing) |
| Styling | Tailwind CSS + shadcn/ui (Radix Primitives) |
| Editor Engine | tiptap (Headless wrapper around ProseMirror) for block-based editing |
| Terminal UI | xterm.js + xterm-addon-fit |

---

## 3. Core Modules & Specifications

### 3.1 Module A: The "Context" Engine (Documentation)

This is the primary interface, replacing external tools like Notion.

| Feature | Description |
|---------|-------------|
| **Storage Strategy** | "Docs as Code" |
| **Storage Location** | All data is stored in a hidden directory: `./.ctx/` at the project root |
| **Format** | Standard Markdown (.md) with YAML Frontmatter for metadata |
| **Editor Features** | Slash commands (`/`) to trigger block insertion (Todo list, H1-H3, Code Block) |

#### Smart Linking (Key Feature)

- **Syntax:** `[[src/components/Button.tsx]]`
- **Behavior:** Parsing this syntax creates a clickable anchor. Clicking triggers a backend event to open the file in the editor.

---

## 4. Data Structures & IPC

### 4.1 Document Structure

```typescript
interface Document {
  id: string;          // UUID
  title: string;
  created_at: string;  // ISO 8601
  updated_at: string;
  content: string;     // Markdown body
  frontmatter: {
    status: 'todo' | 'in-progress' | 'done';
    assignee?: string;
    linked_files: string[]; // Array of file paths
  };
}
```

### 4.2 IPC Commands (Tauri)

Invokable commands from Frontend:

| Command | Parameters | Returns |
|---------|------------|---------|
| `fs_list_dir` | `path: string` | `FileEntry[]` |
| `fs_read_file` | `path: string` | `string` |
| `fs_write_doc` | `path: string, content: string` | `void` |
| `pty_spawn` | `shell: string` | `number` (returns pid) |
| `pty_write` | `pid: number, data: string` | `void` |
| `pty_resize` | `pid: number, cols: number, rows: number` | `void` |
| `ai_completion_stream` | `prompt: string, context: string` | `Stream` |

---

## 5. UI/UX Layout (Grid System)

The application uses a resizeable 3-pane layout:

```
+------------------+------------------------------------------------+
|                  |  Tab Bar (Doc Title / Breadcrumbs)             |
|                  +------------------------------------------------+
|  SIDEBAR (20%)   |                                                |
|                  |  MAIN EDITOR (Context / Docs)                  |
|  - File Tree     |  (Tiptap Instance)                             |
|  - .ctx Docs     |                                                |
|                  |                                                |
|                  +------------------------------------------------+
|                  |                                                |
|                  |  TERMINAL / AI CHAT (Bottom Panel - 30%)       |
|                  |  (xterm.js container)                          |
+------------------+------------------------------------------------+
```

---

## 6. Security Considerations

| Concern | Mitigation |
|---------|------------|
| **CSP (Content Security Policy)** | Strict CSP to prevent injection attacks within the Webview |
| **File Access** | Scope filesystem access strictly to the opened project root directory to prevent sandbox escape |
| **API Keys** | Never sync API keys to git |

---

## 7. Roadmap

| Phase | Milestone |
|-------|-----------|
| Phase 1 | Scaffold Tauri app, implement File System bridging and basic Layout |
| Phase 2 | Implement Tiptap editor with Markdown serialization |
| Phase 3 | Integrate portable-pty and xterm.js for a working shell |
| Phase 4 | Implement "Smart Linking" (Doc -> File) |
| Phase 5 | Basic AI Chat integration |
