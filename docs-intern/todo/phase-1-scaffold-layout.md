# Phase 1: Project Initialization & Layout

## Status: Pending | Priority: High

**Goal:** Scaffold Tauri v2 app with React + TypeScript, implement basic 3-pane layout and File System bridging.

---

## 1.1 Project Scaffolding

### 1.1.1 Initialize Tauri v2 Project
- [ ] Create new Tauri v2 project using `npm create tauri-app`
- [ ] Select React + TypeScript template
- [ ] Install dependencies: `npm install`
- [ ] Verify project builds: `npm run tauri build`

### 1.1.2 Configure Project Structure
- [ ] Create directory structure:
  ```
  CTX/
  ├── src/
  │   ├── components/
  │   ├── hooks/
  │   ├── stores/
  │   └── lib/
  ├── src-tauri/
  ├── docs/
  └── .ctx/
  ```
- [ ] Configure TypeScript (tsconfig.json)
- [ ] Configure Vite (vite.config.ts)
- [ ] Set up path aliases (@/ for src/)

### 1.1.3 Install Frontend Dependencies
- [ ] Install Tailwind CSS
  - [ ] npm install -D tailwindcss postcss autoprefixer
  - [ ] npx tailwindcss init -p
  - [ ] Configure tailwind.config.js
  - [ ] Add Tailwind directives to index.css
- [ ] Install shadcn/ui dependencies
  - [ ] npm install class-variance-authority clsx tailwind-merge lucide-react
  - [ ] npm install @radix-ui/react-slot @radix-ui/react-tooltip @radix-ui/react-dropdown-menu
- [ ] Install state management
  - [ ] npm install zustand @tanstack/react-query
- [ ] Install Tiptap editor
  - [ ] npm install @tiptap/react @tiptap/starter-kit @tiptap/extension-placeholder
- [ ] Install terminal dependencies
  - [ ] npm install xterm xterm-addon-fit

### 1.1.4 Configure Rust Backend
- [ ] Update Cargo.toml with dependencies:
  - [ ] tokio = { features = ["full"] }
  - [ ] portable-pty = "0.8"
  - [ ] async-openai = "0.18"
  - [ ] serde = { features = ["derive"] }
  - [ ] serde_json = "1.0"
- [ ] Run `cargo check` to verify compilation
- [ ] Build project: `cargo build`

---

## 1.2 UI Layout Implementation

### 1.2.1 Create 3-Pane Layout Components
- [ ] Create Sidebar component (`src/components/Sidebar.tsx`)
  - [ ] Implement resizeable sidebar (20% width)
  - [ ] Add file tree view
  - [ ] Add .ctx docs navigation
- [ ] Create Main Editor component (`src/components/Editor.tsx`)
  - [ ] Implement Tiptap instance container
  - [ ] Add tab bar for document navigation
  - [ ] Implement breadcrumbs
- [ ] Create Terminal Panel component (`src/components/TerminalPanel.tsx`)
  - [ ] Implement xterm.js container
  - [ ] Set initial size (30% height)

### 1.2.2 Implement Layout State Management
- [ ] Create layout store (`src/stores/layoutStore.ts`)
  - [ ] Sidebar width state
  - [ ] Terminal panel height state
  - [ ] Is resizing state
- [ ] Implement resize handlers
  - [ ] Sidebar drag to resize
  - [ ] Terminal panel drag to resize
- [ ] Add window resize event handling

### 1.2.3 Create Main App Layout
- [ ] Create App.tsx with 3-pane grid structure
- [ ] Implement responsive layout
- [ ] Add CSS for grid system
- [ ] Test resize interactions

---

## 1.3 File System Bridging

### 1.3.1 Implement Rust FS Commands
- [ ] Create `fs_list_dir` command (`src-tauri/src/commands/fs.rs`)
  - [ ] Accept `path: string` parameter
  - [ ] Return `FileEntry[]` with name, path, is_directory
  - [ ] Use `tokio::fs::read_dir` for async I/O
- [ ] Create `fs_read_file` command
  - [ ] Accept `path: string` parameter
  - [ ] Return file content as string
  - [ ] Handle errors gracefully
- [ ] Create `fs_write_doc` command
  - [ ] Accept `path: string, content: string`
  - [ ] Write to .ctx/ directory
  - [ ] Ensure directory exists before writing

### 1.3.2 Register Tauri Commands
- [ ] Update `src-tauri/src/lib.rs` to register commands
- [ ] Add proper error handling
- [ ] Configure allowed directories (project root only)

### 1.3.3 Implement Frontend FS Hooks
- [ ] Create `useFileSystem` hook (`src/hooks/useFileSystem.ts`)
  - [ ] Implement listDir function (IPC call)
  - [ ] Implement readFile function (IPC call)
  - [ ] Implement writeDoc function (IPC call)
- [ ] Create file tree component
  - [ ] Recursively render directory structure
  - [ ] Add expand/collapse functionality
  - [ ] Add file click to open in editor

### 1.3.4 Security Configuration
- [ ] Configure Tauri security.json
  - [ ] Set CSP restrictions
  - [ ] Scope filesystem permissions to project root
  - [ ] Disable default shell execution

---

## 1.4 Project Root Management

### 1.4.1 Project Opening Flow
- [ ] Implement project selector dialog
- [ ] Store opened project path in app state
- [ ] Validate project root (must contain .ctx/ or create it)
- [ ] Save recent projects list

### 1.4.2 Initialize .ctx Directory
- [ ] Check if .ctx/ exists on project open
- [ ] Create .ctx/ if missing
- [ ] Create welcome document in .ctx/
- [ ] Set up .ctx/.gitignore to prevent syncing

---

## 1.5 Verification Checklist

- [ ] `npm run tauri dev` runs successfully
- [ ] All 3 panes render correctly
- [ ] Resize handles work smoothly
- [ ] File tree displays project structure
- [ ] Clicking files opens them in editor
- [ ] File system commands return correct data
- [ ] `cargo check` passes with no warnings
- [ ] TypeScript compilation succeeds
- [ ] ESLint passes with no errors

---

## Notes

- **Blocking:** FS commands must be tested before Phase 2
- **Testing:** Manual testing of all resize interactions
- **Dependencies:** Ensure Rust toolchain is up to date
