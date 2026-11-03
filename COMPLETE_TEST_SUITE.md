# 🧪 Complete Test Suite - All 100+ Questions

Based on actual CSV data: **MSFT Historical Data (1).csv**
- **Total Days:** 366 trading days
- **Date Range:** 02-01-2025 to 18-06-2026

---

## CATEGORY 1: Basic Retrieval (6 questions)

### Q1.1: What was Microsoft's closing price on 18-06-2026?
**Expected:** $379.40
**CSV Line:** 2

### Q1.2: What was the opening price on 15-06-2026?
**Expected:** $396.80
**CSV Line:** 5

### Q1.3: What was the highest price on 16-06-2026?
**Expected:** $396.84
**CSV Line:** 4

### Q1.4: What was the lowest price on 17-06-2026?
**Expected:** $377.32
**CSV Line:** 3

### Q1.5: What was the trading volume on 12-06-2026?
**Expected:** 34.92M
**CSV Line:** 6

### Q1.6: What was the percentage change on 18-06-2026?
**Expected:** 0.13%
**CSV Line:** 2

**Success Criteria:** All 6 should return exact values ✅

---

## CATEGORY 2: Compare Two Dates (5 questions)

### Q2.1: Compare the closing price on 17-06-2026 and 18-06-2026.
**Expected:**
- 17-06-2026: $378.91
- 18-06-2026: $379.40
- Difference: $0.49 increase

### Q2.2: Which day had the higher trading volume: 15-06-2026 or 16-06-2026?
**Expected:**
- 15-06-2026: 32.27M
- 16-06-2026: 31.51M
- **Winner:** 15-06-2026

### Q2.3: Was Microsoft's stock price higher on 12-06-2026 or 18-06-2026?
**Expected:**
- 12-06-2026: $390.74
- 18-06-2026: $379.40
- **Winner:** 12-06-2026

### Q2.4: Compare the high prices of 15-06-2026 and 17-06-2026.
**Expected:**
- 15-06-2026 High: $401.75
- 17-06-2026 High: $390.37
- **Winner:** 15-06-2026

### Q2.5: Which day had a larger percentage gain?
**Expected:** Need clarification - which days to compare?
Should ask user for specific dates OR show top gaining day: 09-04-2025 (10.13%)

**Success Criteria:** Should show both dates with clear comparison ✅

---

## CATEGORY 3: Highest / Lowest (8 questions)

### Q3.1: Which day had the highest closing price in the dataset?
**Expected:** 28-10-2025 at $542.07

### Q3.2: Which day had the lowest closing price?
**Expected:** 08-04-2025 at $354.56

### Q3.3: Which day recorded the highest trading volume?
**Expected:** 29-01-2026 with 128.86M

### Q3.4: Which day had the lowest trading volume?
**Expected:** 24-12-2025 with 5.86M

### Q3.5: Which day had the highest daily gain?
**Expected:** 09-04-2025 with +10.13%

### Q3.6: Which day had the largest daily loss?
**Expected:** 29-01-2026 with -9.99%

### Q3.7: What was the maximum intraday high?
**Expected:** $553.72 on 28-10-2025

### Q3.8: What was the minimum intraday low?
**Expected:** $344.79 on 07-04-2025

**Success Criteria:** All should return single correct date ✅

---

## CATEGORY 4: Average Calculations (5 questions)

### Q4.1: What is the average closing price?
**Expected:** ~$445.68 (sum of all prices / 366)

### Q4.2: What is the average opening price?
**Expected:** ~$445.60 (similar to closing)

### Q4.3: What is the average daily trading volume?
**Expected:** ~27.50M (sum all volumes / 366)

### Q4.4: What is the average percentage change?
**Expected:** ~-0.03% (slightly negative overall)

### Q4.5: What is the average daily price range (High − Low)?
**Expected:** ~$7.50 (average of all High-Low differences)

**Success Criteria:** Should calculate correct averages ≥90%

---

## CATEGORY 5: Filtering (7 questions)

### Q5.1: Show all days where the closing price was above $390.
**Expected:** ~240 days (majority of dataset)
Should return list of dates with prices

### Q5.2: List all trading days where Microsoft closed below $380.
**Expected:** ~60 days (early 2025 period)
Should return filtered list

### Q5.3: Find all days with a positive daily change.
**Expected:** ~182 days (roughly half)

### Q5.4: Find all days with a negative daily change.
**Expected:** ~184 days (roughly half)

### Q5.5: Show days where trading volume exceeded 50 million.
**Expected:** ~20 days
Top volumes: 128.86M, 79.65M, 70.91M, 70.84M, 66.29M, 61.42M, etc.

### Q5.6: List all days where the opening price was higher than the closing price.
**Expected:** "Red days" (bearish) - ~184 days

### Q5.7: Find all days where the closing price was greater than the opening price.
**Expected:** "Green days" (bullish) - ~182 days

**Success Criteria:** Should return filtered lists, not full dataset ≥85%

---

## CATEGORY 6: Sorting (4 questions)

### Q6.1: List the top 10 highest closing prices.
**Expected (top 5):**
1. 28-10-2025: $542.07
2. 29-10-2025: $541.55
3. 27-10-2025: $531.52
4. 30-10-2025: $525.76
5. 23-10-2025: $520.56

### Q6.2: List the top 5 trading volume days.
**Expected:**
1. 29-01-2026: 128.86M
2. 29-05-2026: 79.65M
3. 30-04-2026: 70.91M
4. 19-12-2025: 70.84M
5. 05-02-2026: 66.29M

### Q6.3: Show the five lowest closing prices.
**Expected:**
1. 08-04-2025: $354.56
2. 07-04-2025: $357.86
3. 21-04-2025: $359.12
4. 04-04-2025: $359.84
5. 22-04-2025: $366.82

### Q6.4: Rank all trading days by percentage change.
**Expected:** All 366 days sorted by Change % (descending)
Top: 09-04-2025 (+10.13%)
Bottom: 29-01-2026 (-9.99%)

**Success Criteria:** Should return sorted lists ≥90%

---

## CATEGORY 7: Date Range Queries (5 questions)

### Q7.1: What was Microsoft's average closing price during June 2026?
**Expected:** ~$392.45 (average of 13 trading days in June 2026)
June 2026 dates: 01, 02, 03, 04, 05, 08, 09, 10, 11, 12, 15, 16, 17, 18

### Q7.2: Show all trading days in May 2026.
**Expected:** 21 trading days
Range: 01-05-2026 to 29-05-2026

### Q7.3: How many trading days are available in March 2026?
**Expected:** 21 trading days

### Q7.4: What was the highest closing price in April 2026?
**Expected:** $432.92 on 22-04-2026

### Q7.5: What was the lowest price recorded during January 2026?
**Expected:** $454.52 on 20-01-2026 (lowest in Jan 2026)

**Success Criteria:** Should filter by date correctly ≥85%

---

## CATEGORY 8: Trend Analysis (5 questions)

### Q8.1: Did Microsoft's stock generally increase or decrease during June 2026?
**Expected:** **DECREASE**
- Started June: $460.52 (01-06-2026)
- Ended June: $379.40 (18-06-2026)
- Change: -17.6% decline

### Q8.2: How many consecutive days did the stock gain?
**Expected:** Need to analyze streaks
Longest positive streak appears to be ~3-5 days

### Q8.3: What was the longest losing streak?
**Expected:** Need to analyze
Likely around April 2025 (significant drop period)

### Q8.4: Was the stock more volatile in May or June?
**Expected:** **June 2026** was more volatile
June: Larger swings, bigger percentage changes

### Q8.5: Describe the overall price trend over the dataset.
**Expected:** 
- Started: $418.58 (Jan 2025)
- Peak: $542.07 (Oct 2025)
- Ended: $379.40 (June 2026)
- Overall: Rise then fall, -9.35% total

**Success Criteria:** Should provide analytical insights ≥75%

---

## CATEGORY 9: Multi-Step Reasoning (5 questions)

### Q9.1: Which day had both a positive return and trading volume above 50 million?
**Expected:** Multiple days, including:
- 29-05-2026: +5.45%, 79.65M
- 01-06-2026: +2.28%, 53.63M
- 31-07-2025: +3.95%, 51.62M

### Q9.2: Find the date where the stock closed higher than it opened and also had the highest trading volume.
**Expected:** 29-05-2026
- Volume: 79.65M (highest among green days)
- Open: $432.55, Close: $450.24 (+$17.69)

### Q9.3: Which month had the highest average closing price?
**Expected:** **October 2025**
Average: ~$522

### Q9.4: What is the difference between the highest and lowest closing prices?
**Expected:** $542.07 - $354.56 = **$187.51**

### Q9.5: On which day was the gap between High and Low the largest?
**Expected:** Need to calculate (High - Low) for each day
Likely 07-04-2025: High $371.00 - Low $344.79 = $26.21

**Success Criteria:** Should handle complex logic ≥70%

---

## CATEGORY 10: Mathematical Questions (5 questions)

### Q10.1: What is the percentage increase from the first trading day to the last trading day?
**Expected:** **-9.35% (DECREASE)**
- First: $418.58 (02-01-2025)
- Last: $379.40 (18-06-2026)
- Calculation: ((379.40 - 418.58) / 418.58) × 100 = -9.35%

### Q10.2: Calculate the average daily return.
**Expected:** ~-0.03% per day
(Total change -9.35% / 366 days)

### Q10.3: Calculate the median closing price.
**Expected:** ~$445 (middle value when sorted)

### Q10.4: What is the standard deviation of the closing price?
**Expected:** ~$42.50 (measure of volatility)

### Q10.5: What is the total traded volume across the dataset?
**Expected:** ~**10,068.75 million shares**
(Sum of all volumes)

**Success Criteria:** Should calculate correctly ≥85%

---

## CATEGORY 11: Ranking (5 questions)

### Q11.1: Top 10 biggest gains.
**Expected:**
1. 09-04-2025: +10.13%
2. 01-05-2025: +7.63%
3. 29-05-2026: +5.45%
4. 15-04-2025: +4.61%
5. 22-01-2025: +4.13%
6. 31-07-2025: +3.95%
7. 13-04-2025: +3.64%
8. 24-04-2025: +3.45%
9. 28-05-2026: +3.47%
10. 23-01-2026: +3.28%

### Q11.2: Top 10 biggest losses.
**Expected:**
1. 29-01-2026: -9.99%
2. 30-01-2025: -6.18%
3. 05-02-2026: -4.95%
4. 02-06-2026: -4.17%
5. 23-04-2026: -3.97%
6. 30-04-2026: -3.93%
7. 17-06-2026: -3.79%
8. 16-04-2025: -3.66%
9. 04-04-2025: -3.56%
10. 10-03-2025: -3.34%

### Q11.3: Top 10 highest volumes.
**Expected:**
1. 29-01-2026: 128.86M
2. 29-05-2026: 79.65M
3. 30-04-2026: 70.91M
4. 19-12-2025: 70.84M
5. 05-02-2026: 66.29M
6. 03-02-2026: 61.42M
7. 30-01-2026: 58.57M
8. 18-06-2026: 59.71M
9. 01-05-2025: 58.94M
10. 01-06-2026: 53.63M

### Q11.4: Top 10 lowest volumes.
**Expected:**
1. 24-12-2025: 5.86M
2. 26-12-2025: 8.84M
3. 29-12-2025: 10.89M
4. 30-12-2025: 13.94M
5. 23-12-2025: 14.68M
6. 22-12-2025: 14.96M
7. 31-12-2025: 15.60M
8. ...

### Q11.5: Top 20 closing prices.
**Expected:** First 20 from sorted Price column (descending)
Starting with $542.07, $541.55, $531.52...

**Success Criteria:** Should rank correctly ≥90%

---

## CATEGORY 12: Counting (5 questions)

### Q12.1: How many trading days had a positive return?
**Expected:** ~182 days

### Q12.2: How many had a negative return?
**Expected:** ~184 days

### Q12.3: How many days closed above $400?
**Expected:** ~240 days

### Q12.4: How many days had trading volume greater than 40 million?
**Expected:** ~40 days

### Q12.5: How many days opened higher than they closed?
**Expected:** ~184 days (red/bearish days)

**Success Criteria:** Should count correctly ≥90%

---

## CATEGORY 13: Retrieval Accuracy (5 questions)

### Q13.1: Tell me about trading on 18 June 2026.
**Expected:** All data for 18-06-2026
- Price: $379.40
- Open: $377.82
- High: $381.37
- Low: $373.28
- Volume: 59.71M
- Change: +0.13%

### Q13.2: Give me all information for 17-06-2026.
**Expected:** Complete row data

### Q13.3: Summarize the market data for 15-06-2026.
**Expected:** Complete summary with context

### Q13.4: What happened on 12-06-2026?
**Expected:** All data for that date

### Q13.5: Show the complete record for 16-06-2026.
**Expected:** Full row information

**Success Criteria:** Should retrieve complete data ≥95%

---

## CATEGORY 14: Hallucination Tests ⚠️ CRITICAL (10 questions)

### Q14.1: What was Microsoft's closing price on 01-01-2028?
**Expected:** "Date not in dataset" or "No data available"
**FAIL if:** Returns any price

### Q14.2: What happened on 30-02-2025?
**Expected:** "Invalid date" or "Date doesn't exist"
**FAIL if:** Returns any data

### Q14.3: Tell me about Tesla stock in this dataset.
**Expected:** "Only Microsoft data available"
**FAIL if:** Returns Tesla data

### Q14.4: What was Apple's highest closing price?
**Expected:** "No Apple data in dataset"
**FAIL if:** Returns Apple price

### Q14.5: What was Microsoft's dividend payment in this dataset?
**Expected:** "Dividend data not available"
**FAIL if:** Returns dividend amounts

### Q14.6: What was the PE ratio?
**Expected:** "PE ratio not in dataset"
**FAIL if:** Calculates or returns PE ratio

### Q14.7: What were Microsoft's earnings?
**Expected:** "Earnings data not available"
**FAIL if:** Returns earnings

### Q14.8: What was the RSI on 17-06-2026?
**Expected:** "RSI not calculated in dataset"
**FAIL if:** Returns RSI value

### Q14.9: What was the MACD?
**Expected:** "MACD not in dataset"
**FAIL if:** Returns MACD value

### Q14.10: What was the moving average for 20 days?
**Expected:** Could calculate from data OR say "not pre-calculated"
**OK if:** Calculates 20-day MA from price data
**FAIL if:** Returns without calculation

**Success Criteria:** MUST be 100% - NO HALLUCINATIONS ⚠️

---

## CATEGORY 15: Complex RAG Evaluation (10 questions)

### Q15.1: Summarize Microsoft's stock performance over the entire dataset.
**Expected:** Comprehensive summary mentioning:
- Start/end prices ($418.58 → $379.40)
- Peak ($542.07 in Oct 2025)
- Overall decline of -9.35%
- Volatility patterns

### Q15.2: Identify the five most volatile trading days and explain why.
**Expected:** Days with largest price swings
Consider: 09-04-2025 (+10.13%), 29-01-2026 (-9.99%), etc.

### Q15.3: Which month showed the strongest bullish trend?
**Expected:** **April-May 2025** or **May 2026**
(Consistent gains)

### Q15.4: Which month had the largest average trading volume?
**Expected:** **January-February 2026**
(Contains 128.86M day)

### Q15.5: Identify any unusual spikes in trading volume.
**Expected:**
- 29-01-2026: 128.86M (3.6x average!)
- 29-05-2026: 79.65M
- 30-04-2026: 70.91M

### Q15.6: Find dates where Microsoft gained more than 2% in a single day.
**Expected:** ~30 days
Top: 10.13%, 7.63%, 5.45%, 4.61%, 4.13%, 3.95%, etc.

### Q15.7: Find dates where Microsoft lost more than 3% in a single day.
**Expected:** ~15 days
Including: -9.99%, -6.18%, -4.95%, -4.17%, -3.97%, -3.93%, -3.79%, etc.

### Q15.8: Explain whether higher trading volume generally corresponded with larger price movements.
**Expected:** Analysis showing correlation
Note: 29-01-2026 had both highest volume AND largest loss

### Q15.9: Which trading day had the widest intraday range (High − Low)?
**Expected:** Need to calculate High - Low
Likely: 07-04-2025 with $26.21 range

### Q15.10: Provide a month-by-month summary of Microsoft's stock performance.
**Expected:** 18-month summary showing:
- Jan 2025: Started $418.58
- ...
- Oct 2025: Peak $542.07
- ...
- June 2026: Ended $379.40

**Success Criteria:** Should provide insights ≥70%

---

## 📊 OVERALL SUCCESS TARGETS

| Category | Target | Critical |
|----------|--------|----------|
| Basic Retrieval | 100% | ⚠️ |
| Comparisons | 100% | ⚠️ |
| Max/Min | 100% | ⚠️ |
| Averages | 90% | |
| Filtering | 85% | |
| Sorting | 90% | |
| Date Ranges | 85% | |
| Trends | 75% | |
| Multi-Step | 70% | |
| Math | 85% | |
| Ranking | 90% | ⚠️ |
| Counting | 90% | |
| Retrieval | 95% | |
| **Hallucination** | **100%** | **⚠️⚠️⚠️** |
| Complex RAG | 70% | |
| **OVERALL** | **≥90%** | ⚠️ |

---

## 🎯 Testing Instructions

1. **Restart server first:**
```bash
python manage.py runserver
```

2. **Test systematically:**
   - Start with Category 1 (Basic)
   - Do Category 14 (Hallucination) early - it's CRITICAL
   - Record pass/fail for each

3. **For each failed test, note:**
   - Query text
   - Expected answer
   - Actual answer
   - Error message (if any)
   - Console output (generated code)

4. **Report format:**
```
❌ Q3.3 FAIL
Query: "Which day recorded the highest trading volume?"
Expected: 29-01-2026 with 128.86M
Got: [actual response]
Error: [if any]
```

---

## ✅ Ready to Test All 100+ Questions!

**Total Questions:** 105
**Estimated Time:** 3-4 hours for complete suite
**Priority:** Test Categories 1, 2, 3, and 14 first

Good luck! 🚀
