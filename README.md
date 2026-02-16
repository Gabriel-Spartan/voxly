# Voxly – SaaS de Gestión Empresarial

Voxly es un sistema SaaS multi-tenant para pequeñas y medianas empresas, diseñado para centralizar la información operativa con una arquitectura limpia y escalable.

## Tecnologías principales
- **Backend:** .NET 8 (C#) con Arquitectura Onion
- **Frontend:** React + TypeScript (en desarrollo)
- **Base de datos:** SQL Server / PostgreSQL (multi-tenant con esquemas separados)
- **Infraestructura:** Docker, Entity Framework Core, JWT

## Estructura del repositorio
```markdown
├── src/
│ ├── backend/ # API .NET 8 (Onion Architecture)
│ └── frontend/ # Aplicación React (próximamente)
├── docs/ # Documentación del proyecto
└── README.md
```

## Cómo empezar (desarrollo)
1. Clona el repositorio.
2. Asegúrate de tener Docker y .NET 8 SDK instalados.
3. Levanta la base de datos con Docker (SQL Server o PostgreSQL).
4. Abre `src/backend/Voxly.sln` en Rider y ejecuta la API.
5. (Opcional) Para el frontend, accede a `src/frontend` y sigue las instrucciones.

## Documentación
Consulta la carpeta [`docs/`](docs/) para más detalles sobre:
- Arquitectura y decisiones técnicas
- Estrategia de multi-tenancy
- MVP y hoja de ruta

## Licencia
[Pendiente]
