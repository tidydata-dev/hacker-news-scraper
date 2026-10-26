# Hacker News Scraper — Python, Node.js & cURL examples

Get **Hacker News** data through the official HN APIs: **front page, new, best, Ask HN, Show HN and jobs** with rank, **keyword search** across the whole archive, **every story linking to a domain**, **full comment threads** with depth, and **user profiles**. **Only new since the last run** for brand-mention alerts. **$0.20 per 1,000 results**.

Hosted on Apify: **[Hacker News Scraper](https://apify.com/tidydata/hacker-news-scraper?fpr=wtg6qt)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/hacker-news-scraper").call(run_input={
    "searchQueries": [
        "yourcompany.com"
    ],
    "searchIn": "url",
    "sortBy": "date",
    "newerThanDays": 30,
    "onlyNew": True
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['type', 'title', 'points', 'comments', 'hnUrl']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/hacker-news-scraper').call({"lists": ["top"], "maxItems": 30, "includeComments": true});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~hacker-news-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"lists": ["top"], "maxItems": 30, "includeComments": true}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper?fpr=wtg6qt) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper?fpr=wtg6qt) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper?fpr=wtg6qt) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor?fpr=wtg6qt) · [Google News](https://apify.com/tidydata/google-news-scraper?fpr=wtg6qt) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper?fpr=wtg6qt) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets?fpr=wtg6qt) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.

---
<sub>Links to apify.com include our affiliate ID: if you start a paid Apify plan through them, Apify pays TidyData a commission at no extra cost to you.</sub>
