# Deployment Guide

## Frontend Deployment (Vercel)

### Option 1: Deploy via Vercel Dashboard

1. Push your code to GitHub/GitLab/Bitbucket
2. Go to [vercel.com](https://vercel.com) and sign in
3. Click "Add New Project"
4. Import your repository
5. Vercel will auto-detect the configuration from `vercel.json`
6. Add environment variables:
   - `VITE_API_URL` = `https://your-backend-app.onrender.com`
7. Click "Deploy"

### Option 2: Deploy via Vercel CLI

```bash
npm i -g vercel
vercel login
vercel --prod
```

### Environment Variables (Vercel)

Add these in Vercel Dashboard → Settings → Environment Variables:

- `VITE_API_URL` = `https://your-backend-app.onrender.com` (without trailing slash)

### Local Development

Create `frontend/.env.local`:

```
VITE_API_URL=http://localhost:3000
```

---

## Backend Deployment (Render)

### Option 1: Deploy via Render Dashboard

1. Push your code to GitHub/GitLab
2. Go to [render.com](https://render.com) and sign in
3. Click "New +" → "Blueprint"
4. Connect your repository
5. Render will detect `render.yaml`
6. Add environment variables:
   - `MONGODB_URI` - Your MongoDB connection string (MongoDB Atlas recommended)
   - `JWT_SECRET` - Auto-generated or set your own
7. Click "Apply"

### Option 2: Deploy as Web Service (Manual)

1. Go to [render.com](https://render.com)
2. Click "New +" → "Web Service"
3. Connect your repository
4. Configure:
   - **Name**: my-fullstack-backend
   - **Runtime**: Node
   - **Build Command**: `npm install && npx nx build backend --prod`
   - **Start Command**: `node backend/dist/main.js`
   - **Plan**: Free
5. Add environment variables (see below)
6. Click "Create Web Service"

### Environment Variables (Render)

Add these in Render Dashboard → Environment:

- `NODE_ENV` = `production`
- `PORT` = `3000`
- `MONGODB_URI` = Your MongoDB connection string
- `JWT_SECRET` = Your secret key
- `JWT_EXPIRES_IN` = `30d`
- `FRONTEND_URL` = `https://your-frontend.vercel.app` (your Vercel URL)

---

## MongoDB Setup (Required for Backend)

### Use MongoDB Atlas (Recommended)

1. Go to [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster
3. Create a database user
4. Whitelist all IPs (0.0.0.0/0) for Render
5. Get connection string and add to Render environment variables

---

## Post-Deployment

### Update Frontend API URL

After deploying backend to Render:

1. Copy your Render backend URL (e.g., `https://my-fullstack-backend.onrender.com`)
2. Update Vercel environment variable `VITE_API_URL` with this URL
3. Redeploy frontend on Vercel

### Enable CORS on Backend

Make sure your backend allows requests from your Vercel domain. Update `backend/src/main.ts` if needed:

```typescript
app.enableCors({
  origin: ["https://your-frontend.vercel.app"],
  credentials: true,
});
```

---

## Troubleshooting

### Frontend Issues

- Check build logs in Vercel dashboard
- Verify `VITE_API_URL` is set correctly
- Ensure API calls use the environment variable

### Backend Issues

- Check logs in Render dashboard
- Verify MongoDB connection string is correct
- Ensure all environment variables are set
- Free tier on Render spins down after inactivity (takes ~30s to wake up)

### CORS Errors

- Add your Vercel domain to backend CORS configuration
- Ensure credentials are handled properly

---

## Local Testing Before Deployment

### Test Production Build Locally

**Frontend:**

```bash
npx nx build frontend --prod
cd frontend/dist
npx serve
```

**Backend:**

```bash
npx nx build backend --prod
node backend/dist/main.js
```
