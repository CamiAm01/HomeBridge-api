# 🏡 HomeBridge API

API REST backend para la aplicación móvil **HomeBridge** — una plataforma que simplifica la compra, venta y alquiler de inmuebles en Perú.

Desarrollada con **.NET**, estructurada siguiendo principios de **Domain-Driven Design (DDD)** y protegida con **autenticación JWT**.

---

## 📋 Tabla de contenidos

- [Descripción general](#descripción-general)
- [Tecnologías](#tecnologías)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Instalación y ejecución](#instalación-y-ejecución)
- [Autenticación](#autenticación)
- [Endpoints](#endpoints)
- [Contribución](#contribución)

---

## Descripción general

La API de HomeBridge soporta las funcionalidades principales de la app móvil:

- 🔍 Búsqueda y filtrado de publicaciones inmobiliarias
- 📄 Creación, consulta y eliminación de publicaciones
- 👤 Registro, inicio de sesión y gestión del perfil de usuario
- 🔐 Autenticación basada en JWT con soporte para renovación de token

La aplicación está dirigida a personas de 25 a 55 años en zonas urbanas del Perú que buscan comprar, vender o alquilar propiedades.

---

## Tecnologías

| Capa | Tecnología                                                   |
|------|--------------------------------------------------------------|
| Framework | .NET 8                                                       |
| Arquitectura | Domain-Driven Design (DDD)                                   |
| Autenticación | JWT Bearer Tokens                                            |
| Base de datos | MySQL                                                        |
| Documentación | Swagger / OpenAPI                                            |

---

## Estructura del proyecto

Las capas de DDD se reflejan en la organización de carpetas:

```
HomeBridge.API/
│
├── Domain/                   # Lógica de negocio central
│   ├── Entities/
│   ├── ValueObjects/
│   └── Interfaces/
│
├── Application/              # Casos de uso y servicios
│   ├── Publications/
│   ├── Search/
│   └── Users/
│
├── Infrastructure/           # Base de datos, servicios externos y repositorios
│   ├── Persistence/
│
└── API/                      # Controladores y middleware
    ├── Controllers/
    └── Middleware/
```

---

## Instalación y ejecución

### Requisitos previos

- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- Una instancia activa de la base de datos (MySQL)

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/CamiAm01/HomeBridge-api.git
cd homebridge-api

# 2. Configurar variables de entorno
cp appsettings.example.json appsettings.Development.json
# Completar la cadena de conexión y los datos de JWT

# 3. Aplicar migraciones
dotnet ef database update

# 4. Ejecutar la API
dotnet run --project HomeBridge.API
```

La API estará disponible en `https://localhost:5001`.  
Swagger UI: `https://localhost:5001/swagger`

### Claves requeridas en appsettings.json

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "TU_CADENA_DE_CONEXION"
  },
  "Jwt": {
    "Key": "TU_CLAVE_SECRETA",
    "Issuer": "homebridge-api",
    "Audience": "homebridge-app",
    "ExpiresInMinutes": 60,
    "RefreshExpiresInDays": 7
  }
}
```
---

## Autenticación

La API utiliza **JWT Bearer Tokens**.

### Flujo

```
POST /api/v1/user/login
  → retorna { token, refreshToken }

Incluir en las siguientes peticiones:
  Authorization: Bearer <token>

POST /api/v1/user/refreshToken
  → retorna un nuevo token sin necesidad de volver a iniciar sesión
```

Los endpoints marcados con 🔒 requieren un token válido en el header `Authorization`.

---

## Endpoints

### Publication

| Método | Endpoint | Auth | Descripción |
|--------|----------|------|-------------|
| `GET` | `/api/v1/publication/getPublication` | 🔒 | Obtiene una publicación por ID y estado |
| `POST` | `/api/v1/publication/postPublication` | 🔒 | Crea una nueva publicación inmobiliaria |
| `DELETE` | `/api/v1/publication/deletePublication` | 🔒 | Elimina (soft delete) una publicación por ID |

### Search

| Método | Endpoint | Auth | Descripción |
|--------|----------|------|-------------|
| `GET` | `/api/v1/search/main` | 🔒 | Busca publicaciones según criterios de búsqueda |

### UserAuthentication

| Método | Endpoint | Auth | Descripción |
|--------|----------|------|-------------|
| `POST` | `/api/v1/user/login` | Público | Inicia sesión y obtiene un token JWT |
| `POST` | `/api/v1/user/refreshToken` | 🔒 | Renueva el token sin volver a iniciar sesión |

### UserManager

| Método | Endpoint | Auth | Descripción |
|--------|----------|------|-------------|
| `GET` | `/api/v1/user/getUserInformation` | 🔒 | Obtiene la información de un usuario por su ID |

### UserRegister

| Método | Endpoint | Auth | Descripción |
|--------|----------|------|-------------|
| `POST` | `/api/v1/user/register` | Público | Registra un nuevo usuario en el sistema |

---

> HomeBridge — Conectando personas con su próximo hogar. 🏡