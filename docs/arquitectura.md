# Arquitectura de Software – SGB Universidad de Madeira

## Segundo Momento Evaluativo: Layered Architecture (N-Tier)
Basado en el libro ***Fundamentals of Software Architecture*** (Mark Richards & Neal Ford, Cap. 10: *Layered Architecture Style*):

- **Estilo Arquitectónico:** Layered Architecture (N-Tier) con **Particionamiento Técnico (*Technical Partitioning*)**.
- **Topología y Aislamiento (*Layers of Isolation*):** Todas las capas están marcadas como **`[CLOSED]` (Capas Cerradas)**. Ninguna capa puede saltarse a otra (el Frontend jamás se conecta directamente a PostgreSQL; toda petición atraviesa estrictamente `Presentation Layer -> Business Layer -> Persistence Layer -> Database Layer`).
- **Protocolo de Comunicación:** El cliente (React SPA) y el servidor (FastAPI) se comunican exclusivamente mediante **peticiones HTTP REST transmitiendo y recibiendo JSON** (el servidor nunca devuelve HTML).

## Diagrama de Arquitectura 100% en Mermaid (Fundamentals of Software Architecture)

```mermaid
flowchart TB
    ACTOR["Actores: Estudiante / Profesor / Bibliotecario"]

    subgraph TOPOLOGY["ESTILO: LAYERED ARCHITECTURE (N-TIER) - TECHNICAL PARTITIONING"]
        direction TB

        subgraph L1["1. PRESENTATION LAYER [CLOSED] (UI + API Controllers)"]
            direction LR
            subgraph REPO_FE["Repositorio: sgb-frontend (React + Vite)"]
                UI_VIEW["Componentes y Vistas React (src/App.jsx)"]
                HTTP_CLIENT["Cliente HTTP Axios (src/api.js)"]
                UI_VIEW --> HTTP_CLIENT
            end
            subgraph CTRL_BOX["Repositorio: sgb-backend (Controladores HTTP)"]
                ROUTERS["FastAPI Routers / Controladores: auth, usuarios, recursos, prestamos, reservas, multas"]
                SCHEMAS["Contratos DTOs Pydantic (app/schemas/*.py)"]
                ROUTERS --> SCHEMAS
            end
            HTTP_CLIENT <-->|"Peticiones HTTP REST (Solo JSON, sin HTML)"| ROUTERS
        end

        subgraph L2["2. BUSINESS LAYER [CLOSED] (Servicios / Casos de Uso / Reglas de Dominio)"]
            direction LR
            SERVICES["Servicios de Negocio (app/services/*.py): RecursoService, PrestamoService, ReservaService, MultaService, UsuarioService"]
            RULES["Reglas de Dominio (app/constants.py, app/utils.py, app/jobs.py): Dias prestamo, Renovaciones, Multas dias habiles, Perdidos 120d, Cola reservas 5d"]
            SERVICES --> RULES
        end

        subgraph L3["3. PERSISTENCE LAYER [CLOSED] (Repositorios / DAO / ORM)"]
            direction LR
            REPOS["Repositorios / DAO (app/repositories/*.py): UsuarioRepository, RecursoRepository, PrestamoRepository, MultaRepository"]
            ORM["Modelos ORM SQLAlchemy (app/models/*.py): Usuario, Recurso, Prestamo, Reserva, Renovacion, Multa"]
            REPOS --> ORM
        end

        subgraph L4["4. DATABASE LAYER [CLOSED] (Almacenamiento Relacional)"]
            DB[("PostgreSQL 16 (sgb_madeira): 63 Usuarios + 97 Recursos Bibliograficos")]
        end

        ACTOR -->|"Navegador Web"| UI_VIEW
        ROUTERS -->|"Llamada sincrona (Caso de Uso)"| SERVICES
        SERVICES -->|"Abstraccion de datos (DAO)"| REPOS
        ORM -->|"SQL via TCP :5432"| DB
    end
```
