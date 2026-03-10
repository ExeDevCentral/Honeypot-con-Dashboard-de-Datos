# Honeypot Lab (Cowrie + ELK) — modo alumno

> Objetivo: levantar un **honeypot SSH** (Cowrie) que atraiga intentos de login/comandos, enviar esos logs a **ELK** (Elasticsearch + Logstash + Kibana) y visualizarlos en un dashboard.
>
> **Solo para laboratorio local y ataques éticos** sobre tu propia red/VM.

## 0) Requisitos (Windows 10/11)

- **Docker Desktop** (con WSL2 recomendado).
- 8 GB RAM mínimo (16 GB ideal).
- Puertos libres: `2222`, `5601`, `9200`, `5044`.

## 1) ¿Qué vas a construir?

- **Cowrie**: simula un servidor SSH “vulnerable”. No es un SSH real: registra intentos y comandos, y devuelve respuestas falsas.
- **Filebeat**: lee el archivo de logs JSON de Cowrie y los envía.
- **Logstash**: recibe los eventos (Beats), los normaliza y los manda a Elasticsearch.
- **Elasticsearch**: almacén/buscador de eventos.
- **Kibana**: UI para explorar y armar dashboards.

Flujo de datos:

`Ataque/SSH → Cowrie (cowrie.json) → Filebeat → Logstash → Elasticsearch → Kibana`

## 2) Arranque rápido

En PowerShell, parado en esta carpeta (`honeypot-lab/`):

```powershell
docker compose up -d
docker compose ps
```

Revisá logs si algo falla:

```powershell
docker compose logs -f --tail 200
```

## 3) Probá el honeypot (generar datos)

### Conexión SSH “trampa”

Desde tu misma PC:

```powershell
ssh -p 2222 root@localhost
```

- Usuario/clave: probá cosas típicas (ej: `root/root`, `admin/admin`, `test/1234`).
- Entrá comandos (aunque “no haga nada real”): `ls`, `pwd`, `cat /etc/passwd`, `wget ...`, etc.

## 4) Abrir Kibana y crear “Data View”

- Kibana: `http://localhost:5601`
- En Kibana:
  - **Stack Management → Data Views → Create data view**
  - Pattern: `cowrie-*`
  - Time field: `@timestamp`

## 5) Lecciones aprendidas + PDF

Plantilla:

- `docs/reporte-lecciones-aprendidas.md`

## 6) Apagar / resetear

```powershell
docker compose down
```

Para borrar datos (¡borra Elasticsearch + Cowrie logs!):

```powershell
docker compose down -v
```

