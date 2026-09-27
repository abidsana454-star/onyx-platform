# Onyx Platform

Next.js + TypeScript + Tailwind CSS project skeleton.

## Stack

- [Next.js](https://nextjs.org) (App Router, Turbopack)
- TypeScript
- Tailwind CSS v4
- ESLint (flat config, `eslint-config-next` + `eslint-config-prettier`)
- Prettier
- Husky + lint-staged (pre-commit formatting/linting)
- Prisma (ORM, PostgreSQL by default)

## Folder structure

```
app/         Next.js App Router routes, layouts, pages
components/  Shared, reusable React components
lib/         Client-safe utilities and shared logic (e.g. lib/prisma.ts)
server/      Server-only code: server actions, services, integrations
prisma/      Prisma schema and migrations
scripts/     One-off and maintenance scripts
docs/        Project documentation
```

## Getting started

```bash
npm install
cp .env.example .env   # fill in DATABASE_URL, etc.
npm run dev
```

Open http://localhost:3000.

## Scripts

| Script                 | Purpose                     |
| ---------------------- | --------------------------- |
| `npm run dev`          | Start the dev server        |
| `npm run build`        | Production build            |
| `npm run start`        | Start the production server |
| `npm run lint`         | ESLint check                |
| `npm run lint:fix`     | ESLint check with autofix   |
| `npm run format`       | Prettier write              |
| `npm run format:check` | Prettier check (used in CI) |
| `npm run typecheck`    | `tsc --noEmit`              |

## Pre-commit hooks

Husky runs `lint-staged` on every commit, which lints and formats staged files.
Hooks are installed automatically via the `prepare` script after `npm install`.

## Database (Prisma)

Schema lives in `prisma/schema.prisma`. Set `DATABASE_URL` in `.env`, then:

```bash
npx prisma generate
npx prisma migrate dev
```

## CI/CD

- `.github/workflows/ci.yml` — runs format check, lint, typecheck, and build on
  every push/PR to `main` and `develop`.
- `.github/workflows/deploy-staging.yml` — deploys `develop` to a Vercel
  staging environment. Requires the `VERCEL_TOKEN`, `VERCEL_ORG_ID`, and
  `VERCEL_PROJECT_ID` secrets to be set in the repo settings.

## Pushing this repo and deploying to staging

```bash
git add -A
git commit -m "chore: project skeleton"
git branch -M main
git remote add origin git@github.com:<your-org>/onyx-platform.git
git push -u origin main
git checkout -b develop
git push -u origin develop
```

Then, to enable the staging deploy workflow:

1. Create a project on [Vercel](https://vercel.com) linked to this repo.
2. In the GitHub repo settings, add secrets `VERCEL_TOKEN`, `VERCEL_ORG_ID`,
   `VERCEL_PROJECT_ID` (from `vercel link` / the Vercel dashboard).
3. Push to `develop` — the `Deploy Staging` workflow will build and deploy.
