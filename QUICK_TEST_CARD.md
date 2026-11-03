# 🎯 Quick Test Reference Card

## Priority Tests (Test These First!)

### 🔴 **Critical Bug Fix Verification**
These test the bugs we just fixed:

```
1. Which day recorded the highest trading volume?
   ✓ Should work now (Series.to_dict() fix applied)

2. Was Microsoft's stock price higher on 12-06-2026 or 18-06-2026?
   ✓ Should compare both dates (fixed earlier)

3. Compare the closing price on 17-06-2026 and 18-06-2026.
   ✓ Should show both with comparison
```

---

### 🟡 **Hallucination Prevention Tests**
These ensure the agent doesn't make up data:

```
4. What was Microsoft's closing price on 01-01-2028?
   ✓ Should say: "Date not in dataset"
   ✗ Should NOT: Make up a price

5. Tell me about Tesla stock in this dataset.
   ✓ Should say: "Only Microsoft data available"
   ✗ Should NOT: Return any Tesla data

6. What was the PE ratio?
   ✓ Should say: "Not in this dataset"
   ✗ Should NOT: Calculate or guess PE ratio
```

---

### 🟢 **Core Functionality Tests**

```
7. What was Microsoft's closing price on 18-06-2026?
   Expected: ~$379.40

8. Show all days where the closing price was above $390.
   Expected: List of dates with prices

9. What is the average closing price?
   Expected: Calculated average

10. Top 10 highest closing prices.
    Expected: Sorted list of 10 dates
```

---

## Testing Workflow

```
Start Server → Upload CSV → Test Queries → Check Results → Report Issues
```

### Step 1: Start Server
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

### Step 2: Open Browser
```
http://localhost:8000
```

### Step 3: Upload File
```
File: /Users/rkkbti/Downloads/Finance-bot/media/uploads/MSFT Historical Data (1).csv
```

### Step 4: Test Queries
Start with the 10 priority queries above

### Step 5: Check Each Response
- ✅ Natural language (not technical)
- ✅ Accurate data
- ✅ No hallucinations
- ✅ No raw data dumps

---

## Pass/Fail Criteria

### ✅ PASS Examples:

**Query:** "Which day had the highest price?"

**Good Response:**
```
The highest closing price was recorded on May 29, 2026, 
when Microsoft stock closed at $450.24.

This represented a significant 5.45% daily gain on notably 
high trading volume of 79.65 million shares.
```

✓ Natural
✓ Accurate
✓ Context provided
✓ No technical jargon

---

### ❌ FAIL Examples:

**Query:** "Compare 17 and 18 June"

**Bad Response 1:**
```
[{'Date': '17-06-2026', 'Price': '378.91'}, {'Date': '18-06-2026', 'Price': '379.40'}]
```
✗ Raw data dump, not natural

**Bad Response 2:**
```
Unfortunately, we don't have data for 18-06-2026.
```
✗ Wrong! We DO have data

**Bad Response 3:**
```
On 17-06-2026, the stock closed at $378.91. 
Please note this is based on limited data and should not be 
considered investment advice...
```
✗ Unnecessary disclaimer

---

## Common Issues to Watch For

### 🐛 Code Generation Errors
**Console shows:**
```
Series.to_dict() takes 1 positional argument but 2 were given
```
**Fix:** Already applied - check if still happening

---

### 🐛 Volume Parsing Issues
**Query:** "Show days where trading volume exceeded 50 million"

**Issue:** Volume column has "79.65M" format
**Check:** Does agent parse "M" correctly?

---

### 🐛 Date Format Issues
**Query:** "What happened on 15 June 2026?"

**Issue:** User says "15 June", CSV has "15-06-2026"
**Check:** Does agent handle different date formats?

---

### 🐛 Ambiguous Queries
**Query:** "Which day had a larger percentage gain?"

**Issue:** No dates specified
**Check:** Does agent ask for clarification?

---

## Quick Stats

**Total Test Queries:** ~100
**Priority Tests:** 10 (do these first)
**Expected Time:** ~2-3 hours for full suite
**Target Pass Rate:** 95%+

---

## Reporting Format

For each failed query, report:

```
Query: [copy exact query]
Response: [copy full response]
Console: [copy any error from terminal]
Expected: [what should have happened]
Issue: [wrong answer / error / hallucination / etc.]
```

---

## All Test Categories

1. ✅ Basic Retrieval (6 queries) - START HERE
2. ✅ Compare Two Dates (5 queries) - PRIORITY
3. ✅ Highest/Lowest (8 queries) - PRIORITY (includes our bug fix)
4. ⚠️ Average Calculations (5 queries)
5. ⚠️ Filtering (6 queries)
6. ⚠️ Sorting (4 queries)
7. ⚠️ Date Range (5 queries)
8. ⚠️ Trend Analysis (5 queries)
9. ⚠️ Multi-Step (5 queries)
10. ⚠️ Mathematical (5 queries)
11. ⚠️ Ranking (5 queries)
12. ⚠️ Counting (5 queries)
13. ⚠️ Retrieval Accuracy (5 queries)
14. 🔴 Hallucination Tests (10 queries) - CRITICAL
15. ⚠️ Complex RAG (10 queries)

**Legend:**
- ✅ Do these first
- 🔴 Critical for accuracy
- ⚠️ Important but can test later

---

## Success Metrics

| Metric | Target |
|--------|--------|
| Basic queries work | 100% |
| Comparisons work | 100% |
| No hallucinations | 100% |
| Natural responses | 95% |
| Complex queries | 85% |
| **Overall** | **95%+** |

---

## Next Steps

1. **Start Testing:** Begin with 10 priority queries
2. **Track Results:** Note pass/fail for each
3. **Report Issues:** Share failed queries with details
4. **Fix Together:** We'll debug and improve
5. **Retest:** Verify fixes work
6. **Complete Suite:** Test remaining categories

---

## Need Help?

If stuck or seeing errors:
1. Check server console for generated code
2. Note the exact error message
3. Share the query + response + console output
4. We'll debug together!

---

**Ready to test! Start with the 10 priority queries above.** 🚀

Let me know:
- ✅ Which queries pass
- ❌ Which queries fail
- 🐛 What errors you see

Good luck! 🎯
