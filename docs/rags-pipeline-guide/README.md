# Building a RAG pipeline with Python, ChromaDB and Google Gemini 

## Introduction
Today, we all know how powerful Large Language Models (LLMs) are, but they have an underlying limitation: they can only answer questions based on the data they were trained on. When you ask an LLM about your internal product documentation, a private knowledge database, or anything that they weren't trained on before, either it will refuse to answer, or, worse, make something up on its own.
Here Retrieval-Augmented Generation (RAG) comes into the picture.
RAG works by connecting an LLM to an external knowledge base. Instead of relying solely on training data, the system first retrieves relevant documents from your knowledge base, then passes those documents to the LLM as context. The LLM generates an answer grounded in your actual data — not guesswork.
In this guide, you will build a complete RAG pipeline from scratch using:
- **Python** as the primary language
- **ChromaDB** as the vector database
- **Google Gemini** as the embedding model and LLM
- **LangChain** as the orchestration framework

By the end of this guide, you will have a working question-answering system that can accurately answer questions from any knowledge base you provide.

## Prerequisites
Before you begin, ensure you have the following:
- **Python 3.8 or higher** installed on your machine
- A **Google AI Studio Account** with Gemini API key ([Get your free API key here](https://aistudio.google.com))
- **Google Colab** or a local Python environment 
-  Fundamental understanding of Python and REST APIs
> **Note:** This guide uses Google Colab for all code examples. 
> If you're running locally, the code is identical - just install the packages in your terminal instead.

## Installation
Install the required packages using pip in your local Python environment or Google Colab:
```python
!pip install langchain langchain-community langchain-core langchain_google_genai chromadb -q
```

| Package | Purpose |
|--|--|
| `langchain` | Core orchestration framework |
|  `langchain-community` | Community integrations including ChromaDB |
| `langchain-core` | Core abstractions and interfaces |
| `langchain-google-genai` | Google Gemini integration for LangChain |
| `chromadb` | Vector database for storing embeddings |
> **Note:** The -q flag runs pip in quiet mode, suppressing verbose installation output. 

## Setting Up Your API Key 
Before running any code, configure your Gemini API key as an environment variable:
```python
import os
os.environ["GOOGLE_API_KEY"] = "your-api-key-here"
```
> ⚠️ **Security Warning:** It is not a recommended practice to hardcode your API key directly in your code or commit it to a public GitHub repository for security reasons. Always use environment variables or a secrets manager to handle your API keys safely.
If you are working on Google Colab, you can use Google Colab's built-in secret manager. Click the 🔑 **key icon** in the left sidebar, add your key as `GOOGLE_API_KEY`, and access it in your code like this:
```python
from google.colab import userdata
os.environ["GOOGLE_API_KEY"] = userdata.get("GOOGLE_API_KEY")
```

## Understanding the Architecture
Before diving straight to the code, it is important to understand how the different components interact in the system. 
A RAG pipeline has two clearly defined phases:

**Phase 1 - Indexing (runs once)**
1. Load your documents
2. Break your documents into smaller chunks
3. Represent each chunk as a vector embedding
4. Store embeddings in ChromaDB

**Phase 2 - Retrieval and Generation (runs on every query)**

1. Convert the user's query into a vector embedding
2. Search for the most similar document chunk in ChromaDB
3. Pass retrieves chunks as context to Gemini
4. Finally, Gemini generates a grounded response

Here is how the data flows through the system:
```
 User Question   
    ↓   
 Embedding Model (Gemini)      
    ↓   
 Vector Search (ChromaDB)      
    ↓   
 Retrieved Documents + Original Question 
    ↓   
 LLM (Gemma) → Final Answer
  ```


## Building the Knowledge Base
First, import the required libraries and initialize your embedding model:
```python
from langchain_google_genai import GoogleGenerativeAIEmbeddings
from langchain_community.vectorstores import Chroma
from langchain_core.documents import Document

# Initialize the embedding model 
embeddings = GoogleGenerativeAIEmbeddings(model="models/gemini-embedding-001")
```
Next, define your documents. In this guide, we use a fictional API called DataSync to keep things self-contained. In a real-world application, these documents might be extracted from PDFs, databases, web pages, or any text source.
```python
documents = [
    "DataSync API allows developers to synchronize data between multiple     databases in real-time. It supports PostgreSQL, MongoDB, and MySQL.",       
    "Authentication in DataSync API uses API keys. Generate one from your     dashboard under Settings > API Keys.",       
    "Rate limits are 100 requests/minute for Free tier and 1000     requests/minute for Pro tier.",        
    # Add more documents as needed
]
```
Now convert the documents into LangChain Document objects and store  them in ChromaDB:
```python
# Convert strings to Documents objects
docs = [Document(page_content=text) for text in documents]

# Create vector store - this embeds and indexes all documents
vectorstore = Chroma.from_documents(
   documents=docs,
   embedding=embeddings,
   collection_name="your_collection_name"
)
print(f"Successfully indexed {len(docs)} documents")
```
When `Chroma.from_documents()` runs, each document is converted into a high-dimensional vector and stored in ChromaDB's in-memory index. This is a one-time process but needs to be run every time the knowledge base changes. 


## Setting up the Retriever and LLM 
With the knowledge base ready, initialize the retriever and language model:
```python
from langchain_google_genai import ChatGoogleGenerativeAI

# Create retriever from the vector store
retriever = vectorstore.as_retriever(search_kwargs={"k":3})

# Initialize the LLM
llm = ChatGoogleGenerativeAI(
   model="gemma-3-1b-it",
   temperature=0.3
)
```
**Getting an understanding of used parameters**
`search_kwargs={"k": 3}` is used to instruct the retriever to retrieve the top 3 most semantically similar documents for each query. You can set this 
number higher for broader context, but keep in mind that the more documents you retrieve, the more tokens you are  sending to the LLM, which impacts both cost and response time.

`temperature=0.3` is used to control how creative or deterministic the model's 
responses are. The temperature range is 0 to 1, where 0 is completely deterministic and 1 is highly creative. When you're documenting or answering factual questions, a low temperature setting of 0.3 is ideal to ensure the output is accurate and consistent.

## Building the RAG Chain

Now we integrate the retriever and LLM into a complete pipeline using LangChain's modern chain syntax:

```python
from langchain_core.prompts import PromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_core.output_parsers import StrOutputParser

# Define the prompt template
prompt_template = """You are a helpful assistant for DataSync API documentation.
Use the following context to answer the question accurately.
If the answer is not in the context, say "I don't have information about that."

Context:
{context}

Question: {question}

Answer:"""

prompt = PromptTemplate(
    template=prompt_template,
    input_variables=["context", "question"]
)

# Helper function to format retrieved documents
def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

# Build the RAG chain
rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)
```
**How the chain works:**

The `|` operator chains operations together. When a question comes in:

1. The retriever fetches relevant documents
2. `format_docs()` converts them into plain text
3. Both context and question are injected into the prompt template
4. The filled prompt goes to the LLM
5. `StrOutputParser()` extracts the clean text response

This structure allows each component to be kept modular and testable.

## Testing the Pipeline 
With the RAG chain constructed, you can now ask questions:

```python
def ask(question):
    response = rag_chain.invoke(question)
    print(f"Q: {question}")
    print(f"A: {response}\n")

# Test with questions from the knowledge base
ask("How do I authenticate with DataSync API?")
ask("What are the rate limits for the free tier?")
ask("How much does the Pro plan cost?")

# Test with a question NOT in the knowledge base
ask("Does DataSync support Firebase?")
```

**Expected behavior:**

The first three questions should return accurate answers directly retrieved 
from your documents. The last question about Firebase — which we never mentioned in our knowledge base - should return something like  "I don't have information about that."

This behavior is called **staying grounded**. Unlike a standard LLM that could hallucinate an answer, a well constructed RAG system only responds with information it can retrieve from the knowledge base.

**Example output:**

```
Q: How do I authenticate with DataSync API?
A: You can authenticate with DataSync API using API keys. Generate one 
from your dashboard under Settings > API Keys.

Q: Does DataSync support Firebase?
A: I don't have information about that.
```

## Next Steps and Best Practices
You now have a functional RAG pipeline. Here are some ways to optimize and extend it for production use:

### Loading Real Documents
Instead of using hardcoded strings, load documents from real sources:
```python
from langchain_community.document_loaders import PyPDFLoader, TextLoader
# Load from PDF
loader = PyPDFLoader("your_document.pdf")
documents = loader.load()
# Load from text 
fileloader = TextLoader("knowledge_base.txt")
documents = loader.load()
```

### Text Splitting for Large Documents
Large documents should be split into smaller chunks to improve retrieve accuracy:
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter
text_splitter = RecursiveCharacterTextSplitter(
  chunk_size=1000,
  chunk_overlap=200
)
split_docs = text_splitter.split_documents(documents)
```

### Persisting the Vector Store
ChromaDB will store data in memory by default. In production, store it on disk instead:
```python
vectorstore = Chroma.from_documents(
   documents=docs,
   embedding=embeddings,
   persist_directory="./chroma_db"
)
```

### Monitoring and Evaluation
Monitor which documents are being retrieved and whether the answers are accurate. Consider logging retrieval scores and gathering user feedback.

### Alternative Models
This tutorial uses Gemma, but you can easily substitute in another model:
```python
# Use a different Gemini model
llm = ChatGoogleGenerativeAI(model="gemini-2.0-flash-lite")
# Or switch to OpenAI
from langchain_openai import ChatOpenAI
llm = ChatOpenAI(model="gpt-4")
```

## Conclusion
You have now created a Retrieval-Augmented Generation pipeline from scratch. Your model is now capable of answering questions correctly based on any knowledge base you choose to supply, without hallucinating any information.

**What you learned:**
- What RAG is and how it can be useful
- How to convert documents into vector embeddings
- How to store and search embeddings using ChromaDB
- How to link a retriever to an LLM model using LangChain
- How to ensure your answers are grounded in source documents

RAG is quickly becoming the standard architecture for any AI-powered knowledge system. Whether you are building customer support chatbots, in-house documentation assistants, or research tools, the concepts in this guide will be applicable.

**Questions or Feedback?** Feel free to reach out:
- GitHub: [Shruti-Rajpurohit](https://github.com/Shruti-Rajpurohit)
- LinkedIn: [Shruti Rajpurohit](https://www.linkedin.com/in/shruti-rajpurohit-976b44226)
- Email: rajpurohitshruti20@gmail.com