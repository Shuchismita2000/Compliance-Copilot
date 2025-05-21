# Compliance-Copilot

## GenAI Assistant for Regulatory Intelligence

### 1. **Problem Statement**

> In highly regulated sectors like banking and fintech, compliance and risk teams are expected to stay updated with rapidly evolving guidelines from regulators such as **RBI**, **SEBI**, and **FATF**. These documents are lengthy, legalistic, and frequently updated.
> 
> Teams currently rely on manual search or traditional knowledge bases, which are:

- Time-consuming and error-prone
    
- Lacking natural language support
    
- Not adaptive to new or custom internal documents
    

This results in:

- Delayed compliance responses
    
- Increased regulatory risk
    
- Knowledge silos within the organization
    


### 2. **Solution Overview**

**Compliance Copilot** is a GenAI-powered assistant that enables finance, risk, and compliance professionals to:

- Ask natural language questions about regulatory documents (e.g., “What are RBI’s digital lending rules?”)
    
- Get contextual answers **grounded in official sources**
    
- View **citations** and **summaries** of lengthy sections
    
- Understand jargon via **explanation tools**
    

Built using:

- **LangChain agents** for tool orchestration
    
- **RAG pipeline** (Vector DB + retrieval)
    
- **OpenAI GPT-3.5 / local models** for generation
    
- Streamlit-based **interactive frontend**
    

### 3. **User Personas**

|Persona|Role|Use Case|
|---|---|---|
|**Asha**|Compliance Officer, Fintech Startup|Quickly answers founders' questions about RBI KYC norms|
|**Rahul**|Legal Analyst, VC-backed NBFC|Explores FATF rules for lending model risk|
|**Neha**|Product Manager, Digital Bank|Verifies SEBI disclosure rules for launching a new product|


### Tech Stack

|Layer|Tools / Libraries|
|---|---|
|LLM|OpenAI (GPT-4 / GPT-3.5), or Mistral|
|Embeddings|`text-embedding-3-small` or BGE|
|RAG|LangChain + FAISS / ChromaDB|
|Agent Framework|LangChain ReAct Agent + Tools|
|File Ingestion|PyMuPDF, pdfplumber|
|Deployment|Docker + FastAPI or Streamlit + Render/HF|
|Optional Monitoring|LangSmith, logging JSONL|

### Agent Tools Design

|Tool Name|Description|
|---|---|
|`SearchDocsTool`|Vector DB retrieval using LangChain Retriever|
|`SummarizeTool`|Takes long-form regulation and condenses into bullet points|
|`ExplainTool`|Translates jargon into beginner-friendly text|
|`CitationTool`|Returns exact clause + PDF filename + page number|



# High Level POC 

```
             ┌───────────────────────────────┐
             │         User Interface        │
             │ (Streamlit Web App or CLI)    │
             └────────────┬──────────────────┘
                          │
        ┌─────────────────▼──────────────────┐
        │      FastAPI / Streamlit Backend   │
        │ (User query, auth, formatting, UI) │
        └────────────┬───────────────────────┘
                     │
       ┌─────────────▼──────────────────────────────┐
       │            LangChain Agent (ReAct)         │
       │      [Orchestration & Tool Calling]        │
       └────────────┬─────────────┬─────────────────┘
                    │             │
      ┌─────────────▼──┐     ┌────▼────────────┐
      │  Vector Search │     │ Summarizer Tool │
      │ (FAISS/Chroma) │     │   (LLM Prompt)  │
      └───────────────┘     └──────────────────┘
                    │             │
      ┌─────────────▼─────────────▼────────────┐
      │           Local LLM API / OpenAI       │
      │   (gpt-3.5-turbo or open-source LLM)   │
      └────────────────────────────────────────┘
                    │
      ┌─────────────▼────────────┐
      │ PDF/Doc Ingestion + RAG  │
      │  (Chunking, Embeddings)  │
      │   PyMuPDF + LangChain    │
      └─────────────┬────────────┘
                    │
      ┌─────────────▼────────────┐
      │     Raw Regulatory Docs  │
      │ (RBI, SEBI, FATF - PDFs) │
      └──────────────────────────┘

```


## Tooling & Components Summary

|Layer|Component|Details|
|---|---|---|
|**UI**|Streamlit / CLI|Lightweight frontend|
|**Backend**|FastAPI (optional if not CLI-only)|Expose agent via API|
|**Agent**|LangChain ReAct Agent|Orchestrates the tools|
|**Tools**|Search / Summarize / Cite / Explain|Modular LangChain Tools|
|**RAG DB**|FAISS or Chroma (local)|Store embeddings for PDF chunks|
|**Embedding**|OpenAI `text-embedding-3-small` or HuggingFace `BGE`|Free or low-cost vectorization|
|**LLM**|OpenAI GPT-3.5 / Mistral / LLaMA 3 (optional local)|Responses + generation|
|**Docs**|RBI/SEBI/FATF PDFs|Manually downloaded, chunked|
|**Chunking**|LangChain + PyMuPDF|Clean, smart chunking with metadata|
|**Hosting (optional)**|Hugging Face Spaces (free)|Zero-cost app hosting|
|**Docker**|Local deployment|Package everything for sharing|
