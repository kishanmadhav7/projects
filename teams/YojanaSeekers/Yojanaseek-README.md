\# YojanaSeekers



\*\*Autonomous Civic-Tech Welfare Scheme Assistant\*\*



YojanaSeekers is an autonomous multi-agent civic-tech system designed to help citizens discover government welfare schemes, verify their eligibility, audit required documents, and prepare for applications.



The system transforms a high-level user request into an end-to-end workflow using specialized AI agents coordinated through CrewAI.



For example:



> "Find and help me apply for all eligible education scholarships."



YojanaSeekers analyzes the user's profile, discovers relevant welfare schemes, verifies eligibility using supporting evidence, checks uploaded documents, and generates a personalized application-readiness checklist.



\## Table of Contents



\- \[Problem Statement](#problem-statement)

\- \[Solution](#solution)

\- \[Features](#features)

\- \[Agent Workflow](#agent-workflow)

\- \[Agent Architecture](#agent-architecture)

\- \[Agent Responsibilities](#step-1--user-input)

\- \[RAG \& Eligibility Verification](#step-6--rag-retriever)

\- \[Application Readiness](#step-8--application-readiness-agent)

\- \[Transparency](#transparency)

\- \[Tech Stack](#tech-stack)

\- \[Memory](#memory)

\- \[Tools \& APIs](#tools--rag)

\- \[Setup / How to Run](#setup--how-to-run)

\- \[Expected Output](#example)

\- \[Future Scope](#future-scope)

\- \[Repository](#repository)

\- \[Demo](#demo)



\## Problem Statement



Navigating government welfare schemes is complex.



Citizens often have to search across multiple government portals, understand fragmented eligibility rules, identify applicable schemes, and collect numerous certificates before they can even begin an application.



This creates several problems:



\- Difficulty discovering relevant schemes.

\- Fragmented eligibility criteria.

\- Complex income, category, location, and education requirements.

\- Uncertainty about which documents are required.

\- Manual verification of certificates.

\- Lack of personalized application guidance.



Existing basic chatbots primarily provide static answers rather than completing a multi-step workflow.



As a result, eligible citizens may miss financial, educational, and social benefits.



\## Solution



YojanaSeekers addresses this problem through an autonomous multi-agent architecture.



Instead of simply answering a citizen's question, the system decomposes the request into multiple tasks and assigns them to specialized agents.



The overall pipeline is:



```

USER INPUT

&#x20;   ↓

CREWAI ORCHESTRATOR

&#x20;   ↓

PROFILE ANALYST AGENT

&#x20;   ↓

SCHEME DISCOVERY AGENT

&#x20;   ↓

DOCUMENT AUDITOR AGENT

&#x20;   ↘      ↓      ↙

&#x20;      RAG RETRIEVER

&#x20;          ↓

ELIGIBILITY INVESTIGATOR AGENT

&#x20;          ↓

APPLICATION READINESS AGENT

&#x20;          ↓

FINAL OUTPUT

```



The architecture is based on the uploaded YojanaSeekers design, which defines CrewAI as the orchestrator and separates profile analysis, scheme discovery, document auditing, eligibility investigation, and application readiness into dedicated stages.



\## Features



\### 1. Personalized Welfare Scheme Discovery



YojanaSeekers analyzes user-specific information such as:



\- Age

\- Income

\- Location

\- Education

\- Category



to identify potentially relevant government welfare schemes.



\### 2. Official Scheme Search



The Scheme Discovery Agent searches for relevant welfare schemes and looks for:



\- Eligibility requirements

\- Application deadlines

\- Official scheme information

\- Government application sources



The architecture uses Tavily and government portals as discovery sources.



\### 3. Document Auditing



Users can upload certificates and supporting documents.



The Document Auditor Agent:



\- Checks uploaded certificates.

\- Extracts relevant document information.

\- Determines whether required documentation is available.

\- Helps identify missing documents.



The architecture uses PyMuPDF and Tesseract OCR for document processing.



\### 4. RAG-Based Information Retrieval



A dedicated RAG Retriever provides relevant information to the eligibility investigation stage.



This allows the system to retrieve supporting scheme information and evidence rather than relying only on the language model's general knowledge.



\### 5. Eligibility Investigation



The Eligibility Investigator Agent evaluates the user's information against retrieved scheme requirements.



The result can include:



\- Eligibility status

\- Supporting evidence

\- Relevant scheme requirements

\- Information used to reach the eligibility result



\### 6. Application Readiness



The Application Readiness Agent converts the results into actionable preparation guidance.



The final output can include:



\- Matching schemes

\- Eligibility status

\- Supporting evidence

\- Missing documents

\- Personalized application checklist

\- Official sources and application links



These outputs correspond directly to the architecture specification.



\## Agent Workflow



\### Step 1 — User Input



The user provides a high-level objective in natural language.



Example:



```

Find all government scholarships I am eligible for

and tell me what documents I need to apply.

```



The system sends this objective to the CrewAI orchestrator.



\### Step 2 — CrewAI Orchestrator



The CrewAI Orchestrator manages the multi-agent workflow.



It coordinates:



\- Agent execution

\- Task delegation

\- Information flow

\- Tool usage

\- Results from previous agents



This enables multiple specialized agents to collaborate rather than requiring one agent to perform the entire task.



\### Step 3 — Profile Analyst Agent



The Profile Analyst Agent extracts the key attributes needed for welfare-scheme matching.



\*\*Profile Parameters\*\*



\- Age

\- Income

\- Location

\- Education

\- Category



The agent uses the Groq API / LLM layer for language understanding and structured profile extraction.



\*\*Example\*\*



```json

{

&#x20; "age": 20,

&#x20; "income": 250000,

&#x20; "location": "Maharashtra",

&#x20; "education": "Undergraduate",

&#x20; "category": "OBC"

}

```



\### Step 4 — Scheme Discovery Agent



The Scheme Discovery Agent searches for government welfare schemes matching the user's requirements.



\*\*Responsibilities\*\*



\- Find relevant welfare schemes.

\- Search official government portals.

\- Retrieve eligibility requirements.

\- Identify application deadlines.

\- Collect source information.



\*\*Tools\*\*



\- Tavily

\- Government Portals



The architecture specifically identifies Tavily and government portals for this stage.



\### Step 5 — Document Auditor Agent



The Document Auditor Agent processes documents uploaded by the citizen.



\*\*Responsibilities\*\*



```

Uploaded Certificates

&#x20;       ↓

Document Extraction

&#x20;       ↓

Information Identification

&#x20;       ↓

Document Availability Check

```



The architecture specifies:



\- PyMuPDF for PDF/document processing.

\- Tesseract OCR for extracting text from scanned documents.



\### Step 6 — RAG Retriever



Information gathered from scheme sources and stored knowledge is retrieved through the RAG layer.



```

Scheme Information

&#x20;       ↓

Embedding / Retrieval

&#x20;       ↓

Relevant Context

&#x20;       ↓

Eligibility Investigation

```



The architecture uses PostgreSQL + pgvector for retrieval.



\### Step 7 — Eligibility Investigator Agent



The Eligibility Investigator Agent combines:



\- Structured user profile

\- Retrieved scheme information

\- Eligibility requirements

\- Supporting evidence

\- Relevant document information



It then determines the user's eligibility for each identified scheme.



\*\*Example Output\*\*



```

Scheme: Post-Matric Scholarship



Eligibility: ELIGIBLE



Supporting Evidence:

✓ Income requirement satisfied

✓ Education requirement satisfied

✓ Location requirement satisfied

✓ Category requirement satisfied

```



The purpose of this stage is to provide an eligibility status together with supporting evidence, as specified in the architecture.



\### Step 8 — Application Readiness Agent



Once eligible schemes have been identified, the Application Readiness Agent prepares the citizen for the next step.



It combines:



```

Eligible Schemes

&#x20;      +

Eligibility Evidence

&#x20;      +

Document Audit

&#x20;      ↓

Application Readiness

```



The agent identifies missing documents and creates a personalized application checklist.



\## Final Output



YojanaSeekers produces a consolidated result containing:



\*\*✓ Matching Schemes\*\*

Government welfare schemes relevant to the user's profile.



\*\*✓ Eligibility Status + Supporting Evidence\*\*

An eligibility result supported by retrieved scheme information.



\*\*✓ Missing Documents\*\*

Documents or certificates that still need to be obtained or provided.



\*\*✓ Personalized Application Checklist\*\*

A practical list of actions required before applying.



\*\*✓ Official Sources / Application Links\*\*

Relevant official government sources and application destinations.



These five output categories are defined in the YojanaSeekers architecture.



\## Agent Architecture



```

&#x20;                        ┌─────────────────┐

&#x20;                        │   USER INPUT    │

&#x20;                        └────────┬────────┘

&#x20;                                 │

&#x20;                                 ▼

&#x20;                   ┌──────────────────────────┐

&#x20;                   │   CREWAI ORCHESTRATOR    │

&#x20;                   └────────────┬─────────────┘

&#x20;                                │

&#x20;                                ▼

&#x20;                   ┌──────────────────────────┐

&#x20;                   │ PROFILE ANALYST AGENT    │

&#x20;                   │                          │

&#x20;                   │ Age                      │

&#x20;                   │ Income                   │

&#x20;                   │ Location                 │

&#x20;                   │ Education                │

&#x20;                   │ Category                 │

&#x20;                   └────────────┬─────────────┘

&#x20;                                │

&#x20;                                ▼

&#x20;                   ┌──────────────────────────┐

&#x20;                   │ SCHEME DISCOVERY AGENT   │

&#x20;                   │                          │

&#x20;                   │ Government Schemes       │

&#x20;                   │ Eligibility              │

&#x20;                   │ Deadlines                │

&#x20;                   └────────────┬─────────────┘

&#x20;                                │

&#x20;                                ▼

&#x20;                   ┌──────────────────────────┐

&#x20;                   │ DOCUMENT AUDITOR AGENT   │

&#x20;                   │                          │

&#x20;                   │ Certificates             │

&#x20;                   │ Document Extraction      │

&#x20;                   │ OCR                      │

&#x20;                   └────────────┬─────────────┘

&#x20;                                │

&#x20;                      ┌─────────┴─────────┐

&#x20;                      ▼                   ▼

&#x20;               ┌──────────────┐   ┌──────────────┐

&#x20;               │ RAG RETRIEVER │   │ Document Data│

&#x20;               └───────┬──────┘   └──────┬───────┘

&#x20;                       └────────┬─────────┘

&#x20;                                ▼

&#x20;                   ┌──────────────────────────┐

&#x20;                   │ ELIGIBILITY INVESTIGATOR │

&#x20;                   │          AGENT            │

&#x20;                   └────────────┬─────────────┘

&#x20;                                │

&#x20;                                ▼

&#x20;                   ┌──────────────────────────┐

&#x20;                   │ APPLICATION READINESS    │

&#x20;                   │          AGENT            │

&#x20;                   └────────────┬─────────────┘

&#x20;                                │

&#x20;                                ▼

&#x20;                   ┌──────────────────────────┐

&#x20;                   │      FINAL OUTPUT        │

&#x20;                   │                          │

&#x20;                   │ Matching Schemes         │

&#x20;                   │ Eligibility + Evidence   │

&#x20;                   │ Missing Documents        │

&#x20;                   │ Application Checklist    │

&#x20;                   │ Official Sources         │

&#x20;                   └──────────────────────────┘

```



\### LLM \& Framework



\*\*Groq API + Open-Source LLM\*\*



The LLM layer is powered by the Groq API together with an open-source LLM.



The LLM is primarily responsible for language understanding, profile analysis, task execution, and agent-level reasoning.



\*\*CrewAI\*\*



CrewAI provides the multi-agent orchestration layer.



It enables task-based collaboration between the specialized agents in the YojanaSeekers workflow.



\### Reasoning



YojanaSeekers follows a ReAct-style tool-use approach with task-based agent collaboration.



The agents can:



```

Understand Task

&#x20;    ↓

Plan Action

&#x20;    ↓

Use Appropriate Tool

&#x20;    ↓

Retrieve Information

&#x20;    ↓

Process Result

&#x20;    ↓

Pass Result to Next Agent

```



The architecture describes this as ReAct-style tool use with task-based agent collaboration.



\## Memory



YojanaSeekers uses two levels of memory.



\### Short-Term Memory



Maintains:



\- Current task state

\- Conversation state

\- Intermediate workflow information



This allows agents to maintain context throughout a single workflow.



\### Long-Term Memory



Uses PostgreSQL to store relevant user profile and history information.



```

Short-Term

&#x20;  ↓

Task / Conversation State



Long-Term

&#x20;  ↓

PostgreSQL

&#x20;  ↓

User Profile + History

```



\## Tools \& RAG



The system integrates multiple external tools.



| Tool / Technology | Purpose |

|---|---|

| Tavily | Web/search-based scheme discovery |

| Government Portals | Official welfare scheme information |

| PyMuPDF | PDF/document processing |

| Tesseract OCR | Text extraction from scanned documents |

| PostgreSQL | Persistent storage |

| pgvector | Vector-based retrieval / RAG |

| Groq API | LLM inference |

| CrewAI | Multi-agent orchestration |



These tools and technologies correspond to the architecture's defined Tools \& RAG and LLM/Framework layers.



\## Transparency



A major feature of YojanaSeekers is its real-time UI trace.



The interface can display:



```

Agent Actions

&#x20;    ↓

Tool Calls

&#x20;    ↓

Sources

&#x20;    ↓

Results

&#x20;    ↓

Current Workflow Status

```



This gives users visibility into what the system is doing and which sources are being used.



\### Important Privacy Principle



The UI exposes agent actions, tool calls, sources, and results, rather than exposing private chain-of-thought. This distinction is explicitly included in the architecture.



\### Example UI Trace



```

\[✓] Profile Analyst

&#x20;   Extracted age, income, location and education



\[✓] Scheme Discovery

&#x20;   Found relevant government schemes



\[✓] Document Auditor

&#x20;   Processed uploaded certificates



\[✓] RAG Retriever

&#x20;   Retrieved scheme eligibility information



\[✓] Eligibility Investigator

&#x20;   Evaluated eligibility with supporting evidence



\[✓] Application Readiness

&#x20;   Generated personalized checklist

```



\## Tech Stack



\### Frontend



\- Web-based UI

\- Real-time agent execution trace

\- Scheme results

\- Eligibility results

\- Document status

\- Application checklist



\### Backend



\- Python-based agent backend

\- API layer

\- Agent workflow execution

\- Document processing

\- Database integration



\### AI / LLM



\- Groq API

\- Open-source LLM

\- ReAct-style tool usage



\### Agent Framework



\- CrewAI

\- Task-based multi-agent collaboration



\### Retrieval



\- RAG

\- PostgreSQL

\- pgvector



\### Search / External Data



\- Tavily

\- Government portals



\### Document Processing



\- PyMuPDF

\- Tesseract OCR



The architecture identifies these technologies as the core stack for LLM/framework, reasoning, memory, tools, and RAG.



\## Setup / How to Run



\### 1. Clone the Repository



```bash

git clone <repository-url>

cd YojanaSeekers

```



\### 2. Create a Virtual Environment



```bash

python -m venv venv

```



\*\*Windows\*\*



```bash

venv\\Scripts\\activate

```



\*\*Linux / macOS\*\*



```bash

source venv/bin/activate

```



\### 3. Install Dependencies



```bash

pip install -r requirements.txt

```



\### 4. Configure Environment Variables



Create a `.env` file containing the required configuration:



```

GROQ\_API\_KEY=<your-groq-api-key>

TAVILY\_API\_KEY=<your-tavily-api-key>

DATABASE\_URL=<your-postgresql-url>

```



Add any additional configuration required by the implementation.



\### 5. Configure PostgreSQL



Ensure PostgreSQL is running and that the required database and pgvector extension are configured.



\### 6. Run the Backend



```bash

uvicorn main:app --reload

```



\### 7. Run the Frontend



```bash

npm install

npm run dev

```



Then open the local frontend URL provided by the development server.



> \*\*Note:\*\* The exact commands and environment variables may need to be adjusted according to the final repository implementation.



\## Example



\### User Input



```

I am a 20-year-old undergraduate student from Maharashtra.

My family's annual income is ₹2.5 lakh.

Find all scholarships and welfare schemes I may be eligible for

and tell me what documents I need.

```



\### Agent Execution



```

User Input

&#x20;   ↓

Profile Analyst

&#x20;   ↓

Structured Profile

&#x20;   ↓

Scheme Discovery

&#x20;   ↓

Government Scheme Results

&#x20;   ↓

Document Auditor

&#x20;   ↓

RAG Retriever

&#x20;   ↓

Eligibility Investigator

&#x20;   ↓

Application Readiness

```



\### Final Result



```

MATCHING SCHEMES

\----------------

Scheme A

Scheme B

Scheme C



ELIGIBILITY

\-----------

Scheme A: Eligible

Scheme B: Eligible

Scheme C: Needs additional information



MISSING DOCUMENTS

\-----------------

Income Certificate

Domicile Certificate



APPLICATION CHECKLIST

\---------------------

1\. Obtain income certificate

2\. Verify domicile certificate

3\. Prepare educational documents

4\. Review official application requirements

5\. Apply through the official portal

```



\## Key Advantages



\*\*Autonomous\*\*

The system performs multiple connected tasks instead of stopping after answering the initial question.



\*\*Personalized\*\*

Scheme discovery and eligibility investigation are based on individual user information.



\*\*Multi-Agent\*\*

Specialized agents divide the workflow into manageable responsibilities.



\*\*Evidence-Oriented\*\*

Eligibility results are accompanied by supporting information retrieved through the system.



\*\*Document-Aware\*\*

The system considers the citizen's actual uploaded documents when preparing for an application.



\*\*Transparent\*\*

Users can see real-time agent actions, tool calls, sources, and results without exposing private chain-of-thought.



\## Future Scope



Potential extensions include:



\- More government scheme and portal integrations.

\- Support for additional welfare categories.

\- Multilingual citizen interaction.

\- Voice-based welfare assistance.

\- Improved document verification.

\- Automated deadline reminders.

\- Application form assistance.

\- Additional retrieval sources.

\- Expanded long-term personalization.

\- Integration with additional official government services.



\## Repository



\*\[Optional]\*



`<GitHub Repository URL>`



\## Demo



\*\[Optional]\*



`<Live Demo URL>`



\## Disclaimer



YojanaSeekers is an assistance system for discovering welfare schemes and preparing for applications.



Eligibility decisions, document acceptance, and final application approval remain the responsibility of the respective government authority. Users should verify important eligibility requirements, deadlines, and application instructions using the relevant official government sources before submitting an application.



\---



\*\*YojanaSeekers\*\*

\*Autonomous Civic-Tech Welfare Scheme Assistant\*



Discover schemes. Verify eligibility. Prepare documents. Apply with confidence.



