# Fintech-Hybrid-Engine-RAG & Adaptive Routing

![](attachments/Pasted%20image%2020260203210435.png)


---

## 🚀 Project Overview

**Fintech-Hybrid-Engine** is a distributed orchestration infrastructure designed for environments requiring **strict data sovereignty** without sacrificing state-of-the-art AI capabilities.

Instead of relying on a "one-size-fits-all" model, this system implements a **Semantic Router**. It evaluates the complexity and sensitivity of every prompt to select the optimal execution path:

1. **Secure Route (On-Premise):** For financial data, personal notes, and proprietary code. Data never leaves the local infrastructure.
    
2. **Cloud Route (High-Reasoning):** For logic puzzles, advanced coding, or general knowledge tasks.



###  Figure 1: The Brain (Semantic Router)

The system analyzes user intent to dynamically route queries between a private local GPU (for sensitive data) and high-performance cloud models (for complex reasoning).

![](attachments/Pasted%20image%2020260203210502.png)



### Figure 2: The Nervous System (Sync Pipeline)

A distributed ETL infrastructure that keeps the Vector Database synchronized with Obsidian notes and technical documentation, featuring automated conflict resolution and energy management.

![](attachments/Pasted%20image%2020260203210408.png)



---



## 💼 Real-World Scenarios

This architecture is not just a personal tool; it addresses critical compliance and IP challenges found in **High-Frequency Trading (HFT)** firms, **Blockchain Audit** agencies, and **Legal** departments.

### 🛡️ Scenario A: The Smart Contract Engineer (Code Security)
*Protecting Intellectual Property and avoiding pre-deployment leaks.*

| Query Type | Routing Decision | Why? |
| :--- | :--- | :--- |
| **"Audit `Vault.sol` for reentrancy bugs."** | 🟢 **Local Route (7800XT)** | Prevents unreleased code/contracts from leaking to third-party model providers. **Zero-Trust Audit.** |
| **"Explain the difference between ERC-721 and ERC-1155."** | ☁️ **Cloud Route (Gemini)** | General documentation questions do not require privacy but benefit from better reasoning. |

### ⚖️ Scenario B: The Legal & Quant Analyst (Data Privacy)
*Handling sensitive financial instruments and client data.*

| Query Type | Routing Decision | Why? |
| :--- | :--- | :--- |
| **"Summarize this M&A NDA PDF."** | 🟢 **Local Route (7800XT)** | Client names and deal terms must never leave the on-premise infrastructure (GDPR/Compliance). |
| **"Draft a polite email declining a meeting."** | ⚡ **Cloud Route (Flash)** | Routine tasks are offloaded to cheap/fast cloud models to save local GPU resources. |
        


---

## 🏗 Distributed Hardware Architecture

The following represents the **Reference Implementation** used in production. While the system is hardware-agnostic, this distributed topology is recommended to balance energy consumption (laptop) vs. inference power (GPU):

|**Node**|**Role**|**Hardware**|**Function**|
|---|---|---|---|
|**Node A**|**Client & Trigger**|MacBook M4|User Interface, Manual Webhook Triggers, Data Source.|
|**Node B**|**Orchestrator**|HP Laptop|Hosting **n8n**, Wireguard, PiHole. Handles business logic and traffic routing.|
|**Node C**|**Compute Unit**|Ryzen 5600 + **RX 7800XT**|Hosting **Ollama** & **Qdrant**. Heavy lifting for Embeddings and Inference.|


---

##  Module 1: The Semantic Router (Inference Engine)

This is the core intelligence of the system. Instead of relying on a single model, the **Orchestrator (n8n)** acts as a gateway that classifies user intent in real-time using a lightweight local LLM.

![](attachments/Pasted%20image%2020260203210502.png)


### How it Works
1.  **Intent Classification:** The prompt enters the `Router` node (running a fast local model like `llama3:8b` or `mistral`).
2.  **Structured Output:** The model forces a JSON response indicating the strict category of the query.
3.  **Branching:** The `Switch` node directs the traffic to one of three specialized backends:

| Mode | Routing Logic | Backend Stack | Use Case |
| :--- | :--- | :--- | :--- |
| **🛡️ LOCAL_HARD** | Query requires context from personal files or strict privacy. | **Ollama + Qdrant (RAG)** | *"Search my Obsidian vault for the 'Black-Scholes' notes."* |
| **⚡ CLOUD_EASY** | Query is generic, simple, or requires up-to-date internet info. | **Gemini Flash/Lite** | *"Write a python regex to validate emails."* |
| **🧠 CLOUD_HARD** | Query requires deep reasoning, complex math, or creative writing. | **Gemini Pro/Ultra** | *"Explain the implications of Quantum Computing on RSA encryption."* |

> **Key Feature:** The decision to route data to the cloud is made **locally**. If the Router classifies the intent as "Sensitive/Local", the data never leaves your LAN.




---


##  Module 2: The Data Pipeline (ETL & Sync)

While the Router handles intelligence, this module handles **Memory**. It is a sophisticated ETL (Extract, Transform, Load) pipeline running on **n8n** that ensures the Vector Database reflects the exact state of the local file system without redundancy or corruption.

### 🛡️ Layer 1: Hardware Orchestration & Safety
Before processing a single file, the system performs a series of "Health Checks" to ensure infrastructure stability:

![](attachments/Pasted%20image%2020260203211810.png)



* **Concurrency Control (Mutex Locking):**
    To prevent database corruption from overlapping triggers (e.g., a manual push happening during a scheduled cron job), the pipeline creates a physical lock file (`/tmp/obsidian.lock`). If a second execution starts while the file exists, it immediately terminates with a `BUSY` status.
* **Energy Management (Smart WoL):**
    The GPU Server (Ryzen 5600 + 7800XT) is power-hungry. The pipeline pings `192.168.1.52`. If unreachable:
    1.  Sends a **Wake-on-LAN** magic packet.
    2.  Enters a **120-second Wait Loop** to allow Ollama/Qdrant services to initialize.
    3.  Re-validates connection before proceeding.
* **Night Guard:**
    Automated schedules are restricted to a **10:00 AM - 12:00 AM** window to prevent the server from waking up and spinning fans at 3 AM, unless a manual "Force Run" webhook is received.


### 🧠 Layer 2: Sync Logic & Identity Management
The system does not blindly upload files. It employs a **"Smart Sync"** strategy to optimize GPU cycles:
![](attachments/Pasted%20image%2020260203211834.png)


* **The "Delta" Decision:**
    A JavaScript normalization node determines the scope of the sync:
    * **Delta Sync:** Default mode. Only processes files modified in the last **24 hours**.
    * **Full Wipe:** Triggered manually or every Sunday at 00:00. It purges the entire Qdrant collection to ensure zero "ghost data".
* **Idempotency via `path_limpio`:**
    Every file is assigned a unique ID based on its clean path. Before inserting new vectors, the pipeline performs a **preventive DELETE** on Qdrant for that specific ID.
    > *Result:* You can rename, move, or edit a note in Obsidian, and the system guarantees no duplicate vectors will exist in the DB.



### 🔧 Layer 3: Ingestion & The "MIME-Type Hack"
Data quality is paramount for RAG. The pipeline splits traffic based on file content to apply specialized processing:

![](attachments/Pasted%20image%2020260203211900.png)




* **Metadata Extraction:**
    The folder structure is parsed into a 3-level hierarchy (Area -> Subject -> File) and injected as JSON metadata. This allows the LLM to filter answers by context (e.g., *“Search only in 'Quantitative Finance'”*).
* **The "Code-as-Text" Bypass:**
    Standard loaders often fail with raw code files (`.java`, `.py`, `.cpp`) due to MIME-type validation errors (e.g., `text/x-java-source`).
    * **Solution:** The pipeline forces a `Text` data format override for these extensions, bypassing the MIME check and treating code as plain text. This ensures proprietary algorithms are indexed correctly without validation errors.
* **Recursive Chunking:**
    Files are not sliced randomly. A language-aware splitter adjusts the strategy:
    * **Markdown:** Splits by headers (`#`, `##`).
    * **Code:** Splits by class/function definitions based on the extension detected in `m_lang`.



---


## 💡 Key Engineering Insights

This project highlights several critical trade-offs faced in modern AI Architecture:

### 1. Cost & Energy Optimization ("Green AI")
Running a dedicated GPU server (7800XT) 24/7 is inefficient for sporadic workloads.
* **Energy:** By implementing the **"Night Guard"** window and **Wake-on-LAN** protocols, the system reduces idle power consumption by approximately **90%** compared to an always-on server.
* **OpEx:** The Semantic Router acts as a financial firewall. By handling 80% of queries (RAG, summarization) locally, the system drastically reduces external API costs (Gemini/OpenAI), reserving budget only for high-value reasoning tasks.

### 2. Consistency over Availability (The CAP Theorem)
In financial data systems, data integrity is non-negotiable.
* The implementation of **Mutex Locking (`.lock`)** deliberately sacrifices immediate availability (rejecting a trigger if the system is busy) to guarantee **Database Consistency**.
* This prevents "Race Conditions" where a Delta Sync and a Wipe operation could corrupt the vector index.

### 3. The "Dark Data" Challenge
Standard ETL tools often fail to process proprietary technical formats.
* The custom **MIME-Type Bypass** implemented in n8n allows the system to ingest raw source code (`.cpp`, `.py`) and academic PDFs seamlessly. This turns a passive file repository into an active, queryable knowledge base for Quantitative Finance.







---
## 📋 Requirements & Tech Stack

To replicate this architecture, you will need a distributed environment (or a single powerful machine) running the following:

### Core Infrastructure
* **Docker & Docker Compose:** To run n8n and Qdrant services.
* **Network:** Static IPs configured for the AI Node and Orchestrator Node (crucial for inter-container communication).
* **Utilities:** `wakeonlan` package installed on the Orchestrator node (for the power management pipeline).

### Software Services
1.  **n8n (Self-Hosted):** The brain of the operation.
2.  **Qdrant:** Running in production mode (port 6333).
3.  **Ollama:** Must be accessible via network (Host binding `0.0.0.0`).

### AI Models (Required Pulls)
You must pull these specific models in your Ollama instance for the pipeline to function:

```bash
# 1. For Vector Embeddings (Required by the ETL pipeline)
ollama pull nomic-embed-text

# 2. For the Router Logic (Required by n8n)
ollama pull llama3  # or 'mistral'
```





