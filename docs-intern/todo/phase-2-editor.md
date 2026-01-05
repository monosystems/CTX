# Phase 2: Editor Implementation

## Status: Pending | Priority: High

**Goal:** Implement Tiptap editor with block-based editing, slash commands, and Markdown serialization.

---

## 2.1 Tiptap Setup & Configuration

### 2.1.1 Install Tiptap Dependencies
- [ ] npm install @tiptap/react @tiptap/starter-kit
- [ ] npm install @tiptap/extension-placeholder
- [ ] npm install @tiptap/extension-task-list @tiptap/extension-task-item
- [ ] npm install @tiptap/extension-code-block-lowlight lowlight
- [ ] npm install @tiptap/pm prosemirror-model prosemirror-state prosemirror-view

### 2.1.2 Create Editor Component Structure
- [ ] Create `src/components/editor/Editor.tsx` (main component)
- [ ] Create `src/components/editor/Toolbar.tsx` (floating or fixed)
- [ ] Create `src/components/editor/BlockMenu.tsx` (slash command menu)
- [ ] Create `src/components/editor/extensions/index.ts` (extension configuration)

### 2.1.3 Configure Tiptap Extensions
- [ ] Configure StarterKit (bold, italic, strike, code, bulletList, orderedList, blockquote, codeBlock, heading, paragraph)
- [ ] Configure TaskList extension
- [ ] Configure TaskItem with checkbox functionality
- [ ] Configure Placeholder extension
- [ ] Configure CodeBlock with syntax highlighting
- [ ] Create custom extension for Smart Linking [[file]]

---

## 2.2 Slash Commands System

### 2.2.1 Define Slash Command Types
- [ ] Create command types enum:
  - HEADING_1, HEADING_2, HEADING_3
  - BULLET_LIST, ORDERED_LIST
  - TASK_LIST
  - CODE_BLOCK
  - QUOTE
  - DIVIDER
- [ ] Create command metadata interface

### 2.2.2 Implement Slash Command Menu UI
- [ ] Create SlashMenu component
- [ ] Detect `/` character trigger
- [ ] Show filtered command list
- [ ] Implement keyboard navigation (Arrow keys, Enter)
- [ ] Add icons for each command type
- [ ] Style menu (popover style, z-index)

### 2.2.3 Implement Slash Command Logic
- [ ] Create `handleSlashCommand` function
- [ ] Insert block based on selection
- [ ] Delete the `/` trigger after insertion
- [ ] Focus cursor at correct position

---

## 2.3 Block-Based Editing

### 2.3.1 Implement Block Operations
- [ ] Create Block interface/type
- [ ] Implement block selection
- [ ] Implement block deletion (Backspace at start of block)
- [ ] Implement block splitting (Enter at middle of block)
- [ ] Implement block merging (Delete at end of block)

### 2.3.2 Implement Drag & Drop
- [ ] Add drag handles to each block
- [ ] Implement block reordering
- [ ] Visual indicator during drag
- [ ] Drop zone highlighting

### 2.3.3 Implement Block Type Conversion
- [ ] Convert paragraph to heading
- [ ] Convert list item to task
- [ ] Convert to code block

---

## 2.4 Markdown Serialization

### 2.4.1 Implement Markdown Export
- [ ] Create `editorToMarkdown` utility
- [ ] Serialize headings (H1 → #)
- [ ] Serialize lists (bullet → - [ ] or -)
- [ ] Serialize code blocks with language tags
- [ ] Serialize blockquotes (→ >)
- [ ] Handle smart links → [[path]]

### 2.4.2 Implement Frontmatter Handling
- [ ] Extract frontmatter from document
- [ ] Parse YAML frontmatter
- [ ] Update frontmatter on document changes
- [ ] Serialize frontmatter back to string

### 2.4.3 Implement Markdown Import
- [ ] Create `markdownToEditor` utility
- [ ] Parse markdown syntax
- [ ] Create corresponding Tiptap nodes
- [ ] Parse and apply frontmatter

---

## 2.5 Document State Management

### 2.5.1 Create Document Store
- [ ] Create `src/stores/documentStore.ts`
- [ ] Track current document ID
- [ ] Track document content (as JSON or Markdown)
- [ ] Track document metadata (title, status, linked_files)
- [ ] Implement save/draft states

### 2.5.2 Implement Auto-Save
- [ ] Debounced save function
- [ ] Save to .ctx/ directory
- [ ] Show save status indicator
- [ ] Handle concurrent edits

### 2.5.3 Document List Management
- [ ] Fetch documents from .ctx/
- [ ] Display document list in Sidebar
- [ ] Add new document function
- [ ] Delete document function
- [ ] Rename document function

---

## 2.6 Editor UI Polish

### 2.6.1 Implement Floating Menu
- [ ] Show on text selection
- [ ] Add bold, italic, strike, code buttons
- [ ] Add link button
- [ ] Add heading selector

### 2.6.2 Implement Bubble Menu
- [ ] Inline formatting options
- [ ] Quick text styling

### 2.6.3 Keyboard Shortcuts
- [ ] Cmd/Ctrl+B → Bold
- [ ] Cmd/Ctrl+I → Italic
- [ ] Cmd/Ctrl+S → Save
- [ ] Cmd/Ctrl+N → New document
- [ ] Tab → Indent (in lists)
- [ ] Shift+Tab → Outdent (in lists)

### 2.6.4 Styling
- [ ] Style editor content (typography)
- [ ] Style task list checkboxes
- [ ] Style code blocks with syntax colors
- [ ] Style blockquotes
- [ ] Focus ring and selection colors

---

## 2.7 Testing

### 2.7.1 Unit Tests
- [ ] Test Markdown serialization
- [ ] Test frontmatter parsing
- [ ] Test slash command insertion
- [ ] Test block operations

### 2.7.2 Integration Tests
- [ ] Test document save/load cycle
- [ ] Test document switching
- [ ] Test concurrent edits

### 2.7.3 Manual Testing
- [ ] Test all slash commands
- [ ] Test keyboard navigation
- [ ] Test markdown copy/paste
- [ ] Test large documents

---

## Notes

- **Blocking:** Editor must handle Markdown before Phase 4 (Smart Linking)
- **UX:** Slash menu should appear instantly
- **Performance:** Consider virtual scrolling for large documents
