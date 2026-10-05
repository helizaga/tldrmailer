# TLDRMailer

**Personalized AI newsletters: give it a topic and a reason, and it finds fresh articles, summarizes them with GPT, and emails the result.**

> This repository was published under the working name **AI-Newsletter**; it is the original **TLDRMailer** codebase. The public repo name simply differed from the product name (the repo was renamed in 2026 to match, and GitHub redirects the old URL).

## What it does and why

Keeping up with a niche topic means sifting through a lot of search results. TLDRMailer does that sifting for you. A user describes a topic and why they care about it. TLDRMailer then runs a retrieval-augmented generation (RAG) pipeline: it searches the web, ranks what it finds against the user's intent, summarizes the best new articles, and writes a newsletter that links back to its sources. The newsletter is then emailed to the user's mailing list.

TLDRMailer was a personalized AI newsletter product I built as founding engineer (Feb 2023 - Jan 2025). It reached **200+ users**; an early cohort achieved approximately **80% first-month retention**.

## My role

Founding engineer. I designed and built the product end to end: the TypeScript/Node.js content-delivery pipeline, REST APIs, email delivery and administration flows, and the React dashboard.

## Architecture

```text
React dashboard (Vite, Material UI, Auth0 login)
        │  REST over axios → http://localhost:3001/api
        ▼
Express API (TypeScript) ──── Prisma ────► PostgreSQL
        │                                  admins + mailing lists, newsletters,
        │                                  content history, used articles,
        │                                  per-URL article summaries
        ▼
RAG pipeline  (server/src/dataProcessing.ts)
  1. GPT turns the topic + reason into a web search query
  2. Bing Web Search returns candidate articles
  3. Puppeteer loads each page; the text is extracted and cleaned
  4. GPT scores each article's relevance (0-1); the top 4 not used before are kept
  5. GPT summarizes each kept article
  6. GPT writes the Markdown newsletter, citing the article URLs
        │
        ▼
AWS SES ───► each subscriber, with a personal unsubscribe link
```

Other behavior worth knowing:

- **No repeats.** Every article used in a newsletter is recorded per user, topic, and reason, so a regenerated or later newsletter only uses new sources.
- **Regenerate with history.** Newsletters can be regenerated from the dashboard; earlier versions are kept in `ContentHistory`.
- **Cost tracking.** Token usage is counted with `tiktoken` to estimate the OpenAI cost of each generation.
- **Mock mode.** `USE_MOCKS=true` replaces the search and GPT calls with stubs for offline development.

## Stack

TypeScript · Node.js · Express · React · Vite · Material UI · Auth0 · Prisma · PostgreSQL · OpenAI GPT (via LangChain) · Bing Web Search API · Puppeteer · AWS SES

## Project layout

```text
client/                 React dashboard (Vite)
  src/components/       Newsletter form, list, detail and mailing-list UI
  src/config/           Auth0 SPA settings (domain and client ID)
server/
  src/index.ts          Express entry point and unsubscribe route
  src/routes/           /api/admin and /api/newsletters routes
  src/controllers/      Request handlers, including SES sending
  src/dataProcessing.ts RAG pipeline: search, scrape, rank, summarize, compose
  src/services/         Bing and GPT clients (with mocks)
  db/prisma/            Prisma schema and migrations
```

## Run locally

Requirements: Node.js 18+, PostgreSQL, an OpenAI API key, an AWS SES sender, and an Auth0 single-page application.

> Microsoft retired the Bing Search APIs in August 2025, so live article search no longer works. Set `USE_MOCKS=true` to run the full flow with stubbed search and GPT responses.

1. Install dependencies (Puppeteer downloads a Chromium build on first install):

   ```bash
   npm install
   npm --prefix client install
   ```

2. Create `.env` in the repository root:

   ```bash
   DATABASE_URL=postgresql://USER:PASSWORD@localhost:5432/tldrmailer
   GPT_API_KEY=your_openai_api_key
   BING_API_KEY=your_bing_search_key
   AWS_REGION=us-west-2
   AWS_ACCESS_KEY_ID=your_aws_access_key_id
   AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key
   USE_MOCKS=true
   # PORT=3001
   ```

3. Apply the database migrations:

   ```bash
   npx prisma migrate deploy
   ```

4. Point the dashboard at your own Auth0 tenant by editing `client/src/config/auth0-config.json`, and add `http://localhost:3000` to the application's allowed callback, logout, and web origin URLs.

5. Start the API (compiles TypeScript, then listens on port 3001) and the dashboard (Vite on port 3000) in two terminals:

   ```bash
   npm start
   npm --prefix client start
   ```

6. Open <http://localhost:3000> and log in. Your account and a mailing list containing your own address are created on first login.

Note: the SES sender address in `server/src/controllers/newsletterController.ts` is hard-coded; change it to an address verified in your SES account before sending.

## Timeline and status

Active development in this repository ran **May 2023 - July 2024** (70 commits). The product operated from **February 2023 to January 2025** and has since been wound down; this repository is preserved as the historical codebase and is not maintained.
