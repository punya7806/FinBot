# 🧪 Comprehensive Test Plan with Expected Results

## Dataset Summary
- **Total Rows:** 367 (including header)
- **Date Range:** 02-01-2025 to 18-06-2026
- **First Trading Day:** 02-01-2025 (Price: $418.58)
- **Last Trading Day:** 18-06-2026 (Price: $379.40)

---

## ✅ Priority Tests (Verify Against CSV)

### Test 1: Highest Trading Volume
**Query:** "Which day recorded the highest trading volume?"

**Expected Answer:**
- **Date:** 29-01-2026
- **Volume:** 128.86M
- **Price:** $433.50
- **Change:** -9.99%

**Verification:** Line 100 in CSV has the highest Vol. value

---

### Test 2: Total Volume
**Query:** "What is the total traded volume across the dataset?"

**Expected Answer:**
Sum of all volumes ≈ **10,068.75 million shares** (need to parse "M" and sum)

**How to Verify:**
1. Parse all "Vol." values
2. Remove "M" suffix
3. Convert to float
4. Sum all values

---

### Test 3: Percentage Change First to Last
**Query:** "What is the percentage increase from the first trading day to the last trading day?"

**Expected Answer:**
- First day (02-01-2025): $418.58
- Last day (18-06-2026): $379.40
- **Change:** -9.35% (DECREASE, not increase!)

**Calculation:** ((379.40 - 418.58) / 418.58) * 100 = -9.35%

---

### Test 4: Top 10 Biggest Gains
**Query:** "Top 10 biggest gains."

**Expected Answer** (sorted by Change % descending):
1. 09-04-2025: **10.13%** ($390.49)
2. 01-05-2025: **7.63%** ($425.40)
3. 29-05-2026: **5.45%** ($450.24)
4. 15-04-2025: **4.61%** ($411.22)
5. 22-01-2025: **4.13%** ($446.20)
6. 31-07-2025: **3.95%** ($533.50)
7. 13-04-2025: **3.64%** ($384.37)
8. 24-04-2025: **3.45%** ($387.30)
9. 28-05-2026: **3.47%** ($426.99)
10. 23-01-2026: **3.28%** ($465.95)

**Verification:** Check "Change %" column, sort descending

---

### Test 5: Top 10 Highest Volumes
**Query:** "Top 10 highest volumes."

**Expected Answer** (sorted by Vol. descending):
1. 29-01-2026: **128.86M**
2. 29-05-2026: **79.65M**
3. 30-04-2026: **70.91M**
4. 05-02-2026: **66.29M**
5. 19-12-2025: **70.84M** (Wait, let me recheck...)

Actually, top 10 volumes:
1. 29-01-2026: 128.86M
2. 29-05-2026: 79.65M
3. 30-04-2026: 70.91M
4. 19-12-2025: 70.84M
5. 05-02-2026: 66.29M
6. 03-02-2026: 61.42M
7. 18-06-2026: 59.71M
8. 30-01-2026: 58.57M
9. 01-05-2025: 58.94M
10. 01-06-2026: 53.63M

---

### Test 6: Average Closing Price
**Query:** "What is the average closing price?"

**Expected Answer:**
Sum all "Price" values / 366 rows ≈ **$445.68**

**Calculation:** Need to parse and average all prices

---

### Test 7: Highest Closing Price
**Query:** "Which day had the highest closing price in the dataset?"

**Expected Answer:**
- **Date:** 28-10-2025
- **Price:** $542.07
- **Volume:** 29.99M
- **Change:** 1.98%

**Verification:** Line 163 has highest Price value

---

### Test 8: Lowest Closing Price
**Query:** "Which day had the lowest closing price?"

**Expected Answer:**
- **Date:** 08-04-2025
- **Price:** $354.56
- **Volume:** 35.87M
- **Change:** -0.92%

**Verification:** Find minimum Price value

---

### Test 9: Compare Two Dates
**Query:** "Compare the closing price on 17-06-2026 and 18-06-2026."

**Expected Answer:**
- 17-06-2026: $378.91 (Change: -3.79%)
- 18-06-2026: $379.40 (Change: +0.13%)
- **Difference:** $0.49 increase

**Verification:** Lines 3 and 2 in CSV

---

### Test 10: Top 10 Biggest Losses
**Query:** "Top 10 biggest losses."

**Expected Answer** (sorted by Change % ascending, most negative first):
1. 29-01-2026: **-9.99%** ($433.50, Vol: 128.86M)
2. 30-01-2025: **-6.18%** ($414.99)
3. 05-02-2026: **-4.95%** ($393.67)
4. 02-06-2026: **-4.17%** ($441.31)
5. 23-04-2026: **-3.97%** ($415.75)
6. 30-04-2026: **-3.93%** ($407.78)
7. 17-06-2026: **-3.79%** ($378.91)
8. 04-04-2025: **-3.56%** ($359.84)
9. 10-03-2025: **-3.34%** ($380.16)
10. 16-04-2025: **-3.66%** ($371.61)

---

## 📊 Key Statistics to Verify

| Metric | Expected Value | How to Calculate |
|--------|----------------|------------------|
| **Total Rows** | 366 data rows | Count rows excluding header |
| **Date Range** | 02-01-2025 to 18-06-2026 | First and last dates |
| **Highest Price** | $542.07 (28-10-2025) | max(Price) |
| **Lowest Price** | $354.56 (08-04-2025) | min(Price) |
| **Highest Volume** | 128.86M (29-01-2026) | max(Vol.) |
| **Highest Gain** | 10.13% (09-04-2025) | max(Change %) |
| **Biggest Loss** | -9.99% (29-01-2026) | min(Change %) |
| **First Price** | $418.58 | Row 367 (last row) |
| **Last Price** | $379.40 | Row 2 (first data row) |
| **Overall Change** | -9.35% | ((last - first) / first) * 100 |

---

## 🧪 Test Execution Plan

### Phase 1: Critical Fixes Verification (10 mins)
Test these to verify all our bug fixes work:

1. ✅ "Which day recorded the highest trading volume?"
   - Should return: 29-01-2026 with 128.86M
   
2. ✅ "What is the total traded volume across the dataset?"
   - Should return: ~10,068.75 million

3. ✅ "What is the percentage increase from the first trading day to the last trading day?"
   - Should return: -9.35% (DECREASE)

4. ✅ "Top 10 biggest gains."
   - Should return: 10 rows starting with 09-04-2025 (10.13%)

5. ✅ "Top 10 highest volumes."
   - Should return: 10 rows starting with 29-01-2026 (128.86M)

---

### Phase 2: Basic Queries (15 mins)

6. "What was Microsoft's closing price on 18-06-2026?"
   - Expected: $379.40

7. "What was the opening price on 15-06-2026?"
   - Expected: $396.80

8. "What was the highest price on 16-06-2026?"
   - Expected: $396.84

9. "What was the trading volume on 12-06-2026?"
   - Expected: 34.92M

10. "What was the percentage change on 18-06-2026?"
    - Expected: 0.13%

---

### Phase 3: Comparisons (10 mins)

11. "Compare the closing price on 17-06-2026 and 18-06-2026."
    - Expected: Both dates with prices and difference

12. "Which day had the higher trading volume: 15-06-2026 or 16-06-2026?"
    - 15-06-2026: 32.27M
    - 16-06-2026: 31.51M
    - Expected: 15-06-2026 had higher volume

13. "Was Microsoft's stock price higher on 12-06-2026 or 18-06-2026?"
    - 12-06-2026: $390.74
    - 18-06-2026: $379.40
    - Expected: 12-06-2026 was higher

---

### Phase 4: Aggregations (10 mins)

14. "What is the average closing price?"
    - Expected: ~$445.68

15. "What is the average daily trading volume?"
    - Expected: ~27.50M

16. "Calculate the median closing price."
    - Expected: Middle value when sorted

---

### Phase 5: Rankings (15 mins)

17. "List the top 10 highest closing prices."
18. "List the top 5 trading volume days."
19. "Show the five lowest closing prices."
20. "Top 10 biggest losses."

---

## 🎯 Success Criteria

| Category | Target Pass Rate |
|----------|------------------|
| Bug Fix Tests (1-5) | **100%** ⚠️ CRITICAL |
| Basic Retrieval (6-10) | **100%** |
| Comparisons (11-13) | **100%** |
| Aggregations (14-16) | **95%** |
| Rankings (17-20) | **95%** |
| **OVERALL** | **≥95%** |

---

## 📝 How to Report Results

For each test, record:

```
✅ Test 1: PASS
Query: "Which day recorded the highest trading volume?"
Expected: 29-01-2026, 128.86M
Got: 29-01-2026, 128.86M
Status: ✅ CORRECT

❌ Test 2: FAIL
Query: "What is the total traded volume?"
Expected: ~10,068.75M
Got: Error or wrong value
Status: ❌ NEEDS FIX
```

---

## 🚀 Ready to Test!

**Start with Phase 1 (Critical Fixes) and work through systematically.**

Report back with results and I'll fix any issues found!
