# Totaljobs Scraper

Extract structured data from [totaljobs.com](https://totaljobs.com) — job listings from totaljobs.com — UK's largest job board. Salary data, full descriptions, company profiles, contact info, and incremental monitoring.

**[Run on Apify →](https://apify.com/blackfalcondata/totaljobs-scraper)**

---

## Key features

🔍 **Smart search with filters**

Search by keyword, location, and multiple filters. Smart input resolution ensures you always get results.

📄 **Detail enrichment**

Fetch full job descriptions, salary data, employer profiles, and contact information for each listing.

🔄 **Incremental mode**

Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

⚡ **Compact output for AI agents**

Core-fields-only mode optimized for MCP and AI agent workflows. Description truncation to control output size.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from totaljobs on a schedule. Export to CSV, JSON, or directly to your database.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from totaljobs.

**AI and LLM workflows**
Use compact mode and description truncation to feed data into AI agents, MCP servers, and LLM pipelines without exceeding token budgets.

---

## Quick start

```json
{
  "query": "software-developer",
  "maxResults": 50,
  "includeDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `startUrls` | array | — | Direct StepStone search URLs to scrape. If provided, query/location/geo are ignored for URL building. |
| `query` | string | — | Job search keyword (e.g., 'software-developer', 'data-analyst'). Not required when startUrls is provided. |
| `location` | string | — | Location filter (e.g., 'Berlin', 'München') |
| `geo` | string | `"TOTALJOBS"` | Fixed to Totaljobs (UK). |
| `sort` | enum | `"relevance"` | Sort order for results |
| `age` | integer | — | Only show jobs posted within this many days |
| `remote` | boolean | — | Only show remote/home office positions |
| `radius` | enum | — | Search radius around location in km (requires location) |
| `minSalary` | integer | — | Minimum annual salary filter (EUR) |
| `contractType` | enum | — | Filter by contract/employment type |
| `experience` | enum | — | Filter by experience level |
| `workType` | enum | — | Filter by work schedule (full-time / part-time) |
| `companyId` | integer | — | Filter by StepStone company ID (numeric) |
| `language` | string | — | Filter by job posting language (e.g., 'en', 'de') |
| `applicationMethod` | enum | — | Filter by how to apply (e.g., INTERNAL = apply on company site only) |
| `excludeSponsored` | boolean | `false` | Skip sponsored/promoted listings. Filters out items where isSponsored=true. |
| `includeDetails` | boolean | `true` | Fetch each job's detail page for full description, ISO dates, salary, and geo coordinates. Slower but much richer data. |
| `detailEngine` | enum | `"auto"` | Controls how job detail pages are fetched. 'auto' (recommended) = actor chooses best method automatically. 'cheerio' = lightweight HTTP-only detail fetching. 'playwright' = browser-based detail fetching (heavier, for WAF-protected portals). |
| `descriptionFormat` | enum | `"html"` | Format for job description text (requires Include Detail Pages) |
| `maxResults` | integer | `100` | Maximum number of job listings to return (0 = unlimited) |
| `maxPages` | integer | `10` | Maximum number of search result pages to scrape |
| `mode` | enum | `"full"` | full = return all jobs found. incremental = only return jobs not seen in previous runs (requires stateStoreName). |
| `stateStoreName` | string | `"stepstone-state"` | KV store name for incremental state. Use same name across scheduled runs. State is scoped per query+location+geo combination. |
| `dedupStoreName` | string | — | Named KV store for cross-run dedup. Same name across runs = only new jobs pushed. |
| `dedupKey` | string | `"jobKey"` | Field used as unique key for dedup (default: jobKey) |
| `datasetName` | string | — | Custom dataset name. Supports masks: {DATE} = YYYYMMDD, {TIME} = HHMMSS |
| `outputFields` | array | — | Select subset of output fields. Use dot notation for nested: 'ceSalary.min'. If empty: all fields returned. |
| `compact` | boolean | `false` | When true, each result contains only the 11 most essential fields: jobKey, title, company, location, url, portalUrl, datePosted, workFromHome, unifiedSalary, geo, and description. Use this in AI-agent and MCP workflows where token budgets matter. |
| `descriptionMaxLength` | integer | `0` | Truncate the description field to this many characters, appending '...' if truncated. 0 means no truncation. Use in AI-agent workflows to control context window usage. Pairs well with compact mode. |
| `proxyConfiguration` | object | `{"useApifyProxy":true}` | Apify proxy configuration. Some portals require residential proxy (handled automatically). Your Apify plan must include proxy access. |
| `maxConcurrency` | integer | `5` | Maximum number of concurrent requests |
| `maxRequestRetries` | integer | `3` | Maximum number of retries per request |

---

## FAQ

**How to scrape totaljobs?**
Use this actor on Apify to extract structured data from totaljobs.com. Set your search query and filters in the input, then run — no coding needed.

**Is there a totaljobs API?**
totaljobs.com does not provide a public API. This actor works as an unofficial API, returning structured JSON data from totaljobs.

**Is it legal to scrape totaljobs.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check the target site's terms of service for your specific use case.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage.

---

## Known limitations

<!-- WRITE: 4-6 honest limitations -->

- <!-- WRITE: limitation 1 -->
- <!-- WRITE: limitation 2 -->

---

## Related products by Black Falcon Data

| Product | Description |
|:--------|:------------|
| [StepStone Jobs API](https://github.com/BlackFalconData-org/stepstone-jobs-api) | Job listings from 18 European portals |
| [Company Jobs Tracker](https://github.com/BlackFalconData-org/company-jobs-tracker-api) | Track new/removed jobs per company |
| [Indeed Jobs Feed](https://github.com/BlackFalconData-org/indeed-jobs-feed) | Indeed job listings with salary data |
| [Glassdoor Jobs Feed](https://github.com/BlackFalconData-org/glassdoor-jobs-feed) | Glassdoor listings with company ratings |
| [Arbeitsagentur Jobs Feed](https://github.com/BlackFalconData-org/arbeitsagentur-jobs-feed) | Germany's federal job portal (1M+ listings) |
| [Naukri Jobs Feed](https://github.com/BlackFalconData-org/naukri-jobs-feed) | India's largest job portal |
| [Bilbasen Scraper](https://github.com/BlackFalconData-org/bilbasen-scraper) | Denmark's largest car marketplace |

---

*Last updated: 2026 03*
