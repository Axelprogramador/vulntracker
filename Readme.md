# 🛡️ VulnTracker

API REST para gestión y seguimiento de vulnerabilidades de seguridad dentro de una organización, desarrollada con Spring Boot siguiendo arquitectura hexagonal.

## Tecnologías

- **Java 21** + **Spring Boot 3.5**
- **Arquitectura hexagonal** (ports & adapters)
- **Spring Security** + **JWT** para autenticación
- **Hibernate** + **Spring Data JPA**
- **MySQL 8**
- **Swagger UI** para documentación de la API
- **Docker** + **Docker Compose**
- **JUnit 5** + **Mockito** para tests unitarios

## Funcionalidades

- Registro y login de usuarios con tokens JWT
- Roles: `ADMIN` y `ANALYST`
- CRUD de vulnerabilidades con severidad (LOW, MEDIUM, HIGH, CRITICAL)
- Cambio de estado (OPEN → IN_PROGRESS → RESOLVED)
- Filtrado por severidad
- Dashboard web con KPIs y tabla de vulnerabilidades
- API documentada con Swagger UI

## Arquitectura
```
domain/
├── model/          ← Entidades y enums (Java puro, sin frameworks)
├── port/in/        ← Casos de uso (interfaces)
├── port/out/       ← Puertos de salida (interfaces)
└── service/        ← Implementación de la lógica de negocio

application/
├── controller/     ← Controllers REST
├── dto/            ← DTOs de entrada y salida
└── security/       ← JWT y Spring Security

infrastructure/
├── persistence/    ← Adaptadores Hibernate y repositorios JPA
└── config/         ← Configuración de Spring, Swagger y beans
```

## Arrancar el proyecto

### Con Docker Compose (recomendado)

```bash
docker compose up --build
```

La app estará disponible en `http://localhost:8080`

### En local

Requisitos: Java 21, Maven, MySQL 8

```bash
# 1. Levanta MySQL
docker compose up mysql -d

# 2. Arranca la app
./mvnw spring-boot:run
```

## API

La documentación completa está disponible en Swagger UI: 
http://localhost:8080/swagger-ui/index.html

### Endpoints principales

| Método | Ruta | Descripción | Auth |
|--------|------|-------------|------|
| POST | `/api/auth/register` | Registro de usuario | No |
| POST | `/api/auth/login` | Login, devuelve JWT | No |
| GET | `/api/vulnerabilities` | Listar todas | Sí |
| POST | `/api/vulnerabilities` | Crear vulnerabilidad | Sí |
| PATCH | `/api/vulnerabilities/{id}/status` | Cambiar estado | Sí |
| GET | `/api/vulnerabilities/filter?severity=HIGH` | Filtrar por severidad | Sí |

### Ejemplo de uso

```bash
# 1. Registro
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username": "axel", "password": "password123"}'

# 2. Usar el token devuelto
curl -X GET http://localhost:8080/api/vulnerabilities \
  -H "Authorization: Bearer <token>"
```

## Tests

```bash
./mvnw test
```