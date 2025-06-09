
# Rastreo Vial API – **POST `/login`**

Authenticate a user and receive a `user_api_hash`.  
Save this hash and include it in every subsequent request (e.g., `user_api_hash=<hash>`).  
The hash **changes** if the user resets their password—at that point the API will return an authentication error and you must re‑login.

> **Base URL**  
> `https://gps.rastreovial.com`

| Method | Endpoint |
| ------ | -------- |
| `POST` | `/login` |

---

## Request

**Content‑Type:** `multipart/form-data`

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `email` | `string` | **Yes** | User’s email address |
| `password` | `string` | **Yes** | User’s password |
| `as` | `string` | No | *Optional.* Log in **as another user** (provide their email). Only available to admins. |

---

## Successful Response `200 OK`

```json
{
  "status": 1,
  "user_api_hash": "$2y$10$5RACGMNxUdz3h1ug9yAttu95U2acugM0YG1K5wx01ZrNMvpL6BWMS"
}
```

| Field | Type | Description |
| ----- | ---- | ----------- |
| `status` | `integer` | `1` on success |
| `user_api_hash` | `string` | Token to authenticate future calls |

---

## Error Response `401 Unauthorized`

```json
{ "status": 0, "error": "invalid_credentials" }
```

---

## Example Request (JavaScript `fetch`)

```js
const url = 'https://gps.rastreovial.com/login';

const form = new FormData();
form.append('email', 'user@example.com');
form.append('password', 'p@ssw0rd');

const options = {
  method: 'POST',
  headers: { Accept: 'application/json' },
  body: form
};

(async () => {
  try {
    const res = await fetch(url, options);
    const data = await res.json();
    console.log(data);           // { status: 1, user_api_hash: '...' }
  } catch (err) {
    console.error(err);
  }
})();
```


---

> © 2025 Rastreo Vial – All rights reserved.
