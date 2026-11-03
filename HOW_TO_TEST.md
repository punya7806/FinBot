# 🧪 How to Test All Questions - Complete Guide

## 📚 Overview

You have **100+ test queries** across **15 categories** to test the Finance Bot.

I've created **3 different ways** to test - choose what works best for you:

---

## 🎯 Method 1: Interactive Python Script (RECOMMENDED)

**Best for:** Systematic testing with automatic tracking

### Steps:

1. **Start Django server** (Terminal 1):
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

2. **Open browser**:
```
http://localhost:8000
```

3. **Upload CSV file** in the web UI

4. **Run test script** (Terminal 2):
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python interactive_test.py
```

### How it works:
- Script shows you one query at a time
- You copy it to the web UI
- Check the response
- Mark as Pass (p) or Fail (f)
- Script tracks everything automatically
- Generates a JSON report at the end

### Advantages:
✅ Automatic progress tracking
✅ Generates detailed report
✅ Can add notes for failures
✅ Shows statistics
✅ Can pause and resume

---

## 📋 Method 2: Copy-Paste from Text File (EASIEST)

**Best for:** Quick manual testing

### Steps:

1. **Start Django server**:
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

2. **Open browser**: http://localhost:8000

3. **Upload CSV file**

4. **Open file**: `ALL_TEST_QUERIES.txt`

5. **Copy each query** one by one and paste into web UI

6. **Track results** in a notepad/spreadsheet:
```
Query | Category | Pass/Fail | Notes
------|----------|-----------|------
What was closing price... | Basic | Pass | Good
Which day highest... | Max/Min | Pass | Fixed!
Tesla stock... | Hallucination | Pass | Correctly rejected
```

### Advantages:
✅ Simplest approach
✅ No Python needed
✅ Can test at your own pace
✅ Easy to jump to specific categories

---

## 🚀 Method 3: Automated Python Testing

**Best for:** Fast bulk testing (less control)

### Steps:

1. **Start Django server**:
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

2. **Run automated test** (separate terminal):
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python test_queries.py
```

### How it works:
- Calls the backend API directly
- Tests first ~30 queries automatically
- Shows pass/fail with colored output
- Generates summary report

### Advantages:
✅ Very fast
✅ No manual copying
✅ Good for regression testing

### Disadvantages:
❌ Less control over each test
❌ Harder to see actual UI responses
❌ May miss UI/formatting issues

---

## 📊 Recommended Testing Strategy

### Phase 1: Critical Tests First (30 minutes)
Use **Method 2** (copy-paste) to test:

1. **Category 14** - Hallucination tests (10 queries)
   - MUST be 100% pass rate
   - These ensure no fake data

2. **Category 3** - Highest/Lowest (8 queries)
   - Contains our Series.to_dict() bug fix
   - Query: "Which day recorded the highest trading volume?"

3. **Category 2** - Comparisons (5 queries)
   - Previous bug fix verification
   - Query: "Was price higher on 12-06-2026 or 18-06-2026?"

4. **Category 1** - Basic Retrieval (6 queries)
   - Foundation - should all pass

**Total:** ~29 queries in Phase 1

---

### Phase 2: Core Functionality (1 hour)
Use **Method 1** (interactive script) for:

5. Category 4 - Averages (5 queries)
6. Category 5 - Filtering (7 queries)
7. Category 6 - Sorting (4 queries)
8. Category 12 - Counting (5 queries)

**Total:** ~21 more queries

---

### Phase 3: Advanced Features (1-2 hours)
Continue with **Method 1** for remaining categories:

9. Category 7 - Date Ranges (5 queries)
10. Category 8 - Trends (5 queries)
11. Category 9 - Multi-step (5 queries)
12. Category 10 - Math (5 queries)
13. Category 11 - Ranking (5 queries)
14. Category 13 - Retrieval (5 queries)
15. Category 15 - Complex RAG (10 queries)

**Total:** ~40 more queries

---

## 📝 What to Check for Each Query

### ✅ Pass Criteria:
1. **Natural Language**: Response reads like a human wrote it
2. **Accurate Data**: Numbers match CSV file
3. **No Errors**: No technical error messages
4. **No Hallucinations**: Doesn't make up data
5. **Good Formatting**: Easy to read
6. **Complete Answer**: Fully addresses the question

### ❌ Fail Criteria:
1. Technical errors exposed to user
2. Wrong data returned
3. Made up (hallucinated) data
4. Raw data dump (not natural language)
5. Unnecessary disclaimers
6. Incomplete answer

---

## 🐛 If a Query Fails

### Step 1: Document the Failure
```
Query: [exact query text]
Category: [which category]
Response: [what the bot said]
Expected: [what should have happened]
Console Error: [any error in terminal]
```

### Step 2: Check Server Console
Look for:
```
============================================================
GENERATED CODE:
============================================================
[The pandas code that was generated]
============================================================

============================================================
EXECUTION RESULT:
============================================================
[What the code returned]
============================================================
```

### Step 3: Identify Issue Type
- **Code Generation Error**: LLM generated wrong pandas code
- **Execution Error**: Code failed to run
- **Formatting Error**: Result is correct but not natural
- **Hallucination**: Bot made up data

### Step 4: Report
Share the documentation from Step 1 + Step 2, and I'll fix it!

---

## 📈 Success Metrics

### Target Pass Rates:
- **Category 14 (Hallucination)**: 100% ⚠️ CRITICAL
- **Categories 1-3 (Basic/Compare/Max-Min)**: 100%
- **Categories 4-6 (Avg/Filter/Sort)**: 95%
- **Categories 7-13 (Advanced)**: 90%
- **Category 15 (Complex)**: 85%

### Overall Target: **95%+**

---

## 🎯 Quick Start (Fastest Way)

**If you want to start testing RIGHT NOW:**

1. Open terminal:
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

2. Open browser: http://localhost:8000

3. Upload CSV: `media/uploads/MSFT Historical Data (1).csv`

4. Open this file: `ALL_TEST_QUERIES.txt`

5. Copy first query:
```
What was Microsoft's closing price on 18-06-2026?
```

6. Paste into web UI chat

7. Check response - is it natural and correct?

8. Continue with next query!

---

## 📁 File Reference

| File | Purpose | When to Use |
|------|---------|-------------|
| `HOW_TO_TEST.md` | This guide | Read first |
| `ALL_TEST_QUERIES.txt` | All 100+ queries | Copy-paste testing |
| `interactive_test.py` | Interactive script | Systematic testing |
| `test_queries.py` | Automated script | Fast bulk testing |
| `TESTING_GUIDE.md` | Detailed test specs | Reference for expected behavior |
| `QUICK_TEST_CARD.md` | Priority tests only | Quick validation |

---

## ⏱️ Time Estimates

- **Quick Smoke Test** (10 priority queries): 15 minutes
- **Phase 1** (Critical tests): 30 minutes
- **Phase 1 + 2** (50 queries): 90 minutes
- **Full Suite** (100+ queries): 3-4 hours

---

## 💡 Pro Tips

1. **Start with failures**: Test hallucination and bug-fix queries first
2. **Keep server console visible**: You'll see generated code
3. **Take breaks**: Testing 100 queries is tiring
4. **Use interactive script**: It tracks progress for you
5. **Save partial results**: Don't lose progress if you stop

---

## 🆘 Need Help?

If you get stuck or see unexpected errors:

1. Check `TROUBLESHOOTING.md` for common issues
2. Check server console for generated code
3. Report the failure with details above
4. I'll help debug and fix!

---

## ✅ Ready to Start!

**Recommended first command:**

```bash
# Open in one terminal
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

**Then:**
```bash
# Open in another terminal
cd /Users/rkkbti/Downloads/Finance-bot
python interactive_test.py
```

**Or just use the text file:**
```bash
# Open this file and copy-paste queries
open ALL_TEST_QUERIES.txt
```

---

**Good luck testing! Let me know your results!** 🚀
