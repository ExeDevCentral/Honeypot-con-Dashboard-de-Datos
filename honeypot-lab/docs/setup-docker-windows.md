# Setup rápido: Docker Desktop en Windows (para este lab)

## 1) Verificación (qué te está pasando ahora)

Si ves un error como:

- `failed to connect to the docker API at npipe:////./pipe/dockerDesktopLinuxEngine`

Significa: **Docker Desktop (Engine) no está ejecutándose**.

## 2) Solución (paso a paso)

1) Abrí **Docker Desktop** (Inicio → Docker Desktop).
2) Esperá a que diga **“Docker Desktop is running”**.
3) En Docker Desktop:
   - Settings → **General**: “Use the WSL 2 based engine” (recomendado)
   - Settings → **Resources → WSL Integration**: habilitá tu distro si usás WSL
4) Volvé a PowerShell y probá:

```powershell
docker version
docker info
```

`docker info` tiene que mostrar datos del servidor (no error).

## 3) Reintentar levantar el lab

```powershell
cd C:\Users\exeme\Desktop\ciberSecurty\honeypot-lab
docker compose up -d
docker compose ps
```

Si algo falla:

```powershell
docker compose logs -f --tail 200
```

## 4) Nota de recursos

ELK consume RAM. Si se te cuelga:

- Cerrá apps pesadas
- Bajá Java heap en `docker-compose.yml` (ES y Logstash)
- O corré solo Cowrie primero (y ELK después)

