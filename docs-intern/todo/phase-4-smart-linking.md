# Phase 4: Smart Linking

## Status: Pending | Priority: Medium

**Goal:** Implement [[path/to/file]] syntax for creating clickable anchors that open files in the editor.

---

## 4.1 Link Parsing & Detection

### 4.1.1 Create SmartLink Extension for Tiptap
- [ ] Create `src/components/editor/extensions/SmartLink.ts`
- [ ] Define custom Tiptap Node for smart links
- [ ] Implement `parseInput` to detect `[[...]]` pattern
- [ ] Implement regex pattern: `/\[\[([^\]]+)\]\]/g`
- [ ] Create HTML render function for link display

### 4.1.2 Implement Link Parsing Logic
- [ ] Parse link content (extract file path)
- [ ] Validate file path format
- [ ] Handle relative vs absolute paths
- [ ] Normalize path separators (convert \ to / on Windows)
- [ ] Support `[[path/to/file:line]]` syntax (optional)

### 4.1.3 Create Link Validation
- [ ] Verify file exists (async check via IPC)
- [ ] Handle non-existent file gracefully
- [ ] Show different style for broken links
- [ ] Add tooltip with full path on hover

---

## 4.2 Link Click Handler

### 4.2.1 Create Click Handler
- [ ] Add click event listener to smart link nodes
- [ ] Prevent default click behavior
- [ ] Extract file path from link
- [ ] Trigger file open event

### 4.2.2 Implement File Opening
- [ ] Create `openFile(path)` function
- [ ] Check if file is in project scope
- [ ] If text file: open in editor
- [ ] If image: show in preview
- [ ] If binary: show error or system open

### 4.2.3 Handle File Opening States
- [ ] Show loading state while checking file
- [ ] Handle permission errors
- [ ] Handle file not found
- [ ] Track recently opened files

---

## 4.3 Link Display & UI

### 4.3.1 Style Smart Links
- [ ] Create CSS class for smart links
- [ ] Add distinctive color (e.g., blue/purple)
- [ ] Add underline on hover
- [ ] Add cursor pointer
- [ ] Style broken links differently (red/strikethrough)

### 4.3.2 Add Hover Tooltip
- [ ] Show full file path on hover
- [ ] Show file status (exists/missing)
- [ ] Add file icon based on type
- [ ] Show relative path from project root

### 4.3.3 Create Link Context Menu
- [ ] Right-click to open context menu
- [ ] "Open" option
- [ ] "Open in new tab" option
- [ ] "Copy path" option
- [ ] "Copy wikilink syntax" option

### 4.3.4 Visual Feedback
- [ ] Animate link on click
- [ ] Add focus ring for keyboard navigation
- [ ] Show loading spinner for external files
- [ ] Transition between editor and linked file

---

## 4.4 Document Interface Update

### 4.4.1 Update Document Type
- [ ] Modify `Document` interface to include `linked_files: string[]`
- [ ] Add `backlinks: string[]` for reverse linking
- [ ] Update frontmatter schema

### 4.4.2 Implement Backlink Tracking
- [ ] Create backlink index (HashMap<path, docId[]>)
- [ ] Update backlinks when document is saved
- [ ] Remove backlinks when link is deleted
- [ ] Store backlink data in .ctx/backlinks.json

### 4.4.3 Extract Links on Save
- [ ] Parse document content for [[...]] patterns
- [ ] Extract all file paths
- [ ] Update `linked_files` in frontmatter
- [ ] Update backlink index

### 4.4.4 Validate Links on Load
- [ ] Check each linked file exists
- [ ] Update link status (valid/broken)
- [ ] Update backlinks for current document

---

## 4.5 Advanced Features

### 4.5.1 Auto-completion
- [ ] Detect `[[` trigger
- [ ] Show file path auto-complete dropdown
- [ ] Search files as user types
- [ ] Navigate with arrow keys
- [ ] Insert selected path

### 4.5.2 Link Preview
- [ ] Add hover card with file preview
- [ ] Show first N lines of file
- [ ] Show file metadata (size, modified date)
- [ ] Lazy load preview content

### 4.5.3 Graph View (Future)
- [ ] Visualize document relationships
- [ ] Show backlink connections
- [ ] Interactive graph exploration

### 4.5.4 Rename/Move Handling
- [ ] Detect file renames in editor
- [ ] Update all links pointing to renamed file
- [ ] Show refactoring confirmation dialog
- [ ] Batch update all links

---

## 4.6 Cross-File Navigation

### 4.6.1 Implement Tab System
- [ ] Allow multiple files open simultaneously
- [ ] Show open tabs in tab bar
- [ ] Switch between tabs
- [ ] Close tabs
- [ ] Pin frequently used files

### 4.6.2 Track Navigation History
- [ ] Create navigation stack (back/forward)
- [ ] Keyboard shortcuts (Cmd+[, Cmd+])
- [ ] Show history dropdown
- [ ] Restore cursor position when revisiting

### 4.6.3 Sync Editor State
- [ ] Remember scroll position per file
- [ ] Remember cursor position per file
- [ ] Persist state to .ctx/editor-state.json

---

## 4.7 Testing

### 4.7.1 Unit Tests
- [ ] Test regex pattern matching
- [ ] Test path normalization
- [ ] Test link extraction from content
- [ ] Test backlink indexing

### 4.7.2 Integration Tests
- [ ] Test link creation and rendering
- [ ] Test file opening from link
- [ ] Test broken link detection
- [ ] Test backlink updates

### 4.7.3 Manual Testing
- [ ] Test various file types (md, ts, py, json)
- [ ] Test nested paths
- [ ] Test relative paths
- [ ] Test path resolution edge cases

---

## Notes

- **Dependencies:** Requires Phase 1 (FS) and Phase 2 (Editor) to be complete
- **Scope:** Initially support text files only
- **UX:** Auto-complete is high priority for usability
