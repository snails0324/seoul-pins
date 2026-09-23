# Seoul Pins

Seoul Pins is a mobile-first map for saving and organizing places in Korea. It is designed for people who collect restaurant, cafe, shopping, and travel recommendations from Naver, Kakao, and friends, then want to find them again without scrolling through chat history.

## What it does

- Save a place with its name, address, type, category, and map share link.
- Resolve supported Naver Place links into coordinates for map display.
- Fall back to address geocoding when a share link cannot be resolved.
- Browse saved places on a Leaflet map or in a categorized list.
- Create and delete personal categories.
- Keep the app usable offline with the local data layer and PWA setup.
- Deploy the frontend and serverless resolver through Vercel.

## Tech stack

- React 19 + Vite
- React Leaflet + Leaflet
- Supabase for persistent place and category data
- `idb-keyval` for local/offline storage support
- Vercel serverless function for share-link resolution
- Vite PWA plugin

## Project structure

```text
.
├── api/
│   └── resolve.ts              # Serverless URL resolver for supported map links
├── public/                     # PWA icons and static assets
├── src/
│   ├── db/                     # Supabase and place/category data access
│   ├── utils/                  # Geocoding and shared utilities
│   ├── App.jsx                 # Main screens and place workflow
│   ├── MapView.jsx             # Map rendering and markers
│   ├── CategoryManager.jsx     # Category management UI
│   └── Offline.jsx             # Offline experience
├── .env.example                # Required environment variable names
├── vercel.json                 # SPA fallback and deployment routing
└── vite.config.js              # Vite and PWA configuration
```

## Run locally

Requirements: Node.js 20+ and npm.

```bash
npm ci
cp .env.example .env
npm run dev
```

On Windows PowerShell, use this instead of `cp`:

```powershell
Copy-Item .env.example .env
```

Then fill in the Supabase values in `.env`:

```text
VITE_SUPABASE_URL=...
VITE_SUPABASE_ANON_KEY=...
```

## Available scripts

```bash
npm run dev       # Start the Vite development server
npm run build     # Create a production build
npm run preview   # Preview the production build locally
npm run lint      # Run ESLint
```

## Deployment

The project is structured for Vercel deployment:

1. Import the repository into Vercel.
2. Add `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` to the project environment variables.
3. Use the default Vite build settings: `npm run build` with `dist` as the output directory.
4. Deploy. `vercel.json` keeps client-side routes working through the SPA fallback.

## Security and data notes

- Credentials and local environment files are intentionally excluded from the repository.
- The public Supabase anon key is used only through environment variables; database access should still be protected with Supabase Row Level Security policies.
- The Naver resolver returns minimal coordinate data in normal operation. Detailed diagnostic output is only enabled with the explicit `debug=1` query parameter.
- A share link or address may fail to resolve when the provider changes its page structure or blocks automated requests; the app can still save the place without coordinates.

## Portfolio focus

This project demonstrates a practical full-stack workflow: a responsive React interface, map-based data visualization, persistent storage, offline-friendly behavior, and a small serverless integration that normalizes third-party place links.
