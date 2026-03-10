# Honeypot con Dashboard (Cowrie + ELK)

Laboratorio local para practicar **detección y análisis de logs estilo SOC**:

- **Honeypot SSH**: Cowrie (puerto `2222`)
- **Pipeline + Dashboard**: Elasticsearch + Logstash + Kibana

## Ejecutar (Windows + Docker Desktop)

```powershell
cd .\honeypot-lab
docker compose up -d
```

- Kibana: `http://localhost:5601`
- Probar honeypot:

```powershell
ssh -p 2222 root@localhost
```

## Documentación clave

- Guía principal: `honeypot-lab/README.md`
- Setup Docker Windows: `honeypot-lab/docs/setup-docker-windows.md`
- Dashboard base (qué paneles crear): `honeypot-lab/docs/dashboard-base-kibana.md`
- Importar dashboard (NDJSON): `honeypot-lab/kibana/IMPORTAR_DASHBOARD.md`
- Plantilla “lecciones aprendidas”: `honeypot-lab/docs/reporte-lecciones-aprendidas.md`

