# Vercel Deployment Checklist

## Pre-Deployment

- [ ] Update all dependencies: `npm install`
- [ ] Test locally: `npm run dev` (backend) and `npm run dev` (frontend)
- [ ] Verify `.env` files are in `.gitignore`
- [ ] Commit all changes to git: `git add .` and `git commit -m "Prepare for Vercel deployment"`
- [ ] Push to your main branch: `git push origin main`

## Vercel Setup

- [ ] Create/login to Vercel account at https://vercel.com
- [ ] Install Vercel CLI: `npm i -g vercel`
- [ ] Link project: `vercel link` (from project root)
- [ ] Select your Git repository

## Environment Variables in Vercel Dashboard

Go to your Vercel project settings and add these environment variables:

### Database
- [ ] `DATABASE_URL` - Your PostgreSQL connection string
  - Format: `postgresql://user:password@host:5432/database_name`
  - Should include connection pooling parameters if needed
  
### Frontend
- [ ] `CORS_ORIGIN` - Your Vercel deployment URL (e.g., `https://yourapp.vercel.app`)

### Authentication
- [ ] `JWT_SECRET` - Generate a long random string (min 32 characters)
- [ ] `JWT_EXPIRE` - Set to: `7d`
- [ ] `BCRYPT_ROUND` - Set to: `10`

### Payment (Optional - if using Razorpay)
- [ ] `RAZORPAY_KEY_ID` - Your Razorpay API key
- [ ] `RAZORPAY_KEY_SECRET` - Your Razorpay API secret

### Environment
- [ ] `NODE_ENV` - Set to: `production`
- [ ] `PORT` - Set to: `5000` (or leave empty for default)

## Database Setup

Choose one:

### Option 1: Neon (Recommended for PostgreSQL)
1. Go to https://neon.tech
2. Create a new project
3. Copy the connection string
4. Set as `DATABASE_URL` in Vercel
5. Run initial migrations in Vercel dashboard

### Option 2: Vercel Postgres
1. In Vercel dashboard, go to Storage → Postgres
2. Create a new database
3. Connection string auto-populated
4. Migrations run automatically during build

### Option 3: Other Providers (Render, AWS RDS, etc.)
1. Create PostgreSQL database
2. Whitelist Vercel's IP if required
3. Add connection string as `DATABASE_URL`

## Database Verification

- [ ] Test connection string locally first
- [ ] Run migrations: `npm run build --prefix backend`
- [ ] Verify schema is created: Check database with your client tool

## Deploy

```bash
# Option 1: Deploy to staging
vercel

# Option 2: Deploy to production
vercel --prod
```

- [ ] Wait for build to complete (about 2-3 minutes)
- [ ] Check build logs for any errors
- [ ] Visit your deployment URL
- [ ] Test login functionality
- [ ] Test API calls to ensure backend is working

## Post-Deployment Testing

- [ ] Frontend loads and displays correctly
- [ ] All UI elements render properly
- [ ] Login/authentication works
- [ ] API calls function correctly
- [ ] Database reads/writes work
- [ ] File uploads work if applicable
- [ ] Payment integration works (if configured)
- [ ] Error messages display properly

## Troubleshooting

### Build Fails
```bash
# Check build logs
vercel logs --since 1h

# Redeploy
vercel --prod
```

### Database Connection Issues
- [ ] Verify `DATABASE_URL` format is correct
- [ ] Check database is online and accessible
- [ ] Verify IP whitelisting if applicable
- [ ] Check connection pool limits

### Frontend Not Loading
- [ ] Verify `frontend/dist` folder exists after build
- [ ] Check `CORS_ORIGIN` matches your Vercel URL
- [ ] Hard refresh browser (Ctrl+Shift+R)

### API 404 Errors
- [ ] Verify backend server is running
- [ ] Check that `CORS_ORIGIN` is set correctly
- [ ] Verify API routes are correct (`/api/v1/*`)
- [ ] Check backend logs: `vercel logs`

### CORS Errors
- [ ] Add your Vercel URL to `CORS_ORIGIN`
- [ ] Format: `https://your-app.vercel.app`
- [ ] Redeploy after updating: `vercel --prod`

## Useful Commands

```bash
# View deployment logs
vercel logs

# View environment variables
vercel env ls

# Add new environment variable
vercel env add VARIABLE_NAME

# Redeploy
vercel --prod

# View all deployments
vercel list

# Remove deployment
vercel remove my-project-name
```

## Quick Reference

| Issue | Solution |
|-------|----------|
| "Build failed" | Check logs with `vercel logs`, verify env vars set |
| "Cannot connect to database" | Verify `DATABASE_URL` and database is online |
| "Frontend blank" | Clear cache, verify build succeeded, check console errors |
| "API calls fail" | Verify `CORS_ORIGIN`, check backend logs |
| "Auth not working" | Verify `JWT_SECRET` is set, check token endpoint |

## Success Indicators ✓

- [ ] Build completes without errors
- [ ] Frontend loads and renders correctly  
- [ ] Can login to the application
- [ ] API calls return data
- [ ] Database operations work
- [ ] No CORS errors in browser console
- [ ] No 500 errors from backend

## Rollback (if needed)

```bash
# View previous deployments
vercel list

# Promote a previous deployment
vercel promote <deployment-url>
```

---

**Need Help?**
- Vercel Docs: https://vercel.com/docs
- Check build logs: `vercel logs`
- Contact support through Vercel dashboard
