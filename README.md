# Tars‑AI Chat Application

This repository contains a **Next.js 16** application that powers a conversational AI assistant named *TARS*. The project integrates several services:

- **Convex** for real‑time database and presence (hosted at `convex.cloud`).
- **Groq AI** (`@ai-sdk/groq`) to drive the chat model with primary/fallback key logic.
- **Clerk** for authentication and user management.
- **Tailwind CSS** + `lucide-react` for styling and icons.

The code lives in the `app/` directory (Next.js App Router) and includes a streaming API route (`app/api/chat/route.ts`) that handles message forwarding, key rotation, and quota failures.

---

## Prerequisites

- Node.js **18+** (LTS) or newer
- npm, yarn, or pnpm

Clone the repo and install dependencies:

```bash
git clone <repo-url>
cd Tars-AI
npm install        
```

### Environment variables

Create a `.env.local` file in the project root (it’s ignored by git) and populate the following values:

```env
# Convex (public URL used by the client) – required
NEXT_PUBLIC_CONVEX_URL="https://<your-convex>.convex.cloud"
NEXT_PUBLIC_CONVEX_SITE_URL="https://<your-convex>.convex.site"
CONVEX_DEPLOYMENT="<env>:<name>"             # e.g. "prod:successful-tern-162"

# Groq API keys – used server‑side
GROQ_API_KEY="gsk_..."
GROQ_FALLBACK_API_KEY="gsk_..."

# Clerk authentication
CLERK_JWT_ISSUER_DOMAIN="https://<your-clerk>.clerk.accounts.dev"
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_test_..."
CLERK_SECRET_KEY="sk_test_..."
```

> ❗ Do **not** commit this file. Keep your secrets secure.

The application logs the active key and performs a 24‑hour cooldown when the primary key hits its token limit.

---

## Development

Start the development server with hot reload:

```bash
npm run dev
```

Visit http://localhost:3000 in your browser. The console will show env loading and any AI‑related logs.

If you change `.env.local`, restart the server (Next.js reloads on start).

## Building & Deployment

```bash
npm run build
npm run start   
```

You can deploy to Vercel, Netlify, or any Node‑capable host. Ensure the same environment variables are set in your deployment environment.

---

## Project structure highlights

- `app/page.tsx` – main UI including chat interface and sidebar.
- `app/providers.tsx` – wraps the app with Convex and theme providers.
- `app/api/chat/route.ts` – serverless API route that streams responses using `ai` and Groq SDK.
- `convex/` – definitions for database schema, auth config, and server logic.
- `components/` – UI components (ChatWindow, Sidebar, ThemeProvider).

### AI key logic

The route maintains an `activeKey` (`PRIMARY` or `FALLBACK`) and performs a lightweight preflight request to determine if the key has quota. Upon exhaustion it switches keys and enforces a 24‑hour cooldown.
If both keys are exhausted the system falls back to `llama-3.1-8b-instant` to keep the bot alive.

---

## Further reading

For more information on each dependency, see their respective docs:

- [Next.js App Router](https://nextjs.org/docs/app)
- [Convex](https://convex.dev/docs)
- [Groq AI SDK](https://docs.groq.com)
- [Clerk for Next.js](https://clerk.dev/docs/nextjs)
- [Tailwind CSS](https://tailwindcss.com/docs)

Happy coding!
