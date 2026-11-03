# Finance-bot Project Analysis Report
**Date**: June 27, 2026  
**Analyst**: Claude  
**Status**: Current Implementation Issues Identified

---

## Executive Summary

This Finance-bot is a Django-based RAG (Retrieval Augmented Generation) chatbot designed to answer questions about financial data, particularly from CSV files containing stock market data. The current implementation has **critical architectural flaws** causing hallucinations and poor retrieval performance, especially with numerical/tabular data. Additionally, the UI has usability issues including broken auto-scroll functionality and an outdated design.

---

## 1. Project Architecture Overview

### Tech Stack
- **Backend**: Django (Python web framework)
- **LLM**: Groq API with `llama-3.3-70b-versatile` model
- **Embeddings**: HuggingFace `BAAI/bge-small-en-v1.5`
- **Vector Store**: FAISS
- **Additional Retrieval**: BM25 (keyword-based)
- **Frontend**: Bootstrap 5 with vanilla JavaScript
- **Data Processing**: Pandas, LangChain

### File Structure
```
Finance-bot/
├── chatbot/
│   ├── agents.py          # Core agent logic (finance, general, classifier)
│   ├── views.py           # Django views (upload, query handling)
│   ├── templates/
│   │   └── index.html     # Main UI
│   └── static/js/
│       └── script.js      # Frontend logic
├── rag/
│   ├── loader.py          # Document loading (CSV, PDF, DOCX, XLSX)
│   ├── splitter.py        # Document chunking (CURRENTLY BROKEN)
│   ├── embeddings.py      # Embedding model config
│   ├── vectorstore.py     # FAISS vector store management
│   ├── retriever.py       # FAISS retrieval logic
│   ├── bm25.py            # BM25 keyword retrieval
│   └── pandas_agent.py    # Direct pandas calculations for profit/loss
├── media/uploads/         # User-uploaded files
│   └── MSFT Historical Data (1).csv
├── faiss_index/           # Persisted FAISS index
└── requirements.txt
```

---

## 2. Current Data Structure

### MSFT Historical Data CSV (365 rows)
```csv
Date,Price,Open,High,Low,Vol.,Change %
18-06-2026,379.40,377.82,381.37,373.28,59.71M,0.13%
17-06-2026,378.91,390.25,390.37,377.32,41.99M,-3.79%
...
```

**Characteristics**:
- Pure numerical tabular data
- Time-series financial data
- 7 columns: Date, Price, Open, High, Low, Vol., Change %
- Date format: DD-MM-YYYY
- Prices are floating-point numbers
- Volume has 'M' suffix (millions)

---

## 3. Critical Problems Identified

### 🚨 PROBLEM #1: Fundamentally Broken RAG Strategy for Numerical Data

**Root Cause**: Text embeddings are semantic similarity-based and **cannot understand numerical relationships**.

#### How Current RAG Works:
1. CSV loaded via LangChain's `CSVLoader`
2. Each row becomes a text string: `"Date: 18-06-2026\nPrice: 379.40\nOpen: 377.82..."`
3. Text is embedded using `BAAI/bge-small-en-v1.5` (semantic embeddings)
4. User asks: *"What was the price on 15-06-2026?"*
5. Embedding model tries to match question semantically to row text
6. **FAILURE**: Semantic similarity ≠ exact date/value matching

#### Why It Causes Hallucinations:
- Question: *"What was the highest price in May 2026?"*
- Retriever might return random rows with similar words like "high", "price", "May"
- LLM sees wrong context → generates plausible but incorrect answers
- Prompt tells LLM to "never hallucinate" but gives it wrong data → impossible task

#### Evidence from Code:
```python
# splitter.py - NO ACTUAL SPLITTING HAPPENING
def split_documents(documents):
    chunks = []
    for doc in documents:
        chunks.append(
            Document(
                page_content=doc.page_content,
                metadata=doc.metadata
            )
        )
    return chunks
```
The splitter just passes documents through without chunking, meaning entire CSV rows are embedded as-is.

---

### 🚨 PROBLEM #2: Ineffective Hybrid Retrieval

**Current Approach** (`agents.py` lines 46-73):
```python
# FAISS retrieval (k=5)
faiss_docs = retrieve_docs(vectorstore, question)

# BM25 retrieval (k=20)
bm25_docs = bm25.invoke(question)

# Combine both
retrieved_docs = bm25_docs + faiss_docs
```

**Issues**:
1. **BM25 also fails on numerical data**: BM25 is keyword-based but doesn't understand:
   - Date ranges
   - Numerical comparisons (max, min, average)
   - Temporal queries ("last month", "Q1 2026")

2. **Duplicate removal is naive**: Uses `doc.page_content` in a set, but similar rows with different dates might have identical text representation

3. **No ranking/reranking**: Simply concatenates results without considering relevance scores

4. **Context window pollution**: Sends 25 potentially irrelevant documents to LLM

---

### 🚨 PROBLEM #3: Hardcoded Regex Pattern for Profit/Loss

**In `agents.py` lines 22-45**:
```python
dates = re.findall(r'\d{2}-\d{2}-\d{4}', question)
shares_match = re.search(r'(\d+)\s+shares', question)
if len(dates) >= 2 and shares_match:
    # Use pandas_agent.calculate_profit_loss()
```

**Issues**:
- Only works if user types **exact format**: "buy on DD-MM-YYYY, sell on DD-MM-YYYY with X shares"
- Brittle pattern matching
- Doesn't handle natural language variations:
  - "I bought 100 shares in March 2026 and sold them last week"
  - "Calculate profit if I purchase on March 5th 2026"

**Why This Exists**: This is a **workaround** because RAG can't handle numerical queries. The developer added direct pandas calculations for one specific use case.

---

### 🚨 PROBLEM #4: Auto-Scroll Bug in UI

**Location**: `index.html` line 678

```javascript
chatBox.scrollTop = chatBox.scrollHeight;
```

**The Bug**:
- `chatBox` refers to `<div id="chatBox"></div>` (line 589)
- However, this div is **inside** `.chat-area` (line 456), which has `overflow-y:auto`
- `chatBox` itself has no height constraint or overflow
- **Result**: Scrolling the wrong element

**Should be**:
```javascript
let chatArea = document.querySelector('.chat-area');
chatArea.scrollTop = chatArea.scrollHeight;
```

---

### 🚨 PROBLEM #5: Outdated UI Design

**Current Design Issues**:
1. **Dark theme but poor contrast**: 
   - Background: `#0f172a` 
   - Text on cards: `#cbd5e1`
   - Low readability in dim lighting

2. **Rigid layout**: Fixed 3-9 column split, not responsive on mobile

3. **No loading indicators**: When processing query, user sees nothing

4. **No message timestamps**: Can't tell when responses were generated

5. **Sidebar features are non-functional**: "New Chat", "History", "Analytics", "Settings" do nothing

6. **Upload UI is confusing**: Says "Drop CSV" but requires clicking file input

7. **No error handling UI**: If query fails, user sees nothing in chat

---

## 4. Data Flow Analysis

### Current Query Flow:
```
User submits query
    ↓
classify_agent() → Determines "finance" or "general"
    ↓
[IF FINANCE]
    ↓
finance_agent(question, path)
    ↓
Check regex for profit/loss pattern
    ↓
[YES: Direct pandas calculation]
[NO: RAG retrieval]
    ↓
Load FAISS vectorstore
    ↓
Retrieve 5 docs from FAISS
    ↓
Load CSV, split, create BM25 index
    ↓
Retrieve 20 docs from BM25
    ↓
Merge + deduplicate
    ↓
Create context from 25 docs
    ↓
Send to LLM with strict prompt
    ↓
Return answer
```

**Performance Issues**:
- **BM25 index recreated on every query** (lines 57-60 in `agents.py`)
  ```python
  documents = load_document(path)
  chunks = split_documents(documents)
  bm25 = get_bm25_retriever(chunks)
  ```
  This is extremely inefficient for a 365-row CSV!

---

## 5. Why Hallucinations Occur

Despite aggressive prompt engineering (lines 85-108 in `agents.py`):
```python
prompt = f"""
    You are FinRAG AI.
    Rules:
    1. Use ONLY information from Context.
    2. Never hallucinate.
    3. If information is unavailable say so.
    4. Preserve exact numerical values.
    5. Preserve exact dates.
    ...
"""
```

**The LLM still hallucinates because**:
1. **Context is wrong**: Retrieved documents don't contain the answer
2. **Context is noisy**: 25 random rows when user asked about 1 specific date
3. **LLM tries to help**: When context has partial info, model infers/guesses
4. **Date format confusion**: User might ask "June 15, 2026" but CSV has "15-06-2026"

**Example Failure**:
- User: *"What was the highest price in June 2026?"*
- FAISS retrieves 5 random June rows (not necessarily the max)
- BM25 retrieves 20 rows containing "June", "2026", "price"
- LLM sees 25 rows with various prices
- LLM picks highest from **those 25** (not from all 365 rows)
- Answer is wrong but sounds confident

---

## 6. Recommendations Summary

### For RAG System:
1. **Replace text embedding RAG with SQL/Pandas-first approach**
   - Parse natural language to structured queries
   - Use pandas for filtering, aggregation, sorting
   - Only use LLM for query planning and result narration

2. **If keeping RAG, implement hybrid search properly**:
   - Add metadata filtering (date ranges, numerical ranges)
   - Use reranking model
   - Implement query decomposition for complex questions

3. **Cache BM25 index** or remove it entirely if switching to SQL/Pandas

4. **Add validation layer**: Check if retrieved context actually contains dates/values mentioned in question

### For UI:
1. **Fix auto-scroll**: Target `.chat-area` instead of `#chatBox`

2. **Add loading states**: Show spinner while processing query

3. **Redesign with modern framework**: Consider using React/Vue + Tailwind CSS

4. **Add error handling**: Display error messages in chat when query fails

5. **Make sidebar functional or remove it**

6. **Add message metadata**: Timestamps, copy button, regenerate button

---

## 7. Proposed Architecture (For Next Phase)

### Option A: Pandas-First Agent (Recommended for CSV data)
```
User Query → LLM Query Planner → Pandas Operations → LLM Result Narrator
```

**Advantages**:
- 100% accuracy for numerical queries
- No hallucinations
- Fast execution
- No vector indexing needed

**Implementation**:
- Use LangChain's `create_pandas_dataframe_agent`
- Or custom prompt-to-pandas parser
- Keep LLM for explaining results in natural language

### Option B: Enhanced RAG with Metadata Filtering
```
User Query → Query Analysis → Metadata Filter → Vector Search → Rerank → LLM
```

**Implementation**:
- Extract dates/numerical constraints from query
- Filter dataframe before embedding search
- Use remaining rows for semantic search
- Rerank results by relevance score

---

## 8. Code Quality Observations

**Good**:
- Clean file organization
- Proper use of Django sessions for file paths
- CSRF protection in frontend

**Needs Improvement**:
- No error handling in `agents.py` (what if FAISS index doesn't exist?)
- No logging (impossible to debug query failures)
- No unit tests
- Hardcoded values (`k=5`, `k=20`)
- No environment variable validation
- `splitter.py` has commented-out code that should be removed
- No requirements.txt version pinning (will break in future)

---

## 9. Critical Files Requiring Changes

| File | Priority | Issues |
|------|----------|--------|
| `chatbot/agents.py` | 🔴 HIGH | Core RAG logic, hallucination source |
| `rag/splitter.py` | 🔴 HIGH | Not actually splitting |
| `rag/retriever.py` | 🟡 MEDIUM | Basic retrieval, no advanced features |
| `chatbot/templates/index.html` | 🟡 MEDIUM | Auto-scroll bug, outdated design |
| `rag/bm25.py` | 🟡 MEDIUM | Needs caching |
| `chatbot/views.py` | 🟢 LOW | Works but needs error handling |

---

## 10. Estimated Effort for Fixes

| Task | Effort | Impact |
|------|--------|--------|
| Implement Pandas-first agent | 2-3 hours | 🔥 Eliminates hallucinations |
| Fix auto-scroll bug | 5 minutes | ✅ Quick win |
| Redesign UI with modern framework | 4-6 hours | 🎨 Major UX improvement |
| Add error handling & logging | 1-2 hours | 🛡️ Production readiness |
| Cache BM25 index | 30 minutes | ⚡ Performance boost |
| Add loading indicators | 30 minutes | ✅ Quick win |

---

## Conclusion

The Finance-bot's core issue is **architectural**: RAG with text embeddings is fundamentally unsuited for numerical/tabular data queries. The system attempts to force semantic similarity search onto structured data that requires exact matching and numerical operations.

**Immediate Actions**:
1. Replace RAG with pandas-based agent for numerical queries
2. Fix UI auto-scroll bug
3. Add loading indicators

**Next Phase**:
1. Complete UI redesign
2. Implement proper error handling
3. Add unit tests
4. Optimize caching

This analysis provides the foundation for a complete rebuild of the retrieval and UI systems.
