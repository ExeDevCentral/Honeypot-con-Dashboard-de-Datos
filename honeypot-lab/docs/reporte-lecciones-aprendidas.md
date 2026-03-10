# Reporte — Honeypot Cowrie + Dashboard (ELK)

## 1. Resumen ejecutivo (5–10 líneas)

- Qué se construyó:
- Qué se observó:
- Resultado general:

## 2. Alcance y reglas del laboratorio

- Entorno (local/VM/red):
- Qué NO se hizo (ética y seguridad):

## 3. Arquitectura (alto nivel)

`Atacante → Cowrie → Filebeat → Logstash → Elasticsearch → Kibana`

## 4. Configuración y ejecución

- OS/Hardware:
- Docker Desktop/WSL2:
- Comandos usados:
  - `docker compose up -d`
  - `docker compose logs -f`

## 5. Simulaciones realizadas (éticas)

| Fecha/Hora | Origen (IP) | Técnica | Herramienta | Resultado esperado | Resultado observado |
|-----------:|-------------|--------|-------------|--------------------|--------------------|
|            |             |        |             |                    |                    |

## 6. Hallazgos principales (con evidencias)

- Top IPs origen:
- Top usernames:
- Top passwords:
- Comandos más usados:
- Eventos más comunes (`event_id`):

## 7. Indicadores útiles para un SOC

- Volumen de intentos por minuto/hora/día
- IPs repetitivas vs IPs nuevas
- Diccionarios típicos (usuarios/contraseñas)
- Comandos por etapa (recon → descarga → persistencia)

## 8. Lecciones aprendidas

- Qué fue lo más difícil:
- Qué harías diferente:
- Qué mejorarías para “producción”:

## 9. Próximos pasos (S-Tier)

- Alertas en Kibana por picos
- Enriquecimiento GeoIP/ASN en Logstash
- Enviar a SIEM (Splunk Free) o duplicar a Grafana/Loki

