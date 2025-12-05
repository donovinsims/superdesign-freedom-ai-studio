# Project Requirements Document: superdesign-freedom-ai-studio

## 1. Project Overview

Freedom AI Studio is a two-part tool—a Next.js web application and a Chrome extension—built to simplify how developers and designers turn real websites into editable designs and production-ready code. With a single click in the browser, users can clone any page’s structure (DOM) and styles, import them into a visual canvas for drag-and-drop editing, then ask an AI to generate or refine HTML, CSS, and React code automatically.

The project addresses the tedious back-and-forth between designers, developers, and manual coding. By combining a visual editor (powered by React Flow) with AI models (Google Gemini and Ollama), Freedom AI Studio aims to reduce design-to-code time, lower entry barriers for non-coders, and offer an open-source alternative for rapid UI prototyping. Success means users can clone a site, reshape its look, and export clean, working code in minutes.

## 2. In-Scope vs. Out-of-Scope

**In-Scope (First Version):**
- Chrome extension (Manifest V3) to capture page DOM and computed styles.
- Next.js 14 web app with App Router and TypeScript.
- React Flow–based canvas for node-style visual editing of UI elements.
- AI chat interface to send prompts and receive code suggestions.
- Code panel powered by Monaco Editor for inspecting and tweaking generated code.
- IndexedDB (Dexie.js) for local project storage and offline access.
- Integration with Google Gemini API (cloud) and Ollama (local) as AI backends.
- Next.js API routes acting as pass-throughs or orchestrators for AI calls.
- Basic user interface: project dashboard, canvas workspace, chat & editor panes.

**Out-of-Scope (Later Phases):**
- User authentication, multi-user collaboration, or cloud sync.
- Mobile or desktop (Electron) versions.
- Support for additional browsers beyond Chrome.
- Built-in hosting or deployment pipelines for generated sites.
- Advanced AI model marketplace or plugin system.
- Server-side rendering of user projects (beyond Next.js defaults).

## 3. User Flow

A new user installs the Freedom AI Studio Chrome extension and clicks its icon on any website. The extension’s content script scrapes the page’s DOM nodes and computed styles, then sends this data to the Next.js web application (either via `chrome.runtime.sendMessage` or a dedicated API endpoint). The user is redirected or guided to the web app, where the imported page appears as draggable nodes in a React Flow canvas.

Inside the web app, users see a sidebar or dashboard listing existing projects and a main canvas area showing the cloned design. They can click and drag elements, adjust layout properties, or remove unwanted sections. At any point, they open the AI chat pane, type a request—like “Make this header responsive” or “Convert this to a styled React component”—and press send. AI responses appear in the chat and can be applied directly to the code in the Monaco Editor or reflected on the canvas. Changes are auto-saved to IndexedDB, so users can close and later reopen their designs offline.

## 4. Core Features

- **Website Cloning Extension**: Captures DOM structure + CSS, packages data, handles permissions (Manifest V3).
- **Project Dashboard**: List, create, rename, delete local projects saved in IndexedDB.
- **Visual Canvas Editor**: React Flow graph with nodes representing HTML elements; drag, resize, and property editing.
- **AI Chat Interface**: Bi-directional chat for prompts and code suggestions; supports context of current design state.
- **Code Editor Panel**: Monaco Editor integration for direct code inspection and manual tweaks.
- **AI Integration Layer** (`lib/ai.ts`): Abstracts calls to Google Gemini API and local Ollama model.
- **Local Database** (`lib/db.ts`): Schema and operations for IndexedDB via Dexie.js.
- **State Management**: Global stores with Zustand for project data, canvas state, and chat history.
- **Next.js API Routes**: Endpoints to proxy AI requests, handle extension messages, and orchestrate data flow.

## 5. Tech Stack & Tools

- **Frontend Web App**: Next.js 14 (App Router), React, TypeScript
- **Styling**: Tailwind CSS, shadcn/ui components (accessible UI library)
- **Visual Editing**: React Flow (node-based editor)
- **State Management**: Zustand (lightweight store)
- **Local Storage**: Dexie.js (wrapper for IndexedDB)
- **Code Editing**: Monaco Editor (VS Code–style editor in browser)
- **Browser Extension**: Chrome Manifest V3, vanilla JavaScript (content.js, background.js, popup.js)
- **AI Models**: Google Gemini API (cloud), Ollama (local LLM)
- **APIs & Orchestration**: Next.js API Routes (serverless functions)
- **Build Tools**: Node.js (v18+), npm/yarn, PostCSS
- **IDE Plugins (optional)**: ES Lint, Prettier, and TypeScript extensions for code quality

## 6. Non-Functional Requirements

- **Performance**: Canvas interactions <50ms per action; initial page load <2 seconds on broadband.
- **Scalability**: Support designs up to 500 nodes without major slowdowns (optimize React Flow rendering).
- **Security**: Follow Chrome extension best practices (least privileges); sanitize all AI outputs before injecting into DOM.
- **Reliability**: Gracefully handle AI API errors, network failures, and IndexedDB quota issues.
- **Usability**: Keyboard navigation and ARIA labels in UI; mobile-responsive fallback for non-canvas pages.
- **Privacy**: No project data sent to cloud by default; local-first storage unless user opts in.

## 7. Constraints & Assumptions

- **Environment**: Users run Chrome on desktop; IndexedDB is available and not blocked.
- **AI Availability**: Google Gemini API keys or a locally running Ollama server are preconfigured.
- **No Server Backend**: Except Next.js API routes acting as proxies; design data remains client-side.
- **Network**: Stable internet required for Gemini; Ollama allows offline use but may need installation.
- **Browser Limits**: IndexedDB storage limited by browser quotas (~50–100 MB).

## 8. Known Issues & Potential Pitfalls

- **API Rate Limits**: Gemini free tier may throttle; implement retries with exponential backoff and local caching of prompts.
- **Large DOM Snapshots**: Complex pages could exceed memory; batch transfer of nodes or selective cloning may be needed.
- **React Flow Performance**: Rendering hundreds of nodes can lag; use `memo`, virtualization, and avoid deep prop trees.
- **Extension-Web App Communication**: CORS or message-size limits; adopt structured messages and chunk large data payloads.
- **Error Handling**: Unstructured AI responses can break code injection; sanitize and validate JSON before use.
- **Browser Updates**: Future Chrome Manifest changes (V4) may require extension updates; keep an eye on Chrome Dev Blog.

---

This document serves as the single source of truth for AI-driven generation of all downstream artifacts—frontend guidelines, backend layouts, file structures, and CI/CD configurations—ensuring clarity and completeness for automation or human review.