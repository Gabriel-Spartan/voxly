# Arquitectura de Voxly

## Visión General
Voxly está construido siguiendo los principios de la **Arquitectura Onion** (también conocida como Arquitectura 
Hexagonal o Puertos y Adaptadores). Esta arquitectura se caracteriza por tener una clara separación de responsabilidades
y un fuerte aislamiento del dominio, lo que facilita el mantenimiento, las pruebas y la escalabilidad.

La regla fundamental es que las dependencias siempre apuntan hacia adentro: las capas externas pueden depender de las
internas, pero nunca al revés.

## Capas de la Arquitectura
### 1. Domain (Núcleo)
- **Ubicación:** `Voxly.Domain`
- **Responsabilidad:** Contiene las entidades del negocio, objetos de valor, enumeraciones y las interfaces de 
repositorio (contratos) que definen las operaciones de persistencia necesarias para el dominio.
- **Reglas:**
  - Depende únicamente de la capa `Domain`.
  - No debe tener lógica de infraestructura (como acceso a base de datos o llamadas HTTP).
- **Ejemplos en Voxly:**
  - Servicios: `CreateClientService`, `RegisterSaleService`
  - DTOs: `ClientDto`, `SaleDto`
  - Interfaces de servicios que serán implementados en capas superiores.
### 2. Application
- **Ubicación:** `Voxly.Application`
- **Responsabilidad:** Orquesta los casos de uso de la aplicación. Contiene servicios de aplicación, DTOs (Data
Transfer Objects), mapeadores y lógica de coordinación.
- **Reglas:**
  - Depende únicamente de la capa `Domain`.
  - No debe tener lógica de infraestructura (como acceso a base de datos o llamadas HTTP).
- **Ejemplos en Voxly:**
  - Servicios: `CreateClienteService`, `RegisterSaleService`
  - DTOs: `ClientDto`, `SaleDto`
  - Interfaces de servicios que serán implementados en capas superiores.
### 3. Infrastructure
- **Ubicación:** `Voxly.Infrastructure`
- **Responsabilidad:** Implementa los contratos definidos en `Domain` y `Application`. Aquí residen los repositorios
concretos (Entity Framework), el `DbContext`, el envío de correos, la lógica de almacenamiento, etc.
- **Reglas:**
  - Depende de `Application` (y por ende de `Domain`).
  - No debe exponer detalles de infraestructura a otras capas.
- **Ejemplos en Voxly:**
  - Repositorios: `ClientRepository` (implementa `IClientRepository`).
  - Implementación de servicios de aplicación: `EmailService`.
  - Configuración de Entity Framework y migraciones.
### 4. API (Presentation)
- **Ubicación:** `Voxly.API`
- **Responsabilidad:** Expone la funcionalidad a través de endpoints HTTP (REST). Maneja la autenticación, autorización,
validación de entrada y formatea las respuestas.
- **Reglas:**
  - Depende de `Infrastructure` y `Application`.
  - No debe contener lógica de negocio; solo actúa como puerta de entrada.
- **Ejemplos en Voxly:**
  - Controladores: `ClientsController`, `SalesController`
  - Middlewares (manejo de excepciones, JWT, tenancy)
  - Program.cs (configuración de servicios y pipeline)

## Flujo de Dependencias
```markdown
Domain (Entidades, Interfaces de repositorio)
        ↑
Application (Casos de uso, DTOs)
        ↑
Infrastructure (Repositorios concretos, DbContext)
        ↑
API (Controladores, Middlewares)
```

## Beneficios para Voxly
- **Mantenibilidad:** Los cambios en una capa no afectan a las demás. 
- **Testabilidad:** El dominio y la aplicación pueden probarse de forma aislada (unit tests) sin necesidad de 
infraestructura real.
- **Flexibilidad tecnológica:** Podemos cambiar EF Core por Dapper, o SQL Server por PostgreSQL, modificando solo la 
capa de Infrastructure.
- **Claridad conceptual:** Cada desarrollador entiende rápidamente dónde debe colocar cada pieza de código.

## Gestión de Multi-tenancy (Resumen)
La estrategia de tenencia (shared database – separate schemas) se implementa principalmente en la capa de 
Infrastructure, dentro del DbContext. El esquema activo se determina a partir del tenant extraído del JWT del usuario
autenticado. Este aspecto se detalla en [multi-tenancy.md](docs/multi-tenancy.md).

## Decisiones Técnicas Clave (Relacionadas con la Arquitectura)
- **Separación de migraciones:** Las migraciones de EF Core se mantienen en proyectos independientes 
(Voxly.Infrastructure.Migrations.SqlServer, etc.) para permitir múltiples proveedores de base de datos.
- **Inyección de dependencias:** Toda la configuración de servicios se realiza en Program.cs de la API, utilizando 
- contenedor nativo de .NET.


    


