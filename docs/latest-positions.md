
# 📘 Documentación de la API de Rastreo Vial

## Endpoint: Obtener Última Posición de los Dispositivos

Este endpoint permite recuperar la última posición registrada de todos los dispositivos asociados a la cuenta autenticada en la plataforma de Rastreo Vial.

- **URL Base**: `https://gps.rastreovial.com/api/`
- **Endpoint**: `devices/last_position`
- **Método HTTP**: `GET`
- **Autenticación**: Requiere token JWT en el encabezado

---

## 🔐 Parámetros de Consulta

Este endpoint no requiere parámetros adicionales en la consulta. Solo es necesario incluir el token de autenticación.

### Encabezado de Autenticación

```
Authorization: Bearer {token}
```

Reemplaza `{token}` con el token JWT obtenido durante el proceso de autenticación.

---

## ✅ Respuesta Exitosa

Si la solicitud es válida y el token de autenticación es correcto, la API responderá con la última posición de cada dispositivo.

### Código de Estado: `200 OK`

### Cuerpo de la Respuesta

```json
[
  {
    "id": 1,
    "name": "Vehículo 1",
    "imei": "123456789012345",
    "latitude": 19.432608,
    "longitude": -99.133209,
    "speed": 62,
    "status": "online",
    "timestamp": "2025-05-18T14:45:00Z"
  },
  {
    "id": 2,
    "name": "Vehículo 2",
    "imei": "987654321098765",
    "latitude": 20.659698,
    "longitude": -103.349609,
    "speed": 0,
    "status": "offline",
    "timestamp": "2025-05-17T09:20:00Z"
  }
]
```

### Campos de la Respuesta

| Campo       | Tipo   | Descripción                                     |
|--------------|--------|------------------------------------------------|
| `id`        | int    | ID único del dispositivo                        |
| `name`      | string | Nombre asignado al dispositivo                  |
| `imei`      | string | Número IMEI del dispositivo                     |
| `latitude`  | float  | Última latitud registrada                       |
| `longitude` | float  | Última longitud registrada                      |
| `speed`     | int    | Velocidad actual en km/h                        |
| `status`    | string | Estado actual del dispositivo (`online` o `offline`) |
| `timestamp` | string | Fecha y hora de la última actualización (ISO 8601) |

---

## ❌ Respuesta de Error

Si el token de autenticación es inválido o ha expirado, la API responderá con un mensaje de error.

### Código de Estado: `401 Unauthorized`

### Ejemplo de Respuesta de Error

```json
{
  "status": false,
  "message": "Token de autenticación inválido o expirado."
}
```

---

## 📌 Notas Adicionales

- La posición reflejada es la última registrada, puede no ser en tiempo real.
- Si requieres obtener la posición en tiempo real, utiliza el servicio de monitoreo en vivo.
- Asegúrate de proteger tu token de autenticación y no compartirlo públicamente.

