# App Store & Google Play Reviews Scraper

Collect **iOS App Store and Google Play reviews** for as many apps and countries as you need in a single run. Every review comes back in one unified format: platform, rating, title, text, author, date, app version, helpful votes and (on Google Play) the developer's reply. Export CSV, JSON or Excel, or call it from code or an AI agent.

Use it for competitor research, churn and complaint analysis, release-quality tracking, ASO (app store optimisation), or feeding reviews to an LLM for summaries.

## What you get

| Field | Description |
|---|---|
| `platform`, `appId`, `appName`, `country` | Which app and store the review is from |
| `rating` | 1-5 stars |
| `title` | Review title (App Store only) |
| `text`, `author`, `date` | Review text, reviewer name, ISO date |
| `appVersion` | App version the reviewer used |
| `helpfulCount` | Helpful votes |
| `developerReply` | Developer's reply (Google Play only) |
| `reviewId`, `url` | Review id and store page |

```json
{
  "platform": "android",
  "appId": "com.example.app",
  "appName": "Example App",
  "country": "us",
  "rating": 2,
  "text": "Crashes every time I open settings.",
  "date": "2026-10-13T14:04:14.850Z",
  "appVersion": "9.1.76",
  "helpfulCount": 4,
  "developerReply": "Thanks for the report, we're on it."
}
```

(Illustrative values.)

## Input

| Field | Default | Notes |
|---|---|---|
| `apps` | required | App Store ids/URLs and Google Play packages/URLs, mixed freely |
| `countries` | `["us"]` | Two-letter store codes. Each app is read once per country |
| `maxReviewsPerApp` | `100` | Per app and country (matching reviews) |
| `sortBy` | `newest` | `newest` or `helpful` |
| `minRating`, `maxRating` | `1`, `5` | Keep only a star range, e.g. `1`-`2` for complaints |

```json
{ "apps": ["324684580", "com.spotify.music"], "countries": ["us", "gb"], "maxReviewsPerApp": 200, "maxRating": 2 }
```

## Pricing (pay per event)

| Event | Price |
|---|---|
| Actor start | $0.00005 per run |
| Review scraped | $0.0005 per review |

That is **$0.50 per 1,000 reviews**. Example: 5 apps x 2 countries x 100 reviews = 1,000 reviews, about $0.50. Set a maximum cost per run in the Apify console; the run stops cleanly when it is reached.

## Limits

- **App Store:** Apple's public reviews feed returns up to about 500 reviews per app and country (50 per page, 10 pages), newest or most helpful first. It does not provide older history.
- **Google Play:** many more reviews per app are available, but Google may rate-limit very large runs; the Actor retries automatically.
- Reviews are public store content in the reviewer's language; the Actor does not translate them.
- Respect each store's terms and applicable privacy law when using reviewer names.

## FAQ

**Can I scrape reviews from several countries at once?** Yes. List country codes in `countries`; each app is read once per country.

**How do I find an app's id?** App Store: the number after `id` in the app's URL. Google Play: the `id=` value in the URL, such as `com.spotify.music`.

**Can I get only negative reviews?** Set `maxRating` to 2 (or 3).

**Something broke?** Open an issue from the Issues tab with the run URL and your input.


## Run it

This repository documents the hosted Actor. Run it from the [Apify Store](https://apify.com/neon_innovation_lab/app-store-play-store-reviews-scraper), or from code:

```python
from apify_client import ApifyClient
client = ApifyClient("<APIFY_TOKEN>")
run = client.actor("neon_innovation_lab/app-store-play-store-reviews-scraper").call(run_input={})  # add the input fields from the table above
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

Built by [Neon Innovation Lab](https://neoninnovationlab.com/actors).
