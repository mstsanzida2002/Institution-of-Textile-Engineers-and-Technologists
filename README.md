# Institution of Textile Engineers and Technologists

The official ITET Bangladesh website and member-services platform for the Institution of Textile Engineers and Technologists. The application combines public information pages with authenticated member, client, and administrator portals.

## Overview

This project is a full-stack Next.js application with:

- Public ITET information pages, including history, mission and vision, constitution, committee members, former leaders, welfare trust, notices, publications, events, jobs, and gallery content.
- A membership login and role-based dashboards.
- Member profiles, approval status, event registrations, member directory data, and a member discussion forum.
- An administrator dashboard for membership approvals, users, content, and platform management.
- A client/advertiser portal for job opportunities, advertisements, and marketplace price information.
- A local SQLite database managed with Prisma ORM.
- Responsive layouts styled with Tailwind CSS and reusable React components.

## Technology Stack

- Next.js 16 App Router
- React 19
- TypeScript
- Prisma 6
- SQLite
- Tailwind CSS 4
- bcryptjs for password hashing
- Lucide React for icons
- Node.js 22+ recommended for local development

## Project Structure

```text
app/
  actions.ts                    Server actions for authentication and mutations
  components/                   Shared header, footer, hero, and content components
  lib/session.ts                Cookie-based session helpers
  utils/db.server.ts            Prisma client setup
  admin-dashboard/              Administrator dashboard
  client-portal/                Client and advertiser portal
  member-dashboard/             Authenticated member dashboard and forum
  membership-login/             Login page
  events/                       Events listing
  notices/                      Notices listing
  publication/                  Publications listing
prisma/
  schema.prisma                 Database schema
  seed.ts                       Development/demo data seed
public/assets/                  Images, logos, and static assets
```

## Requirements

Install the following before starting:

- Node.js 22 or newer
- npm 10 or newer
- Git

Node.js 24 is also supported by the included Dockerfile.

## Local Development

### 1. Clone the repository

```bash
git clone https://github.com/mstsanzida2002/Institution-of-Textile-Engineers-and-Technologists.git
cd Institution-of-Textile-Engineers-and-Technologists
```

### 2. Install dependencies

```bash
npm ci
```

### 3. Configure the database

Create a `.env` file in the project root:

```env
DATABASE_URL="file:./dev.db"
```

The `.env` file and local database are intentionally ignored by Git. Never commit production credentials or database files.

### 4. Generate the Prisma client

```bash
npx prisma generate
```

### 5. Create or update the local database

For a fresh local database, apply the schema:

```bash
npx prisma db push
```

To load the development dataset:

```bash
npx prisma db seed
```

The seed script clears and recreates local application data. It creates demo users for the `ADMIN`, `MEMBER`, and `CLIENT` roles, as well as sample events, forum content, job opportunities, advertisements, notices, publications, and marketplace data. Use the credentials defined in `prisma/seed.ts` only for local development, and replace them before any deployment.

### 6. Start the development server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Available Scripts

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start the Next.js development server with hot reload |
| `npm run build` | Create an optimized production build |
| `npm run start` | Start the production server after building |
| `npx prisma generate` | Generate the Prisma client |
| `npx prisma db push` | Synchronize the Prisma schema with the database |
| `npx prisma db seed` | Reset and populate the development database |
| `npx prisma studio` | Open Prisma Studio for local database inspection |

## Authentication and Roles

Users sign in at `/membership-login`. After successful authentication, the application redirects users according to their role:

| Role | Main destination | Purpose |
| --- | --- | --- |
| `ADMIN` | `/admin-dashboard` | Manage members, approvals, content, and platform data |
| `MEMBER` | `/member-dashboard` | View member information, directory data, events, and forum features |
| `CLIENT` | `/client-portal` | Manage client-facing job and advertising features |

The `/advertiser-portal` route is an alias for the client portal. Protected pages use the session cookie and database role checks before rendering.

Passwords are stored as bcrypt hashes. Before production use:

1. Replace all seed/demo credentials.
2. Use strong, unique passwords.
3. Configure production environment variables through the hosting provider.
4. Do not expose `.env`, `prisma/dev.db`, or password hashes.
5. Review and harden session handling for the production deployment.

## Main Routes

### Public pages

- `/`
- `/history-itet`
- `/mission-vision`
- `/constitution`
- `/committee`
- `/former-leaders`
- `/welfareTrust`
- `/membership-overview`
- `/members-benifits`
- `/events`
- `/job-circular`
- `/notices`
- `/publication`
- `/gallery`
- `/contact`

### Authenticated pages

- `/membership-login`
- `/member-dashboard`
- `/member-dashboard/forum`
- `/member-portal`
- `/admin-dashboard`
- `/client-portal`
- `/advertiser-portal`

## Database

The Prisma schema is in `prisma/schema.prisma`. The principal models include:

- `User`
- `MemberProfile`
- `Event`
- `EventRegistration`
- `QaPost`
- `QaReply`
- `QaLike`
- `JobOpportunity`
- `MarketplacePrice`
- `AdBanner`
- `Notice`
- `Publication`

For local development, SQLite is used through `DATABASE_URL="file:./dev.db"`. For production, configure a managed database and update the Prisma datasource and deployment process as required.

## Production Build

Validate a production build locally:

```bash
npm run build
npm run start
```

Then open [http://localhost:3000](http://localhost:3000) and verify the public and authenticated routes with production configuration.

## Docker

Build and run the included image:

```bash
docker build -t itet-website .
docker run --env-file .env -p 3000:3000 itet-website
```

Before deploying the Docker image, configure the production database and environment variables in the hosting platform. Do not bake secrets or a development SQLite database into the image.

## Deployment Checklist

- Set `NODE_ENV=production`.
- Configure a production `DATABASE_URL`.
- Run Prisma client generation as part of the build.
- Apply the production schema/migrations through the deployment process.
- Use strong production admin credentials.
- Confirm HTTPS is enabled.
- Verify protected routes and role restrictions.
- Confirm static assets load correctly.
- Review dependency audit results before release.
- Configure backups and monitoring for the production database.

## Contributing

1. Create a feature branch.
2. Make focused changes consistent with the existing component and route structure.
3. Run `npm run build`.
4. Test affected public and authenticated routes locally.
5. Submit a pull request describing the change.

Do not commit:

- `.env` files
- Local database files
- `node_modules`
- `.next` build output
- Passwords, API keys, or other secrets

## License and Ownership

This repository contains the ITET Bangladesh website and related application source code. Review the project owner’s requirements before redistributing or reusing the content and assets.
