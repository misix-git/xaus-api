# PHP examples

## Live spot price

```php
<?php
$response = file_get_contents('https://xaus.com/api/v1/spot?currency=EUR');
$data = json_decode($response, true);

echo $data['spot_usd_oz'];    // USD per troy ounce
echo $data['xau']['price'];   // EUR per troy ounce
```

## Respecting the data_state contract

```php
<?php
$ctx = stream_context_create(['http' => ['ignore_errors' => true]]);
$response = file_get_contents('https://xaus.com/api/v1/spot?compact=1', false, $ctx);
$data = json_decode($response, true);

if (isset($data['data_state']) && $data['data_state']['status'] === 'stale') {
    error_log('XAUS: serving last real price from ' . $data['data_state']['as_of']);
}
echo $data['spot_usd_oz'];
```
