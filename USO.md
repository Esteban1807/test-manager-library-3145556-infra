# Cómo levantar Manager Library

Este repositorio levanta los contenedores, la red, los volúmenes y un motor PostgreSQL **vacío**. No contiene DDL: el esquema lo crean las migraciones de la API al arrancar (constitución, artículo V).

## Requisitos

- Docker con Docker Compose.
- Los repositorios `api` y `app` clonados junto a este, con sus nombres originales. Si están en otra ruta, define `API_PATH` y `APP_PATH` en `.env`.

## Un solo comando

1. Copia `.env.example` a `.env` y completa los valores vacíos.
2. Ejecuta:

```bash
docker compose up --build
```

| Servicio | Dirección |
|---|---|
| Frontend | http://localhost:5173 |
| API | http://localhost:8000/api/v1 |
| Salud de la API | http://localhost:8000/api/v1/health |

## Evidencia de T-20: la base arranca vacía

Levanta solo el motor y lista las tablas del esquema `library`. No debe haber ninguna:

```bash
docker compose up -d db
docker compose exec db psql -U library -d library -c "\dt library.*"
```

Las tablas aparecen después de que arranca el servicio `api`, porque es quien corre las migraciones.

## Datos de demostración

Con `LIBRARY_DEMO_CLOCK=true` en `.env`, el sembrador del repositorio `tool` crea libros, socios, préstamos y tres vencidos a través de la API.

## Empezar de cero

```bash
docker compose down -v
```

Borra también los volúmenes: la base y las portadas.
