# Nuytelab implementation plan

## Product and architecture

Nuytelab is a GitHub-inspired home for collaborative research: signed-in members contribute source material, generate and revise a synthesis, publish a readable paper, and let the community discover, discuss, rate, and review it. The app uses the initialized React + TypeScript client, Express/tRPC server, Drizzle ORM, and managed MySQL database. Intake accepts pasted text plus PDF, text, and Markdown files; PDF text is extracted in the browser, with scanned PDFs requiring OCR. Public visitors can browse released papers, ask questions, and post rate-limited reviews; any signed-in Manus account can access and edit shared drafts and raw sources, with no invitation or allowlist step. A protected project secret matches the owner's verified Manus email to the sole admin role for Notion publishing. Contributions and revisions use the verified session identity rather than typed authorship.

The data model separates papers, source contributions, paper revisions, community reviews, and Notion edition records. tRPC exposes a public projection for community listing, released-paper reading and visitor Q&A; protected procedures require Manus sign-in for source intake, draft/source reads, synthesis, editorial revisions and release, but do not apply an email allowlist. The sole admin role is reserved for direct Notion publishing. Public responses omit raw source text; private source fetches are attributed to the authenticated session. Synthesis and Q&A call the platform-provided LLM server-side; platform credentials never reach the browser. The browser provides a responsive public library, shared signed-in lab, readable detail view, version history, editor controls and paper-specific Q&A.

Direct Notion publishing targets the connected single-source Notion database and verifies its exact `Name` (title), `Date` (date), `Summary` (rich text), and `Review` (rich text) property types. Each approved version creates a row with the paper title, its Nuytelab release date, the full synthesis in `Summary`, and a concise synthesis in `Review`. The connected table view orders `Summary` directly after `Date`; the row page also contains the abstract, contributors, and citation trail. The integration token and database ID/URL are protected project secrets, consumed only by the server. Until configured, the interface offers a Notion-ready Markdown export and never claims a row was synced.

## Project structure

- `client/src/pages/Home.tsx` — main Nuytelab workspace, discovery, submission and paper-reading/chat interactions.
- `client/src/index.css` — warm editorial design tokens, responsive layout and interaction details.
- `client/src/App.tsx` — existing app shell and root route.
- `server/routers.ts` / `server/research.ts` — tRPC app router, signed-in research procedures, public projections, and rate limiting; `server/notion.ts` — server-only Notion target/data-source resolution, table-schema validation, row creation and content-block formatting.
- `server/db.ts` — existing database access plus paper data operations.
- `drizzle/schema.ts` and `drizzle/` — paper, contribution, version, review and Notion-publication schema/migrations; any legacy membership table is left unused rather than dropped destructively.
- `client/public/manus-routes.json` — source-synchronized route manifest.

## Design: Editorial Marginalia

- **Design movement:** a warm, independent research journal crossed with an annotated working manuscript.
- **Core principles:** clarity before decoration; contribution lineage stays visible; editorial calm; a visible path from raw draft to reviewed release.
- **Color philosophy:** paper ivory (`#F4EDE1`) makes research feel readable and authored; ink (`#26251F`) carries long-form text; burnt sienna (`#9A3412`) is the signature action/annotation color; olive and slate are reserved for status and evidence metadata.
- **Layout paradigm:** an editorial two-rail workspace: stable left navigation, an asymmetric reading/list column, and a narrow right margin for activity and source notes. Paper detail expands into a manuscript-like reader rather than a centered dashboard grid.
- **Signature elements:** small marginal source labels; a connected version spine with numbered revision stamps; hand-drawn underline/annotation strokes beneath key section labels.
- **Interaction philosophy:** search and filters respond immediately; selecting a paper opens the reading context without losing the directory; synthesis, review and publishing actions show explicit state and preserve the submitted text on errors.
- **Animation:** restrained 140–220 ms opacity/translate transitions for drawers and cards; no looping motion; a brief progress indication while AI synthesis runs; honor reduced-motion preferences.
- **Typography:** Source Serif 4 for paper titles and long-form reading, with a neutral system sans-serif for navigation, controls and dense metadata. Use generous line length and readable 1.65 body leading.
- **Brand essence:** “The open notebook where teams turn sources into shared research.” Personality: thoughtful, collaborative, evidence-led.
- **Brand voice:** specific, curious, calm. Example lines: “Good research has a paper trail.” “Bring the sources. Leave with a sharper question.”
- **Wordmark & logo:** a custom lowercase `n` monogram with a small sienna margin-mark, paired with a tracked uppercase NUYTELAB wordmark.
- **Signature brand color:** burnt sienna `#9A3412`.
