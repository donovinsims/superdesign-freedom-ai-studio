# superdesign-freedom-ai-studio App Flow Document

## Onboarding and Sign-In/Sign-Up

When a user first encounters the Freedom AI Studio web application, they arrive at a clean landing page that describes the key features: cloning websites, visual editing, AI-powered code generation, and local project management. There is no requirement to sign in or create an account because all data is stored locally in the browser. Instead of an account system, the user simply clicks a button labeled “Get Started” or “Open Studio,” which takes them directly into the application. Password recovery and social logins are not relevant because there is no user authentication layer. For the Chrome extension, the user installs it from the Chrome Web Store. After installation, they see the extension icon in the browser toolbar. Clicking that icon opens a small popup interface that guides them to either capture the current page or navigate to the web application.

## Main Dashboard or Home Page

Once the user has clicked “Open Studio” on the landing page, they arrive at the main dashboard. The top of the screen contains a header bar showing the Freedom AI Studio logo on the left and a settings icon on the right. Below the header, a collapsible sidebar appears on the left side of the screen. This sidebar lists saved projects with their names and creation dates, and includes a button at the bottom to create a new project. The rest of the screen is divided into two main panels: a wide canvas area on the right, where visual editing takes place, and a narrower side panel on the right that toggles between a chat interface and a code editor. From this main dashboard, users can launch any feature by selecting a project from the sidebar, clicking “New Project,” opening the chat tab, or switching to the code editor tab.

## Detailed Feature Flows and Page Transitions

### Cloning Websites with the Chrome Extension

To begin cloning a website, the user navigates to any page in their browser and clicks the Freedom AI Studio extension icon. The popup presents two buttons: one to capture the current page and another to open the web app. When the user clicks “Capture Page,” the extension’s content script runs. It collects the DOM structure and computed styles of the page and sends that data to the background script. The background script then opens a new tab pointing to the web application with the captured data encoded in a temporary workspace. The web app automatically detects this incoming data, creates a new project named after the page URL, and populates the canvas with nodes representing each UI element.

### Visual UI Editing in the Web App

After the clone data loads, the user sees the React Flow canvas filled with nodes corresponding to page elements. They can click and drag any node to reposition it, resize it by grabbing handles, or open a properties panel by double-clicking. The properties panel allows the user to adjust style attributes such as colors, fonts, margins, and padding. As adjustments are made, the canvas updates in real time, reflecting the visual changes instantly. When the user clicks outside the canvas or selects a different tool, the side panel collapses back into its tabbed view.

### AI-Powered Code Generation and Chat

With the design in place, the user can switch to the chat tab in the right side panel to access the AI assistant. They type requests like “Generate React components for this design” or “Fix styling inconsistencies in my header.” When the user submits a prompt, the front end sends the current canvas state along with the prompt to the AI integration library. Depending on the user’s preference, the library will call either the Google Gemini API or the local Ollama model. While the AI processes, a spinner appears. Once the response arrives, the assistant displays code snippets, suggestions, or even inline fixes directly in the chat window. The user can click a snippet’s “Apply to Canvas” button to update the visual editor with the AI’s proposed changes.

### Code Editor and Manual Tweaks

If the user wants to fine-tune the generated code, they switch to the code editor tab in the right side panel. This tab hosts the Monaco Editor loaded with the current project’s code files. The editor shows a file tree at the top, listing HTML, CSS, and JavaScript or React component files. Clicking a filename opens its contents in the editor. The user can type freely, see syntax highlighting, and enjoy code completion. Any edits made in the code editor can be applied back to the canvas by clicking the “Sync to Canvas” button at the bottom of the editor. This two-way sync ensures that manual adjustments and visual design stay in harmony.

### Local Project Management

All user actions and project changes are stored in the browser’s IndexedDB database, managed by Dexie.js. When a new project is created—either by cloning a website, starting from a blank canvas, or importing a project file—the data is saved and appears in the project list in the sidebar. If the user reloads the page or closes and reopens the browser, the last state of each project is restored. The user can rename or delete projects by right-clicking a project name in the sidebar, which opens a context menu with options for “Rename” and “Delete.” These operations are performed instantly and update the database directly.

## Settings and Account Management

Clicking the settings icon in the header opens a modal window with tabs for preferences and AI configuration. In Preferences, users set their default canvas grid size, toggle dark mode, and choose their favorite color palette preset. In AI Configuration, they select between Gemini and Ollama, enter any required API keys for Gemini, or set the local Ollama endpoint URL. They can also adjust prompt templates and AI temperature settings here. Once settings are updated, the user clicks “Save” to close the modal and return to the canvas. All settings are stored in local storage and take effect immediately without reloading the page.

## Error States and Alternate Paths

If the AI call fails due to a network error or invalid API key, the chat panel shows a red error banner with a brief message and a “Retry” button. Clicking “Retry” repeats the call with the same prompt. If cloning a page fails because the content script did not inject properly, the extension popup displays an alert instructing the user to refresh the page or reinstall the extension. In the web app, if IndexedDB is unavailable or full, a modal warns the user that local storage is not accessible. The user can choose to clear older projects to free space or export data to a JSON file for backup. Throughout the app, invalid input in forms—such as empty project names or bad URLs—triggers inline validation messages prompting the user to correct the error.

## Conclusion and Overall App Journey

In a typical session, the user installs the Chrome extension, captures a live website, and jumps straight into Freedom AI Studio without ever creating an account. On the main dashboard, they open or create a project and use the React Flow canvas to adjust the layout visually. Next, they use the AI chat to generate or refine code, apply changes back to the canvas, and then manually fine-tune code in the embedded editor. Their work is saved automatically in the browser, and they can revisit any project at any time. When they need to adjust preferences or switch AI providers, they use the settings modal. If anything goes wrong, clear error messages guide them back on track. By the end of their session, they have a polished design with matching code, all without leaving the app or signing into any external service.