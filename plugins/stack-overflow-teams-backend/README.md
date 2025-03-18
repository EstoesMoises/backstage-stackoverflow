# Stack Overflow Teams Backend Plugin

Backend counterpart of the Stack Overflow for Teams Plugin.

## Areas of Responsibility

The **Stack Overflow for Teams Backend plugin** is responsible for:

- **Indexing all questions** from the private Stack Overflow instance (an enhanced version of the existing community plugins in the Backstage repository).
- **Handling API requests** via ``createStackOverflowApi`` and ``createStackOverflowService`` to the Stack Overflow instance for retrieving:
  - `/users`
  - `/tags`
  - `/questions`
  - Posting new questions via `/questions`
- **Managing OAuth authentication flow** to securely access Stack Overflow private instances via ``createStackOverflowAuth``
- **Encrypts** the Stack Overflow Token before sending it as an http-only cookie to the frontend.

## OAuth Authentication Flow

The backend is the only component that directly utilizes the **encrypted Stack Overflow access tokens** for requests.

### **Authorization Flow Details**

#### **`/auth/start`**

- Generates **PKCE Code Verifier**.
- Hashes Code Verifier to obtain **Code Challenge**.
- Generates a **state** (random string).
- Stores **Code Verifier** and **State** in a **secure, HTTP-only cookie** accessible only to the server.

#### **`/callback`**

- Retrieves the stored **Code Verifier** and **State**.
- Validates that the received **state** matches the one from Stack Overflow's query string parameter.
- The backend requests an **Access Token** using the stored **Code Verifier**.
- Backend **encrypts the token**, using the JWT secret stored in memory.
- Stores the **encrypted Stack Overflow Access Token** in a **secure, HTTP-only cookie**.

## Installation

This plugin is installed via the `backstage-plugin-stack-overflow-teams-backend` package. To install it to your backend package, run the following command:

```bash
yarn --cwd packages/backend add backstage-plugin-stack-overflow-teams-backend
```

Then add the plugin to your backend in `packages/backend/src/index.ts`:

```ts
const backend = createBackend();

// ...

backend.add(import('backstage-plugin-stack-overflow-teams-backend'));
```

## Development

This plugin backend can be started in a standalone mode from directly in this package with `yarn start`. It is a limited setup that is most convenient when developing the plugin backend itself.

If you want to run the entire project, including the frontend, run `yarn dev` from the root directory.