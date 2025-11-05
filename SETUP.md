# Quick Setup Guide

## ✅ What Has Been Configured

### 1. Backend API Connection

- **Production URL**: `https://ecom-server-l301.onrender.com/api/product`
- **Development URL**: `http://localhost:4000/api/product`

### 2. Environment Files Created

- `.env.development` - For local development
- `.env.production` - For production build
- `.env.example` - Template file

### 3. Features Added

- ✅ Loading spinner while fetching data
- ✅ Error handling with retry button
- ✅ Automatic environment detection
- ✅ Better error messages

## 🚀 How to Use

### For Development (Local)

```bash
npm run dev
```

This will use `http://localhost:4000/api/product`

### For Production Build

```bash
npm run build
```

This will use `https://ecom-server-l301.onrender.com/api/product`

### Test Production Build Locally

```bash
npm run preview
```

## 📝 Important Notes

### Before Deploying Frontend:

1. **Test Backend Connection**

   - Open: https://ecom-server-l301.onrender.com/api/product
   - Should return JSON with products

2. **Check CORS**

   - Backend should allow requests from your frontend domain
   - Current backend already has `app.use(cors())` enabled

3. **Build the Project**

   ```bash
   npm run build
   ```

4. **Deploy the `dist` folder** to your hosting service

### Common Hosting Platforms:

#### Vercel (Recommended)

```bash
npm install -g vercel
vercel
```

#### Netlify

```bash
npm install -g netlify-cli
netlify deploy --prod
```

#### GitHub Pages

- Push `dist` folder to `gh-pages` branch
- Enable GitHub Pages in repository settings

## 🔧 Troubleshooting

### Issue: "Failed to load products"

**Solutions:**

1. Check if backend is online: https://ecom-server-l301.onrender.com/api/product
2. Check browser console for errors
3. Click "Try Again" button
4. Verify CORS is enabled on backend

### Issue: "Mixed Content" Error

**Solution:**

- Make sure backend URL uses HTTPS (already configured)
- Check browser console for security warnings

### Issue: Environment variables not working

**Solution:**

1. File names must be exact: `.env.production` or `.env.development`
2. Variables must start with `VITE_`
3. Rebuild project after changing env files
4. Clear browser cache

## 📦 Project Structure

```
frontend/
├── .env.development      # Dev environment
├── .env.production       # Prod environment
├── .env.example          # Template
├── src/
│   └── App.vue          # Updated with new API URL
├── DEPLOYMENT.md        # Detailed deployment guide
└── package.json
```

## ✨ Next Steps

1. **Test Locally**

   ```bash
   npm run dev
   ```

2. **Build for Production**

   ```bash
   npm run build
   ```

3. **Deploy**
   - Choose your hosting platform
   - Upload the `dist` folder
   - Enjoy! 🎉

## 📞 Need Help?

If you encounter issues:

1. Check browser console (F12)
2. Verify backend is running
3. Check CORS configuration
4. Review error messages in the UI

---

**Backend URL**: https://ecom-server-l301.onrender.com
**API Endpoint**: https://ecom-server-l301.onrender.com/api/product
