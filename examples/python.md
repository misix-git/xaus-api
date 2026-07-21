# Python examples

Uses `requests` (`pip install requests`).

## Live spot price

```python
import requests

r    = requests.get("https://xaus.com/api/v1/spot")
data = r.json()
print(data["spot_usd_oz"])    # e.g. 4019.30
print(data["per_gram_usd"])   # USD per gram
```

## Currency and unit conversion

```python
r = requests.get(
    "https://xaus.com/api/v1/spot",
    params={"currency": "JPY", "unit": "kg"},
)
print(r.json()["xau"]["price"])   # JPY per kg
```

## Respecting the data_state contract

```python
r = requests.get("https://xaus.com/api/v1/spot", params={"compact": 1})
if r.status_code == 503:
    # unavailable: no real value exists right now; do not invent one
    print("XAUS: price unavailable")
else:
    d = r.json()
    state = d["data_state"]
    if state["status"] == "stale":
        print(f"XAUS: last real price from {state['as_of']} ({state['age_seconds']}s old)")
    print(d["spot_usd_oz"])
```

## Daily history into a DataFrame

```python
import pandas as pd
import requests

r  = requests.get("https://xaus.com/api/v1/history")
df = pd.DataFrame(r.json()["points"])          # columns: d (date), c (close), h, l
df["d"] = pd.to_datetime(df["d"])
print(df.tail())
print("52w high:", r.json()["high_52w"])
```
