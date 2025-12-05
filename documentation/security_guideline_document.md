# Security Guidelines for superdesign-freedom-ai-studio

This document provides actionable security best practices for the **superdesign-freedom-ai-studio** repository—comprising a Next.js web application and a Chrome extension—to ensure a secure, robust, and privacy-preserving design-to-code platform.

---

## 1. Security by Design: Core Principles

- **Embed Security Early**: Integrate these controls at design, implementation, testing, and deployment phases.  
- **Least Privilege**: Grant only the minimum permissions required (both for the Chrome extension and the Next.js application).  
- **Defense in Depth**: Layer multiple safeguards so that no single failure leads to a full compromise.  
- **Fail Securely**: On errors or timeouts, avoid leaking sensitive details; present generic error messages in production.  
- **Secure Defaults**: Configure all features (CSP, cookies, headers, DB) with the most restrictive, secure settings by default.

---

## 2. Authentication & Access Control

> *Even in a local-first app, plan for future multi-user or cloud sync features.*

- **Protect API Routes**: If introducing user accounts or cloud sync, enforce robust authentication (e.g., OAuth2, JWT with `exp`, secure signing).  
- **Role-Based Access Control (RBAC)**: Define roles (e.g., admin, designer, viewer) and enforce server-side permission checks on every sensitive operation.  
- **Session Management** (if applicable):  
  - Use `HttpOnly`, `Secure`, `SameSite=Strict` cookies.  
  - Implement idle and absolute timeouts.  
  - Prevent session fixation by rotating identifiers on login.  
- **Multi-Factor Authentication (MFA)**: Strongly consider for administrative or cloud-sync account access.

---

## 3. Input Validation & Output Encoding

- **Sanitize Extension Messages**: Any data sent from `content.js` to your app or background script must be strictly validated against a JSON schema.  
- **Prevent Injection**:  
  - Use parameterized queries when interacting with IndexedDB (Dexie.js protects against NoSQL injection but validate all keys/values).  
  - Avoid `eval()`, `new Function()`, or dynamic template compilation—especially for user-supplied HTML/CSS.  
- **Context-Aware Encoding**: Escape user content before inserting into the DOM, React Flow node labels, or chat UI.  
- **Template Injection**: Do not interpolate raw user input into your React components or Moncao Editor without sanitization.  
- **File Uploads (if future feature)**: Validate extension, MIME type, size; store outside public webroot.

---

## 4. Data Protection & Privacy

- **Encrypt In Transit**:  
  - Serve the Next.js app over HTTPS (TLS 1.2+).  
  - Use TLS for all calls to Google Gemini or Ollama.  
- **Secrets Management**:  
  - Store API keys in environment variables or a secrets manager (e.g., AWS Secret Manager, Vault)—never in source code.  
  - Rotate AI service credentials regularly.  
- **IndexedDB Storage**:  
  - Treat all locally stored data as potentially sensitive; avoid storing PII in cleartext.  
  - Consider encrypting IndexedDB entries if storing user-provided sensitive data.  
- **Logging & Errors**:  
  - Strip stack traces and internal paths from client-facing errors.  
  - Log only sanitized, non-PII data for debugging.

---

## 5. API & Service Security

- **HTTPS Enforcement**:  
  - Redirect all HTTP traffic to HTTPS.  
  - HSTS header with `max-age=31536000; includeSubDomains; preload`.  
- **CORS Policy**:  
  - Only allow trusted origins (e.g., your domain) for API routes.  
  - Disallow wildcard (`*`) origins, especially for endpoints that accept extension payloads.  
- **Rate Limiting & Throttling**:  
  - Apply rate limits on AI proxy endpoints to prevent abuse and DoS.  
- **API Versioning**:  
  - Version your AI orchestration endpoints (`/api/v1/ai`) to manage future changes securely.

---

## 6. Chrome Extension Security

- **Least-Privilege Manifest** (`manifest.json`):  
  - Restrict `host_permissions` to only needed domains (avoid `<all_urls>`).  
  - Only request the minimal set of permissions (e.g., `activeTab`, `storage`) necessary for cloning.  
- **Content Script Hardening**:  
  - Avoid injecting inline scripts—use external scripts with CSP in the extension context.  
  - Validate that captured DOM/style data cannot contain malicious payloads before forwarding.  
- **Secure Messaging**:  
  - Use `chrome.runtime.sendMessage` with a strict message schema.  
  - Verify `sender.origin` and `sender.id` in background code.  
- **Extension CSP**:  
  - Define a Content Security Policy in `manifest.json` under `content_security_policy` to disallow unsafe-eval and inline scripts.

---

## 7. Web Application Security Hygiene

- **Security Headers**:  
  - `Content-Security-Policy`: Restrict sources for scripts, styles, images, and frames.  
  - `X-Frame-Options: DENY` or `frame-ancestors 'none'`.  
  - `X-Content-Type-Options: nosniff`.  
  - `Referrer-Policy: no-referrer-when-downgrade`.  
- **CSRF Protection**:  
  - For any state-changing form or API call, implement a synchronizer token (NextAuth or custom).  
- **Secure Cookies**:  
  - `HttpOnly`, `Secure`, `SameSite=Strict`.  
- **Subresource Integrity (SRI)**:  
  - If using CDN-hosted scripts (e.g., React, Tailwind), include integrity hashes.

---

## 8. Infrastructure & Configuration

- **Secure Defaults**:  
  - Disable Next.js telemetry and debug in production.  
  - Turn off verbose error overlays.  
- **Dependency Management**:  
  - Maintain and commit `package-lock.json`/`pnpm-lock.yaml`.  
  - Run SCA tools (e.g., `npm audit`, Dependabot, Snyk) regularly.  
- **Patch Management**:  
  - Keep Node.js, Next.js, React Flow, and all libraries up to date.  
- **File Permissions**:  
  - In CI/CD and production, ensure `.env` and build artifacts have restrictive file system permissions.

---

## 9. Monitoring, Testing & Incident Response

- **Security Testing**:  
  - Implement automated vulnerability scans in CI (Snyk, OWASP Dependency-Check).  
  - Add unit and integration tests covering input validation, message passing, and AI proxy endpoints.  
- **Penetration Testing**:  
  - Periodically engage in manual or automated pen-testing against staging.  
- **Incident Plan**:  
  - Define roles and runbooks for handling security incidents.  
  - Establish alerting for anomalous API usage or extension permission changes.

---

Adherence to these guidelines will harden **superdesign-freedom-ai-studio** against common threats, protect user data, and lay the foundation for future secure enhancements and a trustworthy user experience.