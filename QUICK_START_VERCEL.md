# Quick Start: Deploy to Vercel in 5 Minutes

## 1. Prepare Your Code

```bash
# From project root
git add .
git commit -m "Prepare for Vercel deployment"
git push
```

## 2. Connect to Vercel

```bash
npm i -g vercel
vercel link
```

Follow the prompts and select your Git repository.

## 3. Configure Environment Variables

In the Vercel dashboard (https://vercel.com/dashboard):

1. Select your project
2. Go to **Settings** → **Environment Variables**
3. Add these variables:

```
DATABASE_URL = postgresql://username:password@host:port/database
CORS_ORIGIN = https://your-project.vercel.app
JWT_SECRET = (generate a random string, min 32 chars)
JWT_EXPIRE = 7d
BCRYPT_ROUND = 10
RAZORPAY_KEY_ID = (optional)
RAZORPAY_KEY_SECRET = (optional)
NODE_ENV = production
```

## 4. Deploy

```bash
vercel --prod
```

Wait 2-3 minutes for the build to complete.

## 5. Verify

1. Visit your URL: `https://your-project.vercel.app`
2. Test login
3. Check browser console (F12) for errors
4. Check Vercel logs if issues: `vercel logs`

## 🎉 Done!

Your ERP is now live on Vercel!

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Build fails | Check `vercel logs` and env variables |
| Blank page | Clear cache (Ctrl+Shift+R), check build logs |
| API errors | Verify `DATABASE_URL` and `CORS_ORIGIN` |
| Auth fails | Ensure `JWT_SECRET` is set |

For detailed steps, see [DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md)
