# XAUS Gold Data API

Free, keyless JSON API for gold market data, from [xaus.com](https://xaus.com), an independent, non-profit gold data platform.

- **No API key. No registration. No enforced rate limits** (fair use).
- CORS open, GET only, JSON everywhere.
- **Trust policy: no fabricated data, ever.** During upstream outages the API serves the last known real price, explicitly flagged, or an honest 503. Prices are never simulated.

## Quickstart

```bash
curl https://xaus.com/api/v1/spot
```

```json
{
  "spot_usd_oz": 4019.30,
  "xau": { "price": 4019.30, "currency": "USD", "unit": "oz" },
  "data_state": { "status": "fresh", "as_of": "2026-07-21T07:58:12Z", "source": "upstream", "age_seconds": 0 },
  "updated_at": "2026-07-21T07:58:12Z"
}
```

Response shown abridged. See the [full documentation](https://xaus.com/api/) and the interactive explorer.

## Endpoints

| Endpoint | Data | Key params |
|---|---|---|
| [`/api/v1/spot`](https://xaus.com/api/v1/spot) | Live XAU/USD spot, silver, ratios, tokenized gold, fundamentals | `?currency=EUR` `?unit=oz\|gram\|kg` `?compact=1` |
| [`/api/v1/intraday`](https://xaus.com/api/v1/intraday?symbol=xau&hours=24) | First-party recorded series, sampled every 2 minutes | `?symbol=xau\|xag` `?hours=1..48` |
| [`/api/v1/history`](https://xaus.com/api/v1/history) | Up to 5 years of daily closes, 52-week stats, ranges, returns | none |
| [`/api/v1/chart`](https://xaus.com/api/v1/chart?symbol=gold&range=1y) | Multi-asset OHLCV: gold, silver, BTC, S&P 500, WTI and more | `?symbol` `?range` `?interval` |
| [`/api/v1/reserves`](https://xaus.com/api/v1/reserves) | Central-bank gold holdings by country, monthly cadence | none |

Machine-readable contract: [`openapi.yaml`](./openapi.yaml) in this repo, served live at [xaus.com/api/openapi.json](https://xaus.com/api/openapi.json).

## The data_state contract

Every response describes its own freshness. Automated consumers should read it instead of assuming:

| status | Meaning |
|---|---|
| `fresh` | Value sourced from the live upstream on this request or within its cache TTL. |
| `stale` | Upstream outage: this is the last real observed price, with `as_of` and `age_seconds` telling you exactly how old. |
| `unavailable` | No real value exists to serve. Core price endpoints return HTTP 503 rather than a guess. |

AI agents and automated consumers: use `?compact=1` on `/spot` (omits the ~160-entry FX table), and validate `updated_at` if your fetcher caches aggressively. Full guidance in the [API docs](https://xaus.com/api/) and [llms.txt](https://xaus.com/llms.txt).

## Examples

Runnable snippets in [`examples/`](./examples): [curl](./examples/curl.md), [JavaScript](./examples/javascript.md), [Python](./examples/python.md), [PHP](./examples/php.md).

## Widgets

Embeddable live price, chart and converter modules built on this API: [xaus.com/widgets](https://xaus.com/widgets/).

## Sources and methodology

Every data source is documented on the [About page](https://xaus.com/about/). Spot prices are indicative mid-market rates, not tradable quotes. XAUS is independent and provides no financial advice. Definitions for every term used by the API: [xaus.com/definitions](https://xaus.com/definitions/).

## License

Code in this repository: [MIT](./LICENSE). API data: free to use, attribution appreciated (a link to xaus.com).
