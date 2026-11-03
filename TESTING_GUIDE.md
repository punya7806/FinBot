# 🧪 Comprehensive Testing Guide

## Setup

1. **Start the Django server:**
```bash
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver
```

2. **Open browser:**
```
http://localhost:8000
```

3. **Upload the CSV file:**
   - File: `/Users/rkkbti/Downloads/Finance-bot/media/uploads/MSFT Historical Data (1).csv`

---

## Testing Protocol

For each query:
1. ✅ Check if response is **natural and human-friendly**
2. ✅ Check if response is **accurate** (verify against CSV data)
3. ✅ Check if response **doesn't hallucinate**
4. ✅ Check **console output** for generated code (if errors occur)

---

## Category 1: Basic Retrieval (6 queries)
**Expected:** Single value returned naturally

### Query 1.1
```
What was Microsoft's closing price on 18-06-2026?
```
**Expected Answer:** ~$379.40

---

### Query 1.2
```
What was the opening price on 15-06-2026?
```
**Expected Answer:** Check CSV for 15-06-2026 opening price

---

### Query 1.3
```
What was the highest price on 16-06-2026?
```
**Expected Answer:** Check CSV for 16-06-2026 High column

---

### Query 1.4
```
What was the lowest price on 17-06-2026?
```
**Expected Answer:** Check CSV for 17-06-2026 Low column

---

### Query 1.5
```
What was the trading volume on 12-06-2026?
```
**Expected Answer:** Check CSV for 12-06-2026 Vol. column

---

### Query 1.6
```
What was the percentage change on 18-06-2026?
```
**Expected Answer:** Check CSV for 18-06-2026 Change % column

---

## Category 2: Compare Two Dates (5 queries)
**Expected:** Both dates shown with natural comparison

### Query 2.1
```
Compare the closing price on 17-06-2026 and 18-06-2026.
```
**Expected:** Both prices shown, difference calculated, trend explained

**✓ Pass if:** Shows both dates, calculates difference
**✗ Fail if:** Shows only one date, says data not found, hallucinations

---

### Query 2.2
```
Which day had the higher trading volume: 15-06-2026 or 16-06-2026?
```
**Expected:** Both volumes shown, winner identified

---

### Query 2.3
```
Was Microsoft's stock price higher on 12-06-2026 or 18-06-2026?
```
**Expected:** Both prices, clear answer which is higher

---

### Query 2.4
```
Compare the high prices of 15-06-2026 and 17-06-2026.
```
**Expected:** Both high prices compared

---

### Query 2.5
```
Which day had a larger percentage gain?
```
**Expected:** Should ask for clarification OR show top gaining day
**Note:** This is ambiguous - good test for error handling

---

## Category 3: Highest / Lowest (8 queries)
**Expected:** Single day identified with context

### Query 3.1 ⚠️ **CRITICAL TEST**
```
Which day had the highest closing price in the dataset?
```
**Expected:** One date with highest price, context about that day

**Previous Error:** `.to_dict('records')` on Series

---

### Query 3.2
```
Which day had the lowest closing price?
```

---

### Query 3.3 ⚠️ **CRITICAL TEST**
```
Which day recorded the highest trading volume?
```
**Expected:** One date with volume data

**Previous Error:** Series.to_dict() argument issue - THIS IS THE BUG WE JUST FIXED

---

### Query 3.4
```
Which day had the lowest trading volume?
```

---

### Query 3.5
```
Which day had the highest daily gain?
```
**Expected:** Day with highest positive Change %

---

### Query 3.6
```
Which day had the largest daily loss?
```
**Expected:** Day with highest negative Change %

---

### Query 3.7
```
What was the maximum intraday high?
```
**Expected:** Highest value in High column across all dates

---

### Query 3.8
```
What was the minimum intraday low?
```
**Expected:** Lowest value in Low column across all dates

---

## Category 4: Average Calculations (5 queries)
**Expected:** Numerical average with explanation

### Query 4.1
```
What is the average closing price?
```
**Expected:** Mean of all closing prices

---

### Query 4.2
```
What is the average opening price?
```

---

### Query 4.3
```
What is the average daily trading volume?
```
**Note:** Volume is in format "79.65M" - needs parsing

---

### Query 4.4
```
What is the average percentage change?
```

---

### Query 4.5
```
What is the average daily price range (High − Low)?
```
**Expected:** Calculate High - Low for each day, then average

---

## Category 5: Filtering (6 queries)
**Expected:** List of matching days

### Query 5.1
```
Show all days where the closing price was above $390.
```
**Expected:** Multiple rows, formatted nicely

---

### Query 5.2
```
List all trading days where Microsoft closed below $380.
```

---

### Query 5.3
```
Find all days with a positive daily change.
```

---

### Query 5.4
```
Find all days with a negative daily change.
```

---

### Query 5.5
```
Show days where trading volume exceeded 50 million.
```
**Note:** Volume parsing challenge (M suffix)

---

### Query 5.6
```
List all days where the opening price was higher than the closing price.
```
**Expected:** "Red days" (bearish days)

---

## Category 6: Sorting (4 queries)
**Expected:** Ordered list, top N

### Query 6.1
```
List the top 10 highest closing prices.
```

---

### Query 6.2
```
List the top 5 trading volume days.
```

---

### Query 6.3
```
Show the five lowest closing prices.
```

---

### Query 6.4
```
Rank all trading days by percentage change.
```
**Expected:** All days sorted by Change %

---

## Category 7: Date Range Queries (5 queries)
**Expected:** Filtered by month/date range

### Query 7.1
```
What was Microsoft's average closing price during June 2026?
```

---

### Query 7.2
```
Show all trading days in May 2026.
```

---

### Query 7.3
```
How many trading days are available in March 2026?
```

---

### Query 7.4
```
What was the highest closing price in April 2026?
```

---

### Query 7.5
```
What was the lowest price recorded during January 2026?
```

---

## Category 8: Trend Analysis (5 queries)
**Expected:** Analytical insights

### Query 8.1
```
Did Microsoft's stock generally increase or decrease during June 2026?
```
**Expected:** Compare first vs last day of June, overall trend

---

### Query 8.2
```
How many consecutive days did the stock gain?
```
**Expected:** Longest streak of positive changes

---

### Query 8.3
```
What was the longest losing streak?
```

---

### Query 8.4
```
Was the stock more volatile in May or June?
```
**Expected:** Compare standard deviation or range

---

### Query 8.5
```
Describe the overall price trend over the dataset.
```

---

## Category 9: Multi-Step Reasoning (5 queries)
**Expected:** Complex logic, multiple conditions

### Query 9.1
```
Which day had both a positive return and trading volume above 50 million?
```

---

### Query 9.2
```
Find the date where the stock closed higher than it opened and also had the highest trading volume.
```

---

### Query 9.3
```
Which month had the highest average closing price?
```

---

### Query 9.4
```
What is the difference between the highest and lowest closing prices?
```

---

### Query 9.5
```
On which day was the gap between High and Low the largest?
```

---

## Category 10: Mathematical Questions (5 queries)
**Expected:** Statistical calculations

### Query 10.1
```
What is the percentage increase from the first trading day to the last trading day?
```

---

### Query 10.2
```
Calculate the average daily return.
```

---

### Query 10.3
```
Calculate the median closing price.
```

---

### Query 10.4
```
What is the standard deviation of the closing price?
```

---

### Query 10.5
```
What is the total traded volume across the dataset?
```

---

## Category 11: Ranking (5 queries)

### Query 11.1
```
Top 10 biggest gains.
```

---

### Query 11.2
```
Top 10 biggest losses.
```

---

### Query 11.3
```
Top 10 highest volumes.
```

---

### Query 11.4
```
Top 10 lowest volumes.
```

---

### Query 11.5
```
Top 20 closing prices.
```

---

## Category 12: Counting (5 queries)

### Query 12.1
```
How many trading days had a positive return?
```

---

### Query 12.2
```
How many had a negative return?
```

---

### Query 12.3
```
How many days closed above $400?
```

---

### Query 12.4
```
How many days had trading volume greater than 40 million?
```

---

### Query 12.5
```
How many days opened higher than they closed?
```

---

## Category 13: Retrieval Accuracy (5 queries)

### Query 13.1
```
Tell me about trading on 18 June 2026.
```
**Expected:** All data for that date

---

### Query 13.2
```
Give me all information for 17-06-2026.
```

---

### Query 13.3
```
Summarize the market data for 15-06-2026.
```

---

### Query 13.4
```
What happened on 12-06-2026?
```

---

### Query 13.5
```
Show the complete record for 16-06-2026.
```

---

## Category 14: Hallucination Tests ⚠️ **CRITICAL**
**Expected:** Should say "data not available" NOT make up answers

### Query 14.1
```
What was Microsoft's closing price on 01-01-2028?
```
**✓ Pass if:** Says data not available for this date
**✗ Fail if:** Makes up a price

---

### Query 14.2
```
What happened on 30-02-2025?
```
**Note:** Invalid date (Feb 30 doesn't exist)
**Expected:** Says date invalid or not found

---

### Query 14.3
```
Tell me about Tesla stock in this dataset.
```
**✓ Pass if:** Says only Microsoft data available
**✗ Fail if:** Makes up Tesla data

---

### Query 14.4
```
What was Apple's highest closing price?
```
**✗ Fail if:** Returns any Apple data

---

### Query 14.5
```
What was Microsoft's dividend payment in this dataset?
```
**✓ Pass if:** Says dividend data not in dataset
**✗ Fail if:** Makes up dividend amounts

---

### Query 14.6
```
What was the PE ratio?
```
**Expected:** Not in dataset

---

### Query 14.7
```
What were Microsoft's earnings?
```
**Expected:** Not in dataset

---

### Query 14.8
```
What was the RSI on 17-06-2026?
```
**Expected:** RSI not calculated in this dataset

---

### Query 14.9
```
What was the MACD?
```
**Expected:** Not in dataset

---

### Query 14.10
```
What was the moving average for 20 days?
```
**Expected:** Could calculate from data OR say not pre-calculated

---

## Category 15: Complex RAG Evaluation (10 queries)

### Query 15.1
```
Summarize Microsoft's stock performance over the entire dataset.
```

---

### Query 15.2
```
Identify the five most volatile trading days and explain why.
```

---

### Query 15.3
```
Which month showed the strongest bullish trend?
```

---

### Query 15.4
```
Which month had the largest average trading volume?
```

---

### Query 15.5
```
Identify any unusual spikes in trading volume.
```

---

### Query 15.6
```
Find dates where Microsoft gained more than 2% in a single day.
```

---

### Query 15.7
```
Find dates where Microsoft lost more than 3% in a single day.
```

---

### Query 15.8
```
Explain whether higher trading volume generally corresponded with larger price movements.
```

---

### Query 15.9
```
Which trading day had the widest intraday range (High − Low)?
```

---

### Query 15.10
```
Provide a month-by-month summary of Microsoft's stock performance.
```

---

## Testing Checklist

For EACH query, verify:

- [ ] ✅ Response is natural and conversational
- [ ] ✅ Response is factually accurate
- [ ] ✅ No hallucinations or made-up data
- [ ] ✅ No technical jargon exposed to user
- [ ] ✅ No unnecessary disclaimers
- [ ] ✅ Formatting is clean and readable
- [ ] ✅ If error occurs, check console for generated code

---

## Priority Tests

**Start with these high-priority tests:**

1. **Query 3.3** - "Which day recorded the highest trading volume?" (The bug we just fixed)
2. **Query 2.3** - "Was Microsoft's stock price higher on 12-06-2026 or 18-06-2026?" (Previous bug)
3. **Query 14.1-14.10** - All hallucination tests (Critical for accuracy)
4. **Query 5.5** - "Show days where trading volume exceeded 50 million." (Tests "M" parsing)

---

## Expected Pass Rate

| Category | Target Pass Rate |
|----------|------------------|
| Basic Retrieval | 100% |
| Compare Two Dates | 100% |
| Highest/Lowest | 100% |
| Averages | 95% |
| Filtering | 90% |
| Sorting | 90% |
| Hallucination Tests | 100% (CRITICAL) |
| Complex RAG | 85% |

**Overall Target:** 95%+ pass rate

---

## How to Report Issues

If a query fails:

1. **Copy the query**
2. **Copy the response**
3. **Check server console for generated code**
4. **Note what went wrong:**
   - Wrong answer?
   - Error message?
   - Hallucination?
   - Code generation issue?

Example:
```
Query: "Which day recorded the highest trading volume?"
Error: Series.to_dict() takes 1 positional argument but 2 were given
Generated Code: result = df.loc[max_idx].to_dict('records')  # ❌ BUG
Fix Needed: Should be .to_dict() not .to_dict('records')
```

---

## Quick Start Commands

```bash
# Terminal 1: Start server
cd /Users/rkkbti/Downloads/Finance-bot
python manage.py runserver

# Terminal 2: Monitor logs
cd /Users/rkkbti/Downloads/Finance-bot
tail -f nohup.out  # If using nohup

# Or just watch the server terminal for debug output
```

---

## Good Luck! 🚀

Start with Category 1 and work your way through systematically.

Report back with:
- ✅ Queries that worked perfectly
- ⚠️ Queries that worked but need improvement
- ❌ Queries that failed completely
