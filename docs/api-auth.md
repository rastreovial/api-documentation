
# 📘 Documentación de la API de Rastreo Vial

## Endpoint: Crear un Login

Este endpoint permite autenticar a un usuario en la plataforma de Rastreo Vial y obtener un token de acceso para futuras solicitudes a la API.

- **URL Base**: `https://gps.rastreovial.com/api/`
- **Endpoint**: `auth/login`
- **Método HTTP**: `POST`
- **Autenticación**: No requiere token previo

---

## 🔐 Parámetros de la Solicitud

La solicitud debe enviarse en formato JSON con los siguientes campos:

| Parámetro | Tipo   | Requerido | Descripción                         |
|-----------|--------|-----------|-------------------------------------|
| `email`   | string | Sí        | Correo electrónico del usuario      |
| `password`| string | Sí        | Contraseña del usuario              |

### Ejemplo de Solicitud

```json
{
  "email": "usuario@ejemplo.com",
  "password": "tu_contraseña_segura"
}
```

---

## ✅ Respuesta Exitosa

Si las credenciales son correctas, la API responderá con un token de autenticación y detalles del usuario.

### Código de Estado: `200 OK`

### Cuerpo de la Respuesta

```json
{
  "status": true,
  "data": {
    "id": 123,
    "email": "usuario@ejemplo.com",
    "api_hash": "abc123def456ghi789jkl012mno345pq",
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
  }
}
```

### Campos de la Respuesta

| Campo      | Tipo   | Descripción                                      |
|------------|--------|--------------------------------------------------|
| `status`   | bool   | Indica si la autenticación fue exitosa           |
| `data.id`  | int    | ID único del usuario                             |
| `data.email`| string| Correo electrónico del usuario                   |
| `data.api_hash`| string| Hash único del usuario para autenticación     |
| `data.token`| string| Token JWT para autenticación en futuras solicitudes |

---

## ❌ Respuesta de Error

Si las credenciales son incorrectas o falta algún parámetro, la API responderá con un mensaje de error.

### Código de Estado: `401 Unauthorized` o `400 Bad Request`

### Ejemplo de Respuesta de Error

```json
{
  "status": false,
  "message": "Credenciales inválidas."
}
```

---

## 🛡️ Uso del Token de Autenticación

El token recibido debe incluirse en el encabezado de las solicitudes posteriores para acceder a endpoints protegidos.

### Encabezado de Autenticación

```
Authorization: Bearer {token}
```

Reemplaza `{token}` con el valor del campo `data.token` obtenido en la respuesta exitosa.

---

## 📌 Notas Adicionales

- Asegúrate de mantener tu token seguro y no compartirlo públicamente.
- El token tiene una duración limitada; consulta la documentación de autenticación para detalles sobre la expiración y renovación del token.
- En caso de múltiples intentos fallidos de inicio de sesión, la cuenta puede ser bloqueada temporalmente por razones de seguridad.
