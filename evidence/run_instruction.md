# Run Instructions — COMPANY WEB

**COMPANY WEB** is the standalone marketing website for COMPANY CLOUD, built with Next.js 15 (App Router), TypeScript, and Tailwind CSS. It is optimized for Vercel deployment and generates 30 fully static pages at build time.

---

## Prerequisites

| Tool    | Minimum Version | Install                      |
| ------- | --------------- | ---------------------------- |
| Node.js | 18.0.0+         | https://nodejs.org           |
| pnpm    | 9.12.3          | `npm install -g pnpm@9.12.3` |

> npm and yarn are not supported. This project uses pnpm workspaces and pnpm-lock.yaml.

---

## 1. Clone the Repository

```bash
git clone https://GITHUB-MIRROR.git
cd COMPANY-WEB
```

---

## 2. Install Dependencies

```bash
pnpm install
```

This installs all dependencies and runs the `husky` prepare hook for git hooks.

---

## 3. Configure Environment Variables

Copy the example environment file and fill in the required values:

```bash
cp .env.example .env.local
```

Open `.env.local` and set the following:

| Variable                                    | Description                                      | Required |
| ------------------------------------------- | ------------------------------------------------ | -------- |
| `NEXT_PUBLIC_VERCEL_PROJECT_PRODUCTION_URL` | Production domain (e.g. `COMPANY.com`)            | Yes      |
| `NEXT_PUBLIC_GA_MEASUREMENT_ID`             | Google Analytics 4 Measurement ID                | Yes      |
| `NEXT_PUBLIC_POSTHOG_KEY`                   | PostHog project API key                          | Yes      |
| `NEXT_PUBLIC_POSTHOG_HOST`                  | PostHog host URL                                 | Yes      |
| `RESEND_AUDIENCE_ID`                        | Resend email audience ID                         | Yes      |
| `RESEND_FROM`                               | Sender email address (e.g. `noreply@COMPANY.com`) | Yes      |
| `RESEND_TOKEN`                              | Resend API token                                 | Yes      |

> The app will build and run without these values, but analytics, contact form submission, and email features will not function.

---

## 4. Run in Development

```bash
pnpm dev
```

Opens the development server at [http://localhost:3001](http://localhost:3001) with hot reload enabled.

---

## 5. Build for Production

```bash
pnpm build
```

This runs two steps automatically:

1. `content-collections build` — compiles MDX content (blog posts, case studies, legal docs)
2. `next build` — generates the static site (30 pages)

Expected output:

```
✓ Compiled successfully
✓ Finished TypeScript
✓ Generating static pages (30/30)
```

---

## 6. Preview the Production Build

```bash
pnpm start
```

Serves the production build at [http://localhost:3001](http://localhost:3001).

---

## 7. Lint and Format

```bash
# Run ESLint
pnpm lint

# Auto-fix lint issues
pnpm lint:fix

# Format all files with Prettier
pnpm format

# Check formatting without writing
pnpm format:check
```

---

## Project Structure

```
COMPANY-WEB/
├── app/                    # Next.js App Router pages
│   ├── (home)/             # Home page
│   ├── blog/               # Blog listing + [slug] pages
│   ├── case-studies/       # Case study listing + [slug] pages
│   ├── services/           # Service pages (AI, cloud, custom dev)
│   ├── legal/              # Terms, privacy, acceptable use
│   └── contact/            # Contact form with Resend email
│
├── components/             # Site-level React components
├── ui/                     # Shared UI library (shadcn/ui primitives)
├── content/                # MDX content (blog, case studies, legal)
├── lib/                    # Utilities and constants
├── public/                 # Static assets (images, logos)
└── shared-types/           # Shared TypeScript type definitions
```

---

## Tech Stack

| Layer           | Technology                                    |
| --------------- | --------------------------------------------- |
| Framework       | Next.js 16.1.6 (App Router, React 19)         |
| Language        | TypeScript 5.6+                               |
| Styling         | Tailwind CSS 3.4 + shadcn/ui                  |
| Content         | Content Collections (MDX)                     |
| Animation       | Framer Motion                                 |
| Forms           | React Hook Form + Zod                         |
| Email           | Resend                                        |
| Analytics       | Vercel Analytics, PostHog, Google Analytics 4 |
| Error tracking  | Sentry                                        |
| Deployment      | Vercel                                        |
| Package manager | pnpm 9.12.3                                   |

---

## Deploy to Vercel

1. Import the repository in the [Vercel Dashboard](https://vercel.com/new)
2. Set the following build configuration:
   - **Root Directory**: `/`
   - **Build Command**: `pnpm build`
   - **Output Directory**: `.next`
   - **Install Command**: `pnpm install`
3. Add all environment variables from `.env.example`
4. Deploy — every push to `main` triggers a production deployment

---

## Common Issues

**Build fails with "Module not found"**

```bash
find . -type f \( -name "*.ts" -o -name "*.tsx" \) \
  -exec sed -i 's|@repo/ui|@ui|g' {} +
pnpm build
```

**Port 3001 already in use**

```bash
pkill -f "next dev"
# or
pnpm dev -- -p 3002
```

**Clear cache and rebuild from scratch**

```bash
rm -rf .next node_modules/.cache .content-collections
pnpm install
pnpm build
```

---

## Pages Generated at Build Time

| Route                            | Description                         |
| -------------------------------- | ----------------------------------- |
| `/`                              | Home — hero, features, testimonials |
| `/about`                         | About COMPANY CLOUD                  |
| `/pricing`                       | Pricing plans                       |
| `/contact`                       | Contact form                        |
| `/services/ai-solutions`         | AI/ML infrastructure                |
| `/services/cloud-infrastructure` | Cloud infrastructure                |
| `/services/custom-development`   | Custom development                  |
| `/blog`                          | Blog listing (10 posts)             |
| `/blog/[slug]`                   | Individual blog posts               |
| `/case-studies`                  | Case studies listing                |
| `/case-studies/[slug]`           | Individual case studies             |
| `/legal/terms`                   | Terms of service                    |
| `/legal/privacy`                 | Privacy policy                      |
| `/legal/acceptable-use`          | Acceptable use policy               |
| `/sitemap.xml`                   | XML sitemap                         |
| `/robots.txt`                    | Robots directives                   |

---

**Version**: 3.0.0
**Maintained by**: COMPANY CLOUD Team
**License**: Proprietary — COMPANY INC.
