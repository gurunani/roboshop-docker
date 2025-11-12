
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

Got it — you want **everything**:
✅ Table
✅ Bullet list version
✅ Examples/snippets
✅ Collapsible sections (`<details>` style)
✅ Still clean and README-friendly

Here is the **complete `.md` block** you can paste directly:

````markdown
## 8. Docker Best Practices (Important)

### 📌 Summary Table

| Practice | Why It Matters |
|----------|----------------|
| Minimum & Official Images | Smaller, safer, verified base images |
| Multi-Stage Builds | Build once, ship lightweight runtime image |
| Optimising Layers | Combine RUN lines → faster + smaller images |
| Non-Root Containers | Prevents privilege escalations |
| Custom Docker Networks | Service discovery + isolation |
| Implementing Volumes | Data survives container deletion |
| COPY over ADD | COPY is safer and predictable |
| .dockerignore | Prevents bloated image builds |
| Health Checks | Auto-restart unhealthy containers |
| Limiting Resources | Stops containers from hogging CPU/RAM |
| Secrets Manager | No passwords inside images or env files |
| Volumes (Again) | Required for DBs, logs, uploads |

---

### 🔹 Bullet Version (Quick Notes)

- Use **official & minimal images** like `alpine`, `debian:slim`
- Build production images using **multi-stage builds**
- **Combine RUN instructions** to reduce layers
- Always run apps as a **non-root user**
- Use **custom networks** for microservices
- Use **volumes** for DBs, logs, uploads
- Prefer `COPY` instead of `ADD`
- Add a `.dockerignore` to skip junk files during build
- Add **HEALTHCHECK** so Docker knows when app is broken
- Limit CPU & RAM using `--cpus` and `--memory`
- Store passwords & keys in **cloud secret managers**, not ENV
- Re-mention: volumes are **mandatory** for stateful containers

---

### 🔍 Expanded (With Examples)

<details>
<summary><b>1. Minimum & Official Base Images</b></summary>

```dockerfile
FROM python:3.12-slim   # ✅ small, official, secure
# instead of: FROM ubuntu:latest  # ❌ huge, unnecessary
````

</details>

<details>
<summary><b>2. Multi-Stage Builds</b></summary>

```dockerfile
FROM golang:1.21 as builder
WORKDIR /app
RUN go build -o app main.go

FROM alpine
COPY --from=builder /app/app /app
CMD ["/app"]
```

✅ Final image ~10MB instead of 800MB

</details>

<details>
<summary><b>3. Optimising Layers</b></summary>

```dockerfile
# Bad (creates 3 layers)
RUN apt update
RUN apt install -y curl
RUN apt clean

# Good (1 layer)
RUN apt update && apt install -y curl && apt clean
```

</details>

<details>
<summary><b>4. Non-Root Containers</b></summary>

```dockerfile
RUN adduser -D appuser
USER appuser
```

</details>

<details>
<summary><b>5. Custom Docker Networks</b></summary>

```sh
docker network create roboshop
docker run --network=roboshop cart
docker run --network=roboshop user
```

</details>

<details>
<summary><b>6. Implementing Volumes</b></summary>

```yaml
services:
  mongodb:
    image: mongo
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

</details>

<details>
<summary><b>7. COPY over ADD</b></summary>

```dockerfile
COPY app.py /app/  # ✅
ADD app.py /app/   # ❌ same result, no need for ADD
```

</details>

<details>
<summary><b>8. .dockerignore</b></summary>

```
.git
node_modules
*.log
.env
```

⏳ Speeds up builds, avoids bloated images

</details>

<details>
<summary><b>9. Health Checks</b></summary>

```dockerfile
HEALTHCHECK CMD curl -f http://localhost:8080/health || exit 1
```

</details>

<details>
<summary><b>10. Limiting Resources</b></summary>

```sh
docker run --cpus=1 --memory=512m app
```

</details>

<details>
<summary><b>11. Secrets from Secrets Manager</b></summary>

✅ Use AWS Secrets Manager, Vault, GCP Secret Manager
❌ Never commit `.env` with passwords

Example (AWS CLI):

```sh
aws secretsmanager get-secret-value --secret-id db_password
```

</details>

<details>
<summary><b>12. Volumes (Reminder)</b></summary>

Containers are **temporary**. Data is **not**.

No volume = data gone after `docker rm`

</details>

---

```


## 1. Minimum & Official Images
Use the smallest official base images (`alpine`, `slim`) to reduce image size and improve security.

## 2. Multi-Stage Builds
Build dependencies in one stage and copy only the required output into a lightweight runtime stage.

## 3. Optimizing Layers / Combining RUN
Each instruction creates a layer. Combine `RUN` commands to reduce layers and shrink the final image.

## 4. Non-Root Containers
Run your container as a normal (non-root) user to improve security and avoid privilege issues.

## 5. Customised Networks
Use custom Docker networks so containers communicate privately and securely.

## 6. Implementing Volumes
Store important data in volumes so it persists even if the container stops or is removed.

## 7. COPY Over ADD
Use `COPY` for simple file transfers. Avoid `ADD` unless you need its special features (like auto-extract).

## 8. .dockerignore
Ignore unnecessary files (logs, node_modules, secrets) during build to reduce context size and prevent leaks.

## 9. Implementing Health Checks
Use `HEALTHCHECK` to let Docker detect broken containers and restart them automatically.

## 10. Limiting Resources
Set CPU and memory limits so no single container can overload the host.

## 11. Getting Secrets from Secrets Manager
Never hardcode secrets in images. Fetch them at runtime from a secrets manager (AWS, Vault, Docker secrets).

---



