# Deployment Guide

This guide describes production deployment topologies, Docker configurations, and scaling recommendations for the AI Customer Service Agent.

---

## 🐳 Docker Deployment

The project contains a [Dockerfile](file:///c:/Users/hp/OneDrive/Desktop/Code/AI-Customer-Support-Agent/Dockerfile) to build self-contained, reproducible container packages.

### 1. Build the Docker Image
From the root workspace directory, run:
```bash
docker build -t ai-customer-agent:latest .
```

### 2. Run the Container
Expose the FastAPI default port `8000` and map your local environment credentials:
```bash
docker run -d \
  -p 8000:8000 \
  --name customer-agent-service \
  -e OPENAI_API_KEY="your-api-key-here" \
  -e DEFAULT_MODEL="gpt-4o" \
  -e ENABLE_EVALUATION="true" \
  -e ENABLE_SENTIMENT_ANALYSIS="true" \
  ai-customer-agent:latest
```

---

## 🏗️ Production Topology

### 1. Reverse Proxy & SSL (Nginx)
In production, place FastAPI behind a reverse proxy (e.g. Nginx, Traefik) to handle SSL termination, load balancing, and rate limiting:
```nginx
server {
    listen 443 ssl;
    server_name agent.yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 2. Scaling (Gunicorn/Uvicorn Workers)
The default FastAPI start script in the Dockerfile launches Uvicorn. For production workloads with high concurrency, use `gunicorn` with multiple `uvicorn` workers:
```bash
pip install gunicorn
gunicorn -w 4 -k uvicorn.workers.UvicornWorker src.customer_service_agent.api:app --bind 0.0.0.0:8000
```

---

## 📁 Persistence & Logs Volume Mounts

The agent outputs transaction logs locally (`customer_agent.log`). To prevent log data loss when a container restarts, mount a persistent volume from your host to the container:
```bash
docker run -d \
  -p 8000:8000 \
  -v /var/log/customer-agent:/app/logs \
  -e LOG_FILE="/app/logs/customer_agent.log" \
  -e OPENAI_API_KEY="your-api-key-here" \
  ai-customer-agent:latest
```
