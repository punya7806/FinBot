# 🧪 Complete Testing Instructions

## 📋 Overview

We have fixed multiple bugs in the dynamic pandas agent and need to systematically verify all 105 test queries work correctly. This document provides step-by-step instructions for comprehensive testing.

---

## 🚀 Quick Start

### Method 1: Automated Testing (Recommended)

**Step 1: Start the server**
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

**Step 2: Upload the CSV file**
- Open browser: http://localhost:8000
- Click "Upload Document"
- Select: `media/uploads/MSFT Historical Data (1).csv`
- Wait for upload confirmation

**Step 3: Run automated tests**
```bash
# In a new terminal
cd /Users/rkkbti/Downloads/Finance-bot
python simple_test.py
```

This will test 10 priority queries and show pass/fail for each.

---

### Method 2: Manual Testing via Web UI

1. **Start server:** `python manage.py runserver`
2. **Open browser:** http://localhost:8000
3. **Upload CSV:** `media/uploads/MSFT Historical Data (1).csv`
4. **Test queries one by one** from the list below
5. **Compare responses** against expected results

---

## 📊 Complete Test Suite (105 Questions)

### 🔵 CATEGORY 1: Basic Retrieval (6 questions) - TARGET: 100%

| ID | Query | Expected | Status |
|----|-------|----------|--------|
| Q1.1 | What was Microsoft's closing price on 18-06-2026? | $379.40 | ⬜ |
| Q1.2 | What was the opening price on 15-06-2026? | $396.80 | ⬜ |
| Q1.3 | What was the highest price on 16-06-2026? | $396.84 | ⬜ |
| Q1.4 | What was the lowest price on 17-06-2026? | $377.32 | ⬜ |
| Q1.5 | What was the trading volume on 12-06-2026? | 34.92M | ⬜ |
| Q1.6 | What was the percentage change on 18-06-2026? | 0.13% | ⬜ |

### 🔵 CATEGORY 2: Compare Two Dates (5 questions) - TARGET: 100%

| ID | Query | Expected | Status |
|----|-------|----------|--------|
| Q2.1 | Compare the closing price on 17-06-2026 and 18-06-2026. | Both dates with comparison | ⬜ |
| Q2.2 | Which day had the higher trading volume: 15-06-2026 or 16-06-2026? | 15-06-2026 (32.27M) | ⬜ |
| Q2.3 | Was Microsoft's stock price higher on 12-06-2026 or 18-06-2026? | 12-06-2026 ($390.74) | ⬜ |
| Q2.4 | Compare the high prices of 15-06-2026 and 17-06-2026. | Both with comparison | ⬜ |
| Q2.5 | Which day had a larger percentage gain? | Ask for dates OR show top | ⬜ |

### 🔵 CATEGORY 3: Highest / Lowest (8 questions) - TARGET: 100%

| ID | Query | Expected | Status |
|----|-------|----------|--------|
| Q3.1 | Which day had the highest closing price in the dataset? | 28-10-2025 at $542.07 | ⬜ |
| Q3.2 | Which day had the lowest closing price? | 08-04-2025 at $354.56 | ⬜ |
| Q3.3 | Which day recorded the highest trading volume? | 29-01-2026 with 128.86M | ⬜ |
| Q3.4 | Which day had the lowest trading volume? | 24-12-2025 with 5.86M | ⬜ |
| Q3.5 | Which day had the highest daily gain? | 09-04-2025 with +10.13% | ⬜ |
| Q3.6 | Which day had the largest daily loss? | 29-01-2026 with -9.99% | ⬜ |
| Q3.7 | What was the maximum intraday high? | $553.72 on 28-10-2025 | ⬜ |
| Q3.8 | What was the minimum intraday low? | $344.79 on 07-04-2025 | ⬜ |

### 🔵 CATEGORY 4: Average Calculations (5 questions) - TARGET: 90%

| ID | Query | Expected | Status |
|----|-------|----------|--------|
| Q4.1 | What is the average closing price? | ~$445.68 | ⬜ |
| Q4.2 | What is the average opening price? | ~$445.60 | ⬜ |
| Q4.3 | What is the average daily trading volume? | ~27.50M | ⬜ |
| Q4.4 | What is the average percentage change? | ~-0.03% | ⬜ |
| Q4.5 | What is the average daily price range (High − Low)? | ~$7.50 | ⬜ |

### 🔵 CATEGORY 10: Mathematical Questions (5 questions) - TARGET: 85%

| ID | Query | Expected | Status |
|----|-------|----------|--------|
| Q10.1 | What is the percentage increase from the first trading day to the last trading day? | **-9.35% (DECREASE)** | ⬜ |
| Q10.2 | Calculate the average daily return. | ~-0.03% per day | ⬜ |
| Q10.3 | Calculate the median closing price. | ~$445 | ⬜ |
| Q10.4 | What is the standard deviation of the closing price? | ~$42.50 | ⬜ |
| Q10.5 | What is the total traded volume across the dataset? | ~10,068.75 million | ⬜ |

### 🔵 CATEGORY 11: Ranking (5 questions) - TARGET: 90% ⚠️ CRITICAL

| ID | Query | Expected | Status |
|----|-------|----------|--------|
| Q11.1 | Top 10 biggest gains. | 1. 09-04-2025: +10.13% | ⬜ |
| Q11.2 | Top 10 biggest losses. | 1. 29-01-2026: -9.99% | ⬜ |
| Q11.3 | Top 10 highest volumes. | 1. 29-01-2026: 128.86M | ⬜ |
| Q11.4 | Top 10 lowest volumes. | 1. 24-12-2025: 5.86M | ⬜ |
| Q11.5 | Top 20 closing prices. | Start with $542.07 | ⬜ |

### 🔵 CATEGORY 14: Hallucination Tests (10 questions) - TARGET: 100% ⚠️⚠️ CRITICAL

| ID | Query | Expected | FAIL IF |
|----|-------|----------|---------|
| Q14.1 | What was Microsoft's closing price on 01-01-2028? | Date not in dataset | Returns any price |
| Q14.2 | What happened on 30-02-2025? | Invalid date | Returns any data |
| Q14.3 | Tell me about Tesla stock in this dataset. | Only Microsoft data | Returns Tesla data |
| Q14.4 | What was Apple's highest closing price? | No Apple data | Returns Apple price |
| Q14.5 | What was Microsoft's dividend payment in this dataset? | Not available | Returns dividend |
| Q14.6 | What was the PE ratio? | Not in dataset | Returns PE ratio |
| Q14.7 | What were Microsoft's earnings? | Not available | Returns earnings |
| Q14.8 | What was the RSI on 17-06-2026? | Not calculated | Returns RSI value |
| Q14.9 | What was the MACD? | Not in dataset | Returns MACD |
| Q14.10 | What was the moving average for 20 days? | Calculate OR not pre-calculated | ✅ OK if calculates |

---

## 🎯 Success Criteria

| Category | Questions | Target Pass Rate | Critical? |
|----------|-----------|------------------|-----------|
| Basic Retrieval | 6 | 100% | ⚠️ YES |
| Comparisons | 5 | 100% | ⚠️ YES |
| Max/Min | 8 | 100% | ⚠️ YES |
| Averages | 5 | 90% | |
| Math | 5 | 85% | |
| Ranking | 5 | 90% | ⚠️ YES |
| **Hallucination** | 10 | **100%** | ⚠️⚠️⚠️ **CRITICAL** |
| **OVERALL** | 44+ | **≥90%** | ⚠️ **YES** |

---

## 📝 How to Record Results

### For each test:

✅ **PASS** - Response is correct, matches expected
❌ **FAIL** - Response is wrong, error, or hallucination
⚠️  **PARTIAL** - Response is close but not exact

### Recording template:
```
Q1.1: What was Microsoft's closing price on 18-06-2026?
Expected: $379.40
Got: [actual response]
Status: ✅ PASS / ❌ FAIL
Notes: [any observations]
```

---

## 🐛 Known Fixes Applied

We have fixed the following bugs in `rag/dynamic_pandas_agent.py`:

1. ✅ **Series.to_dict('records') error** - Fixed in instruction #6
2. ✅ **Comparison queries returning one date** - Fixed in instruction #4-7
3. ✅ **Unwanted disclaimers** - Fixed in formatting prompt instruction #10-11
4. ✅ **F-string dictionary syntax** - Changed to `dict()` constructor
5. ✅ **.loc[-1] vs .iloc[-1]** - Fixed in instruction #11
6. ✅ **Ranking queries processing too few rows** - Fixed in instructions #123-132
7. ✅ **Ranking on Series instead of DataFrame** - Added critical instruction #130

---

## 🔧 If Tests Fail

### For each failure, record:

1. **Query ID and text**
2. **Expected result**
3. **Actual response**
4. **Generated pandas code** (check server console)
5. **Error message** (if any)

### Report format:
```
❌ Q3.3 FAIL
Query: "Which day recorded the highest trading volume?"
Expected: 29-01-2026 with 128.86M
Got: [actual response]
Generated code:
[paste code from console]
Error: [if any]
```

---

## 📊 Priority Testing Order

Test in this order to catch critical issues first:

1. **Phase 1: Critical Fixes** (10 queries)
   - Q1.1, Q3.3, Q10.1, Q10.5, Q11.1, Q11.3

2. **Phase 2: Hallucination Tests** (10 queries)
   - Q14.1 through Q14.10 - MUST BE 100%!

3. **Phase 3: Full Suite** (Remaining queries)
   - Test all categories systematically

---

## 💡 Tips

1. **Start server first**: Server must be running for tests
2. **Upload CSV once**: Upload persists during server session
3. **Check console**: Generated pandas code shows in terminal
4. **One at a time**: Test systematically, don't skip
5. **Document everything**: Record every failure with details

---

## 📁 File Locations

- **CSV Data:** `/Users/rkkbti/Downloads/Finance-bot/media/uploads/MSFT Historical Data (1).csv`
- **Agent Code:** `/Users/rkkbti/Downloads/Finance-bot/rag/dynamic_pandas_agent.py`
- **Test Script:** `/Users/rkkbti/Downloads/Finance-bot/simple_test.py`
- **Complete Suite:** `/Users/rkkbti/Downloads/Finance-bot/COMPLETE_TEST_SUITE.md`

---

## ✅ Ready to Test!

**Start here:**
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

Then open http://localhost:8000 and begin testing!

Good luck! 🚀
