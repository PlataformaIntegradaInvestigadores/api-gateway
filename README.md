# api-gateway

Gateway central (nginx) que enruta `/api`, `/ws` y `/media` a los backends de la red `centinela-net`.

## Levantar local (Docker)

```bash
docker compose up -d --build
```

Healthcheck: `curl -f http://localhost:8080/health`

## Rutas (ver `nginx.conf`)

Esquema `/api/<servicio>/`:

| Ruta | Upstream |
|------|----------|
| `/api/identity/` | `profile-identity-web:8002` |
| `/api/social/` | `social-consensus-web:8000` |
| `/api/search/` | `search-engine-backend:8001` |
| `/api/search/v2/` | `search-microservice-backend:8002` |
| `/api/predictive/` | `predictive-backend:8003` |
| `/api/rag/` | `centinela-rag:8181` |
| `/ws/` | `social-consensus-web:8000` (WebSocket) |
| `/media/` | `social-consensus-web:8000` / `profile-identity-web:8002` |

## Notas

- Auth vía `auth_request /_auth_identity` → identity `/internal/auth/validate-token/`.
- Usa `resolver 127.0.0.11` (DNS de Docker) → tolera orden de arranque.
- El frontend (`frontend-app`) proxea `/api/`, `/ws/` y `/media/` aquí; el gateway es el punto central.
