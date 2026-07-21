# curl examples

## Live spot price

```bash
curl https://xaus.com/api/v1/spot
```

## Currency and unit conversion

```bash
# EUR per troy ounce
curl "https://xaus.com/api/v1/spot?currency=EUR"

# GBP per gram
curl "https://xaus.com/api/v1/spot?currency=GBP&unit=gram"
```

## Compact mode for automated consumers

Omits the ~160-entry FX table, roughly 75% smaller:

```bash
curl "https://xaus.com/api/v1/spot?compact=1"
```

## Other endpoints

```bash
# First-party intraday series, last 24 hours
curl "https://xaus.com/api/v1/intraday?symbol=xau&hours=24"

# 5 years of daily history with 52-week stats
curl https://xaus.com/api/v1/history

# Multi-asset OHLCV
curl "https://xaus.com/api/v1/chart?symbol=gold&range=1y"

# Central-bank reserves by country
curl https://xaus.com/api/v1/reserves
```

## Checking freshness in a script

```bash
curl -s https://xaus.com/api/v1/spot?compact=1 | jq '{price: .spot_usd_oz, state: .data_state.status, age: .data_state.age_seconds}'
```
