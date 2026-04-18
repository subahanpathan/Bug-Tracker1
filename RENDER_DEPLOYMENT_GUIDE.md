# Bug Tracker - Render + Vercel Deployment Guide

Deploy your Bug Tracker with **Render** (backend) and **Vercel** (frontend).

---

## 📋 Prerequisites

- GitHub account with code pushed to [Bug-Tracker1](https://github.com/subahanpathan/Bug-Tracker1)
- Vercel account (free at https://vercel.com)
- Render account (free at https://render.com)
- Supabase credentials from your `.env` file

---

## 🔧 Part 1: Deploy Backend to Render

### Step 1: Create Render Account
1. Go to https://render.com
2. Sign up with GitHub (click "Continue with GitHub")
3. Authorize Render to access your GitHub

### Step 2: Create New Web Service
1. Dashboard → "New +" → "Web Service"
2. Select: `Bug-Tracker1` repository
3. Click "Connect"

### Step 3: Configure Service
| Setting | Value |
|---------|-------|
| Name | `bug-tracker-api` |
| Runtime | `Node` |
| Build Command | `npm install` |
| Start Command | `npm start` |
| Plan | `Free` |

### Step 4: Add Environment Variables
Click "Advanced" → "Add Environment Variable" for each:

**Required Variables:**
| Key | Value | Source |
|-----|-------|--------|
| `NODE_ENV` | `production` | Fixed |
| `PORT` | `3000` | Fixed |
| `SUPABASE_URL` | `https://uwpahibmqpfxlbakjtkw.supabase.co` | From your `.env` |
| `SUPABASE_KEY` | (your anon key) | From your `.env` |
| `SUPABASE_SERVICE_KEY` | (your service role key) | From your `.env` |
| `JWT_SECRET` | (your UUID) | From your `.env` |
| `JWT_EXPIRE` | `7d` | Fixed |
| `CORS_ORIGIN` | (will update later) | Leave as is |
| `MAX_FILE_SIZE` | `5242880` | Fixed |
| `UPLOAD_DIR` | `/tmp/uploads/` | Fixed |

### Step 5: Deploy
1. Click "Create Web Service"
2. ⏳ Wait 3-5 minutes for initial deployment
3. ✅ Once "Live" appears, copy the URL (e.g., `https://bug-tracker-api-xyz.onrender.com`)

### Step 6: Update CORS in Render
1. Go to Service Settings
2. Find "Environment" section
3. Update `CORS_ORIGIN` = `https://your-vercel-frontend-url` (will update after frontend deployment)
4. Manual Deploy (see the "Deploy" button)

---

## 🌐 Part 2: Deploy Frontend to Vercel

### Step 1: Go to Vercel
1. Go to https://vercel.com
2. Sign in with GitHub

### Step 2: Import Project
1. "New Project" → Select `Bug-Tracker1`
2. Click "Import"

### Step 3: Configure Frontend
1. **Root Directory**: `frontend`
2. **Framework Preset**: `Vite`
3. **Build Command**: `npm run build`
4. **Install Command**: `npm install`

### Step 4: Set Environment Variables
Click "Environment Variables" and add:

| Key | Value |
|-----|-------|
| `VITE_API_URL` | `https://bug-tracker-api-xyz.onrender.com/api` |

**Replace** `bug-tracker-api-xyz` with your actual Render URL.

### Step 5: Deploy
1. Click "Deploy"
2. ⏳ Wait 3-5 minutes
3. ✅ Get Frontend URL (e.g., `https://bug-tracker-frontend.vercel.app`)

---

## 🔗 Part 3: Connect Frontend & Backend

### Step 1: Update Render CORS
1. Go back to Render → bug-tracker-api service
2. Environment → Edit `CORS_ORIGIN`
3. Set to: `https://your-frontend-url.vercel.app` (from Vercel)
4. Click "Save Changes"
5. Manual Redeploy (Top right → "Manual Deploy")

### Step 2: Verify Connection
1. Open your frontend URL in browser
2. Try to log in
3. Check Network tab for successful API calls

---

## ✅ Deployment Complete!

Your app is now live:
- **Frontend**: https://your-frontend.vercel.app
- **Backend API**: https://bug-tracker-api-xyz.onrender.com

---

## 🚀 Post-Deployment Checklist

- [ ] Frontend loads without 404
- [ ] Login page appears
- [ ] Can register new account
- [ ] Can log in
- [ ] Can create a project
- [ ] Can create a ticket
- [ ] Can view activity log
- [ ] Can manage email preferences
- [ ] Real-time updates working (Socket.io)

---

## 🔄 Updating Your Code

Every time you push to GitHub:
1. Vercel auto-deploys frontend ✅
2. Render auto-deploys backend ✅
3. No manual steps needed!

---

## 💰 Cost

| Service | Plan | Cost |
|---------|------|------|
| Render | Free | $0 (includes 750 compute hours/month) |
| Vercel | Free | $0 |
| **Total** | | **$0/month** |

---

## ❓ Troubleshooting

### Backend won't start
- Check Render Logs: Service → Logs
- Verify all env vars are set
- Check SUPABASE_URL is correct

### Frontend shows API errors
- Verify `VITE_API_URL` in Vercel env vars
- Check Render CORS_ORIGIN is set to frontend URL
- Clear browser cache (Ctrl+Shift+Delete)

### Real-time updates not working
- Verify Socket.io connection in browser DevTools → Network
- Check backend logs for connection errors
- Ensure CORS includes WebSocket protocol

---

## 📞 Support

- **Render Docs**: https://docs.render.com
- **Vercel Docs**: https://vercel.com/docs
- **Supabase Docs**: https://supabase.com/docs
