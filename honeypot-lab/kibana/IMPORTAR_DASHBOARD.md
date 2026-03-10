# Importar dashboard en Kibana (1 click)

## 1) Abrí Kibana

`http://localhost:5601`

## 2) Importar Saved Objects

1) **Stack Management → Saved Objects**
2) Click **Import**
3) Seleccioná el archivo:
   - `honeypot-lab/kibana/cowrie-dashboard.ndjson`
4) Confirmá el import.

## 3) Abrir el dashboard

Andá a **Dashboard** y buscá:

- **Cowrie — Overview**

## Nota

Este export incluye:

- Data View: `cowrie-*` (campo tiempo `@timestamp`)
- Dashboard base (sin “Lens” pesado para que sea 100% importable en cualquier Kibana 8.x)

Luego podés agregar los paneles Lens siguiendo `docs/dashboard-base-kibana.md`.

