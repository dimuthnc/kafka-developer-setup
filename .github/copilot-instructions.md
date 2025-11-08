# GitHub Copilot – Project Instructions

## 📘 Project Overview
This repository provides a ready-to-use **local Kafka cluster setup** for developers using **Docker Compose** on macOS (including Apple Silicon with Colima).  
It runs three services:
1. **Zookeeper** – Wurstmeister image (lightweight, ARM-compatible)
2. **Kafka Broker** – Wurstmeister Kafka image
3. **Kafka UI** – ProvectusLabs Kafka UI dashboard

The environment is intended for:
- Local development and testing of event-driven microservices
- Reusable dev-tooling across projects
- Educational / open-source usage

## ⚙️ Core Components
- **docker-compose.yml** defines three services.
- **Kafka data** and **Zookeeper metadata** are persisted in `./data` (mapped to local host directories).
- Ports:
  - Kafka: `9092`
  - Zookeeper: `2181`
  - Kafka UI: `8080`
- Works natively on **Apple M1/M2** machines via **Colima** or Docker Engine CLI (no Docker Desktop required).

## 🧱 Directory Structure
```
.
├── docker-compose.yml
├── data/
│   ├── kafka/        # Kafka logs & topics (local volume)
│   └── zookeeper/    # Zookeeper snapshots & logs
└── .github/
    └── copilot-instructions.md
```

## 🧠 Copilot Guidance
When generating or modifying files, Copilot should:
- Keep Kafka advertised listeners as:
  ```
  KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://kafka:9092
  ```
- Always mount host volumes for persistence:
  ```
  ./data/kafka:/kafka-logs
  ./data/zookeeper:/data
  ./data/zookeeper/datalog:/datalog
  ```
- Avoid suggesting `localhost` for inter-container communication.
- Prefer ARM-compatible or multi-arch images (e.g., `wurstmeister` instead of Bitnami).
- Assume usage of `docker-compose` (legacy CLI), not `docker compose` plugin, for maximum portability.
- Recommend `docker-compose down` (without `-v`) when stopping to preserve data.

## 🧩 Common Developer Commands
```bash
docker-compose up -d
docker-compose down
docker ps
docker logs kafka -f
docker exec -it kafka bash
```

## 🚀 Future Enhancements
- Optionally support KRaft mode (Kafka without Zookeeper)
- Add Prometheus + Grafana for monitoring
- Include Makefile or CLI wrapper for common tasks

---
_Authored by Dimuth Menikgama — Public open-source utility for developers working with Kafka locally._
