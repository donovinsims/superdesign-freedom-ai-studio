# Backend Structure Document

This document outlines the backend architecture, database setup, API design, hosting environment, infrastructure components, security measures, monitoring, and maintenance strategies for the **superdesign-freedom-ai-studio** project. It is written in everyday language so that anyone can understand how the backend is organized and operates.

## 1. Backend Architecture

**Overview**
The backend is implemented as part of the Next.js web application. It consists mainly of API routes that:

- Act as a bridge between the frontend (web app and Chrome extension) and external AI services.  
- Handle incoming data from the extension (cloned website DOM).  
- Manage project data in the browser’s local database (IndexedDB).

**Design Patterns & Frameworks**

- Next.js API Routes (serverless style): Each endpoint is a small function that runs on demand.  
- Modular structure: `lib/ai.ts` for AI calls, `lib/db.ts` for database logic, keeping concerns separate.  
- Typed interfaces (TypeScript) for clear data contracts.

**Scalability, Maintainability & Performance**

- **Scalability:**  
  - Serverless functions auto-scale with traffic.  
  - Supports both cloud (Gemini) and local (Ollama) AI models, distributing load.  
- **Maintainability:**  
  - Clear folder structure (`api/`, `lib/`, `components/`).  
  - Single-purpose modules make updates easier.  
- **Performance:**  
  - Edge caching (via hosting provider) for static assets and API responses where appropriate.  
  - Local-first data storage avoids repeated round-trips for project data.

## 2. Database Management

**Database Technology**

- **Type:** NoSQL, client-side database.  
- **System:** IndexedDB accessed through Dexie.js.

**Data Structure & Access**

- **Dexie.js** abstracts IndexedDB complexity, offering a simple API for CRUD operations.  
- Data is stored locally in the user’s browser, enabling offline work and quick access.  
- All project-related data (design nodes, edges, AI chat history) lives in this database.

**Data Management Practices**

- Versioned schema: Dexie migrations handle upgrades when the data model changes.  
- Transactional updates: Ensures that related changes (e.g., node edits + chat logs) succeed or roll back together.  
- Indexed fields on project IDs and timestamps for quick lookups.

## 3. Database Schema

The schema below describes how project data is organized in human-friendly terms, followed by the SQL-like definition that Dexie uses internally.

Human-readable schema:

- **Projects Table**  
  • id: Unique identifier for the design project  
  • name: User-defined project name  
  • createdAt: Timestamp when the project was first saved  
  • updatedAt: Timestamp of the last update  
  • nodes: Array of visual nodes (position, type, properties)  
  • edges: Array of connections between nodes  
  • chatHistory: Array of AI chat messages with timestamps and roles  

Dexie schema definition (similar to SQL):

```
Projects:
  ++id,         // auto-incremented primary key
  &name,       // unique or user-friendly identifier
  createdAt,
  updatedAt,
  nodes,
  edges,
  chatHistory
```

> Note: `&` marks a unique index, `++` auto-increments the primary key.

## 4. API Design and Endpoints

All backend endpoints live under `/api/` in the Next.js app. They use RESTful conventions:

- **/api/ai/generate**  
  • Method: POST  
  • Purpose: Send a prompt and design context to the selected AI model (Gemini or Ollama) and return the generated code or suggestions.  
  • Payload: `{ model: 'gemini'|'ollama', prompt: string, context: DesignState }`

- **/api/ai/models**  
  • Method: GET  
  • Purpose: List available AI models and their capabilities.  

- **/api/projects/save**  
  • Method: POST  
  • Purpose: (Optional) Proxy call to store data in a remote database if cloud sync is enabled in the future.  
  • Payload: Full project object (same structure as in IndexedDB).

- **/api/extension/import**  
  • Method: POST  
  • Purpose: Receive cloned webpage data from the Chrome extension and return an initial design state.  
  • Payload: `{ dom: string, styles: object }`

- **/api/health**  
  • Method: GET  
  • Purpose: Basic health check for automated monitoring or uptime tests.

These endpoints are stateless and scale automatically. CORS and authentication checks (if any) are applied at the route level.

## 5. Hosting Solutions

**Environment**

- **Cloud Provider:** Vercel (or a similar platform that supports Next.js serverless functions).  
- **Runtime:** Serverless Node.js functions for API routes, managed by the provider.  
- **Storage:** No external storage needed for IndexedDB (entirely client-side).

**Benefits**

- **Reliability:** Automatic failover and global edge network.  
- **Scalability:** Unlimited horizontal scaling of API routes.  
- **Cost-effectiveness:** Pay-as-you-go pricing; minimal costs when idle.

## 6. Infrastructure Components

- **CDN (Content Delivery Network):**  
  - Serves static assets (JavaScript, CSS) from the edge for fast load times worldwide.  
- **Serverless Functions:**  
  - Next.js API routes run on demand with automatic scaling.  
- **Load Balancing:**  
  - Handled by the cloud provider’s edge network; no manual setup required.  
- **Caching Layer:**  
  - Edge caching for GET endpoints (e.g., `/api/ai/models`, `/api/health`).  
- **Client-side Cache:**  
  - IndexedDB for speed and offline support.

Together, these components deliver fast responses, high availability, and a smooth user experience.

## 7. Security Measures

- **Authentication & Authorization:**  
  - Currently open access for ease of use.  
  - Future extension could add API keys or OAuth for user-level projects.  
- **Data Encryption:**  
  - All API calls to Gemini or Ollama use HTTPS.  
  - Sensitive environment variables (AI API keys) stored securely in the hosting platform.  
- **CORS Policies:**  
  - Restrict API access to the project’s own domain and the Chrome extension origin.  
- **Input Validation:**  
  - Sanitize all incoming data from the extension and user prompts to prevent injection attacks.  
- **Extension Security:**  
  - Minimal permissions in `manifest.json`.  
  - Content script only reads DOM and styles, does not modify the page.

These measures protect user data and maintain compliance with best practices.

## 8. Monitoring and Maintenance

- **Monitoring Tools:**  
  - Vercel Analytics or similar for request rates, latencies, and errors.  
  - Sentry (or LogRocket) for capturing runtime exceptions in API routes and client code.  
- **Logging:**  
  - Serverless functions log to the provider’s built-in logging system.  
- **Alerts:**  
  - Set up alerting on high error rates or latency spikes.  
- **Maintenance Strategy:**  
  - Regular dependency updates (Next.js, Dexie, AI SDKs).  
  - Scheduled audits of API usage and cost.  
  - Periodic review of IndexedDB migrations as the schema evolves.

## 9. Conclusion and Overall Backend Summary

The backend of **superdesign-freedom-ai-studio** is built on serverless Next.js API routes and a client-side IndexedDB database. It leverages external AI services (Google Gemini and Ollama) to handle code generation tasks. Hosted on a modern platform like Vercel, it benefits from automatic scaling, global CDN coverage, and pay-as-you-go pricing.

Key strengths:

- Seamless AI integration via well-defined API routes.  
- Local-first data storage offering offline support and instant responsiveness.  
- Clear separation of concerns in code organization, making the backend easy to maintain and extend.

This setup aligns perfectly with the project’s goal of providing an open, flexible, and performant AI-driven design studio, minimizing traditional server overhead while maximizing user experience and reliability.