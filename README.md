# TLDRMailer

> This repository was published under the working name **AI-Newsletter**; it is the original **TLDRMailer** codebase. The public repo name simply differed from the product name (the repo was renamed in 2026 to match — GitHub redirects the old URL).

TLDRMailer was a personalized AI newsletter product I built as founding engineer (Feb 2023 - Jan 2025). Given a topic, it discovered articles via the Bing Search API, summarized them with GPT, assembled a personalized newsletter with engagement metrics and article links, and delivered it by email. It reached **200+ users**; an early cohort achieved approximately **80% first-month retention**.

## My role

Founding engineer — designed and built the product end to end: the TypeScript/Node.js content-delivery pipeline, REST APIs, email delivery and administration flows, and the React dashboard.

## How it worked

- **Content pipeline** — Bing Search API for article discovery, GPT summarization over the retrieved articles, newsletter assembly with links, summaries, and engagement tracking
- **Dashboard** — React (Vite) + Material UI/Emotion, Auth0 authentication, newsletter creation and administration
- **Backend** — Node.js/TypeScript REST APIs; Prisma + PostgreSQL data models; scheduled email generation and delivery

## Stack

TypeScript · Node.js · React · Vite · Auth0 · Prisma · PostgreSQL · OpenAI GPT · Bing Search API · Material UI

## Timeline and status

Active development in this repository ran **May 2023 - July 2024** (70 commits). The product operated from **February 2023 to January 2025** and has since been wound down; this repository is preserved as the historical codebase and is not maintained.
