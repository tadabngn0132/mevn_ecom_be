# Frontend Deployment Guide

## Environment Configuration

This project uses environment variables to manage different API URLs for development and production.

### Files:

- `.env.development` - Used when running `npm run dev`
- `.env.production` - Used when running `npm run build`
- `.env.example` - Template file (not used by the app)

### Current Configuration:

**Development:**

```
VITE_API_URL=http://localhost:4000/api/product
```

**Production:**

```
VITE_API_URL=https://ecom-server-l301.onrender.com/api/product
```

## Deployment Steps

### 1. Build for Production

```bash
npm run build
```

### 2. Test Production Build Locally

```bash
npm run preview
```

### 3. Deploy to Hosting Service

#### Option A: Vercel

```bash
npm install -g vercel
vercel
```

#### Option B: Netlify

```bash
npm install -g netlify-cli
netlify deploy --prod
```

#### Option C: GitHub Pages

Update `vite.config.js`:

```javascript
export default defineConfig({
  base: "/your-repo-name/",
  // ... rest of config
});
```

Then build and push to gh-pages branch.

### 4. Environment Variables on Hosting Platform

If deploying to Vercel/Netlify, set environment variable:

- Key: `VITE_API_URL`
- Value: `https://ecom-server-l301.onrender.com/api/product`

## Important Notes

⚠️ **CORS Configuration:**
Make sure your backend allows requests from your frontend domain.

⚠️ **HTTPS:**
If frontend is on HTTPS, backend must also be on HTTPS (Render provides this).

⚠️ **API URL:**
Always use HTTPS URL for production backend.

## Troubleshooting

### Issue: "Failed to load products"

- Check if backend is running: https://ecom-server-l301.onrender.com/api/product
- Check browser console for CORS errors
- Verify environment variables are set correctly

### Issue: Mixed Content Error

- Ensure backend URL uses HTTPS, not HTTP
- Check browser console for security warnings

## Local Development

```bash
# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build
```

The app will automatically use the correct API URL based on the environment.
