# Sistema de Gestión Bibliotecaria (SGB) – Universidad de Madeira

Proyecto Final – Desarrollo de Software II

## Repositorios
Este es el **repositorio principal**. Une los dos repositorios del sistema (como submódulos de Git) y contiene el `docker-compose.yml` que levanta todo junto.

| Repositorio | Contenido | Despliegue |
|---|---|---|
| [desarrollo_Software_II](https://github.com/kevin9897/desarrollo_Software_II) | docker-compose, documentación | – |
| [sgb-backend](https://github.com/kevin9897/sgb-backend) | API Python (FastAPI + SQLAlchemy) | Render (Docker + PostgreSQL) |
| [sgb-frontend](https://github.com/kevin9897/sgb-frontend) | React + Vite, HTML, CSS | Vercel |

## Arquitectura
Arquitectura **por capas (N-Tier, 3 capas)**:
Presentación (React) → Lógica de negocio (FastAPI) → Datos (PostgreSQL).
Dentro del backend: `routers` → `services` → `models`.

## Clonar el proyecto completo
```bash
git clone --recurse-submodules https://github.com/kevin9897/desarrollo_Software_II.git
cd desarrollo_Software_II
```
Si ya lo clonaste sin `--recurse-submodules`:
```bash
git submodule update --init --recursive
```

## Ejecución local
```bash
cp backend/.env.example backend/.env
docker compose up --build
```
- Frontend: http://localhost:5173
- API / Swagger: http://localhost:8000/docs

## Trabajar con los submódulos
Cada carpeta (`backend/`, `frontend/`) es un repositorio independiente:
```bash
cd backend
git add . && git commit -m "mensaje" && git push   # sube al repo sgb-backend
cd ..
git add backend && git commit -m "Actualiza backend" && git push   # registra la nueva versión en el principal
```
Traer los últimos cambios de todo:
```bash
git pull
git submodule update --remote --merge
```
