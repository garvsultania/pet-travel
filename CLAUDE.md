# Pet Travel PWA — project guide for Claude

## What this is

A mobile-first Progressive Web App that helps Indian pet parents plan a trip with a dog or cat: Indian Railways, domestic flights, and international moves (export from India and import into India).

## Source of truth

- `docs/PRD.md` — product requirements. Follow it for product decisions and build order (Section 17).
- `docs/RESEARCH.md` — market research and the official rules fact base. Follow it for facts.
- `docs/research-notes/SOURCES.md` — raw notes with a URL and date for every claim. Check here before adding or changing any rule.

If the PRD and the research disagree, the PRD wins for product decisions and the research wins for facts.

## Non-negotiable rules

1. **Rules are data, not code.** Every travel rule shown to users comes from the rules records (PRD Section 9 and Appendix A). Never hard-code a limit, fee, time window or policy in UI copy or logic.
2. **Every rule carries a source and a date.** Show the source link, source type and "last checked" date wherever a rule appears.
3. **Label confidence honestly.** Official rule, secondary, traveller-reported, or conflicting. Never present a traveller tip as a rule. Never show coupe or slot chances as percentages in v1.
4. **Items marked [VERIFY] stay configurable.** Store them as data with `confidence: "conflicting"` or `"secondary"`, surface them as conditions with a warning, and list any you touch in the milestone summary.
5. **No fabricated documents.** Letters are drafts the user sends. The vet certificate is a format with a mandatory watermark for the vet to complete and sign. Never produce anything that looks issued by a vet, railway, airline or government body.
6. **Lawful, honest guidance only.** No advice to use fictitious passenger names, misstate pet details, or make informal payments. Point users to official complaint channels.
7. **No medical dosing.** Sedation and health guidance is general and tells users to consult their vet.

## Stack (recommended in PRD Section 11)

Next.js (App Router) with TypeScript, Tailwind CSS, Supabase (Postgres, Auth with phone OTP and email, Storage with row-level security), Zod, `@react-pdf/renderer` and `docx` for documents, service worker for offline and web push, next-intl for i18n.

## How to work

- Build one milestone at a time, in the order in PRD Section 17.
- Before starting a milestone, write a short plan. After finishing it, run the tests and summarise what was built, what is left, and any [VERIFY] items touched.
- Write unit tests for the rules engine that cover every acceptance criterion in PRD M2, M4 and M5.
- Mobile first: design for a 360 px wide screen, then scale up.
- Copy style: short, plain sentences. No marketing language.
- Keep secrets in `.env.local`; commit a `.env.example` only.
