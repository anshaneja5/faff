# 🐳 Docker Setup Guide for Chat App

This guide explains how to set up and run your chat application using Docker containers.

## 📋 Prerequisites

- Docker Engine 20.10+
- Docker Compose 2.0+
- At least 4GB RAM available
- OpenAI API key
- Qdrant API key (optional for local development)

## 🚀 Quick Start

### 1. Environment Setup

Copy the environment template and configure your variables:

```bash
cp env.example .env
```

Edit `.env` with your actual values:

```bash
# Required: OpenAI API Key
OPENAI_API_KEY=sk-your-actual-openai-key-here

# Optional: Customize database credentials
POSTGRES_PASSWORD=your-secure-password
```

### 2. Build and Run

```bash
# Build all containers
docker-compose build

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f
```

### 3. Access Your Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **Qdrant**: http://localhost:6333
- **PostgreSQL**: localhost:5432

## 🏗️ Architecture Overview

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Frontend  │    │   Backend   │    │   Qdrant    │
│   :3000    │◄──►│    :5000    │◄──►│    :6333    │
│  (React)   │    │ (Node.js)   │    │ (Vector DB) │
└─────────────┘    └─────────────┘    └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ PostgreSQL  │
                    │    :5432    │
                    │  (Chat DB)  │
                    └─────────────┘
```

## 🔧 Container Details

### Frontend Container
- **Base**: nginx:alpine
- **Port**: 3000 → 80 (internal)
- **Features**: 
  - Multi-stage build with Node.js 18
  - Optimized nginx configuration
  - Gzip compression
  - Security headers
  - Client-side routing support

### Backend Container
- **Base**: node:18-alpine
- **Port**: 5000
- **Features**:
  - Non-root user for security
  - Health checks
  - Proper signal handling with dumb-init
  - Production dependencies only

### Qdrant Container
- **Base**: qdrant/qdrant:latest
- **Ports**: 6333 (HTTP), 6334 (gRPC)
- **Features**:
  - Vector database for semantic search
  - Persistent storage
  - Health monitoring

### PostgreSQL Container
- **Base**: postgres:15-alpine
- **Port**: 5432
- **Features**:
  - Persistent data storage
  - Health checks
  - Automatic database initialization

## 🛠️ Development vs Production

### Development
```bash
# Use default docker-compose.yml
docker-compose up -d

# View logs for specific service
docker-compose logs -f backend
```

### Production
```bash
# Use production overrides
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d

# Scale services if needed
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --scale backend=3
```

## 📊 Monitoring & Health Checks

All containers include health checks:

```bash
# Check container health
docker-compose ps

# View health check logs
docker-compose exec backend node -e "require('http').get('http://localhost:5000/health', console.log)"
```

## 🔍 Troubleshooting

### Common Issues

1. **Port Conflicts**
   ```bash
   # Check what's using a port
   netstat -tulpn | grep :3000
   
   # Change ports in docker-compose.yml if needed
   ports:
     - "3001:80"  # Change 3000 to 3001
   ```

2. **Memory Issues**
   ```bash
   # Check container resource usage
   docker stats
   
   # Increase Docker memory limit in Docker Desktop
   ```

3. **Database Connection Issues**
   ```bash
   # Check PostgreSQL logs
   docker-compose logs postgres
   
   # Test connection
   docker-compose exec postgres psql -U postgres -d chat_app
   ```

### Logs and Debugging

```bash
# View all logs
docker-compose logs

# Follow specific service logs
docker-compose logs -f backend

# View logs with timestamps
docker-compose logs -f --timestamps

# Check container status
docker-compose ps
```

## 🧹 Maintenance

### Regular Tasks

```bash
# Update images
docker-compose pull

# Rebuild containers
docker-compose build --no-cache

# Clean up unused resources
docker system prune -f

# Backup volumes
docker run --rm -v chat-app_postgres_data:/data -v $(pwd):/backup alpine tar czf /backup/postgres-backup.tar.gz -C /data .
```

### Scaling

```bash
# Scale backend instances
docker-compose up -d --scale backend=3

# Scale with production config
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --scale backend=3
```

## 🔐 Security Considerations

- All containers run as non-root users
- Network isolation with custom bridge network
- Environment variables for sensitive data
- Health checks prevent unhealthy containers from receiving traffic
- Security headers in nginx configuration

## 📈 Performance Optimization

- Multi-stage builds reduce image sizes
- Gzip compression for static assets
- Proper caching headers
- Resource limits prevent memory issues
- Health checks ensure service availability

## 🚀 Deployment

### Single Server
```bash
# Clone and setup
git clone <your-repo>
cd <your-repo>
cp env.example .env
# Edit .env with production values

# Start with production config
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

### Multi-Server
- Use Docker Swarm or Kubernetes
- Separate database and vector storage
- Load balancer for frontend/backend
- Shared volumes across nodes

## 📞 Support

For issues or questions:
1. Check container logs: `docker-compose logs`
2. Verify environment variables
3. Check port availability
4. Ensure sufficient system resources
5. Review this documentation

---

**Happy Dockerizing! 🐳✨**

