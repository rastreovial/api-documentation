
# Rastreo Vial API – **GET `/get_devices`**

Retrieve a paginated list of devices that belong to the authenticated user.  
Use the filters below to narrow the results by status, identifiers, group, model, etc.

> **Base URL**  
> `https://gps.rastreovial.com`

| Method | Endpoint |
| ------ | -------- |
| `GET`  | `/get_devices` |

---

## Query Parameters

| Name | Type | Required | Default | Description |
| ---- | ---- | -------- | ------- | ----------- |
| `lang` | `string` | **Yes** | `en` | Response language |
| `user_api_hash` | `string` | **Yes** | – | Your personal API key (hash) |
| `device_model` | `string` | No | – | Exact `device_model` match |
| `group_id` | `string` | No | – | Exact `group_id` match |
| `id` | `string` | No | – | Exact internal device ID |
| `imei` | `string` | No | – | Exact device IMEI |
| `limit` | `integer` | No | `100` | Items per page (max 100) |
| `msisdn` | `string` | No | – | Exact MSISDN |
| `object_owner` | `string` | No | – | Exact object owner |
| `offline` | `string` | No | – | Devices **offline** for at least *N* minutes |
| `online` | `string` | No | – | Devices **online** for at least *N* minutes |
| `page` | `integer` | No | `1` | Page number (1‑based) |
| `plate_number` | `string` | No | – | Exact license‑plate value |
| `registration_number` | `string` | No | – | Exact registration number |
| `s` | `string` | No | – | Full‑text search across properties |
| `sim_number` | `string` | No | – | Exact SIM number |
| `status` | `array[string]` (CSV) | No | – | One or more status values |
| `vin` | `string` | No | – | Exact VIN |

---

## Successful Response `200 OK`

```jsonc
[
  {
    "title": "Ungrouped",
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
        "sensors": [
          { "name": "Sensor test", "value": "- nn", "show_in_popup": "0" },
          { "name": "test acc", "value": "- nn", "show_in_popup": "1" }
        ],
        "services": [
          { "name": "Test service", "value": "Engine hours Left (1000 )" }
        ],
        "tail": [
          { "lat": "55.91986482", "lng": "23.3255625" },
          { "lat": "55.91590619", "lng": "23.33778733" },
          { "lat": "55.91928624", "lng": "23.34572509" },
          { "lat": "55.92336524", "lng": "23.34666575" },
          { "lat": "55.92297793", "lng": "23.34665713" }
        ],
        "distance_unit_hour": "kph",
        "sim_expiration_date": "0000-00-00",
        "device_data": {
          "id": "3",
          "traccar_device_id": "3",
          "icon_id": "8",
          "active": "1",
          "deleted": "0",
          "name": "Device name",
          "imei": "789832",
          "fuel_measurement_id": "1",
          "fuel_quantity": "0.00",
          "fuel_price": "0.00",
          "fuel_per_km": "0.00",
          "sim_number": "",
          "device_model": "",
          "plate_number": "",
          "vin": "",
          "registration_number": "",
          "object_owner": "",
          "expiration_date": "0000-00-00",
          "sim_expiration_date": "0000-00-00",
          "tail_color": "#33cc33",
          "tail_length": "5",
          "engine_hours": "gps",
          "detect_engine": "gps",
          "min_moving_speed": "6",
          "min_fuel_fillings": "10",
          "min_fuel_thefts": "10",
          "snap_to_road": "0",
          "created_at": "2016-04-25 16:21:19",
          "updated_at": "2016-06-26 15:52:46",
          "pivot": {
            "user_id": "2",
            "device_id": "3",
            "group_id": null,
            "current_driver_id": "1",
            "active": "1",
            "timezone_id": null
          },
          "group_id": null,
          "current_driver_id": "1"
        }
      }
    ]
  }
]
```

### Response Field Reference (per device)

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `integer` | Internal device ID |
| `name` | `string` | Display name |
| `online` | `string` | `online` / `offline` |
| `alarm` | `string` | Active alarm code (if any) |
| `time` | `string` | Last position time (YYYY-MM-DD HH:MM:SS) |
| `timestamp` | `integer` | Unix timestamp of `time` |
| `speed` | `integer` | Current speed |
| `lat`, `lng` | `number` | Latitude / Longitude |
| `course` | `string` | Heading (°) |
| `power` | `string` | External power or battery level |
| `altitude` | `integer` | Elevation |
| `address` | `string` | Reverse‑geocoded address |
| `protocol` | `string` | Tracker protocol |
| `driver` | `string` | Current driver’s name |
| `sensors` | `array<object>` | Custom sensor list |
| `services` | `array<object>` | Service reminders |
| `tail` | `array<object>` | Recent path points |
| `distance_unit_hour` | `string` | Speed unit (`kph` / `mph`) |
| `…` | `…` | (See sample above for full set) |

---

## Error Responses

| Code | Meaning | Payload |
| ---- | ------- | ------- |
| **400** | Bad request / wrong params | `{"error": "invalid_parameters"}` |
| **401** | Authentication failed | `{"error": "unauthorized"}` |
| **500** | Server error | `{"error": "server_error"}` |

---

## Example Request (JavaScript `fetch`)

```js
const url =
  'https://gps.rastreovial.com/get_devices' +
  '?lang=en' +
  '&user_api_hash=$2y$10$5RACGMNxUdz3h1ug9yAttu95U2acugM0YG1K5wx01ZrNMvpL6BWMS' +
  '&limit=50&online=5';

const options = { method: 'GET', headers: { Accept: 'application/json' } };

(async () => {
  try {
    const response = await fetch(url, options);
    const data = await response.json();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
})();
```

---

> © 2025 Rastreo Vial – All rights reserved.
