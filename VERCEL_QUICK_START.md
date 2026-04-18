# Quick Vercel Deployment - 5 Minutes

## Prerequisites
- GitHub account (push your code there)
- Vercel account (free at vercel.com)
- Your Supabase credentials handy

## Step 1: Push to GitHub
```bash
cd "c:\Users\Hariom\Downloads\BUG TRACKER"
git push origin main
```

## Step 2: Deploy Backend (2 minutes)

1. Go to https://vercel.com/dashboard
2. Click "Add New" → "Project"
3. Select your bug-tracker repository
4. Click Import
5. Configure:
   - **Root Directory**: `backend`
   - Click "Deploy"
6. Wait for deployment ✅

**Copy your backend URL** (e.g., `https://bug-tracker-api-xyz.vercel.app`)

## Step 3: Add Environment Variables to Backend

1. Go to backend project → Settings → Environment Variables
2. Add these variables:
   - `SUPABASE_URL` = (from your .env)
   - `SUPABASE_KEY` = (from your .env)
   - `SUPABASE_SERVICE_KEY` = (from your .env)
   - `JWT_SECRET` = (from your .env)
   - `CORS_ORIGIN` = (leave blank for now, we'll update after frontend deployment)

3. Redeploy: Deployments tab → "..." → "Redeploy"

## Step 4: Deploy Frontend (2 minutes)

1. Go to https://vercel.com/dashboard
2. Click "Add New" → "Project"
3. Select your bug-tracker repository
4. Click Import
5. Configure:
   - **Root Directory**: `frontend`
   - Click "Deploy"
6. Wait for deployment ✅

**Copy your frontend URL** (e.g., `https://bug-tracker-frontend-xyz.vercel.app`)

## Step 5: Update CORS on Backend (1 minute)

1. Go to backend project → Settings → Environment Variables
2. Update `CORS_ORIGIN` = your frontend URL (the one you just copied)
3. Add `VITE_API_URL` environment variable in frontend with backend URL
4. Redeploy both projects

## Done! 🎉

Your app is now live at your frontend URL!

### Test It
- Visit your frontend URL
- Sign up / Login
- Create a project
- Create a ticket
- All features working? ✅

### Troubleshooting
- **Can't connect?** → Check CORS_ORIGIN matches your frontend URL exactly
- **Shows old version?** → Clear browser cache (Ctrl+Shift+Delete)
- **API errors?** → Check backend logs in Vercel Deployments tab

### Next Steps
- Set up custom domain (optional)
- Monitor performance in Vercel Analytics tab
- Configure automatic backups in Supabase

For full guide, see VERCEL_DEPLOYMENT.md
