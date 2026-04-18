# Deploy Bug Tracker to Vercel - Step by Step Guide

## 🚀 Overview
You'll deploy your Bug Tracker app in 3 parts:
1. Push code to GitHub
2. Deploy Backend API to Vercel
3. Deploy Frontend UI to Vercel

**Total time: 15 minutes**

---

## Part 1: Push Code to GitHub (5 minutes)

### 1.1 Create GitHub Account
- Go to https://github.com
- Sign up if you don't have an account
- Verify email

### 1.2 Create a New Repository
- Click "+" → "New repository"
- **Name**: `bug-tracker` (or your preference)
- **Description**: "Professional Bug Tracker Application"
- Choose **Public** or **Private** (your choice)
- Click "Create repository"
- **Copy the repository URL** (you'll need it next)

### 1.3 Push Your Code
Open PowerShell and run:

```powershell
cd "c:\Users\Hariom\Downloads\BUG TRACKER"

# Initialize git if not done
git init
git add .
git commit -m "Initial commit: Bug Tracker application"

# Add your GitHub repository
git remote add origin https://github.com/YOUR_USERNAME/bug-tracker.git
git branch -M main
git push -u origin main
```

**Replace `YOUR_USERNAME` with your actual GitHub username**

Check it worked:
- Go to https://github.com/YOUR_USERNAME/bug-tracker
- You should see your code there ✅

---

## Part 2: Deploy Backend to Vercel (5 minutes)

### 2.1 Create Vercel Account
- Go to https://vercel.com
- Click "Sign Up"
- Choose "GitHub" (easier integration)
- Authorize Vercel to access your GitHub
- You'll be redirected to Vercel dashboard

### 2.2 Import Your Project
- Click "Add New" → "Project"
- Find your `bug-tracker` repository
- Click "Import"
- Wait for preview to load

### 2.3 Configure Project
You'll see a form. Fill it out:

**Project Name**: `bug-tracker-api`  
**Root Directory**: Select `backend` from dropdown  
**Framework**: Leave as "Other"  
**Build Command**: `npm install`  
**Start Command**: `npm start`  

Click "Deploy"

### 2.4 Wait for Deployment
- Vercel will build and deploy
- Takes 2-3 minutes
- You'll see a green "Congratulations" message
- **Copy this URL** - you'll need it next (looks like: `https://bug-tracker-api-xyz.vercel.app`)

### 2.5 Add Environment Variables
1. In Vercel, go to project → "Settings" tab
2. Click "Environment Variables" (left sidebar)
3. Add these variables:

Click "+ Add New" and add each one:

| Name | Value |
|------|-------|
| `SUPABASE_URL` | `https://uwpahibmqpfxlbakjtkw.supabase.co` |
| `SUPABASE_KEY` | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...` (from your .env) |
| `SUPABASE_SERVICE_KEY` | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...` (from your .env) |
| `JWT_SECRET` | `65646472-8f69-4979-a996-0facacb276e8` (from your .env) |
| `JWT_EXPIRE` | `7d` |
| `NODE_ENV` | `production` |
| `MAX_FILE_SIZE` | `5242880` |
| `UPLOAD_DIR` | `/tmp/uploads/` |

*Copy values from your `backend/.env` file*

4. Click "Save"

### 2.6 Redeploy Backend
1. Go to "Deployments" tab
2. Find the latest deployment
3. Click the "..." menu
4. Click "Redeploy"
5. Wait 1-2 minutes for it to redeploy with the new variables

**✅ Backend is now deployed!**

Save this URL for later: `https://bug-tracker-api-xyz.vercel.app`

---

## Part 3: Deploy Frontend to Vercel (5 minutes)

### 3.1 Create Second Project
- In Vercel dashboard, click "Add New" → "Project"
- Find your `bug-tracker` repository again
- Click "Import"
- Wait for preview to load

### 3.2 Configure Project
**Project Name**: `bug-tracker-frontend`  
**Root Directory**: Select `frontend` from dropdown  
**Framework**: Select `Vite` from dropdown  
**Build Command**: `npm run build`  
**Output Directory**: `dist`  

Click "Deploy"

### 3.3 Wait for Deployment
- Vercel will build your frontend
- Takes 2-3 minutes
- You'll see green "Congratulations"
- **Copy this URL** (looks like: `https://bug-tracker-frontend-xyz.vercel.app`)

### 3.4 Add Environment Variables
1. Go to project → "Settings" tab
2. Click "Environment Variables"
3. Add these two:

| Name | Value |
|------|-------|
| `VITE_API_URL` | `https://bug-tracker-api-xyz.vercel.app/api` |
| `VITE_ENV` | `production` |

**Replace `bug-tracker-api-xyz` with your actual backend URL from Part 2**

4. Click "Save"

### 3.5 Redeploy Frontend
1. Go to "Deployments" tab
2. Find the latest deployment
3. Click the "..." menu
4. Click "Redeploy"
5. Wait 1-2 minutes

**✅ Frontend is now deployed!**

---

## Part 4: Final Configuration (2 minutes)

### 4.1 Update Backend CORS
Now that you have both URLs, let's fix the backend CORS:

1. Go back to **Backend project** in Vercel
2. Settings → Environment Variables
3. Find `CORS_ORIGIN` (or add it if it doesn't exist)
4. Set value to your **frontend URL**: `https://bug-tracker-frontend-xyz.vercel.app`
5. Click "Save"

### 4.2 Redeploy Backend Again
1. Deployments tab
2. Click "..." on latest deployment
3. Click "Redeploy"
4. Wait 1-2 minutes

---

## 🎉 You're Done!

Your app is now live!

### Test It Out
1. Open your frontend URL in browser: `https://bug-tracker-frontend-xyz.vercel.app`
2. You should see your Bug Tracker login page
3. Try these:
   - ✅ Create an account
   - ✅ Log in
   - ✅ Create a project
   - ✅ Add a team member
   - ✅ Create a ticket
   - ✅ Edit your profile
   - ✅ Check activity feed

Everything working? **Congratulations!** 🎊

---

## 🆘 Troubleshooting

### Problem: "Can't connect to API"
**Solution**: 
1. Check browser console (F12 → Console tab)
2. Look for CORS error
3. Go to Backend Vercel project
4. Verify CORS_ORIGIN matches your frontend URL exactly
5. Redeploy backend

### Problem: "Login fails"
**Solution**:
1. Check that SUPABASE_KEY is correct in backend
2. Verify Supabase project is still active
3. Check backend logs: Deployments → click deployment → Logs

### Problem: "Shows old version of page"
**Solution**: Hard refresh (Ctrl+Shift+Delete or Cmd+Shift+Delete) → clear all cache

### Problem: "Build failed"
**Solution**:
1. Check Vercel logs: Deployments → latest → Build Logs
2. Check for errors
3. Try building locally: `npm run build` in that directory
4. Fix errors locally and push to GitHub
5. Vercel will automatically redeploy

---

## 📱 Share Your App

Your application is now live at:
```
https://bug-tracker-frontend-xyz.vercel.app
```

You can:
- Share this link with anyone
- They can sign up and use it
- No setup needed on their end

---

## 🔐 Security Note

**Verify `.env` files are NOT in GitHub**:
1. Go to your GitHub repo
2. Verify you don't see `.env` files in the code
3. You should only see `.env.production` (which is template only)

If you see `.env` files:
```powershell
git rm --cached backend/.env
git rm --cached frontend/.env
git commit -m "Remove .env files"
git push
```

---

## 📊 Monitor Your App

### Check Deployment Status
- Go to Vercel project
- Click "Deployments"
- Green = working, Red = error

### View Logs
- Click on any deployment
- "Logs" tab shows what happened
- Helps debug issues

### Performance Analytics
- Click "Analytics" tab
- See how fast your app loads
- Monitor Core Web Vitals

---

## 🚀 Next Steps (Optional)

### Custom Domain
Instead of `bug-tracker-frontend-xyz.vercel.app`, use `myapp.com`:
1. Buy domain (GoDaddy, Namecheap, etc.)
2. In Vercel project → Settings → Domains
3. Add your domain
4. Follow DNS instructions from your registrar

### Automatic Backups
1. Go to Supabase dashboard
2. Project Settings → Backups
3. Toggle automatic backups ON
4. Your database is now backed up daily

### Team Collaboration
1. In Vercel project → Settings → Team
2. Click "Invite"
3. Share project with team members
4. They can help manage deployments

---

## 📞 Help & Support

- **Vercel Issues?** → https://vercel.com/docs
- **Supabase Issues?** → https://supabase.com/docs
- **GitHub Issues?** → Contact GitHub support
- **Report Bugs**: Post in your GitHub Issues tab

---

## Summary

What you just did:
1. ✅ Pushed code to GitHub
2. ✅ Deployed Backend API on Vercel
3. ✅ Deployed Frontend UI on Vercel
4. ✅ Configured connection between frontend and backend
5. ✅ Made your app publicly accessible

**Your Bug Tracker is now live on the internet!** 🌍

---

**Deployment Date**: April 18, 2026  
**Time to Deploy**: ~15 minutes  
**Status**: ✅ PRODUCTION LIVE
