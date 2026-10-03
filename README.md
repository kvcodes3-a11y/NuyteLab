# Nuytelab

**Research, in the open.** Nuytelab is a hackathon-ready collaborative paper workspace: collect sources, build an AI-assisted synthesis, keep a version trail, and share it with the community to read and review.

## Prototype capabilities

- Create shared paper workspaces and add attributed contributions from multiple members.
- Upload searchable PDFs, text, or Markdown; PDF text extraction happens in the browser. Scanned PDFs need OCR, and the original PDF is not stored.
- Generate a source-grounded synthesis, edit it, save tracked revisions, and publish an approved edition to the Nuytelab commons.
- Read papers with contributor and citation trails, ask source-grounded questions, search/filter the community, and submit 1–5-star ratings and reviews.
- Publish each approved paper version as a Notion database row with **Name**, **Date**, **Summary**, and **Review**; **Summary** holds the full synthesis in a text field immediately beside **Date**, while **Review** is a concise synthesis and the row page retains source citations.

### Notion connection setup

Direct publishing uses a Notion internal integration token and a database URL/ID. The connected database schema is **Name** (title), **Date** (date), **Summary** (rich text), and **Review** (rich text); the table view places **Summary** immediately after **Date**. Nuytelab writes the full synthesis into **Summary**, a concise synthesis into **Review**, and retains the citations on the row page. Protected credentials are configured through the project-secret card; never paste secrets into chat or source files. The protected owner Manus email bootstraps the sole workspace admin. In Notion, add the integration to the database using **Add connections**. Fresh deployments should configure their own credentials before publishing. Visitors also need permission to view the Notion database; publishing is admin-only.

Sample papers and activity are fictional demo content. Replace those sources before treating a paper as research evidence. Any signed-in Manus account can contribute to the shared lab; no invitations are required. Public visitors can read and review published papers, while draft/source reads and editing require sign-in. The protected owner email remains the sole admin for direct Notion publishing. Public reviews/Q&A and team AI synthesis use per-process limits, with anonymous limits keyed from Express's connection IP rather than caller-supplied forwarding headers. A shared proxy IP may make visitors share a bucket; move limits to trusted edge identity/shared storage before horizontal scaling. Full review moderation is not included in this hackathon MVP. API bodies are capped because PDFs are parsed locally and only bounded text is submitted.

## Development

- `pnpm dev` — start the Express/tRPC server and client dev server (default port 3000; honors `PORT`).
- `pnpm build` and `pnpm start` — production build and serve `dist/index.js` plus `dist/public/`.
- `pnpm check` — TypeScript check.
- `pnpm test` — existing Vitest suite.
- `pnpm db:migrate` — apply checked-in migrations.
- `pnpm db:push` — generate/apply a database migration after schema changes.

Research records use the project's managed MySQL database and server-side platform AI. Private platform credentials stay on the server. The route manifest is `client/public/manus-routes.json`.
