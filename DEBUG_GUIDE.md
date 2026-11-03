# 🐛 Debug Guide for Dynamic Agent

## The Issue You Found

**Query:** "Was Microsoft's stock price higher on 12-06-2026 or 18-06-2026?"

**Wrong Response:** "Unfortunately, we don't have information on the stock price for 18-06-2026"

**Reality:** Both dates ARE in the CSV! 
- 12-06-2026: $390.74
- 18-06-2026: $379.40

---

## Why This Happened

The LLM-generated code likely:
1. Only checked one date
2. Had a logic error
3. Didn't properly extract both dates from the question

---

## How to Debug

### Step 1: Enable Debug Mode

Debug mode is now ON by default. You'll see console output showing:

```
============================================================
GENERATED CODE:
============================================================
dates = ['12-06-2026', '18-06-2026']
result = df[df['Date'].isin(dates)][['Date', 'Price']].to_dict('records')
============================================================

============================================================
EXECUTION RESULT:
============================================================
[
  {'Date': '12-06-2026', 'Price': '390.74'},
  {'Date': '18-06-2026', 'Price': '379.40'}
]
============================================================
```

### Step 2: Check the Console

When you restart the server and submit a query, check the terminal where Django is running.

You'll see:
- The exact pandas code generated
- The raw result from execution
- Any errors

### Step 3: Common Issues

**Issue 1: Date extraction failed**
```python
# Bad code:
dates = ['12-06-2026']  # Only found one date!

# Good code:
dates = ['12-06-2026', '18-06-2026']  # Both dates
```

**Issue 2: Logic error**
```python
# Bad code:
if df[df['Date'] == '18-06-2026'].empty:
    result = {'error': 'Not found'}  # Gives up too easily

# Good code:
dates = ['12-06-2026', '18-06-2026']
result = df[df['Date'].isin(dates)].to_dict('records')  # Get both
```

**Issue 3: Wrong column check**
```python
# Bad code:
result = df[df['Date'] == date].iloc[0]  # Crashes if empty

# Good code:
filtered = df[df['Date'].isin(dates)]
if not filtered.empty:
    result = filtered.to_dict('records')
```

---

## Testing Protocol

### Test 1: Two Date Comparison
```
Query: "Was the price higher on 12-06-2026 or 18-06-2026?"

Expected Code:
dates = ['12-06-2026', '18-06-2026']
result = df[df['Date'].isin(dates)][['Date', 'Price']].to_dict('records')

Expected Result:
[
  {'Date': '12-06-2026', 'Price': '390.74'},
  {'Date': '18-06-2026', 'Price': '379.40'}
]

Expected Answer:
"The price was higher on 12-06-2026 ($390.74) compared to 18-06-2026 ($379.40)"
```

### Test 2: Multiple Dates
```
Query: "Compare prices on 15, 16, 17, 18 June"

Expected Code:
dates = ['15-06-2026', '16-06-2026', '17-06-2026', '18-06-2026']
result = df[df['Date'].isin(dates)].to_dict('records')

Expected Result:
[Four rows with all data]

Expected Answer:
Natural language explanation with trend analysis
```

### Test 3: Date Range
```
Query: "Show me prices from June 1 to June 10"

Expected Code:
june_data = df[df['Date'].str.contains('-06-2026')].head(10)
result = june_data[['Date', 'Price']].to_dict('records')

Expected Result:
[10 rows]
```

---

## Fixing Code Generation Issues

### If Code is Wrong:

**Option 1: Improve Prompt**
Edit `rag/dynamic_pandas_agent.py`, function `_generate_pandas_code()`, and enhance the instructions.

**Option 2: Add More Examples**
Add specific examples for the failing query type in the prompt.

**Option 3: Fallback to Static**
```python
# In agents.py
USE_DYNAMIC_AGENT = False  # Temporarily use static agent
```

---

## Debugging Workflow

```
1. User submits query
   ↓
2. Check console for GENERATED CODE
   ↓
3. Is the code correct?
   ├─ YES → Check EXECUTION RESULT
   │          ├─ Correct? → Issue is in LLM formatting
   │          └─ Wrong? → Pandas code has logic error
   └─ NO → Issue is in code generation (LLM prompt needs improvement)
```

---

## Common Fixes

### Fix 1: Better Date Extraction

**Add to prompt:**
```
For questions like "which date had higher X", extract ALL dates mentioned:
- "12-06-2026 or 18-06-2026" → ['12-06-2026', '18-06-2026']
- "15, 16, 17 June" → ['15-06-2026', '16-06-2026', '17-06-2026']
```

### Fix 2: Always Fetch Data First

**Add to prompt:**
```
NEVER say data doesn't exist without checking first.
ALWAYS fetch the data, then let the formatter handle the response.
```

### Fix 3: Robust Error Handling

**Add to examples:**
```python
# Check if data exists before processing
filtered = df[df['Date'].isin(dates)]
if filtered.empty:
    result = {'error': 'Dates not found', 'searched': dates}
else:
    result = filtered.to_dict('records')
```

---

## Monitor These Metrics

1. **Code Generation Success Rate**
   - How often does LLM generate valid pandas code?
   - Target: >95%

2. **Data Retrieval Accuracy**
   - Does the code fetch the right data?
   - Target: 100%

3. **Response Quality**
   - Is the final answer natural and correct?
   - Target: >90%

---

## When to Use Static vs Dynamic

### Use Dynamic When:
- ✅ Query needs flexibility (N dates, complex filters)
- ✅ You want natural language adaptability
- ✅ LLM is generating correct code consistently

### Use Static When:
- ✅ Query patterns are well-defined (exactly 2 dates)
- ✅ Speed is critical (<1s response time)
- ✅ LLM code generation is unreliable for this query type

---

## Current Status

**Debug Mode:** ✅ Enabled

**Next Steps:**
1. Restart server
2. Try the failing query again
3. Check console for generated code
4. Analyze where it went wrong
5. Fix prompt or add examples

---

## Quick Restart

```bash
# Stop server (Ctrl+C)
python manage.py runserver
```

Now try the query again and watch the console!

---

## Expected Console Output

```
Entered finance agent - File type: tabular
Routing to Pandas Agent...

============================================================
GENERATED CODE:
============================================================
dates = ['12-06-2026', '18-06-2026']
result = df[df['Date'].isin(dates)][['Date', 'Price']].to_dict('records')
============================================================

============================================================
EXECUTION RESULT:
============================================================
[{'Date': '12-06-2026', 'Price': '390.74'}, {'Date': '18-06-2026', 'Price': '379.40'}]
============================================================

[27/Jun/2026 16:00:00] "POST /ask/ HTTP/1.1" 200 1234
```

If you see this, the dynamic agent is working correctly! ✅

If you see different code or wrong results, we'll know exactly where to fix it. 🐛

---

**The debug output will tell us exactly what went wrong!**
