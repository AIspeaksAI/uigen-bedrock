# UIGen

AI-powered React component generator with live preview.

> This is a sample application used in the [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action) training by Anthropic.

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

   b. Fill in your gateway values in `.env` (use the same values as your Claude Code config):

   ```
   ANTHROPIC_AUTH_TOKEN=<your bearer token>
   ANTHROPIC_BASE_URL=https://your-gateway.example.com
   ANTHROPIC_BEDROCK_BASE_URL=https://your-gateway.example.com/bedrock
   ```

   - `ANTHROPIC_AUTH_TOKEN` is the bearer token used to authenticate with the gateway. If it is missing or left as the placeholder, the app uses the mock provider.
   - `ANTHROPIC_BASE_URL` is the gateway **root** (no path). The app appends `/v1` automatically and calls the Anthropic Messages API at `/v1/messages`.
   - `ANTHROPIC_BEDROCK_BASE_URL` is kept for reference; the current provider talks to the Anthropic-compatible `/v1` endpoint (which the gateway serves via Bedrock).
   - The default model id is `claude-haiku-4-5-20251001`. Set `ANTHROPIC_MODEL` to override it; it must match an id returned by `GET /v1/models` on your gateway.

   c. **Corporate CA / TLS:** if your gateway uses a corporate certificate, Node must trust it. Set `NODE_EXTRA_CA_CERTS` (in your shell, **not** `.env` — it must exist before Node starts) to your CA bundle when running the app:

   ```bash
   export NODE_EXTRA_CA_CERTS=/path/to/corporate-ca-bundle.pem
   ```

   You can verify connectivity directly before starting the app:

   ```bash
   curl --cacert "$NODE_EXTRA_CA_CERTS" \
     -X POST "$ANTHROPIC_BASE_URL/v1/messages" \
     -H "Authorization: Bearer $ANTHROPIC_AUTH_TOKEN" \
     -H "anthropic-version: 2023-06-01" \
     -H "content-type: application/json" \
     -d '{"model":"claude-haiku-4-5-20251001","max_tokens":16,"messages":[{"role":"user","content":"ping"}]}'
   ```

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

If your gateway uses a corporate CA, start the server so Node trusts it:

```bash
NODE_EXTRA_CA_CERTS=/path/to/corporate-ca-bundle.pem npm run dev
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
