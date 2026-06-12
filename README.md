# sustain-a-prompt

paste an llm prompt and see what it costs the planet. the app estimates the prompt's energy, co2, and water footprint, then proposes a leaner version with projected savings and shows a model answer.

live at [sustain-a-prompt.vercel.app](https://sustain-a-prompt.vercel.app). won mlh's best use of auth0 award at pennapps xxvi.

## how it works

you type a prompt into the console. the backend estimates its footprint in energy (wh), co2 (g), and water (l). gemini rewrites the prompt into a tighter version and the ui shows the projected savings next to the original. cerebras runs the chat and the answer gets passed through a regex sanitizer that strips disclaimers, code fences, and citation noise before display. login is optional, handled by better-auth with auth0 as the social provider.

## stack

next.js 15 app router, typescript, tailwind v4, shadcn/ui. gemini for prompt optimization, cerebras for chat, better-auth + auth0 for login, turso + drizzle for the auth db.

## local setup

```bash
npm install
npm run dev
```

create a `.env` in the repo root:

```
GOOGLE_GEMINI_API_KEY=...
CEREBRAS_API_KEY=...
TURSO_CONNECTION_URL=...
TURSO_AUTH_TOKEN=...
AUTH0_CLIENT_ID=...
AUTH0_CLIENT_SECRET=...
AUTH0_ISSUER_BASE_URL=...
```

gemini and cerebras keys are enough for the core flow. the turso and auth0 vars are only needed if you want login to work.

## api routes

- `POST /api/gemini/optimize` returns the optimized prompt and savings estimates
- `POST /api/cerebras/chat` chats with the selected model
- `GET /api/cerebras/models` lists available models

## notes

- do not use styled-jsx. it breaks builds on next 15 here. tailwind only.
- pages under `src/app` are server components by default. interactive logic lives in `HomeClient.tsx` behind a `"use client"` boundary.
- the sanitizer falls back to the original response if it strips too much.

mit license. built at pennapps xxvi.
