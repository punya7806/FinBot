# 🔧 Troubleshooting Guide

## Common Issues and Solutions

---

## ❌ Error: "GROQ_API_KEY environment variable not set"

### Symptoms
```
groq.GroqError: The api_key client option must be set either by passing api_key 
to the client or by setting the GROQ_API_KEY environment variable
```

### Solutions

#### ✅ Solution 1: Verify .env file exists
```bash
cd /Users/rkkbti/Downloads/Finance-bot
ls -la | grep .env
```

Should show: `.env`

#### ✅ Solution 2: Check .env content
```bash
cat .env
```

Should contain:
```
GROQ_API_KEY=YOUR_GROQ_API_KEY

#### ✅ Solution 3: Restart Django server
After creating or modifying `.env`, you **must restart** the Django server:

```bash
# Stop server (Ctrl+C)
# Then start again:
python manage.py runserver
```

#### ✅ Solution 4: Verify API key is valid
Test your API key:
```bash
python -c "from dotenv import load_dotenv; load_dotenv(); import os; print('API Key:', os.getenv('GROQ_API_KEY'))"
```

Should print your API key (not None)

#### ✅ Solution 5: Manual fix (if above don't work)
If the automatic loading still fails, you can manually set it:

**Edit `chatbot/agents.py`:**
```python
def get_llm():
    # Temporary hardcode for testing
    api_key = "YOUR_GROQ_API_KEY"  # Replace with your key
    return ChatGroq(
        model="llama-3.3-70b-versatile",
        api_key=api_key
    )
```

**⚠️ Warning**: Don't commit hardcoded API keys to git!

---

## ❌ Error: "No module named 'dotenv'"

### Solution
```bash
pip install python-dotenv
```

---

## ❌ Error: "File upload not working"

### Symptoms
- Upload button doesn't respond
- File doesn't appear in `media/uploads/`

### Solutions

#### ✅ Check directory permissions
```bash
mkdir -p media/uploads
chmod 755 media/uploads
```

#### ✅ Check Django settings
Verify `settings.py` has:
```python
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'
```

---

## ❌ Error: "FAISS index not found"

### Symptoms
```
FileNotFoundError: [Errno 2] No such file or directory: 'faiss_index/index.faiss'
```

### Solution
Upload a file first! The FAISS index is created during file upload.

```bash
# Or manually create directory:
mkdir -p faiss_index
```

---

## ❌ Auto-scroll not working

### Solution
This should be fixed in the new UI. If still broken:

1. **Open browser console** (F12)
2. **Check for JavaScript errors**
3. **Verify** `.chat-area` element exists:
   ```javascript
   console.log(document.querySelector('.chat-area'));
   ```

If returns `null`, the CSS class is wrong. Should return an HTMLElement.

---

## ❌ Queries return "Please upload a financial document first"

### Cause
No file uploaded in current session

### Solution
1. Upload a CSV, XLSX, PDF, or DOCX file
2. Wait for success message
3. Then submit queries

---

## ❌ CSV queries still hallucinating

### Diagnostic
Check console logs for:
```
Routing to Pandas Agent
```

If you see `Routing to RAG Agent` for a CSV, the file type detection failed.

### Solution
Verify session storage:
```python
# Add debug print in views.py ask_question():
print(f"File type: {request.session.get('file_type')}")
print(f"File path: {request.session.get('file_path')}")
```

Should show:
```
File type: tabular
File path: /path/to/your.csv
```

---

## ❌ BM25 cache errors

### Symptoms
```
PickleError: Can't pickle <class>
```

### Solution
Delete cache and rebuild:
```bash
rm -rf bm25_cache/
```

Cache will rebuild on next query.

---

## ❌ UI looks broken (no styles)

### Symptoms
- Plain white page
- No colors/gradients
- Buttons unstyled

### Cause
Bootstrap CSS not loading

### Solution
Check browser console for:
```
Failed to load resource: net::ERR_INTERNET_DISCONNECTED
```

Bootstrap is loaded from CDN. Ensure internet connection.

**Offline alternative**: Download Bootstrap CSS locally.

---

## ❌ Slow query performance

### Expected Times
- First query: 2-3 seconds (builds cache)
- Subsequent queries: 0.5-1 second

### If slower:
1. Check CPU usage
2. Check if BM25 caching is working (console should show "Using cached BM25 index")
3. Try smaller files first

---

## ❌ Port 8000 already in use

### Symptoms
```
Error: That port is already in use.
```

### Solution
```bash
# Find process using port 8000
lsof -i :8000

# Kill it
kill -9 <PID>

# Or use different port
python manage.py runserver 8001
```

---

## ❌ Database locked error

### Symptoms
```
sqlite3.OperationalError: database is locked
```

### Solution
```bash
# Stop server
# Delete database
rm db.sqlite3

# Recreate
python manage.py migrate
```

---

## 🔍 Debug Mode

### Enable detailed logging

**In `agents.py`**, add debug prints:
```python
def finance_agent(question, path, file_type='textual'):
    print(f"🔍 DEBUG: Question: {question}")
    print(f"🔍 DEBUG: File type: {file_type}")
    print(f"🔍 DEBUG: Path: {path}")
    
    # ... rest of code
```

### Check browser console
Press **F12** → **Console tab**

Look for:
- JavaScript errors (red text)
- Network errors (failed requests)
- CORS issues

---

## 🧪 Test API Key Separately

Create `test_api.py`:
```python
from langchain_groq import ChatGroq
from dotenv import load_dotenv
import os

load_dotenv()

try:
    llm = ChatGroq(
        model="llama-3.3-70b-versatile",
        api_key=os.getenv("GROQ_API_KEY")
    )
    response = llm.invoke("Say hello!")
    print("✅ API Key works!")
    print(f"Response: {response.content}")
except Exception as e:
    print(f"❌ Error: {e}")
```

Run:
```bash
python test_api.py
```

---

## 📞 Still Having Issues?

1. **Check all logs**:
   - Django server console
   - Browser console (F12)
   - Network tab (F12 → Network)

2. **Verify file structure**:
   ```bash
   ls -R | grep -E "agents.py|views.py|index.html|.env"
   ```

3. **Try minimal test**:
   - Start fresh server
   - Upload simple CSV
   - Ask one question
   - Check logs at each step

4. **Review documentation**:
   - `README.md` - Setup instructions
   - `QUICK_START.md` - Basic usage
   - `DEPLOYMENT_CHECKLIST.md` - Full testing

---

## ✅ Verification Checklist

Before asking for help, verify:

- [ ] `.env` file exists in project root
- [ ] `.env` contains valid `GROQ_API_KEY=...`
- [ ] Django server restarted after creating `.env`
- [ ] No Python errors in server console
- [ ] No JavaScript errors in browser console
- [ ] File uploads successfully
- [ ] `media/uploads/` directory exists
- [ ] Internet connection working (for CDN resources)

---

## 🎯 Quick Fix Summary

| Problem | Quick Fix |
|---------|-----------|
| API Key Error | Add to `.env`, restart server |
| File Upload Error | `mkdir -p media/uploads` |
| FAISS Error | Upload file first |
| Auto-scroll Not Working | Check browser console |
| Slow Queries | Delete `bm25_cache/`, restart |
| Port In Use | `kill -9 <PID>` or use port 8001 |
| Database Locked | Delete `db.sqlite3`, migrate |

---

**If you've tried everything above and still have issues, check the error message carefully - it usually tells you exactly what's wrong!**
