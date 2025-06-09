
# Rastreo Vial API – **GET `/get_devices_latest`**

Return only the devices whose status or telemetry **changed since your last call**.  
Every successful response also includes a top-level integer field `time`; use that value (in **seconds**) as the recommended delay before you issue the next polling request.

> **Base URL**  
> `https://gps.rastreovial.com`

| Method | Endpoint |
| ------ | -------- |
| `GET`  | `/get_devices_latest` |

---

## Query Parameters

### Required

| Name | Type | Default | Description |
| ---- | ---- | ------- | ----------- |
| `lang` | `string` | `en` | Response language |
| `user_api_hash` | `string` | – | Your personal API key (hash) |

### Poll-control

| Name | Type | Default | Description |
| ---- | ---- | ------- | ----------- |
| `time` | `string (UNIX timestamp)` | *Now – 5 s* | Only return devices updated **after** this timestamp. |

### Filters (`filters[...]`)

All filters are **optional**; omit any you don’t need.  
Use the `filters[...]` bracket notation exactly as shown.

| Filter | Type | Description |
| ------ | ---- | ----------- |
| `filters[device_model]` | `string` | Exact device model |
| `filters[group_id]` | `integer` | Exact group ID |
| `filters[id]` | `integer` | Exact internal device ID |
| `filters[imei]` | `string` | Exact IMEI |
| `filters[msisdn]` | `string` | Exact MSISDN |
| `filters[object_owner]` | `string` | Exact owner |
| `filters[offline]` | `string` | Devices **offline** for ≥ *N* minutes |
| `filters[online]` | `string` | Devices **online** for ≥ *N* minutes |
| `filters[plate_number]` | `string` | Exact license-plate |
| `filters[registration_number]` | `string` | Exact registration number |
| `filters[sim_number]` | `string` | Exact SIM number |
| `filters[status]` | `array[string]` (CSV) | One or more status values |
| `filters[vin]` | `string` | Exact VIN |

---

## Successful Response `200 OK`

```jsonc
{
  "items": [
    {
      "id": 3,
      "name": "Device name",
      "online": "offline",
      "alarm": "",
      "time": "2016-04-29 21:01:26",
      "timestamp": 1461956486,
      "acktimestamp": 0,
      "speed": 0,
      "lat": 55.922996,
      "lng": 23.3466906,
      "course": "0",
      "power": "-",
      "altitude": 175,
      "address": "-",
      "protocol": "osmand",
      "driver": "Drive first",
      "sensors": "[… JSON string …]",
      "services": "[…]",
      "tail": "[…]",
      "distance_unit_hour": "kph",
      "device_data": { /* see previous endpoint for full schema */ }
    }
  ],
  "events": [],
  "time": 1466992735,
  "version": "2.5.1"
}
```

### Notable Top-Level Fields

| Field | Type | Meaning |
| ----- | ---- | ------- |
| `items` | `array<object>` | List of devices whose data changed |
| `events` | `array<object>` | Optional list of triggered events |
| `time` | `integer` | Seconds to wait **before next poll** |
| `version` | `string` | API version of server |

Device objects have the **same structure** as in `/get_devices`; see that endpoint for a detailed field reference.

---

## Error Responses

| Code | Meaning | Payload |
| ---- | ------- | ------- |
| **400** | Bad request / wrong params | `{"error":"invalid_parameters"}` |
| **401** | Authentication failed | `{"error":"unauthorized"}` |
| **500** | Server error | `{"error":"server_error"}` |

---

## Example Request (JavaScript `fetch`)

```js
const url =
  'https://gps.rastreovial.com/get_devices_latest' +
  '?lang=en' +
  '&user_api_hash=$2y$10$5RACGMNxUdz3h1ug9yAttu95U2acugM0YG1K5wx01ZrNMvpL6BWMS' +
  '&time=' + Math.floor(Date.now() / 1000 - 5);      // now minus 5 s

const options = { method: 'GET', headers: { Accept: 'application/json' } };

(async () => {
  try {
    const res = await fetch(url, options);
    const data = await res.json();
    console.log(data);
    // Next poll: wait data.time seconds
  } catch (err) {
    console.error(err);
  }
})();
```


---

> © 2025 Rastreo Vial – All rights reserved.
