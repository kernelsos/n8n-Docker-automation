# n8n-Docker-automation

1. The first thing you need is a Docker Compose file (docker-compose.yml)

# Top-level

version: '3.8' → Compose spec version.
volumes → persistent storage definitions:
    n8n_oss_storage: stores n8n configs/workflows.
    postgres_oss_storage: stores Postgres database files.
networks → custom Docker network oss_demo so containers can talk to each other securely.

Services
1. postgres-oss
Runs Postgres using image postgres:16-alpine.

Joins oss_demo network.

Restarts unless you stop it.

Sets environment variables:

POSTGRES_USER: n8n

POSTGRES_PASSWORD: n8n

POSTGRES_DB: n8n

Mounts postgres_oss_storage → persists database data inside container path /var/lib/postgresql/data.

2. n8n-oss

Runs n8n using image n8nio/n8n:latest.

Joins oss_demo network.

Exposes port 5678 → you can open http://localhost:5678 in the browser.

Restarts unless stopped.

Environment variables configure DB connection:
  DB_TYPE=postgresdb
  DB_POSTGRESDB_HOST=postgres-oss (links to Postgres service)
  DB_POSTGRESDB_PORT=5432
  DB_POSTGRESDB_DATABASE=n8n
  DB_POSTGRESDB_USER=n8n
  DB_POSTGRESDB_PASSWORD=n8n
  N8N_ENCRYPTION_KEY=a_secure_encryption_key (used to encrypt sensitive credentials inside n8n).
Mounts n8n_oss_storage to /home/node/.n8n → saves workflows, creds, and settings persistently.

When you run docker-compose up -d:

Postgres database container launches first.
n8n container launches, connects to Postgres, and stores workflows/credentials.
Data and configs are preserved across restarts using volumes.
