# @auth-service/embedded-auth

Browser helpers for embedding Auth Service `GET /embedded-login` in an iframe with **optional** postMessage **v2** (envelope, `EMBED_READY`, `INIT`, `THEME_UPDATE`).

- Protocol: in this repo, [`docs/EMBEDDED_IFRAME_PROTOCOL.md`](../../docs/EMBEDDED_IFRAME_PROTOCOL.md).
- The OAuth client must have **Embedded iframe login** and **parent origins** set; for v2, enable **Embedded protocol v2** in the admin UI.

## Install

**From npm** (when published):

```bash
npm i @auth-service/embedded-auth
```

**From GitHub** (monorepo subdirectory; replace `OWNER` / `REPO` / branch):

```bash
npm install "github:OWNER/REPO#main:AAuthLib/embedded-auth"
```

On install, `prepare` runs `tsc` so `dist/` is generated (TypeScript is a devDependency of this package).

## Usage

```ts
import { createEmbeddedAuth } from "@auth-service/embedded-auth";

const { iframe, load, initHandshake, destroy, updateTheme, logout } = createEmbeddedAuth({
  issuer: "https://auth.example.com",
  clientId: "myclient",
  allowedMessageOrigins: ["https://app.example.com", "https://auth.example.com"],
  onSuccess: ({ access_token, refresh_token }) => {
    // For confidential flows you may get refresh_token; iframe deployments often omit it.
    // For production BFF flows, exchange via POST /api/session-code then grant_type=embedded_session.
  },
  onError: (e) => console.warn(e),
  onReady: () => {
    initHandshake();
  },
});

document.getElementById("auth-slot")?.append(iframe);
load();
```

## Build (maintainers / local checkout)

```bash
cd AAuthLib/embedded-auth && npm ci && npm run build
```
