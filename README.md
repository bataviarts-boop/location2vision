# Location2Vision

**REAL LOCATION → ARCHITECTURAL VISION**

A production-oriented Next.js web app that turns a real-world location (Google Maps / Street View link) plus property information into a grounded architectural strategy and production-ready image-generation prompts.

Every factual claim is labeled by evidence: **Verified**, **Inferred**, **Recommended**, or **Verify on Site** — the app never pretends to have performed a professional land, structural, or zoning survey.

## Product flow

Location → Property Profile → Architectural Style → Site References (photos) → Project Brief → Analyze → Results Dashboard → Refine → Compare → Export / Share

## Tech stack

- **Next.js** (App Router) + React + TypeScript
- **OpenAI API** (`gpt-5` via the Responses API) for the reasoning + vision engine
- Optional **Google Maps Geocoding API** for reverse geocoding coordinates parsed from the URL
- No database required — sharing works via a self-contained link (`/vision?data=...`); Supabase env vars are scaffolded for future persistence but not required

## Run locally

```bash
npm install
cp .env.example .env.local   # then fill in OPENAI_API_KEY
npm run dev
```

Open http://localhost:3000

## Environment variables

| Variable | Required | Purpose |
|---|---|---|
| `OPENAI_API_KEY` | Yes | Powers the analysis, refine, and compare engines. Server-side only. |
| `GOOGLE_MAPS_API_KEY` | No | Enables reverse geocoding of coordinates found in the pasted URL (city/region/country). Without it, those fields are labeled "Inferred" instead of "Verified". |
| `NEXT_PUBLIC_SUPABASE_URL` / `NEXT_PUBLIC_SUPABASE_ANON_KEY` / `SUPABASE_SERVICE_ROLE_KEY` | No | Reserved for a future persistence layer. Not used by the current build. |

Never expose `OPENAI_API_KEY` or `GOOGLE_MAPS_API_KEY` to client code — they are only read inside `app/api/*/route.ts` server routes.

## Deploying for free, publicly

See [`docs/DEPLOY.md`](docs/DEPLOY.md) for step-by-step instructions to publish this for free on Netlify or Vercel.

## Project structure

```
app/
  page.tsx              Main wizard: location, property, style, references, brief, results
  vision/page.tsx        Read-only shared-vision viewer (no backend needed)
  api/analyze/route.ts   Grounded analysis engine (URL parsing + geocoding + vision + LLM)
  api/refine/route.ts    Refines an existing vision from a text instruction
  api/compare/route.ts   Generates alternative visions in different styles, same site
  api/export/route.ts    Builds a downloadable Markdown summary
  api/visualize/route.ts Prompt hand-off endpoint (no image provider wired by default)
components/
  ResultsView.tsx        Shared results dashboard (used by both the wizard and share page)
lib/
  mapsUrlParser.ts        Parses Google Maps/Street View URLs without ever fabricating data
  geocode.ts              Optional server-side reverse geocoding
  openai.ts               OpenAI client
types/vision.ts           Shared TypeScript types + evidence model
docs/
  LOCATION2VISION_MASTER_SPEC.md   Full 40-section product spec this app implements
  DEPLOY.md                        Free public deployment guide
  SECURITY.md                      Security checklist before public launch
```

## Product principle

Location2Vision is an **architectural concept and visualization intelligence tool**. It is **not** a land survey, legal property verification, structural engineering, construction documentation, or zoning approval tool. Every result carries this disclaimer:

> Conceptual visualization only. Site measurements, zoning, structural conditions, utilities, property boundaries, and regulatory requirements must be professionally verified before design or construction decisions.
