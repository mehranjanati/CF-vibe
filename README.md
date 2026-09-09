# CF-vibe — Cloudflare side of V2 vibe (dual-plane)

Execution plane + SPA of the V2 vibe platform:

- **React SPA** (Vite + Tailwind v4 + TanStack Query) — chat, client-side preview (Sandpack / static srcdoc), file editor, workflow visualizer.
- **Lightweight Worker** (`worker/light-index.ts`, configured in `wrangler.v2.jsonc`): serves the SPA (ASSETS), D1-backed auth (`users`, `linked_identities`), GitHub OAuth + export, capabilities/status routes. Intentionally dependency-light.

Pairs with the Go control plane: [mehranjanati/go-server-](https://github.com/mehranjanati/go-server-) (sessions, VFS, LLM streaming, Pages deploys). Point the SPA at it with `VITE_CONTROL_PLANE_URL`.

## Commands

```bash
bun install
bun run dev            # http://localhost:5173
bun run typecheck      # tsc -b
bun run lint
bun run build          # SPA + Worker bundle (dist/)
bun run deploy         # wrangler deploy via .prod.vars (never committed)
```

## Environment

- Local: `cp .dev.vars.example .dev.vars` and fill values.
- Prod: `.prod.vars` (secrets — never committed).
- D1 auth tables come from `migrations/`.

## Notes

- Derived from Cloudflare VibeSDK (MIT) — see `LICENSE`. Upstream-only artifacts (SDK package, CI workflows, debug tools, changelog) were removed; the `test:integration` script that referenced the removed `sdk/` is inactive.
