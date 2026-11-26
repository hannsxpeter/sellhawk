# Sellhawk

<p align="center"><strong>The open-source local marketing agent for brick-and-mortar businesses in the US and Canada, on your Claude or ChatGPT plan.</strong></p>

<p align="center">
Sellhawk helps a restaurant, salon, clinic, gym or shop bring customers back, collect reviews, answer neighbours asking for a recommendation, and build partnerships with businesses nearby.
It runs on your own machine, checks consent in code before anything is sent, and by default nothing goes out until you approve it.
</p>

> **Status: early (0.1).** Sellhawk still runs the engine it started from: it finds local businesses, writes to the person who decides, sends from your own mailbox and handles the replies. The consumer side (customer import, consent per channel, review requests, win-back, neighbourhood listening, the US and Canada rules) is being built in the order on the [roadmap](docs/roadmap.md), consent checks first. The research behind it is in [docs/research/local-customers.md](docs/research/local-customers.md).

## Inside your agent

Sellhawk lives where you already work, in Claude Code, Claude desktop and Codex. Run `sellhawk connect` once (or press **Connect** in the dashboard), then ask your agent **"What needs me?"** or **"What did you do overnight?"**. It answers with a card in the chat: what Sellhawk did, what waits for your OK, how each campaign is doing. One click opens the full app beside the chat, already signed in.

Your agent can read everything, draft, plan and explain. Approving, sending, buying and resuming are always your clicks, in the app. Your agent gets its own key with limited reach, never your dashboard token, and Set up, Your agent logs every call it makes and has a switch that cuts it off. More: [Sellhawk inside your agent app](docs/agent-app.md).

## The app

Today, Outbound, Calendar and Numbers each open with one sentence on where things stand. A dock at the bottom holds six places: **Today** (what your agent did and what only you can decide), **Outbound** (drafts waiting for you, replies, campaigns, finding businesses, signals, social), **Inbound** (leads from your site's forms, texts, mentions), **People** (and companies), **Calendar** (week and month, with meetings and scheduled sends) and **Numbers** (charts over time and a found-to-meeting funnel), plus **Set up**. It fits a phone and the narrow side panel of an agent app the same way.

<sub>The demo data is invented: "Bookwell" selling online booking to roofers, dental practices and HVAC shops, on `.example` and `.test` domains.</sub>

## What it does

Sellhawk is a pipeline you can run end to end, or stop at any step.

| | What happens | More |
|---|---|---|
| **Find companies** | Searches for businesses that match your customer: free on OpenStreetMap, or on a paid source you pick. Every paid search shows its cost before it runs. | [Discovery](docs/signals-and-pipeline.md#1-discover-who-exists) |
| **Learn why they'd buy** | Reads each company's own website for the buying signals you confirmed, like no online booking, ads running or a role they're hiring for. No model calls, no cost. | [Signals](docs/signals-and-pipeline.md) |
| **Find who decides** | Finds the owner or manager named on the company's own pages, two people per company at most. | [Enrich](docs/signals-and-pipeline.md#3-enrich-who-decides) |
| **Check the address** | Learns the company's email pattern and verifies one address. Every address is marked verified, risky, guess or invalid, and only deliverable ones are ever sent to. | [Verify](docs/signals-and-pipeline.md#4-verify-can-you-reach-them) |
| **Write and send** | Writes a short sequence for each person from what it found, then sends from your Gmail, Microsoft 365 or SMTP mailbox inside your daily limit and their office hours. | [Sending](docs/approval-and-sending.md) |
| **Handle replies** | Reads every reply, drafts an answer, sets auto-replies aside, honours opt-outs at once, and hands you anything it shouldn't answer alone. | [Dashboard](docs/dashboard.md) |

Only want the list? Stop after verifying and run `sellhawk export` for a sequencer-ready CSV of everything it found.

```
$ sellhawk run

============================================================
Sellhawk is online.
============================================================
Checking pipeline state...
Decision: write_campaign (3 new prospects with no drafts)
  handler   → Elena Park, Harbor Dental Studio     interested, answer drafted
  handler   → Priya Nair, Highline Roofing         objection, answer drafted
  writer    → 2-email sequence: Phoenix HVAC hiring a dispatcher
  outbox    → 5 emails waiting for your approval
Cycle complete. Sleeping for 15 minutes.
```

## No model API bill

Sellhawk thinks through the Claude Code CLI on your Claude Pro or Max plan, or through Codex on a ChatGPT plan. There's no API key and no per-token billing, and Sellhawk throttles itself to leave room for your own work.

| | |
|---|---|
| Claude Pro or Max, or a ChatGPT plan with Codex | the subscription you already have |
| Finding businesses | **$0** on the default source, or about $0.37 per 1,000 on the cheapest paid one |
| Reading their websites | **$0**: three HTTP requests each, no model call |
| Writing and sending | your subscription and a mailbox (about $7 a month for Google Workspace) |
| Prospecting tools | **None.** No Apollo, ZoomInfo, Clearbit or Clay. |

How the loop, the budget and the isolation work: [How Sellhawk works](docs/how-it-works.md).

## Quick start

You need:

- **Python 3.11 or newer.** macOS ships 3.9, so run `brew install python@3.13` first if `python3 --version` says so.
- **Claude Pro or Max** with the CLI signed in (`claude login`), or a **ChatGPT plan with the Codex CLI** ([Codex setup](docs/codex.md)).
- **A mailbox to send from**, ideally Google Workspace on a separate sending domain. [Microsoft 365](docs/microsoft365.md) and SMTP work too.
- **Warmup on that mailbox.** We recommend [TrulyInbox](https://trulyinbox.com/): it warms a new mailbox before cold email and keeps it out of spam after, and Sellhawk reads its inbox rate to tell you when you're ready.
- **An address checker.** Without one, found addresses are guesses and are never sent. We recommend [Reoon](https://www.reoon.com/email-verifier/) for its free tier (600 checks a month), and [ZeroBounce](https://www.zerobounce.net/members/signup) as well if you write to Microsoft 365 or Google Workspace domains.

Finding businesses and people is free: businesses come from OpenStreetMap, people from each company's own site. When that runs thin, we recommend [DataForSEO](https://app.dataforseo.com/register) for more businesses (about $0.37 per 1,000, $1 free to try) and [Hunter](https://hunter.io/users/sign_up) for the right person at each (25 searches a month free). `sellhawk recommended` lists the one service we recommend for each job, and why.

### Let Claude set it up

```bash
git clone https://github.com/hannsxpeter/sellhawk.git
cd sellhawk
claude
```

Then say **"set up Sellhawk for me"**. The repo's `AGENTS.md` turns Claude Code into the setup guide: it checks where you are, installs what's missing, asks what it needs and trains Sellhawk on your product.

### Or do it yourself

```bash
# 1. Install. Use python3.13, not python3: on macOS bare python3 is 3.9.
python3.13 -m venv .venv && source .venv/bin/activate && pip install -e .

# 2. Add credentials. Every variable is documented inline.
cp .env.example .env && $EDITOR .env

# 3. Learn your product from your website (writes sellhawk.local.yaml).
sellhawk train https://your-company.com

# 4. Confirm what makes a good prospect.
sellhawk signals --confirm free

# 5. Find businesses. The default source is free and needs no account.
sellhawk discover

# 6. Open the dashboard and start the loop.
sellhawk dashboard          # prints a private link to http://127.0.0.1:5555
sellhawk run
```

On Linux you can also [run it with Docker](docs/docker.md).

### Connect your agent

```bash
sellhawk connect
```

It finds Claude Code, Claude desktop and Codex on your machine, shows the line it will add to each, and adds them all on one yes. Then ask your agent "Open Sellhawk" or "What needs me?":

- **Claude Code** and **Codex** (the CLI and the app): start a new session.
- **Claude desktop**: quit it completely and open it again. It can open the dashboard in its side panel and show cards in the chat.

Or click **Connect** in the dashboard, under Set up, Your agent: one row per agent, with Connect, Reconnect (when Sellhawk has moved) or Remove. It works on macOS and Windows, including the Microsoft Store build of Claude desktop.

`sellhawk connect claude-desktop` (or `claude`, `codex`) does just one. `sellhawk connect --remove` takes Sellhawk out again. Nothing goes into those configs but the command and Sellhawk's folder: no key, no token.

ChatGPT and claude.ai in a browser connect from their own servers, so they can't reach Sellhawk on your machine. On a ChatGPT plan, use Codex.

## Nothing sends without you

Every outgoing email waits in the Outbox for your approval until you turn on autopilot yourself. Before anything leaves, a pre-send check with no model involved rejects unfilled merge tags, banned phrases, overlong emails, extra links and undeliverable addresses. Autopilot still holds back any email that links to a domain that isn't yours or quotes Sellhawk's own instructions. `sellhawk sending pause` stops everything at once.

Details: [Approval and sending](docs/approval-and-sending.md) and [Legal and deliverability](docs/deliverability.md).

## Where your files live

Your config, knowledge and data live in one home folder. Which one, first match wins:

1. `SELLHAWK_HOME`, if you set it (the Docker image sets it to `/app`).
2. A checkout: the checkout folder itself, as before.
3. Otherwise, for an installed package (`pip` or `pipx`), your user data folder:

| | Home folder |
|---|---|
| macOS | `~/Library/Application Support/sellhawk` |
| Windows | `%APPDATA%\sellhawk` |
| Linux | `~/.local/share/sellhawk` (or `$XDG_DATA_HOME/sellhawk`) |

Inside it are `sellhawk.yaml` (and `sellhawk.local.yaml` once you train), `.env`,
`prompts/` and `skills/`, all yours to edit. An installed package copies the defaults in on
first run and never overwrites them, so your edits survive upgrades. `data/` holds the
database, the logs and the dashboard token.

## Documentation

| Guide | What's in it |
|---|---|
| [How Sellhawk works](docs/how-it-works.md) | The heartbeat loop, the subscription budget, project structure and the ideas behind it |
| [Signals and the prospecting pipeline](docs/signals-and-pipeline.md) | The 29 signals, cohorts, and the discover, profile, enrich and verify stages |
| [Skills and sub-agents](docs/skills-and-agents.md) | The editable sales playbooks and which agent uses which |
| [Approval and sending](docs/approval-and-sending.md) | The Outbox, autopilot, office hours and email signatures |
| [The dashboard and the data](docs/dashboard.md) | Every screen, how sign-in works, and how the data is stored |
| [Configuration and commands](docs/configuration.md) | `.env`, `sellhawk.yaml` and every command |
| [Extensions](docs/extensions.md) | Adding routes, heartbeat steps and commands from a separate package |
| [Using a ChatGPT plan (Codex)](docs/codex.md) | Running on Codex instead of Claude |
| [Sellhawk inside your agent app](docs/agent-app.md) | Connecting your agent, cards in the chat, and the dashboard in its side panel |
| [Inbound leads](docs/inbound-leads.md) · [Texting](docs/texting.md) · [Mentions](docs/mentions.md) · [Social](docs/social.md) · [Events](docs/events.md) | Leads from your forms, consent-first texts, brand mentions, social playbooks and webhooks |
| [Legal and deliverability](docs/deliverability.md) | The rules you're sending under, and warmup |
| [Microsoft 365](docs/microsoft365.md) · [Precleared](docs/precleared.md) | Provider setup guides |
| [Docker](docs/docker.md) · [Troubleshooting](docs/troubleshooting.md) · [Roadmap](docs/roadmap.md) | Running, fixing and what's next |

## Contributing

Pull requests are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) first: it covers keeping real contact data out of the repo, test email, and signing off your commits. Found a security problem? Follow [SECURITY.md](SECURITY.md) instead of opening an issue.

See [CHANGELOG.md](CHANGELOG.md) for what changed in each release.

## License

Sellhawk is licensed under the [GNU Affero General Public License v3.0](LICENSE).

<sub>Sellhawk began as a fork of [Sellbird](https://github.com/ethanplusai/sellbird) by Ethan Rogers. Its notices are kept in [NOTICE](NOTICE).</sub>
