#  End-to-End Agentic AI

An **End-to-End Agentic AI application** built with **LangGraph, LangChain, Groq, Tavily, and Streamlit**.

This project demonstrates how to build practical AI agents using **graph-based workflows**, tool calling, web search, and an interactive Streamlit interface.

The application currently supports multiple agentic use cases including:

*  Basic Chatbot
*  Chatbot with Tools
*  AI News Explorer

---

##  Features

###  1. Basic Chatbot

A simple conversational AI workflow powered by an LLM.

The request flows through a LangGraph workflow:

```text
User Input
    ↓
Chatbot Node
    ↓
LLM Response
    ↓
Final Output
```

---

###  2. Chatbot with Tools

The tool-enabled chatbot can decide when it needs external information and invoke tools through LangGraph.

Currently, the project integrates **Tavily Search** as a web-search tool.

```text
User Query
    ↓
Chatbot Node
    ↓
Tool Decision
    ↓
Tavily Search
    ↓
Chatbot
    ↓
Final Response
```

The project uses LangGraph's conditional routing and tool nodes to implement this workflow.

---

###  3. AI News Explorer

The AI News workflow retrieves recent AI-related news using Tavily and then summarizes the results using the configured LLM.

The workflow is:

```text
Fetch AI News
      ↓
Summarize News
      ↓
Save Markdown File
```

The application supports:

* Daily AI News
* Weekly AI News
* Monthly AI News

The generated summaries are saved under the `AINews/` directory.

---

##  Architecture

The project follows a modular architecture built around **LangGraph**.

```text
                    ┌──────────────────────┐
                    │    Streamlit UI      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      LLM Layer       │
                    │        Groq           │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │     GraphBuilder      │
                    │      LangGraph        │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
       Basic Chatbot     Chatbot + Tools    AI News
              │                │                │
              │                ▼                ▼
              │          Tavily Search     Fetch News
              │                │                │
              │                ▼                ▼
              │           Tool Node       Summarization
              │                                 │
              └─────────────────────────────────┤
                                                ▼
                                           Final Output
```

The `GraphBuilder` currently defines separate LangGraph workflows for **Basic Chatbot**, **Chatbot with Tool**, and **AI News**.

---

## 🏗️ Project Structure

```text
End-to-End-Agentic-AI/
│
├── app.py
├── requirements.txt
├── README.md
├── .gitignore
│
└── src/
    └── langgraphagenticai/
        │
        ├── LLMS/
        │   └── groqllm.py
        │
        ├── graph/
        │   └── graph_builder.py
        │
        ├── nodes/
        │   ├── basic_chatbot_node.py
        │   ├── chatbot_with_Tool_node.py
        │   └── ai_news_node.py
        │
        ├── state/
        │   └── state.py
        │
        ├── tools/
        │   └── serach_tool.py
        │
        ├── ui/
        │   ├── streamlitui/
        │   │   ├── loadui.py
        │   │   └── display_result.py
        │   │
        │   └── uiconfigfile.py
        │
        └── vectorstore/
```

The repository currently contains the `LLMS`, `graph`, `nodes`, `state`, `tools`, `ui`, and `vectorstore` modules under the main application package.

---

## Tech Stack

| Technology   | Purpose                          |
| ------------ | -------------------------------- |
|  Python    | Core programming language        |
|  LangChain | LLM and AI application framework |
|  LangGraph | Agent workflow orchestration     |
|  Groq       | LLM inference                    |
|  Tavily    | Web search and AI news retrieval |
|  Streamlit | Interactive web interface        |
|  FAISS     | Vector store support             |

These dependencies are listed in the project's `requirements.txt`.

---

##  Prerequisites

Before running the project, make sure you have:

* Python 3.9+
* Git
* A Groq API Key
* A Tavily API Key

You can obtain the required API keys from:

* [Groq Console](https://console.groq.com/keys)
* [Tavily](https://app.tavily.com/home)

---

##  Installation

### 1. Clone the repository

```bash
git clone https://github.com/khatana706/End-to-End-Agentic-AI.git
```

### 2. Navigate to the project

```bash
cd End-to-End-Agentic-AI
```

### 3. Create a virtual environment

#### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

#### macOS / Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

### 4. Install dependencies

```bash
pip install -r requirements.txt
```

---

##  API Keys

The Streamlit interface accepts the required API keys through the UI.

For the Groq-powered LLM, provide:

```text
GROQ API KEY
```

For the tool-enabled chatbot and AI News workflows, provide:

```text
TAVILY API KEY
```

The current Streamlit UI exposes Groq model selection and API-key input, and requests a Tavily key for the tool-enabled and AI News use cases.

---

##  Run the Application

Start the Streamlit application with:

```bash
streamlit run app.py
```

Then open the local Streamlit URL shown in your terminal, typically:

```text
http://localhost:8501
```

The project's `app.py` loads the main LangGraph Agentic AI application through `load_langgraph_agenticai_app()`.

---

##  How to Use

After launching the application:

### Step 1 — Select the LLM

Choose the available LLM configuration from the sidebar.

### Step 2 — Select the Use Case

Choose one of:

```text
Basic Chatbot
Chatbot with Tool
AI News
```

### Step 3 — Provide API Keys

Enter the required Groq and/or Tavily API key.

### Step 4 — Interact with the Agent

Enter your query in the chat interface.

For **AI News**, select:

```text
Daily
Weekly
Monthly
```

and click:

```text
 Fetch Latest AI News
```

The AI News workflow retrieves news, summarizes it, and writes the result to a Markdown file.

---

##  Agentic Workflow

One of the main goals of this project is to demonstrate how traditional LLM applications can be converted into **agentic workflows**.

Instead of simply sending a prompt to an LLM, LangGraph is used to control the flow between different nodes.

### Basic Chatbot

```text
START
  ↓
Chatbot
  ↓
END
```

### Tool-Enabled Chatbot

```text
START
  ↓
Chatbot
  ↓
Does the agent need a tool?
  │
  ├── No ──→ END
  │
  └── Yes
        ↓
     Tool Node
        ↓
     Chatbot
```

### AI News Agent

```text
START
  ↓
Fetch News
  ↓
Summarize News
  ↓
Save Result
  ↓
END
```

These workflows are implemented through the project's `GraphBuilder` class.

---

## AI News Output

When the AI News workflow runs, the generated summary is stored in:

```text
AINews/
```

Example:

```text
AINews/
├── daily_summary.md
├── weekly_summary.md
└── monthly_summary.md
```

The news agent retrieves AI-related news and creates a Markdown summary containing dates, summaries and source URLs.

---

## Tool Calling

The tool-enabled chatbot currently uses **Tavily Search**.

Example workflow:

```text
User:
"What are the latest developments in AI?"

        ↓

LangGraph Chatbot

        ↓

Tool Required?

        ↓

Tavily Search

        ↓

Search Results

        ↓

LLM

        ↓

Final Answer
```

The Tavily search tool is configured as a LangGraph tool node and can return up to two search results per invocation in the current implementation.

---

## LangGraph

LangGraph is useful for building agentic applications where the application needs:

* Stateful workflows
* Multiple processing nodes
* Conditional routing
* Tool calling
* Human-in-the-loop extensions
* Multi-step reasoning workflows
* Reusable agent components

This project uses `StateGraph`, conditional tool routing and dedicated nodes to demonstrate these concepts.


##  Contributing

Contributions are welcome!

If you would like to improve this project:

1. Fork the repository
2. Create a new branch

```bash
git checkout -b feature/your-feature
```

3. Make your changes
4. Commit your changes

```bash
git add .
git commit -m "Add your feature"
```

5. Push your branch

```bash
git push origin feature/your-feature
```

6. Open a Pull Request

---

##  Disclaimer

API usage may incur costs depending on the provider and your account configuration.

Always keep API keys and other sensitive credentials private.

---

##  Support

If you find this project useful, consider giving the repository a  on GitHub.

Your feedback, suggestions, and contributions are always welcome.

---

##  Author

**NARENDRA SINGH**

GitHub:
https://github.com/khatana706

Project:
https://github.com/khatana706/End-to-End-Agentic-AI

---
