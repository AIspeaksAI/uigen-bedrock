# UIGen

AI-powered React component generator with live preview.

## Prerequisites

- Node.js 18+
- npm

## Setup

1. **Optional** Configure Claude through **Amazon Bedrock** to generate real components.

   The project talks to Claude through an Amazon Bedrock-backed, Anthropic-compatible gateway (the same `ANTHROPIC_*` convention Claude Code uses). It runs without any credentials — it falls back to a mock provider that returns canned components instead of calling Claude. To enable real generation:

   a. Copy the example env file:

   ```bash
   cp .env.example .env
   ```

   b. Fill in your gateway values in `.env`:

   ```
   ANTHROPIC_AUTH_TOKEN=<your bearer token>
   ANTHROPIC_BASE_URL=https://your-gateway.example.com/v1
   ANTHROPIC_BEDROCK_BASE_URL=https://your-bedrock-gateway.example.com
   ```

   - `ANTHROPIC_AUTH_TOKEN` is the bearer token used to authenticate with the gateway. If it is missing or left as the placeholder, the app uses the mock provider.
   - `ANTHROPIC_BASE_URL` must point at the Anthropic Messages API root, typically ending in `/v1`. If only `ANTHROPIC_BEDROCK_BASE_URL` is set, it is used as a fallback automatically.
   - Optionally set `ANTHROPIC_MODEL` to override the default model name (`claude-haiku-4-5`) if your gateway exposes it under a different id.

2. Install dependencies and initialize the database:

```bash
npm run setup
```

> **Don't run `npm audit fix`.** Dependencies are pinned to specific versions that work together. The vulnerability warnings are cosmetic for a local-only project, and `audit fix` can bump packages past compatible versions and break the app.

This command will:

- Install all dependencies
- Generate Prisma client
- Run database migrations

## Running the Application

### Development

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Usage

1. Sign up or continue as anonymous user
2. Describe the React component you want to create in the chat
3. View generated components in real-time preview
4. Switch to Code view to see and edit the generated files
5. Continue iterating with the AI to refine your components

## Features

- AI-powered component generation using Claude
- Live preview with hot reload
- Virtual file system (no files written to disk)
- Syntax highlighting and code editor
- Component persistence for registered users
- Export generated code

## Tech Stack

- Next.js 15 with App Router
- React 19
- TypeScript
- Tailwind CSS v4
- Prisma with SQLite
- Anthropic Claude AI (via Amazon Bedrock)
- Vercel AI SDK
