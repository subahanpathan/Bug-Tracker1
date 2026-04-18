# Bug Tracker - Vercel Deployment Guide

## Overview
This guide covers deploying the Bug Tracker application to Vercel. The project consists of:
- **Backend**: Node.js/Express API (deployed as serverless functions)
- **Frontend**: React/Vite SPA (deployed as static site)

## Prerequisites
- GitHub account (repository for deploying from)
- Vercel account (free: https://vercel.com)
- Supabase project already set up and running

## Step 1: Prepare Your Repository

### 1.1 Initialize Git (if not already done)
```bash
cd "c:\Users\Hariom\Downloads\BUG TRACKER"
git init
git add .
git commit -m "Initial commit: Bug Tracker app"
git remote add origin https://github.com/YOUR_USERNAME/bug-tracker.git
git branch -M main
git push -u origin main
```

### 1.2 Create .gitignore if needed
Both `backend/.gitignore` and `frontend/.gitignore` should already exist. Verify they exclude:
- `node_modules/`
- `.env`
- `.env.production`
- `dist/`
- `build/`

## Step 2: Deploy Backend to Vercel

### 2.1 Create Vercel Project for Backend
1. Go to https://vercel.com/dashboard
2. Click "Add New" → "Project"
3. Import your GitHub repository
4. Select the root of your repository
5. Configure project settings:
   - **Framework Preset**: Other
   - **Root Directory**: `backend`
   - **Build Command**: `npm install`
   - **Output Directory**: Leave empty
   - **Install Command**: `npm install`
   - **Start Command**: `npm start`

### 2.2 Set Environment Variables
In Vercel project settings, go to "Environment Variables" and add:

```
SUPABASE_URL = your_supabase_url
SUPABASE_KEY = your_supabase_key
SUPABASE_SERVICE_KEY = your_supabase_service_key
JWT_SECRET = your_jwt_secret
JWT_EXPIRE = 7d
NODE_ENV = production
CORS_ORIGIN = https://your-frontend-domain.vercel.app
MAX_FILE_SIZE = 5242880
UPLOAD_DIR = /tmp/uploads/
```

**Important**: Get these values from your local `.env` file:
- SUPABASE_URL, SUPABASE_KEY, SUPABASE_SERVICE_KEY from your Supabase project
- JWT_SECRET: Use the same value as local development or generate a new secure one

### 2.3 Deploy Backend
1. Click "Deploy" button in Vercel
2. Wait for deployment to complete
3. Copy the deployment URL (e.g., `https://bug-tracker-api.vercel.app`)
4. Update CORS_ORIGIN if needed (you'll need this for the frontend)

**Note**: After frontend deployment URL is known, update CORS_ORIGIN again with the correct frontend URL.

## Step 3: Deploy Frontend to Vercel

### 3.1 Create Vercel Project for Frontend
1. Go to https://vercel.com/dashboard
2. Click "Add New" → "Project"
3. Import your GitHub repository
4. Configure project settings:
   - **Framework Preset**: Vite
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`

### 3.2 Set Environment Variables
In Vercel project settings, go to "Environment Variables" and add:

```
VITE_API_URL = https://bug-tracker-api.vercel.app/api
VITE_ENV = production
```

Replace `https://bug-tracker-api.vercel.app` with your actual backend URL from Step 2.3.

### 3.3 Deploy Frontend
1. Click "Deploy" button in Vercel
2. Wait for deployment to complete
3. Copy the deployment URL (e.g., `https://bug-tracker-frontend.vercel.app`)

## Step 4: Update CORS on Backend

After frontend deployment is complete:

1. Go to your backend Vercel project
2. Go to Settings → Environment Variables
3. Update `CORS_ORIGIN` to include your frontend URL:
   ```
   CORS_ORIGIN = https://bug-tracker-frontend.vercel.app
   ```
4. Redeploy the backend from the Deployments tab
5. Click "..." → "Redeploy"

## Step 5: Verify Deployment

### Test the Application
1. Visit your frontend URL: `https://bug-tracker-frontend.vercel.app`
2. Create an account or login
3. Try these operations:
   - Create a project ✅
   - Add team member ✅
   - Create a ticket ✅
   - Update profile ✅
   - Delete project ✅

### Check Backend Logs
In Vercel dashboard:
1. Go to backend project
2. Click "Deployments"
3. Click the latest deployment
4. Go to "Logs" to see API requests and errors

## Troubleshooting

### CORS Errors
- **Problem**: "Access to XMLHttpRequest blocked by CORS"
- **Solution**: Check that CORS_ORIGIN in backend includes the frontend URL exactly

### 401 Unauthorized Errors
- **Problem**: Authentication fails
- **Solution**: Ensure JWT_SECRET is the same on backend and matches the token generation

### Supabase Connection Errors
- **Problem**: "Failed to connect to Supabase"
- **Solution**: Verify SUPABASE_URL and SUPABASE_KEY are correct in backend environment variables

### Frontend Build Errors
- **Problem**: Build fails on Vercel but works locally
- **Solution**: 
  - Check Node.js version (Vercel uses Node 18+ by default)
  - Clear Vercel cache: go to Settings → Deployments → "Clear Cache"

### Uploads Not Working
- **Problem**: File uploads fail
- **Solution**: Vercel serverless functions have 50MB request size limit. Update MAX_FILE_SIZE if needed.

## Custom Domain (Optional)

To use a custom domain:

1. **For Backend**:
   - Go to backend Vercel project
   - Settings → Domains
   - Add your domain (e.g., `api.yoursite.com`)
   - Update frontend VITE_API_URL to `https://api.yoursite.com/api`

2. **For Frontend**:
   - Go to frontend Vercel project
   - Settings → Domains
   - Add your domain (e.g., `yoursite.com`)

3. **Update CORS**:
   - Backend CORS_ORIGIN: Add both custom domains

## Database Backups

Since you're using Supabase, backups are automatically managed. To manually backup:

1. Go to Supabase dashboard
2. Project Settings → Backups
3. Click "Backup now"

## Environment Variable Best Practices

### Never Commit Secrets
- `.env` files should be in `.gitignore`
- Add all sensitive values through Vercel UI
- Use Vercel's secret management system

### Rotate Keys Periodically
- Every 3-6 months, rotate JWT_SECRET
- After rotating, redeploy both backend and frontend

### Different Values for Different Environments
```
Production: Use real, secure values
Staging: Use test database with test values
Local Development: Use local values
```

## Performance Monitoring

After deployment, monitor performance:

1. Vercel Analytics (built-in):
   - Go to your project
   - Tab "Analytics" to view Core Web Vitals

2. Supabase Metrics:
   - Go to Supabase dashboard
   - View database performance and API usage

## Rollback Deployment

If something goes wrong:

1. Go to Vercel project
2. Click "Deployments"
3. Find the previous working deployment
4. Click "..." → "Promote to Production"

## Local Development vs Production

### Environment Variables
- Local: Use `.env` file with localhost URLs
- Production: Use Vercel UI with production URLs

### API URLs
- Local: `http://localhost:5000/api`
- Production: `https://bug-tracker-api.vercel.app/api`

### Build vs Dev
- Local: `npm run dev` (hot reload, unoptimized)
- Production: `npm run build` (optimized, minified)

## CI/CD Pipeline

Vercel automatically:
- Deploys on every push to main
- Runs build command automatically
- Sets environment variables from UI
- Provides preview deployments for PR branches

To see deployments:
1. Go to your Vercel project
2. Click "Deployments" tab
3. Each commit will show a deployment status

## FAQ

**Q: Can I use a free Vercel account?**
A: Yes! Free plan includes:
- 10 projects
- Unlimited bandwidth
- Automated deployments
- Email support

**Q: What are the limits on Vercel?**
A: Free tier limits:
- 100 GB-hours of serverless function execution per month
- 1GB of artifact storage
- Max 50MB function size

**Q: How long does deployment take?**
A: Typically 2-5 minutes for initial deployment, 30 seconds-2 minutes for updates.

**Q: Can I deploy from a local machine?**
A: Yes, install Vercel CLI:
```bash
npm install -g vercel
vercel login
vercel deploy
```

**Q: How do I set up automatic deployments?**
A: Already configured! Push to GitHub and Vercel automatically deploys.

## Next Steps

1. ✅ Set up GitHub repository
2. ✅ Deploy backend first
3. ✅ Deploy frontend
4. ✅ Test all functionality
5. ✅ Monitor performance
6. ✅ Set up custom domain (optional)
7. ✅ Configure backups

## Support

- Vercel Documentation: https://vercel.com/docs
- Supabase Documentation: https://supabase.com/docs
- GitHub Issues: Report bugs in your repository

---

**Deployment Date**: April 18, 2026
**Last Updated**: April 18, 2026
