# Docker - Containerization Made Simple

> Learn Docker fundamentals for modern application deployment

## 📋 Table of Contents

- [What is Docker?](#what-is-docker)
- [Core Concepts](#core-concepts)
- [Basic Commands](#basic-commands)
- [Dockerfile](#dockerfile)
- [Docker Compose](#docker-compose)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Interview Questions](#interview-questions)

---

## What is Docker?

**Docker** is a platform for developing, shipping, and running applications in containers.

### 🎯 Explain Like I'm 10

Imagine you want to build a LEGO castle:

**Without Docker (Traditional):**
- You need to find all the right LEGO pieces
- They might not work on your friend's table
- Some pieces might be missing
- Hard to rebuild exactly the same way

**With Docker:**
- Everything you need is in one box (container)
- Works the same on ANY table (environment)
- Easy to give to friends (share containers)
- Rebuild perfectly every time

---

## Core Concepts

### Container vs Virtual Machine

```
┌─────────────────────────┐  ┌─────────────────────────┐
│     Virtual Machine     │  │       Container         │
├─────────────────────────┤  ├─────────────────────────┤
│        App A            │  │        App A            │
│      Libraries          │  │      Libraries          │
│   Guest OS (Linux)      │  │                         │
│                         │  ├─────────────────────────┤
├─────────────────────────┤  │    Docker Engine        │
│      Hypervisor         │  │                         │
├─────────────────────────┤  ├─────────────────────────┤
│     Host OS             │  │      Host OS            │
└─────────────────────────┘  └─────────────────────────┘

     Heavy & Slow                Light & Fast
```

### Key Components

**1. Image**: Blueprint/template for containers
- Like a recipe
- Read-only
- Can be shared

**2. Container**: Running instance of an image
- Like the actual meal from recipe
- Isolated process
- Can be started, stopped, deleted

**3. Dockerfile**: Instructions to build an image
- Like writing the recipe

**4. Registry**: Store for images (e.g., Docker Hub)
- Like a cookbook library

---

## Basic Commands

### Images

```bash
# List images
docker images
docker image ls

# Pull image from Docker Hub
docker pull nginx
docker pull node:18
docker pull python:3.11

# Build image from Dockerfile
docker build -t my-app .
docker build -t my-app:v1.0 .

# Remove image
docker rmi image-name
docker rmi -f image-id

# Inspect image
docker inspect nginx
```

### Containers

```bash
# Run container
docker run nginx                    # Run in foreground
docker run -d nginx                 # Run in background (detached)
docker run --name my-nginx nginx    # Give it a name
docker run -p 8080:80 nginx        # Map ports (host:container)
docker run -e VAR=value nginx      # Set environment variable

# List containers
docker ps                          # Running containers
docker ps -a                       # All containers (including stopped)

# Stop container
docker stop container-name
docker stop container-id

# Start stopped container
docker start container-name

# Remove container
docker rm container-name
docker rm -f container-name        # Force remove (even if running)

# View container logs
docker logs container-name
docker logs -f container-name      # Follow logs (like tail -f)

# Execute command in running container
docker exec -it container-name bash
docker exec container-name ls /app

# View container resource usage
docker stats
```

### Clean Up

```bash
# Remove all stopped containers
docker container prune

# Remove all unused images
docker image prune

# Remove all unused data (containers, images, networks, volumes)
docker system prune

# Remove everything (⚠️ dangerous!)
docker system prune -a
```

---

## Dockerfile

### Basic Structure

```dockerfile
# Start from base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Set environment variables
ENV NODE_ENV=production

# Command to run when container starts
CMD ["node", "server.js"]
```

### Dockerfile Instructions

```dockerfile
# FROM - Base image
FROM ubuntu:22.04
FROM node:18-alpine
FROM python:3.11-slim

# WORKDIR - Set working directory
WORKDIR /app
# All subsequent commands run from this directory

# COPY - Copy files from host to container
COPY package.json ./
COPY src/ ./src/
COPY . .

# ADD - Like COPY but can extract archives
ADD app.tar.gz /app

# RUN - Execute commands during build
RUN apt-get update && apt-get install -y curl
RUN npm install
RUN pip install -r requirements.txt

# ENV - Set environment variables
ENV NODE_ENV=production
ENV PORT=3000

# EXPOSE - Document which ports the app uses
EXPOSE 3000
EXPOSE 8080 8443

# CMD - Default command when container starts
CMD ["node", "server.js"]
CMD ["python", "app.py"]

# ENTRYPOINT - Configure container as executable
ENTRYPOINT ["python"]
CMD ["app.py"]  # Can be overridden

# VOLUME - Create mount point
VOLUME /app/data

# USER - Set user for subsequent commands
USER node

# ARG - Build-time variables
ARG VERSION=1.0
RUN echo "Building version $VERSION"
```

### Multi-Stage Builds

**Reduces image size by separating build and runtime**

```dockerfile
# Stage 1: Build
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Production
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

---

## Docker Compose

**Manage multi-container applications**

### docker-compose.yml

```yaml
version: '3.8'

services:
  # Web application
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=db
      - DB_NAME=myapp
    depends_on:
      - db
      - redis
    volumes:
      - ./src:/app/src
    networks:
      - app-network

  # Database
  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=myapp
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-network

  # Redis cache
  redis:
    image: redis:7-alpine
    networks:
      - app-network

volumes:
  db-data:

networks:
  app-network:
    driver: bridge
```

### Docker Compose Commands

```bash
# Start all services
docker-compose up
docker-compose up -d              # Detached mode

# Stop all services
docker-compose down

# Stop and remove volumes
docker-compose down -v

# View logs
docker-compose logs
docker-compose logs web           # Specific service
docker-compose logs -f            # Follow logs

# Build images
docker-compose build
docker-compose build --no-cache

# Execute command in service
docker-compose exec web bash
docker-compose exec db psql -U admin

# Scale services
docker-compose up -d --scale web=3
```

---

## Common Patterns

### Node.js Application

```dockerfile
FROM node:18-alpine

# Create app directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm ci --only=production

# Copy app source
COPY . .

# Create non-root user
USER node

EXPOSE 3000

CMD ["node", "server.js"]
```

```bash
# Build
docker build -t my-node-app .

# Run
docker run -d \
  --name my-app \
  -p 3000:3000 \
  -e NODE_ENV=production \
  my-node-app
```

### Python Application

```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application
COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

### React Application

```dockerfile
# Build stage
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Full Stack with Docker Compose

```yaml
version: '3.8'

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      - REACT_APP_API_URL=http://localhost:5000
    depends_on:
      - backend

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_NAME=myapp
      - DB_USER=admin
      - DB_PASSWORD=secret
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=myapp
    volumes:
      - postgres-data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    volumes:
      - redis-data:/data

volumes:
  postgres-data:
  redis-data:
```

---

## Best Practices

### 1. Use Specific Tags

```dockerfile
# ❌ Bad - version changes unexpectedly
FROM node:latest

# ✅ Good - explicit version
FROM node:18-alpine
FROM node:18.16.0-alpine
```

### 2. Minimize Image Size

```dockerfile
# Use alpine images
FROM node:18-alpine     # ~180MB
# vs
FROM node:18            # ~1GB

# Multi-stage builds
# Remove dev dependencies
RUN npm ci --only=production

# Clean up in same layer
RUN apt-get update && \
    apt-get install -y package && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
```

### 3. Optimize Layer Caching

```dockerfile
# ❌ Bad - copies everything first
COPY . .
RUN npm install

# ✅ Good - copy package.json first
COPY package*.json ./
RUN npm install
COPY . .
# If package.json doesn't change, npm install is cached!
```

### 4. Security

```dockerfile
# Don't run as root
USER node

# Use .dockerignore
# .dockerignore file:
node_modules
.git
.env
*.md
```

### 5. Health Checks

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:3000/health || exit 1
```

---

## Interview Questions

### 1. What is Docker?
**Answer**: Platform for building, shipping, and running applications in containers. Containers package app with dependencies, ensuring consistency across environments.

### 2. Container vs Virtual Machine?
**Answer**:
- **Container**: Shares host OS kernel, lightweight, fast startup
- **VM**: Full OS per instance, heavier, slower startup

### 3. What is a Docker image?
**Answer**: Read-only template with application code and dependencies. Used to create containers. Built from Dockerfile.

### 4. Explain Dockerfile
**Answer**: Text file with instructions to build Docker image. Contains commands like FROM, COPY, RUN, CMD.

### 5. What is Docker Compose?
**Answer**: Tool for defining and running multi-container applications using YAML file. Manages related services together.

### 6. Docker layers?
**Answer**: Each Dockerfile instruction creates a layer. Layers are cached for faster rebuilds. Order matters for optimization.

### 7. What is .dockerignore?
**Answer**: File listing patterns to exclude from build context. Like .gitignore. Reduces build context size and prevents sensitive files from being copied.

### 8. CMD vs ENTRYPOINT?
**Answer**:
- **CMD**: Default command, can be overridden
- **ENTRYPOINT**: Configures container as executable, CMD becomes arguments

### 9. How do you reduce image size?
**Answer**:
- Use alpine base images
- Multi-stage builds
- Remove dev dependencies
- Clean up in same RUN layer
- Use .dockerignore

### 10. Docker volumes?
**Answer**: Persist data outside container filesystem. Data survives container deletion. Types: named volumes, bind mounts, tmpfs.

---

## Quick Reference

```bash
# Essential Commands Cheat Sheet
docker build -t name .              # Build image
docker run -d -p 8080:80 name       # Run container
docker ps                           # List running
docker stop name                    # Stop container
docker rm name                      # Remove container
docker logs name                    # View logs
docker exec -it name bash           # Enter container

# Docker Compose
docker-compose up -d                # Start services
docker-compose down                 # Stop services
docker-compose logs -f              # Follow logs
```

---

**Keywords**: Docker tutorial, containerization, Docker basics, Dockerfile, Docker Compose, container vs VM, Docker commands, DevOps, Docker interview questions, container deployment

---

*Note: Practice with [Play with Docker](https://labs.play-with-docker.com/) - free online Docker playground!*
