# ChiroTouch API integration and MCP server: connect ChiroTouch to ChatGPT, Claude and any AI agent

**ChiroTouch API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your ChiroTouch account.** ChiroTouch has no official MCP server and no ChatGPT or Claude app; its API is for partners only (checked 26 Nov 2026). Cotlet.dev connects your own signed-in ChiroTouch account to any AI agent, so you ask in plain words and the AI helps you book requests, chase claims and reactivate patients.

## What you can ask

> Book these new patient requests from my website form into ChiroTouch and tell me the confirmed times.

> List insurance claims unpaid over 30 days by payer, with the last status of each.

> Find patients seen in the last 6 months with no future visit and draft a reactivation text for each. Send after my OK.

> Who missed or cancelled an appointment this week and has not rebooked?

> Every evening, send me today's visits, collections and new patients.

## What chiropractic clinic owners and front desks ask their AI to do in ChiroTouch

From chiropractic clinic owners and front desks' posts on Reddit, forums, review sites and the ChiroTouch help center (checked 26 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Online bookings into ChiroTouch | "Book these new patient requests from my website form into ChiroTouch and tell me the confirmed times." | Your own endpoint, called from a sheet or n8n |
| Unpaid insurance claims | "List insurance claims unpaid over 30 days by payer, with the last status of each." | Weekly |
| Reactivate lapsed patients | "Find patients seen in the last 6 months with no future visit and draft a reactivation text for each. Send after my OK." | Weekly |
| No-shows and cancellations | "Who missed or cancelled an appointment this week and has not rebooked?" | Daily |
| Daily visits and collections | "Every evening, send me today's visits, collections and new patients." | Daily |
| Patient list with insurance in bulk | "Download all active patients with their insurance and last visit into a sheet." | Your own endpoint, called from a sheet or n8n |
| Patient ledger for an attorney or insurer | "Print the full transaction history for this patient as a PDF for the attorney." | When you ask |

Your AI asks you before it sends, books, replies or changes anything. Payments stay in your hands.

## ChiroTouch API

ChiroTouch has no public self-serve API; integrations go through the ChiroTouch Marketplace and HL7 interfaces set up per customer (checked 26 Nov 2026). With Cotlet.dev you build your own ChiroTouch API: show your AI agent a job once in your signed-in ChiroTouch account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/chirotouch

Endpoints you can build in a few minutes (none of them exists until you build it):

- `unpaid-claims`: Insurance claims unpaid over a number of days, by payer
- `patients-export`: Active patients with insurance and last visit
- `lapsed-patients`: Patients seen recently with no future visit
- `no-shows-this-week`: Patients who missed or cancelled and have not rebooked
- `daily-summary`: Today's visits, collections and new patients

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/unpaid-claims`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its ChiroTouch tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"chirotouch":{"command":"uvx","args":["chirotouch-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t chirotouch-mcp .
docker run -i --rm -e COTLET_API_KEY chirotouch-mcp
```

Tools: `chirotouch_guide`, `chirotouch_status`, `chirotouch_open`, `chirotouch_read`, `chirotouch_click`, `chirotouch_type`, `chirotouch_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install chirotouch-mcp        # or run it once: uvx chirotouch-mcp connect
chirotouch-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 chirotouch_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 chirotouch_agent.py status    # is ChiroTouch ready in your Cotlet.dev account
python3 chirotouch_agent.py guide     # ChiroTouch screens and sign-in link
python3 chirotouch_agent.py read      # open ChiroTouch, read the page, close the browser
python3 chirotouch_agent.py tools     # list Cotlet.dev tools
python3 chirotouch_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from ChiroTouch users

**Does ChiroTouch have an API?**

ChiroTouch has no public self-serve API; integrations go through the ChiroTouch Marketplace and HL7 interfaces set up per customer (checked 26 Nov 2026). With Cotlet.dev you build your own ChiroTouch API on your signed-in account and call it from n8n, Make or your own code: https://cotlet.dev/integrations/api/chirotouch

**How do I connect ChiroTouch to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to ChiroTouch once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no ChiroTouch API key.

**Is there a way to use Claude with ChiroTouch?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own ChiroTouch account.

**Is there a ChiroTouch MCP server?**

ChiroTouch has no official MCP server for your account. Cotlet.dev is a hosted MCP server that works in your own ChiroTouch account for you, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**ChiroTouch has no public API. How do I get bookings from my website or GoHighLevel into it?**

Build your own endpoint with Cotlet.dev. Your form or GoHighLevel calls it, and your AI books the visit in ChiroTouch Cloud on the same screen your front desk uses.

**Does this work with ChiroTouch Server?**

It works with ChiroTouch Cloud, which runs in the browser. The older server edition is installed on your office computers, so the Cloud Browser cannot reach it.

## Find it on

- [ChiroTouch on Cotlet.dev](https://cotlet.dev/integrations/mcp/chirotouch?utm_source=github&utm_medium=readme&utm_campaign=chirotouch&utm_content=find)
- [PyPI package chirotouch-mcp](https://pypi.org/project/chirotouch-mcp/)
- [Odel](https://odel.app/modules/cotlet/chirotouch)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/chirotouch/)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [Alto MCP](https://github.com/cotlet-connect/alto-mcp)
- [Rightmove Plus MCP](https://github.com/cotlet-connect/rightmove-plus-mcp)
- [TurboTenant MCP](https://github.com/cotlet-connect/turbotenant-mcp)
- [Innago MCP](https://github.com/cotlet-connect/innago-mcp)
- [Property Finder MCP](https://github.com/cotlet-connect/property-finder-mcp)
- [LoopNet MCP](https://github.com/cotlet-connect/loopnet-mcp)
- [Dotloop MCP](https://github.com/cotlet-connect/dotloop-mcp)
- [SkySlope MCP](https://github.com/cotlet-connect/skyslope-mcp)
- [ShowingTime MCP](https://github.com/cotlet-connect/showingtime-mcp)
- [connectMLS MCP](https://github.com/cotlet-connect/connectmls-mcp)
- [WebPT MCP](https://github.com/cotlet-connect/webpt-mcp)
- [Tebra MCP](https://github.com/cotlet-connect/tebra-mcp)
- [ChiroFusion MCP](https://github.com/cotlet-connect/chirofusion-mcp)
- [Semble MCP](https://github.com/cotlet-connect/semble-mcp)
- [Nookal MCP](https://github.com/cotlet-connect/nookal-mcp)
- [Cliniko MCP](https://github.com/cotlet-connect/cliniko-mcp)
- [Gentu MCP](https://github.com/cotlet-connect/gentu-mcp)
- [splose MCP](https://github.com/cotlet-connect/splose-mcp)
- [Physitrack MCP](https://github.com/cotlet-connect/physitrack-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/chirotouch?utm_source=github&utm_medium=readme&utm_campaign=chirotouch

Start free: https://cotlet.dev/signup?app=chirotouch&utm_source=github&utm_medium=readme&utm_campaign=chirotouch
