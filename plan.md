# Development Plan: Multi-Model Support & .env API Keys

## Goals

1.  Refactor the LLM interaction logic to support multiple providers/models, starting with OpenAI.
2.  Replace the current `electron-store` based API key entry with a `.env` file for a smoother development workflow.

## Phase 1: API Key Management (.env Implementation)

**Objective:** Load API keys from a `.env` file instead of requiring manual input on startup.

1.  **Setup `.env` Loading:**
    - **File Creation:** Create a `.env.example` file outlining necessary keys (e.g., `OPENAI_API_KEY=YOUR_KEY_HERE`).
    - **Loading Logic:** Integrate `dotenv` loading early in the Electron main process startup (`electron/main.ts`). Ensure it loads before any code needing the API key.
    ```typescript
    // electron/main.ts (near top)
    import dotenv from "dotenv";
    dotenv.config();
    ```
2.  **Update Key Retrieval:**
    - **Modify `problemHandler.ts`:** Change `store.get("openaiApiKey")` to `process.env.OPENAI_API_KEY`. Handle cases where the key might be missing.
    - **Check Other Uses:** Search codebase for other instances of `store.get("openaiApiKey")` and update them similarly if they exist.
3.  **Remove Manual Key Entry UI:**
    - **Remove `ApiKeyAuth.tsx`:** Delete `src/components/ApiKeyAuth.tsx`.
    - **Update `App.tsx`:** Remove the authentication state (`isAuthenticated`), the logic calling `setApiKey`, and the conditional rendering related to `ApiKeyAuth`. Assume authenticated if the app loads (key should be in `.env`).
    - **Update IPC:** Remove the `setApiKey` IPC handler in `electron/ipcHandlers.ts` and its definition in `electron/preload.ts`. Remove the `onUnauthorized` event handling if it's solely tied to the missing key _on input_, though keep checks for invalid/out-of-credit keys during API calls.
4.  **Update Documentation:**
    - Modify `README.md` setup instructions to mention creating a `.env` file from `.env.example` and adding the API key there.
    - Update relevant sections in `memory-bank/` files (`techContext.md`, `systemPatterns.md`) to reflect the use of `.env`.

## Phase 2: Refactor for Multi-Model Support

**Objective:** Abstract LLM interactions to allow easy addition of new providers/models.

1.  **Design Abstraction Layer:**
    - **Interfaces:** Define TypeScript interfaces for the core LLM operations required (e.g., processing images+text for problem extraction, generating solutions, debugging code).
      - `LLMProvider` interface (e.g., methods like `extractProblem`, `generateSolution`, `debugCode`).
      - Common input/output data structures for these methods.
    - **Configuration:** Define how the active provider and model will be selected (e.g., initially via `.env` variables like `LLM_PROVIDER=openai`, `OPENAI_MODEL=gpt-4o-mini`).
2.  **Implement OpenAI Provider:**
    - **Create Module:** Create a new file (e.g., `electron/llm/openaiProvider.ts`).
    - **Implement Interface:** Implement the `LLMProvider` interface using the OpenAI API.
    - **(Recommended)** **Refactor to `openai` Library:** Replace the current `axios` implementation with the official `openai` Node.js library for better type safety, maintainability, and features.
      - `npm install openai`
      - Instantiate the OpenAI client using the API key from `process.env`.
      - Use `openai.chat.completions.create(...)` instead of `axios.post`.
3.  **Integrate Abstraction Layer:**
    - **Modify `problemHandler.ts` / Handlers:** Update the functions (`extractProblemInfo`, `generateSolutionResponses`, `debugSolutionResponses`, etc.) to:
      - Determine the active provider (based on config/`.env`).
      - Instantiate the corresponding provider implementation (e.g., `OpenAIProvider`).
      - Call the methods defined in the `LLMProvider` interface instead of directly calling `axios` or the `openai` library.
4.  **Refine API/IPC Communication:**
    - Ensure the data passed back to the renderer process via IPC (`mainWindow.webContents.send(...)`) uses the common data structures defined in the abstraction layer. Update `preload.ts` and front-end listeners (`src/App.tsx`, pages) if necessary to handle any changes in data format.
5.  **Testing:**
    - Thoroughly test the refactored OpenAI flow: screenshot capture -> problem extraction -> solution generation -> debugging.
6.  **Update Documentation:**
    - Document the new abstraction layer (`LLMProvider` interface) in `memory-bank/systemPatterns.md`.
    - Explain how to configure the active provider/model (`techContext.md` or `README.md`).
    - Briefly mention how a new provider could be added by implementing the interface.

## Considerations

- **Error Handling:** Implement robust error handling for missing `.env` variables, invalid API keys, network issues, and API errors from LLM providers.
- **Configuration Flexibility:** While starting with `.env`, consider if a UI-based configuration for model selection might be needed later.
- **Security:** Ensure `.env` remains in `.gitignore`. Avoid committing API keys.
- **Dependency Management:** Add `openai` as a dependency if switching from `axios`.
