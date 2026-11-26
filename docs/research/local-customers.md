# Research: winning local consumers (B2C) for brick-and-mortar businesses

Collected 2026-11-26 to shape Sellhawk. Most marketing figures come from vendors with a commercial interest, so treat them as direction, not proof. The legal notes are a summary of public guidance and law-firm alerts, not legal advice: check the primary rule (or a lawyer) before relying on any of it.

## What works for local businesses

**Reviews are the front door.**

- Google carries about 71% of local review-site use, and consumers check about six review sites on average ([BrightLocal Local Consumer Review Survey](https://www.brightlocal.com/research/local-consumer-review-survey/)).
- 88% of consumers expect a business to answer good and bad reviews; 71% would not use a business averaging under three stars ([BrightLocal data, summarised](https://searchlabdigital.com/blog/data-from-brightlocals-consumer-review-survey/)). 72% say positive reviews make them trust a local business more ([BrightLocal](https://www.brightlocal.com/blog/local-businesses-and-referral-marketing/)).
- Experts rank review signals at roughly 16-20% of local pack ranking weight, and the 2026 ranking factors survey adds Google Business Profile post quality and AI search visibility ([Whitespark](https://whitespark.ca/local-search-ranking-factors/)). This is expert opinion, not a measured study.
- Drafting review replies is the most common use of AI in small-business marketing, and the main complaint is generic, bot-sounding replies ([Localogy](https://www.localogy.com/2026/04/an-smb-ai-killer-app-emerges-responding-to-reviews/embed/)).
- Replying through Google's Business Profile API needs an application and approval, granted per Cloud project ([Google](https://developers.google.com/my-business/content/faq)). A local tool should draft replies the owner posts, and connect to the API later.

**Your own customers are the cheapest growth.**

- Win-back sequences reactivate roughly 10-30% of lapsed customers in e-commerce roundups ([Opensend](https://opensend.com/post/win-back-campaign-success-rate-statistics)); no solid restaurant or salon figure was found.
- Define "lapsed" per customer, from their own visit rhythm (time since the last visit past a multiple of their usual gap), not one fixed cutoff, and measure against a holdout group, because some lapsed customers come back unprompted ([Emarsys](https://emarsys.com/learn/blog/how-to-win-back-lost-customers-proven-strategies-to-reduce-churn/)).
- 85% of 7,500 North American small-business owners named word-of-mouth referrals the best way to win local customers ([Entrepreneur, on an Alignable survey](https://entrepreneur.com/growing-a-business/the-majority-of-small-business-owners-rely-on-word-of-mouth/302229)).
- Texts are opened about twice as often as email (about 51% vs 24% in one 2025 vendor sample) ([NP Digital](https://neilpatel.com/marketing-stats/email-vs-text-message-marketing/)), but texting needs stronger consent (below).

**Where the market leaves room.**

- Podium, Birdeye and Thryv cover review requests, review monitoring, texting and missed-call capture. Reviewers praise consolidation and complain about setup and measuring results ([G2](https://www.g2.com/compare/podium-vs-thryv), [Capterra](https://www.capterra.com/compare/156926-164285/Thryv-vs-Podium)). Thryv's marketing plan starts around $244 a month.
- Gaps an open-source, local-first agent can fill: tying each message to a return visit, win-back driven by real visit history, and one low-cost place for reviews, campaigns, listening and partnerships.

## The rules Sellhawk is built around

| Behaviour | US | Canada | EU | UK |
|---|---|---|---|---|
| Cold email to a scraped or bought consumer address | Legal under CAN-SPAM with an opt-out, but Sellhawk refuses it | Needs consent (CASL) | Needs consent (ePrivacy) | Needs consent (PECR) |
| Cold text to a consumer | Needs prior express written consent (TCPA) | Needs consent | Needs consent | Needs consent |
| Text to a customer who opted in | Allowed: honour STOP, avoid 21:00-08:00, register 10DLC | Allowed: sender id and unsubscribe | Allowed with consent | Allowed with consent |
| Email to your own past customer | Allowed with an opt-out | Implied consent for 2 years after a purchase, 6 months after an inquiry | Soft opt-in: own customers, similar products, opt-out offered at collection and in every message | Same soft opt-in |
| Public reply to "can anyone recommend..." | Allowed: say who you are, follow the group's rules | Same | Same | Same |
| Message to a stranger who posted publicly | Platform rules forbid most of it | Likely needs consent | Needs consent | Needs consent |
| Review request to every customer | Allowed, no incentive | Same | Same | Same |
| Review request only to happy customers | Breaks Google's policy (review gating) | Same | Same | Same |

Sources: [FTC CAN-SPAM guide](https://www.ftc.gov/business-guidance/resources/CAN-SPAM-act-compliance-guide-business), [the vacated one-to-one consent rule](https://www.mofo.com/resources/insights/250130-eleventh-circuit-vacates-fcc-s-tcpa-one-to-one-consent-rule), [TCPA quiet hours litigation](https://www.nixonpeabody.com/insights/alerts/2026/05/13/a-loud-decision-on-tcpa-quiet-hours), [Florida and Maryland updates](https://www.insideprivacy.com/advertising-marketing/tcpa/maryland-and-florida-update-their-telemarketing-laws/), [Oklahoma](https://www.mofo.com/resources/insights/220607-another-state-imposes-prior-consent-obligations), [CRTC CASL guidance](https://crtc.gc.ca/eng/com500/guide.htm), [ICO email marketing guidance](https://ico.org.uk/for-organisations/direct-marketing-and-privacy-and-electronic-communications/guidance-on-direct-marketing-using-electronic-mail/), [EDPB scraping guidelines](https://www.edpb.europa.eu/system/files/2026-07/edpb_guidelines_2020603_webscraping_v1_en_0.pdf), [Google review incentives](https://www.applausehq.com/blog/googles-rules-for-incentivizing-reviews), [review gating](https://www.podium.com/online-reviews/review-gating/), [Nextdoor on self-promotion](https://blog.nextdoor.com/self-promotion-update).

### Canada and US states, in more detail

- **CASL** has three parts: consent, identification, and an unsubscribe mechanism. Implied consent only covers the listed cases, and an existing business relationship counts only inside its time window ([CRTC FAQ](https://crtc.gc.ca/eng/com500/faq500.htm), [CRTC implied consent guidance](https://crtc.gc.ca/eng/com500/guide.htm)). An address someone published counts only if it was not published with a note refusing unsolicited messages, and those messages still need identification and an unsubscribe.
- **Florida, Oklahoma and Maryland** limit marketing texts to 08:00-20:00 on the recipient's clock and to three in 24 hours (Florida and Maryland per subject, Oklahoma for all marketing). Florida applies by area code; Maryland by area code or where the person is reasonably presumed to live ([Klaviyo](https://help.klaviyo.com/hc/en-us/articles/44447515845019-Understanding-State-Laws-Limiting-Frequency-of-Marketing-Text-Messages), [Davis Wright Tremaine](https://dwt.com/blogs/broadband-advisor/2023/05/telemarketing-tcpa-maryland-florida)).
- **Texas** brought marketing texts under its telemarketing registration law on 1 September 2025 ([Morgan Lewis](https://www.morganlewis.com/pubs/2025/09/texas-telephone-solicitation-law-now-covers-text-messages)). Businesses texting only current or former customers under the same name for two years are exempt, and a November 2025 settlement exempted consent-based marketing texts ([Barnes and Thornburg](https://btlaw.com/en/insights/alerts/2025/texas-exempts-consent-based-text-marketing-from-sb-140), [National Law Review](https://natlawreview.com/article/texas-sb-140-after-november-settlement-consent-cold-texting-out)).
- **Quebec** (the Charter of the French Language as amended by Bill 96) requires French in commercial documents, and commentators treat websites and social media as such; no source addressed email or text directly ([Osler](https://osler.com/en/insights/updates/bill-96-understanding-quebec-s-french-language-law/?pdf=1), [CFIB](https://www.cfib-fcei.ca/en/site/qc-law-14-bill-96)). Sellhawk writes French for Quebec customers as the safe reading.

Open questions to check against primary sources before relying on them: whether the TCPA quiet hours apply to consented marketing texts (courts are split), current Florida and Oklahoma text, each EU country's soft opt-in, and Meta's rules for Facebook groups.

## What this means for Sellhawk

1. **Consent first, for every consumer, on every channel.** A marketing email or text to a person goes out only with a stored, unexpired basis (their yes, their purchase, or their inquiry), and every one carries who you are and a way out. This is one check in code at send time, like the existing text consent check.
2. **Reviews:** ask every customer after a visit, never only the happy ones, never with a reward, and draft a reply to every review in the owner's voice, with complaints handed to the owner.
3. **Win-back** from each customer's own visit rhythm, measured against a holdout.
4. **Neighbourhood listening:** find people asking for a recommendation, draft a public reply that says who is answering, and let the owner post it. No direct messages to strangers.
5. **Local partnerships:** the original business-to-business engine, pointed at nearby businesses and employers that can send customers your way (cross-promotion, staff perks, shared events). Cold email to a business stays legal where cold email to a consumer is not.
6. **Measure return visits**, the gap reviewers of the big tools keep naming.
