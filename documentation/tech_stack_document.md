# Tech Stack Document

This document explains the technology choices behind **superdesign-freedom-ai-studio**, an AI-driven UI design and code generation platform. It covers the tools and libraries used, how they work together, and why they were chosen, in simple terms for non-technical readers.

---

## 1. Frontend Technologies

The frontend is everything users interact with in their browser. Here are the main tools:

- **Next.js 14 (App Router)**
  • A React-based framework that handles page routing and server-side rendering, making pages load quickly and improving SEO.
  • Built-in API routes let us create lightweight backend endpoints without a separate server.

- **TypeScript**
  • Adds type checking to JavaScript, catching bugs early and making code easier to understand and maintain.

- **Tailwind CSS & shadcn/ui**
  • Tailwind is a utility-first CSS framework that helps us style components quickly with small, reusable classes.
  • shadcn/ui provides pre-built, accessible UI components that follow best design practices—saving time and ensuring consistency.

- **React Flow**
  • A library for building node-based editors and diagrams. It powers the drag-and-drop canvas where users visually edit cloned websites.

- **Zustand**
  • A lightweight state management library for React. It keeps track of application state (e.g., current project data, UI selections) in a simple and performant way.

- **Dexie.js (IndexedDB)**
  • An easy interface for the browser’s IndexedDB storage. It persistently saves user projects locally so work is never lost and can be accessed offline.

- **Monaco Editor**
  • The same code editor that powers VS Code. It allows users to review and tweak generated code (HTML, CSS, JavaScript/React) within the app.

Together, these tools create a responsive, interactive, and user-friendly interface where visual design and code editing blend seamlessly.

---

## 2. Backend Technologies

Although the project is frontend-focused, it relies on a few backend elements to manage data and AI interactions:

- **Next.js API Routes**
  • Act as simple serverless functions within the Next.js app. They serve as intermediaries between the frontend and external services.

- **AI Services (Gemini & Ollama)**
  • **Google Gemini API** (cloud) for high-quality AI code generation.
  • **Ollama** for running large language models locally, giving users an offline or privacy-focused option.
  • These services handle tasks like generating component code, improving styles, fixing bugs, and adding documentation based on user prompts.

- **IndexedDB via Dexie.js**
  • Stores project data (cloned DOM structures, style information, AI chat history) on the client side—no separate database server is required.

By using Next.js API routes and external AI APIs, we minimize traditional server maintenance while keeping the system flexible and easy to extend.

---

## 3. Infrastructure and Deployment

To keep the project reliable and easy to deploy, we chose the following infrastructure tools:

- **Version Control: Git & GitHub**
  • All code is stored in Git repositories on GitHub, enabling collaboration, code reviews, and history tracking.

- **CI/CD Pipeline: GitHub Actions**
  • Automates testing and deployment whenever code is pushed, ensuring changes work as expected before going live.

- **Hosting Platform: Vercel**
  • Optimized for Next.js apps, providing automatic builds, previews, and global content delivery for fast page loads.

- **Chrome Web Store**
  • The extension is packaged and published here, making it easy for users to install and update.

These choices guarantee that new features and fixes can be released quickly, with minimal downtime.

---

## 4. Third-Party Integrations

We connect to several external services to extend functionality:

- **Google Gemini API**
  • Cloud-based AI for code generation and assistance.

- **Ollama**
  • Local AI model runner, allowing offline use or data privacy.

- **Chrome Extension Manifest V3**
  • Defines how the extension interacts with web pages safely and efficiently.

Each integration enriches the user experience—AI services automate coding tasks, while the Chrome extension captures live website layouts for editing.

---

## 5. Security and Performance Considerations

We’ve implemented measures to keep user data safe and the app running smoothly:

Security:
- **Manifest V3 & Content Script Sandboxing** in the Chrome extension limit access to only needed permissions and isolate injected code.
- **Data Protection:** All project data stays in the user’s browser (IndexedDB), so nothing is sent to unknown servers without consent.
- **Secure API Calls:** AI requests go through API routes, allowing us to handle credentials safely and avoid exposing keys in the frontend.

Performance:
- **Server-Side Rendering (SSR)** via Next.js speeds up initial page loads and improves SEO.
- **Tailwind PurgeCSS** removes unused CSS classes, keeping stylesheets small.
- **React Flow Optimization:** Uses memoization to reduce unnecessary re-renders on complex canvases.
- **Lazy Loading & Code Splitting:** Next.js automatically splits code so users only download what they need when they need it.

These optimizations ensure a responsive experience, even on large designs or slower connections.

---

## 6. Conclusion and Overall Tech Stack Summary

**superdesign-freedom-ai-studio** combines modern web technologies and AI services to create a seamless design-to-code workflow. By choosing a mostly frontend-driven approach with Next.js, TypeScript, and IndexedDB, we ensure ease of development and local project management. Integrating cloud-based and local AI models (Gemini & Ollama) provides flexibility around performance, cost, and privacy. Finally, hosting on Vercel with GitHub CI/CD and publishing the extension on the Chrome Web Store guarantees reliable and fast delivery of updates.

Key takeaways:
- A powerful, interactive UI powered by React Flow, Tailwind CSS, and shadcn/ui.
- Robust code editing via Monaco Editor and AI-driven code generation.
- Local-first data storage with Dexie.js and IndexedDB.
- Flexible AI backend supporting both cloud and local models.
- Streamlined deployment and version control using GitHub, GitHub Actions, and Vercel.

This combination of technologies aligns perfectly with our goal: an open-source, AI-powered studio that makes cloning, editing, and coding UIs easy and efficient for everyone.