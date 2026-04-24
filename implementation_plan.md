# Deploy NR Dry Fruits Online

**Strategy:** Deploy backend to **Render** (free tier), database to **MongoDB Atlas** (free 512 MB), and frontend to **Vercel** (free tier). All three are free-forever tiers and take less than 15 minutes.

---

## Architecture

```
Users → Vercel (React Frontend) → Render (Express API) → MongoDB Atlas
```

---

## Step-by-Step Deployment Plan

### Phase 1 — MongoDB Atlas (Free Cloud Database)

1. Sign up at https://cloud.mongodb.com (free)
2. Create a **free M0 cluster** (512 MB, no credit card needed)
3. Create a DB user with a password
4. Whitelist IP `0.0.0.0/0` (allow all — required for Render)
5. Copy the **connection string** → looks like:
   `mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/nr-dry-fruit`

---

### Phase 2 — Backend on Render

**Files I'll add to your project:**
- `backend/render.yaml` — Render service config
- Update `backend/.env.example` — document all prod vars

**What I'll configure:**
- `NODE_ENV=production`
- `MONGODB_URI` → Atlas URI (you paste it)
- `CLIENT_URL` → Vercel frontend URL (set after frontend deploy)
- `USE_IN_MEMORY_DB=false`
- Strong `JWT_SECRET`
- `UPLOAD_MODE=local` (images stored on Render disk — see note below)

**Deploy steps:**
1. Push to GitHub (I'll verify `.gitignore` ignores `.env`)
2. Go to https://render.com → New Web Service → connect GitHub repo
3. Set **Root Directory**: `backend`
4. Set **Build Command**: `npm install`
5. Set **Start Command**: `npm start`
6. Add all environment variables in Render dashboard

---

### Phase 3 — Frontend on Vercel

**Files I'll add:**
- `frontend/vercel.json` — SPA routing fix (all routes → `index.html`)

**What I'll update:**
- `frontend/.env` → `VITE_API_URL=https://your-backend.onrender.com/api`

**Deploy steps:**
1. Go to https://vercel.com → Import Git Repository
2. Set **Root Directory**: `frontend`
3. Set **Build Command**: `npm run build`
4. Set **Output Directory**: `dist`
5. Add env var: `VITE_API_URL=<Render backend URL>/api`

---

### Phase 4 — Wire Everything Together

After both are deployed:
- Go back to Render → update `CLIENT_URL` to your Vercel URL
- CORS will now allow requests from your live frontend

---

## Files I Will Create / Modify

### Backend
- **[NEW]** `backend/render.yaml` — Render deploy config
- **[MODIFY]** `backend/src/app.js` — support multiple CORS origins (Vercel + localhost) and add production CORS headers
- **[MODIFY]** `.gitignore` (root) — ensure `.env` files are excluded

### Frontend
- **[NEW]** `frontend/vercel.json` — rewrite all routes to index.html for React Router SPA support

---

## Important Notes

> [!WARNING]
> **Image Uploads:** Currently images are saved to the local disk (`/backend/uploads`). On Render's free tier, the disk **resets on every deploy**. For this deployment, uploads will work between deploys but not persist permanently. For permanent image storage, we'd integrate Cloudinary — let me know if you want that too.

> [!IMPORTANT]
> **MongoDB Atlas URI:** You will need to create a free MongoDB Atlas account and provide the connection string. I will walk you through it step by step during execution.

> [!NOTE]
> **Render Free Tier cold start:** The free Render backend spins down after 15 minutes of inactivity. The first request after inactivity takes ~30 seconds to wake up. This is normal for the free tier.

---

## Verification Plan

1. Hit `https://your-backend.onrender.com/api/health` → should return `{"status":"ok"}`
2. Open Vercel frontend URL → app loads, products appear (seeded data)
3. Login with `admin@nrdryfruit.com / Admin@123` → admin panel accessible
4. Login with `aisha@example.com / Customer@123` → customer flow works
