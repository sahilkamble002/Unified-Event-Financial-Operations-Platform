# Vercel Deployment Guide

This project is configured for deployment on Vercel. Follow these steps to deploy your ERP application.

## Prerequisites

- Vercel account (sign up at https://vercel.com)
- Git repository connected to your Vercel project
- PostgreSQL database (recommended: Neon or Vercel Postgres)

## Deployment Steps

### 1. Connect Your Repository to Vercel

```bash
npm i -g vercel
vercel link
```

### 2. Set Up Environment Variables

Add these environment variables in your Vercel project settings:

```
DATABASE_URL=postgresql://user:password@host/database
CORS_ORIGIN=https://your-domain.com
JWT_SECRET=your_long_random_secret_key
JWT_EXPIRE=7d
BCRYPT_ROUND=10
RAZORPAY_KEY_ID=your_razorpay_key
RAZORPAY_KEY_SECRET=your_razorpay_secret
NODE_ENV=production
```

### 3. Database Setup

For PostgreSQL, you can use:
- **Neon** (https://neon.tech) - PostgreSQL as a service
- **Vercel Postgres** (https://vercel.com/docs/storage/postgres)
- **Render.com** - Free PostgreSQL hosting
- **AWS RDS**

### 4. Deploy

```bash
vercel --prod
```

Or push to your connected Git repository, and Vercel will auto-deploy.

## Project Structure

```
├── backend/           # Express.js API
│   ├── src/
│   ├── prisma/
│   └── package.json
├── frontend/          # React frontend
│   ├── src/
│   └── package.json
├── vercel.json       # Vercel configuration
└── package.json      # Root package.json
```

## How It Works

- **Frontend**: Built as static assets and served from Vercel's CDN
- **Backend**: Runs as a Node.js server on Vercel
- **Database**: Connected via Prisma ORM to your PostgreSQL instance

## Important Notes

1. **Database Migrations**: Automatically runs `prisma migrate deploy` during build
2. **CORS**: Configured to accept requests from your Vercel domain
3. **Build Output**: Frontend builds to `frontend/dist` directory
4. **API Routes**: All requests to `/api/*` are routed to the backend

## Troubleshooting

### Build Fails
- Check that all environment variables are set
- Ensure DATABASE_URL is correct and accessible
- Check backend logs: `mkdir -p .vercel && vercel logs`

### API Not Working
- Verify CORS_ORIGIN is set to your deployed frontend URL
- Check that the database is accessible from Vercel's servers
- Enable "Whitelist IPs" in your database if using external connections

### Frontend Not Loading
- Verify frontend builds locally: `npm run build --prefix frontend`
- Check that the build output is in `frontend/dist`

## Local Development

### Development Mode

```bash
# Install dependencies
npm install

# Start both frontend and backend
npm run dev
```

### Backend Only

```bash
cd backend
npm install
npm run dev
```

### Frontend Only

```bash
cd frontend
npm install
npm run dev
```

## Environment Variables Reference

| Variable | Purpose | Example |
|----------|---------|---------|
| DATABASE_URL | PostgreSQL connection string | postgresql://user:pass@host/db |
| CORS_ORIGIN | Frontend URL for CORS | https://example.com |
| JWT_SECRET | Secret key for JWT tokens | any_secret_string |
| JWT_EXPIRE | JWT token expiration | 7d |
| BCRYPT_ROUND | Password hashing rounds | 10 |
| RAZORPAY_KEY_ID | Razorpay API key | razorpay_key_xxx |
| RAZORPAY_KEY_SECRET | Razorpay API secret | razorpay_secret_xxx |
| NODE_ENV | Environment | production |

## Useful Vercel Commands

```bash
# Link current directory to Vercel project
vercel link

# Deploy to production
vercel --prod

# View logs
vercel logs

# View current environment variables
vercel env ls

# Set environment variable
vercel env add VARIABLE_NAME

# Remove environment variable
vercel env rm VARIABLE_NAME
```

## Support

For deployment issues:
- Check Vercel documentation: https://vercel.com/docs
- View build logs in Vercel dashboard
- Check database connectivity and permissions
