# 🚀 Real-Time Chat Application with Semantic Search

A modern, real-time chat application built with React, Node.js, PostgreSQL, and Qdrant vector database. Features include user authentication, real-time messaging, and semantic search through conversation history.

## ✨ Features

- **Real-time Chat**: Instant messaging with Socket.IO
- **User Authentication**: Secure login/registration system
- **Semantic Search**: AI-powered search through message history using OpenAI embeddings
- **Modern UI**: Beautiful, responsive design with TailwindCSS
- **Dark Mode**: Toggle between light and dark themes
- **Mobile Responsive**: Works perfectly on all devices
- **Vector Database**: Qdrant for efficient semantic search

## 🏗️ Architecture

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

## 🐳 Quick Start with Docker

### Prerequisites

- **Docker Desktop** installed and running
- **Git** for cloning the repository
- **OpenAI API Key** for semantic search features

### 1. Clone the Repository

```bash
git clone https://github.com/anshaneja5/faff
cd faff-Assignment
```

### 2. Environment Setup

Copy the environment template and configure your variables:

```bash
cp env.example .env
```

Edit `.env` with your actual values:

```bash
# Required: OpenAI API Key for semantic search
OPENAI_API_KEY=sk-your-actual-openai-key-here

# Optional: Customize database credentials (defaults work fine)
```

### 3. Start All Services

```bash
# Build and start all containers
docker-compose up --build -d

# Or just start if already built
docker-compose up -d
```

### 4. Access Your Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **Qdrant Dashboard**: http://localhost:6333/dashboard
- **PostgreSQL**: localhost:5432

## 🛠️ Development Commands

### Docker Management

```bash
# View running containers
docker-compose ps

# View logs
docker-compose logs -f

# View specific service logs
docker-compose logs -f backend
docker-compose logs -f frontend

# Stop all services
docker-compose down

# Rebuild and restart
docker-compose up --build -d

# Clean up everything
docker-compose down -v
docker system prune -f
```

### Service-Specific Commands

```bash
# Restart specific service
docker-compose restart backend

# Scale backend instances
docker-compose up -d --scale backend=3

# Check service health
docker-compose exec backend node -e "require('http').get('http://localhost:5000/health', console.log)"
```

## 🔧 Manual Setup (Alternative to Docker)

### Backend Setup

```bash
cd backend

# Install dependencies
npm install

# Create .env file
cp env.example .env
# Edit .env with your database and OpenAI credentials

# Start development server
npm run dev
```

### Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start development server
npm run dev
```

### Database Setup

1. Install PostgreSQL locally
2. Create database: `chat_app`
3. Update backend `.env` with local database URL

## 📱 API Endpoints

### Authentication
- `POST /auth/register` - Create new user account
- `POST /auth/login` - User login

### Users
- `GET /users` - Get all users
- `GET /users/:id` - Get user by ID

### Messages
- `POST /messages` - Send a message
- `GET /messages?userId=:id&limit=:limit` - Get user messages
- `GET /messages/semantic-search?userId=:id&q=:query` - Semantic search

### Health
- `GET /health` - Service health check

## 🐛 Troubleshooting

### Common Issues

1. **Port Already in Use**
   ```bash
   # Check what's using a port
   netstat -tulpn | grep :3000
   
   # Change ports in docker-compose.yml if needed
   ```

2. **Database Connection Issues**
   ```bash
   # Check PostgreSQL logs
   docker-compose logs postgres
   
   # Test connection
   docker-compose exec postgres psql -U postgres -d chat_app
   ```

3. **Build Failures**
   ```bash
   # Clean rebuild
   docker-compose down
   docker system prune -f
   docker-compose up --build -d
   ```

4. **Frontend Not Loading**
   - Check if backend is running: `docker-compose ps`
   - Verify API URL in frontend environment
   - Check browser console for errors

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

## 🚀 Production Deployment

### Docker Production

```bash
# Use production configuration
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d

# Scale services
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --scale backend=3
```

### Cloud Deployment

- **Backend**: Deploy to Render, Railway, or AWS
- **Frontend**: Deploy to Vercel, Netlify, or AWS S3
- **Database**: Use managed PostgreSQL (Supabase, Neon, AWS RDS)
- **Vector DB**: Use Qdrant Cloud or self-hosted

## 📚 Project Structure

```
faff-Assignment/
├── backend/                 # Node.js + Express + Socket.IO
│   ├── src/
│   │   ├── routes/         # API endpoints
│   │   ├── models/         # Database models
│   │   ├── services/       # Business logic
│   │   └── index.js        # Server entry point
│   ├── Dockerfile          # Backend container
│   └── package.json
├── frontend/                # React + Vite + TailwindCSS
│   ├── src/
│   │   ├── components/     # UI components
│   │   ├── pages/          # Application pages
│   │   ├── contexts/       # React contexts
│   │   └── App.jsx         # Main app component
│   ├── Dockerfile          # Frontend container
│   ├── nginx.conf          # Nginx configuration
│   └── package.json
├── docker-compose.yml       # Service orchestration
├── docker-compose.prod.yml  # Production overrides
├── env.example             # Environment template
└── README.md               # This file
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Commit changes: `git commit -am 'Add feature'`
4. Push to branch: `git push origin feature-name`
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 🆘 Support

If you encounter any issues:

1. Check the troubleshooting section above
2. Review container logs: `docker-compose logs`
3. Verify environment variables
4. Check port availability
5. Ensure Docker Desktop is running

## 🎯 Next Steps

After getting the application running:

1. **Test Features**: Try creating accounts, sending messages, and using semantic search
2. **Customize**: Modify the UI, add new features, or integrate additional services
3. **Deploy**: Deploy to your preferred cloud platform
4. **Scale**: Add more backend instances or implement load balancing

---

**Happy Chatting! 🚀✨**
