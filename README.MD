
---

````markdown
# Roboshop Docker

This repository contains the Docker setup for a microservices-based e-commerce application called **Roboshop**. Each service is containerized and can be orchestrated using **Docker Compose**.

---

## 1. Microservices Included

| Service     | Description |
|-------------|-------------|
| frontend    | Web UI (Nginx) |
| cart        | Shopping cart service |
| catalogue   | Product info service |
| user        | User accounts & auth |
| shipping    | Shipping management |
| payment     | Payment processing |
| mongodb     | DB for cart, catalogue, user |
| mysql       | DB for shipping |

---

## 2. Run the Project (Docker Compose)

### Prerequisites
- Docker  
- Docker Compose  

### Steps
```sh
# Clone repo
git clone https://github.com/gurunani/roboshop-docker.git
cd roboshop-docker

# Start all services
docker-compose up -d

# Check running containers
docker-compose ps

# Stop and remove all
docker-compose down
````

---

## 3. Useful Docker Commands

| Command                        | Usage                   |
| ------------------------------ | ----------------------- |
| `docker build -t <name:tag> .` | Build image             |
| `docker run <image>`           | Run container           |
| `docker ps`                    | List running containers |
| `docker stop <id>`             | Stop container          |
| `docker images`                | List images             |
| `docker rmi <id>`              | Remove image            |
| `docker logs <id>`             | View logs               |
| `docker push <image>`          | Push image to registry  |

---

## 4. Docker Volumes 

```yaml
services:
  mongodb:
    image: mongo
    volumes:
      - /data/mongodb:/data/db
```

---

## 5. Docker Networking Types

| Type             | Notes                                      |
| ---------------- | ------------------------------------------ |
| bridge (default) | Containers talk inside same network        |
| host             | Shares host network (fast but less secure) |
| none             | No network access                          |
| overlay          | Multi-host networking (Docker Swarm)       |

---

## 6. Build & Push All Images 

```bash
for i in $(ls -d */); do
  cd $i
  name=$(basename "$i")
  echo "Building $name:v1..."
  docker build -t 9eguru/$name:v1 .
  echo "Pushing $name:v1..."
  docker push 9eguru/$name:v1
  cd ..
done
```

---


