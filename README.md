<div align="center">

# Cyber Deception Lab • Cowrie & ELK Stack
### *Asymmetric Threat Intelligence, SSH/Telnet Deception Horizon & SOC Telemetry Architecture*

[![Elastic Stack](https://img.shields.io/badge/SIEM-Elasticsearch_%7C_Logstash_%7C_Kibana-005571?style=for-the-badge&logo=elastic&logoColor=white)](https://www.elastic.co/)
[![Deception: Cowrie](https://img.shields.io/badge/Deception-Cowrie_Medium_Interaction-critical?style=for-the-badge)](https://github.com/cowrie/cowrie)
[![Docker Compose](https://img.shields.io/badge/Containerization-Docker_Compose_v2-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Ingestion: Filebeat](https://img.shields.io/badge/Ingestion-Filebeat_Pipeline-00BFB3?style=for-the-badge&logo=elasticstack&logoColor=white)](https://www.elastic.co/beats/filebeat)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

<p align="center">
  <b>An enterprise-grade cyber deception and security operations center (SOC) telemetry laboratory deploying containerized medium-interaction SSH/Telnet honeypots, automated Filebeat/Logstash pipelines, and real-time Kibana threat intelligence dashboards.</b>
</p>

---

</div>

## 1. Abstract & Deception Taxonomy

Adversary reconnaissance vectors are asymmetric: defenders must secure all ingress points, whereas attackers need locate only a single vulnerable interface. Cyber deception platforms invert this asymmetry by deploying decoy network endpoints that generate high-fidelity, zero-false-positive threat intelligence upon initial interaction.

**Cyber Deception Lab** operationalizes this paradigm:
- **Medium-Interaction Deception Engine (Cowrie):** Emulates an authentic POSIX shell environment over port `2222`, logging brute-force credential pairs, interactive terminal keystrokes, and downloaded binary artifacts into sandboxed storage.
- **Unified Ingestion Pipeline (Filebeat $\to$ Logstash):** Transforms raw JSON event streams with GeoIP enrichment, ASN identification, and timestamp normalization.
- **Elasticsearch Analytics & Kibana Visual Cockpit:** Indexes incident telemetry into structured Elasticsearch clusters, visualizing adversary geography, credential permutation heatmaps, and session playback graphs.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        ADVERSARY DECEPTION PERIMETER                   │
│   [Attacker Ingress: Port 2222] ──► [Cowrie POSIX Virtual Shell]       │
│   [Session Keystroke Telemetry] ──► [Structured JSON Audit Log]        │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│                  STREAM TRANSFORMATION & GEOIP ENRICHMENT              │
│       • Filebeat Harvest Pipeline with Backpressure Control            │
│       • Logstash Grok Filters & MaxMind GeoIP Database Resolution      │
│       • Asynchronous Bulk Ingestion into Elasticsearch Index Patterns  │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │
         ┌─────────────────────────┴─────────────────────────┐
         ▼                                                   ▼
┌──────────────────────────────────┐        ┌──────────────────────────────────┐
│      ELASTICSEARCH INDEX CLUSTER │        │       KIBANA SOC DASHBOARD       │
│   Time-Series B-Tree/LSM-Tree    │        │   Adversary Geographic Heatmap   │
│   Credential Frequency Arrays    │        │   Interactive Session Replay     │
└──────────────────────────────────┘        └──────────────────────────────────┘
```

---

## 2. Ingestion Pipeline & Geo-Spatial Normalization

### 2.1. Threat Density & Credential Entropy
Let $\mathcal{C} = \{(u_i, p_i)\}_{i=1}^M$ denote the sequence of captured credential combinations during a brute-force sweep. Adversary credential entropy is computed to isolate automated dictionary attacks from targeted reconnaissance:

$$H(\mathcal{C}) = -\sum_{k=1}^K P(c_k) \cdot \log_2 P(c_k)$$

Low entropy signatures indicate automated public botnet dictionaries (e.g., Mirai, DarkNexus variants), while high entropy pairs signal manual human infiltration.

### 2.2. Logstash Pipeline Filter Topology
```text
input {
  beats { port => 5044 }
}
filter {
  if [eventid] == "cowrie.login.success" or [eventid] == "cowrie.login.failed" {
    geoip {
      source => "src_ip"
      target => "geoip"
    }
  }
}
output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "cowrie-logstash-%{+YYYY.MM.dd}"
  }
}
```

---

## 3. Technology Stack & Multi-Tier Topology

| Layer | Technology | Architectural Specification |
| :--- | :--- | :--- |
| **Deception Core** | Cowrie Honeypot | Medium-interaction emulated SSH/Telnet server with TTY logging |
| **Shipper** | Elastic Filebeat | Lightweight log harvester with non-blocking kernel event notifications |
| **ETL Pipeline** | Logstash | Dynamic log enrichment, GeoIP lookup, and index routing |
| **Search Engine** | Elasticsearch 8.x | Distributed JSON document store with Lucene inverted indexes |
| **Visualization** | Kibana 8.x | SOC operations dashboard, geospatial maps, and threat heatmaps |
| **Orchestration** | Docker Compose v2 | Isolated bridge network with containerized health check dependencies |

---

## 4. Systems Directory Hierarchy

```text
Honeypot-con-Dashboard-de-Datos/
├── honeypot-lab/
│   ├── docker-compose.yml       # Multi-container orchestration (Cowrie, ELK, Filebeat)
│   ├── filebeat/                # Ingestion configuration and harvester definitions
│   ├── logstash/                # Pipeline filters, GeoIP mappings & output configurations
│   ├── kibana/                  # Pre-configured dashboard templates (NDJSON exports)
│   └── docs/                    # Deployment specifications, Linux tuning & lab reports
└── README.md                    # Primary architectural and operational reference
```

---

## 5. Deployment & Operational Lifecycle

### Prerequisites
- **Docker Engine**: $\ge 24.x$
- **Docker Compose**: $\ge 2.20$
- **Host Memory**: $\ge 4 \text{ GB RAM}$ (Allocated to Elasticsearch JVM heap)

### Linux Host Virtual Memory Configuration
Elasticsearch requires adequate `mmap` counts:
```bash
sudo sysctl -w vm.max_map_count=262144
```

### Stack Orchestration
```bash
# Clone the repository
git clone https://github.com/ExeDevCentral/Honeypot-con-Dashboard-de-Datos.git
cd Honeypot-con-Dashboard-de-Datos/honeypot-lab

# Initialize containerized multi-tier stack
docker compose up -d

# Verify operational status of all microservices
docker compose ps
```

### Service Endpoints & Telemetry Ingestion
- **Kibana SOC Analytics Cockpit:** `http://localhost:5601`
- **Cowrie Emulated SSH Traps:** `ssh -p 2222 root@localhost`
- **Elasticsearch Cluster Health:** `curl http://localhost:9200/_cluster/health`

---

## 6. License

Distributed under the **MIT License**. Created for defensive cybersecurity research, threat analysis, and SOC engineering education.

<div align="center">
  <sub>Engineered by <a href="https://github.com/ExeDevCentral">ExeDevCentral</a>.</sub>
</div>
