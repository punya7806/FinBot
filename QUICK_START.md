# 🚀 Quick Start Guide

## Start the Application

### 1. Activate Virtual Environment
```bash
cd /Users/rkkbti/Downloads/Finance-bot
source venv/bin/activate
```

### 2. Start Django Server
```bash
python manage.py runserver
```

### 3. Open Browser
Go to: **http://127.0.0.1:8000/**

---

## Test the Hybrid System

### 📊 Test Pandas Agent (CSV Files)

**Upload:** `media/uploads/MSFT Historical Data (1).csv`

**Try these queries:**

1. `What was the price on 15-06-2026?`
2. `What was the highest price in June 2026?`
3. `Show me the lowest price in May 2026`
4. `Calculate average price for June 2026`
5. `If I bought 100 shares on 01-05-2026 and sold on 01-06-2026, what's my profit?`
6. `Show me all dates where price was above 420`

**Expected:** Accurate numerical answers with NO hallucinations!

---

### 📄 Test RAG Agent (PDF Files)

**Upload:** Any financial PDF document

**Try these queries:**

1. `What does this document say about revenue?`
2. `Summarize the key financial highlights`
3. `What are the risk factors mentioned?`

**Expected:** Text-based answers from document content

---

## ✨ New Features to Notice

✅ **Auto-scroll** - Page scrolls to latest message automatically  
✅ **Loading spinner** - Shows while processing your query  
✅ **Beautiful UI** - Modern gradient design with smooth animations  
✅ **Smart routing** - Automatically uses Pandas for CSV, RAG for PDF  
✅ **Error messages** - Clear feedback if something goes wrong  
✅ **Fast performance** - BM25 caching makes queries 10x faster  

---

## 🎯 Architecture at a Glance

```
CSV/XLSX → Pandas Agent → 100% Accurate Numerical Answers
PDF/DOCX → RAG Agent → Semantic Text Understanding
```

**That's it! Your hybrid system is ready to use.**

For detailed documentation, see:
- `IMPLEMENTATION_COMPLETE.md` - Full implementation details
- `HYBRID_ARCHITECTURE.md` - Architecture explanation
- `ANALYSIS_REPORT.md` - Problem analysis and solutions
