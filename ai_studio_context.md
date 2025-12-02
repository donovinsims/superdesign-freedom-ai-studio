# SuperDesign Freedom Edition - Project Context

## Overview
SuperDesign Freedom Edition is a free, open-source web app and Chrome extension for UI design and code generation. It allows users to clone websites, edit them in a React Flow canvas, and generate code using AI (Gemini/Ollama) without any paywalls.

## Tech Stack
- **Web App**: Next.js 14, TypeScript, Tailwind CSS, shadcn/ui, React Flow, Zustand, Dexie.js (IndexedDB).
- **Extension**: Chrome Manifest V3, Vanilla JS.
- **AI**: Google Gemini API (Free Tier), Ollama (Local).

## Project Structure

### Web App (`superdesign-freedom-web`)
- `app/`: Next.js App Router pages.
- `components/canvas/`: React Flow canvas implementation.
- `components/chat/`: AI chat interface with streaming response.
- `components/editor/`: Monaco Editor for code viewing.
- `lib/ai.ts`: AI service handling Gemini and Ollama requests.
- `lib/db.ts`: IndexedDB schema for storing projects.
- `store/`: Zustand state management.

### Extension (`superdesign-freedom-extension`)
- `manifest.json`: V3 configuration.
- `content.js`: Logic for capturing DOM and computed styles.
- `background.js`: Service worker for message passing.
- `popup.html/js`: Extension UI.

## Key Features
1.  **No Paywalls**: All features are free.
2.  **Export**: ZIP download, VS Code/Cursor integration.
3.  **Cloning**: Copy full pages or specific components with styles.
4.  **AI**: Generate and edit UI code using natural language.
