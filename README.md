# Longevity Journey with Tiwi

GitHub-ready source package · version 14 · 27 September 2026

An English-language 3D browser exploration game and AI health companion. Explore a neighbourhood, meet six original adult characters, discover 365 longevity-themed activities and follow related educational YouTube videos.

Demo: https://tiwi-longevity.r-7r.chatgpt.site

## Included in this release

- Third-person 3D world, mobile joystick, touch camera controls and compact menus.
- Arden (food/hydration), Rowan (mental wellbeing), Elias (sleep), Leon (movement), Noel (body rhythms) and Sage (purpose/prevention).
- 365 searchable activities with saved completion, 290 positive and 75 fictional hazard cards, video links and read-aloud.
- Fictional score starting at 90 years: +1 or −1 game day per eligible event. Duplicate rewards are prevented; accidental hazards can be undone. Scores do not predict real lifespan.
- One-button activity completion, with no repeated checkbox.
- Personal character chat with saved context enabled and no repeated consent checkboxes. The main health companion sends without the “Ready to ask Tiwi?” popup; context remains adjustable in settings.
- Direct video links on all 18 friend advice notes, food pickups, virus encounters and chat replies. Replies use curated topic matching, with general wellbeing fallback.
- Editable text, supported voice dictation, camera input, account-scoped memory, report uploads, citations and care links.
- Editable Blender scenes, reproducible Python generation scripts, GLB exports and existing cinematic trailer.

## Requirements

Node.js 22.13 or newer, pnpm (version specified in package.json), and a modern WebGL-capable browser. Blender 3.6 is only needed to edit/rebuild the models.

## Quick start

Extract the ZIP and open a terminal in the `Tiwi-Longevity-Journey` folder.

```bash
corepack enable
pnpm install --frozen-lockfile
```

Copy `.env.example` to `.env` (Windows: `copy .env.example .env`; macOS/Linux: `cp .env.example .env`). To activate AI, set your own server-side key:

```dotenv
OPENAI_API_KEY=your_own_key_here
OPENAI_MODEL=gpt-4.1-mini
AI_ACCESS_EXPIRES_AT=
```

A blank expiry means no app-level deadline. An optional ISO UTC timestamp controls the app’s AI access window; it does not change your provider key’s expiry. The hosted demo’s secrets and settings are not included in this ZIP. Never commit `.env` or put a key in client code.

Build once to generate the local Worker configuration:

```bash
pnpm build
```

Apply these migrations once to a fresh local database:

```bash
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0000_closed_sally_floyd.sql
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0001_smart_steve_rogers.sql
```

Then start development:

```bash
pnpm dev
```

Open http://localhost:5173. Clean clones use the portable development profile with development-only mock identity. The game is at `/`; the health companion is at `/companion`. AI needs a valid key, model access and billing; without it, the app clearly shows Evidence guide mode. Camera and microphone require browser permission and a secure context (HTTPS or localhost).

## Demo flow

1. Enter the island, walk using keyboard or joystick, and meet a neighbour.
2. Read their advice and open its related YouTube video.
3. Select Make this personal, edit a question, then Send.
4. Open Journal, search an activity, view its video and complete it or choose Find in world.
5. Collect a fruit/vegetable or encounter a virus to see its information and video.
6. Open the health companion for voice/photo questions, records and memory settings.

Videos open on YouTube and do not automatically mark completion. Continuous five-second camera analysis still has a separate explicit start/permission step.

## GitHub upload

Create an empty GitHub repository, then run from the extracted project folder:

```bash
git init
git add .
git commit -m "Initial Tiwi source"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
git push -u origin main
```

Replace the repository URL. Use Git rather than the GitHub browser uploader for the complete asset folder. No repository was created or pushed as part of this download.

## Deployment

This is a dynamic Cloudflare Workers/Vinext project with D1 (`DB`) and R2 (`BUCKET`), not a static GitHub Pages site. In ChatGPT Sites, register/import it as your own project, associate your own project ID, set runtime secrets and publish using the Sites workflow. The original deployment ID was removed from this package; `.openai/hosting.json` retains only logical resource bindings.

For independent hosting, configure your own Worker, D1 database, R2 bucket, migrations, secrets and verified authentication. The production APIs trust Sites dispatcher identity headers; do not expose them directly to the Internet without an authentication boundary that validates sessions and overwrites spoofable headers. Development mock identity must not be used for public production.

## Project map

- `app/game-shell.tsx`, `game/engine.ts`: game UI and 3D world.
- `app/personal-chat.tsx`, `app/dashboard.tsx`: character chat and health companion.
- `app/api/`: authenticated chat, saved state, memory and reports.
- `game/activity-seed.json`, `activity-videos.json`: editable activity catalogue.
- `game/video-recommendations.ts`: curated video matching and friend advice mapping.
- `public/tiwi-365-activities.csv`: full activity/video list.
- `assets/blender/`: editable scenes and reproduction scripts.
- `public/models/`: runtime GLBs.
- `drizzle/`: database migrations.
- `THIRD_PARTY_ASSETS.md`: asset attribution and provenance.
- `docs/DEVELOPMENT_HISTORY.md`, `GAME_README.md`: earlier development notes; this README describes the current release.

## Checks

```bash
pnpm exec tsc --noEmit
node tests/game-logic.mjs
node tests/video-recommendations.mjs
pnpm build
node tests/worker-smoke.mjs
node tests/personal-ai.mjs
```

The source release passed TypeScript, build, game/video coverage tests, 25 integration checks and mocked AI transport/history tests. These do not prove clinical accuracy, real-provider response quality or phone-browser compatibility. No new real-device visual test was performed.

## Boundaries and data

This is a hackathon prototype, not a validated medical device, diagnostic service or lifespan calculator. No foundation model was trained. No comprehensive EHR, pharmacy, wearable or live food-delivery integration is included. Individual needs vary; exercise, sleep and water cards are not universal prescriptions. Medical disclosures never deduct points.

Personal chats send selected context to the AI provider when AI is active. Review the processing notice and privacy controls before using real medical records. API keys, private records, local databases, installed dependencies, Git history and build output are excluded. Review THIRD_PARTY_ASSETS.md before redistribution; this package does not invent a new licence for third-party material.
