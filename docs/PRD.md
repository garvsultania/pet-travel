# Product Requirements Document: Pet Travel Planner for India

**Working name:** TBD
**Version:** 1.0 (draft for build)
**Date:** 25 September 2026
**Companion document:** `RESEARCH.md` (evidence for every decision below). Raw source notes: `research-notes/SOURCES.md`.

---

## How to use this document (for the build agent)

- Build in the order given in **Section 17 (Build plan)**.
- Every rule shown to users must come from the rules data described in **Section 9**. Never hard-code a rule in UI copy.
- Treat items tagged **[VERIFY]** as open. Build them as data fields that can be changed later, not as fixed logic.
- Appendix A holds seed rules data. Appendix B holds decision tables. Appendix C holds document templates.
- When this document and `RESEARCH.md` disagree, this document wins for product decisions, and `RESEARCH.md` wins for facts.

---

## 1. Problem

Pet parents in India who need to travel with a dog or cat face three problems:

1. **Uncertainty.** On trains, they learn only at charting, hours before departure, whether a coupe was allotted and whether the pet can travel. On flights, cabin slots are limited to one or two pets per flight.
2. **Conflicting information.** Railway zones, parcel offices, TTEs, airline call centres, blogs and viral reels give different answers. Much of it is out of date.
3. **Scattered, informal process.** The steps, offices, documents and timings are spread across government PDFs, airline pages and Reddit threads. Nobody plans for what happens when something fails.

The result is anxiety, wasted money, pets in unsafe conditions, and people who stop travelling with their pets.

## 2. Goal

Give a pet parent one place to plan a specific trip from start to finish:

- Which travel options are actually possible for this pet on this route.
- What to do, in what order, and by when.
- Which documents to carry, with drafts of anything they need to write.
- What to do if something goes wrong.
- Every rule shown with its source and the date it was last checked.

### Non-goals for v1

- Booking train or flight tickets inside the app. We link out to IRCTC, airlines and the IRCTC pet booking page.
- Handling payments to railways, airlines or government.
- Road trips, pet taxis and pet-friendly stays. These can come later.
- Species other than dogs and cats (Alliance Air accepts birds; out of scope for now).
- Acting as a relocation agency.

## 3. Users

Full personas are in `RESEARCH.md`, Section 3. The product must serve all six:

| Persona | v1 priority | Why |
|---|---|---|
| Festival homecomer (rail or air, domestic) | High | Largest volume; October–December peak |
| Relocator with a medium or large dog | High | No cabin option; rail and hold are the only paths |
| Cat parent | High | Underserved; kitten-basket and weight edge cases |
| Solo traveller | High | Two-berth problem; safety concerns |
| Emigrant (export from India) | Medium | High stakes, long timeline, lower volume |
| Returning NRI or visitor (import into India) | Medium | Confusing paperwork, clear rules |

## 4. Product principles

1. **Every rule has a source and a date.** Show "Source: [name], checked [date]" beside every rule. Show a warning when a rule is past its review date.
2. **Separate official rules from traveller experience.** Label content as *Official rule*, *Traveller-reported*, or *Unverified*. Never present a community tip as a rule.
3. **Plan for the failure path.** Every plan includes what to do if the key step fails (no coupe, no cabin slot, counter refusal).
4. **Be specific to this trip.** The pet's species, weight, breed, age and number, and the traveller's situation decide what is shown.
5. **Honest and lawful advice only.** No advice to use fictitious names, pay bribes, or misstate a pet's details. Tell users their rights and how to use official complaint channels.
6. **Never fabricate official documents.** The app drafts letters the user sends and provides certificate formats a vet completes and signs. It never produces a document that looks issued by a vet, railway, airline or government.
7. **Works on a phone, on a train.** The trip pack must be readable offline.
8. **Plain language.** Short sentences. English first; Hindi and Hinglish next.

## 5. Platform decision

**Recommendation: a mobile-first Progressive Web App (PWA).**

Reasons from the research:

- People find this information through Instagram, WhatsApp and Google. A link that opens instantly converts better than an app-store install for a task done a few times a year.
- Rules pages need to rank in search. A web app with server-rendered public pages does this; a native app does not.
- A PWA can be installed to the home screen, work offline (trip pack on the train), and send web push notifications (Android, and iOS 16.4+ when installed).
- Native apps can follow in v2 if retention and reminder use justify it.

## 6. Scope by release

### v1 (MVP)

1. Pet profile and document vault (M1)
2. Trip wizard and options advisor (M2)
3. Rail playbook (M3)
4. Domestic flight playbook (M4)
5. International planner: export to USA, Canada, UK, EU, UAE; import into India; explain why Australia and New Zealand are not direct (M5)
6. Document generator (M6)
7. Trip timeline and reminders (M7)
8. Public rules library with sources and dates (M8)
9. Post-trip report (M9, basic)
10. Shareable checklists (M10)
11. Admin: rules editor and freshness dashboard (M11)

### v1.1

- Hindi and Hinglish content.
- WhatsApp reminders.
- Coupe-odds indicator informed by trip reports.
- More destinations: Singapore, Australia (via third country), New Zealand.
- Vande Bharat sleeper pet box, once an official source exists.

### v2

- Referral to vetted relocation agencies for complex international moves (paid leads).
- Partner vets for certificates.
- Paid concierge help.
- Road travel, pet taxis, pet-friendly stays.
- Native apps.

---

## 7. Core user flows

### Flow A: Plan a trip (all modes)

1. Land on the home page or a rules page. Tap "Plan a trip".
2. Add or select a pet (can skip sign-in until saving).
3. Enter the trip: from, to, date, one-way or return, number of travellers, any infant or wheelchair user, number of pets.
4. See **Options**: every mode ranked as *Possible*, *Possible with conditions*, or *Not possible*, each with reasons, estimated cost, time needed and a stress note.
5. Choose an option. Get the **Trip plan**: a dated timeline, a checklist, documents to prepare, and the fallback plan.
6. Save (sign in by phone OTP or email). Reminders are scheduled.
7. Download or share the **Trip pack** (PDF and an image checklist). Available offline.
8. After the trip, answer a short **Trip report**.

### Flow B: Check a rule quickly

1. Search or browse the rules library (for example "Air India pet weight limit").
2. See the rule, its source link, the date last checked, and any recent change.
3. Call to action: "Plan a trip with this".

### Flow C: Something went wrong (on the day)

1. From the trip plan, tap "Something went wrong".
2. Choose the situation (for example "No coupe allotted", "Online pet booking says not allowed", "TTE says I can't travel", "Airline says no slot").
3. Get specific next steps, the relevant official text to show staff, and what to do if they still refuse.

---

## 8. Functional requirements

Each module lists requirements and acceptance criteria (AC).

### M1. Pet profile and document vault

**Requirements**

- Pet fields: name, species (dog or cat), breed (searchable list with a "brachycephalic" flag), date of birth, weight (kg), weight with carrier (kg, optional), microchip number and implant date, sex, photo.
- Vaccinations: vaccine type (rabies, DHPPi/DHLPP, FVRCP, etc.), date given, valid until, vet name. Upload a photo or PDF of the vaccination card.
- Documents: store uploads with a type (vaccination card, fitness certificate, microchip certificate, titre result, AQCS certificate, NOC, airline indemnity form, other), issue date and expiry.
- Multiple pets per account.
- Carrier profile: dimensions (L×W×H in inches or cm) and type (soft or hard), used by the carrier fit check.

**AC**

- A user can create a pet in under two minutes with only name, species, weight and date of birth. Other fields are optional.
- Uploaded files are private to the account (row-level security) and can be deleted.
- Selecting a brachycephalic breed flags the pet in all airline hold checks.
- Expiring documents show on the dashboard 30, 7 and 1 days before expiry.

### M2. Trip wizard and options advisor

**Requirements**

- Inputs: origin city or station, destination city or station, travel date, return date (optional), travellers (count), infant travelling (yes or no), wheelchair assistance (yes or no), pets (select from profile), domestic or international (inferred from destination).
- The **rules engine** (Section 9) evaluates every mode and operator:
  - Rail: 1AC coupe or cabin; brake-van dog box; kitten or puppy in a basket (any class); Vande Bharat sleeper pet box (hidden until verified).
  - Air (domestic): Air India cabin, Air India hold, Akasa cabin, Akasa hold, Akasa cargo, Fly91 cabin, Alliance Air cabin, SpiceJet cargo.
  - International: airline-agnostic plan plus airline notes; destination rules; India export or import rules.
  - Always show "Not possible" operators with the reason (for example "IndiGo carries no pets").
- For each option show: status, blocking or conditional reasons, estimated cost range, lead time needed, "what you'll need", and a short stress or risk note.
- Sort by feasibility first, then by the user's chosen priority (lowest stress, lowest cost, fastest).

**AC**

- A 12 kg dog shows all domestic cabin options as *Not possible* with the weight reason, and shows Air India and Akasa hold as *Possible with conditions*.
- A 4 kg cat shows Air India, Akasa, Fly91 and Alliance Air cabin as *Possible with conditions* (slot availability, route served, and weight including carrier within each airline's limit), and shows rail 1AC as *Possible with conditions*. No cabin option is ever shown as unconditionally possible, because slots are capped per flight.
- A traveller with an infant sees Air India, Akasa and Fly91 cabin marked *Not possible* for the pet in the cabin.
- A brachycephalic breed sees Air India and Akasa hold marked *Not possible*.
- A trip to Australia or New Zealand shows "Not possible directly from India" with the third-country explanation.
- Every reason links to the rule record and its source.

### M3. Rail playbook

This is the most important module. It turns the informal coupe process into a clear plan.

**M3.1 Train selection help**

- User enters a train number or picks from trains between two stations (via the train data provider; see Section 11).
- For each train, show: whether it has 1AC; the originating station; whether the user's boarding station is the origin; number of 1AC coaches (if known); whether a brake-van dog box is likely (older ICF rake) or unknown; long halts on the route (5+ minutes) for toilet breaks.
- Label "boarding at origin" as a *Traveller-reported* advantage, with the explanation from research. Never state it as a rule.

**M3.2 Booking strategy**

- Explain the core rule: the whole coupe (2 berths) or cabin (4 berths) must be on one PNR.
- Guidance for:
  - Two travellers: book 2 berths on one PNR and select the coupe preference.
  - Three or four travellers: book 4 berths on one PNR.
  - Solo traveller: the risk that a single ticket will not get a coupe, and the options. The second berth must be booked for a real person. Explain that an unoccupied berth can be reallotted by the TTE. Do not suggest fictitious names or booking in the pet's name.
- Show the booking window (Advance Reservation Period) and a reminder to book when it opens. [VERIFY current ARP value; store as data.]
- Explain that confirmation is needed; waitlisted or RAC tickets cannot get a coupe.
- **Still waitlisted or RAC close to travel:** show the decision point (a user-set date, default 2 days before). Options: keep the ticket and prepare a fallback, book a confirmed 1AC ticket on another train or date, or switch mode. Show the cancellation cost for each choice.

**M3.3 Coupe request**

- Explain what the letter does and does not do: official text says requests go to the DRM or GM office and "due consideration is given". It improves the odds; it is not required and not a guarantee.
- **Submission office directory:** for the train's origin division (and the user's boarding division if different), show the office, address, opening hours and days, drop point instructions, accepted methods (hand delivery, drop box, email), suggested lead time, and the last verified date and source (official, traveller-reported, or team-verified by phone). Warn when the suggested submission day falls on a weekend or holiday.
- Generate the request letter (Appendix C1) with the user's trip and pet details, as PDF and DOCX, and as plain text for email.
- Checklist of enclosures: ticket copy, photo ID, vaccination record, fitness certificate if available.
- Let the user record "Letter submitted on [date] at [office]".

**M3.4 Chart day**

- Compute the expected first-chart time from the departure time at the charting station, using the charting rule in the rules data (Section 9.5).
- Reminders: the day before, 1 hour before expected charting, and at expected charting.
- At charting, prompt: "Check your coupe status" with a link to PNR status, then "What did you get?" with four answers:
  - **Full coupe or cabin on my PNR:** go to pet ticket booking (M3.5).
  - **A cabin shared with another PNR:** explain that the pet is not permitted under the rules unless the whole cabin is on one PNR, that a co-passenger objection means the dog moves to the guard's van with no refund, and that the TTE has discretion. Then go to the fallback plan (M3.6).
  - **No coupe or cabin:** go to the fallback plan (M3.6).
  - **Not sure:** show how to read berth numbers against the coach layout. [VERIFY coupe berth numbers per coach type.]

**M3.5 Pet ticket**

- Primary path: IRCTC online pet booking (parcel.indianrail.gov.in/LTBook). Window: after the first chart until the final chart. Step-by-step guide with screenshots. Tell the user to save a screenshot of the confirmation, as email delivery is unreliable.
- Fallback path: parcel office at least 3 hours before departure, with the documents list. Show the parcel office's opening hours and warn when an early-morning departure falls outside them. [Opening hours: collect per station.]
- Known error: "No Dog/Cat booking allowed". Show the steps: check that the coupe is on one PNR, retry, try another browser, then go to the parcel office.
- Show the fare estimate (Section 9.4) and the non-refund rule clearly.

**M3.6 Fallback plan (no coupe, or refusal)**

- Options, in order, based on train data:
  1. Ask at the origin station's reservation office or the TTE whether a coupe can be reassigned after charting (*Traveller-reported*, not guaranteed).
  2. Brake-van dog box, if the train likely has one. Only one dog fits per dog box, and trains typically carry one box [VERIFY]. Show the safety warning (heat, long journeys, no ventilation control) and when not to use it (for example summer journeys over 12 hours, brachycephalic breeds, elderly pets). [Thresholds to be advised by a vet; store as data.]
  3. Cancel and rebook another train or date. Show the cancellation cost logic and the pet freight non-refund rule.
  4. Switch mode: flight (if the pet qualifies), road, or a pet transporter.
- The user can mark which option they took; this feeds the trip report.

**M3.7 On board**

- Checklist: leash, collar and chain, muzzle (depending on rules and temperament), water, food, pee pads, litter tray for cats, cleaning supplies, printed documents.
- Halt list with times for toilet breaks.
- **Rule sheet** (Appendix C2): official text with source citations and a QR code to the IRCTC terms, to show a TTE or parcel clerk.
- "If asked to pay a fine": explain the official penalty rule, that any fine should come with an official railway receipt [VERIFY receipt type, e.g. Excess Fare Ticket], and how to complain via RailMadad (Appendix C6). Do not suggest informal payments.

**M3.8 Cats, kittens and puppies**

- Explain the basket rule: kittens and puppies in a basket may travel in any class with the usual luggage charge. Stations enforce this inconsistently; carry the rule sheet.
- Adult cats: same as dogs (1AC coupe or cabin), or caged in the brake van.

**AC for M3**

- Given a train number, boarding station and date, the app shows whether the train has 1AC and whether boarding is at the origin.
- The generated letter includes PNR, train, date, berths, pet details and the official IRCTC basis, and downloads as PDF and DOCX.
- Chart-time reminders fire at the computed times in the user's time zone (IST).
- Choosing "No coupe" on chart day always shows at least two fallback options suited to that train.
- The rule sheet prints on one A4 page with sources and the date last checked.

### M4. Domestic flight playbook

**Requirements**

- For the chosen airline and mode (cabin, hold, cargo), show: weight limits (pet plus carrier), carrier dimensions, number of pets per flight and per passenger, minimum age, breed restrictions, fee, booking channel and deadline, reporting time, certificate validity, seat rules, muzzle rule, non-stop-only rules.
- **Carrier fit check:** compare the saved carrier's type, dimensions and weight with the airline limit, including hold and cargo crate limits for larger dogs. Show pass, fail or borderline, with the margin. "Borderline" means within 0.5 inch on any side or within 0.5 kg of the weight limit. Air India cabin requires a soft carrier.
- **Hold and cargo safety:** for hold or cargo options, show vet-reviewed guidance on heat, long layovers, crate training and breed risk.
- **Booking script:** for phone-only airlines (Akasa), show a call script with the details to have ready. For portal-based airlines (Air India), show the steps and the documents to upload. Record "Pet booking confirmed: reference ___".
- **Certificate timing:** compute the window for the vet visit from the airline's validity rule and the flight date. Show the exact dates.
- **Airport day timeline:** arrive by [time], counters to visit, weighing, payment, security, boarding.
- **Cabin slot risk:** explain the per-flight cap and advise booking the pet slot as soon as the ticket is booked.
- **Sedation:** state airline rules (sedated pets refused) and advise speaking to a vet. Never recommend a drug or dose.
- **Connecting flights:** show whether each airline allows pets on connections (Air India: Air India-to-Air India connections only; Akasa: non-stop only) and whether fees apply per sector.

**AC**

- A saved carrier of 18×11×9 inches is marked "fails" for Air India (17×10×9) and "passes" for Akasa (19×12.6×10).
- For a flight on 20 November, Air India's certificate window shows "vet visit between 13 and 18 November". Reason: the certificate must be issued within 7 days before departure (counted inclusively), and it must be uploaded before the 48-hour pet booking cutoff.
- Akasa's certificate rule displays the conflict: "Get it within 72 hours of travel to be safe. Akasa's FAQ says it is valid for 15 days." [VERIFY]
- Selecting IndiGo or Air India Express shows "This airline does not carry pets" with the source.

### M5. International planner

**Requirements**

- Directions: **export from India** (to USA, Canada, UK, EU countries, UAE in v1) and **import into India** (from any country).
- **Backward timeline** from the travel date, per destination. Example for the UK or EU:
  1. Microchip (before or on the day of rabies vaccination).
  2. Rabies vaccination.
  3. Titre blood sample at least 30 days after vaccination. Sample sent to an approved lab abroad (none in India). Include guidance on how vets and couriers send the sample, and a lab turnaround allowance.
  4. Wait 3 months from the sample date.
  5. Destination health certificate within 10 days of travel.
  6. AQCS export certificate appointment about 7 days before travel; certificate valid 10 days.
  7. Airline booking (cargo only for the UK).
- The planner shows the **earliest possible travel date** given today's date and the pet's current status, and flags if the user's target date is impossible.
- **USA:** explain the CDC high-risk rules, the CDC-approved lab requirement, the registered care facility reservation, minimum age of 6 months, and the CDC Dog Import Form. Flag that Air India does not carry pets to the USA.
- **Round trips from the USA:** warn visitors bringing a US-vaccinated dog to India and back that entering a high-risk country after filing the CDC Dog Import Form invalidates the receipt, and show what to redo.
- **Transit traps:** warn when a routing transits the EU or UK, which can add titre or entry requirements even for a transit.
- **Import into India:**
  - Decision: transfer of residence (two years abroad) or visitor or re-import. Show whether an AQCS NOC alone is enough or a DGFT licence is needed. When a DGFT licence is needed, add it to the timeline with a lead time of 30–45 days (traveller-reported).
  - Accompanied or unaccompanied: an unaccompanied pet must arrive within one month of the owner's arrival; otherwise advance customs permission is needed.
  - ATITHI 2.0 advance NOC: at least 7 days before departure; steps and documents.
  - Rabies vaccine window: more than one month and within 12 months before departure.
  - Arrival: inspection, final NOC, and the 15-day quarantine rule if the pet is found unfit.
  - Two-pet limit under baggage rules.
- **Australia and New Zealand:** explain that direct travel from India is not possible and outline the third-country route. No detailed planner in v1.
- **DIY versus agency:** show published cost ranges from research, what agencies typically handle, and when an agency is advisable (USA with CDC rules, cargo-only routes, large dogs, tight timelines). No agency listings in v1.

**AC**

- For a UK move with no titre done yet, the earliest travel date is: the later of (vaccination date + 30 days) and today, plus 3 months, plus the lab turnaround allowance. If not yet vaccinated, the vaccination date is today.
- For import into India, answering "visitor" shows the DGFT licence requirement. Answering "transfer of residence" shows NOC only when the user confirms at least two years' residence abroad; otherwise it shows the DGFT or re-import route.
- Each destination page shows its sources and last-checked dates.

### M6. Document generator

**Requirements**

- Templates (Appendix C), filled from pet and trip data, editable before export:
  - C1 Coupe or cabin request letter (rail).
  - C2 Rule sheet for staff (rail), with citations and QR code.
  - C3 Vet fitness certificate format (rail and air), for the vet to complete and sign. Watermark: "Format for vet use. Valid only when completed, signed and stamped by a registered veterinarian."
  - C4 AQCS export appointment request email.
  - C5 Import into India document checklist (ATITHI NOC).
  - C6 RailMadad complaint text.
  - C7 Airline pet booking request (Air India portal text; Akasa call script).
- Export as PDF and DOCX; copy as text; share to WhatsApp or email.
- Templates are stored as data with versions, so wording can change without a code release.

**AC**

- All placeholders are filled or clearly highlighted if missing.
- The vet format cannot be exported without the watermark.
- Generated PDFs render correctly on A4 and on phone screens.

### M7. Trip timeline and reminders

**Requirements**

- Each trip has a timeline of dated tasks from the playbook (for example "Book tickets when booking opens", "Submit coupe letter", "Vet visit", "Chart check", "Book pet ticket", "Leave for station").
- Task states: to do, done, skipped, failed (failed opens the fallback flow).
- Reminders by web push and email in v1; WhatsApp in v1.1.
- Return trips get their own timeline, pre-filled from the outbound trip.

**AC**

- Changing the travel date recalculates all task dates.
- A user can turn reminders off per trip.

### M8. Rules library (public)

**Requirements**

- Public, server-rendered pages per operator and topic (for example "/rules/rail/indian-railways/dogs", "/rules/air/air-india", "/rules/international/uk-from-india").
- Each rule shows: plain-language statement, official text quote where available, source link, source type, last-checked date, confidence tag, and change history.
- A "What changed" page listing recent rule changes.
- Common myths section, sourced from `RESEARCH.md` Section 5 (for example "7 kg on Air India is out of date").
- SEO: page titles, descriptions, structured data (FAQPage where appropriate).

**AC**

- Every rule page shows a last-checked date. Rules past their review date show a visible "Being re-checked" notice.
- The library renders without JavaScript for search engines.

### M9. Trip reports

**Requirements**

- After the travel date, prompt a short report. Rail questions: train, boarding station, origin or not, berths booked, letter submitted (where, when), coupe allotted (yes or no), how the pet ticket was booked (online or parcel office), any problems, dog box present (if used), documents checked (yes or no). Air questions: airline, route, cabin or hold, booking experience, documents checked, problems.
- Reports are anonymous in public views. Show aggregated counts only when there are at least 5 reports for a train or office.
- Moderation queue in admin for free-text fields.

**AC**

- A report takes under 90 seconds.
- Aggregated insight appears on train pages (for example "7 of 9 travellers boarding at the origin got a coupe") only when the threshold is met, labelled *Traveller-reported*.

### M10. Shareable checklists

**Requirements**

- For each trip and for each generic scenario (rail 1AC, dog box, Air India cabin, Akasa cabin, UK export, import into India), generate:
  - A portrait image (1080×1350) for Instagram and WhatsApp.
  - A one-page PDF.
- Include the "last checked" date and a link back to the live page.

**AC**

- Images are legible on a phone at full width. Text is never below 28 px at 1080 px width.

### M11. Admin

**Requirements**

- Role-based access (admin, editor).
- Rules editor: create, edit, supersede rules; attach sources; set review interval; record who verified and how.
- Freshness dashboard: rules due or overdue for review, grouped by change frequency (Section 10).
- Submission office directory editor.
- Trip report moderation.
- Template editor with versions.

**AC**

- Editing a rule creates a new version; the old version is kept and shown in the change history.
- Overdue rules appear at the top of the dashboard.

---

## 9. Rules engine

### 9.1 Principle

Rules are data, not code. The engine reads rule records and the trip context and returns options, conditions, blockers and steps. Content editors can update rules without a code release.

### 9.2 Rule record (schema)

```ts
type Rule = {
  id: string;                 // e.g. "air.air-india.cabin.max-weight"
  mode: "rail" | "air" | "international";
  operator: string;           // e.g. "indian-railways", "air-india", "akasa", "aqcs", "uk"
  topic: string;              // e.g. "max_weight_kg", "carrier_dimensions_in", "fee_inr"
  appliesTo?: {
    species?: ("dog" | "cat")[];
    travelClass?: string[];   // e.g. ["1A","FC"], ["cabin"], ["hold"]
    direction?: "export" | "import";
  };
  value: unknown;             // typed per topic, validated with Zod
  statement: string;          // plain-language text shown to users
  officialQuote?: string;     // verbatim rule text if available
  sources: {
    url: string;
    title: string;
    type: "official" | "secondary" | "traveller";
    sourceDate?: string;      // date on the source document
    retrievedAt: string;      // when we read it
  }[];
  confidence: "official" | "secondary" | "traveller" | "conflicting";
  verifiedAt: string;         // last time a person checked it
  reviewEveryDays: number;
  effectiveFrom?: string;
  supersedes?: string;        // previous version id
  notes?: string;             // internal
};
```

### 9.3 Evaluation

```ts
evaluateOptions(pets: Pet[], trip: Trip, rules: Rule[]): TravelOption[]

type TravelOption = {
  id: string;                         // e.g. "air.akasa.cabin"
  status: "possible" | "conditional" | "not_possible";
  blockers: RuleRef[];                // reasons it is not possible
  conditions: RuleRef[];              // things that must be true
  costEstimateInr?: { min: number; max: number; basis: string };
  leadTimeDays?: number;
  steps: StepTemplateRef[];           // drives the timeline
  riskNotes: string[];
};
```

Decision tables for v1 are in Appendix B. The engine must:

- Check every hard limit (weight, dimensions, age, breed, species, infant, wheelchair, pets per passenger, pets per flight, route bans).
- Mark an option *conditional* when success depends on something outside the user's control (coupe allotment, cabin slot availability, station discretion).
- Mark data with `confidence: "conflicting"` as a condition with a warning, not as a blocker.

### 9.4 Rail fare estimate

- Formula: luggage rate (Scale L) for the distance × notional weight (60 kg for a dog with the passenger; 30 kg for the dog box; 20 kg for a basket), with a minimum of ₹30, plus 2% development charge, plus 5% GST.
- Until the Scale L table is sourced [VERIFY], show the reported range (about ₹160–800) labelled *Traveller-reported*, not a computed figure.

### 9.5 Chart time

- Store the charting rule as data. Current default (from about December 2025):
  `{ firstChartHoursBefore: 10, morningWindow: ["05:00", "14:00"], morningChartTime: "20:00", morningChartDay: "previous", relativeTo: "charting_station" }`
- History: about 4 hours until July 2025; 8 hours (21:00 previous day for morning trains) from 10 July 2025.
- Confidence: official change reported by news; [VERIFY against the Railway Board circular].
- Always tell users the time is an estimate and to check PNR status.

---

## 10. Content, data sources and freshness

### 10.1 Sources

| Data | Source | v1 approach |
|---|---|---|
| Railway pet rules | IRCTC terms PDF, zonal railway releases, Coaching Tariff notes | Seeded from research; team reviews |
| Charting and booking windows | Railway press releases | Data record |
| Train schedules, routes, halts, classes | No public official API | Adapter interface; licensed third-party provider or curated seed data [VERIFY licensing] |
| Coach composition (1AC coaches, ICF vs LHB) | Not reliably available | Curated seed for major trains; trip reports fill gaps |
| Submission offices | Traveller reports and phone verification | Curated directory; start with Delhi, Bengaluru, Secunderabad, Mumbai, Chennai, Kolkata |
| Airline rules | Airline pages and PDFs | Seeded; monthly checks |
| AQCS and destination rules | Government sites | Seeded; quarterly or monthly checks |

### 10.2 Review intervals (from research)

| Rule set | Review every |
|---|---|
| Airline fees | 30 days |
| Airline limits and pet acceptance | 90 days |
| Air India route bans | 90 days |
| IRCTC online booking and charting | 90 days, plus news watch |
| Railway luggage rates | At each fare revision |
| AQCS process | 90 days |
| CDC rules and lab list | 30 days |
| New Zealand standard | 30 days until April 2027 |
| UK, EU, Singapore | 90–180 days |

### 10.3 Change monitoring (v1.1)

- A scheduled job fetches each official source page, stores a hash, and flags changes to the admin dashboard for human review. No automatic rule changes.

---

## 11. Technical approach (recommended)

These are recommendations. Change them if you have a reason.

| Layer | Choice | Notes |
|---|---|---|
| Framework | Next.js (App Router), TypeScript | Server rendering for public rules pages; PWA support |
| Styling | Tailwind CSS | Mobile-first design tokens |
| Database, auth, storage | Supabase (Postgres, Auth with phone OTP and email, Storage) | Row-level security on all user data |
| Validation | Zod | Shared schemas for rules and forms |
| Rules content | Seed JSON in repo, loaded into Postgres; edited through admin | Versioned records |
| PDF and DOCX | `@react-pdf/renderer` and `docx` | Server-side generation |
| Notifications | Web push (service worker) and email (Resend or similar) | WhatsApp via a Business API provider in v1.1 |
| Offline | Service worker caching the trip pack and rule sheet | Works on trains with poor coverage |
| Analytics | PostHog (or similar) | Privacy-respecting, event-based |
| i18n | next-intl | English in v1; Hindi in v1.1 |
| Train data | `TrainDataProvider` interface with a seed implementation | Swap in a licensed API later |

### 11.1 Data model (core entities)

- `User` (id, phone, email, name, locale)
- `Pet` (id, userId, name, species, breedId, dob, weightKg, weightWithCarrierKg, microchipNo, microchipDate, sex, photoUrl)
- `Breed` (id, species, name, brachycephalic)
- `Vaccination` (id, petId, type, dateGiven, validUntil, vetName, documentId)
- `Document` (id, userId, petId, type, fileUrl, issuedOn, expiresOn)
- `Carrier` (id, userId, type, lengthIn, widthIn, heightIn, weightKg)
- `Trip` (id, userId, origin, destination, date, returnDate, travellers, hasInfant, needsWheelchair, direction, status)
- `TripPet` (tripId, petId)
- `TripOption` (id, tripId, optionKey, status, chosen)
- `TripLeg` (id, tripId, mode, operator, trainNo or flightNo, boardingStation, originStation, class, pnr, bookingRef)
- `Task` (id, tripId, key, title, dueAt, state, reminderAt)
- `Rule` (as in Section 9.2) and `RuleVersion`
- `Source` (id, url, title, type, sourceDate, retrievedAt)
- `Train` (number, name, originStation, destinationStation, has1A, oneACoaches, rakeType, stops[])
- `Station` (code, name, city, division, zone)
- `SubmissionOffice` (id, division, zone, officeName, address, openingHours, dropInstructions, methods[], leadTimeDays, verifiedAt, verificationType)
- `ParcelOffice` (stationCode, openingHours, notes, verifiedAt)
- `Template` and `TemplateVersion`
- `GeneratedDocument` (id, tripId, templateVersionId, fileUrl, createdAt)
- `TripReport` (id, tripId, mode, answers JSON, moderated)
- `AdminUser` (id, role)

---

## 12. Non-functional requirements

- **Performance:** first load under 2.5 seconds on a mid-range Android phone on 4G. Rules pages statically generated where possible.
- **Offline:** trip pack, rule sheet and checklist available offline once opened.
- **Accessibility:** WCAG 2.1 AA; large tap targets; readable at 200% zoom.
- **Privacy:** comply with India's Digital Personal Data Protection Act, 2023. Collect only what the trip needs. Clear consent for reminders. Users can export and delete their data.
- **Security:** row-level security on all user tables; signed URLs for documents; no documents in public storage.
- **Reliability:** reminders must be scheduled server-side, not only on the device.
- **Content accuracy:** no rule without a source; admin warning on overdue reviews.

## 13. Legal and trust

- Clear statement that the app is not affiliated with Indian Railways, IRCTC, any airline or any government body.
- Disclaimer that rules change and users should confirm with the operator; every rule links to its source.
- No advice to use fictitious names, misstate pet details, or make informal payments.
- Vet certificate formats are templates only (see principle 6).
- Veterinary safety advice (heat, sedation, long journeys) is general, reviewed by a vet before launch, and tells users to consult their vet.

## 14. Success metrics

**Activation**

- % of visitors who create a trip.
- % of trips with a pet profile and a chosen option.

**Value**

- % of trips where the user completes the checklist.
- Rail: % of trips reporting a coupe allotted (tracked over time; not a target we control).
- % of users who open the fallback flow and report a resolved trip.

**Retention**

- % of users who plan a second trip within 12 months. (Reference point: 26% of Akasa pet flyers flew more than once between November 2022 and early 2026. That period is about three years, so it is not a like-for-like 12-month benchmark.)

**Quality**

- % of rules within their review interval (target: 95%).
- Trip reports submitted per completed trip (target: 30%).

**Distribution**

- Checklist shares per trip.
- Organic search sessions to rules pages.

## 15. Monetisation (later; not in v1)

Options observed in the market, for later evaluation:

- Referral fees from vetted relocation agencies for complex international moves.
- Partner vet bookings for fitness and health certificates.
- Paid concierge help (for example submitting the coupe letter in person).
- Affiliate sales of IATA-compliant carriers and crates, or crate rental.
- A paid tier for multi-pet households (vault, reminders, multiple trips).

v1 stays free so the rules library and trip reports can grow.

## 16. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Rules go stale and users are harmed | Review intervals, change monitoring, visible last-checked dates, conflict labels |
| Users treat community tips as guarantees | Clear labels; never show probabilities as percentages in v1 |
| No reliable train data source | Adapter interface; curated seed; trip reports; manual entry fallback |
| Legal risk from "gaming" advice | Principle 5; legal review of rail content before launch |
| Pet harm in dog box or cargo | Vet-reviewed safety guidance; explicit "do not use when" conditions |
| Low report volume | Short reports, reminder after trip, show users how reports help others |
| Scam agencies later | Vetting criteria before any agency listing (v2) |

## 17. Build plan

Build in this order. Each milestone ends with working, tested software.

1. **Foundation.** Next.js app, Tailwind tokens, Supabase project, auth (phone OTP and email), Zod schemas, seed loader for rules (Appendix A).
2. **Pets and vault (M1).** Pet CRUD, breeds list with brachycephalic flag, vaccinations, document upload, carrier profile.
3. **Rules engine and options (M2).** Implement `evaluateOptions` with Appendix B tables. Unit tests for every AC in M2.
4. **Rules library (M8).** Public pages from rule data, sources, last-checked dates, myths page.
5. **Rail playbook (M3).** Train data adapter with seed data, booking strategy, submission office directory, letter generator (C1), chart-day flow, pet ticket guide, fallback flow, rule sheet (C2).
6. **Domestic flight playbook (M4).** Airline detail views, carrier fit check, certificate window calculator, booking scripts (C7), airport timeline.
7. **Timeline and reminders (M7).** Task generation from option steps, date recalculation, web push and email.
8. **International planner (M5).** Backward timeline engine, USA, Canada, UK, EU, UAE export; import into India flow; Australia and New Zealand explainer; C4 and C5.
9. **Document generator (M6).** PDF and DOCX export for all templates; watermarking for C3.
10. **Shareable checklists (M10)** and **offline trip pack**.
11. **Trip reports (M9)** and **admin (M11)**.
12. **Hardening.** Accessibility pass, performance budget, analytics events, legal copy, vet review of safety content, content verification of every [VERIFY] item.

## 18. Open questions

1. Train data provider: which licensed API, and at what cost? Or curated data only for v1?
2. Pets per PNR on trains: one or more?
3. First chart timing: confirm the December 2025 change (10 hours; 20:00 previous day for morning trains) from the Railway Board circular.
4. Scale L luggage rate table, for an exact rail fare.
5. Muzzle versus collar and chain on trains.
6. Which trains still carry brake-van dog boxes.
7. Submission offices for each railway division, and whether any accept email.
8. Akasa certificate validity: 72 hours or 15 days.
9. SpiceJet domestic pet cargo: current availability, weights, fees.
10. Vande Bharat sleeper pet box: official circular, routes, charges.
11. Product name and brand.
12. Which vet will review safety content before launch.

---

## Appendix A: Seed rules (v1)

All values as of 25 September 2026. Source URLs are listed in `RESEARCH.md` and `research-notes/SOURCES.md`. Implement as JSON records matching Section 9.2.

Where a row shows two confidence labels (for example "official / conflicting"), store the weaker one. Treat "official (snippet)" as "secondary".

### A1. Indian Railways

| id | Value | Confidence |
|---|---|---|
| rail.ir.dog.allowed_classes | ["1A","FC"] with full coupe (2) or cabin (4) on one PNR | official |
| rail.ir.dog.disallowed_classes | All classes except 1A and FC (includes 2A, 3A, 3E, EC, CC, SL, 2S, GN) | official |
| rail.ir.coupe.request_office | DRM office or GM office; "due consideration", no guarantee | official |
| rail.ir.pet_ticket.online_window | After first chart until final chart | official |
| rail.ir.pet_ticket.online_url | https://parcel.indianrail.gov.in/LTBook | official |
| rail.ir.pet_ticket.offline_min_hours_before | 3 | official |
| rail.ir.pet_ticket.refund | None, including train cancellation or late running | official |
| rail.ir.docs.fitness_cert_hours_before | 24–48 | official |
| rail.ir.docs.required | Fitness certificate, vaccination card, confirmed ticket, photo ID | official |
| rail.ir.fare.notional_weight_kg | { withPassenger: 60, dogBox: 30, basket: 20, seeingEyeDog: 30 } | secondary (official training notes; tariff not retrieved) [VERIFY] |
| rail.ir.fare.minimum_inr | 30 (+2% development charge, +5% GST) | secondary (official training notes) [VERIFY] |
| rail.ir.fare.reported_range_inr | 160–800 | traveller |
| rail.ir.dogs_per_pnr | 1 (Southern Railway); IRCTC screen allows selecting more | conflicting |
| rail.ir.penalty.unbooked_dog | 6× Scale L; minimum ₹30 or ₹50 | conflicting |
| rail.ir.basket.kitten_puppy_any_class | true, usual luggage charge; Station Master permission | official |
| rail.ir.copassenger_objection | Dog moved to guard's van; no refund | official |
| rail.ir.dogbox.availability | Brake vans of ICF coaches; LHB unknown | official / conflicting |
| rail.ir.dogbox.restraint | Collar and chain; muzzle requirement disputed | conflicting |
| rail.ir.chart.first_chart | ≥10 h before departure at the charting station; 20:00 previous day for departures 05:00–14:00 (from ~Dec 2025) | secondary [VERIFY circular] |
| rail.ir.dogbox.capacity | One dog per dog box | official |

### A2. Domestic airlines

| id | Value | Confidence |
|---|---|---|
| air.air-india.accepts | Dogs, cats | official |
| air.air-india.cabin.max_weight_kg | 10 (incl. carrier) | official |
| air.air-india.cabin.carrier_in | 17×10×9, soft, no wheels | official |
| air.air-india.cabin.class | Economy only; aisle seats in last 5 rows | official |
| air.air-india.cabin.pets_per_flight | 2 (including service animals) | official |
| air.air-india.pets_per_passenger | 1 | official |
| air.air-india.hold.weight_kg | 10–32 (aircraft-dependent); >32 cargo | official |
| air.air-india.fee_inr | { cabin: 7500, hold: 16000 } per sector, plus taxes | official |
| air.air-india.min_age | { cabin: "8 weeks", hold: "3 months" } | official |
| air.air-india.booking | Support portal or counter; ≥48 h before | official |
| air.air-india.report_hours_before | 3 | official |
| air.air-india.health_cert_days | Issued within 7 days of departure | official |
| air.air-india.indemnity_copies | { direct: 2, connecting: 4 } | official |
| air.air-india.brachy_hold | Not permitted | official |
| air.air-india.blocked_with | Infant, wheelchair assistance, unaccompanied minor | official |
| air.air-india.intl_route_bans | No pets to/from USA, Canada, Australia; UK cargo only; no cabin pets India→UAE | official |
| air.akasa.cabin.max_weight_kg | 10 (incl. container) | official |
| air.akasa.cabin.container_in | 19×12.6×10 | official |
| air.akasa.pets_per_flight | { cabin: 2, hold: 1 } | official |
| air.akasa.pets_per_passenger | { cabin: 1, hold: 1 } (some 2025 news reports say 2 per passenger) [VERIFY] | official |
| air.akasa.hold.max_weight_kg | 32; cargo terminal up to 100 | official |
| air.akasa.hold.container_in | ≤36×28×28 (check-in and cargo) | official |
| air.akasa.fee_inr | { cabin: 7500, hold: 15000 } incl. taxes | official |
| air.akasa.booking | Cabin: Akasa Care Centre +91 96061 12131 only. Hold 10–32 kg: airport counter. >32 kg: Care Centre or cargo terminal. ≥24 h before | official |
| air.akasa.report_hours_before | 2 | official |
| air.akasa.health_cert | Within 72 h of travel; FAQ says valid 15 days | conflicting |
| air.akasa.muzzle | Dogs muzzled during flight | official |
| air.akasa.seat | Window | official |
| air.akasa.nonstop_only | true | official |
| air.akasa.min_age | 3 months; not >4 weeks pregnant | official |
| air.akasa.brachy_hold | Not permitted | official |
| air.akasa.esa | Not accepted | official |
| air.fly91.cabin.max_weight_kg | 7 (incl. container) | official |
| air.fly91.cabin.container_in | 17×10×8 | official |
| air.fly91.pets_per_flight | 1 (including service dogs) | official |
| air.fly91.fee_inr | 5000 per sector | official |
| air.fly91.booking | Support ticket; ≥24 h before | official |
| air.fly91.muzzle | Dogs muzzled | official |
| air.alliance.cabin | Dogs, cats, birds; pet ≤5 kg, ≤8 kg with container; kennel ≤18×18×12 in; 2 per flight; last row | official |
| air.alliance.fee | 2× excess baggage rate for the sector | official |
| air.alliance.hold | Not permitted | official |
| air.spicejet.cabin | Not permitted | official |
| air.spicejet.cargo | Domestic cargo via cargocare@spicejet.com; details unpublished | official / conflicting |
| air.indigo.pets | Not carried (service dogs only) | official / secondary |
| air.air-india-express.pets | Not carried (service animals by exception) | official |
| air.star-air.pets | Not carried | official |
| air.all.sedation | Sedated pets refused (Air India, Akasa, Fly91) | official |

### A3. International

| id | Value | Confidence |
|---|---|---|
| intl.aqcs.export.appointment_days_before | ~7 | official |
| intl.aqcs.export.cert_valid_days | 10 | official |
| intl.aqcs.export.fee_inr | 500 | official |
| intl.aqcs.export.stations | Delhi, Mumbai, Chennai, Kolkata, Bengaluru, Hyderabad | official |
| intl.aqcs.export.docs | Vet fitness cert, microchip cert, vaccination record, passport, ticket, airway bill (cargo), 2 photos (4×6 in), destination requirements | official |
| intl.aqcs.import.advance_noc | ATITHI 2.0; ≥7 days before departure; issued within 1 working day | official |
| intl.aqcs.import.fee_inr | 1000 | official |
| intl.aqcs.import.max_pets_baggage | 2 dogs/cats (transfer of residence, 2 years abroad) | official |
| intl.aqcs.import.dgft_needed | When not transfer of residence or re-import | official |
| intl.aqcs.import.rabies_window | >1 month and ≤12 months before departure (dogs/cats ≥3 months old) | official |
| intl.aqcs.import.quarantine_if_unfit_days | 15 | official |
| intl.titre.labs_in_india | None approved by CDC or EU; nearest listed: CVRL Dubai | official |
| intl.usa.high_risk | India high-risk for rabies | official |
| intl.usa.requirements | Microchip before vaccine; CDC foreign vaccination form (≤30 days, govt vet endorsed); titre from CDC-approved lab (≥30 days after vaccine, ≥28 days before entry) or 28-day quarantine; registered care facility reservation; min age 6 months; CDC Dog Import Form | official |
| intl.uk.requirements | Unlisted country: titre ≥30 days after vaccine, ≥0.5 IU/ml, 3-month wait; GB health certificate; enter within 10 days; tapeworm treatment 1–5 days before (dogs); cargo on approved routes | official |
| intl.eu.requirements | Titre at designated lab; 3-month wait; EU health certificate within 10 days; designated entry point; max 5 pets | official |
| intl.uae.requirements | MOCCAE import permit; titre for high-risk countries; min 15 weeks | secondary |
| intl.canada.requirements | Personal pets: rabies certificate; commercial/rescue dogs from high-risk countries banned | official / secondary |
| intl.australia.direct | Not possible; ~180 days in approved country first | official (snippet) |
| intl.nz.direct | Not possible; new standard mandatory from 1 Apr 2027 | official / secondary |

---

## Appendix B: Decision tables (v1)

### B1. Domestic air, cabin

| Check | Air India | Akasa | Fly91 | Alliance Air |
|---|---|---|---|---|
| Species | Dog, cat | Dog, cat | Dog, cat | Dog, cat (birds out of scope) |
| Weight with carrier ≤ | 10 kg | 10 kg | 7 kg | 8 kg (pet ≤5 kg) |
| Carrier ≤ | 17×10×9 in (soft) | 19×12.6×10 in | 17×10×8 in | 18×18×12 in |
| Minimum age | 8 weeks | 3 months | 3 months | Not stated |
| Traveller with infant | Blocked | Blocked (hold allowed) | Blocked | Not stated |
| Wheelchair assistance | Blocked | Not recommended | Blocked | Not stated |
| Pets per passenger | 1 | 1 | 1 (per flight) | Not stated |
| Connecting flight | Air India-to-Air India only (4 indemnity copies) | Blocked (non-stop only) | Check | Check |
| Route served | Check route | Check route | Small network | Regional network |
| Always conditional on | Slot availability (2/flight) | Slot availability (2/flight) | Slot (1/flight) | Slot (2/flight), captain's approval |

### B2. Domestic air, hold

| Check | Air India | Akasa | SpiceJet |
|---|---|---|---|
| Weight with crate | 10–32 kg (aircraft-dependent) | ≤32 kg (cargo ≤100 kg) | Unknown [VERIFY] |
| Brachycephalic breed | Blocked | Blocked | Unknown |
| Minimum age | 3 months | 3 months | Unknown |
| Per flight | Aircraft-dependent | 1 | Unknown |

### B3. Rail

| Situation | Result |
|---|---|
| Dog or adult cat, 1A/FC, full coupe or cabin on one PNR, confirmed | Conditional: needs coupe/cabin allotment at charting |
| Dog or adult cat, any other class | Not possible with passenger; offer dog box if train likely has one |
| Kitten or puppy that fits in a basket, any class, confirmed ticket | Conditional: station discretion; carry rule sheet |
| Train has no 1A | 1A option not possible; show dog box (if any) and other trains |
| Ticket waitlisted or RAC | Coupe not possible until confirmed |
| Solo traveller with one berth | Conditional, low chance; explain options |
| Boarding station is not the origin | Conditional; add traveller-reported note on lower odds |
| Brachycephalic, elderly, or summer journey >12 h | Dog box shown with strong safety warning [thresholds VERIFY with vet] |
| Two or more dogs | Warn: one dog per PNR per Southern Railway; unclear on IRCTC |
| Two or more dogs, dog box | Warn: one dog per dog box; usually one box per train [VERIFY] |
| Cabin allotted but shared with another PNR | Not permitted under the rules; go to fallback |

### B4. International, first cut

| Destination | Direct from India | Titre | Wait after titre | Notes |
|---|---|---|---|---|
| USA | Yes | Yes (CDC lab, abroad) | Sample ≥28 days before entry | CDC facility reservation; Air India won't carry |
| Canada | Yes | No (personal pets) [VERIFY] | — | Few airlines take dogs |
| UK | Yes | Yes (approved lab, abroad) | 3 months | Cargo only on approved routes |
| EU | Yes | Yes (designated lab, abroad) | 3 months | Max 5 pets |
| UAE | Yes | Yes [VERIFY] | [VERIFY] | Import permit; no cabin pets India→UAE on Air India |
| Australia | No | — | — | 180 days in approved country first |
| New Zealand | No | — | — | New standard 2026–27 |

---

## Appendix C: Document templates

Placeholders are in `{{double_braces}}`. All templates are editable by the user before export.

### C1. Coupe or cabin request letter (rail)

> To,
> {{office_title}}
> {{office_name}}, {{division}} Division, {{zone}}
> {{office_address}}
>
> Date: {{today}}
>
> **Subject: Request for allotment of a {{coupe_or_cabin}} in First AC to travel with my pet {{species}}, PNR {{pnr}}**
>
> Respected Sir or Madam,
>
> I have booked {{berth_count}} berths in First AC on Train No. {{train_no}} ({{train_name}}), from {{from_station}} to {{to_station}}, departing on {{journey_date}}. The PNR is {{pnr}}.
>
> I will be travelling with my pet {{species}}, {{pet_name}}, a {{breed}} aged {{pet_age}} and weighing about {{pet_weight}} kg. {{pet_name}} is fully vaccinated, and I will carry the vaccination record and a veterinary fitness certificate issued within 48 hours of the journey.
>
> Under the IRCTC terms for carrying dogs and cats, a pet may travel with its owner in First AC only when a full coupe or cabin is allotted to a single PNR. The terms also state that requests for such allotment may be made to the Divisional Railway Manager's office or the General Manager's office.
>
> I request you to kindly allot a {{coupe_or_cabin}} to PNR {{pnr}} at the time of charting.
>
> I will book my pet's luggage ticket after the chart is prepared. I take full responsibility for my pet's behaviour, hygiene and care throughout the journey.
>
> Enclosures:
> 1. Copy of ticket
> 2. Copy of photo ID
> 3. Copy of vaccination record
> {{#if fitness_cert}}4. Veterinary fitness certificate{{/if}}
>
> Yours faithfully,
> {{passenger_name}}
> Phone: {{phone}}
> Email: {{email}}

### C2. Rule sheet for staff (rail), one A4 page

- Title: "Travelling with a pet dog or cat: Indian Railways rules"
- Passenger, train, PNR, coupe or cabin number, pet ticket number.
- Quoted official text (from rules data, with source and date):
  - Dogs and cats may travel with the owner in First AC or First Class when the full coupe or cabin is allotted to a single PNR. (IRCTC)
  - Puppies and kittens that can be carried in a basket may be carried in any class after payment of the usual charges. (IRCTC)
  - Pet ticket booked online after charting appears on the TTE's handheld device. (IRCTC)
  - Documents carried: fitness certificate (date), vaccination card.
- QR code to the IRCTC terms PDF.
- Footer: "Rules checked on {{verified_at}}. This sheet is prepared by the passenger for reference."

### C3. Veterinary fitness certificate format

Watermarked: *"Format for vet use. Valid only when completed, signed and stamped by a registered veterinarian."*

> **Certificate of health and fitness to travel**
>
> I have examined the animal described below on {{exam_date}} at {{exam_time}}.
>
> Species: {{species}} Breed: {{breed}} Sex: {{sex}}
> Name: {{pet_name}} Age: {{pet_age}} Weight: ____ kg
> Colour and markings: ________ Microchip no.: {{microchip_no}}
> Owner: {{owner_name}} Phone: {{phone}}
>
> Rabies vaccination: date ________ valid until ________
> Other vaccinations: ________
>
> In my opinion, the animal is healthy, free from any infectious or contagious disease, and fit to travel by {{mode}} on {{travel_date}}.
>
> Veterinarian's name: ________
> Registration no.: ________
> Clinic address: ________
> Signature and stamp: ________ Date: ________

### C4. AQCS export appointment request (email)

> Subject: Request for appointment for export health certificate, pet {{species}}, departure {{departure_date}}
>
> Dear Sir or Madam,
>
> I plan to travel from {{departure_airport}} to {{destination_country}} on {{departure_date}} with my pet {{species}}, {{pet_name}} ({{breed}}, microchip {{microchip_no}}).
>
> I request an appointment at the AQCS {{station}} station for inspection and issue of the export health certificate, around {{appointment_target_date}}.
>
> I will bring the local vet's fitness certificate, microchip certificate, vaccination record, my passport and ticket, {{#if cargo}}the airway bill, {{/if}}two 4×6 inch photographs of my pet, and the destination country's requirements ({{destination_docs}}).
>
> Please let me know an available date and time.
>
> Regards,
> {{owner_name}}
> {{phone}}

### C5. Import into India checklist (ATITHI 2.0 advance NOC)

- Owner passport and visa or residence proof; proof of transfer of residence (if applicable).
- Air ticket (accompanied pet) or airway bill (unaccompanied pet, arriving within one month of the owner; otherwise advance customs permission).
- Official health certificate with Annexure 2.1.1, signed by the exporting country's competent authority, with the owner's name matching the ticket. The pet must be examined within 7 days of shipment.
- Rabies vaccination certificate (given more than 1 month and within 12 months before departure).
- Microchip certificate.
- DGFT import licence (if not transfer of residence or re-import).
- Apply on ATITHI 2.0 at least 7 days before departure. Fee ₹1,000 via Bharatkosh.
- On arrival: present the pet to AQCS for inspection and the final NOC.

### C6. RailMadad complaint

> I am travelling on Train No. {{train_no}}, PNR {{pnr}}, coach {{coach}}, with my pet {{species}} booked on luggage ticket {{luggage_ticket_no}}. At {{time}} near {{location}}, {{description_of_issue}}. I hold a confirmed {{coupe_or_cabin}} allotted to my PNR, as required by the IRCTC terms for carrying dogs and cats. I request assistance and a review of this incident.

### C7. Airline pet booking request

**Air India (support portal text)**

> I have booked PNR {{pnr}} on flight {{flight_no}} from {{from}} to {{to}} on {{date}}. I request a booking to carry my pet {{species}} ({{breed}}, {{pet_weight}} kg; with carrier {{weight_with_carrier}} kg) in the {{cabin_or_hold}}. Carrier size: {{carrier_dims}}. Attached: vaccination record, health certificate (or date it will be issued), indemnity form. Please confirm the booking and fee.

**Akasa (call script)**

- Have ready: PNR, flight number and date, pet species, breed, age, weight with container, container dimensions.
- Ask: "Is an in-cabin pet slot available on this flight?" Then: "Please add the pet booking to my PNR and confirm the fee and reference number."
- Note the reference number and the agent's name.
