# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Development
```bash
npm run dev                 # Start development server at http://localhost:5173
npm run build               # Build for production (compiles TypeScript + Vite build)
npm run preview             # Preview production build locally
npm run lint                # Run ESLint on all TypeScript/TSX files
npm run check               # Full check: TypeScript compilation + build + dry-run deploy
```

### Cloudflare Workers
```bash
npm run deploy              # Deploy to Cloudflare Workers
npm run cf-typegen          # Generate Cloudflare Worker types (wrangler types)
wrangler tail               # Monitor worker logs in real-time
wrangler deploy --dry-run   # Test deployment without actually deploying
```

### Testing and Monitoring
```bash
npx wrangler tail           # Monitor deployed worker logs
```

## Architecture

This is a full-stack application combining React SPA with Cloudflare Workers backend:

### Project Structure
- `src/react-app/` - React frontend application
  - `App.tsx` - Main React component with API integration example
  - `main.tsx` - React app entry point
  - `assets/` - Static assets (logos, SVGs)
- `src/worker/` - Hono-based Cloudflare Worker backend
  - `index.ts` - Worker entry point with Hono app and API routes
- `dist/client/` - Built React assets served by the Worker (generated)
- `llm-docs/` - Documentation for various frameworks and services

### Tech Stack Integration
- **Frontend**: React 19 + Vite + TypeScript
- **Backend**: Hono framework running on Cloudflare Workers
- **Deployment**: Cloudflare Workers with static asset serving
- **Build**: Vite with Cloudflare plugin for Workers integration

### TypeScript Configuration
The project uses a multi-config TypeScript setup:
- `tsconfig.app.json` - React app configuration (includes `src/react-app/`)
- `tsconfig.worker.json` - Worker configuration (includes `src/worker/`)  
- `tsconfig.node.json` - Node.js tooling configuration
- `worker-configuration.d.ts` - Auto-generated Cloudflare Worker types

### Key Architecture Patterns
- **Single Page Application**: React app with client-side routing handled by Cloudflare Workers
- **API Integration**: Frontend calls `/api/*` endpoints served by the Hono worker
- **Asset Serving**: Static React build assets served directly from Workers with SPA fallback
- **Type Safety**: Full TypeScript coverage for both frontend and backend with Cloudflare bindings

### Cloudflare Workers Configuration
- **Main Entry**: `src/worker/index.ts` 
- **Compatibility**: Uses `nodejs_compat` flag and 2025-04-01 compatibility date
- **Assets**: Serves React build from `dist/client/` with SPA handling
- **Observability**: Enabled for monitoring and debugging
- **Source Maps**: Upload enabled for better debugging experience

### Development Workflow
1. `npm run dev` starts Vite dev server for React frontend
2. Worker API routes are accessible at `/api/*` during development  
3. `npm run build` compiles both React app and Worker
4. `npm run deploy` deploys the complete application to Cloudflare Workers

The application demonstrates full-stack integration where the React frontend consumes APIs provided by the Hono worker, all deployed as a single Cloudflare Worker with static asset serving.