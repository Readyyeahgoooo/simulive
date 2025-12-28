# Xiaomi MiMo-V2-Flash Model Guide

## What Was Fixed

### 1. Model Configuration
- ✅ Changed from `deepseek/deepseek-r1` to `xiaomi/mimo-v2-flash:free`
- ✅ This is the free model you wanted to use

### 2. JSON Response Handling
- ✅ Added explicit JSON format instructions to the AI
- ✅ Improved error messages when JSON parsing fails
- ✅ Better validation of API responses

### 3. Error Handling
- ✅ More detailed error messages
- ✅ Console logging for debugging
- ✅ Specific error types (rate limit, invalid response, API errors)

## Important Notes About Free Models

### Xiaomi MiMo-V2-Flash Characteristics:

**Pros:**
- ✅ Completely free to use
- ✅ No rate limits (usually)
- ✅ Fast responses

**Cons:**
- ⚠️ May not always return perfect JSON format
- ⚠️ Less reliable than paid models (GPT-4, Claude, etc.)
- ⚠️ May occasionally fail or return unexpected responses
- ⚠️ Less sophisticated reasoning than premium models

### What This Means For Your App:

1. **"Sync Error" May Still Happen**
   - Free models are less consistent
   - If you get a sync error, just try again
   - The error message will now tell you what went wrong

2. **Simpler Gameplay**
   - The free model may not create as rich narratives as Gemini Pro
   - Questions may be simpler
   - Story continuity may be less sophisticated

3. **Recommended Alternatives** (if you want better quality):

| Model | Cost | Quality | Best For |
|-------|------|---------|----------|
| `xiaomi/mimo-v2-flash:free` | Free | Basic | Testing, casual use |
| `meta-llama/llama-3-8b-instruct:free` | Free | Better | Free alternative |
| `anthropic/claude-3-haiku` | ~$0.25/1M tokens | Good | Production use |
| `openai/gpt-4o-mini` | ~$0.15/1M tokens | Great | Best balance |
| `anthropic/claude-3.5-sonnet` | ~$3/1M tokens | Excellent | Premium experience |

## How to Change Models

### Option 1: Via Vercel Environment Variable

1. Go to Vercel Dashboard → Your Project → Settings → Environment Variables
2. Change `OPENROUTER_MODEL` to any model from the table above
3. Redeploy

### Option 2: Via Code (for testing)

Edit `services/geminiService.ts`:

```typescript
export class GeminiService {
  private model = "meta-llama/llama-3-8b-instruct:free"; // Change this line
```

## Testing Your Deployment

### Step 1: Try Starting a Simulation

1. Visit your Vercel URL
2. Fill out the setup form
3. Click "Initialize"
4. **Expected:** You should see a story/scenario appear

### Step 2: If You Get "Sync Error"

**Check the browser console (F12):**

1. Look for error messages
2. Common issues:
   - "Invalid response structure" → Model didn't return JSON
   - "API request failed" → OpenRouter API issue
   - "Rate limit" → Too many requests

**Solutions:**
- Try again (free models can be flaky)
- Check your OpenRouter API key is valid
- Try a different model (see table above)

### Step 3: Test Multiple Turns

1. After the first event appears, try responding
2. Type something or click an option
3. **Expected:** New event should appear with updated stats

## Debugging Tips

### Enable Detailed Logging

Open browser console (F12) before starting simulation. You'll see:

```
Simulation start error: [detailed error]
API Error: [API response]
Invalid response structure: [what was received]
```

### Check OpenRouter Dashboard

1. Go to https://openrouter.ai/activity
2. See your API calls
3. Check for errors or rate limits

### Test API Directly

You can test if your API key works:

```bash
curl https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "xiaomi/mimo-v2-flash:free",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

## Expected Behavior

### ✅ Working Correctly:

1. Setup form loads
2. Click "Initialize" → Loading spinner appears
3. After 2-5 seconds → Story/scenario appears
4. Stats show on sidebar
5. You can type responses or click options
6. New events appear after each turn

### ❌ Not Working:

1. "Sync error" appears immediately
2. Loading spinner never stops
3. Blank screen after clicking Initialize
4. Console shows errors

## Comparison: Gemini vs Xiaomi

| Feature | Gemini Pro (Original) | Xiaomi MiMo (Current) |
|---------|----------------------|----------------------|
| Cost | Paid | Free |
| JSON Reliability | Excellent | Good |
| Story Quality | Excellent | Basic |
| Response Time | Fast | Fast |
| PDF Support | Native | Not supported |
| Consistency | Very High | Moderate |

## Recommendations

### For Testing/Development:
- ✅ Use `xiaomi/mimo-v2-flash:free` (current setup)
- It's free and good enough for testing

### For Production/Real Use:
- 💰 Consider `openai/gpt-4o-mini` (~$0.15/1M tokens)
- Much more reliable
- Better story quality
- Consistent JSON responses
- Still affordable

### For Premium Experience:
- 💎 Use `anthropic/claude-3.5-sonnet`
- Best quality
- Most reliable
- Closest to original Gemini Pro experience

## Next Steps

1. **Test your current deployment** with Xiaomi model
2. **If it works:** Great! You have a free working app
3. **If it's too unreliable:** Consider upgrading to a paid model
4. **Monitor costs:** Check OpenRouter dashboard regularly

## Getting Help

If you continue to have issues:

1. Check browser console for specific errors
2. Check Vercel function logs
3. Test your OpenRouter API key
4. Try a different model
5. Check OpenRouter status page

Remember: Free models are great for testing, but paid models provide a much better user experience!
