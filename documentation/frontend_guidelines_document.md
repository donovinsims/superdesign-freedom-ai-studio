# Frontend Guideline Document

This document outlines the frontend setup, architecture, and best practices for the **superdesign-freedom-ai-studio** project. It’s written in everyday language so anyone—even without a heavy technical background—can understand how the frontend is built, styled, and organized.

---

## 1. Frontend Architecture

### 1.1 Overall Structure
- The project is split into two parts:
  1. **Web Application** (`superdesign-freedom-web`) built with Next.js 14 (App Router).
  2. **Chrome Extension** (`superdesign-freedom-extension`) using Manifest V3.

### 1.2 Key Frameworks and Libraries
- **Next.js 14** (App Router): Handles routing, server-side rendering, and API endpoints.
- **TypeScript**: Adds type safety, reducing bugs and making the code easier to understand.
- **Tailwind CSS + shadcn/ui**: Utility-first styling plus a set of accessible, pre-built UI components.
- **React Flow**: Powers the drag-and-drop, node-based canvas for visual editing.
- **Zustand**: Manages global state in a simple, performant way.
- **Dexie.js (IndexedDB)**: Stores user projects locally in the browser.
- **Vanilla JavaScript** (Chrome Extension): Captures and transfers webpage data.

### 1.3 Scalability, Maintainability, Performance
- **Modular Code**: Components and utilities are organized in clear folders (e.g., `components/`, `lib/`, `store/`). This structure makes it easy to add, remove, or update features.
- **Separation of Concerns**: AI logic (`lib/ai.ts`), database logic (`lib/db.ts`), and UI components live in separate places.
- **Performance**: Next.js handles code splitting and server-side rendering. React Flow virtualizes canvas elements. Tailwind’s JIT mode removes unused CSS.

---

## 2. Design Principles

1. **Usability**: Interfaces are intuitive. Buttons, forms, and menus behave as users expect.
2. **Accessibility**: We follow WCAG guidelines—semantic HTML, ARIA attributes, and keyboard navigation.
3. **Responsiveness**: Layouts adapt smoothly from mobile to desktop screens.
4. **Consistency**: A unified color palette, typography, and spacing ensure a cohesive look.

How we apply them:
- Form inputs have clear labels and focus states.
- All interactive elements are reachable via keyboard (Tab navigation).
- UI scales using responsive utility classes in Tailwind (e.g., `sm:`, `md:`, `lg:`).
- shadcn/ui components come with built-in accessibility features.

---

## 3. Styling and Theming

### 3.1 Styling Approach
- **Tailwind CSS**: Utility-first classes (`bg-primary`, `text-lg`, `p-4`) for rapid styling.
- **No separate CSS files**—all styles live alongside markup in JSX.
- Tailwind’s configuration (`tailwind.config.ts`) centralizes colors, fonts, and breakpoints.

### 3.2 Theming
- We define a theme in `tailwind.config.ts` for consistent colors and typography.
- Dark mode is enabled via the `class` strategy (`<html class="dark">`).

### 3.3 Visual Style
- **Overall Style**: Modern flat design with subtle glassmorphism on panels (semi-transparent backgrounds and soft shadows).

### 3.4 Color Palette
| Role        | Color Name  | Hex      |
| ----------- | ----------- | -------- |
| Primary     | Deep Blue   | #1E3A8A  |
| Secondary   | Soft Indigo | #5B21B6  |
| Accent      | Bright Teal | #14B8A6  |
| Background  | White        | #FFFFFF  |
| Surface     | Light Gray  | #F3F4F6  |
| Text Primary| Almost Black| #111827  |
| Success     | Green       | #10B981  |
| Warning     | Amber       | #F59E0B  |
| Error       | Red         | #EF4444  |

### 3.5 Typography
- **Font Family**: Inter, sans-serif (imported via Google Fonts).
- Base font sizes and line heights set in Tailwind (`font-base`, `leading-relaxed`).

---

## 4. Component Structure

### 4.1 Organization
- **`components/`**: Top-level folder with subfolders by domain:
  - `canvas/` (React Flow canvas)
  - `chat/` (AI chat UI)
  - `editor/` (Monaco code editor)
  - `ui/` (general UI building blocks)

- Each component folder contains:
  - `index.tsx` (main component)
  - `types.ts` (props or model types)
  - Optional `tests/` folder for unit tests

### 4.2 Reusability
- Components are small and focused (one responsibility each).
- Shared UI pieces (buttons, modals, cards) live in `components/ui/` and are used across the app.
- We export components centrally via barrel files (`components/index.ts`).

### 4.3 Benefits of Component-Based Architecture
- **Maintainability**: Changes in one component don’t ripple unpredictably.
- **Testability**: Isolated components are easier to unit-test.
- **Collaboration**: Developers can work on separate pieces without conflicts.

---

## 5. State Management

### 5.1 Library: Zustand
- **Why Zustand**: Minimal boilerplate, good performance, and easy to learn.

### 5.2 Store Structure
- **`store/projectStore.ts`**: Manages current project data and persistence status.
- **`store/canvasStore.ts`**: Tracks node/edge states, selection, zoom level.
- **`store/chatStore.ts`**: Holds chat history and AI response states.

### 5.3 Sharing State
- Components subscribe only to the slices they need.
- State updates trigger re-render only where necessary.
- Persistence: ProjectStore syncs to IndexedDB via Dexie whenever relevant data changes.

---

## 6. Routing and Navigation

### 6.1 Next.js App Router
- **File-based Routing**: Pages live under `app/`. For example:
  - `app/page.tsx` → `/`
  - `app/projects/[id]/page.tsx` → `/projects/:id`

- **Layouts**: Shared layouts (`app/layout.tsx`) wrap nested routes.

### 6.2 Navigation Patterns
- **Next.js `<Link>` component** for client-side transitions.
- **Dynamic Routes** for project-specific pages and settings.
- **Sidebar Menu** or Top Nav bar guides users through: Home → Projects → Editor → Settings.

---

## 7. Performance Optimization

### 7.1 Code Splitting & Lazy Loading
- **Dynamic Imports** (`next/dynamic`) for heavy components (React Flow, Monaco Editor).

### 7.2 Asset Optimization
- **Image Optimization**: Next.js `<Image>` component.
- **SVG Icons**: Inlined or imported as React components.

### 7.3 Runtime Optimizations
- **React Flow Memoization**: Use `React.memo` for node/edge renderers.
- **Debouncing**: Limit expensive updates (e.g., resizing, AI calls).

### 7.4 CSS Tree-Shaking
- Tailwind’s JIT mode ensures unused classes aren’t included in production builds.

---

## 8. Testing and Quality Assurance

### 8.1 Unit and Integration Tests
- **Jest + React Testing Library**:
  - Unit tests for utility functions (`lib/ai.ts`, `lib/db.ts`).
  - Component tests for UI pieces (buttons, chat bubbles).
  - Integration tests for key flows (e.g., chat submission → AI response).

### 8.2 End-to-End (E2E) Tests
- **Cypress or Playwright**:
  - Test full user journeys: cloning a website, editing nodes, generating code.

### 8.3 Linting and Formatting
- **ESLint** with recommended rules for TypeScript and React.
- **Prettier** for consistent code style.
- **Husky** pre-commit hooks to run lint and tests automatically.

---

## 9. Conclusion and Overall Frontend Summary

The **superdesign-freedom-ai-studio** frontend is built on a solid, modern foundation:
- A **Next.js + TypeScript** web app for rich UI and API handling.
- A **Chrome Extension** to capture real-world websites.
- **Tailwind CSS** and **shadcn/ui** for rapid, accessible styling.
- **React Flow** for a flexible visual editor.
- **Zustand** and **Dexie.js** for state and local storage.
- **AI integration** via Gemini and Ollama for code generation.

This architecture supports scalability, maintainability, and performance while keeping the codebase approachable. By following these guidelines—from component structure to testing—you’ll ensure a consistent developer experience and an intuitive, reliable product for end users.

Happy coding!