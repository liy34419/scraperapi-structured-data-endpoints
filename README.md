# ScraperAPI Structured Data Endpoints: What Comes Back, How Many Credits Each Request Costs, and Which Plan You Actually Need (Full Endpoint List + Cost Table)

Structured Data Endpoints return parsed JSON for a fixed list of supported sites, and they work on every ScraperAPI plan, including the $49 Hobby tier. The price per request is set by the domain rather than the individual endpoint: Amazon 5 credits, Google 25, LinkedIn 30, an ordinary page 1. On Hobby's 100,000 monthly credits that buys 20,000 Amazon product pages, which is roughly $2.45 per thousand.

Below is the full endpoint list with the fields each one returns, the credit maths including the multipliers that surprise people, a plan-by-plan cost table, and the limits that only show up once you are already running jobs.

> **Quick pick**
> - Amazon or Walmart price monitoring under ~20,000 product pages a month: **Hobby, $49** (100,000 credits, 20 threads, US and EU geotargeting only).
> - SEO or SERP work, or somewhere between 20,000 and 200,000 pages a month: **Startup, $149** (1,000,000 credits, 50 threads).
> - You need proxies outside the US and EU: **Business, $299**, the first tier with 50+ countries.
> - 7-day trial gives you 5,000 credits with no card, which is enough to benchmark real success rates before paying. 👉 [See current plan pricing and start the trial](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## What a structured data endpoint is, and what it saves you

Send a product ID or a search query, get back named JSON fields. No HTML, no selectors, no parser to maintain when the target site redesigns its template. Each endpoint carries the site-specific proxy rules, anti-bot handling and HTML mapping for its domain, and ScraperAPI returns either JSON (default) or CSV.

It is one of six request paths in the product: the standard API, async API, proxy port, structured endpoints (`https://api.scraperapi.com/structured/`), DataPipeline, and some language SDKs. Requests cap at 50MB, and every structured endpoint works through both the sync and async APIs.

Endpoints generate the target URL themselves. If you need to hit a URL you constructed yourself, say a custom Amazon product address, the documented route is Auto Parsing instead.

## All plans side by side

| Plan | Monthly price | Annual (per month) | Credits/month | Geotargeting | Get it |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000 | None | [Free plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | US & EU | [Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | US & EU | [Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 50+ countries | [Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 50+ countries | [Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | — | [Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 | $1,778 | 21,500,000 | — | [Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | Custom | 5M+ | 50+ countries | [Enterprise](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Concurrency scales with the tier: 5 threads on Free, 20 on Hobby, 50 on Startup, 100 on Business, 200 on Scaling and Enterprise. Thread counts for Professional and Advanced are not part of the pricing figures above.

Annual billing is a flat 10% off across the paid tiers. Credits reset at renewal and unused ones do not carry over, which matters if your workload is seasonal. Pay-as-you-go overage is restricted to Scaling and above; on Hobby, Startup and Business you get paused at zero unless you switch on auto-upgrade, which bumps you to the next tier when the allowance runs out.

## The endpoint list, by domain

### Amazon: 5 credits per request, any Amazon endpoint

| Endpoint | Fields returned |
| --- | --- |
| Product API | Title, price, rating, review count, ASIN, availability, images, description, product specifications, seller info |
| Search API | Per-listing title, price, ASIN, rating, sponsored flag |
| Offers API | Third-party seller name, condition, price, shipping |
| Reviews | Star distribution, top positive review, top critical review, review array, pagination URLs |

The product response is deeper than the marketing summary suggests. A real payload for ASIN B09R93MDJX returned a `product_information` block with package dimensions, item weight, battery requirements, a `best_sellers_rank` array listing separate category ranks (`#3,386 in Portable Bluetooth Speakers`, `#19,205 in MP3 & MP4 Player Accessories`), date first available and manufacturer; plus `feature_bullets`, `full_description`, `product_category`, `average_rating`, `total_reviews`, and `customization_options` where every colour and style variant carries its own ASIN. If you are tracking BSR movement or variant-level stock, that structure is the reason to use this endpoint over a raw HTML fetch.

The reviews endpoint returns a rating histogram (the sample product showed 17 five-star, 5 four-star, 5 three-star, 5 two-star and 4 one-star, out of 35 ratings), the top positive and top critical review, and pagination URLs for the rest.

Amazon coverage spans regional marketplaces, from .com through .co.jp and .com.br, reported at 21 marketplaces.

### Google: 25 credits per request

| Endpoint | Fields returned |
| --- | --- |
| Search (SERP) | Organic results, ads, featured snippets, knowledge panel, related searches |
| Shopping | Price, merchant, rating, availability |
| News | Headline, source, date, summary |
| Jobs | Title, company, location, salary, description |
| Maps | Name, address, phone, rating, reviews, hours |

SERP responses also carry shopping panels, People Also Ask and pagination in third-party descriptions of the endpoint, along with country/language targeting and time-range filters.

### Walmart: four endpoints, standard-domain pricing

Product, Search, Category and Reviews scrapers. ScraperAPI's structured data documentation lists these at standard-domain cost (1 credit); some third-party write-ups price Walmart at 5. If Walmart volume is central to your budget, run a handful of calls through the Domain Cost Estimator in the dashboard, which shows the exact credit cost for any URL, before committing to a tier.

### Redfin and other supported domains

Redfin has four endpoints: sale pages, search pages, rental properties, agent details. Beyond those, the catalogue includes eBay product and search, Etsy, YouTube, Reddit, G2, Trustpilot, TechCrunch, Quora, GitHub, Google Play, Apple App Store, LinkedIn job listings (30 credits), Indeed and a general job boards endpoint. Real-estate scrapers also exist for Zillow, Realtor.com, Rightmove, Zoopla, Homes.com, Idealista and LoopNet.

Credit cost follows the domain, not the endpoint: every Amazon endpoint is 5, every Google endpoint is 25, no matter which one you call.

## How the credit maths actually works

Base costs are only half the bill. These request parameters add credits on top:

| Parameter | Extra credits |
| --- | --- |
| `render=true` (JavaScript rendering) | +10 |
| `screenshot=true` | +10 |
| `premium=true` (residential proxy) | +10 |
| `ultra_premium=true` | +30 |
| Anti-bot bypass (Cloudflare, DataDome, PerimeterX) | +10 each, applied automatically |

Stacking is not linear. `premium=true` plus `render=true` costs +25, not +20. `ultra_premium=true` plus `render=true` costs +75, not +40. That second jump is the one that empties accounts.

Two worked examples on the Hobby plan:

- **1,000 Amazon product pages, default settings.** 5 credits each, so 5,000 credits. That is 5% of the monthly allowance, or $2.45.
- **1,000 Amazon product pages with `ultra_premium=true` and `render=true`.** 80 credits each, so 80,000 credits, which is 80% of Hobby's entire month for a thousand pages. In cash terms, about $39.20.

For plain product and SERP work you should not need the add-ons at all: the structured endpoint already handles the target domain's rendering and anti-bot requirements. The multipliers matter when you push a structured endpoint at a target that needs residential IPs or when you enable rendering out of habit rather than need. If you are budgeting a first month, 👉 [check the plan allowances against your page count](https://www.scraperapi.com/pricing/?fp_ref=coupons) before you pick a tier.

## What a month of real work costs

Prices below assume each plan is used to its full allowance at base credit rates.

| Workload | Hobby $49 | Startup $149 | Business $299 | Scaling $475 |
| --- | --- | --- | --- | --- |
| Amazon product pages (5 credits) | 20,000 | 200,000 | 600,000 | 1,000,000 |
| Cost per 1,000 Amazon pages | $2.45 | $0.75 | $0.50 | $0.48 |
| Google SERPs (25 credits) | 4,000 | 40,000 | 120,000 | 200,000 |
| Cost per 1,000 SERPs | $12.25 | $3.73 | $2.49 | $2.38 |

The practical reading: a small price-monitoring operation watching a few thousand ASINs daily fits comfortably in Hobby. An SEO agency pulling tens of thousands of SERPs a month needs Startup, because 25 credits per query burns through 100,000 credits in 4,000 searches. Once you need geotargeting outside the US and EU, Business becomes the floor regardless of volume, since Hobby and Startup only cover those two regions.

A third-party estimate puts Startup as sufficient for most teams running 100,000 to 500,000 effective requests a month against standard ecommerce targets. That range only holds if you stay near base credit costs; add rendering or residential proxies and the same money buys a fraction of the pages.

## Seven things that catch people out

1. **Forced 10-minute caching on hard targets.** Difficult or heavily protected sites return a cached result for up to 10 minutes. For flash sales or live inventory, plan around the staleness rather than discovering it in production.
2. **404s are billed.** A successful request and a not-found both consume credits; only requests that fail outright (timeouts, server errors) are exempt. Batch ASIN lists full of delisted products cost real money.
3. **No proactive credit alerts.** Nothing emails you at 80% or 95% usage. You have to watch the dashboard, or set `max_cost` per scrape to cap what any single request can spend.
4. **Credits expire at renewal.** Unused allowance does not roll over, so an annual plan sized for peak months wastes the difference.
5. **Pay-as-you-go is a high-tier feature.** Only Scaling and above keep running past the limit at overage rates. Below that you stop until the next cycle unless auto-upgrade is enabled.
6. **DataPipeline bills differently.** A basic request through the no-code DataPipeline costs 6 DataPipeline credits rather than the 1 you would pay on the standard API, so the same job priced on API credits will look cheaper than it is.
7. **Concurrency rejections are free but fatal to a deadline.** Exceeding your thread limit returns a 429 rather than queueing the request, and there is no extra charge for the refusal.

## Official claims versus measured results

ScraperAPI's own product material has quoted 99.99% success on supported domains, which is a 2023 marketing figure. Independent benchmarking published in April 2026 measured Amazon at 98%, Etsy at 99%, Zillow at 100% and Walmart at 93% on the structured endpoints. Google SERP was described as reliable on standard queries, with one 2025 benchmark placing ScraperAPI below several competitors, so heavy SERP volume deserves your own test before you commit.

Review scores sit in the same range: Capterra 4.6/5 across 62 reviews (ease of use 4.9/5, its highest sub-score), G2 4.4/5 across 16, Trustpilot 4.5/5 across 43 with 93% five-star. The recurring praise is documentation quality and reliability on Amazon and Google. The recurring complaint is the credit multiplier producing bills people did not expect, plus the US/EU geotargeting cap below Business.

## Who should not buy this

- **Non-technical users wanting a point-and-click tool.** Structured endpoints are API calls with an API key. DataPipeline adds scheduling and webhook delivery, but you are still configuring requests.
- **Anyone whose main targets are social platforms or login-walled sites.** Instagram, Twitter/X and Booking.com are not reliably scrapeable here, and they are not on the supported list.
- **Projects aimed at sites outside the catalogue.** The structured layer only covers the documented domains.

If you fall into one of those groups, the money is better spent elsewhere, and no plan size fixes it. For everyone else, the endpoints remove the parser maintenance that eats most scraping budgets, and the pricing is predictable once you know the multipliers.

## FAQ

**Do structured data endpoints work on the free plan?**
Yes. They are available on every plan including Free, which carries 1,000 credits a month, and the new-account trial adds 5,000 credits over 7 days with no card.

**How many credits is an Amazon request?**
5, whichever Amazon endpoint you call. Google is 25, LinkedIn 30, a standard domain 1.

**Can I get CSV instead of JSON?**
Yes. JSON is the default; CSV is available via the output format parameter.

**Do the endpoints support async calls?**
Yes, both the sync and async APIs return structured output.

**Can I scrape a URL I built myself?**
Not through the structured endpoints, which construct their own URLs. Use Auto Parsing for custom addresses.

**What happens when I run out of credits?**
On Hobby, Startup and Business, scraping pauses until renewal. Scaling and above can continue at pay-as-you-go rates up to a cap you set.

## How to choose, and what to confirm before you pay

Match the plan to the credit cost of your dominant workload rather than to raw page counts. Under 20,000 Amazon pages or 4,000 SERPs a month, Hobby at $49 covers it. Between there and roughly 200,000 Amazon pages or 40,000 SERPs, Startup at $149 is the cheaper per-page option at $0.75 per thousand. Needing non-US/EU proxies forces Business at $299. Needing overage protection forces Scaling at $475.

Three checks before you enter a card: run 50 to 100 requests against your actual targets during the trial and watch the credit consumption rather than trusting the documentation; confirm your required countries are covered by the tier you are buying; and decide whether a 10-minute cache window is acceptable for the data you are collecting. Then 👉 [pick the plan that matches your monthly page count](https://www.scraperapi.com/pricing/?fp_ref=coupons).
