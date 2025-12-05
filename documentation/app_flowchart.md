flowchart TD
    A[Extension icon clicked] --> B[Content JS captures DOM and styles]
    B --> C[Background JS forwards data]
    C --> D[NextJS API route receives data]
    D --> E[React Flow canvas initialized]
    E --> F[User edits UI visually]
    F --> G[User sends prompt to AI via chat]
    G --> H[lib/ai communicates with Gemini or Ollama]
    H --> I[AI returns generated code]
    I --> J[Monaco Editor displays code]
    J --> K[lib/db saves project to IndexedDB]
    K --> E