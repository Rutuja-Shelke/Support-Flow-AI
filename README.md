# Support-Flow-AI
# SupportFlow AI — AI-Powered Customer Support Workflow

## Overview

SupportFlow AI is a production-style AI-assisted customer support workflow designed to automate repetitive L1 customer support queries while safely escalating uncertain or sensitive issues to human agents.

Unlike traditional chatbots that rely only on conversational generation, this system focuses on:

* Retrieval-grounded responses (RAG)
* Reliability over unrestricted generation
* Confidence-aware escalation
* Human-AI collaboration
* Observability and workflow transparency

The system ingests company documents such as FAQs, refund policies, onboarding guides, and troubleshooting manuals, retrieves relevant information using vector search, and generates grounded responses using an LLM.

---

# Problem Statement

Businesses often receive a large volume of repetitive support queries related to:

* refunds
* subscriptions
* onboarding
* troubleshooting
* account management

Handling these queries manually:

* increases operational cost
* slows response time
* overloads support teams

This project aims to streamline that process using an AI-assisted workflow that:

1. Answers repetitive L1 queries automatically
2. Grounds responses using company documentation
3. Detects uncertainty or sensitive cases
4. Escalates complex issues to human agents
5. Logs interactions for analytics and improvement

---

# Key Features

## Document Ingestion

* Upload PDFs and text documents
* Automatic chunking and embedding generation
* Vector database indexing using FAISS/ChromaDB

## Retrieval-Augmented Generation (RAG)

* Retrieves relevant context before generation
* Prevents unsupported or hallucinated responses
* Includes source citations in responses

## Query Classification

Classifies incoming queries into categories such as:

* informational
* billing
* technical support
* complaints
* sensitive requests

## Confidence-Aware Response Handling

The system evaluates:

* retrieval quality
* context relevance
* response confidence

Low-confidence or high-risk queries are escalated.

## Human Escalation Workflow

Escalation triggers include:

* low confidence
* unsupported queries
* billing/legal concerns
* frustrated customer behavior
* repeated failed interactions

Escalated cases include:

* conversation summary
* escalation reason
* retrieved context
* query category

## Conversation Memory

* Maintains lightweight conversational context
* Supports multi-turn interactions

## Logging & Observability

Stores:

* user queries
* generated responses
* escalation status
* confidence scores
* feedback

Useful for:

* analytics
* debugging
* workflow optimization
* future retraining

## Feedback Loop

* Helpful / Not Helpful feedback
* Supports iterative improvement

---
This is a full AI customer support workflow system, not just a chatbot. It has four layers:

Chat — answers customer queries
Knowledge base — uploads and indexes company docs
Escalation queue — hands off risky cases to humans
Logs / analytics — stores every interaction and shows performance

Walkthrough of all features
1) Main chat page

This is the customer-facing screen.

What happens when a user asks something:

the query is classified first
greetings and very short inputs are handled separately
off-topic inputs are redirected
on-topic queries go through RAG
the system retrieves relevant document chunks
Gemini generates an answer using only that context
confidence is scored
escalation is triggered if needed
the full interaction is logged

2) Query classification

The classifier checks:

category: General, Technical, Billing, Account, Complaint, Legal, Security, Other
is_sensitive
is_off_topic
is_greeting
is_too_short

It has rule-based shortcuts for:

pure greetings like “hi”
too-short queries like “?”

Then Gemini classifies the rest.

3) Document ingestion page

This is where the knowledge base is built.

It supports:

PDF
TXT
MD

It:

extracts text
splits documents into chunks
embeds chunks locally
saves them into FAISS
persists the index for later use

This is important because the AI only answers from uploaded docs, not general memory.

4) Retrieval pipeline

For normal queries, the system does:

query expansion
retrieval with MMR
multi-query merging
source ranking
context block building

This improves recall and reduces missing answers.

5) Confidence scoring

Confidence is not just one number from the model.

It uses:

retrieval quality
top chunk score
context density
penalty if the model says it is unsure
penalty if the response is too short

That is a strong design choice because it makes escalation more reliable.

6) Escalation logic

The system escalates based on:

low confidence
legal or security category
complaint category
human request
critical keywords
high-risk keywords
medium-risk keywords with low confidence
repeated off-topic behavior

Each escalation gets:

severity: CRITICAL, HIGH, MEDIUM, LOW
priority
SLA deadline
recommended action
7) Escalation queue page

This is the human side of the system.

It lets a reviewer:

see open tickets
filter by severity/status
update status
add agent notes
export the queue
8) Logs page

This is the observability layer.

It shows:

all interactions
filters by escalation / feedback / query type / severity
individual query and response inspection
retrieved context
source citations
confidence breakdown
latency and token usage
performance charts
escalation analysis

This is one of the strongest parts of the project.

9) Conversation memory

The chat keeps a sliding window of recent turns so follow-up questions make sense.

10) Feedback loop

Users can mark answers:

helpful
not helpful

That is logged and visible in analytics.

---

# System Workflow

User Query
→ Query Classification
→ Retrieval Pipeline (RAG)
→ AI Response Generation
→ Confidence Evaluation
→ Escalation (if needed)
→ Logging & Feedback

---
# Architecture

## Components

### Frontend

* Streamlit UI
* Chat-based interaction
* Document upload interface
* Escalation visibility
* Analytics dashboard

### Backend

* Python
* Modular workflow components

### AI Components

* OpenAI/Gemini LLM
* LangChain orchestration
* Embedding models

### Retrieval Layer

* Vector database (FAISS/ChromaDB)
* Semantic similarity search
* Query expansion

### Storage

* SQLite interaction logging
* Vector store persistence

---

# Design Considerations

## Reliability Over Pure Generation

The system prioritizes:

* grounded responses
* traceability
* safe escalation

instead of unrestricted response generation.

## Hallucination Prevention

Mitigation strategies:

* retrieval grounding
* source citations
* confidence thresholds
* escalation fallback

## Human-AI Collaboration

The system is designed to assist support teams rather than replace them entirely.

## Operational Awareness

The workflow considers:

* ambiguous queries
* retrieval failures
* sensitive requests
* escalation routing
* user frustration signals

---

# Tech Stack

## Frontend

* Streamlit

## Backend

* Python

## AI / NLP

* OpenAI API / Gemini API
* LangChain

## Retrieval

* FAISS / ChromaDB

## Database

* SQLite

---

# Example Queries

## Successful Retrieval

"What is your refund policy?"

## Escalation Example

"I was charged incorrectly after upgrading my subscription."

---

# Future Improvements

* WhatsApp integration
* CRM/ticketing integration
* multilingual support
* advanced reranking
* agent memory
* analytics dashboard improvements
* automated ticket prioritization
* role-based support workflows

---

# Setup Instructions

## Clone Repository

```bash
git clone <repository_url>
cd supportflow-ai
```

## Install Dependencies

```bash
pip install -r requirements.txt
```

## Configure Environment Variables

Create a `.env` file:

```env
OPENAI_API_KEY=your_api_key
```

## Run Application

```bash
streamlit run app.py
```

---

# Demo Focus

This project focuses on:

* practical AI workflows
* reliability
* escalation handling
* grounded retrieval
* real-world support system design

rather than purely conversational AI.

---

# Author

Developed for an internship application
