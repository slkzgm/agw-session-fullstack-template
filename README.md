# Abstract Wallet + AGW Session Keys: Fullstack SIWE & Session Management Template

This repository provides a unified starting point for building modern Web3 applications using Abstract Wallet, Ethereum-based authentication (SIWE - Sign-In with Ethereum), and on-chain session management.

The solution integrates two independent repositories:

- **Frontend**: Next.js app integrated with Abstract Wallet.
- **Backend**: Dockerized Express server with Prisma ORM, JWT auth, SIWE integration, and session management.

---

## 🔗 Repositories Overview

| Repository       | URL                                            | Description                                   |
| ---------------- | ---------------------------------------------- | --------------------------------------------- |
| **Frontend**     | [nextera-abstract (branch: abstract-sessions)](https://github.com/slkzgm/nextera/tree/abstract-sessions) | Next.js app integrated with Abstract Wallet. |
| **Backend**      | [web3-auth-backend](https://github.com/slkzgm/express-abstract-sessions) | Dockerized Express API server (SIWE, Prisma, sessions). |

---

## 🚧 Project Architecture

```

agw-session-fullstack-template/
┣ 📄 README.md (this documentation)
┃
┣ 📁 frontend [https://github.com/slkzgm/nextera/tree/abstract-sessions]
┃ ┣ 📁 Next.js app (Abstract, theme management, sessions)
┃ ┗ 📁 Components, hooks, and API integrations
┃
┗ 📁 backend [Dockerized Express server]
  ┣ 📁 src (Express, JWT, SIWE handlers, Prisma ORM)
  ┣ 📁 prisma (schema & migrations)
  ┣ 📄 Dockerfile (multi-stage Node.js build)
  ┣ 📄 docker-compose.yml (Postgres & Backend)
  ┗ 📄 docker-compose.override.yml (Dev mode overrides)

````

---

## 📦 Requirements

- **Docker & Docker Compose**
- **Node.js** (v18+)
- **npm or pnpm**
- **Abstract Wallet Account**

---

## 🛠 Installation & Configuration

### ▶️ Backend Setup (Dockerized)

Clone backend repository and enter directory:

```bash
git clone https://github.com/slkzgm/express-abstract-sessions backend
cd backend
````

**Environment Configuration (`.env`):**

Create a `.env` file in the backend root directory:

```env
NODE_ENV=development
PORT=3000
HOST_PORT=3000

DATABASE_URL="postgresql://postgres:postgres@database:5432/postgres"
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=postgres
POSTGRES_HOST_PORT=5432
POSTGRES_PORT=5432

JWT_SECRET="your_jwt_secret"

CHAIN=abstractTestnet

SIWE_DOMAIN="yourdomain.com"
SIWE_STATEMENT="Sign in with Ethereum"
SIWE_URI="https://yourdomain.com"
SIWE_CHAIN_ID=1
NONCE_EXPIRY_MS=300000
```

### 🚀 Launch Backend with Docker Compose

**Development Mode**:

```bash
docker compose -f docker-compose.yml -f docker-compose.override.yml up --build
```

This runs:

* PostgreSQL DB with persistent volume.
* Express backend in dev mode with auto-reload on code changes.

Backend API available at:
👉 [http://localhost:3000](http://localhost:3000)

**Production Mode**:

```bash
docker compose up --build
```

Production mode uses pre-built, optimized code, and migrations applied automatically.

---

### 🎯 Frontend Setup

Clone frontend repository and enter directory:

```bash
git clone -b abstract-sessions https://github.com/slkzgm/nextera frontend
cd frontend
npm install
```

**Frontend Environment (`.env.local`):**

```env
NEXT_PUBLIC_BACKEND_URL="http://localhost:3000"
```

### 🖥 Launch Frontend (Next.js Dev)

```bash
npm run dev
```

Frontend app runs at:
👉 [http://localhost:3001](http://localhost:3001)

---

## 🚀 Recommended Simultaneous Launch (Optional)

If you prefer, you can easily run both services simultaneously using [concurrently](https://www.npmjs.com/package/concurrently):

At the project root directory (`web3-fullstack-template`):

```bash
npm install concurrently
```

**Create a root `package.json`** with:

```json
{
  "scripts": {
    "dev": "concurrently \"docker compose -f backend/docker-compose.yml -f backend/docker-compose.override.yml up\" \"npm --prefix frontend run dev\""
  }
}
```

Run both frontend and backend together:

```bash
npm run dev
```

---

## 📚 Project Workflows Explained

### 🪙 **Sign-In with Ethereum (SIWE)**

* Connect wallet via frontend.
* Fetch SIWE challenge (`GET /api/auth/challenge`).
* Sign challenge using Abstract Wallet.
* Authenticate via backend (`POST /api/auth/login`), receiving JWT cookie.

### 🔑 **On-chain Sessions**

* Initiate a session via backend (`GET /api/session`).
* Confirm session on-chain (frontend action).
* Confirm session via backend (`POST /api/session/confirm`).
* Interact securely (e.g., mint NFTs using session API endpoints).

### 🔒 **Protected API routes**

Use JWT-based authorization:

* Example protected route (`GET /api/protected`).

---

## 📖 Useful Documentation

* [Next.js](https://nextjs.org/docs)
* [Abstract Wallet](https://docs.abs.xyz)
* [Express.js](https://expressjs.com/)
* [Prisma ORM](https://www.prisma.io/docs)
* [Docker Compose](https://docs.docker.com/compose/)

---

## 🎗 License

Frontend and backend are MIT licensed; please verify individual repositories for specific details.

---

## 🚩 Why Docker for the Backend?

* **Consistency & Isolation:** Ensures consistent environment across development, staging, and production.
* **Rapid Development:** Automatic code reload in dev mode via Docker Compose overrides.
* **Production Ready:** Multi-stage builds ensure optimized and secure deployment.

```

---

## ✅ **Final Considerations:**

- Replace all placeholders (`YOUR_BACKEND_REPO_LINK`, domains, secrets) with your real values.
- Clearly outlines both Dockerized backend and traditional Next.js frontend setup for easy adoption.
- Keeps development independent and simple, aligning perfectly with your preferred approach of separate repository management.
