# API de Autenticación - NestJS

API de autenticación completa con JWT para el proyecto de financiamiento.

## 🚀 Características

- ✅ **Autenticación JWT** completa
- ✅ **Registro de usuarios** con validación
- ✅ **Login de usuarios** con validación
- ✅ **Protección de rutas** con guards
- ✅ **Validación de datos** con class-validator
- ✅ **Encriptación de contraseñas** con bcrypt
- ✅ **CORS habilitado** para frontend
- ✅ **Respuestas estandarizadas** con formato consistente

## 📁 Estructura del Proyecto

```
src/
├── auth/
│   ├── dto/
│   │   ├── login.dto.ts          # DTO para login
│   │   └── register.dto.ts       # DTO para registro
│   ├── guards/
│   │   └── jwt-auth.guard.ts     # Guard JWT
│   ├── interfaces/
│   │   └── auth.interface.ts     # Interfaces TypeScript
│   ├── strategies/
│   │   └── jwt.strategy.ts       # Estrategia JWT
│   ├── auth.controller.ts        # Controlador de auth
│   ├── auth.service.ts           # Servicio de auth
│   └── auth.module.ts            # Módulo de auth
├── users/
│   ├── users.service.ts          # Servicio de usuarios
│   └── users.module.ts           # Módulo de usuarios
└── main.ts                       # Configuración principal
```

## 🔧 Instalación y Configuración

### 1. Instalar dependencias

```bash
pnpm install
```

### 2. Variables de entorno (opcional)

```bash
# .env
JWT_SECRET=tu-clave-secreta-super-segura
PORT=3000
```

### 3. Ejecutar en desarrollo

```bash
pnpm run start:dev
```

### 4. Ejecutar en producción

```bash
pnpm run build
pnpm run start:prod
```

## 📡 Endpoints de la API

### Base URL

```
http://localhost:3000
```

### 1. Registro de Usuario

```http
POST /auth/register
Content-Type: application/json

{
  "email": "usuario@example.com",
  "password": "123456",
  "name": "Usuario Ejemplo"
}
```

**Respuesta exitosa:**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": 2,
      "email": "usuario@example.com",
      "name": "Usuario Ejemplo",
      "createdAt": "2024-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-01T00:00:00.000Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  },
  "message": "Usuario registrado exitosamente"
}
```

### 2. Login de Usuario

```http
POST /auth/login
Content-Type: application/json

{
  "email": "usuario@example.com",
  "password": "123456"
}
```

**Respuesta exitosa:**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": 1,
      "email": "usuario@example.com",
      "name": "Usuario Ejemplo",
      "createdAt": "2024-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-01T00:00:00.000Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  },
  "message": "Login exitoso"
}
```

### 3. Obtener Perfil (Protegido)

```http
GET /auth/profile
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Respuesta exitosa:**

```json
{
  "success": true,
  "data": {
    "id": 1,
    "email": "usuario@example.com",
    "name": "Usuario Ejemplo",
    "createdAt": "2024-01-01T00:00:00.000Z",
    "updatedAt": "2024-01-01T00:00:00.000Z"
  },
  "message": "Perfil obtenido exitosamente"
}
```

## 🔐 Autenticación

### Usuario por defecto

```
Email: admin@example.com
Password: password
```

### Cómo usar el token JWT

1. Hacer login o registro para obtener el token
2. Incluir el token en el header `Authorization: Bearer <token>`
3. El token expira en 24 horas

## 🛡️ Validaciones

### Registro

- ✅ Email válido
- ✅ Contraseña mínimo 6 caracteres
- ✅ Nombre requerido
- ✅ Email único

### Login

- ✅ Email válido
- ✅ Contraseña mínimo 6 caracteres
- ✅ Credenciales correctas

## 🔄 Integración con Frontend

### Configuración en el adaptador HTTP

Actualizar la URL base en el frontend:

```typescript
// src/app/core/adapters/http.adapter.ts
private baseUrl = 'http://localhost:3000'; // API de NestJS
```

### Ejemplo de uso

```typescript
// Login
this.apiService
  .login({
    email: 'usuario@example.com',
    password: '123456',
  })
  .subscribe({
    next: (response) => {
      console.log('Login exitoso:', response);
      // Guardar token en localStorage
      localStorage.setItem('token', response.data.token);
    },
    error: (error) => {
      console.error('Error de login:', error);
    },
  });
```

## 🚨 Manejo de Errores

### Errores comunes

```json
// Credenciales inválidas
{
  "success": false,
  "error": "Credenciales inválidas",
  "message": "Unauthorized"
}

// Email ya registrado
{
  "success": false,
  "error": "El email ya está registrado",
  "message": "Conflict"
}

// Validación fallida
{
  "success": false,
  "error": "Validation failed",
  "message": "Bad Request"
}
```

## 🔧 Próximos Pasos

1. **Base de datos**: Integrar con PostgreSQL/MySQL
2. **Refresh tokens**: Implementar renovación automática
3. **Roles y permisos**: Sistema de autorización
4. **Logs**: Sistema de auditoría
5. **Rate limiting**: Protección contra ataques
6. **Tests**: Pruebas unitarias y e2e

## 📝 Notas

- Los usuarios se almacenan en memoria (se pierden al reiniciar)
- El JWT_SECRET por defecto es 'your-secret-key' (cambiar en producción)
- CORS está habilitado para desarrollo
- La validación es estricta (rechaza campos no permitidos)
