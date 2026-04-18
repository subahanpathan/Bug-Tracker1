# Render + Vercel Deployment - Quick Start

## 🚀 3 Steps to Production

### Step 1: Deploy Backend to Render (5 minutes)
```bash
1. Go to https://render.com
2. Sign in with GitHub
3. New → Web Service
4. Select Bug-Tracker1 repo
5. Name: bug-tracker-api
6. Runtime: Node
7. Build: npm install
8. Start: npm start
9. Add Environment Variables (scroll to "Advanced"):
   - SUPABASE_URL=https://uwpahibmqpfxlbakjtkw.supabase.co
   - SUPABASE_KEY=(from your .env)
   - SUPABASE_SERVICE_KEY=(from your .env)
   - JWT_SECRET=(from your .env)
   - JWT_EXPIRE=7d
   - NODE_ENV=production
   - PORT=3000
   - CORS_ORIGIN=https://your-vercel-url (update later)
   - MAX_FILE_SIZE=5242880
   - UPLOAD_DIR=/tmp/uploads/
10. Click "Create Web Service"
11. ✅ Wait for "Live" status
12. 📋 Copy the URL (e.g., https://bug-tracker-api-xyz.onrender.com)
```

### Step 2: Deploy Frontend to Vercel (5 minutes)
```bash
1. Go to https://vercel.com
2. Sign in with GitHub
3. New Project
4. Select Bug-Tracker1
5. Root Directory: frontend
6. Framework: Vite
7. Environment Variables:
   - VITE_API_URL=https://bug-tracker-api-xyz.onrender.com/api (use your Render URL)
8. Deploy
9. ✅ Wait for deployment
10. 📋 Copy the URL (e.g., https://bug-tracker-frontend-xyz.vercel.app)
```

### Step 3: Update Backend CORS (1 minute)
```bash
1. Go back to Render → bug-tracker-api
2. Settings → Environment
3. Update CORS_ORIGIN=https://your-vercel-url.vercel.app
4. Save
5. Manual Deploy (top right)
```

---

## ✅ Done!
- Frontend: https://your-vercel-url.vercel.app
- Backend: https://bug-tracker-api-xyz.onrender.com/api

---

## 📋 Environment Variables Reference

### From Your `.backend/.env` File:
```
SUPABASE_URL=https://uwpahibmqpfxlbakjtkw.supabase.co
SUPABASE_KEY=eyJhbGc...
SUPABASE_SERVICE_KEY=eyJhbGc...
JWT_SECRET=65646472-8f69-4979-a996-0facacb276e8
```

Find these values in your local:
```bash
cat backend/.env
```

---

## 🔄 Auto-Deploy on GitHub Push
- Push to main → Vercel auto-deploys frontend ✅
- Push to main → Render auto-deploys backend ✅
- No manual steps needed after initial setup!

---

## 💡 Pro Tips
- Check Render Logs if backend won't start: Service → Logs
- Check Vercel Logs if frontend fails: Deployments → Logs
- Always update CORS_ORIGIN after frontend is deployed
- Test with: Frontend URL → Try to log in
