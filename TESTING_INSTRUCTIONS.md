# 🧪 Testing Instructions - Updated

## ⚠️ Important: Test Script Fix Applied

The `test_queries.py` script had a wrong Django settings module name. It's now fixed, but you have **better testing options** below.

---

## 🎯 Best Way to Test: Simple Automated Script

I've created `simple_test.py` which is **easier and doesn't need Django imports**.

### **Step 1: Start the Server**
```bash
cd /Users/rkkbti/Downloads/Finance-bot
source venv/bin/activate
python manage.py runserver
```

### **Step 2: Upload CSV File**
- Open browser: http://localhost:8000
- Upload: `/Users/rkkbti/Downloads/Finance-bot/media/uploads/MSFT Historical Data (1).csv`

### **Step 3: Run Test Script**
Open a **new terminal**:
```bash
cd /Users/rkkbti/Downloads/Finance-bot
source venv/bin/activate
python simple_test.py
```

### **What It Does:**
- ✅ Tests 10 priority queries automatically
- ✅ Makes HTTP requests to your running server
- ✅ Checks for hallucinations
- ✅ Shows pass/fail for each query
- ✅ Saves results to JSON file
- ✅ No Django imports needed!

---

## 📋 Manual Testing (Easiest)

If you prefer manual testing:

### **Step 1: Start Server**
```bash
cd /Users/rkkbti/Downloads/Finance-bot
source venv/bin/activate
python manage.py runserver
```

### **Step 2: Open Browser**
http://localhost:8000

### **Step 3: Upload CSV**

### **Step 4: Copy Queries from File**
Open `ALL_TEST_QUERIES.txt` and copy-paste queries one by one.

---

## 🎯 The 10 Priority Tests

Copy these into the web UI:

### **Bug Fix Tests:**
1. `Which day recorded the highest trading volume?`
2. `Was Microsoft's stock price higher on 12-06-2026 or 18-06-2026?`
3. `Compare the closing price on 17-06-2026 and 18-06-2026.`

### **Hallucination Tests (CRITICAL):**
4. `What was Microsoft's closing price on 01-01-2028?`
   - ✅ Should say: "Date not in dataset"
   - ❌ Should NOT: Make up a price

5. `Tell me about Tesla stock in this dataset.`
   - ✅ Should say: "Only Microsoft data"
   - ❌ Should NOT: Return Tesla data

6. `What was the PE ratio?`
   - ✅ Should say: "Not in dataset"
   - ❌ Should NOT: Calculate or guess

### **Basic Functionality:**
7. `What was Microsoft's closing price on 18-06-2026?`
8. `Show all days where the closing price was above $390.`
9. `What is the average closing price?`
10. `Top 10 highest closing prices.`

---

## ✅ What to Check

For each response:

### **Good Response ✓**
```
Query: "Which day recorded the highest trading volume?"

Response:
"The highest trading volume was recorded on May 29, 2026, 
with 79.65 million shares traded.

On that day, Microsoft's stock closed at $450.24, showing 
a significant 5.45% increase."
```
✓ Natural language
✓ Specific data
✓ Context provided

### **Bad Response ✗**
```
Query: "Which day recorded the highest trading volume?"

Response:
"Error: Series.to_dict() takes 1 positional argument but 2 were given"
```
✗ Technical error exposed

---

## 🐛 If Tests Fail

### **Check Server Console**

When you run queries, check the terminal where the server is running. You should see:

```
============================================================
GENERATED CODE:
============================================================
dates = ['12-06-2026', '18-06-2026']
filtered = df[df['Date'].isin(dates)]
result = filtered[['Date', 'Price']].to_dict('records')
============================================================

============================================================
EXECUTION RESULT:
============================================================
[{'Date': '12-06-2026', 'Price': '390.74'}, ...]
============================================================
```

This shows:
1. The pandas code that was generated
2. The result that was returned
3. Any errors that occurred

### **Report Issues**

Share:
```
Query: [exact query]
Response: [what you got]
Console Output: [generated code + result from server terminal]
Expected: [what should happen]
```

---

## 📊 Test Scripts Comparison

| Script | Method | Best For |
|--------|--------|----------|
| `simple_test.py` | HTTP requests | ✅ **Automated testing (RECOMMENDED)** |
| `ALL_TEST_QUERIES.txt` | Manual copy-paste | ✅ **Manual testing** |
| `interactive_test.py` | Interactive with browser | ⚠️ Advanced users |
| `test_queries.py` | Django imports | ⚠️ Complex setup |

---

## 🚀 Quick Start Commands

### **Terminal 1 (Server):**
```bash
cd /Users/rkkbti/Downloads/Finance-bot
source venv/bin/activate
python manage.py runserver
```

### **Terminal 2 (Testing):**

**Option A - Automated:**
```bash
cd /Users/rkkbti/Downloads/Finance-bot
source venv/bin/activate
python simple_test.py
```

**Option B - Manual:**
```bash
# Just open the queries file
open ALL_TEST_QUERIES.txt
# Then copy-paste into browser at http://localhost:8000
```

---

## 📈 Expected Results

After running `simple_test.py`:

```
================================================================================
📊 TEST SUMMARY
================================================================================

Total Tests:  10
✅ Passed:    9 (90.0%)
❌ Failed:    1
⚠️  Errors:    0

✨ GOOD! Minor improvements needed.
```

**Target:** 90%+ pass rate for priority tests

---

## 💡 Tips

1. **Always start server first** before running tests
2. **Upload CSV** before testing (server needs the file)
3. **Watch server console** to see generated code
4. **Test hallucination queries carefully** - they're critical
5. **Report failures immediately** so we can fix

---

## 🆘 Troubleshooting

### **Error: Server not running**
```bash
# Start the server:
cd /Users/rkkbti/Downloads/Finance-bot
source venv/bin/activate
python manage.py runserver
```

### **Error: Module not found**
```bash
# Install dependencies:
source venv/bin/activate
pip install -r requirements.txt
```

### **Error: No file uploaded**
- Open http://localhost:8000
- Upload the CSV file first
- Then run tests

---

## ✅ Ready!

**Recommended approach:**

1. **Start server** (Terminal 1)
2. **Upload CSV** (Browser)
3. **Run simple_test.py** (Terminal 2)
4. **Check results**
5. **Report any failures**

Good luck! 🚀
