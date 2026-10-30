# AI Testing Automation Agent
### Next.js foundation for a planned AI testing application

This repository currently provides a **SaaS application scaffold**: Next.js pages, Clerk authentication integration, a Neon/PostgreSQL data layer and Stripe subscription endpoints. AI test generation and automated test execution are **planned**, and are not implemented in the current source tree.

## Current implementation

| Area | Present in the repository |
| --- | --- |
| UI | Starter landing page and reusable button/card components |
| Authentication | Clerk provider, sign-in/sign-up pages and middleware integration |
| Data | Drizzle schema with users, initial credits and posts |
| User API | Endpoint to look up or insert a user record |
| Billing | Stripe Checkout session endpoint |
| Webhooks | Stripe signature verification and payment-event logging |

The webhook currently logs successful checkout; it does not fulfill subscriptions in the database. Checkout redirects to `/dashboard`, but a dashboard page is not currently present.

## Stack

Next.js 16 · React 19 · TypeScript · Tailwind CSS 4 · Clerk · Drizzle ORM · Neon PostgreSQL · Stripe

Dependency versions and commands are defined in [package.json](package.json).

## Run locally

Prerequisites: Node.js compatible with Next.js 16, npm, a development PostgreSQL database, and Clerk/Stripe test accounts.

```bash
git clone https://github.com/Krish-rk/ai-testing-automation-agent.git
cd ai-testing-automation-agent
npm ci
cp .env.example .env.local
```

On Windows PowerShell, copy the environment file with:

```powershell
Copy-Item .env.example .env.local
```

Replace the example values in `.env.local`:

| Variable | Purpose |
| --- | --- |
| `NEXT_PUBLIC_APP_URL` | Local application URL, normally `http://localhost:3000` |
| `DATABASE_URL` | Your development PostgreSQL connection string |
| `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` | Clerk publishable key |
| `CLERK_SECRET_KEY` | Clerk server-side key |
| `STRIPE_SECRET_KEY` | Stripe test secret key |
| `STRIPE_WEBHOOK_SECRET` | Signing secret for your test webhook |
| `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` | Stripe publishable key |

Keep the sign-in and sign-up URL values from the example file. Do not commit your populated environment file.

Set `DATABASE_URL` in the shell or IDE environment used to run Drizzle. The current `drizzle.config.ts` reads `process.env.DATABASE_URL` directly and does not explicitly load `.env.local`.

Push the schema to a **development database only**:

```bash
npm run db:push
npm run dev
```

Open `http://localhost:3000`.

Next.js reads `.env.local` for application startup. Review schema changes before pushing them to a shared database.

## Source guide

| Path | Purpose |
| --- | --- |
| `app/page.tsx` | Starter landing page |
| `app/sign-in/`, `app/sign-up/` | Clerk authentication screens |
| `app/api/users/route.tsx` | User record lookup/creation |
| `app/api/checkout/stripe/route.ts` | Subscription Checkout session creation |
| `app/api/webhooks/stripe/route.ts` | Webhook signature verification and logging |
| `db/schema.ts` | Database tables and types |
| `proxy.ts` | Clerk middleware integration |

## Validation

```bash
npx tsc --noEmit
npm run build
```

These commands require appropriate dependencies and configuration; they have not been run for this documentation update. The repository currently has no dedicated automated test script.

## Roadmap

- [ ] Add a project dashboard and working checkout return pages.
- [ ] Add explicit authentication and authorization guards to protected API operations.
- [ ] Validate allowed Stripe prices and associate checkout sessions with users.
- [ ] Implement persistent, idempotent subscription fulfillment.
- [ ] Implement AI test-case generation.
- [ ] Add isolated test execution, run history and reports.
- [ ] Add automated tests and CI.

## Scaffold attribution

The current landing page credits [Tubeguruji](https://www.youtube.com/@tubeguruji) for the starter scaffold. That attribution remains in the source. Repository-specific extensions should be described separately from the scaffold.
