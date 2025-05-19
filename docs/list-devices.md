
# 📘 Documentación de la API de Rastreo Vial

## Endpoint: Obtener Lista de Dispositivos

Este endpoint permite recuperar una lista de todos los dispositivos asociados a la cuenta autenticada en la plataforma de Rastreo Vial.

- **URL Base**: `https://gps.rastreovial.com/api/`
- **Endpoint**: `devices`
- **Método HTTP**: `GET`
- **Autenticación**: Requiere token JWT en el encabezado

---

## 🔐 Parámetros de Consulta

Este endpoint no requiere parámetros adicionales en la consulta. Sin embargo, se debe incluir el token de autenticación en el encabezado de la solicitud.

### Encabezado de Autenticación

```
Authorization: Bearer {token}
```

Reemplaza `{token}` con el token JWT obtenido durante el proceso de autenticación.

---

## ✅ Respuesta Exitosa

Si la solicitud es válida y el token de autenticación es correcto, la API responderá con una lista de dispositivos asociados al usuario.

### Código de Estado: `200 OK`

### Cuerpo de la Respuesta

```json
[
  {
    "id": 1,
    "name": "Vehículo 1",
    "imei": "123456789012345",
    "device_model": "Modelo X",
    "sim_number": "5551234567",
    "status": "online",
    "created_at": "2025-01-15T08:30:00Z",
    "updated_at": "2025-05-18T14:45:00Z"
  },
  {
    "id": 2,
    "name": "Vehículo 2",
    "imei": "987654321098765",
    "device_model": "Modelo Y",
    "sim_number": "5557654321",
    "status": "offline",
    "created_at": "2025-02-20T10:15:00Z",
    "updated_at": "2025-05-17T09:20:00Z"
  }
]
```

### Campos de la Respuesta

| Campo         | Tipo   | Descripción                                      |
|----------------|--------|-------------------------------------------------|
| `id`          | int    | ID único del dispositivo                         |
| `name`        | string | Nombre asignado al dispositivo                   |
| `imei`        | string | Número IMEI del dispositivo                      |
| `device_model`| string | Modelo del dispositivo                           |
| `sim_number`  | string | Número de la tarjeta SIM asociada                |
| `status`      | string | Estado actual del dispositivo (`online` o `offline`) |
| `created_at`  | string | Fecha de creación del dispositivo (ISO 8601)     |
| `updated_at`  | string | Fecha de última actualización del dispositivo (ISO 8601) |

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

- Asegúrate de mantener tu token seguro y no compartirlo públicamente.
- La lista de dispositivos puede ser utilizada para obtener información detallada de cada uno mediante otros endpoints de la API.
- Si necesitas filtrar o paginar los resultados, consulta la documentación adicional de la API para conocer los parámetros disponibles.
