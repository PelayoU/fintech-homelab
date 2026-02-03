# Fintech Engineering Home Lab

![](attachments/Pasted%20image%2020260203233715.png)


### Quantitative Finance | DeFi | Sovereign AI Infrastructure

> *A personal research environment bridging the gap between academic theory (Statistics & Quant Finance) and production-grade engineering.*

---

## 👨‍💻 About The Lab
Welcome to my central repository for the **Master in Fintech Engineering**. This monorepo hosts a collection of projects exploring:
* **Algorithmic Trading:** From statistical arbitrage to HFT simulation.
* **Sovereign AI:** Private RAG implementations for financial data analysis.
* **DeFi Protocols:** Smart contract auditing and on-chain analytics.

**Core Philosophy:** Strict data sovereignty, energy efficiency, and modular architecture.

---

## 📂 Project Portfolio

### AI & Orchestration
* **[Fintech-Hybrid-Engine](./Secure-Quant-RAG-Distributed%20Vector%20Logic)**
    * A distributed RAG architecture designed for financial data sovereignty. It utilizes a Semantic Router to intelligently switch between local private AI (Ollama) for sensitive tasks like **Smart Contract Audits** or **M&A Document Analysis**, and cloud models (Gemini) for general reasoning. This ensures proprietary algorithms and client data never leave the on-premise infrastructure.
    * **Stack:** n8n, Qdrant, Ollama, Gemini.
    
    ![](attachments/Pasted%20image%2020260203235127.png)

### 📈 Quantitative Finance (In Progress)
* *(Aquí irán tus futuros proyectos de trading)*
* *Link a carpetas de backtesting, etc.*

### ⛓️ Blockchain & DeFi (In Progress)
* *Smart Contract Audits*
* *Tokenomics Simulations*



---

## 🏗️ Distributed Infrastructure
Unlike standard development setups, this lab operates on a custom **3-Node Hybrid Cluster** to balance compute power and energy efficiency:

| Node | Type | Key Role |
| :--- | :--- | :--- |
| **🟢 Orchestrator** | HP Laptop (Low Power) | Runs **n8n**, Docker Swarm, and Network Routing (Wireguard). |
| **🔴 Compute Node** | Ryzen 5 + RX 7800XT | Dedicated **AI Inference (Ollama)** and Vector Search (**Qdrant**). |
| **🔵 Client** | MacBook M4 | Development, visualization, and manual triggers. |

---


## 🛠️ Technology Stack
* **Languages:** Python (QuantLib, Pandas), Solidity, C++.
* **Infrastructure:** Docker, Portainer, Wireguard.
* **AI/ML:** Ollama, TensorFlow, Qdrant Vector DB.
* **Automation:** n8n Workflow Automation.

---
### 📬 Contact & Research
Created by **[Tu Nombre]**.
*Focusing on the future of decentralized finance.*