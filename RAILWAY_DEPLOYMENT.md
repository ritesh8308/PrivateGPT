# Railway Deployment Guide for PrivateGPT with Gemini API

## ✅ Setup Complete!

Your repository is now configured for Railway deployment with Google Gemini AI.

## 📋 Prerequisites

- [Railway Account](https://railway.app) (free)
- GitHub Account (for automatic deployments)
- Google Gemini API Key from [Google AI Studio](https://makersuite.google.com/app/apikey)

## 🚀 Deploy to Railway (3 Steps)

### Step 1: Create Railway Account & Connect GitHub

1. Go to https://railway.app
2. Click **Sign Up** → Choose **Sign up with GitHub**
3. Authorize Railway to access your GitHub account
4. Click **New Project** → **Deploy from GitHub repo**
5. Search for and select **ritesh8308/PrivateGPT**

### Step 2: Add Environment Variables

After connecting your GitHub repo, Railway will ask for environment variables:

**Click "Add Variable" and add the following:**

| Variable Name | Value |
|---|---|
| `GOOGLE_API_KEY` | `AIzaSyD...` (your actual Gemini API key) |
| `PRIVATE_GPT_SETTINGS_FOLDER` | `.` |
| `PGPT_PROFILES` | `gemini` |
| `PYTHONUNBUFFERED` | `1` |

**🔐 Security Note:** Railway encrypts all environment variables. Never commit secrets to Git.

### Step 3: Deploy

1. Railway automatically detects `Procfile` and `requirements.txt`
2. Click **Deploy** button
3. Wait 2-3 minutes for deployment to complete
4. You'll see a public URL like: `https://privatgpt-prod-abc123.railway.app`

## 🧪 Test Your Deployment

Once deployed, test these endpoints:

### Health Check
```bash
curl https://your-railway-url.railway.app/health
```

### Chat with Gemini (No Documents)
```bash
curl -X POST https://your-railway-url.railway.app/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [
      {
        "role": "user",
        "content": "What is artificial intelligence?"
      }
    ],
    "use_context": false
  }'
```

### Get OpenAPI Docs
Open in your browser:
```
https://your-railway-url.railway.app/docs
```

## 📊 Monitor Your Deployment

In Railway Dashboard:
- **Logs** tab: See real-time application logs
- **Metrics** tab: Monitor CPU, memory, and network usage
- **Settings** tab: Update environment variables anytime

To update `GOOGLE_API_KEY`:
1. Go to your project in Railway
2. Click **Variables** 
3. Edit `GOOGLE_API_KEY` 
4. Railway automatically redeploys

## 🔄 Auto-Deployment from GitHub

Every time you push to GitHub:
1. Railway automatically detects changes
2. Rebuilds the application
3. Deploys new version (usually 1-2 minutes)

Example:
```bash
git add .
git commit -m "Update settings"
git push origin main
# Railway automatically deploys!
```

## 📝 Project Structure

```
PrivateGPT/
├── private_gpt/          # Main application code
├── requirements.txt      # Python dependencies ✅
├── Procfile              # Railway startup command ✅
├── railway.json          # Railway configuration ✅
├── settings-gemini.yaml  # Gemini LLM settings ✅
├── .env.example          # Template for env vars ✅
└── ...
```

## 🛠️ Customization

### Change Models

Edit `settings-gemini.yaml`:

```yaml
gemini:
  api_key: ${GOOGLE_API_KEY}
  model: models/gemini-1.5-pro      # Change model
  embedding_model: models/embedding-001
```

Available Gemini models:
- `models/gemini-pro` (default)
- `models/gemini-1.5-pro` (newer)
- `models/gemini-1.5-flash` (faster, cheaper)

After changes:
```bash
git push origin main  # Railway redeploys automatically
```

### Increase Memory/CPU

In Railway Dashboard:
1. Go to **Settings** → **Plan**
2. Upgrade if needed (free tier has limits)

### Add Database (PostgreSQL)

For persistent vector storage:

1. In Railway Dashboard: **Create** → **Database** → **PostgreSQL**
2. Railway auto-injects `DATABASE_URL`
3. Update `settings.yaml`:

```yaml
vectorstore:
  database: postgres
  postgres:
    connection_string: ${DATABASE_URL}
```

## 🔍 Troubleshooting

### Deployment Failed?

Check the **Build Logs** in Railway Dashboard:
- Look for `ERROR` messages
- Most common: Missing environment variable

### Application Won't Start?

1. Check **Runtime Logs**
2. Ensure `GOOGLE_API_KEY` is set
3. Verify `settings-gemini.yaml` is in repo root

### API Returns 502 Bad Gateway?

1. App likely crashed - check **Runtime Logs**
2. Common causes:
   - Missing `GOOGLE_API_KEY`
   - Invalid API key
   - Insufficient memory (upgrade to paid plan)

## 📞 Common Commands

### View Logs Locally (with Railway CLI)
```bash
npm install -g @railway/cli
railway login
railway link
railway logs
```

### Restart Application
Railway Dashboard → **Settings** → **Redeploy**

### Roll Back to Previous Version
Railway Dashboard → **Deployments** → Click previous version

## 💡 Tips & Best Practices

✅ **Keep API key private** - Never commit to Git  
✅ **Monitor logs** - Watch for errors after each deployment  
✅ **Test before pushing** - Run locally first  
✅ **Use meaningful commit messages** - For easy rollbacks  
✅ **Regular updates** - Keep dependencies current  

## 🚀 What's Next?

1. ✅ Deploy to Railway (you just did!)
2. 📤 Upload documents for RAG (via API)
3. 🔐 Add authentication (if needed)
4. 📊 Set up monitoring alerts
5. 🌐 Custom domain (Railway supports this)

## 🎯 Example API Usage

### Chat with Context (Upload documents first)
```bash
curl -X POST https://your-railway-url.railway.app/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [{"role": "user", "content": "Summarize my documents"}],
    "use_context": true
  }'
```

### Ingest Documents
```bash
curl -X POST https://your-railway-url.railway.app/v1/ingest \
  -F "file=@document.pdf"
```

## 📚 More Resources

- [Railway Docs](https://docs.railway.app)
- [PrivateGPT Docs](https://docs.privategpt.dev)
- [Google Gemini API](https://ai.google.dev)
- [FastAPI Docs](https://fastapi.tiangolo.com)

---

**Deployment Status:** Ready to Deploy ✅  
**Configuration:** Complete ✅  
**Files Created:** 5 ✅

Happy deploying! 🎉
