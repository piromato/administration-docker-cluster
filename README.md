# Administration: Neo4j in Docker

Supporting scripts for the GraphAcademy lab
[Running Neo4j in Docker](https://graphacademy.neo4j.com/courses/administration-docker-cluster/).

Open this repository in a [GitHub Codespace](https://github.com/codespaces) and
work through the lab from the terminal. Docker is already available.

## Contents

| Path | Purpose |
|---|---|
| `single-instance/docker-compose.yml` | A single Neo4j Enterprise container |
| `cluster/docker-compose.yml` | A cluster of 3 primaries and 2 secondaries |
| `cluster/docker-compose.broken.yml` | The same cluster without a secondaries count, so the secondaries stay `Free`. Used by the "Allocate the secondaries" lesson |
| `scripts/wait-for-cluster.sh` | Polls until all 5 members are `Enabled` |
| `.devcontainer/devcontainer.json` | Codespace definition with Docker |

## Run a single instance

```bash
docker compose -f single-instance/docker-compose.yml up -d
docker exec -it neo4j cypher-shell -u neo4j -p neo4j-password
```

Stop it before starting the cluster, because `core-1` reuses ports 7474/7687:

```bash
docker compose -f single-instance/docker-compose.yml down
```

## Run the cluster

```bash
docker compose -f cluster/docker-compose.yml up -d
bash scripts/wait-for-cluster.sh
docker exec -it core-1 cypher-shell -u neo4j -p neo4j-password
```

Tear it down when you are finished:

```bash
docker compose -f cluster/docker-compose.yml down
```

## Credentials

All containers use `neo4j` / `neo4j-password`.
