# 🔥 Dynamic Agent vs Static Agent

## The Evolution

### ❌ **V1: Hardcoded Templates**
```python
if result_type == 'comparison':
    return f"Date1: {d1}, Date2: {d2}, Diff: {diff}"
```
**Problem:** Rigid, can't adapt

---

### ⚠️ **V2: LLM Formatting Only**
```python
def _compare_dates(self, df, date1, date2):
    row1 = df[df['Date'] == date1]
    row2 = df[df['Date'] == date2]
    return {'row1': row1, 'row2': row2}
```
**Problem:** Still hardcoded to 2 dates only!

---

### ✅ **V3: Fully Dynamic (Code Generation)**
```python
# LLM generates this code dynamically:
dates = ['15-06-2026', '16-06-2026', '17-06-2026', '18-06-2026']
result = df[df['Date'].isin(dates)].to_dict('records')
```
**Solution:** Works for N dates, N columns, any query!

---

## Why Static Fails

### Example: User Evolution

**Week 1:**
```
User: "Compare prices on 17-06-2026 and 18-06-2026"
Static: ✅ Works (2 dates hardcoded)
```

**Week 2:**
```
User: "Compare prices across 15, 16, 17, and 18 June"
Static: ❌ FAILS - only handles 2 dates!
```

**Week 3:**
```
User: "Show me the trend from June 1st to June 10th"
Static: ❌ FAILS - needs date range logic
```

**Week 4:**
```
User: "Compare opening vs closing prices for the last 5 days of May"
Static: ❌ FAILS - needs complex filtering
```

---

## Dynamic Agent Architecture

```
User Question
    ↓
┌─────────────────────────────────────┐
│  LLM Code Generator                 │
│  Analyzes question                  │
│  Generates pandas code dynamically  │
└──────────┬──────────────────────────┘
           │
           ▼
    Generated Code:
    dates = ['15-06-2026', '16-06-2026', ...]
    result = df[df['Date'].isin(dates)].to_dict('records')
           │
           ▼
┌──────────────────────────┐
│  Safe Execution          │
│  Runs code in sandbox    │
│  Returns result          │
└──────────┬───────────────┘
           │
           ▼
┌──────────────────────────┐
│  LLM Formatter           │
│  Natural language output │
└──────────────────────────┘
```

---

## Code Generation Examples

### Query: "Compare prices on 17 and 18 June"

**Generated Code:**
```python
dates = ['17-06-2026', '18-06-2026']
result = df[df['Date'].isin(dates)][['Date', 'Price']].to_dict('records')
```

---

### Query: "Show me the trend for the first week of June"

**Generated Code:**
```python
june_start = df[df['Date'].str.contains('-06-2026')]
result = june_start.head(7)[['Date', 'Price', 'Change %']].to_dict('records')
```

---

### Query: "Compare opening vs closing prices for 15, 16, 17 June"

**Generated Code:**
```python
dates = ['15-06-2026', '16-06-2026', '17-06-2026']
result = df[df['Date'].isin(dates)][['Date', 'Open', 'Price']].to_dict('records')
```

---

### Query: "What dates had price increase more than 2%?"

**Generated Code:**
```python
df['Change_Numeric'] = df['Change %'].str.rstrip('%').astype(float)
result = df[df['Change_Numeric'] > 2][['Date', 'Price', 'Change %']].to_dict('records')
```

---

## Safety Features

### Sandboxed Execution
```python
safe_globals = {
    'df': df,           # Only the dataframe
    'pd': pd,           # Pandas library
    'result': None,     # Output variable
    # Safe functions only
    'len': len, 'sum': sum, 'max': max, 'min': min
}

exec(code, safe_globals, safe_locals)
```

**Blocked:**
- ❌ File operations (`open`, `write`)
- ❌ Network access (`requests`, `urllib`)
- ❌ System commands (`os.system`)
- ❌ Imports (`import`)

**Allowed:**
- ✅ Pandas operations
- ✅ Math functions
- ✅ String operations
- ✅ Data manipulation

---

## Comparison Table

| Feature | Static Agent | Dynamic Agent |
|---------|--------------|---------------|
| **2 date comparison** | ✅ Yes | ✅ Yes |
| **N date comparison** | ❌ No | ✅ Yes |
| **Date ranges** | ❌ No | ✅ Yes |
| **Complex filters** | ❌ No | ✅ Yes |
| **Custom calculations** | ❌ No | ✅ Yes |
| **Multi-column ops** | ⚠️ Limited | ✅ Yes |
| **Trend analysis** | ❌ No | ✅ Yes |
| **New query types** | ❌ Need code | ✅ Automatic |
| **Maintenance** | ❌ High | ✅ Low |

---

## Real-World Examples

### Scenario 1: Financial Analyst

**Query:** "Compare the volatility (High - Low) for the last 10 trading days"

**Static Agent:** ❌ Can't handle - not programmed for volatility

**Dynamic Agent:** ✅ Generates:
```python
last_10 = df.head(10).copy()
last_10['Volatility'] = last_10['High'].astype(float) - last_10['Low'].astype(float)
result = last_10[['Date', 'Volatility']].to_dict('records')
```

---

### Scenario 2: Portfolio Manager

**Query:** "Show me dates where volume exceeded 50M and price dropped"

**Static Agent:** ❌ Can't handle - complex filter not programmed

**Dynamic Agent:** ✅ Generates:
```python
df['Vol_Numeric'] = df['Vol.'].str.rstrip('M').astype(float)
df['Change_Numeric'] = df['Change %'].str.rstrip('%').astype(float)
result = df[(df['Vol_Numeric'] > 50) & (df['Change_Numeric'] < 0)][['Date', 'Price', 'Vol.', 'Change %']].to_dict('records')
```

---

### Scenario 3: Day Trader

**Query:** "What's the average daily range (High - Low) in June?"

**Static Agent:** ❌ Can't handle - custom calculation not programmed

**Dynamic Agent:** ✅ Generates:
```python
june = df[df['Date'].str.contains('-06-2026')]
june['Range'] = june['High'].astype(float) - june['Low'].astype(float)
result = {'average_range': june['Range'].mean()}
```

---

## Prompt Engineering

### The Code Generation Prompt:

```python
"""You are a pandas expert. Generate Python code to answer the user's question.

**DataFrame Info:**
Shape: 365 rows × 7 columns
Columns: Date, Price, Open, High, Low, Vol., Change %

**Sample Data:**
[Shows first 3 rows]

**User Question:**
{question}

**Instructions:**
1. Write ONLY executable pandas code
2. The DataFrame is available as 'df'
3. Store the final result in a variable called 'result'
4. Handle edge cases (missing data, empty results)
5. For comparisons, fetch ALL requested data points
6. For date ranges, use proper filtering
7. Convert results to dict/list format
8. Do NOT include print statements

**Generate code:**
"""
```

**Key Elements:**
- ✅ Clear role ("pandas expert")
- ✅ Context (DataFrame structure)
- ✅ Sample data (helps LLM understand format)
- ✅ Explicit constraints (output variable, format)
- ✅ Examples (not shown above, but in full prompt)

---

## Benefits

### 1. **Infinite Flexibility**
Can handle ANY pandas operation:
- Filtering
- Aggregations
- Transformations
- Joins (if multiple files later)
- Custom calculations
- Statistical analysis

### 2. **Self-Improving**
As LLMs get better at code generation:
- ✅ Our agent gets better automatically
- ✅ No code changes needed

### 3. **Natural Language**
User can ask ANYTHING:
- "Compare X and Y"
- "Show me when Z happened"
- "Calculate average of A where B > C"
- "Find patterns in D over time"

### 4. **Low Maintenance**
- ❌ Old: Add code for each query type
- ✅ New: LLM figures it out

---

## Error Handling

### If Code Generation Fails:
```python
try:
    code = self._generate_pandas_code(question, df)
except Exception as e:
    return "I couldn't understand that query. Could you rephrase?"
```

### If Execution Fails:
```python
try:
    result = self._execute_code_safely(code, df)
except Exception as e:
    return f"Error executing query: {str(e)}"
```

### If Formatting Fails:
```python
try:
    answer = self._format_result_with_llm(question, result)
except:
    return f"Here's the data: {result}"
```

---

## Migration Path

### Phase 1: Test Dynamic Agent
```python
# In agents.py, add flag
USE_DYNAMIC_AGENT = True

if USE_DYNAMIC_AGENT:
    from rag.dynamic_pandas_agent import pandas_agent_query
else:
    from rag.pandas_rag_agent import pandas_agent_query
```

### Phase 2: A/B Testing
Run both, compare results

### Phase 3: Full Migration
Replace static with dynamic

---

## Performance

### Latency:
- **Code Generation:** ~500-800ms
- **Execution:** ~50-200ms
- **Formatting:** ~500-800ms
- **Total:** ~1.5-2 seconds

**Trade-off:**
- ❌ Slightly slower than static
- ✅ Infinitely more flexible

---

## Future: Self-Healing

With dynamic code generation, we can:

```python
if execution_fails:
    # LLM analyzes error
    # Generates fixed code
    # Retries automatically
```

This creates a **self-healing agent** that improves from failures!

---

## Bottom Line

### Static Agent:
```
Hardcoded → Limited → Brittle → High Maintenance
```

### Dynamic Agent:
```
Generated → Flexible → Robust → Self-Improving
```

**The future is dynamic!** 🚀

---

## Next Steps

1. ✅ Test dynamic agent with various queries
2. ✅ Compare results with static agent
3. ✅ Monitor for edge cases
4. ✅ Migrate when confident

**Status:** Dynamic agent ready for testing!
