# Clinical AI Assistant 🏥

An advanced AI assistant powered by GPT-4o and state-of-the-art RAG (Retrieval-Augmented Generation) technology, designed to answer clinicians' questions in real time with credible, evidence-based medical information.

## ✨ Features

- **🤖 GPT-4o Integration**: Uses OpenAI's latest and most efficient model
- **🎯 Advanced RAG System**: Retrieves relevant medical literature before answering
- **📚 Source Citations**: Every answer includes references to source documents
- **💾 Smart Caching**: Vector store is cached to avoid rebuilding on every run
- **🔄 Retry Logic**: Automatic retry with exponential backoff for API failures
- **🎨 Interactive CLI**: User-friendly command-line interface
- **🔍 Better Embeddings**: Uses all-mpnet-base-v2 for superior semantic search
- **⚡ Fast Performance**: Optimized for speed and efficiency

## 🚀 Quick Start

### 1. Install Dependencies

```powershell
pip install -r requirements.txt
```

### 2. Configure API Key

Create a `.env` file in the project root:

```env
OPENAI_API_KEY='sk-your-api-key-here'
```

Alternatively, set it as an environment variable:

```powershell
$env:OPENAI_API_KEY='sk-your-api-key-here'
```

### 3. Run the Application

```powershell
python main.py
```

## 📖 How It Works

### Data Sources

The system processes medical literature from the `datasets/` folder:
- **COVID-19**: 5 PDFs + 1 CSV (clinical trials)
- **Diabetes**: 5 PDFs + 1 CSV (clinical trials)
- **Heart Attack**: 5 PDFs + 1 CSV (clinical trials)
- **Knee Injury**: 5 PDFs + 1 CSV (clinical trials)

### Processing Pipeline

1. **Document Loading**: Reads PDFs and CSVs with metadata preservation
2. **Text Chunking**: Splits documents into 1000-character chunks with 200-char overlap
3. **Embedding Generation**: Creates semantic embeddings using all-mpnet-base-v2
4. **Vector Storage**: Stores embeddings in ChromaDB for fast retrieval
5. **Query Processing**: 
   - Retrieves top 8 most relevant chunks using MMR (Maximum Marginal Relevance)
   - Sends context + query to GPT-4o
   - Returns answer with source citations

## 🎯 Key Improvements

### Model Upgrades
- ✅ **GPT-4o** instead of GPT-4 (faster, cheaper, better)
- ✅ **all-mpnet-base-v2** embeddings instead of all-MiniLM-L6-v2 (higher accuracy)

### Performance Enhancements
- ✅ **Vector Store Caching**: Loads existing store instead of rebuilding
- ✅ **Optimized Chunking**: Better chunk size (1000) and overlap (200)
- ✅ **MMR Retrieval**: Balances relevance and diversity in search results

### Reliability Features
- ✅ **Retry Logic**: Automatic retry with exponential backoff (3 attempts)
- ✅ **Error Handling**: Graceful error messages and exception handling
- ✅ **Query Validation**: Validates input before processing

### User Experience
- ✅ **Interactive Mode**: Ask multiple questions in one session
- ✅ **Source Citations**: See which documents informed each answer
- ✅ **Progress Indicators**: Clear feedback during processing
- ✅ **.env Support**: Secure API key management

## 📊 Performance Metrics

- **Documents Loaded**: 133 (20 PDFs + 4 CSVs)
- **Total Chunks**: ~2,500-3,000 (optimized chunking)
- **First Run**: ~2-3 minutes (builds vector store)
- **Subsequent Runs**: ~5-10 seconds (loads cached store)
- **Query Response Time**: ~3-5 seconds

## 🛠️ Technical Stack

- **LLM**: OpenAI GPT-4o
- **Embeddings**: sentence-transformers/all-mpnet-base-v2
- **Vector Database**: ChromaDB
- **Framework**: LangChain
- **PDF Processing**: PyPDF
- **Retry Logic**: Tenacity

## 💡 Usage Examples

```
🔍 Your question: What are the best predictive algorithms for heart attack detection?

🤔 Thinking...

================================================================================
📋 ANSWER:
================================================================================
Based on the medical literature, several machine learning algorithms have shown
effectiveness in heart attack prediction:

1. **Support Vector Machine (SVM)**: Demonstrated high accuracy in detecting
   heart disease patterns with 85-90% accuracy in multiple studies.

2. **K-Nearest Neighbors (KNN)**: Effective for classification with accuracy
   rates around 82-87% when properly tuned.

3. **Random Forest**: Ensemble method showing robust performance with 88-92%
   accuracy and good handling of imbalanced datasets.

[Full detailed answer...]

--------------------------------------------------------------------------------
📚 SOURCES:
--------------------------------------------------------------------------------
1. heart attack - Machine_Learning_Approaches_for_Predicting_Heart_Attacks.pdf
2. heart attack - Machine_Learning_Model_for_Heart_Disease_Detection_A_Comparative_Analysis_of_SVM_vs_KNN.pdf
3. heart attack - Fog-Driven_Heart_Attack_Prediction_from_Wearable_Edge_Devices.pdf
================================================================================
```

## 🔧 Configuration

### Embedding Model
To change the embedding model, edit `model/rag_model.py`:
```python
embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/your-model-here"
)
```

### Chunk Size
Adjust chunking parameters in `model/rag_model.py`:
```python
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,  # Adjust as needed
    chunk_overlap=200  # Adjust as needed
)
```

### Retrieval Settings
Modify retrieval parameters in `query_rag_system()`:
```python
search_kwargs={
    "k": 8,  # Number of chunks to retrieve
    "fetch_k": 20,  # Candidates for MMR
    "lambda_mult": 0.7  # Relevance vs diversity balance
}
```

## 📝 Notes

- Vector store is cached in `./chroma/disease_studies/`
- CSV files limited to 100 rows to prevent memory issues
- First run downloads the embedding model (~420MB for all-mpnet-base-v2)
- Requires OpenAI API key with GPT-4o access

## 🐛 Troubleshooting

**Issue**: Memory errors during loading
- **Solution**: CSV files are already limited to 100 rows. If issues persist, reduce chunk_size.

**Issue**: Pydantic errors
- **Solution**: Run `pip install --upgrade pydantic`

**Issue**: Tokenizers version conflict
- **Solution**: Run `pip install 'tokenizers>=0.21,<0.22'`

**Issue**: API timeout
- **Solution**: Retry logic is built-in. Check your internet connection.

## 📄 License

This project is for educational and research purposes.
