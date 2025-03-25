# RealWorld Application Setup Guide

This guide will help you set up and run the RealWorld application, which consists of a React frontend and a Node.js backend with PostgreSQL database.

## Prerequisites

- Node.js (recommended version 16.x or higher)
- Docker and Docker Compose
- Git

## Project Structure

```
realworld-full/
├── frontend-react/    # React frontend application
├── backend-nodeJS/    # Node.js backend application
└── docker-compose.yml # Docker configuration for PostgreSQL
```

## Step 1: Database Setup

1. Create `docker-compose.yml` in the root directory:

```yaml
version: '3.8'
services:
  postgres:
    image: postgres:14
    environment:
      POSTGRES_USER: realworld
      POSTGRES_PASSWORD: realworld
      POSTGRES_DB: realworld
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

2. Start PostgreSQL:
```bash
docker-compose up -d
```

## Step 2: Backend Setup

1. Navigate to the backend directory:
```bash
cd backend-nodeJS
```

2. Create `.env` file in the backend-nodeJS directory:
```
DATABASE_URL="postgresql://realworld:realworld@localhost:5432/realworld?schema=public"
JWT_SECRET="your-super-secret-key-123"
NODE_ENV=development
```

3. Install dependencies and set up the database:
```bash
npm install
npx prisma generate
npx prisma migrate deploy
npx prisma db seed
```

4. Start the backend server:
```bash
npm start
```

The backend will be running at http://localhost:3000

## Step 3: Frontend Setup

1. Open a new terminal and navigate to the frontend directory:
```bash
cd frontend-react
```

2. Install dependencies:
```bash
npm install
```

3. Start the frontend development server:
```bash
npm start
```

The frontend will be running at http://localhost:4100

## Verification

1. Open http://localhost:4100 in your browser
2. You should see the RealWorld application homepage
3. Try to:
   - Register a new account
   - Log in with existing credentials
   - Create articles
   - Add comments
   - Follow other users

## Troubleshooting

1. If you see "address already in use" error for port 3000:
   ```bash
   # Find the process using port 3000
   lsof -i :3000
   # Kill the process
   kill -9 <PID>
   ```

2. If the database connection fails:
   - Make sure Docker is running
   - Check if PostgreSQL container is up: `docker ps`
   - Verify database credentials in `.env` file

3. If you can't connect to the API:
   - Ensure backend is running on http://localhost:3000
   - Check browser console for CORS errors
   - Verify API_ROOT in frontend-react/src/agent.js points to http://localhost:3000/api

## Development Notes

- The frontend development server supports hot reloading
- Backend changes require server restart
- Database schema changes require new migrations 