# Proposal: repository structure, database schema and rules seed format

**Status:** Draft for review. Nothing here is built yet.
**Date:** 25 September 2026
**Based on:** `PRD.md` Sections 9, 11 and 17, and Appendix A.

Milestone numbers below follow PRD Section 17 (1 = Foundation, 2 = Pets and vault).

---

## 1. Repository structure

A single Next.js app. No monorepo; the rules schema is shared inside `src/lib/rules`.

```
pet-travel/
├── CLAUDE.md                     # project rules (missing from the repo today)
├── docs/                         # PRD, research, sources, this proposal
├── rules/                        # rules seed data (JSON, reviewed like code)
│   ├── sources.json              # shared source registry
│   ├── operators.json            # operators and display names
│   ├── rail/indian-railways.json
│   ├── air/air-india.json
│   ├── air/akasa.json
│   ├── air/fly91.json
│   ├── air/alliance-air.json
│   ├── air/spicejet.json
│   ├── air/not-carrying.json     # IndiGo, Air India Express, Star Air
│   ├── air/all.json              # cross-airline rules (sedation)
│   └── international/*.json      # aqcs, usa, uk, eu, uae, canada, australia, new-zealand, titre-labs
├── data/
│   └── breeds.json               # milestone 2
├── scripts/
│   ├── rules-validate.ts         # Zod check of every seed file (runs in CI)
│   └── rules-seed.ts             # loads seed into Postgres via publish_rule_version()
├── supabase/
│   ├── config.toml
│   └── migrations/
│       ├── 20260925000100_foundation.sql
│       └── 20260925000200_pets_vault.sql
├── src/
│   ├── app/
│   │   ├── (public)/             # home, later the rules library (server-rendered)
│   │   ├── (app)/                # signed-in area: pets, trips
│   │   ├── auth/                 # sign-in, OTP verify, callback
│   │   ├── manifest.ts           # PWA manifest
│   │   └── layout.tsx
│   ├── components/ui/            # buttons, fields, rule-source badge
│   ├── lib/
│   │   ├── supabase/             # server, browser and middleware clients
│   │   ├── rules/
│   │   │   ├── schema.ts         # Rule, Source, confidence (Zod)
│   │   │   ├── topics.ts         # value schema per topic (Zod)
│   │   │   └── queries.ts        # read rules_current
│   │   └── dates.ts              # IST helpers
│   ├── i18n/messages/en.json     # next-intl, English only in v1
│   └── middleware.ts             # session refresh
├── tests/
│   ├── unit/                     # Vitest
│   ├── db/                       # RLS and versioning tests against local Supabase
│   └── e2e/                      # Playwright at 360 px
└── .github/workflows/ci.yml      # lint, typecheck, unit, rules-validate, db tests
```

**Stack (from PRD Section 11):** Next.js 16 (App Router), TypeScript, Tailwind CSS 4, Supabase (Postgres, Auth, Storage), Zod 4, next-intl, Vitest, Playwright, pnpm.

---

## 2. Decisions I will make unless you object

1. **Rule IDs.** Keep the Appendix A IDs exactly as written (for example `air.air-india.cabin.max_weight_kg`). The Section 9.2 example (`max-weight`) is treated as illustrative.
2. **Operator slugs.** Full names in the `operator` field (`indian-railways`, `air-india`, `aqcs`). The `rail.ir.*` IDs stay as they are.
3. **Versioning.** Two tables, `rules` (stable identity) and `rule_versions` (immutable content), as in Section 11.1. The `supersedes` field from 9.2 is kept on each version.
4. **Sources.** One shared registry (`sources.json` and a `sources` table). Each rule version links to one or more sources with its own `retrievedAt` date and an optional locator (page or section).
5. **Every rule needs a source.** Enforced in the database at commit time, not only in the app.
6. **Weaker confidence.** Order used for "store the weaker one": official → secondary → traveller → conflicting.
7. **Event-based reviews.** "At each fare revision" becomes `reviewEveryDays: 180` plus `reviewTrigger: "fare_revision"`.
8. **[VERIFY] items.** Stored on each rule version as `verify: [{ field, note }]`. This makes them listable in admin and in each milestone report.
9. **Internal notes.** Stored in a separate staff-only table, so they never reach public pages.
10. **Seed versus admin edits.** Re-running the seed never overwrites a rule that was edited in admin. It reports the clash instead.
11. **Carrier dimensions.** Stored in inches, because airline limits are in inches. Centimetre input is converted, and the unit entered is kept for display.
12. **Weight used for airline checks.** `weight_with_carrier_kg` if entered; otherwise pet weight plus carrier weight if a carrier is selected; otherwise the check becomes a condition ("weigh your pet in the carrier"), not a blocker.
13. **Dates and times.** Calendar dates stored as `date`; moments as `timestamptz`; everything displayed in IST.

---

## 3. Rules seed file format

### 3.1 File shape

```jsonc
// rules/air/air-india.json
{
  "operator": "air-india",
  "mode": "air",
  "rules": [
    {
      "id": "air.air-india.cabin.max_weight_kg",
      "topic": "max_weight_kg",
      "appliesTo": { "species": ["dog", "cat"], "travelClass": ["cabin"] },
      "value": { "kg": 10, "includesCarrier": true },
      "statement": "In the cabin, your pet and its carrier together can weigh up to 10 kg.",
      "officialQuote": "Up to 10 kg – In the cabin with you (Economy)",
      "sources": [
        { "ref": "air-india-pets-page", "retrievedAt": "2026-09-25", "locator": "Weight section" }
      ],
      "confidence": "official",
      "verifiedAt": "2026-09-25",
      "verifiedBy": "desk-research-2026-09",
      "verificationMethod": "desk",
      "reviewEveryDays": 90,
      "verify": [],
      "notes": "Older newsroom article says 7 kg; superseded by the current page."
    }
  ]
}
```

```jsonc
// rules/sources.json
{
  "sources": [
    {
      "id": "air-india-pets-page",
      "url": "https://www.airindia.com/in/en/travel-information/travelling-with-pets.html",
      "title": "Air India: Travelling with pets",
      "publisher": "Air India",
      "type": "official",
      "sourceDate": null
    }
  ]
}
```

### 3.2 A rule with an open [VERIFY] item

```jsonc
{
  "id": "rail.ir.chart.first_chart",
  "topic": "chart_rule",
  "value": {
    "firstChartHoursBefore": 10,
    "morningWindow": ["05:00", "14:00"],
    "morningChartTime": "20:00",
    "morningChartDay": "previous",
    "relativeTo": "charting_station"
  },
  "statement": "The first chart is usually ready at least 10 hours before departure. For trains leaving between 5 am and 2 pm, it is ready by 8 pm the evening before. This is an estimate; check your PNR status.",
  "sources": [
    { "ref": "business-standard-chart-2025-12", "retrievedAt": "2026-09-25" },
    { "ref": "cr-release-chart-2025-07", "retrievedAt": "2026-09-25" }
  ],
  "confidence": "secondary",
  "verifiedAt": "2026-09-25",
  "verifiedBy": "desk-research-2026-09",
  "verificationMethod": "desk",
  "reviewEveryDays": 90,
  "verify": [{ "field": "value", "note": "Confirm against the Railway Board circular for the December 2025 change." }]
}
```

### 3.3 Topic value schemas (Zod, in `src/lib/rules/topics.ts`)

Each `topic` has one value schema. The loader rejects any record whose value does not match. Starting set, drawn from Appendix A:

| Topic | Value shape |
|---|---|
| `pets_accepted` | `{ species: ("dog"\|"cat")[], cabin: bool, hold: bool, cargo: bool, serviceAnimals?: string }` |
| `allowed_classes` | `{ classes: string[], requires?: "full_coupe_or_cabin_single_pnr" }` |
| `disallowed_classes` | `{ classes: string[] }` |
| `max_weight_kg` | `{ kg: number, includesCarrier: bool, petOnlyMaxKg?: number }` |
| `weight_range_kg` | `{ min: number, max: number, dependsOnAircraft?: bool, aboveMax?: "cargo" }` |
| `carrier_dimensions_in` | `{ l: number, w: number, h: number, type?: "soft"\|"hard"\|"any", wheelsAllowed?: bool }` |
| `pets_per_flight` / `pets_per_passenger` / `pets_per_pnr` | `{ cabin?: int, hold?: int, total?: int, includesServiceAnimals?: bool }` |
| `fee_inr` | `{ cabin?: int, hold?: int, per: "sector", taxesIncluded: bool }` |
| `fee_formula` | `{ kind: "excess_baggage_multiple", multiple: number }` |
| `money_range_inr` | `{ min: int, max: int }` |
| `min_age` | `{ cabin?: Duration, hold?: Duration }` where `Duration = { weeks } \| { months }` |
| `booking` | `{ channels: ("portal"\|"phone"\|"counter"\|"email"\|"support_ticket"\|"online")[], minHoursBefore?: int, phone?, email?, url? }` |
| `hours_before` | `{ hours: number }` or `{ minHours, maxHours }` |
| `certificate_validity` | `{ issuedWithinDays?, issuedWithinHours?, validForDays? }` |
| `blocked_with` | `{ situations: ("infant"\|"wheelchair"\|"unaccompanied_minor")[] }` |
| `breed_restriction` | `{ brachycephalic: { cabin: "allowed"\|"blocked", hold: "allowed"\|"blocked" } }` |
| `flag` | `{ value: bool }` (muzzle, non-stop only, sedation refused, ESA not accepted) |
| `route_bans` | `{ bans: { countries: string[], direction: "to"\|"from"\|"both", modes: string[] }[] }` |
| `chart_rule` | as in PRD 9.5 |
| `notional_weight_kg` | `{ withPassenger, dogBox, basket, seeingEyeDog }` |
| `destination_requirements` | structured steps with day offsets (used by milestone 8) |
| `text` | `{ text: string }`, only for rules the engine never evaluates |

Appendix A rows that bundle several facts (for example `air.alliance.cabin`) keep their ID and get one structured object. I will share the full seed files for review at the end of milestone 1.

### 3.4 Validation in CI (`pnpm rules:validate`)

- Every record matches its topic schema.
- IDs are unique and match `^(rail|air|intl)\.`.
- Every `sources[].ref` exists in `sources.json`.
- Every record has at least one source.
- `confidence: "official"` requires at least one source of type `official`.
- `verifiedAt` is not in the future.

---

## 4. Database schema

Two migrations, one per milestone. Later tables (trips, tasks, trains, stations, offices, templates, reports) come with their own milestones.

### 4.1 Migration 1: Foundation (`20260925000100_foundation.sql`)

```sql
-- ============================================================
-- Foundation: profiles, staff roles, operators, sources, rules
-- ============================================================

create type public.rule_mode as enum ('rail', 'air', 'international');
create type public.source_type as enum ('official', 'secondary', 'traveller');
create type public.confidence as enum ('official', 'secondary', 'traveller', 'conflicting');
create type public.staff_role as enum ('admin', 'editor');
create type public.verification_method as enum ('desk', 'phone', 'in_person', 'traveller_report');
create type public.content_origin as enum ('seed', 'admin');

-- ---------- helpers ----------

create function public.set_updated_at() returns trigger
language plpgsql as $$
begin
  new.updated_at := now();
  return new;
end $$;

-- ---------- users ----------

create table public.profiles (
  id uuid primary key references auth.users (id) on delete cascade,
  display_name text check (char_length(display_name) <= 80),
  locale text not null default 'en' check (locale in ('en', 'hi')),
  reminders_consent_at timestamptz,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

create trigger profiles_updated_at before update on public.profiles
  for each row execute function public.set_updated_at();

create function public.handle_new_user() returns trigger
language plpgsql security definer set search_path = '' as $$
begin
  insert into public.profiles (id) values (new.id) on conflict do nothing;
  return new;
end $$;

create trigger on_auth_user_created after insert on auth.users
  for each row execute function public.handle_new_user();

create table public.staff_roles (
  user_id uuid primary key references auth.users (id) on delete cascade,
  role public.staff_role not null,
  granted_at timestamptz not null default now()
);

create function public.is_staff() returns boolean
language sql stable security definer set search_path = '' as $$
  select exists (select 1 from public.staff_roles where user_id = auth.uid());
$$;

-- ---------- reference content ----------

create table public.operators (
  slug text primary key check (slug ~ '^[a-z0-9-]+$'),
  mode public.rule_mode not null,
  name text not null,
  website_url text check (website_url ~ '^https://'),
  created_at timestamptz not null default now()
);

create table public.sources (
  id text primary key check (id ~ '^[a-z0-9-]+$'),
  url text not null check (url ~ '^https?://'),
  title text not null,
  publisher text,
  type public.source_type not null,
  source_date date,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

create trigger sources_updated_at before update on public.sources
  for each row execute function public.set_updated_at();

create table public.rules (
  id text primary key check (id ~ '^(rail|air|intl)\.[a-z0-9_.-]+$'),
  mode public.rule_mode not null,
  operator text not null references public.operators (slug),
  topic text not null,
  current_version_id uuid,
  retired_at timestamptz,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

create table public.rule_versions (
  id uuid primary key default gen_random_uuid(),
  rule_id text not null references public.rules (id),
  version int not null check (version > 0),
  applies_to jsonb not null default '{}',
  value jsonb not null,
  statement text not null check (char_length(statement) between 1 and 600),
  official_quote text,
  confidence public.confidence not null,
  verified_at date not null,
  verified_by text not null,
  verification_method public.verification_method not null,
  review_every_days int not null check (review_every_days between 1 and 730),
  review_trigger text,
  effective_from date,
  supersedes uuid references public.rule_versions (id),
  verify_items jsonb not null default '[]',
  change_summary text,
  origin public.content_origin not null,
  content_hash text not null,
  created_by uuid references auth.users (id) on delete set null,
  created_at timestamptz not null default now(),
  unique (rule_id, version),
  unique (id, rule_id)
);

-- current_version_id must point at a version of the same rule
alter table public.rules
  add constraint rules_current_version_fk
  foreign key (current_version_id, id) references public.rule_versions (id, rule_id)
  deferrable initially deferred;

create table public.rule_version_sources (
  rule_version_id uuid not null references public.rule_versions (id) on delete cascade,
  source_id text not null references public.sources (id),
  retrieved_at date not null,
  locator text,
  primary key (rule_version_id, source_id)
);

-- staff-only; never exposed on public pages
create table public.rule_version_notes (
  rule_version_id uuid primary key references public.rule_versions (id) on delete cascade,
  notes text not null
);

-- rule versions are immutable
create function public.forbid_change() returns trigger
language plpgsql as $$
begin
  raise exception '% rows are immutable', tg_table_name;
end $$;

create trigger rule_versions_immutable before update or delete on public.rule_versions
  for each row execute function public.forbid_change();

-- no rule version without at least one source (checked at commit)
create function public.require_rule_source() returns trigger
language plpgsql as $$
begin
  if not exists (select 1 from public.rule_version_sources where rule_version_id = new.id) then
    raise exception 'rule version % for % has no source', new.id, new.rule_id;
  end if;
  return null;
end $$;

create constraint trigger rule_versions_need_source
  after insert on public.rule_versions
  deferrable initially deferred
  for each row execute function public.require_rule_source();

-- ---------- read model used by the app ----------

create view public.rules_current with (security_invoker = true) as
select
  r.id,
  r.mode,
  r.operator,
  r.topic,
  v.id as version_id,
  v.version,
  v.applies_to,
  v.value,
  v.statement,
  v.official_quote,
  v.confidence,
  v.verified_at,
  v.review_every_days,
  v.review_trigger,
  v.effective_from,
  v.verify_items,
  v.verified_at + v.review_every_days as review_due_on,
  (v.verified_at + v.review_every_days) < (now() at time zone 'Asia/Kolkata')::date as is_overdue,
  coalesce(
    jsonb_agg(
      jsonb_build_object(
        'id', s.id, 'url', s.url, 'title', s.title, 'type', s.type,
        'sourceDate', s.source_date, 'retrievedAt', rvs.retrieved_at, 'locator', rvs.locator
      ) order by s.type, s.id
    ) filter (where s.id is not null),
    '[]'
  ) as sources
from public.rules r
join public.rule_versions v on v.id = r.current_version_id
left join public.rule_version_sources rvs on rvs.rule_version_id = v.id
left join public.sources s on s.id = rvs.source_id
where r.retired_at is null
group by r.id, v.id;

-- ---------- publishing (used by the seed loader and, later, admin) ----------

create function public.publish_rule_version(p_rule jsonb, p_origin public.content_origin default 'admin')
returns jsonb
language plpgsql security definer set search_path = '' as $$
declare
  v_rule_id text := p_rule ->> 'id';
  v_current public.rule_versions;
  v_new_id uuid;
  v_next int;
  v_src jsonb;
begin
  if not (public.is_staff() or coalesce(auth.jwt() ->> 'role', '') = 'service_role') then
    raise exception 'not allowed' using errcode = '42501';
  end if;

  insert into public.rules (id, mode, operator, topic)
  values (v_rule_id, (p_rule ->> 'mode')::public.rule_mode, p_rule ->> 'operator', p_rule ->> 'topic')
  on conflict (id) do nothing;

  select v.* into v_current
  from public.rules r
  left join public.rule_versions v on v.id = r.current_version_id
  where r.id = v_rule_id
  for update of r;

  if v_current.id is not null and v_current.content_hash = p_rule ->> 'contentHash' then
    return jsonb_build_object('status', 'unchanged', 'versionId', v_current.id);
  end if;

  if p_origin = 'seed' and v_current.origin = 'admin' then
    return jsonb_build_object('status', 'skipped_admin_edit', 'versionId', v_current.id);
  end if;

  v_next := coalesce(v_current.version, 0) + 1;

  insert into public.rule_versions (
    rule_id, version, applies_to, value, statement, official_quote, confidence,
    verified_at, verified_by, verification_method, review_every_days, review_trigger,
    effective_from, supersedes, verify_items, change_summary, origin, content_hash, created_by
  ) values (
    v_rule_id, v_next,
    coalesce(p_rule -> 'appliesTo', '{}'),
    p_rule -> 'value',
    p_rule ->> 'statement',
    p_rule ->> 'officialQuote',
    (p_rule ->> 'confidence')::public.confidence,
    (p_rule ->> 'verifiedAt')::date,
    p_rule ->> 'verifiedBy',
    (p_rule ->> 'verificationMethod')::public.verification_method,
    (p_rule ->> 'reviewEveryDays')::int,
    p_rule ->> 'reviewTrigger',
    (p_rule ->> 'effectiveFrom')::date,
    v_current.id,
    coalesce(p_rule -> 'verify', '[]'),
    p_rule ->> 'changeSummary',
    p_origin,
    p_rule ->> 'contentHash',
    auth.uid()
  ) returning id into v_new_id;

  for v_src in select * from jsonb_array_elements(coalesce(p_rule -> 'sources', '[]')) loop
    insert into public.rule_version_sources (rule_version_id, source_id, retrieved_at, locator)
    values (v_new_id, v_src ->> 'ref', (v_src ->> 'retrievedAt')::date, v_src ->> 'locator');
  end loop;

  if nullif(p_rule ->> 'notes', '') is not null then
    insert into public.rule_version_notes (rule_version_id, notes) values (v_new_id, p_rule ->> 'notes');
  end if;

  update public.rules set current_version_id = v_new_id, updated_at = now() where id = v_rule_id;

  return jsonb_build_object('status', 'created', 'versionId', v_new_id, 'version', v_next);
end $$;

revoke execute on function public.publish_rule_version(jsonb, public.content_origin) from public, anon;
grant execute on function public.publish_rule_version(jsonb, public.content_origin) to authenticated, service_role;

-- ---------- row-level security ----------

alter table public.profiles enable row level security;
alter table public.staff_roles enable row level security;
alter table public.operators enable row level security;
alter table public.sources enable row level security;
alter table public.rules enable row level security;
alter table public.rule_versions enable row level security;
alter table public.rule_version_sources enable row level security;
alter table public.rule_version_notes enable row level security;

create policy "own profile" on public.profiles
  for select to authenticated using (id = (select auth.uid()));
create policy "update own profile" on public.profiles
  for update to authenticated using (id = (select auth.uid())) with check (id = (select auth.uid()));

create policy "see own role; staff see all" on public.staff_roles
  for select to authenticated using (user_id = (select auth.uid()) or public.is_staff());

-- public content: anyone can read; only staff can write (writes go through publish_rule_version)
create policy "public read" on public.operators for select to anon, authenticated using (true);
create policy "public read" on public.sources for select to anon, authenticated using (true);
create policy "public read" on public.rules for select to anon, authenticated using (true);
create policy "public read" on public.rule_versions for select to anon, authenticated using (true);
create policy "public read" on public.rule_version_sources for select to anon, authenticated using (true);

create policy "staff write" on public.operators for all to authenticated
  using (public.is_staff()) with check (public.is_staff());
create policy "staff write" on public.sources for all to authenticated
  using (public.is_staff()) with check (public.is_staff());

create policy "staff only" on public.rule_version_notes for select to authenticated
  using (public.is_staff());
```

### 4.2 Migration 2: Pets and vault (`20260925000200_pets_vault.sql`)

```sql
create type public.species as enum ('dog', 'cat');
create type public.pet_sex as enum ('male', 'female', 'unknown');
create type public.carrier_type as enum ('soft', 'hard');
create type public.vaccine_type as enum ('rabies', 'dhppi', 'dhlpp', 'fvrcp', 'other');
create type public.document_type as enum (
  'vaccination_card', 'fitness_certificate', 'microchip_certificate', 'titre_result',
  'aqcs_certificate', 'noc', 'airline_indemnity_form', 'other'
);

-- breeds are reference data; the brachycephalic flag must have a source
create table public.breeds (
  id uuid primary key default gen_random_uuid(),
  species public.species not null,
  name text not null,
  aliases text[] not null default '{}',
  brachycephalic boolean not null default false,
  brachycephalic_source_id text references public.sources (id),
  unique (species, name),
  check (not brachycephalic or brachycephalic_source_id is not null)
);

create table public.pets (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references auth.users (id) on delete cascade,
  name text not null check (char_length(name) between 1 and 60),
  species public.species not null,
  breed_id uuid references public.breeds (id),
  breed_text text check (char_length(breed_text) <= 80),
  is_mixed boolean not null default false,
  flat_face boolean,                 -- owner-reported; used when the breed is mixed or unknown
  date_of_birth date not null,
  dob_is_estimate boolean not null default false,
  weight_kg numeric(5, 2) not null check (weight_kg > 0 and weight_kg <= 120),
  weight_with_carrier_kg numeric(5, 2)
    check (weight_with_carrier_kg is null or weight_with_carrier_kg >= weight_kg),
  weighed_on date,
  microchip_no text check (microchip_no ~ '^[0-9A-Za-z]{9,15}$'),
  microchip_implanted_on date,
  sex public.pet_sex,
  photo_path text,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

create function public.check_breed_species() returns trigger
language plpgsql as $$
begin
  if new.breed_id is not null and not exists (
    select 1 from public.breeds where id = new.breed_id and species = new.species
  ) then
    raise exception 'breed does not match species';
  end if;
  return new;
end $$;

create trigger pets_breed_species before insert or update on public.pets
  for each row execute function public.check_breed_species();

create table public.documents (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references auth.users (id) on delete cascade,
  pet_id uuid references public.pets (id) on delete cascade,
  type public.document_type not null,
  title text check (char_length(title) <= 120),
  storage_path text not null unique,
  mime_type text not null
    check (mime_type in ('application/pdf', 'image/jpeg', 'image/png', 'image/webp', 'image/heic')),
  size_bytes int not null check (size_bytes > 0 and size_bytes <= 10485760),
  issued_on date,
  expires_on date check (expires_on is null or issued_on is null or expires_on >= issued_on),
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  check (storage_path like user_id::text || '/%')
);

create table public.vaccinations (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references auth.users (id) on delete cascade,
  pet_id uuid not null references public.pets (id) on delete cascade,
  vaccine public.vaccine_type not null,
  vaccine_other text check (vaccine <> 'other' or vaccine_other is not null),
  given_on date not null,
  valid_until date check (valid_until is null or valid_until > given_on),
  vet_name text check (char_length(vet_name) <= 120),
  document_id uuid references public.documents (id) on delete set null,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

create table public.carriers (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references auth.users (id) on delete cascade,
  name text check (char_length(name) <= 60),
  type public.carrier_type not null,
  length_in numeric(5, 2) not null check (length_in > 0 and length_in <= 60),
  width_in numeric(5, 2) not null check (width_in > 0 and width_in <= 60),
  height_in numeric(5, 2) not null check (height_in > 0 and height_in <= 60),
  weight_kg numeric(5, 2) check (weight_kg >= 0 and weight_kg <= 40),
  has_wheels boolean not null default false,
  entered_unit text not null default 'in' check (entered_unit in ('in', 'cm')),
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

create trigger pets_updated_at before update on public.pets for each row execute function public.set_updated_at();
create trigger documents_updated_at before update on public.documents for each row execute function public.set_updated_at();
create trigger vaccinations_updated_at before update on public.vaccinations for each row execute function public.set_updated_at();
create trigger carriers_updated_at before update on public.carriers for each row execute function public.set_updated_at();

create index on public.pets (user_id);
create index on public.documents (user_id, expires_on);
create index on public.vaccinations (pet_id, valid_until);
create index on public.carriers (user_id);

-- ---------- row-level security ----------

create function public.owns_pet(p_pet uuid) returns boolean
language sql stable security definer set search_path = '' as $$
  select exists (select 1 from public.pets where id = p_pet and user_id = auth.uid());
$$;

alter table public.breeds enable row level security;
alter table public.pets enable row level security;
alter table public.documents enable row level security;
alter table public.vaccinations enable row level security;
alter table public.carriers enable row level security;

create policy "public read" on public.breeds for select to anon, authenticated using (true);
create policy "staff write" on public.breeds for all to authenticated
  using (public.is_staff()) with check (public.is_staff());

create policy "own pets" on public.pets for all to authenticated
  using (user_id = (select auth.uid()))
  with check (user_id = (select auth.uid()));

create policy "own documents" on public.documents for all to authenticated
  using (user_id = (select auth.uid()))
  with check (user_id = (select auth.uid()) and (pet_id is null or public.owns_pet(pet_id)));

create policy "own vaccinations" on public.vaccinations for all to authenticated
  using (user_id = (select auth.uid()))
  with check (user_id = (select auth.uid()) and public.owns_pet(pet_id));

create policy "own carriers" on public.carriers for all to authenticated
  using (user_id = (select auth.uid()))
  with check (user_id = (select auth.uid()));

-- ---------- private storage ----------
-- Files live at pet-files/<user_id>/<uuid>.<ext>. Downloads use short-lived signed URLs.

insert into storage.buckets (id, name, public, file_size_limit, allowed_mime_types)
values ('pet-files', 'pet-files', false, 10485760,
        array['application/pdf', 'image/jpeg', 'image/png', 'image/webp', 'image/heic']);

create policy "read own files" on storage.objects for select to authenticated
  using (bucket_id = 'pet-files' and (storage.foldername(name))[1] = (select auth.uid())::text);

create policy "upload own files (full accounts only)" on storage.objects for insert to authenticated
  with check (
    bucket_id = 'pet-files'
    and (storage.foldername(name))[1] = (select auth.uid())::text
    and coalesce(((select auth.jwt()) ->> 'is_anonymous')::boolean, false) = false
  );

create policy "delete own files" on storage.objects for delete to authenticated
  using (bucket_id = 'pet-files' and (storage.foldername(name))[1] = (select auth.uid())::text);
```

### 4.3 Later tables (not in these migrations)

- Milestone 3: `trips`, `trip_pets`, `trip_options`.
- Milestone 5: `stations`, `trains`, `train_stops`, `submission_offices`, `parcel_offices`, `trip_legs`.
- Milestone 7: `tasks`, `reminders`, `push_subscriptions`.
- Milestone 9: `templates`, `template_versions`, `generated_documents`.
- Milestone 11: `trip_reports`, moderation fields.

---

## 5. Tests planned for milestone 1

- **Unit (Vitest):** topic schemas, rule schema, weaker-confidence helper, IST date helpers.
- **Seed validation:** every file in `rules/` passes `rules:validate`.
- **Database (local Supabase in Docker):**
  - A rule version without a source is rejected.
  - Rule versions cannot be updated or deleted.
  - Re-seeding unchanged data creates no new versions.
  - A changed seed record creates version 2 and keeps version 1.
  - A seed run never overwrites an admin-edited rule.
  - Anonymous visitors can read `rules_current` but not internal notes.
  - A signed-in non-staff user cannot call `publish_rule_version`.
- **E2E (Playwright, 360 × 800):** home page renders; email OTP sign-in works against local Supabase.
