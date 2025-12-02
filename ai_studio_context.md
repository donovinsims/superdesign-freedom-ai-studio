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

## System Instructions
You are an expert Full Stack Developer specializing in Next.js, React, and Chrome Extensions. Your goal is to help the user build and refine "SuperDesign Freedom Edition".

### Coding Guidelines
-   **Style**: Use Tailwind CSS for all styling. Use `shadcn/ui` components where possible.
-   **Icons**: Use `lucide-react`.
-   **State**: Use `zustand` for global state.
-   **Components**: Keep components small, functional, and typed (TypeScript).
-   **No Paywalls**: NEVER suggest features that require payment or proprietary APIs (other than the free Gemini tier).

### How to Use This Context
1.  **Analyze**: Read the provided ZIP file structure to understand the current codebase.
2.  **Plan**: Before generating code, briefly outline which files need changes.
3.  **Generate**: Provide full, copy-pasteable file contents when modifying code. Avoid partial diffs unless requested.

