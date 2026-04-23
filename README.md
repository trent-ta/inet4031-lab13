# Docker Lab: Containerizing a Three-Tier Application
**INET 4031 - Introductions to Systems**

This lab introduces Docker and Docker Compose by having you containerize a
real, multi-service application. You will package three components: Apache,
Flask, and MariaDB. These will be packaged into separate containers and wired together so they function as a complete application.

# Project Overview

This application demonstrates a simple ticket system that allows users to view and create ticket through a web interface. For the frontend, it is served by Apache, while the backend API is handled through Flask. All of the ticket data is stored in a MariaDb database.

# Prerequisites

Before running this lab, the following must be set up:
- Docker installed on the VM (installed through VS Code SSH terminal)
- Docker Compose installed
- Git installed
- Linux-based environment (Ubuntu VM)
- Port 80 must be free (Stop Apache on the VM is needed)

# Getting Started

1. Clone the repository:

```bash
git clone https://github.com/Programming-Mellow/inet4031-testlab12.git
cd inet4031-testlab12
```
2. Install Docker by following the guide:https: //docs.docker.com/engine/install/ubuntu/

3. Afterwards, verify that Docker and Docker Compose are installed on your VM:

```bash
docker --version
docker compose version
```
4. Create the environment file:

```bash
cp .env.example .env
```
5. Complete the Dockerfiles

In app/Dockerfile, the missing lines are:

```dockerfile
COPY app.py .
CMD ["python", "app.p
```
In apache/Dockerfile, the missing lines are:

```dockerfile
COPY app.py .
CMD ["python", "app.p
```
6. Build and start the containers:

```bash
sudo docker compose up --build
```
7. Open a new terminal and check container status:

```bash
sudo docker compose ps
```

The db and app containers should show healthy, and the web container should show running.
- db = healthy
- app = healthy
- web = running
# Configuration

The .env file stores important settings like database usernames and passwords. It is not included in the repository for security reasons.

Each person needs to create their own .env file by copying the template:

```bash
cp .env.example .env
```
# Verification

1. Build and start the containers:

```bash
sudo docker compose up --build
```
2. Open a new terminal and check container status:

```bash
sudo docker compose ps
```
The db and app containers should show healthy, and the web container should show running.

3. Next, open a browser and type:

VM's IP address

You should see the Ticket System page with a green “API healthy” indicator and at least one ticket.

4. The Flask health endpoint is reachable through Apache:

```bash
curl http://localhost:80/health
```
Expected: {"database": "connected","status": "healthy"}

5. Create a ticket

```bash
curl -X POST http://localhost:80/api/tickets \
  -H "Content-Type: application/json" \
  -d '{"title": "My first ticket", "description": "Testing the API"}'
```
6. Test data persistence:

```bash
sudo docker compose stop db
sudo docker compose start db
```
then:

```bash
curl http://localhost:80/api/tickets
```
Your ticket should still be there.

7. Run the Check Script

```bash
chmod +x check-lab.sh
sudo ./check-lab.sh
```
The script will run through each condition and print PASS or FAIL with a hint for anything that fails.
