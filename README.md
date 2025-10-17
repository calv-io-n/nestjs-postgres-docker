# Replit Refactor

A full-stack application with NestJS API, React frontend, and PostgreSQL database.

## Architecture

- **API**: NestJS backend with TypeORM and PostgreSQL
- **Web**: React frontend with Vite
- **Database**: PostgreSQL 15 with Docker
- **Containerization**: Docker Compose for local development

## Prerequisites

- Docker and Docker Compose
- Node.js 18+ (for local development)

## Quick Start

1. **Clone and setup**:
   ```bash
   git clone <repository-url>
   cd replit-refactor
   ```

2. **Start all services**:
   ```bash
   docker compose up -d
   ```

3. **Access the application**:
   - Web App: http://localhost:5173
   - API: http://localhost:3000
   - PostgreSQL: localhost:5432

## Services

### PostgreSQL Database
- **Container**: `replit-postgres`
- **Database**: `replit_db`
- **User**: `replit_user`
- **Password**: `replit_password`
- **Port**: 5432

### API Service
- **Container**: `replit-api`
- **Port**: 3000
- **Database URL**: `postgresql://replit_user:replit_password@postgres:5432/replit_db`

### Web Service
- **Container**: `replit-web`
- **Port**: 5173
- **API URL**: `http://localhost:3000`

## Development

### Running Individual Services

**Start only the database**:
```bash
docker compose up -d postgres
```

**Start API and database**:
```bash
docker compose up -d postgres api
```

**Start web and API**:
```bash
docker compose up -d postgres api web
```

### Local Development

**Install dependencies**:
```bash
# API
cd api && npm install

# Web
cd web && npm install
```

**Run locally** (with database running):
```bash
# Terminal 1: Start database
docker compose up -d postgres

# Terminal 2: Run API
cd api && npm run start:dev

# Terminal 3: Run web
cd web && npm run dev
```

## Database

The PostgreSQL database is initialized with:
- A `users` table with sample data
- UUID extension enabled
- Proper indexing on email field

### Database Connection

**From API**:
```typescript
// The DATABASE_URL environment variable is automatically set
const connectionString = process.env.DATABASE_URL;
```

**From external tools**:
```bash
# Using psql
psql -h localhost -p 5432 -U replit_user -d replit_db

# Using connection string
postgresql://replit_user:replit_password@localhost:5432/replit_db
```

## Environment Variables

Create a `.env` file in the root directory:

```env
# Database Configuration
DATABASE_URL=postgresql://replit_user:replit_password@localhost:5432/replit_db
POSTGRES_DB=replit_db
POSTGRES_USER=replit_user
POSTGRES_PASSWORD=replit_password
POSTGRES_HOST=localhost
POSTGRES_PORT=5432

# API Configuration
NODE_ENV=development
API_PORT=3000

# Web Configuration
VITE_API_URL=http://localhost:3000
```

## Useful Commands

**View logs**:
```bash
# All services
docker compose logs

# Specific service
docker compose logs postgres
docker compose logs api
docker compose logs web
```

**Stop services**:
```bash
docker compose down
```

**Reset database**:
```bash
docker compose down -v
docker compose up -d postgres
```

**Rebuild services**:
```bash
docker compose up -d --build
```

## Project Structure

```
├── api/                 # NestJS API
│   ├── src/
│   ├── Dockerfile
│   └── package.json
├── web/                 # React frontend
│   ├── src/
│   ├── Dockerfile
│   └── package.json
├── docker-compose.yaml  # Docker services configuration
├── init.sql            # Database initialization
└── README.md
```
