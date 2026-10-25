# Open WebUI — Static Frontend on GitHub Pages

Static build of the **Open WebUI 0.12.4** frontend (SvelteKit + `@sveltejs/adapter-static`, `ssr = false`),
published on GitHub Pages.

- **Site:** https://pctesterssss.github.io/
- **Branch:** `main` (this repository, public)
- **Artifact:** ~181 MB of static files (HTML/JS/CSS/WASM)

> GitHub Pages serves **files only**. There is no backend here (FastAPI, SQLite, Ollama, auth, models).
> Without a backend you will see the *“Backend Required / frontend only”* screen — that is expected.

## Configure a backend

The build carries two local patches, marked with the comment `STATIC-DEPLOY PATCH` (they are **not** part of upstream):

1. `src/lib/constants.ts` — `WEBUI_BASE_URL` is read from the runtime `window.__OWUI_API_BASE__`
   instead of an empty string.
2. `src/app.html` — an inline bootstrap script (runs before the bundle) plus a floating button
   `#owui-api-fab` with panel `#owui-api-panel`.

**Using the panel:** click the floating button (bottom-right), type the address of your own backend
(e.g. `http://localhost:8080`), then:

- **Test** — sends `fetch(<url>/api/config)` and shows whether the backend answers;
- **Save & reload** — stores the address in `localStorage['owui_api_base']` and reloads;
- **Reset** — clears the stored address.

**One-shot alternative:** open `https://pctesterssss.github.io/?api=http://localhost:8080`.
The `api` parameter is saved to `localStorage` and then removed from the URL.

## Limitations & troubleshooting

1. **Mixed content.** The page is served over HTTPS. `http://localhost:8080` / `http://127.0.0.1:8080`
   is allowed by Chrome and Firefox (treated as potentially trustworthy), but `http://<LAN-IP>:8080`
   is usually blocked as mixed content. Expose your backend over HTTPS (reverse proxy) or through an
   HTTPS tunnel (ngrok / cloudflared).
2. **CORS.** The backend uses `CORS_ALLOW_ORIGIN` (default `*`, which logs a warning). The frontend
   sends `credentials: 'include'`, so `*` may not be accepted — run the backend with
   `CORS_ALLOW_ORIGIN=https://pctesterssss.github.io` (multiple origins are separated by `;`).
3. **socket.io.** Traffic to `/ws/socket.io` goes to the same backend; if it is blocked, the fix is
   the same as for CORS.
4. **No backend, no logic.** This static demo shows the interface only — no auth, chat, model list
   or database works without your own backend.

## Rebuild from source

1. Use **Node 22.x** — `engines` in `package.json` requires `>=18.13 <=22`.
2. `git clone https://github.com/open-webui/open-webui.git && cd open-webui && npm ci`
3. `USE_SLIM=true npm run build` → output in `build/`
4. Copy `build/*` into this repository, make sure `.nojekyll` exists and `404.html` is a copy of
   the current `index.html` (SPA fallback routing).
5. Commit, push to `main`; enable **Settings → Pages → Deploy from a branch → `main` / `(root)`**.

## Security

- **Revoke any personal access token that was ever pasted into a chat, log or screenshot:**
  GitHub → *Settings → Developer settings → Personal access tokens* → **Delete**, then create a new one.
- Never commit tokens or secrets to this repository.
- This repository is public; everything pushed here is public.
- Only point the frontend at backends you control.

## License & branding

Open WebUI is licensed under [MIT](https://github.com/open-webui/open-webui/blob/main/LICENSE).

# Open WebUI Branding Assets

This directory contains Open WebUI branding assets and references, including favicons, logos, splash images, app icons, manifests, metadata, and search identifiers.

Open WebUI branding here includes the name, logo, and any visual, textual, or symbolic identifiers that present or preserve Open WebUI attribution.

Do not alter, remove, obscure, or replace Open WebUI branding except as permitted by `LICENSE`.

See also: https://docs.openwebui.com/license.
