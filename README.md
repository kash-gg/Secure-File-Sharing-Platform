# Secure File Sharing Platform

A full-stack, secure file sharing application designed to let users safely upload, manage, and distribute files via encrypted Cloudflare R2 object storage. Features a modern React frontend and a robust Node.js/Express backend, with PostgreSQL (Neon) for metadata and relationship management.

## 🚀 Features

- **Secure Object Storage:** All generic files are stored seamlessly into **Cloudflare R2** via securely generated short-lived AWS S3 Pre-signed URLs.
- **Granular Link Sharing:** Generate tokenized share links with custom expiry times (`expires_at`) and specific scoped permissions (`view` or `download`).
- **In-App File Viewer:** Directly preview images, videos, and PDFs right from the browser window securely using the customized Vite frontend app.
- **Auto-Redirect Direct Downloads:** Automatically triggers secure file downloads for users visiting links configured strictly for downloading.
- **JWT Authentication:** Strict JSON Web Token based stateless sessions.
- **API Security:** Fully configured with `helmet`, `cors`, and `express-rate-limit` to handle rapid deployments to services like Render and Vercel.

## 🛠️ Technology Stack

**Frontend:**
- React 18
- Vite
- React Router DOM
- CSS Modules & Inline Styling Framework

**Backend:**
- Node.js & Express.js
- PostgreSQL (Powered by Neon) + `pg` driver
- `@aws-sdk/client-s3` (For Cloudflare R2 integration)
- JSON Web Tokens (JWT) & bcrypt

---

## 💻 Local Development Setup

### Prerequisites
- Node.js (v18+)
- A PostgreSQL Database (e.g., [Neon.tech](https://neon.tech/))
- A Cloudflare R2 bucket with API keys.

### 1. Clone the repository
```bash
git clone https://github.com/kash-gg/Secure-File-Sharing-Platform.git
cd Secure-File-Sharing-Platform
```

### 2. Backend Setup
```bash
cd backend
npm install
```

Create a `.env` file in the `backend` folder:
```env
PORT=5000
DATABASE_URL=postgresql://<user>:<password>@<host>/<dbname>?sslmode=require
JWT_SECRET=your_super_secret_jwt_key

# Cloudflare R2 Configuration
R2_ACCESS_KEY_ID=your_cloudflare_access_key
R2_SECRET_ACCESS_KEY=your_cloudflare_secret_key
R2_BUCKET_NAME=your_bucket_name
R2_ENDPOINT=https://<account_id>.r2.cloudflarestorage.com

# For production cross-domain cookie configurations (Vercel -> Render)
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

Run database migrations:
```bash
npm run migrate
```

Start the backend standard development server:
```bash
npm run dev
```

### 3. Frontend Setup
In a new terminal window:
```bash
cd frontend
npm install
```

Create a `.env` file in the `frontend` folder:
```env
VITE_API_BASE_URL=http://localhost:5000
```

Start the Vite development server:
```bash
npm run dev
```
Open `http://localhost:5173` in your browser.

---

## 🌐 Production Deployment Guide

This app is optimized for a split-deployment model: **Frontend on Vercel**, and **Backend on Render**. 

### 1. Render Deployment (Backend)
1. Create a **Web Service** on Render pointing to your `backend/` directory.
2. Set `Build Command` to `npm install` and `Start Command` to `npm start`.
3. Fill out all `.env` variables from above directly into the Render Environment dashboard.
4. Critically: Ensure you set `NODE_ENV=production` inside Render so that cross-site cookies are configured with `SameSite: "none"`. 

### 2. Vercel Deployment (Frontend)
1. Import your project into Vercel and set the "Root Directory" to `frontend`.
2. Vercel should auto-detect **Vite** as your preset framework.
3. Under Environment Variables, set `VITE_API_BASE_URL` to your live Render backend URL (e.g., `https://my-backend.onrender.com`).
4. The project incorporates a `vercel.json` file inside `frontend/` to fix Single Page Application (SPA) routing 404 errors by default.

---
*Built with ❤️ for Secure Data Management.*
