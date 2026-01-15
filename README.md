# 🛒 The Price Is Right — Agentic Deal Discovery System

An autonomous, agent-based system that continuously scans online deal feeds, estimates the true value of products using multiple AI models, and surfaces the **best real discounts** through an interactive UI and push notifications.

This project demonstrates how **agentic AI systems** can collaborate to solve a real-world problem: identifying genuinely underpriced deals in a noisy, high-volume e-commerce environment.

---

## 🎯 Problem Statement

Online deal platforms publish hundreds of offers every day. However:

- Many “deals” are misleading (e.g. *$200 off* without a clear final price)
- True value is hard to estimate without historical or comparable pricing
- Manually checking deals is time-consuming and inefficient
- Users often miss the **best opportunities** due to information overload

While Large Language Models (LLMs) can reason about product descriptions, they are far more effective when **specialised agents collaborate**, each handling a specific responsibility.

---

## 💡 Solution Overview

This project implements an **autonomous multi-agent system** that:

- Continuously scans deal RSS feeds
- Uses LLMs to **select and summarise high-quality deals**
- Estimates the *true value* of each product using **multiple pricing agents**
- Combines estimates via an **ensemble model**
- Identifies deals with the **largest pricing gap**
- Notifies users when a high-confidence opportunity appears
- Visualises activity and agent behaviour via a live UI

The system runs unattended, re-evaluating the market at regular intervals.

---

## 🧠 Agent Architecture

The system is composed of specialised agents, each with a clearly defined role.

### 🟢 Planning Agent (Coordinator)

**Role:** Orchestrator of the entire workflow.

**Responsibilities:**
- Triggers the deal discovery cycle
- Delegates deal scanning to the Scanner Agent
- Sends selected deals to the Ensemble Agent for pricing
- Calculates discounts (estimated value − listed price)
- Decides whether a deal meets the alert threshold
- Triggers notifications via the Messaging Agent

This agent is the “brain” of the system.

---

### 🔍 Scanner Agent (Deal Discovery & Filtering)

**Role:** Find and clean high-quality deals.

**Responsibilities:**
- Fetches deals from multiple RSS feeds (electronics, computers, smart home, etc.)
- Removes deals already seen in previous runs (memory-based filtering)
- Uses an LLM with structured outputs to:
  - Select **exactly 5** high-quality deals
  - Ensure prices are explicit and reliable
  - Rewrite descriptions to focus on the product, not marketing language

This agent prevents low-quality or misleading deals from entering the pipeline.

---

### 🧮 Ensemble Agent (Price Estimation)

**Role:** Estimate the *true market value* of a product.

**Responsibilities:**
- Preprocesses the product description
- Queries multiple pricing agents
- Combines predictions using a weighted average:
  - **80% Frontier Agent**
  - **20% Specialist Agent**
- Returns a single robust price estimate

This agent reduces reliance on any single model.

---

### 🔵 Frontier Agent (RAG-Based Pricing)

**Role:** Context-aware price estimation using retrieval.

**How it works:**
- Embeds the product description using a sentence transformer
- Queries a vector database (Chroma) to find **similar products**
- Retrieves known prices from metadata
- Injects these examples into an LLM prompt
- Extracts a numeric price prediction from the model’s response

This agent grounds price estimates in **real comparable products**.

---

### 🔴 Specialist Agent (Fine-Tuned Model)

**Role:** Domain-specific pricing expertise.

**Responsibilities:**
- Calls a fine-tuned pricing model deployed on **Modal**
- Produces a specialised estimate based on prior training

This agent represents a high-precision expert model.

---

### 📣 Messaging Agent (Notifications)

**Role:** User alerts and notifications.

**Responsibilities:**
- Sends push notifications via **Pushover**
- Formats concise deal alerts including:
  - Listed price
  - Estimated true value
  - Discount amount
  - Deal URL
- Optionally uses an LLM to craft more engaging alert messages

This agent closes the loop between automation and the user.

---

## 🖥 User Interface (`price_is_right.py`)

The system is exposed through a **Gradio-based dashboard**.

### Features
- Live table of detected deal opportunities
- Real-time agent logs with colour-coded identities
- 3D visualisation of the vector database
- Click-to-alert functionality for manual deal selection
- Automatic refresh every 5 minutes

The UI is not just visual — it actively interacts with the agent system.

---

## 🔁 Execution Flow

```text
1. UI starts and initialises the agent framework
2. Planning Agent begins a run
3. Scanner Agent fetches and filters deals
4. Ensemble Agent estimates prices
   ├─ Frontier Agent (RAG + LLM)
   └─ Specialist Agent (fine-tuned model)
5. Planning Agent computes discounts
6. Best deal is selected
7. Messaging Agent sends alert if threshold is met
8. UI updates logs, table, and visualisations
9. System repeats every 5 minutes
```

### ▶️ How to Run  
## 1️⃣ Install dependencies  

```bash
pip install -r requirements.txt
```

## 2️⃣ Set environment variables  

 ```env
OPENAI_API_KEY=your_key_here
PUSHOVER_USER=your_user_key
PUSHOVER_TOKEN=your_app_token
```

## 3️⃣ Launch the application  

```bash
python price_is_right.py
```
The Gradio interface will open automatically in your browser.  

---

## 🧪 Tech Stack
1. Python  
2. OpenAI / Claude / GPT models  
3. SentenceTransformers  
4. ChromaDB  
5. Modal (fine-tuned model hosting)  
6. Gradio  
7. Pydantic  
8. RSS / Web scraping  
9. Agent-based architecture

---

## 🧭 Why This Project Matters

This project demonstrates:

- Real-world agentic AI design  
- Multi-model collaboration  
- Retrieval-augmented reasoning beyond chatbots  
- Autonomous decision-making pipelines  
- Practical use of LLMs for economic reasoning  

It goes beyond “single prompt” AI and showcases **systems thinking**.
