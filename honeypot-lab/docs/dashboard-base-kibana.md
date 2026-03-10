# Dashboard base (Kibana) — Cowrie (SOC-style)

## 0) Preparación (una sola vez)

1) Abrí Kibana: `http://localhost:5601`
2) **Stack Management → Data Views → Create data view**
   - **Name**: `Cowrie`
   - **Index pattern**: `cowrie-*`
   - **Time field**: `@timestamp`

Si no ves datos, generá eventos con:

```powershell
ssh -p 2222 -o BatchMode=yes -o StrictHostKeyChecking=no -o UserKnownHostsFile=NUL root@localhost
```

## 1) Dashboard “Cowrie — Overview”

Crealo en **Dashboard → Create dashboard → Create visualization** y agregá estos paneles.

### Panel A — Eventos por minuto (última hora)

- Tipo: Lens → Line chart
- Métrica: Count
- X-axis: Date histogram sobre `@timestamp`

### Panel B — Top IPs origen

- Tipo: Lens → Bar horizontal
- Métrica: Count
- Break down: Top values `src_ip` (Top 10)

### Panel C — Eventos por tipo

- Tipo: Lens → Donut (o Bar)
- Métrica: Count
- Slice/Break down: Top values `event_id` (Top 10)

### Panel D — Sesiones únicas

- Tipo: Lens → Metric
- Métrica: Unique count (cardinality) de `session`

### Panel E — Usernames probados (cuando existan)

- Tipo: Lens → Data table
- Rows: Top values `username` (Top 20)
- Métrica: Count
- Filtro KQL del panel:

```text
username : *
```

### Panel F — Passwords probadas (cuando existan)

- Tipo: Lens → Data table
- Rows: Top values `password` (Top 20)
- Métrica: Count
- Filtro KQL del panel:

```text
password : *
```

## 2) Filtros KQL útiles

- Solo SSH:

```text
protocol : "ssh"
```

- Cierres de sesión (ruido típico):

```text
event_id : "cowrie.session.closed"
```

- Investigar una IP:

```text
src_ip : "172.18.0.1"
```

- Timeline de una sesión:

```text
session : "TU_SESSION_ID"
```

## 3) Cómo contarlo en entrevista (30–45s)

- “Monté un honeypot SSH con Cowrie y lo conecté a un pipeline ELK para observabilidad.”
- “Simulé intentos en un lab local y armé un dashboard tipo SOC: volumen, top IPs, tipos de evento y sesiones.”
- “Con eso practico triage e investigación basada en logs, no teoría.”

