# CTX

<div align="center">

**Local-First Project Management Environment**

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Main Branch](https://img.shields.io/badge/branch-main-green)](https://github.com/monosystems/CTX/tree/main)
[![Phase](https://img.shields.io/badge/Phase-0%3A%20GitHub%20Foundation-yellow)](#)

*Eliminate context switching. Plan and execute in one place.*

</div>

---

## Features

- 📝 **Local-First Documentation** — "Docs as Code" approach with Markdown + YAML frontmatter
- 💻 **Embedded Terminal** — Full xterm.js-powered terminal integration
- 🔗 **Smart Linking** — `[[path/to/file]]` syntax for instant file navigation
- 🤖 **AI Integration (BYOK)** — Bring your own LLM API key
- ⚡ **Tauri v2** — Lightweight, secure, cross-platform desktop app
- 🎨 **Modern UI** — React + TypeScript + Tailwind CSS

## Quick Start

```bash
# Clone the repository
git clone https://github.com/monosystems/CTX.git
cd CTX

# Install dependencies
npm install

# Run development build
npm run tauri dev
```

## Architecture

CTX is built on a "docs as code" philosophy:

```
CTX/
├── .ctx/              # Hidden storage (Markdown + Frontmatter)
├── src/               # Frontend (React + TypeScript + Vite)
│   ├── components/    # UI components
│   ├── hooks/         # Custom React hooks
│   ├── stores/        # Zustand state management
│   └── lib/           # Utilities
├── src-tauri/         # Rust backend (Tauri v2)
├── docs/              # User documentation
└── docs-intern/       # Internal development docs
```

See [Architecture Overview](./docs/architecture.md) for details.

## Contributing

Contributions are welcome! Please read our [Contributing Guide](./CONTRIBUTING.md) for:

- How to file issues
- How to submit pull requests
- Coding standards and conventions
- Development setup
- Testing requirements

## License

This program is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License along with this program. If not, see <https://www.gnu.org/licenses/>.

## Code of Conduct

This project adheres to the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

---

<div align="center">

**Built with ❤️ for developers who value focus**

</div>
