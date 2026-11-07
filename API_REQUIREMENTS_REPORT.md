# API Requirements Report

Generated from `config.py`, the same registry that drives the dashboard's **API Integrations** page.
Prices and free tiers change often: confirm on the provider's page before relying on them (figures checked November 2026).

| # | Provider | Mandatory | Env variables | Free tier | Card needed | Cost |
|---|---|---|---|---|---|---|
| 1 | App security (admin login) | **Yes** | `ADMIN_PASSWORD`, `SECRET_KEY`, `CRON_SECRET` | Not applicable | No | Free |
| 2 | Supabase (PostgreSQL) | **Yes** | `DATABASE_URL` | Yes (free projects; may pause after a period of inactivity) | No for the free plan | Free; paid plans from about $25/month |
| 3 | Anthropic Claude API | Optional | `ANTHROPIC_API_KEY`, `ANTHROPIC_MODEL` | No ongoing free API tier (prepaid credits) | Yes (to buy credits) | Pay per token; estimate a few US cents per fully processed lead (depends on model) |
| 4 | Serper (Google search results API) | Optional | `SERPER_API_KEY` | Yes: about 2,500 free queries on signup | No | Free credits, then roughly $1 per 1,000 queries on bundles |
| 5 | Tavily Search API (alternative) | Optional | `TAVILY_API_KEY` | Yes: about 1,000 credits per month | No | Free tier, then pay-as-you-go |
| 6 | Brave Search API (alternative) | Optional | `BRAVE_SEARCH_API_KEY` | $5/month credit (about 1,000 queries); attribution required | Yes (required even for the free credit) | About $5 per 1,000 requests after credit |
| 7 | Hunter.io | Optional | `HUNTER_API_KEY` | Yes: 50 credits/month incl. API | No | Paid from about $49/month (2,000 credits) |
| 8 | Snov.io | Optional | `SNOV_CLIENT_ID`, `SNOV_CLIENT_SECRET` | 50 credits/month on the free plan; API access on free plans is reported inconsistently, check your account | No for free plan | Paid from about $39/month |
| 9 | ZeroBounce (email verification) | Optional | `ZEROBOUNCE_API_KEY` | Small monthly free allowance (verify current amount) | No for the free allowance | Pay-as-you-go credit packs |
| 10 | Google Gmail API (OAuth) | Optional | `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `GOOGLE_REDIRECT_URI` | Yes | No (no billing needed for Gmail API) | Free (Gmail daily sending limits apply) |
| 11 | Google PageSpeed Insights API | Optional | `PAGESPEED_API_KEY` | Yes | No | Free (quota limits) |
| 12 | LinkedIn (manual-assist mode) | Optional | none | Not applicable | No | Free |
| 13 | Hacker News + RSS feeds (built in) | Optional | none | Yes | No | Free |

## Details

### App security (admin login)

- **What it does:** Protects the dashboard and signs session cookies. CRON_SECRET authenticates Vercel Cron.
- **Why the agent needs it:** Without it anyone who finds your URL could read leads or trigger sends.
- **Mandatory:** Yes
- **Free tier:** Not applicable
- **Credit card required:** No
- **Registration URL:** n/a
- **Environment variables:** `ADMIN_PASSWORD`, `SECRET_KEY` (optional: `CRON_SECRET`)
- **Estimated cost:** Free
- **Setup:** Generate long random values: python -c "import secrets;print(secrets.token_urlsafe(32))"

### Supabase (PostgreSQL)

- **What it does:** Persistent database for leads, contacts, messages, jobs and suppression lists.
- **Why the agent needs it:** Vercel functions have no persistent disk, so every record must live in an external database.
- **Mandatory:** Yes
- **Free tier:** Yes (free projects; may pause after a period of inactivity)
- **Credit card required:** No for the free plan
- **Registration URL:** https://supabase.com/dashboard
- **Environment variables:** `DATABASE_URL`
- **Estimated cost:** Free; paid plans from about $25/month
- **Setup:** Project Settings > Database > Connection string > use the pooled (Supavisor) URI.

### Anthropic Claude API

- **What it does:** Semantic intent classification, company research summaries, personalised message writing, reply analysis.
- **Why the agent needs it:** Without it the app falls back to keyword rules and templates, clearly labelled 'heuristic'/'template'.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** No ongoing free API tier (prepaid credits)
- **Credit card required:** Yes (to buy credits)
- **Registration URL:** https://console.anthropic.com/
- **Environment variables:** `ANTHROPIC_API_KEY` (optional: `ANTHROPIC_MODEL`)
- **Estimated cost:** Pay per token; estimate a few US cents per fully processed lead (depends on model)
- **Setup:** Console > API Keys > Create key.

### Serper (Google search results API)

- **What it does:** Web discovery: finds public posts/pages where people ask for services, plus company websites and public LinkedIn profile snippets.
- **Why the agent needs it:** Main discovery engine. Without any search API only Hacker News and your RSS feeds are searched.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** Yes: about 2,500 free queries on signup
- **Credit card required:** No
- **Registration URL:** https://serper.dev/
- **Environment variables:** `SERPER_API_KEY`
- **Estimated cost:** Free credits, then roughly $1 per 1,000 queries on bundles
- **Setup:** Sign up > API key.

### Tavily Search API (alternative)

- **What it does:** Alternative search provider.
- **Why the agent needs it:** Use instead of / alongside Serper.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** Yes: about 1,000 credits per month
- **Credit card required:** No
- **Registration URL:** https://app.tavily.com/
- **Environment variables:** `TAVILY_API_KEY`
- **Estimated cost:** Free tier, then pay-as-you-go
- **Setup:** Dashboard > API key.

### Brave Search API (alternative)

- **What it does:** Alternative search provider.
- **Why the agent needs it:** Independent index.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** $5/month credit (about 1,000 queries); attribution required
- **Credit card required:** Yes (required even for the free credit)
- **Registration URL:** https://api-dashboard.search.brave.com/
- **Environment variables:** `BRAVE_SEARCH_API_KEY`
- **Estimated cost:** About $5 per 1,000 requests after credit
- **Setup:** Subscribe to the Search plan, then create a key.

### Hunter.io

- **What it does:** Finds business emails for a domain and verifies addresses.
- **Why the agent needs it:** Primary provider for emails that are not published on the company's own site.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** Yes: 50 credits/month incl. API
- **Credit card required:** No
- **Registration URL:** https://hunter.io/users/sign_up
- **Environment variables:** `HUNTER_API_KEY`
- **Estimated cost:** Paid from about $49/month (2,000 credits)
- **Setup:** Dashboard > API.

### Snov.io

- **What it does:** Alternative email finder and verifier (v2 async API).
- **Why the agent needs it:** Second source / second verification opinion.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** 50 credits/month on the free plan; API access on free plans is reported inconsistently, check your account
- **Credit card required:** No for free plan
- **Registration URL:** https://snov.io/
- **Environment variables:** `SNOV_CLIENT_ID`, `SNOV_CLIENT_SECRET`
- **Estimated cost:** Paid from about $39/month
- **Setup:** Account settings > API.

### ZeroBounce (email verification)

- **What it does:** Dedicated email verification including catch-all detection.
- **Why the agent needs it:** Emails are only labelled VERIFIED after a provider says so.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** Small monthly free allowance (verify current amount)
- **Credit card required:** No for the free allowance
- **Registration URL:** https://www.zerobounce.net/
- **Environment variables:** `ZEROBOUNCE_API_KEY`
- **Estimated cost:** Pay-as-you-go credit packs
- **Setup:** API > API keys.

### Google Gmail API (OAuth)

- **What it does:** Save drafts, send approved emails from your own mailbox, read replies and bounces.
- **Why the agent needs it:** Required only for sending and reply management; the app is fully usable for research without it.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** Yes
- **Credit card required:** No (no billing needed for Gmail API)
- **Registration URL:** https://console.cloud.google.com/apis/credentials
- **Environment variables:** `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET` (optional: `GOOGLE_REDIRECT_URI`)
- **Estimated cost:** Free (Gmail daily sending limits apply)
- **Setup:** Create OAuth client (Web). Add redirect URI <your-url>/api/gmail/callback. Then press Connect Gmail in the dashboard.

### Google PageSpeed Insights API

- **What it does:** Real mobile performance score for the website audit.
- **Why the agent needs it:** Without it the audit still checks HTML-level issues but reports no Lighthouse score.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** Yes
- **Credit card required:** No
- **Registration URL:** https://developers.google.com/speed/docs/insights/v5/get-started
- **Environment variables:** `PAGESPEED_API_KEY`
- **Estimated cost:** Free (quota limits)
- **Setup:** Enable PageSpeed Insights API in Google Cloud, create an API key.

### LinkedIn (manual-assist mode)

- **What it does:** Prepared connection notes/messages and profile links. You send them yourself.
- **Why the agent needs it:** LinkedIn offers no general-purpose official prospecting/outreach API; scraping is prohibited, so nothing is automated.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** Not applicable
- **Credit card required:** No
- **Registration URL:** https://www.linkedin.com/
- **Environment variables:** none
- **Estimated cost:** Free
- **Setup:** No keys. Use the Open profile and Copy message buttons.

### Hacker News + RSS feeds (built in)

- **What it does:** Public Hacker News search (Algolia API) and RSS feeds you configure.
- **Why the agent needs it:** Keyless discovery sources.
- **Mandatory:** No (the app works without it; the integration shows NOT CONNECTED)
- **Free tier:** Yes
- **Credit card required:** No
- **Registration URL:** https://hn.algolia.com/api
- **Environment variables:** none
- **Estimated cost:** Free
- **Setup:** Add RSS feed URLs in Settings.

## Accounts you must connect

1. **Supabase** (`DATABASE_URL`) and the three app-security values (`ADMIN_PASSWORD`, `SECRET_KEY`, `CRON_SECRET`). Nothing works without these.

## Features that need paid or external APIs

| Feature | Needs | Without it |
|---|---|---|
| Semantic intent detection, personalised writing, reply understanding | Anthropic API (pay per token) | Keyword rules and templates, labelled `heuristic` / `template` |
| Search-engine discovery, official-website lookup, public LinkedIn profile snippets | One search API (Serper/Tavily free tiers; Brave needs a card) | Only Hacker News + your RSS feeds + manual leads |
| Emails not published on the company site | Hunter and/or Snov (free tiers are small) | Only published addresses are collected |
| `VERIFIED` email status | Hunter, Snov or ZeroBounce | Addresses stay `UNKNOWN` and cannot be emailed under default settings |
| Draft/send/reply tracking | Google OAuth client (free) | Messages stay in the approval queue; copy and send manually |
| Lighthouse mobile score | PageSpeed API key (free) | HTML-level audit only |
| Background automation | Vercel Cron (daily on Hobby) or an external worker (optional) | Manual runs from the dashboard |
