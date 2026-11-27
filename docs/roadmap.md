# Roadmap: from cold outreach to local customers

Sellhawk is for brick-and-mortar businesses in the United States and Canada: restaurants, salons, dental and medical practices, gyms, auto shops, home services and shops. Its job is to bring people through the door and bring them back. It keeps the engine it started from (local-first, runs on your Claude or ChatGPT plan, deterministic where it can be, nothing sent without your approval) and points it at consumers, with consent checked in code and the rules of the place each customer lives built in.

The research behind these choices is in [research/local-customers.md](research/local-customers.md).

## What Sellhawk does

Two jobs: find new customers, and keep the ones you have coming back. A third piece shows which messages worked.

### Find new customers

| | What it does | Who it reaches | Why it is allowed |
|---|---|---|---|
| **Answer the neighbourhood** | Finds people asking "can anyone recommend a ..." near you and drafts a public reply that says who is answering | People choosing a business right now | You post it yourself; no direct messages to strangers |
| **Partner businesses** | Finds nearby businesses and employers that can send customers your way (cross-promotion, staff perks, shared events) and writes to them | Other businesses, and through them their customers and staff | Business-to-business email, within each country's rules |
| **Get reviews** | Asks every customer after a visit, never only the happy ones, never with a reward, and drafts a reply to every review, so you show up higher in Google Maps, where people look for a local business | Asks your own customers; reaches people searching nearby | Google's review rules, and the same basis as any message to a customer |
| **Sign-ups** | A QR code in the shop opens a text that says JOIN (OUI also works in Quebec), and the messages your website form already emails you arrive as leads | People who chose to hear from you | A JOIN text is their own yes; a form message is an inquiry, which allows email for 6 months |

### Keep them coming back

| | What it does | Who it reaches | Why it is allowed |
|---|---|---|---|
| **Win-back** | Writes when a customer is overdue by their own visit rhythm | Your own customers | They bought from you, or said yes, and every message carries who you are and a way out |
| **Birthdays and seasonal news** | A note on a birthday the customer gave you, and news that fits the season (a holiday menu, winter tires) | Your own customers | Same |

### Show what worked

Sellhawk counts return visits after each message against a holdout (a few customers who got nothing), so you see what a message brought in, not only who opened it. It uses your own data and shows it only to you.

### Why not cold-message consumers

The engine Sellhawk started from finds a company, finds who decides and emails them. It cannot do that for people:

- **Canada:** CASL requires consent before any marketing email or text.
- **United States:** cold email is legal under CAN-SPAM, but people don't publish their addresses the way businesses do, so the only sources are bought or scraped lists. Those draw spam complaints above the 0.3% limit in [Gmail's sender guidelines](https://support.google.com/a/answer/81126), which gets the mailbox blocked, and with it the mail your real customers expect.
- **Texts, in both countries:** a marketing text needs consent first. In the US each text sent without it can cost $500 in damages, or up to $1,500 if willful ([47 U.S.C. 227](https://www.law.cornell.edu/uscode/text/47/227)).

So the original engine now finds partner businesses, where cold email is still allowed, and new customers come through the four routes above.

## The rule under all of it

No marketing email or text goes to a person without a stored, unexpired basis for that channel:

- **Their yes** (express consent): who agreed, when, how, with evidence. The only basis for texts, in both countries.
- **Existing customer**: a recorded visit or purchase. Email only, expires 24 months after the last visit (Canada's limit under CASL, applied in the US too).
- **Inquiry**: they contacted you first. Email only, expires 6 months later (also CASL's limit).

An opt-out on any channel stops every channel at once. Cold email to consumers is refused in both countries. The check runs at the moment of sending, has no off switch, and the agent cannot record a yes.

## The United States and Canada

You set where your business is (country, and state or province) once, in setup. Sellhawk then works out where each customer is: from their address when you have it, else from their phone's area code (every North American area code belongs to one state or province), else your own location, since most of your customers live near you. When it cannot be sure, it applies the stricter of the possible rules.

| | United States | Canada |
|---|---|---|
| Law | CAN-SPAM (email), TCPA (texts), state laws | CASL (email and texts), provincial privacy and language laws |
| Email to your customers | Allowed with an opt-out; Sellhawk still requires a basis | Needs consent: express, or implied (purchase in the last 2 years, inquiry in the last 6 months) |
| Texts | Express written consent | Express consent (Sellhawk does not use implied consent for texts) |
| Every message carries | Business name, postal address, opt-out | Business name, mailing address, and a phone number, email or web address; an unsubscribe that keeps working for 60 days |
| Opt-outs honoured | At once (the law allows 10 business days) | At once (the law allows 10 business days) |
| Text hours, on the customer's clock | 08:00 to 21:00; 08:00 to 20:00 in Florida, Oklahoma and Maryland | Weekdays 09:00 to 21:30, weekends 10:00 to 18:00 (the CRTC telemarketing hours, used as the safe default) |
| Text frequency | At most 3 marketing texts in 24 hours in Florida, Oklahoma and Maryland (Sellhawk counts every marketing text) | No legal cap; your own cap applies |
| Language | English | French for customers in Quebec (bilingual allowed with French at least as prominent) |
| Asking for consent | Clear, written, not a condition of purchase | Must say why, who is asking, how to reach you, and that they can withdraw at any time; never a pre-ticked box |
| Partner outreach (B2B) | Allowed with an opt-out and postal address | Only to an address the business published itself (not a guessed one) with no "no unsolicited mail" note, about their line of work; or one message on a named referral |
| Health businesses | HIPAA: never name a treatment | Provincial health privacy laws (Ontario's PHIPA and others): never name a treatment |
| Texting setup | Register your number for business texting (A2P 10DLC) | Check your provider's Canadian sending rules |

The rules live in one module, as data per country and per state or province, so a new place is a new entry with its own tests. The skills (the playbooks the agent writes from) follow the same split: consent-request scripts for each country, the identification lines each one needs, and French versions of every customer template for Quebec.

## Build order

Each step is one change that leaves the tests passing. High-risk steps (consent, sending, database migrations) get an independent review at the highest effort and your look before they land.

The consent and sending checks come first, because every route to a customer, new or returning, goes through them. Keeping customers comes next: those are people you already have a basis to reach, and the same code then carries every new-customer route.

### Consent and sending, under everything

| # | Step | Risk |
|---|---|---|
| 0 | Rename and debrand to Sellhawk | normal |
| 1 | Remove the outside sequencer integration, the only way an email could leave without the per-message check | high |
| 2 | People get a kind (consumer or business contact, defaulting to consumer) and an indexed phone key | high |
| 3 | The consent ledger gains email, a basis and an expiry | high |
| 4 | Where each person is (country, state or province) and the rules for each place: US federal, the Florida, Oklahoma and Maryland text limits, Canada and Quebec | high |
| 5 | One opt-out stops every channel, in one transaction | high |
| 6 | One egress module: the only code that calls a mail or text provider, with the basis and place rules as its last step | high |

### Keep them coming back

| # | Step | Risk |
|---|---|---|
| 7 | Visits, and customer import from a POS or booking export (CSV) with an attestation you type, worded for your country | high |
| 8 | Lifecycle scheduler: review requests, win-back, birthdays, seasonal, frequency caps and text hours per place | normal |
| 9 | Customer messages in English and French: templates first, the model for wording | normal |
| 10 | Heartbeat priorities: customers before partners | normal |

### Find new customers

| # | Step | Risk |
|---|---|---|
| 11 | Sign-ups: a QR code that opens a JOIN text, recorded as that person's yes with their text as the evidence and answered with a confirmation carrying the wording each country requires; website form messages recorded as inquiries | high |
| 12 | Partner lane: nearby businesses and employers, with Canada's published-address rule | normal |
| 13 | Review replies and neighbourhood listening, with disclosure required in drafts | normal |

### Show what worked, then tidy up

| # | Step | Risk |
|---|---|---|
| 14 | Return-visit attribution on Numbers | normal |
| 15 | Retire the B2B-only parts: LinkedIn automation, job-board hiring signals, startup presets | low |

## Not in the first version

Missed-call text-back and hosted forms need a public address a local tool does not have; sign-ups use the JOIN text and your own website form instead (step 11). Also later: the Google Business Profile API (it needs Google's approval), point-of-sale APIs (CSV first), referral reward tracking, any calling, autopilot for consumer messages, reading Nextdoor or Facebook groups, and countries beyond the US and Canada (until a place has its own rules, Sellhawk sends no marketing there).

## Known risks

- **Imported customer lists** are the main legal exposure. The attestation you type when importing is the evidence, so it is specific and never something the agent can do. In Canada, the burden of proving consent is on the sender.
- **Quebec French.** Whether the French-language rules reach a business outside Quebec writing to a Quebec customer is not settled; Sellhawk writes French for every Quebec customer to be safe.
- **US state laws keep moving.** Texas brought texts under its telemarketing law in 2025 (consent-based texts were later exempted in a settlement). The state table needs a review each quarter.
- **Health businesses**: a message that names a treatment can disclose that someone is a patient.
- **Email unsubscribe**: Sellhawk has no public address for an unsubscribe link, so it uses a `List-Unsubscribe: mailto:` header and a reply-to-stop line, treats either as an opt-out, and keeps reading for them for at least 60 days.

Voice is not on this roadmap: AI-generated calls are "artificial voice" calls under the TCPA, and Canada's telemarketing rules add their own limits.

Not legal advice: check each rule against the law itself, or with a lawyer, before you rely on it.
