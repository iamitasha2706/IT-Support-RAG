# IT-Support-RAG
Northstar Retail is a fictional, multi-location retailer whose employees need quick answers from internal operations documents, including store procedures, point-of-sale troubleshooting guides, inventory workflows, and employee policies.

Northstar Retail is a fictional, multi-location retailer whose employees need quick answers from internal operations documents, including store procedures, point-of-sale troubleshooting guides, inventory workflows, and employee policies.

AI-generated content may be incorrect.
change it and give me

Retail Operations Knowledge Copilot — Agentic RAG
Business Problem
Northstar Retail is a fictional, multi-location retailer whose employees need quick answers from internal operations documents, including store procedures, point-of-sale troubleshooting guides, inventory workflows, and employee policies.

Employees often spend time searching across documents or asking support teams repetitive questions. This project provides an AI-powered knowledge assistant that searches the company’s private knowledge base first, evaluates whether the retrieved information is relevant, and uses public web search only when internal information is insufficient—for example, to find current guidance from a software vendor.

Solution
The system uses an agentic RAG workflow to route questions, retrieve and evaluate evidence, rewrite unclear queries, and generate answers grounded in available sources. It also includes an API, a web interface, document ingestion, and a decision trace to make the retrieval process easier to inspect.

Example Scenarios
Private knowledge base: “How do I process a return for an online order in store?”
Private knowledge base: “What steps should I follow if the store’s point-of-sale terminal loses network connectivity?”
External information: “Is there a current outage affecting our payment software provider?”
Weak evidence: “The inventory system is behaving strangely. What should I check?”
Resume Bullet
Built a deployable agentic RAG copilot for retail operations using LangGraph, FastAPI, Pinecone, and Groq, with private-document retrieval, evidence grading, query rewriting, web-search fallback, and document ingestion.
AI-generated content may be incorrect.
give that detail flow and all which i gave you for the readme accordingly for this also

Retail Operations Knowledge Copilot
An end-to-end Agentic RAG application that helps retail employees find answers in internal operations documents. The system searches the private knowledge base first, evaluates the retrieved evidence, and uses public web search only when internal information is insufficient.

Built with LangGraph, FastAPI, Pinecone, Groq, Tavily, Hugging Face embeddings, HTML, CSS, and JavaScript.

1. Business Problem
Customer
Northstar Retail is a fictional, multi-location retail company.

Problem
Store employees and operations teams rely on documents covering:

Store procedures and opening or closing checklists
Point-of-sale troubleshooting
Returns and exchange workflows
Inventory receiving and stock checks
Employee policies and escalation procedures
Vendor software guidance
Employees may struggle to find the right document, receive too many irrelevant results from keyword search, or get unreliable answers from a basic chatbot. Some internal documents may also be incomplete or outdated, while certain questions require current public information from a vendor.

Example
An employee asks:

“How do I process an online order return in store?”

The answer should come from Northstar Retail’s private operations knowledge base. The system should not need to search the public internet.

Another employee asks:

“Is there a current outage affecting our payment software provider?”

If the private knowledge base does not contain current outage information, the system should identify the lack of relevant internal evidence, search the web, evaluate the results, and clearly indicate that the answer uses external sources.

Business Goal
Build a retail operations copilot that:

Searches trusted company documents first.
Evaluates whether retrieved evidence is relevant and sufficient.
Uses web search only when private information is insufficient.
Rewrites unclear questions and retries retrieval when appropriate.
Generates answers grounded in retrieved information.
Shows its decision path for transparency and debugging.
Lets authorized staff add company documents.
2. Why This Is an FDE Project
A Forward Deployed Engineer project goes beyond creating an LLM notebook. It translates a customer’s workflow into a usable application.

text

Customer Problem
      ↓
Discovery and Requirements
      ↓
Solution Architecture
      ↓
Data and Knowledge Integration
      ↓
Agentic RAG Development
      ↓
API Development
      ↓
User Interface
      ↓
Security, Audit, and Testing
      ↓
Deployment
      ↓
Observe and Improve
This project demonstrates these layers through a customer-focused application, knowledge ingestion, an agentic retrieval workflow, an API, and a web interface.

3. Architecture
Mermaid Diagram

POST to chat API

Retail Employee

HTML CSS JavaScript Web UI

FastAPI

LangGraph Agentic RAG Workflow

Pinecone Private Knowledge Base

Tavily Web Search

Groq LLM

SQLite Decision Audit

4. Agentic RAG Workflow
The workflow routes questions, searches the private knowledge base, evaluates evidence, and selects the next action based on the result.

text

Question
   ↓
Route Question
   ├── Greeting or simple conversation → Direct Answer
   │
   └── Retail operations question
                ↓
        Retrieve from private Pinecone KB
                ↓
        Grade private evidence
          ┌─────┴─────┐
          │           │
        GOOD         WEAK
          │           │
          ▼           ▼
   Generate from   Tavily web search
   private KB           ↓
                  Grade web evidence
                    ┌───┴───┐
                    │       │
                  GOOD     WEAK
                    │       │
                    ▼       ▼
              Generate    Rewrite query
              web answer       ↓
                         Retry private KB
                               ↓
                       Retry limit reached
                               ↓
                   Return insufficient evidence
Why Agentic RAG?
A basic RAG system typically follows this pattern:

text

Question → Retrieve → Generate
This project adds decision-making steps:

text

Question
 → Route
 → Retrieve
 → Evaluate evidence
 → Choose private KB or web search
 → Rewrite query if needed
 → Retry with a limit
 → Generate a grounded answer
The workflow selects the next action based on the question and the quality of the evidence retrieved.

5. Technology Stack
Layer	Technology	Purpose
Agent workflow	LangGraph	Stateful routing and conditional workflow decisions
LLM	Groq	Question routing, evidence grading, query rewriting, and answer generation
Embeddings	Hugging Face all-MiniLM-L6-v2	Local text embeddings with 384 dimensions
Vector database	Pinecone	Stores and retrieves private company knowledge
External search	Tavily	Searches the web when internal evidence is insufficient
API	FastAPI	Provides chat, health, and document-ingestion endpoints
Frontend	HTML, CSS, JavaScript	Provides the employee-facing web interface
Audit	SQLite	Records basic query and decision-path information
Packaging	Docker	Supports reproducible application deployment
6. Project Structure
text

FDE_Agentic_RAG_Retail_Copilot/
│
├── app/
│   ├── api/
│   │   └── routes.py              # Chat, health, and ingestion APIs
│   │
│   ├── core/
│   │   ├── config.py              # Environment configuration
│   │   └── logging.py             # Logging configuration
│   │
│   ├── rag/
│   │   ├── state.py               # LangGraph state and structured decisions
│   │   ├── vectorstore.py         # Pinecone and embedding integration
│   │   └── workflow.py            # Agentic RAG graph
│   │
│   ├── services/
│   │   ├── audit.py               # SQLite query audit
│   │   └── ingestion.py           # Document loading and chunking
│   │
│   └── main.py                    # FastAPI application
│
├── data/
│   └── sample_kb/
│       ├── store_operations_handbook.md
│       └── retail_support_runbook.md
│
├── static/
│   ├── css/style.css
│   └── js/app.js
│
├── templates/
│   └── index.html
│
├── tests/
│   └── test_ingestion.py
│
├── uploads/
├── .env.example
├── Dockerfile
├── ingest_sample_kb.py
├── requirements.txt
├── run.py
└── README.md
7. Setup
Step 1 — Create a virtual environment
bash

python -m venv venv
Activate it on Windows:

bash

venv\Scripts\activate
Activate it on macOS or Linux:

bash

source venv/bin/activate
Step 2 — Install dependencies
bash

pip install -r requirements.txt
Step 3 — Configure environment variables
Create a .env file in the project root:

env

# LLM
GROQ_API_KEY=your_groq_api_key_here
GROQ_MODEL=openai/gpt-oss-20b

# External search
TAVILY_API_KEY=your_tavily_api_key_here

# Vector database
PINECONE_API_KEY=your_pinecone_api_key_here
PINECONE_INDEX_NAME=retail-operations-rag
PINECONE_NAMESPACE=northstar-retail-kb

# Embeddings
EMBEDDING_MODEL=sentence-transformers/all-MiniLM-L6-v2

# Security
ADMIN_API_KEY=replace-with-a-secure-random-value

# Application settings
APP_ENV=development
The example uses the Hugging Face all-MiniLM-L6-v2 embedding model described in this README. If your implementation uses an OpenAI embedding model instead, update the configuration and Pinecone index dimensions to match the model used by your code.

Environment variable reference
Variable	Description	Required
GROQ_API_KEY	API key for the Groq LLM	Yes
GROQ_MODEL	Groq model identifier	Optional
TAVILY_API_KEY	API key for Tavily web search	Yes
PINECONE_API_KEY	API key for Pinecone	Yes
PINECONE_INDEX_NAME	Pinecone index name	Optional
PINECONE_NAMESPACE	Namespace used for private retail documents	Optional
EMBEDDING_MODEL	Hugging Face embedding model identifier	Optional
ADMIN_API_KEY	Secret key for document-ingestion endpoints	Recommended
APP_ENV	Application environment, such as development or production	Optional
Security notes
Never commit .env or API keys to version control.
Add .env to .gitignore.
Use a unique, secure value for ADMIN_API_KEY.
Restrict credentials to the services that need them.
Use synthetic or public documents in this repository; do not upload confidential company information.
Add production-grade authentication and authorization before exposing admin or ingestion endpoints.
Step 4 — Load sample knowledge-base documents
bash

python ingest_sample_kb.py
The ingestion process follows this pattern:

text

Retail Operations Documents
   ↓
Load Documents
   ↓
Split Text into Chunks
   ↓
Create Hugging Face Embeddings
   ↓
Store Vectors in Pinecone
Step 5 — Run the application
bash

python run.py
Open the web application:

text

http://127.0.0.1:8000
Open the FastAPI API documentation:

text

http://127.0.0.1:8000/docs
8. Demo Scenarios
Demo A — Private knowledge-base answer
Ask:

How do I process an online order return in store?

Expected path:

text

Router → Private KB
Private KB retrieval
Evidence grade → GOOD
Generate answer from private KB
Teaching point: The system prioritizes approved internal retail procedures and does not need to call web search when the private knowledge base contains sufficient evidence.

Demo B — Store operations policy
Ask:

What should I do if a customer’s payment is declined at checkout?

Expected path:

text

Router → Private KB
Private KB retrieval
Evidence grade → GOOD
Generate answer from private KB
Teaching point: RAG lets the system answer questions using company-specific documents that were not part of the model’s training data.

Demo C — Current external information
Ask:

Is there a current outage affecting our payment software provider?

Expected path when the internal knowledge base does not contain current outage information:

text

Router → Private KB
Private KB retrieval
Private evidence grade → WEAK
Tavily web search
Web evidence grade → GOOD
Generate answer with external-source warning
Teaching point: The system can use public information when internal documents do not provide sufficient evidence, while distinguishing external information from company policy.

Demo D — Query rewriting
Ask a deliberately vague question:

The checkout system is acting strangely after the update. What should I do?

If the initial private and web retrieval results are weak, the workflow can rewrite the query and retry retrieval.

Teaching point: The workflow can improve a vague search query and retry while using a retry limit to prevent loops.

Demo E — Direct conversation
Ask:

Hello!

Expected path:

text

Router → Direct answer
Teaching point: Simple conversation does not need to trigger vector retrieval or web search.

9. Document Upload Demo
The interface includes an Add Company Document option for authorized staff.

Supported file types:

text

.pdf
.txt
.md
.docx
The backend processes an uploaded document as follows:

text

Upload
  ↓
Validate File Type
  ↓
Load Document Text
  ↓
Split into Chunks
  ↓
Create Embeddings
  ↓
Store in Pinecone
  ↓
Make Available for Retrieval
This lets authorized staff add or update retail procedures without editing application code.

10. API Endpoints
GET /api/health
Returns the application health status.

POST /api/chat
Example request:

json

{
  "question": "How do I process an online order return in store?"
}
Example response:

json

{
  "answer": "Follow the online order return procedure in the store operations handbook...",
  "source_used": "private_kb",
  "trace": [
    "Router selected private knowledge base",
    "Retrieved relevant retail operations documents",
    "Private evidence grade: GOOD",
    "Generated answer from private knowledge base"
  ],
  "citations": []
}
POST /api/ingest
Uploads a document and adds its chunks to the Pinecone knowledge base. This endpoint is protected by the X-Admin-Key request header.
