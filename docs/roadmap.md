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
| Text hours, on the customer's clock | 09:00 to 20:00 everywhere, the strictest state hours (Connecticut's start; the Florida, Oklahoma and Maryland end). Federal law alone allows 08:00 to 21:00 | Weekdays 09:00 to 21:30, weekends 10:00 to 18:00 (the CRTC telemarketing hours, used as the safe default) |
| Text frequency | At most 3 marketing texts in 24 hours everywhere, the Florida, Oklahoma and Maryland limit (Sellhawk counts every marketing text) | No legal cap; your own cap applies |
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
| 4 | Where each person is (country, state or province) and the rules for each place: one strict US text profile that state entries can only tighten, Canada and Quebec, each rule dated; health businesses marked in setup | high |
| 5 | One opt-out stops every channel, in one transaction, and reading cannot miss one: from the last confirmed position, a catch-up after downtime, no send without a fresh read, French stop words, `sellhawk retire` | high |
| 6 | One egress module: the only code that sends a person's details anywhere (mail, texts, checkers, webhooks), with the basis, place and content rules (subject lines, treatment words, Quebec French, agreements for health businesses, approval age) as its last step | high |

### Keep them coming back

| # | Step | Risk |
|---|---|---|
| 7 | Visits, and customer import from a POS or booking export (CSV): a date on every row, only the columns needed, an attestation you type worded for your country, the evidence kept, and deletion of people with no basis | high |
| 8 | Lifecycle scheduler: review requests (to every customer, never chosen by how they felt), win-back, birthdays, seasonal, frequency caps, text hours per place, a slow start for imported customers | normal |
| 9 | Customer messages from templates filled in on your machine, so the model never sees a customer; English and French, with fixed identification and unsubscribe lines | high |
| 10 | Heartbeat priorities: customers before partners | normal |

### Find new customers

| # | Step | Risk |
|---|---|---|
| 11 | Sign-ups: a poster Sellhawk prints with a QR code that opens a JOIN text carrying a version code, recorded as that person's yes with their text as the evidence and answered with a confirmation carrying the wording each country requires; website form messages recorded as inquiries; US texts wait for the 10DLC registration | high |
| 12 | Partner lane: nearby businesses and employers, from a sending domain apart from customer mail, with Canada's published-address rule | normal |
| 13 | Review replies (never confirming a patient) and neighbourhood listening, with disclosure required in drafts | normal |

### Show what worked, then tidy up

| # | Step | Risk |
|---|---|---|
| 14 | Return-visit attribution on Numbers | normal |
| 15 | Retire the B2B-only parts: LinkedIn automation, job-board hiring signals, startup presets | low |

## Not in the first version

Missed-call text-back and hosted forms need a public address a local tool does not have; sign-ups use the JOIN text and your own website form instead (step 11). Also later: the Google Business Profile API (it needs Google's approval), point-of-sale APIs (CSV first), referral reward tracking, a check of old phone numbers against the FCC's Reassigned Numbers Database, any calling, autopilot for consumer messages, reading Nextdoor or Facebook groups, and countries beyond the US and Canada (until a place has its own rules, Sellhawk sends no marketing there).

## Risks and what Sellhawk does about them

Each risk names the check that handles it and the build step it lands in. Where something stays with you, it says so. The sources are in [research/local-customers.md](research/local-customers.md#sources-for-the-risks).

### Imported customer lists

The main legal exposure. In Canada the sender has to prove consent, and a bought, rented or scraped list carries none.

- **A date on every row.** Each person's basis comes from their own last visit or purchase, or the day they contacted you, and expires counting from that date, not from the import. A row with no date is kept but gets no basis: nothing is sent to it until they visit again or sign up. (step 7)
- **An import never creates a yes.** It records "existing customer" or "inquiry" only. A column claiming someone agreed to texts is ignored; texts need the person's own yes. (7)
- **An attestation that says something.** You type the source (your booking or POS system), the date range, the number of people (it must match what Sellhawk read) and that the list was not bought, rented or scraped, in wording for your country. The agent cannot type it. Sellhawk keeps it with the time, the wording version and a fingerprint of the file, and refuses to send to anyone whose import record is missing. (6, 7)
- **Only the columns it needs.** Name, email, phone, visit dates and a birthday as month and day. Service, notes and every other column are dropped before anything is stored. (7)
- **An opt-out always wins.** Opt-outs are kept as fingerprints, so a later import brings no one back, even after the rest of that person's data is deleted. An "unsubscribed" or "do not contact" column becomes an opt-out. (5, 7)
- **A stale list stops itself.** Imported customers get their first messages at about 25 a day, and a high bounce or opt-out rate pauses the rest, using the kill switch Sellhawk already has. (8)
- **Ask before consent runs out.** While a customer's implied consent still holds, Sellhawk can ask for their express yes. After it expires it cannot ask by email in Canada: the CRTC treats an email asking for consent as a commercial message itself. (8)

### Quebec French

- **Never English alone.** Quebec customers get French, or French first with English below; a customer who asks for English gets both. When the address and the area code disagree, both places' rules apply, so a possible Quebec customer gets French first. The send step refuses a message to a Quebec or possibly-Quebec customer whose template is not French or French first. (4, 6, 9)
- **French written by a person.** The French templates are written and checked by a Quebec French speaker, in Quebec usage. The identification and unsubscribe lines are fixed text, never model output. A draft for a Quebec customer that does not read as French is held. (9)
- **French opt-out words.** ARRÊT and DÉSABONNER, with or without accents, count like STOP. (5)
- **Signs in the shop.** The QR sign-up poster for a Quebec shop prints in French, with any other language clearly smaller, as the Charter's signage rule requires. (11)
- Whether the language rules reach a business outside Quebec writing to a Quebec customer stays unsettled; Sellhawk applies them anyway.

### US state laws keep moving

- **One strict US profile.** Every US customer gets texts only between 09:00 and 20:00 on their own clock (Connecticut's start, the Florida, Oklahoma and Maryland end) and at most 3 marketing texts in 24 hours. A state entry can only make this stricter. A local business loses little, and a new state law of the same kind is already covered. (4)
- **Edges of the map.** Where an area code spans two time zones (850 in Florida), only hours allowed in both count. A +1 number outside the US and Canada (most of the Caribbean) is refused. (4)
- **Opt-outs are permanent** until the person signs up again, which covers Virginia's rule, in force since January 2026, that a STOP text is honoured for 10 years. (5)
- **Subject lines tell the truth.** Washington's email law covers any false or misleading subject line (Brown v. Old Navy, 2025), with damages per email and a wave of class actions since. Sellhawk holds an email whose subject claims a deadline or scarcity ("last chance", "ends tonight", "only 3 left") unless you set a real end date for the offer. (6)
- **Dated rules.** Each rule carries its source, the date it took effect and the date it was last checked. Set up and `sellhawk status` say when the table is more than 90 days past its last check, and a scheduled job in the repository flags entries past their review date. The quarterly review covers Connecticut, Florida, Maryland, Oklahoma, Texas, Virginia, Washington and the FCC's opt-out rules. (4)

### Health businesses

Dental, medical, chiropractic, physiotherapy, optometry, mental health and pharmacy businesses say so in setup (4). That changes five things:

- **No treatment named, anywhere.** No message names a treatment, service or appointment type, and a list of treatment words holds any draft that slips through. (6, 9)
- **The practice's own services only.** HIPAA treats a message paid for by someone else, such as a product maker, as marketing that needs the patient's written permission, so Sellhawk never sends one. (8)
- **Review replies never confirm a patient.** Dental practices have paid HHS $10,000 (2019) and $23,000 (2022) for replying to Yelp reviews with patient details. A reply never says the reviewer was a patient or mentions their care; it thanks them and invites them to call. (13)
- **An agreement with every service that carries a patient message.** Being on a dentist's list is health information, so the mailbox and the text service need a business associate agreement (BAA). Google Workspace and Microsoft 365 offer one; free gmail.com and outlook.com mailboxes don't, so Sellhawk refuses them for patient mail. Twilio signs one only on its Security and Enterprise editions. Until you confirm you have them, patient messages wait. (6)
- **Patient details never reach the model.** A Claude or ChatGPT subscription comes with no BAA (see the next section, which applies to every business). (9)

In Canada, provincial health privacy laws (Ontario's PHIPA and others) get the same treatment. Washington's My Health My Data Act may reach gyms and med spas; it is on the review list.

### Email unsubscribe without a public address

- **Two ways out in every email.** A `List-Unsubscribe: mailto:` header with a code for each person, so an opt-out sent from a forwarding address still matches, and a visible line: reply STOP, or tap a link that opens an email with "unsubscribe" filled in. CAN-SPAM and CASL both accept an email reply as the way out. Gmail's one-click rule (an HTTPS link) applies to bulk senders, close to 5,000 messages a day to Gmail addresses from one domain, so Sellhawk caps each sending domain well below that. (6)
- **No send without a fresh read.** A marketing message goes out only if the mailbox and Twilio were read in the last 15 minutes and every opt-out found was applied. (5)
- **Reading that cannot miss one.** Sellhawk reads from its last confirmed position in the mailbox, not only unread mail, so an unsubscribe you opened on your phone first still counts. After a gap longer than a reader covers (Sellhawk off for more than a week, or a restored backup), it searches the whole gap for opt-outs before anything sends. Today the SMTP reader takes only unread mail and treats a failed read as an empty inbox; step 5 fixes both. (5)
- **Fixed words first, then caution.** STOP, UNSUBSCRIBE and the other fixed words stop at once; any other reply that reads like an opt-out counts as one until you look. (5)
- **Reading continues for 60 days** after the last message, even with sending paused (CASL).
- **If you stop using Sellhawk,** `sellhawk retire` stops sending, keeps reading opt-outs for 60 days, exports the opt-out list for your next tool, then deletes customer data. Opt-outs also land in your own inbox, whatever happens. (5)

### Customer details reaching outside services

Not on the first list, and the widest risk. Drafting with Claude or Codex sends text to a US service. Quebec's privacy law (Law 25) requires a privacy impact assessment before personal information leaves Quebec, and health information needs a BAA.

- **The model never sees a customer.** It writes a template for a group (regulars who are overdue, this week's birthdays) and never sees a name, address, phone number or visit history; the details are filled in on your machine. The code that calls the model accepts only templates and group descriptions, so a customer record cannot be passed to it by mistake. (9)
- **The other services, too.** Address checkers are skipped for consumers (their addresses came from them, and bounces are handled). The cold-email check (Precleared) is for business contacts only. Webhooks get Sellhawk's own id for a person unless you choose details for that address. A test records every outside request across a customer's whole life in Sellhawk and checks that an email or phone number appears only in the call that sends to them. (6)
- **Kept only while needed.** People with no basis for 12 months are deleted (their opt-outs stay as fingerprints), and the database file is readable only by you. (7)

### Other risks the first list missed

- **Stale approvals.** At the moment of sending, Sellhawk checks the basis, its expiry, the hours and the place again, and an approval older than 7 days goes back to waiting for you. Twilio's own refusal to text a number that sent STOP (error 21610) counts as an opt-out on every channel. (6)
- **Partners and customers sharing one reputation.** Cold email to partner businesses draws more complaints than mail to your own customers, so partner outreach must come from a separate sending domain, and the send step refuses it from the customer-mail domain. A domain must pass the email records check before its first campaign. (6, 12)
- **Review platform rules.** Who gets asked for a review can never depend on how happy they seemed, what they spent or what they replied, and a test proves it. Drafts with a reward or "5 stars" wording are refused. Yelp forbids asking for reviews, so Sellhawk never links to it. Imports don't trigger requests for old visits: only visits in the last 14 days. The holdout that measures results never includes review requests, or "we ask every customer" would be false. The FTC's 2024 rule also bans fake and bought reviews and reviews from staff or family without saying so; Sellhawk never writes or posts a review. (8, 13)
- **Proof of a JOIN.** A yes is only as good as the wording the person saw. Sellhawk prints the poster, and the QR code puts a version code in the text, which ties each yes to its exact wording. US texts wait until Twilio reports the business-texting (10DLC) registration approved. (11)
- **Text opt-outs in any words.** The FCC treats STOP, QUIT, END, REVOKE, OPT OUT, CANCEL and UNSUBSCRIBE as opt-outs, and any other reasonable way of saying it. (5)
- **Reassigned phone numbers.** A number that agreed years ago can belong to someone else now, and the consent does not pass to them. Later: check old numbers against the FCC's Reassigned Numbers Database before texting them.

Voice is not on this roadmap: AI-generated calls are "artificial voice" calls under the TCPA, and Canada's telemarketing rules add their own limits.

Not legal advice: check each rule against the law itself, or with a lawyer, before you rely on it.
