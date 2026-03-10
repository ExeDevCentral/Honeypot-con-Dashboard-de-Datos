# Setup Linux (Ubuntu/Debian/Kali) — Docker + ELK + Cowrie

## 1) Requisitos

- Linux 64-bit
- Docker Engine instalado
- Docker Compose v2 (plugin) instalado
- 8 GB RAM mínimo (16 GB ideal)

## 2) Pre-requisito típico de Elasticsearch (Linux)

Elasticsearch en contenedor necesita este sysctl:

```bash
sudo sysctl -w vm.max_map_count=262144
```

Persistirlo:

```bash
echo 'vm.max_map_count=262144' | sudo tee /etc/sysctl.d/99-elasticsearch.conf
sudo sysctl --system
```

## 3) Levantar el laboratorio

Desde la raíz del repo:

```bash
cd honeypot-lab
docker compose up -d
docker compose ps
```

Logs si algo falla:

```bash
docker compose logs -f --tail 200
```

## 4) URLs y puertos

- Kibana: `http://localhost:5601`
- Elasticsearch: `http://localhost:9200`
- Honeypot SSH (Cowrie): `localhost:2222`

## 5) Generar eventos (ético, local)

```bash
ssh -p 2222 root@localhost
```

Si querés solo “generar intentos” rápido:

```bash
ssh -p 2222 -o BatchMode=yes -o StrictHostKeyChecking=no root@localhost || true
```

## 6) Apagar / resetear

Apagar:

```bash
docker compose down
```

Reset total (borra datos):

```bash
docker compose down -v
```

