# Vercel Deployment Checklist

## Pre-Deployment ✅

### Code Quality
- [ ] All features tested locally
- [ ] No console errors in browser
- [ ] No console errors in backend
- [ ] All git changes committed

### Backend Preparation
- [ ] `backend/.env` has all required values
- [ ] `backend/vercel.json` created
- [ ] `backend/.gitignore` excludes `.env`
- [ ] Node version compatible (14+ required)
- [ ] All dependencies in `package.json`

### Frontend Preparation
- [ ] `frontend/.env` has VITE_API_URL set to localhost
- [ ] `frontend/vercel.json` created
- [ ] `frontend/.gitignore` excludes `.env` and `dist/`
- [ ] Build works locally: `npm run build`
- [ ] All dependencies in `package.json`

### Repository
- [ ] GitHub account created
- [ ] Repository created and pushed
- [ ] `.gitignore` prevents `.env` files from being committed
- [ ] Branch is `main` or `master`

## Deployment Phase ✅

### Backend Deployment
- [ ] Vercel account created (free is fine)
- [ ] Backend project imported to Vercel
- [ ] Root directory set to `backend`
- [ ] Build command: `npm install` and Start: `npm start`
- [ ] Environment variables added:
  - [ ] SUPABASE_URL
  - [ ] SUPABASE_KEY
  - [ ] SUPABASE_SERVICE_KEY
  - [ ] JWT_SECRET
  - [ ] JWT_EXPIRE=7d
  - [ ] NODE_ENV=production
  - [ ] MAX_FILE_SIZE=5242880
  - [ ] UPLOAD_DIR=/tmp/uploads/
- [ ] First deployment successful
- [ ] Backend URL copied (e.g., https://bug-tracker-api.vercel.app)

### Frontend Deployment
- [ ] Frontend project imported to Vercel
- [ ] Root directory set to `frontend`
- [ ] Framework: Vite
- [ ] Build command: `npm run build`
- [ ] Output directory: `dist`
- [ ] Environment variables added:
  - [ ] VITE_API_URL = {backend_url}/api
  - [ ] VITE_ENV=production
- [ ] First deployment successful
- [ ] Frontend URL copied (e.g., https://bug-tracker-frontend.vercel.app)

## Post-Deployment ✅

### CORS Configuration
- [ ] Backend CORS_ORIGIN updated with frontend URL
- [ ] Backend redeployed after CORS update
- [ ] Frontend VITE_API_URL updated with backend URL
- [ ] Frontend redeployed after API URL update

### Functional Testing
- [ ] Frontend loads without errors
- [ ] Login page appears
- [ ] Can create account
- [ ] Can login
- [ ] Can navigate dashboard
- [ ] Can create project
- [ ] Can add team member
- [ ] Can create ticket
- [ ] Can edit profile
- [ ] Can delete project
- [ ] Activity feed shows actions
- [ ] Email preferences appear
- [ ] No CORS errors in browser console
- [ ] No 401 authentication errors

### Performance Verification
- [ ] Page loads in < 3 seconds
- [ ] API responses in < 1 second
- [ ] No memory leaks in Vercel logs
- [ ] File uploads work (if applicable)

### Security Check
- [ ] No `.env` file in GitHub
- [ ] JWT tokens working correctly
- [ ] HTTPS enabled (automatic on Vercel)
- [ ] CORS properly configured
- [ ] No sensitive data in logs

### Monitoring Setup
- [ ] Vercel Analytics enabled
- [ ] Check Core Web Vitals
- [ ] Review error logs
- [ ] Set up deployment notifications

## Troubleshooting Checklist ❌

If something doesn't work:

### CORS Errors
- [ ] Check frontend URL in CORS_ORIGIN (exact match required)
- [ ] Check for typos in environment variable
- [ ] Redeploy backend after changing CORS
- [ ] Hard refresh browser (Ctrl+Shift+Delete)

### 401 Errors
- [ ] Check JWT_SECRET is set in backend
- [ ] Verify token is being sent in Authorization header
- [ ] Check token isn't expired

### API Connection Issues
- [ ] Verify VITE_API_URL in frontend is correct
- [ ] Check backend is deployed and running
- [ ] Review backend logs in Vercel Deployments tab
- [ ] Check network tab in browser DevTools

### Build Failures
- [ ] Check build logs in Vercel Deployments
- [ ] Verify all dependencies are in package.json
- [ ] Try local build: `npm run build`
- [ ] Check Node version compatibility

### Database Connection Issues
- [ ] Verify SUPABASE_URL and SUPABASE_KEY are correct
- [ ] Check Supabase project is active
- [ ] Review Supabase logs for errors
- [ ] Test connection locally first

## Documentation ✅

- [ ] VERCEL_DEPLOYMENT.md created
- [ ] VERCEL_QUICK_START.md created
- [ ] Environment variables documented
- [ ] Custom domain instructions (if applicable)
- [ ] Rollback procedures documented

## Ongoing Maintenance ✅

### Regular Tasks
- [ ] Monitor Vercel Analytics monthly
- [ ] Check for dependency updates quarterly
- [ ] Review security advisories
- [ ] Test database backups monthly

### Before Major Updates
- [ ] Test locally first
- [ ] Create staging deployment
- [ ] Review changes
- [ ] Deploy to production
- [ ] Test thoroughly
- [ ] Monitor for errors

## Team Communication ✅

- [ ] Share frontend URL with team
- [ ] Share backend API docs if applicable
- [ ] Document how to report issues
- [ ] Provide login credentials for testing
- [ ] Share performance metrics

## Final Sign-Off ✅

- [ ] Product manager approval
- [ ] QA testing complete
- [ ] All features working as expected
- [ ] Performance acceptable
- [ ] Ready for use

---

## Quick Reference

**Frontend URL**: https://bug-tracker-frontend.vercel.app  
**Backend URL**: https://bug-tracker-api.vercel.app  
**Admin Dashboard**: https://vercel.com/dashboard  
**Supabase Dashboard**: https://app.supabase.com  

**Common Commands**:
```bash
# Local development
npm run dev              # Start dev server
npm run build          # Build for production

# Vercel CLI (if needed)
npm install -g vercel  # Install Vercel CLI
vercel login          # Login to Vercel
vercel deploy         # Deploy from local machine
```

---

**Deployment Completed**: [Date]  
**Last Updated**: April 18, 2026  
**Status**: ✅ Production Ready
