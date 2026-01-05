# Phase 3: Terminal Integration

## Status: Pending | Priority: High

**Goal:** Integrate portable-pty (Rust) with xterm.js (Frontend) for a fully functional embedded terminal.

---

## 3.1 Rust Backend - PTY Implementation

### 3.1.1 Install portable-pty Dependencies
- [ ] Add `portable-pty = "0.8"` to Cargo.toml
- [ ] Add `tokio = { features = ["full"] }` for async support
- [ ] Run `cargo check` to verify

### 3.1.2 Create PTY Command Handlers
- [ ] Create `src-tauri/src/commands/pty.rs`
- [ ] Implement `pty_spawn` command:
  - [ ] Accept `shell: string` parameter (detect OS shell)
  - [ ] Create `pty::spawn()` with shell command
  - [ ] Return PID and master file descriptor
  - [ ] Store active PTY sessions in HashMap
- [ ] Implement `pty_write` command:
  - [ ] Accept `pid: number, data: string` parameters
  - [ ] Write data to PTY master
  - [ ] Handle encoding (UTF-8)
- [ ] Implement `pty_resize` command:
  - [ ] Accept `pid: number, cols: number, rows: number`
  - [ ] Create `pty::PtySize` with dimensions
  - [ ] Resize the PTY

### 3.1.3 Implement PTY Session Management
- [ ] Create PTY session store (HashMap<Pid, PtyPair>)
- [ ] Implement `pty_read` command (for streaming output):
  - [ ] Accept `pid: number`
  - [ ] Read from PTY master
  - [ ] Return data as string
  - [ ] Handle non-blocking reads
- [ ] Implement `pty_kill` command:
  - [ ] Accept `pid: number`
  - [ ] Kill the process
  - [ ] Remove from session store

### 3.1.4 Handle Process Output Streaming
- [ ] Implement async reading loop
- [ ] Use Tokio for non-blocking I/O
- [ ] Buffer output for efficient streaming
- [ ] Handle process exit events

---

## 3.2 Frontend - xterm.js Integration

### 3.2.1 Install xterm.js Dependencies
- [ ] npm install xterm
- [ ] npm install xterm-addon-fit
- [ ] npm install xterm-addon-web-links (optional)

### 3.2.2 Create Terminal Component
- [ ] Create `src/components/terminal/Terminal.tsx`
- [ ] Create `src/components/terminal/TerminalPane.tsx`
- [ ] Initialize xterm.js instance in useEffect
- [ ] Import xterm.css styles

### 3.2.3 Configure Terminal Options
- [ ] Set font family (monospace)
- [ ] Set font size (14px)
- [ ] Enable cursor blinking
- [ ] Configure colors (solarized dark or similar)
- [ ] Enable convertEol (CRLF → LF)

### 3.2.4 Implement Fit Addon
- [ ] Initialize xterm-addon-fit
- [ ] Call fit() on terminal open
- [ ] Call fit() on resize events
- [ ] Handle window resize

---

## 3.3 Frontend-Backend Communication

### 3.3.1 Create Terminal Store
- [ ] Create `src/stores/terminalStore.ts`
- [ ] Track active PTY PID
- [ ] Track terminal connection status
- [ ] Track is processing flag

### 3.3.2 Implement Terminal IPC Hook
- [ ] Create `src/hooks/useTerminal.ts`
- [ ] Implement `spawnTerminal(shell)`:
  - [ ] Call pty_spawn command
  - [ ] Store returned PID
  - [ ] Initialize xterm.js
- [ ] Implement `writeToTerminal(data)`:
  - [ ] Call pty_write command
- [ ] Implement `resizeTerminal(cols, rows)`:
  - [ ] Call pty_resize command
- [ ] Implement `readTerminalOutput()`:
  - [ ] Poll pty_read command
  - [ ] Write output to xterm.js

### 3.3.3 Implement Output Streaming
- [ ] Create polling loop for PTY output
- [ ] Use `setInterval` or requestAnimationFrame
- [ ] Write received data to xterm.js
- [ ] Handle partial reads

### 3.3.4 Handle Terminal Events
- [ ] xterm.js onData callback:
  - [ ] Call writeToTerminal
  - [ ] Handle special keys (Ctrl+C, Ctrl+D)
- [ ] xterm.js onResize callback:
  - [ ] Calculate new cols/rows
  - [ ] Call resizeTerminal

---

## 3.4 Terminal UI Features

### 3.4.1 Terminal Toolbar
- [ ] Add terminal toolbar above xterm.js
- [ ] Add shell selector dropdown (bash, zsh, fish, pwsh)
- [ ] Add clear terminal button
- [ ] Add kill terminal button
- [ ] Add new tab button

### 3.4.2 Terminal Tabs
- [ ] Implement multiple terminal tabs
- [ ] Store multiple PTY sessions
- [ ] Switch between tabs
- [ ] Close tab functionality

### 3.4.3 Visual Polish
- [ ] Style terminal container
- [ ] Add scrollbar styling
- [ ] Add focus states
- [ ] Custom cursor styling

### 3.4.4 Copy/Paste Support
- [ ] Implement Ctrl+Shift+C for copy
- [ ] Implement Ctrl+Shift+V for paste
- [ ] Enable xterm.js right-click paste

---

## 3.5 Platform-Specific Handling

### 3.5.1 Shell Detection
- [ ] Detect default shell on Linux/macOS
- [ ] Detect default shell on Windows
- [ ] Handle shell not found errors
- [ ] Provide fallback shells

### 3.5.2 Windows PTY Considerations
- [ ] Test with Windows Terminal
- [ ] Test with PowerShell
- [ ] Handle ANSI escape codes
- [ ] Test with CMD.exe

### 3.5.3 Path Handling
- [ ] Set working directory to project root
- [ ] Handle relative paths correctly
- [ ] Display current working directory in prompt

---

## 3.6 Security & Error Handling

### 3.6.1 Shell Command Whitelisting
- [ ] Allow only configured shells
- [ ] Reject dangerous commands
- [ ] Log all executed commands (optional)

### 3.6.2 Error Handling
- [ ] Handle PTY creation failures
- [ ] Handle broken pipe errors
- [ ] Handle process exit codes
- [ ] Show error messages in terminal

### 3.6.3 Process Cleanup
- [ ] Kill processes on app close
- [ ] Clean up PTY sessions on tab close
- [ ] Handle zombie processes

---

## 3.7 Testing

### 3.7.1 Unit Tests
- [ ] Test PTY command creation
- [ ] Test data encoding/decoding
- [ ] Test resize calculations

### 3.7.2 Integration Tests
- [ ] Test shell spawning
- [ ] Test command execution (ls, pwd, echo)
- [ ] Test process termination
- [ ] Test multiple terminals

### 3.7.3 Manual Testing
- [ ] Test basic commands (ls, cd, pwd)
- [ ] Test interactive commands (htop, vim, nano)
- [ ] Test copy/paste
- [ ] Test resize behavior
- [ ] Test on different shells

---

## Notes

- **Blocking:** Terminal must work before Phase 5 (AI Chat streams to terminal)
- **Platform:** Test on Linux, macOS, and Windows
- **Performance:** Consider WebSocket for better streaming performance
