# n8n-Docker-automation

1. You first need a Docker Compose file (docker-compose.yml) and Docker Desktop.

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


# Second thing you need is ollama. 
Download gpt-oss:20b model or any other open source model. 

Open a terminal in the folder in which your yml is located. And run this command.
'''
docker compose up
'''
This command tells Docker to start up the services within the Docker Compose file.

Open your Docker Desktop application, and you will notice a container named n8n-docker running. 
Click on the n8n-docker container. 
Click on the link for n8n localhost - http://localhost:5678⁠

This opens n8n locally on your computer.



<img width="1867" height="992" alt="image" src="https://github.com/user-attachments/assets/e92b2502-6a95-4726-bf04-c82d492543ce" />





 # Now Login/signup into n8n and add create a workflow.
 Add an AI agent node. If you have enough VRAM to run larger models, you can download and run gpt-oss:20b or gpt-oss:120b parameter model.

 <img width="1913" height="912" alt="image" src="https://github.com/user-attachments/assets/2460375a-3721-4f92-83d4-ac54f1a8a521" />


# Then you can add tools like Gmail and Google Sheets using the Google Cloud Console.


<img width="1797" height="677" alt="image" src="https://github.com/user-attachments/assets/3e410cb5-d03f-4162-8483-031f8a722086" />



Reference YouTube tutorial - https://www.youtube.com/watch?v=Myjo1amUZ08
How to connect Email to n8n - https://youtu.be/pWGXlZBGu4k?si=LfH6jnQt-bIBgvh7





