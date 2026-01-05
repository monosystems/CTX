# Phase 5: AI Chat Integration

## Status: Pending | Priority: Medium

**Goal:** Implement basic AI chat integration with BYOK (Bring Your Own Key) support and streaming responses.

---

## 5.1 Backend - OpenAI Integration

### 5.1.1 Install async-openai Dependencies
- [ ] Add `async-openai = "0.18"` to Cargo.toml
- [ ] Add `tokio-stream = "0.1"` for streaming
- [ ] Add `futures-util = "0.3"` for stream utilities
- [ ] Run `cargo check` to verify

### 5.1.2 Create AI Command Handlers
- [ ] Create `src-tauri/src/commands/ai.rs`
- [ ] Implement `ai_completion_stream` command:
  - [ ] Accept `prompt: string, context: string` parameters
  - [ ] Initialize OpenAI client with user API key
  - [ ] Build system message with context
  - [ ] Create chat completion request
  - [ ] Stream response using `Client::chat()`
- [ ] Implement `ai_validate_key` command:
  - [ ] Accept `api_key: string`
  - [ ] Test by creating client and calling models API
  - [ ] Return validation result

### 5.1.3 Configure AI Request
- [ ] Set default model (gpt-4 or gpt-3.5-turbo)
- [ ] Configure temperature (0.7 default)
- [ ] Set max tokens limit
- [ ] Build system prompt with project context
- [ ] Include current document content in context

### 5.1.4 Implement Streaming
- [ ] Create async stream from OpenAI response
- [ ] Convert stream to Tauri events
- [ ] Send chunks as they arrive
- [ ] Handle stream completion
- [ ] Handle stream errors

---

## 5.2 API Key Management (BYOK)

### 5.2.1 Create API Key Storage
- [ ] Store API key in system keyring or encrypted file
- [ ] Use `keyring` or `secrecy` crate
- [ ] Never log or expose API key
- [ ] Provide key input UI if not set

### 5.2.2 Implement Key Input Flow
- [ ] Show API key input dialog on first launch
- [ ] Validate key before saving
- [ ] Mask key input in UI
- [ ] Allow key update in settings

### 5.2.3 Security Measures
- [ ] Add API key to .gitignore
- [ ] Never commit API key to repository
- [ ] Warn if key appears in public repo
- [ ] Provide option to revoke key

### 5.2.4 Multiple Provider Support (Future)
- [ ] Abstract AI provider interface
- [ ] Support Anthropic, Google, etc.
- [ ] Provider selection UI
- [ ] Endpoint configuration

---

## 5.3 Frontend - AI Chat UI

### 5.3.1 Create Chat Panel Component
- [ ] Create `src/components/ai/ChatPanel.tsx`
- [ ] Create `src/components/ai/ChatMessage.tsx`
- [ ] Create `src/components/ai/ChatInput.tsx`
- [ ] Integrate into terminal panel (toggle between Terminal/Chat)

### 5.3.2 Implement Chat Store
- [ ] Create `src/stores/chatStore.ts`
- [ ] Track messages (role, content, timestamp)
- [ ] Track streaming state
- [ ] Track API key status
- [ ] Track model configuration

### 5.3.3 Chat Message Components
- [ ] Render user messages (right-aligned)
- [ ] Render assistant messages (left-aligned)
- [ ] Render system messages (centered/gray)
- [ ] Add typing indicator
- [ ] Add markdown rendering for AI responses

### 5.3.4 Chat Input Component
- [ ] Textarea with auto-resize
- [ ] Send button
- [ ] Keyboard shortcut (Cmd+Enter to send)
- [ ] Loading state
- [ ] Error display

---

## 5.4 Context Integration

### 5.4.1 Build Context from Current State
- [ ] Get current document content
- [ ] Get selected text (if any)
- [ ] Get open file paths
- [ ] Get recent command history (from terminal)
- [ ] Build context string for AI

### 5.4.2 Implement Context Presets
- [ ] "Explain this code" - pass selected code
- [ ] "Write tests" - pass function/code
- [ ] "Refactor" - pass code + refactor goal
- [ ] "Generate documentation" - pass code
- [ ] Custom prompt option

### 5.4.3 Context Optimization
- [ ] Truncate context if too long
- [ ] Prioritize recent information
- [ ] Include file structure summary
- [ ] Handle token limits gracefully

---

## 5.5 Streaming Response Handling

### 5.5.1 Implement Stream Consumer
- [ ] Create `src/hooks/useAIStream.ts`
- [ ] Listen for Tauri events
- [ ] Buffer incoming chunks
- [ ] Update message content as chunks arrive
- [ ] Handle stream completion

### 5.5.2 Streaming UI Updates
- [ ] Animate token-by-token display
- [ ] Update scroll to keep newest content visible
- [ ] Add cursor blink during streaming
- [ ] Show token count/usage

### 5.5.3 Handle Stream Interruption
- [ ] Allow user to cancel streaming
- [ ] Stop backend stream on cancel
- [ ] Show partial response
- [ ] Preserve partial content option

### 5.5.4 Error Handling
- [ ] Handle rate limits (429)
- [ ] Handle invalid key (401)
- [ ] Handle context length errors (400)
- [ ] Show user-friendly error messages

---

## 5.6 AI Chat Features

### 5.6.1 Conversation History
- [ ] Store chat history in memory
- [ ] Include history in context
- [ ] Limit history length (last N messages)
- [ ] Clear conversation option

### 5.6.2 Quick Actions
- [ ] Add toolbar with quick prompts
- [ ] "Explain code" button (when code selected)
- [ ] "Generate tests" button
- [ ] "Refactor" button
- [ ] "Document" button

### 5.6.3 Code Block Handling
- [ ] Render code blocks with syntax highlighting
- [ ] Add copy button to code blocks
- [ ] Add "Apply to editor" button (future)
- [ ] Support language标签

### 5.6.4 Markdown Rendering
- [ ] Render markdown in AI responses
- [ ] Support headings, lists, bold, italic
- [ ] Render tables
- [ ] Support links (open in editor)

---

## 5.7 Terminal/Chat Integration

### 5.7.1 Toggle Between Views
- [ ] Add toggle button in terminal panel
- [ ] Switch between Terminal and Chat tabs
- [ ] Remember last active view
- [ ] Keyboard shortcut to toggle

### 5.7.2 AI Output to Terminal (Optional)
- [ ] Option to stream AI to terminal instead of chat
- [ ] Show AI response as command output
- [ ] Support "Explain" commands in terminal
- [ ] Context-aware AI assistance

### 5.7.3 Hybrid Workflow
- [ ] Select code → right-click → "Ask AI"
- [ ] Opens chat with context
- [ ] AI response appears in chat
- [ ] Copy/paste back to editor

---

## 5.8 Testing

### 5.8.1 Unit Tests
- [ ] Test API key validation
- [ ] Test context building
- [ ] Test stream parsing

### 5.8.2 Integration Tests
- [ ] Test full chat flow
- [ ] Test streaming display
- [ ] Test error handling
- [ ] Test context inclusion

### 5.8.3 Manual Testing
- [ ] Test with valid API key
- [ ] Test streaming speed/responsiveness
- [ ] Test long context handling
- [ ] Test various prompt types

---

## Notes

- **Dependencies:** Requires Phase 3 (Terminal) for potential integration
- **Cost:** Include usage tracking or warnings
- **Privacy:** Emphasize data stays local (only API key sent externally)
- **Fallback:** Provide graceful degradation if API fails
