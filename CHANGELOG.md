# Changelog

All notable API changes. The live changelog is at [xaus.com/api#changelog](https://xaus.com/api/#changelog).

## Platform, 2026-07

- Launched gold data widgets: embeddable live price, historical chart and converter modules at `/widgets/price`, `/widgets/chart` and `/widgets/converter` (dark and light themes, multi-currency), powered by this API.

## v1.5, 2026-07

- Added AI-consumer optimizations: `?compact=1` on `/spot` (omits the FX table), `/api/openapi.json` machine-readable spec, `/llms.txt` AI-crawler map, `/api/v1/intraday` first-party recorded series, and documented freshness-validation rules for automated consumers.

## v1.4, 2026-07

- Added canonical `data_state` object (`status` / `as_of` / `source` / `age_seconds`) on every response including 503. Removes any need to infer state from multiple fields. `stale` and `price_as_of` retained for backward compatibility.

## v1.3, 2026-07

- Added formal data-state contract (`fresh` / `stale` / `unavailable`), `fx_stale` flag, `stats_as_of` vintage for annual statistics, and `?debug=1` observability mode. No breaking changes.

## v1.2, 2026-07

- Changed outage behaviour: the API now serves the last known real price with `stale: true` and `price_as_of`, or an honest 503. Simulated fallback prices removed.
- Added `stale` and `price_as_of` fields.
