# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Backend (Node.js/Fastify)
- `cd backend && npm run dev` - Start backend server in development mode with nodemon
- `cd backend && npm install` - Install backend dependencies

### Frontend (TypeScript/Vite)  
- `cd frontend && npm run dev` - Start frontend dev server on port 5173 with host binding for external access
- `cd frontend && npm run build` - Build frontend for production (runs TypeScript compilation + Vite build)
- `cd frontend && npm run preview` - Preview production build locally
- `cd frontend && npm install` - Install frontend dependencies

### Docker Development
- `docker-compose up` - Start full application stack (requires NGROK_AUTHTOKEN and PUBLIC_URL env vars)
- `docker build -t hellooo .` - Build Docker image

## Architecture Overview

This is a full-stack multiplayer Pong game application with the following structure:

### Backend (/backend)
- **Framework**: Fastify with plugins for CORS, JWT, multipart uploads, static files
- **Database**: SQLite with three main tables: users, friends, match_history
- **Authentication**: JWT tokens stored in HTTP-only cookies, Google OAuth, 2FA support
- **Real-time**: WebSocket server for multiplayer game coordination
- **File Structure**:
  - `app.js` - Main server entry point with auto-detected LAN IP and HTTPS support
  - `routes/` - API route handlers (auth, friends, match, avatar)
  - `controllers/` - Business logic (auth, tournament, Google OAuth, email verification)
  - `models/` - Database models (user model)
  - `middlewares/` - Authentication middleware
  - `db/database.js` - SQLite database setup and table creation
  - `ws.js` - WebSocket server for real-time game features
  - `services/` - Email service for verification and notifications
  - `utils/` - Utility functions (ngrok URL resolution)

### Frontend (/frontend)
- **Framework**: Vanilla TypeScript with Vite bundler
- **Development Server**: Configured with host binding (0.0.0.0) and HTTPS support via certificates
- **Routing**: Hash-based SPA router with authentication guards
- **UI**: Custom CSS with responsive design
- **Internationalization**: i18next with language detection (English, French, Arabic)
- **Game Engine**: Canvas-based Pong implementation with AI opponent
- **Animations**: Canvas confetti effects and Chart.js for data visualization
- **Features**:
  - Local and remote multiplayer Pong
  - AI opponent with adjustable difficulty
  - Tournament system
  - User profiles with avatar upload
  - Friends system
  - Match history tracking

### Key Features
- **Multiplayer Gaming**: Real-time Pong games via WebSocket
- **Tournament System**: Bracket-style tournaments with match tracking
- **User Management**: Registration, email verification, profile management, avatar uploads
- **Social Features**: Friends system with pending/accepted status
- **Remote Play**: Integration with ngrok for external access
- **Security**: JWT authentication, 2FA, email verification, password hashing with bcrypt

### Database Schema
- **users**: id, name, email, password, google_id, avatar, is_verified, is2FAEnabled, verification tokens, timestamps
- **friends**: id, user_id, friend_id, status (pending/accepted)
- **match_history**: id, player1, player2, winner, score1, score2, date

### Environment Configuration
Requires `.env` file with:
- `NGROK_AUTHTOKEN` - For ngrok tunnel authentication
- `PUBLIC_URL` - Base URL for the application (defaults to https://c1r6s6.42beirut.com:3000)
- `JWT_SECRET` - JWT signing secret (defaults to 'forsecret')
- `CORS_42_ORIGINS` - Additional CORS origins (comma-separated)

### Deployment
The application is containerized with Docker and includes:
- Multi-stage build process
- Ngrok integration for external access
- Automatic certificate detection for HTTPS
- Health check endpoint at `/api/health`