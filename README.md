# 🛠️ Multi-Service Project (Go + Python) with Docker & Nginx

This project consists of two backend services:

- `service1` – written in Go
- `service2` – written in Python (e.g., Flask or FastAPI)

Both services are containerized using Docker and exposed via an Nginx reverse proxy.

---

## 📦 Project Structure

```
Project/
├── docker-compose.yml
├── nginx/
│   └── default.conf
├── service_1/ (Go service)
│   ├── Dockerfile
│   ├── main.go
│   └── go.mod
└── service_2/ (Python service)
    ├── Dockerfile
    ├── app.py
    └── requirements.txt
```

---

## 🚀 Setup Instructions

### 🔧 Prerequisites

- Docker
- Docker Compose

### 🏁 Steps to Run

1. **Clone the repository** (or place all code as per the structure above)
2. Open terminal at the project root folder:
   ```bash
   cd path/to/Project
   ```

3. **(Important)** Make sure Dockerfiles are correct and specify:
   - `golang:1.24.4` for Go
   - `python:3.11-slim` for Python

4. **Build and run the containers**
   ```bash
   docker-compose build --no-cache
   docker-compose up
   ```

5. **Access the services:**

| Service     | URL                      |
|-------------|--------------------------|
| Go Service  | http://localhost/service1 |
| Python App  | http://localhost/service2 |

---

## 🔁 How Routing Works (via Nginx)

Nginx acts as a reverse proxy and routes traffic to both services:

### Example `nginx/default.conf`:

```nginx
server {
    listen 80;

    location /service1/ {
        proxy_pass http://service1:8001/;
    }

    location /service2/ {
        proxy_pass http://service2:8002/;
    }
}
```

This means:

- A request to `http://localhost/service1/` is routed to Go app (`localhost:8001`)
- A request to `http://localhost/service2/` is routed to Python app (`localhost:8002`)

Make sure the services inside the Docker Compose expose the correct ports:
- `service1` → `EXPOSE 8001` (in Go Dockerfile)
- `service2` → `EXPOSE 8002` (in Python Dockerfile or Flask/FastAPI app)

---

## 🐳 Docker Compose Example

```yaml
version: '3'

services:
  nginx:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - service1
      - service2

  service1:
    build: ./service_1
    expose:
      - "8001"

  service2:
    build: ./service_2
    expose:
      - "8002"
```

---

## 🧪 Testing

You can test your endpoints after running the app:

```bash
curl http://localhost/service1/
curl http://localhost/service2/
```

---

## ✅ Notes

- If you get Go version errors like `requires go >= 1.24.4`, ensure your Go image in Dockerfile is updated.
- Ensure `go mod init` and `go mod tidy` are run in your `service_1` folder before building Docker image.

