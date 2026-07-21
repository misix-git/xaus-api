# JavaScript examples

Works in browsers (CORS is open) and in Node 18+.

## Live spot price

```javascript
const res  = await fetch('https://xaus.com/api/v1/spot');
const data = await res.json();

console.log(data.spot_usd_oz);   // e.g. 4019.30
console.log(data.xau.price);     // price in requested currency and unit
```

## Currency and unit conversion

```javascript
const res = await fetch('https://xaus.com/api/v1/spot?currency=EUR&unit=gram');
const eurGram = await res.json();
console.log(eurGram.xau.price);  // EUR per gram
```

## Respecting the data_state contract

```javascript
const res  = await fetch('https://xaus.com/api/v1/spot?compact=1');
if (res.status === 503) {
  // unavailable: no real value exists right now; do not invent one
  console.warn('XAUS: price unavailable');
} else {
  const d = await res.json();
  if (d.data_state.status === 'stale') {
    console.warn(`XAUS: serving last real price from ${d.data_state.as_of} (${d.data_state.age_seconds}s old)`);
  }
  console.log(d.spot_usd_oz);
}
```

## Intraday series for a chart

```javascript
const res = await fetch('https://xaus.com/api/v1/intraday?symbol=xau&hours=24');
const { points } = await res.json();   // [{ t: unixSeconds, p: price }, ...]
const series = points.map(pt => ({ x: new Date(pt.t * 1000), y: pt.p }));
```
