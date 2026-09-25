# Pet Travel in India: Market Research

**Date:** 25 September 2026
**Scope:** Pet parents in India travelling by train, domestic flight, and international flight (export from India and import into India).
**Purpose:** Evidence base for the product requirements document (PRD.md).

---

## 1. Summary

1. **The core problem is uncertainty, not missing information.** On trains, people do everything right and still learn only at charting, hours before departure, whether they can take their dog. On flights, cabin slots are capped at one to two pets per flight and go first come, first served.
2. **The rules exist, but they are scattered and contradict each other.** Railway zones, parcel offices, TTEs, airline call centres and viral reels all give different answers. Outdated advice (Air India "7 kg", "go to the parcel office 3 hours early") keeps circulating because nobody corrects old posts.
3. **The railway coupe process runs on informal knowledge.** Which office to send the letter to, which box to drop it in, and which trains work best are shared on Reddit like trade secrets. At least seven different officials are named as the right recipient.
4. **Your two hypotheses held up, with nuance.** Booking from the train's originating station does improve coupe odds according to experienced travellers, but no official rule says so. The letter helps, but it is neither required nor a guarantee.
5. **Online pet booking on IRCTC is live but unreliable.** It opens only after the first chart. About a dozen people across roughly seven threads report "No Dog/Cat booking allowed" errors even with a coupe allotted, so people fall back to the parcel office.
6. **Domestic flights work only for small pets.** Only Air India, Akasa, Fly91 and Alliance Air take pets in the cabin, with limits of 7–10 kg including the carrier. IndiGo and Air India Express take no pets. Owners of medium and large dogs have almost no good option.
7. **International moves are long, expensive and poorly explained.** Export needs a sequence of at least about four months for the UK and EU, and about two months for the US (microchip, vaccine, titre test at a lab abroad, waiting period, AQCS certificate). Agencies charge ₹1.5–5 lakh. There is no approved titre lab in India for the US or EU.
8. **Import into India has its own traps.** The AQCS advance NOC is now online (ATITHI 2.0), but the visitor versus transfer-of-residence distinction, the DGFT licence and the two-pet cap confuse people.
9. **No Indian product covers the whole job.** Agencies sell the knowledge as a service. Content sites and creators give generic tips. The closest global analogue (PadsPass, US) has no India coverage.
10. **The emotional stakes are high.** People describe panic, crying at counters, rehoming pets, and cutting trips home to once or twice a year. Two widely shared deaths (a Labrador in a brake van, a Chow Chow in air cargo) shape how people see the risk.

---

## 2. Method and limits

**What we did**

- Five parallel research tracks using Firecrawl search and scrape: rail community, air community, creator content, official rules, and competitors.
- About 26 rail threads read in full (23 Reddit, 3 Quora), dated December 2019 to September 2026, most from November 2024 onward.
- About 110 sources for air travel (Reddit, Quora, Team-BHP, news, airline pages).
- Official rules read on primary sources where possible: IRCTC, zonal railway sites, airline pages, AQCS, CDC, GOV.UK, EU, CFIA, KSRTC.

**Limits you should know about**

- **Instagram could not be scraped.** Firecrawl refuses instagram.com. All Instagram findings come from search snippets, which show likes, comment counts, dates and caption fragments, but not full comment threads or follower counts.
- **Reddit was read two ways.** Rail threads were read in full through Reddit mirror front-ends. Air threads could only be read as search-result excerpts, so some quotes are partial.
- **Frequency counts are rough.** They count distinct threads or sources that raise an issue, not a statistical sample.
- **Quora content is mostly stale** (2016–2021) or bot-written, and was weighted lower.
- Raw notes with a URL and date for every claim are in `research-notes/SOURCES.md`.

---

## 3. Who travels with pets, and why

| Persona | Situation | Main mode | What they struggle with |
|---|---|---|---|
| **The relocator** | Moving cities for work with a medium or large dog (15–35 kg) | Train (1AC), road, or pet transporter | Too heavy for any cabin; fear of cargo and dog box; coupe uncertainty |
| **The festival homecomer** | Goes home 1–3 times a year, often October–December | Train or flight | Peak-season demand; repeating the whole process for the return leg |
| **The cat parent** | Cats are about 40% of Akasa's pet passengers | Flight (if under limit) or train | Rail content is almost all about dogs; kitten-basket rule is unknown or refused at stations; a 7 kg cat is too heavy for Fly91 |
| **The solo traveller** | Travelling alone, often a woman | Train | Must book two 1AC berths and put a relative's or the pet's name on the second; TTE may reallot the empty berth |
| **The emigrant** | Moving to the US, Canada, UK, EU, UAE or Australia | International flight (often cargo) | Timeline of two to four months or more; titre lab abroad; agency costs; some countries not reachable directly |
| **The returning NRI or visitor** | Bringing a pet into India | International flight | AQCS NOC, DGFT licence for visitors, two-pet cap, fear of quarantine |

**Emotional drivers seen repeatedly**

- The pet is family: "I am more anxious about his move than mine."
- Fear of harm: cargo deaths and brake-van heat are widely known.
- Powerlessness: "at the mercy of the TTE", "begging at the counter".
- Time pressure: job start dates, a pet's surgery, the March 2026 Gulf evacuation.
- Giving up: driving instead, paying a transporter, or rehoming.

---

## 4. Pain points, ranked

Counts are the number of distinct threads or sources raising each issue.

### 4.1 Rail (26 threads)

1. **Coupe uncertainty until charting (~20).**
   - "There is no process to guarantee. You put your preference, write a mail… and pray like crazy."
   - "Till the morning of the journey we did not know whether we will be able to travel."
   - Cancelling at that point loses most of the fare.
2. **Not knowing where, to whom, and when to submit the coupe letter (~17).**
   - "I found 3 reservation offices in Delhi… I worry if I submit to wrong office."
   - "If the DRM is in a different city from boarding station, how is it even possible…?"
3. **Dependence on the TTE and co-passengers; fines and bribes (~10–11).**
   - Reported demands range from ₹500 to a ₹12,000 "fine".
   - "TCs create unnecessary trouble to get a bribe of 500 or so."
4. **Solo travellers and the two-ticket problem (~10).** The second berth needs a name, and a TTE may cancel or reallot it.
5. **Contradictory information between stations (~5).** Kitten rules, pets per PNR, certificate validity, chart timing. One parcel office said no pet ticket was needed; the TTE then threatened a fine.
6. **Dog box conditions (~9).**
   - "The filthiest thing you will see… smells like a gutter."
   - A Labrador died on a 30-hour summer run in the brake van (June 2026, 1.5k upvotes).
   - Nobody can check online which trains still have a dog box.
7. **Did everything right and still failed (~5).** A further ~8 threads describe giving up on the train altogether.
   - "Went to the DRM office, wrote a letter with all certificates attached, and still didn't get a coupe."
   - "I ended up begging at the counter and signing a responsibility letter… cut down my trips home to just once or twice a year."
8. **Online pet booking failures (~7).** "No Dog/Cat booking Allowed" or invalid-amount errors; emailed PDF not arriving.
9. **Toilet breaks and litter on 16–32 hour journeys (~6).** People want to know which halts are long enough.
10. **Co-passenger hostility (~4).** "Dogs in coupes? How revolting." Threats to complain on RailMadad.

### 4.2 Domestic flights

1. **Outdated and contradictory rules (~18 sources).** Cabin limits quoted as 5, 7, 10 and 11 kg. The current limit on Air India and Akasa is 10 kg including the carrier.
2. **Cost surprises.** Cabin fees are about ₹5,000–7,500 per sector; hold about ₹15,000–16,000.
3. **Cabin slot scarcity (~9).** One or two pets per flight, first come, first served. "If someone's pet already got processed before us, we won't be able to travel." One owner offered a stranger a free ticket to carry a second pet.
4. **Fear of cargo (~9).** "A lot of pets have passed away while travelling in cargos."
5. **Airport-day friction (~7).** Separate windows for documents, weighing and payment; arriving 3–5 hours early.
6. **Staff and pilot discretion (~6).** One family was denied boarding after receiving boarding passes (Air India, 2022).
7. **Sedation confusion (~5).** Vets on Reddit recommend gabapentin; airlines refuse sedated pets.
8. **Carrier size (~5).** People cannot find a soft carrier that meets Air India's 17×10×9 inch limit.
9. **Breed bans (~4).** Snub-nosed breeds are banned from the hold on Air India and Akasa.
10. **Booking by phone or ticket.** Akasa books cabin pets only through its call centre (hold pets of 10–32 kg at the airport counter); Fly91 only through a support ticket.

### 4.3 International

1. **Import paperwork (~17).** "If you are visiting then you need DGFT Pet Import License as well as NOC from AQCS… airline won't board."
2. **Cost shock (~15).** "Heartbroken over unexpected costs to bring my dog home… $1,785."
3. **Needing an agency but not trusting one (~11).** "These pet relo companies rip you off majorly" versus "DO NOT USE AN AGENT."
4. **Titre test timelines and labs abroad (~8).** No CDC-approved or EU-designated lab in India. Samples go to Dubai, the UK or the USA. The UK and EU require a three-month wait after the sample.
5. **Countries that cannot be reached directly.** Australia and New Zealand require the pet to live in an approved third country first (about 180 days for Australia). "I'm being forced to rehome my cats."
6. **Transit traps.** Transiting the EU can trigger titre requirements. Air India flies no pets to or from the USA, Canada or Australia.
7. **Multi-pet households.** India allows two pets per passenger under baggage rules; some agents say two per family.

**The most-engaged import post found:** "Bringing our dog to India was harder than moving ourselves" (Toronto to Bangalore, about June 2026, 144 upvotes, 86 comments).

### 4.4 Questions that come up again and again

**Rail**

- How do I make sure I get a coupe?
- Where, to whom, and how many days before do I submit the letter?
- My train starts elsewhere or I board mid-route. Where does the letter go?
- I'm alone. Do I need two tickets, and whose name goes on the second?
- If I get a 4-berth cabin instead of a coupe, can the TTE deboard me?
- Can I take a kitten or puppy in 3AC, 2AC, sleeper, or Vande Bharat?
- How many pets per PNR?
- Which documents, and how recent must the fitness certificate be?
- Why does online booking say "not allowed"?
- Which trains have a dog box? Which halts are long enough for a toilet break?
- Do I repeat everything for the return journey?

**Air**

- Which airlines take pets, and is it cabin or cargo?
- Will my carrier fit? What if my dog is over 10 kg?
- How do I book, how early, and is there a slot on my flight?
- Can I sedate my pet?
- Am I charged twice on a connecting flight?

**International**

- Do I need a DGFT licence or just the NOC? What is the difference between the advance NOC and the final NOC?
- Which titre lab, and how long is the wait?
- Which agency should I use, and is this quote fair?

A frequent r/indianrailways contributor is writing a wiki page because "lots of people ask", and at least one flight question was cross-posted to three subreddits. Both signal that people cannot find one reliable answer.

---

## 5. Your hypotheses, tested

### "Coupe allotment is more likely if the train starts from your city."

**Mostly supported by practitioners, not by any official rule.**

- About eight experienced travellers say this directly:
  - "I've always booked from the originating station… a coupe may or may not be there from your boarding station."
  - "Book the train end to end… always gotten me a coupe."
  - "Only if you can submit your application at the origin station." (7–8 trips, coupe every time)
- The likely reasons: berth quotas for intermediate stations may not include a coupe, and the letter has to reach the office that manages the chart, usually at the origin.
- Counter-examples exist. A Pune boarder hand-delivered the letter in Mumbai (the origin) and got a coupe. A former senior train manager says mid-route 4-berth bookings are handled by the coach conductor.
- The official text (IRCTC) only says requests go to the DRM or GM office and "due consideration is given".

**For the product:** present this as a strategy with evidence ("travellers report better odds"), not as a rule. Collect outcome data to measure it.

### "You have to send a letter to railway authorities for a coupe."

**Misleading as stated.**

- The letter improves the odds. It is not mandatory, and it guarantees nothing.
- Some people got coupes with no letter. Two women on one PNR and couples often get one by default.
- Others sent letters and still failed, as recently as June 2026.
- Who receives it varies by zone. Named recipients include the Chief Reservation Officer, Chief Reservation Supervisor, Senior DCM, CCM, DRM office, station master and zonal HQ.
- Email requests are generally not accepted or answered. People hand-deliver or use drop boxes.
- Known drop points shared online: Delhi (Baroda House, cabin 13, two boxes by travel day), Bengaluru (DRM office near Majestic), Secunderabad (Rail Nilayam).

**For the product:** the letter generator is useful, but the bigger value is telling people exactly where and when to submit it for their train, and what to do if it fails.

### Other common beliefs the research contradicts

| Belief | What the evidence shows |
|---|---|
| "Go to the parcel office 3 hours early." | Outdated where online booking works (since about late 2024). Still needed for the dog box, Vande Bharat pet box, and when the online system fails. |
| "Pets are only allowed in 1AC." | Kittens and puppies in a basket are allowed in any class under IRCTC terms. Stations enforce this inconsistently. |
| "Getting a coupe means you're guaranteed." | Only 2–4 coupes exist per 1AC coach (one in a composite coach). VIP and other requests compete. |
| "Only Air India allows pets in the cabin." | Akasa, Fly91 and Alliance Air also do. |
| "India quarantines imported pets for weeks." | Now a brief inspection on arrival; 15 days' quarantine only if the pet is found unfit. |

---

## 6. Official rules fact base (as of 25 September 2026)

Tags: **[P]** read on an official page; **[S]** secondary sources only; **[C]** conflicting or unverified.

### 6.1 Indian Railways

**Dogs with the passenger**

- Allowed only in AC First (1A) or First Class, and only when a whole 2-berth coupe or 4-berth cabin is allotted to one PNR. [P: IRCTC terms; Southern Railway release 426/2022-23]
- Not allowed in 2A, 3A, chair car, sleeper or second class. [P]
- If co-passengers object, the dog can be moved to the guard's van with no refund. [P]
- One dog per PNR according to Southern Railway; the IRCTC screen lets you choose a number. [C]

**Coupe requests**

- "The request for allotment of cabin/coupe… can be made in the divisional railway manager office or General Manager office. Due consideration is given to such requests." [P: IRCTC]
- Coupes are assigned at charting. [S]

**Booking the pet**

- **Online:** IRCTC → Trains → "Dogs/Cats Booking" (parcel.indianrail.gov.in/LTBook). Opens after the first chart and closes at the final chart. Works for e-tickets and counter tickets. The system checks that a coupe or cabin is allotted to the PNR. [P]
- **Offline:** parcel office at least 3 hours before departure, after coupe confirmation. [P]
- **Refunds:** "No refund of the freight charges… upon PNR cancellation, and even in case of the train cancellation/late running." [P]
- **Chart timing (decides when you learn about the coupe):** the rule has changed twice since mid-2025.
  - Until July 2025: about 4 hours before departure.
  - From 10 July 2025: 8 hours before departure; 9 pm the previous evening for trains leaving 5 am–2 pm. [P: Central Railway release]
  - From about December 2025 (current): at least 10 hours before departure, counted from the charting station; 8 pm the previous evening for trains leaving 5 am–2 pm. [S: Business Standard, 18 Dec 2025, and other news citing the railway announcement; VERIFY against a Railway Board circular]
  - Sources: https://cr.indianrailways.gov.in/view_detail.jsp?lang=0&id=0,4,268&dcd=9856&did=1751953668715F08FEB7D1A698517544F5A29DD31BC5D and https://www.business-standard.com/india-news/indian-railways-rules-2025-new-chart-updates-for-waiting-list-and-racs-nc-125121800905_1.html

**Documents**

- Vet fitness certificate issued 24–48 hours before the journey, stating the dog has no infectious disease. [P]
- Vaccination card. [P]
- Confirmed ticket and photo ID. [P]

**Charges**

- Charged at the luggage rate (Scale L) on a notional weight: 60 kg for a dog with the passenger, 30 kg in the dog box, 20 kg for a small animal in a basket. Minimum ₹30, plus 2% development charge and 5% GST. [P: Coaching Tariff notes]
- Reported actual payments: roughly ₹160–800 depending on distance. [S]
- Penalty for an unbooked dog: six times the luggage rate. The minimum is quoted as ₹30 or ₹50 depending on the source. [C]

**Cats, kittens and puppies**

- Kittens and puppies in a basket: any class, with the usual luggage charge (Station Master permission under the tariff notes). [P]
- Adult cats: treat like dogs (1A coupe or cabin), or in a cage in the brake van. Some tariff notes allow small animals in non-AC classes with Station Master permission and co-passenger consent. [C]

**Brake-van dog box**

- Any class of ticket. Book at the parcel office at least 3 hours before. Hand the guard's foil to the guard at the origin. Owner feeds and waters the dog. [P]
- Exists in brake vans of older ICF coaches; newer LHB trains may not have one. [P for ICF; C for LHB]
- Collar and chain required; sources disagree on a muzzle. [C]

**Vande Bharat sleeper pet box**

- Reported by news in May–June 2026: boxes at both ends near the locomotive, limited routes, offline booking only at the parcel office, one pet per PNR. No Railway Board circular found. The widely quoted "₹30 per kg" appears to be wrong. [S]

### 6.2 Domestic airlines

| Airline | Cabin | Hold / cargo | Fee (per sector) | Booking | Notes |
|---|---|---|---|---|---|
| **Air India** | Dogs and cats, ≤10 kg incl. soft carrier 17×10×9 in; Economy only; max 2 per flight | 10–32 kg as checked baggage (aircraft-dependent); >32 kg cargo | ₹7,500 cabin; ₹16,000 hold | Support portal or counter, ≥48 h ahead | Health certificate within 7 days; indemnity form; min age 8 weeks (cabin), 3 months (hold); no snub-nosed breeds in hold; last-5-row aisle seats [P] |
| **Akasa Air** | ≤10 kg incl. container 19×12.6×10 in; 2 per flight, 1 per passenger | 1 per flight up to 32 kg; cargo up to 100 kg | ₹7,500 cabin; ₹15,000 hold (incl. taxes) | Cabin: call centre only (+91 96061 12131); hold 10–32 kg: airport counter; ≥24 h ahead | Muzzle during flight; window seat; non-stop only; certificate "within 72 hours" vs "valid 15 days" [C]; min age 3 months [P] |
| **Fly91** | ≤7 kg incl. container 17×10×8 in; 1 per flight including service dogs | No | ₹5,000 | Support ticket, ≥24 h ahead | Muzzle; window seat; small regional network [P] |
| **Alliance Air** | Dogs, cats and birds; pet ≤5 kg, ≤8 kg with container; kennel ≤18×18×12 in; 2 per flight | No | Twice the excess-baggage rate | Not stated | Last row; can buy an extra seat [P, FAQ dated Aug 2025] |
| **SpiceJet** | No | Domestic cargo via cargocare@spicejet.com | Not published | Email | Guide dogs exempt [P] |
| **IndiGo** | No (service dogs only) | No [S] | — | — | [P for service dogs] |
| **Air India Express** | No | No | — | — | Service animals by exception [P] |
| **Star Air** | No | No | — | — | Animals listed as unacceptable baggage [P] |

- DGCA has no pet-specific rule; each airline decides. [C]
- Air India's current policy ("Paws on Board") replaced older limits in late 2025. Posts citing 5 or 7 kg are out of date.

### 6.3 International

**Export from India (AQCS)** [P]

- Present the pet in person at an AQCS station, by appointment, about 7 days before departure.
- Stations: Delhi, Mumbai, Chennai, Kolkata, Bengaluru, Hyderabad. The pet must fly from the airport named on the certificate.
- Documents: local vet fitness certificate, microchip certificate, vaccination record, owner's passport and ticket, airway bill (if cargo), two 4×6 inch photos, and any destination requirements (import permit, titre result).
- Export certificate valid for 10 days. Fee ₹500 via Bharatkosh.
- No online export portal found.

**Import into India (AQCS)** [P]

- Advance NOC applied for online on ATITHI 2.0 at least 7 days before departure; issued within one working day. Airlines must refuse the pet without it.
- On arrival, AQCS inspects the pet and issues the final NOC. An unfit pet goes into 15 days' quarantine at the owner's cost.
- Up to two dogs or cats under baggage rules, which require two years' residence abroad and proof of transfer of residence. Otherwise a DGFT licence is needed, or the pet must qualify as a re-import.
- Rabies vaccine given more than one month and within 12 months before departure. Microchip certificate required. Owner's name must match across certificate and ticket.
- Fee ₹1,000 via Bharatkosh. No titre test needed.

**Destination rules (summary)**

| Destination | Key requirements for a pet from India | Direct from India? |
|---|---|---|
| **USA** | India is high-risk for rabies. Microchip before vaccine; CDC foreign-vaccination form endorsed by a government vet; titre from a CDC-approved lab (none in India); reservation at a CDC-registered facility; min age 6 months; CDC Dog Import Form [P] | Yes, but Air India won't carry pets to the US |
| **Canada** | Personal pets: rabies certificate [S]. Commercial and rescue dogs from high-risk countries banned since 2022 [P] | Yes; few airlines take dogs |
| **UK** | India is "unlisted": titre at an approved lab, then a 3-month wait; GB health certificate; tapeworm treatment for dogs; cargo only on approved routes [P] | Yes |
| **EU** | Titre at a designated lab, 3-month wait, EU health certificate within 10 days, designated entry point, max 5 pets [P] | Yes |
| **UAE** | MOCCAE import permit; titre for high-risk countries; min age 15 weeks [S] | Yes; Air India allows no cabin pets India→UAE |
| **Singapore** | Titre ≥28 days after vaccine; at least 30 days' government quarantine [P, India's category inferred] | Yes |
| **Australia** | India is not an approved country. Pet must live about 180 days in an approved country first [P, snippet] | No |
| **New Zealand** | New import standard from 1 July 2026, mandatory from 1 April 2027. India not eligible; about 6 months in an approved country first [P/S] | No |

**Titre labs:** no CDC-approved and no EU-designated lab in India. The nearest listed lab is CVRL in Dubai. [P]

### 6.4 How often these rules change

| Rule set | Observed change rate | Suggested check |
|---|---|---|
| Airline pet fees | 1–2 times a year; Fly91 files a tariff monthly | Monthly |
| Airline limits and which airlines accept pets | About once a year since 2022 | Quarterly |
| Air India route bans | Occasional | Quarterly |
| IRCTC online booking, Vande Bharat pet box | New in 2024–2026 | Quarterly, plus news watch |
| Railway luggage rates | With each fare revision | At every fare revision |
| AQCS process | Recently moved online | Quarterly |
| CDC rules and lab lists | Updated in 2026 | Monthly |
| New Zealand transition | Milestones in Oct 2026 and Apr 2027 | Monthly until Apr 2027 |
| UK, EU, Singapore lists | Periodic | Quarterly to half-yearly |

---

## 7. What creators say, and what they get wrong

**Most active accounts and reach (single-post engagement, from search snippets)**

| Account | Platform | Topic | Top post seen |
|---|---|---|---|
| @milotheshihtzu._ | Instagram | Rail step-by-step | 332K likes, 881 comments (Jan 2026) |
| @iamshivshakti | Instagram | Flight how-to | 214K likes (Apr 2024, now outdated) |
| @golden_pabloescobark | Instagram | Rail "new booking process" | 144K likes |
| @goofy.timtim | Instagram | Rail series | 99K likes |
| @khan.isa | Instagram | Moving abroad from India | 71K likes (2023) |
| @travellingtailz | Instagram | IRCTC online pet booking | 36K likes, 389 comments (Aug 2026) |
| @iambunnuu | Instagram | Flights (28 flights) | 29K likes (Apr 2024, now outdated) |
| @chaosinacoupe (Divya Dugar, author of *Chaos in a Coupe*) | Instagram | Rail and relocation; 85 train trips | 21K likes |
| thePack | YouTube | Rail and flight explainers | 44.5K views |
| MommyNFlurry Tale | YouTube | Coupe letter template | 23K views |
| dogwithblog.in | Blog | Most accurate Indian guide found | — |

**Tips that are repeated and wrong or outdated**

- Air India cabin limit of 5 or 7 kg (now 10 kg).
- "Only Air India allows pets in the cabin."
- "Always go to the parcel office 3–5 hours early" (not needed where online booking works).
- Rail fare figures ranging from "₹30/kg" to "₹100–1,000".
- One YouTube description inverts the age rule ("your pet should be pregnant or less than 3 months old").

**Things almost nobody mentions**

- Pet freight on trains is non-refundable, even if the train is cancelled.
- Kittens and puppies in a basket can go in any class.
- What to do if the coupe isn't allotted.
- Which trains have 1AC coupes or a dog box, and which halts are long enough.
- Dogs over 10 kg on domestic flights.
- The full international sequence, and DGFT versus NOC.

**Formats that perform:** numbered step-by-step reels, multi-part series, "mistakes I made" framing, DM keyword funnels ("comment TRAIN"), policy-news reposts, and downloadable letter templates. Creators say they are "getting too many DMs about the letter".

---

## 8. Competitive landscape

| Player | Type | What it does well | What it lacks |
|---|---|---|---|
| **Carry My Pet** | Relocation agency | Covers rail, air, road, international; publishes price ranges (rail ₹10–20k, domestic air ₹30–60k, international ₹2–5 lakh); strong SEO blog | No self-serve tools; quotes by form or phone |
| **Petfly (Delhi)** | Agency | In-house vet clinic; airport access; claims 20,000+ pets moved | Delhi only; international focus; opaque pricing |
| **AirPets India** | Agency (since 2006) | Six metro offices; IATA and IPATA credentials | Quote-led only |
| **Global Pet Relocation** | Agency (since 2000) | Many reviews; kennels; titre handling | Sells through phone, WhatsApp, Facebook |
| **PAWsome.in** | Agency | International only | Per-route quote forms; no published pricing |
| **MoveMyPet** | Road-first handler platform | Published prices; ₹499 / ₹1,499 platform fee; escrow; GPS vans | Small; not a rules or documents tool |
| **PetFriendlyPlaces.in** | Discovery directory | 6,500+ pet-friendly listings in 71 cities; YouTube reach | Rules content stale; no trip planning or documents |
| **Uber Pet, Rapido, Namma Yatri** | Local rides | Rapido reports ~2,000 pet rides a day in Bengaluru | Local leg only |
| **Supertails, HUFT** | Pet retail | IATA carriers, quick delivery, generic guides | Not trip-specific |
| **PadsPass (US)** | Trip-planning app | Reads vet records, builds per-trip requirement lists, generates certificates, US$99/year | No India coverage |
| **PetTravel.com** | Global content | Rules for 220+ countries and 150+ airlines | Not built for India or Indian rail |

**Informal competitors**

- Pet transporters who "handled all of the paperwork and arranging coupe" for a fee.
- Instagram and WhatsApp operators, some of them scams (Carry My Pet runs a scam-warning page).

**Where no one is:**

- A single, current, dated rules source across rail, each airline and destination countries.
- A per-trip plan and checklist.
- Document drafting.
- A fallback plan when things go wrong.
- A neutral comparison of DIY versus agency, and train versus air versus road.

---

## 9. Market signals

| Signal | Figure | Source | Confidence |
|---|---|---|---|
| Pets in Indian households | 32 million (2024), up from 26 million (2019) | Redseer via BBC, Mar 2025 | Good |
| Pet-care spend | US$3.6 bn (2024), about US$7 bn by 2028 | Redseer, Oct 2024 | Good (other estimates vary widely) |
| Air India pets flown | 7,000+ in 2024 | Air India newsroom, Mar 2025 | Good |
| Akasa pets flown | 10,000+ since Nov 2022; 60% dogs, 40% cats; 93% in cabin; 26% flew more than once; October–December is ~35% of annual pet trips; top route Bengaluru–Delhi | Akasa, Feb 2026 | Good |
| India pet travel services | US$83 m (2024) to US$155 m (2030) | Grand View Research teaser | Weak (method unknown) |
| Pet-friendly stays | Agoda India listings up 87% year on year | Skift, Oct 2025 | Moderate |

**Interpretation (our inference, not measured)**

- Domestic airline pet volume is small, probably 10,000–15,000 a year. Train and road trips are likely larger.
- International moves are probably a few thousand a year, but each is worth ₹1.5–5 lakh to agencies.
- Seasonality is strong: October–December (festivals, winter) is the peak.
- Repeat travel is real: a quarter of Akasa's pet flyers flew again.

---

## 10. What this means for the product

1. **Trust through dated sources.** Every rule should show its source and "last verified" date. The main failure in the market is stale advice.
2. **Plan for failure, not only the happy path.** The strongest unmet need is "what do I do if the coupe isn't allotted, the ticket is waitlisted, or the counter refuses?"
3. **Make the informal knowledge structured.** Submission offices, drop points, coupe outcomes by train and station, dog-box availability and parcel-office behaviour. Collect these from users after each trip. Over time this becomes data no one else has.
4. **Personalise by pet and route.** Weight, species, breed, age, number of pets, and solo versus group travel decide which options are even possible.
5. **Generate the paperwork.** Coupe request letters, vet certificate formats for the vet to complete, AQCS and DGFT cover letters, and a printable rule sheet to show a TTE or parcel clerk.
6. **Work backwards from the travel date.** Most failures are timing failures: certificates too old, titre waits too short, booking windows missed.
7. **Do not compete with agencies on international moves at first.** Guide DIY users well, and refer complex cases (Australia, US/CDC, large dogs in cargo) to vetted agencies.
8. **Meet people where they already look.** Shareable checklists sized for Instagram and WhatsApp, and Hinglish versions. Partner with credible creators.
9. **Include cats and solo travellers on purpose.** Both are underserved.

---

## 11. Open questions to verify before launch

1. How many pets per PNR on trains (one per Southern Railway release; the IRCTC screen allows more)?
2. Penalty for an unbooked dog (minimum ₹30 or ₹50).
3. Muzzle versus collar and chain on trains.
4. Whether LHB-coach trains have brake-van dog boxes, train by train.
5. The actual Scale L rate table, for a fare estimator.
6. Whether documents are checked for online pet bookings.
7. Akasa certificate validity (72 hours versus 15 days).
8. Vande Bharat sleeper pet box: official circular, routes and charges.
9. Which office handles coupe requests in each railway division, and whether any accept email.
10. SpiceJet domestic pet cargo: weights, fees and current availability.
11. Australia and New Zealand eligibility under their new standards.
12. A reliable, licensable source for train schedules, coach composition and halts (no public official API exists).
13. The official circular for the December 2025 charting change (10 hours; 8 pm the previous evening for morning trains). It is reported by several news outlets, but we did not read the circular itself.

---

## Key sources

- IRCTC pet booking terms: https://contents.irctc.co.in/en/PetBookingInTrains.pdf
- Southern Railway release 426/2022-23 on pet dogs: https://sr.indianrailways.gov.in/view_detail.jsp?lang=0&id=0,4,268&dcd=12678&did=16673909645879C87C2781BF933529C087FF60815E236
- NWR parcel rules (Coaching Tariff notes): https://nwr.indianrailways.gov.in/uploads/files/1675832656225-English%20parcel%20rules.pdf
- Central Railway Coaching Theory: https://cr.indianrailways.gov.in/cris//uploads/files/1553600779866-Coaching%20Theory%20English.pdf
- Air India pets: https://www.airindia.com/in/en/travel-information/travelling-with-pets.html
- Akasa pets: https://www.akasaair.com/information/pets-on-akasa
- Fly91 tariff sheet: https://fly91.in/resources/tariff-sheet.pdf
- Alliance Air FAQ: https://plone.allianceair.in/allianceair/en/assets/faqs/faqs-25-8-2025.pdf
- SpiceJet FAQ: https://corporate.spicejet.com/generalairtravelfaq.aspx
- AQCS import: https://aqcsindia.gov.in/Home/ImportPetUnderBaggage
- AQCS export: https://aqcsindia.gov.in/Home/ExportPets
- AQCS import SOP: https://aqcsindia.gov.in/pdfs/india-cats-guidance.pdf
- CDC high-risk country dog import: https://www.cdc.gov/importation/dogs/foreign-vaccinated-high-risk-countries.html
- Akasa pet travel data (press release, Feb 2026) and Air India newsroom (Mar 2025): see SOURCES.md
- Representative Reddit threads (rail):
  - https://www.reddit.com/r/indianrailways/comments/1tyble4/
  - https://www.reddit.com/r/indianrailways/comments/1pxedd3/
  - https://www.reddit.com/r/indianrailways/comments/1f43ogi/
  - https://www.reddit.com/r/indianrailways/comments/1gm8bhh/
  - https://www.reddit.com/r/indianrailways/comments/1ttjmyc/
  - https://www.reddit.com/r/IndianPets/comments/1u98jhi/

Full source notes, with a URL and date for every claim: `research-notes/SOURCES.md`.
