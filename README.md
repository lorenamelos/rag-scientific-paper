# Scientific Paper Analysis System - RAG Implementation  
**Version:** 1.0  
**Author:** Lorena Melo  
**Last Updated:** 17 Feb 2025

---

## 📄 Overview  
This Jupyter Notebook provides an end-to-end Retrieval-Augmented Generation (RAG) system specifically designed for analyzing scientific papers. It enables users to:  

- Process PDF academic papers with specialized preprocessing  
- Create semantic search capabilities over documents  
- Ask natural language questions about paper content  
- Receive answers with citations from source material  

**Key Technologies Used**:  
- OpenAI GPT-4 & Embeddings API  
- ChromaDB vector database  
- Advanced text processing pipelines  

---

## ⚠️ Critical Requirements  
Before using this system, users **MUST**:  

1. **Obtain OpenAI API Key**  
   - Create account at [OpenAI Platform](https://platform.openai.com/)  
   - Generate API key in `API Keys` section  
   - **Important**: Add payment method - API usage incurs costs  

2. **Understand Costs**  
   | Service | Estimated Cost |  
   |---------|----------------|  
   | Embeddings | $0.00013/1k tokens |  
   | GPT-4 | $0.03/1k tokens input |  
   *Costs based on OpenAI pricing as of July 2024*  

---

## 🛠️ Features  

### 1. PDF Processing Engine  
- Header/footer removal  
- DOI detection and filtering  
- Academic structure preservation  

### 2. Vector Database  
- ChromaDB persistent storage  
- `text-embedding-3-large` embeddings (3072-dimension)  
- MMR search algorithm  

### 3. Question Answering  
- GPT-4 Turbo integration  
- Technical term handling  
- Page/section citation system  

---

## 🚀 Installation  

### Requirements  
- Python 3.10+  
- Libraries:  
```bash
pip install langchain langchain-community chromadb pypdf python-dotenv tiktoken
```

---

## ⚙️ Configuration  

1. **Environment Setup**  
   Create `.env` file:  
   ```ini
   OPENAI_API_KEY=sk-your-key-here
   ```

2. **Document Preparation**  
   - Place PDF in specified path:  
   ```python
   DOCUMENT_PATH = "/path/to/your/paper.pdf"
   ```

3. **System Settings** (Optional)  
   ```python
   class Config:
       CHUNK_SIZE = 1500  # Optimal for technical content
       MODEL_NAME = "gpt-4-1106-preview"  # Default model
   ```

---

## 💻 Usage  

### Basic Workflow  
1. **First Run**  
   ```bash
   python scientific_rag.py
   ```  
   - System will:  
     - Process PDF (~2-10 mins depending on paper size)  
     - Create ChromaDB vector store  

2. **Query Interface**  
   ```bash
   Scientific Analysis System Ready. Ask your question about the article (in English):

   
   Question: [Enter your question]
   ```
   🚨 To exit the loop, writ `exit` on the Question space


### Example Session  
```bash
Pergunta: What methodology did the authors use for evaluation?

Resposta:
The authors employed a hybrid evaluation approach combining:
- Quantitative metrics (F1-score: 92.4%, Table 3, page 12)
- Human expert validation (Section 4.2, page 8)
- Comparative analysis against baseline models (Figure 5, page 14)
```

---

## 🔧 Technical Notes  

### Cost Management Tips  
1. Monitor usage at [OpenAI Usage Dashboard](https://platform.openai.com/usage)  
2. For large papers, estimate costs using:  
   ```python
   from tiktoken import get_encoding

   def estimate_cost_from_documents(docs, cost_per_1k=0.0001):
    """
    Given a list of Document objects (each with a 'page_content' attribute),
    combine their text, count tokens using cl100k_base, and return the token count and cost.
    """
    # Use the tokenizer (for GPT-3.5, GPT-4, text-embedding-ada-002)
    encoder = get_encoding("cl100k_base")
    
    # Combine text from all documents into a single string
    combined_text = " ".join(doc.page_content for doc in docs)
    
    # Encode the text to count tokens
    tokens = encoder.encode(combined_text)
    num_tokens = len(tokens)
    
    # Calculate estimated cost (cost per 1K tokens)
    estimated_cost = (num_tokens / 1000) * cost_per_1k
    return num_tokens, estimated_cost

   # Example usage:
   # Assume 'load_scientific_paper' is your function that returns the list of documents
   documents = load_scientific_paper(config.DOCUMENT_PATH)
   num_tokens, cost = estimate_cost_from_documents(documents, cost_per_1k=0.0001)
   print(f"Number of tokens: {num_tokens}")
   print(f"Estimated cost: ${cost:.4f}")
   
   ```

### Model Customization  
Available OpenAI Models:  
```python
# In Config class:
MODEL_NAME = "gpt-4-turbo"  # Alternative
# MODEL_NAME = "gpt-3.5-turbo"  # Cheaper but less accurate
```

---

## 📜 Support & Disclaimer  

### Support Includes  
- Code functionality  
- Configuration assistance  
- Basic usage guidance  

### Not Included  
- OpenAI account management  
- API cost optimization  
- Custom feature development  

### Legal Disclaimer  
```text
This product is dependent on third-party services (OpenAI). 
The developer is not responsible for:
- API service interruptions
- Content generated by AI models
- Financial charges from API usage
```

---

## 📬 Contact  
For support: lorenamelo.engr@gmail.com


---
