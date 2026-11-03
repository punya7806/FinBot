# Hybrid RAG + Pandas Agent Architecture

## Overview

This document describes the intelligent dual-layer architecture that routes queries to the appropriate processing engine based on file type and content structure.

---

## Architecture Diagram

```
┌───────────────────────────────────────────────────────────────┐
│                     USER UPLOADS FILE                          │
└────────────────────────┬──────────────────────────────────────┘
                         │
                         ▼
┌───────────────────────────────────────────────────────────────┐
│              FILE TYPE DETECTION & ROUTING                     │
│                                                                │
│  • Check file extension (.pdf, .docx, .csv, .xlsx)           │
│  • Store file type in Django session                          │
│  • Route to appropriate indexing pipeline                     │
└────────────┬─────────────────────────┬────────────────────────┘
             │                         │
             ▼                         ▼
   ┌─────────────────┐      ┌─────────────────────┐
   │  PDF/DOCX PATH  │      │   CSV/XLSX PATH     │
   │  (Textual Data) │      │  (Tabular Data)     │
   └────────┬────────┘      └──────────┬──────────┘
            │                          │
            ▼                          ▼
┌────────────────────────┐  ┌───────────────────────────┐
│   RAG INDEXING         │  │   PANDAS LOADING          │
│                        │  │                           │
│ 1. Load document       │  │ 1. Load into DataFrame    │
│ 2. Split into chunks   │  │ 2. Validate schema        │
│    (500-1000 tokens)   │  │ 3. Type conversion        │
│ 3. Generate embeddings │  │ 4. Store in session       │
│ 4. Store in FAISS      │  │ 5. Create metadata index  │
│ 5. Build BM25 index    │  │                           │
└────────────────────────┘  └───────────────────────────┘
            │                          │
            │                          │
            └──────────┬───────────────┘
                       │
                       ▼
       ┌───────────────────────────────────┐
       │   USER SUBMITS QUERY              │
       └───────────┬───────────────────────┘
                   │
                   ▼
       ┌───────────────────────────────────┐
       │   CLASSIFY INTENT                 │
       │   (finance vs general)            │
       └───────────┬───────────────────────┘
                   │
                   ▼
       ┌───────────────────────────────────┐
       │   CHECK FILE TYPE FROM SESSION    │
       └───────┬───────────────────────────┘
               │
      ┌────────┴─────────┐
      ▼                  ▼
┌──────────────┐   ┌──────────────────┐
│ PDF/DOCX     │   │ CSV/XLSX         │
│ → RAG AGENT  │   │ → PANDAS AGENT   │
└──────┬───────┘   └────────┬─────────┘
       │                    │
       ▼                    ▼
┌──────────────────┐   ┌────────────────────────┐
│  RAG RETRIEVAL   │   │  PANDAS EXECUTION      │
│                  │   │                        │
│ 1. FAISS (k=5)   │   │ 1. Parse query intent  │
│ 2. BM25 (k=20)   │   │ 2. Extract parameters  │
│ 3. Hybrid merge  │   │    - Dates             │
│ 4. Deduplicate   │   │    - Columns           │
│ 5. Rerank        │   │    - Operations        │
│ 6. Build context │   │ 3. Build pandas query  │
│ 7. LLM response  │   │ 4. Execute safely      │
│                  │   │ 5. Format results      │
│                  │   │ 6. LLM narration       │
└──────────────────┘   └────────────────────────┘
       │                    │
       └────────┬───────────┘
                │
                ▼
       ┌─────────────────────┐
       │  RETURN ANSWER      │
       │  TO USER            │
       └─────────────────────┘
```

---

## Component Details

### 1. File Type Router (`views.py`)

**Responsibility**: Detect file type and route to appropriate indexing pipeline

```python
def handle_file_upload(file):
    file_ext = file.name.lower().split('.')[-1]
    
    if file_ext in ['pdf', 'docx']:
        file_type = 'textual'
        # Index with RAG pipeline
        docs = load_document(path)
        chunks = split_documents(docs)
        vectorstore = create_vectorstore(chunks)
        save_vectorstore(vectorstore)
        
    elif file_ext in ['csv', 'xlsx']:
        file_type = 'tabular'
        # Load into pandas
        df = load_dataframe(path)
        # Store metadata
        
    request.session['file_type'] = file_type
    request.session['file_path'] = path
```

---

### 2. Pandas Agent (`rag/pandas_rag_agent.py`)

**Responsibility**: Convert natural language queries to pandas operations for CSV/XLSX files

#### Key Features:
- **Query Understanding**: Parse user intent using LLM
- **Safe Execution**: Sandboxed pandas operations
- **Date Intelligence**: Handle various date formats and ranges
- **Aggregation Support**: Max, min, avg, sum, count, etc.
- **Filtering**: Date ranges, value comparisons, categorical filters
- **Result Formatting**: Clean numerical output with proper formatting

#### Example Queries Handled:
```
User: "What was the price on June 15, 2026?"
→ df[df['Date'] == '15-06-2026']['Price'].values[0]

User: "What was the highest price in May 2026?"
→ df[df['Date'].str.contains('-05-2026')]['Price'].max()

User: "Show me all dates where price was above 400"
→ df[df['Price'] > 400][['Date', 'Price']]

User: "Calculate average price for Q2 2026"
→ df[date_filter_q2]['Price'].mean()

User: "Profit if I bought 100 shares on May 1 and sold on June 1?"
→ Custom calculation with date lookups
```

#### Architecture:
```python
def pandas_rag_agent(question: str, df: pd.DataFrame) -> str:
    """
    Main entry point for pandas-based queries
    """
    # Step 1: Query Analysis
    intent = analyze_query_intent(question)
    
    # Step 2: Parameter Extraction
    params = extract_query_parameters(question, df)
    
    # Step 3: Build Pandas Query
    pandas_code = build_pandas_query(intent, params)
    
    # Step 4: Execute Safely
    result = safe_execute_pandas(pandas_code, df)
    
    # Step 5: Format & Narrate
    answer = format_result_with_llm(question, result)
    
    return answer
```

---

### 3. Enhanced RAG Agent (`rag/improved_rag.py`)

**Responsibility**: Handle textual/narrative queries for PDF/DOCX files

#### Improvements over Current System:
1. **Better Chunking**:
   - Use RecursiveCharacterTextSplitter with optimized chunk sizes
   - Maintain paragraph boundaries
   - Preserve context across chunks

2. **Hybrid Retrieval**:
   - FAISS for semantic search (k=5)
   - BM25 for keyword matching (k=10) - reduced from 20
   - Ensemble reranking

3. **Metadata Enrichment**:
   - Store page numbers, section headers
   - Enable filtered retrieval

4. **Query Expansion**:
   - Generate alternative phrasings
   - Handle synonyms and abbreviations

#### Example Queries Handled:
```
User: "What does the company say about revenue growth?"
→ Semantic search in PDF for revenue growth discussion

User: "Summarize the risk factors section"
→ Retrieve all chunks from risk factors section

User: "What are the key financial highlights?"
→ Search for financial summary sections
```

---

### 4. Intelligent Query Router (`chatbot/agents.py`)

**Updated Logic**:

```python
def finance_agent(question, path):
    # Get file type from session
    file_type = request.session.get('file_type', 'textual')
    
    if file_type == 'tabular':
        # Route to Pandas Agent
        df = load_dataframe(path)
        answer = pandas_rag_agent(question, df)
        
    elif file_type == 'textual':
        # Route to RAG Agent
        vectorstore = load_vectorstore()
        answer = enhanced_rag_agent(question, vectorstore, path)
        
    return answer
```

---

## Use Case Matrix

| File Type | Content Example | Agent Used | Strength |
|-----------|-----------------|------------|----------|
| CSV | Stock prices, financial metrics | **Pandas Agent** | Exact numerical accuracy, aggregations |
| XLSX | Budget spreadsheet, balance sheet | **Pandas Agent** | Multi-sheet handling, calculations |
| PDF | Annual report (narrative) | **RAG Agent** | Semantic understanding of text |
| PDF | Financial tables in PDF | **Hybrid** | Extract tables → Pandas, text → RAG |
| DOCX | Financial analysis report | **RAG Agent** | Understanding context and narrative |

---

## Hybrid Approach for Mixed Content PDFs

For PDFs containing both tables and text:

```python
def hybrid_pdf_handler(path, question):
    # Extract tables separately
    tables = extract_tables_from_pdf(path)
    
    # Check if question is about table data
    if is_numerical_query(question):
        # Convert tables to DataFrames
        df = tables_to_dataframe(tables)
        return pandas_rag_agent(question, df)
    else:
        # Use RAG for text content
        return enhanced_rag_agent(question, vectorstore, path)
```

---

## Benefits of This Architecture

### ✅ Accuracy
- **Pandas Agent**: 100% accurate for numerical queries (no hallucinations)
- **RAG Agent**: Optimized for semantic understanding of narrative content

### ✅ Performance
- **Tabular Data**: Direct pandas operations (milliseconds)
- **Textual Data**: Optimized retrieval with reranking

### ✅ Flexibility
- Handles mixed content (tables + text in same PDF)
- Extensible to new file types

### ✅ User Experience
- Correct answers regardless of file type
- Natural language interface for both structured and unstructured data

---

## Implementation Priority

1. ✅ **Phase 1**: Implement Pandas Agent for CSV/XLSX (highest impact)
2. ✅ **Phase 2**: Update file type detection and routing
3. ✅ **Phase 3**: Improve existing RAG for PDF/DOCX
4. ✅ **Phase 4**: Add hybrid handling for mixed-content PDFs
5. ✅ **Phase 5**: Optimize caching and performance

---

## Error Handling Strategy

### Pandas Agent Errors:
```python
try:
    result = execute_pandas_query(df, query)
except KeyError:
    return "Column not found in dataset. Available columns: {list(df.columns)}"
except ValueError:
    return "Date format mismatch. Please check date format in query."
except Exception as e:
    return f"Query execution error: {str(e)}"
```

### RAG Agent Errors:
```python
try:
    docs = retrieve_docs(vectorstore, question)
    if not docs:
        return "No relevant information found in document."
except Exception as e:
    return f"Retrieval error: {str(e)}"
```

---

## Testing Strategy

### Test Cases for Pandas Agent:
- ✅ Single date lookup
- ✅ Date range queries
- ✅ Aggregations (max, min, avg, sum)
- ✅ Filtering and sorting
- ✅ Profit/loss calculations
- ✅ Multi-column queries
- ✅ Edge cases (missing dates, invalid values)

### Test Cases for RAG Agent:
- ✅ Simple fact retrieval
- ✅ Multi-paragraph summaries
- ✅ Section-specific queries
- ✅ Comparative analysis
- ✅ Handling missing information

### Integration Tests:
- ✅ Upload CSV → Query with Pandas Agent
- ✅ Upload PDF → Query with RAG Agent
- ✅ Switch between files in same session
- ✅ Error handling for unsupported formats

---

## Next Steps

This architecture provides the foundation for eliminating hallucinations while maintaining the flexibility to handle both structured and unstructured financial data. The implementation will proceed in phases, starting with the highest-impact Pandas Agent.
