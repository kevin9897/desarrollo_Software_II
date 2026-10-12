# Sistema de Gestión Bibliotecaria (SGB) – Universidad de Madeira

Proyecto Final – Desarrollo de Software II

## Stack
| Capa | Tecnología | Despliegue |
|------|------------|------------|
| Presentación | React + Vite, HTML, CSS | Vercel |
| Lógica de negocio | Python (FastAPI + SQLAlchemy) | Render (Docker) |
| Datos | PostgreSQL | Render |
| Local | Docker Compose | – |

## Arquitectura
Arquitectura **por capas (N-Tier, 3 capas)**. El backend es un monolito organizado en capas:
`routers` (HTTP) → `services` (reglas de negocio) → `models` (BD).

## Estructura
```
backend/
  app/
    main.py          punto de entrada FastAPI (registra routers, CORS, jobs)
    config.py        variables de entorno
    database.py      conexión a PostgreSQL
    security.py      login / JWT / roles
    constants.py     días de préstamo, renovaciones máximas, tabla de multas
    jobs.py          tareas automáticas (perdidos >120 días, vencer reservas, multas)
    utils.py         cálculo de días hábiles y fechas
    models/          tablas de la BD
      usuario.py       estudiante, profesor, bibliotecario
      recurso.py       recursos de las 6 colecciones + ejemplares + estados
      prestamo.py      préstamos, reservas y renovaciones
      multa.py         multas
    schemas/         validación de datos de entrada/salida (Pydantic)
    services/        reglas de negocio
    routers/         endpoints REST (/api/v1/...)
  seed.py          datos iniciales (50 estudiantes, 10 profesores, 3 bibliotecarios, colecciones)
  tests/           pruebas de reglas de negocio

frontend/
  src/
    main.jsx         punto de entrada React
    App.jsx          rutas de la aplicación
    api.js           llamadas HTTP al backend
    AuthContext.jsx  sesión del usuario
    index.css        estilos globales
    components/      componentes reutilizables (Navbar, Table, Modal, EstadoBadge)
    pages/           vistas (Login, Dashboard, Recursos, Usuarios, Préstamos, Reservas, Multas)

docs/arquitectura.md
docker-compose.yml   entorno local completo
render.yaml          despliegue backend + BD en Render
```

## Ejecución local
```bash
copy backend\.env.example backend\.env
docker compose up --build
```
- Frontend: http://localhost:5173
- API / Swagger: http://localhost:8000/docs
