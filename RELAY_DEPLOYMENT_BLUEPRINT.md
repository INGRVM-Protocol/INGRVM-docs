# INGRVM Circuit Relay: Deployment Blueprint
**Objective:** Deploy a permanent backup relay for the mesh on a cloud instance (VPS).

## 1. The Dockerfile
```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Install Python requirements
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy core logic
COPY INGRVM/Core /app/INGRVM/Core

# Environment variables
ENV PYTHONPATH=/app/INGRVM/Core
ENV INGRVM_RELAY_PORT=60000

# Expose the Relay port
EXPOSE 60000/tcp

CMD ["python", "INGRVM/Core/tools/run_circuit_relay.py"]
```

## 2. Requirements
```text
trio
requests
pydantic
msgpack
cryptography
```

## 3. Deployment Command
Run this on your VPS to launch the permanent mesh bridge:
```bash
docker build -t ingrvm-relay .
docker run -d --name ingrvm-relay -p 60000:60000 ingrvm-relay
```

## 4. Why this matters
The local Hub on your PC is the "Brain," but if your home internet flickers, the whole mesh loses coordination. By deploying this relay to the cloud (Task #13), you ensure that the Laptop and Mobile nodes can always find each other via a permanent, high-uptime bridge.
