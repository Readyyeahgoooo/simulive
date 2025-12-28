# Vercel Deployment Debugging Guide

## Issue: Black Screen on Vercel

If you're seeing a black screen, follow these steps:

### Step 1: Verify Files Are Pushed to Git

Check that these files exist in your GitHub repository:

```
✅ /api/llm.ts
✅ vercel.json
✅ index.html (with <script type="module" src="/index.tsx"></script>)
✅ package.json (with @vercel/node dependency)
```

**Action:** Run these commands to commit the latest changes:

```bash
git add .
git commit -m "Fix index.html for Vercel deployment"
git push origin main
```

### Step 2: Check Vercel Build Logs

1. Go to your Vercel dashboard
2. Click on your project
3. Click on the latest deployment
4. Look for errors in the "Building" section

**Common build errors:**
- Missing dependencies → Run `npm install` locally first
- TypeScript errors → Run `npm run build` locally to check
- Import errors → Check all file paths are correct

### Step 3: Check Vercel Function Logs

1. Go to your Vercel dashboard
2. Click on your project
3. Click "Functions" tab
4. Click on `/api/llm`
5. Look for errors

**Common function errors:**
- "Missing OPENROUTER_API_KEY" → Environment variable not set
- "Module not found" → Dependency issue

### Step 4: Verify Environment Variables

In Vercel Dashboard → Settings → Environment Variables:

| Variable | Value | Environments |
|----------|-------|--------------|
| `OPENROUTER_API_KEY` | Your actual API key | ✅ Production ✅ Preview ✅ Development |
| `OPENROUTER_MODEL` | `xiaomi/mimo-v2-flash:free` | ✅ Production ✅ Preview ✅ Development |

**IMPORTANT:** After adding/changing environment variables, you MUST redeploy!

### Step 5: Force Redeploy

After fixing environment variables:

**Option A: Via Dashboard**
1. Go to "Deployments" tab
2. Click three dots on latest deployment
3. Click "Redeploy"
4. Check "Use existing Build Cache" is UNCHECKED

**Option B: Via Git**
```bash
git commit --allow-empty -m "Force redeploy"
git push origin main
```

### Step 6: Check Browser Console

1. Open your Vercel URL
2. Press F12 (or Cmd+Option+I on Mac)
3. Go to "Console" tab
4. Look for errors

**Common browser errors:**

#### Error: "Failed to fetch /api/llm"
**Cause:** API endpoint not working
**Fix:** Check Vercel function logs, verify environment variables

#### Error: "Unexpected token '<' in JSON"
**Cause:** API returning HTML error page instead of JSON
**Fix:** Check function logs for the actual error

#### Error: "Cannot read property of undefined"
**Cause:** Missing data in response
**Fix:** Check API response format

### Step 7: Test API Endpoint Directly

Visit: `https://your-app.vercel.app/api/llm`

**Expected:** Error message (because it requires POST)
```json
{"error": "Method Not Allowed"}
```

**If you see:** 404 Not Found
**Problem:** API function not deployed
**Fix:** Verify `/api/llm.ts` is in your Git repo

### Step 8: Check Network Tab

1. Open browser DevTools (F12)
2. Go to "Network" tab
3. Reload the page
4. Look for failed requests (red)

**What to check:**
- Is `index-*.js` loading? (Should be 200 OK)
- Is `/api/llm` being called? (Should be 200 or 400/500 with error)
- Are there any CORS errors?

### Step 9: Verify Vercel Configuration

Check `vercel.json` has:

```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "framework": "vite"
}
```

### Step 10: Local Testing

Test locally before deploying:

```bash
# Install dependencies
npm install

# Build the app
npm run build

# Preview the build
npm run preview
```

Visit `http://localhost:4173` - if it works locally but not on Vercel, it's a deployment config issue.

## Quick Checklist

- [ ] Latest code pushed to Git (including index.html fix)
- [ ] `OPENROUTER_API_KEY` set in Vercel
- [ ] `OPENROUTER_MODEL` set in Vercel
- [ ] Redeployed after setting environment variables
- [ ] Build logs show success
- [ ] Function logs show no errors
- [ ] Browser console shows no errors
- [ ] `/api/llm` endpoint exists (returns 405 for GET)

## Still Not Working?

### Check These Common Issues:

1. **Wrong branch deployed**
   - Vercel Settings → Git → Check which branch is set for production

2. **Build cache issue**
   - Redeploy with "Use existing Build Cache" UNCHECKED

3. **API key invalid**
   - Test your OpenRouter key at https://openrouter.ai/playground

4. **Rate limit hit**
   - Check OpenRouter dashboard for usage limits

5. **Vercel region issue**
   - Try deploying to a different region in Vercel settings

## Get Detailed Logs

### Vercel CLI Method:

```bash
# Install Vercel CLI
npm install -g vercel

# Pull environment variables
vercel env pull

# Run locally
vercel dev

# Check logs
vercel logs
```

## Contact Support

If nothing works:

1. Check Vercel Status: https://www.vercel-status.com/
2. Check OpenRouter Status: https://status.openrouter.ai/
3. Vercel Support: https://vercel.com/support

## Success Indicators

When everything works:

✅ Vercel URL loads the setup form
✅ No errors in browser console
✅ Starting simulation generates AI response
✅ Multiple turns work correctly
✅ Stats update properly

## Example Working Deployment

Your deployment should look like this:

1. **Build Output:**
   ```
   ✓ built in 601ms
   dist/index.html
   dist/assets/index-*.js
   ```

2. **Function Output:**
   ```
   ✓ Serverless Function "api/llm.ts" created
   ```

3. **Browser Console:**
   ```
   (No errors)
   ```

4. **Network Tab:**
   ```
   GET / → 200 OK
   GET /assets/index-*.js → 200 OK
   POST /api/llm → 200 OK
   ```
