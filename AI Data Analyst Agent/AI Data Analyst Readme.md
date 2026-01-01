# 📊 AI Data Analyst Agent: Talk to Your Data

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![AI](https://img.shields.io/badge/GenAI-Google%20Gemini%202.5-purple)
![Technique](https://img.shields.io/badge/Agent-LangGraph%20%26%20Pandas-orange)
![Domain](https://img.shields.io/badge/Data-Automated%20Analysis-red)

## 📌 Project Overview
**AI Data Analyst** is an intelligent conversational agent that acts as a virtual data scientist. It empowers users to interact with their datasets using natural language, automating the complex process of writing code for data analysis and visualization.

Orchestrated by **LangGraph**, the system utilizes a **Stateful Graph Architecture** to plan, execute, and self-correct data workflows. It leverages **Google Gemini 2.5** to reason about data schemas, generate accurate **Pandas** queries, and render **Seaborn/Matplotlib** charts instantly.

---

## 🎯 Objectives
- **Democratize Data Analysis:** Allow non-technical users to query complex datasets using plain English.
- **Automate Visualization:** Automatically select and generate the most appropriate charts (Bar, Line, Scatter, Heatmap) for the data.
- **Self-Correcting Code:** Use a cyclic graph to detect execution errors (e.g., syntax errors) and autonomously fix the code before showing the result.
- **Stateful Reasoning:** Maintain context across the analysis pipeline, understanding the difference between raw data ingestion and analytical output.

---

## 📁 Data & Workflow

This project is agnostic to the specific dataset but is optimized for structured tabular data (CSV, Excel).

### Analysis Pipeline
The agent follows a "Think -> Code -> Execute -> Observe" loop:
1.  **Ingest:** Load the DataFrame and extract schema (column names, types, sample values).
2.  **Plan:** The LLM breaks down the user's question into logical Pandas operations.
3.  **Generate:** Python code is generated dynamically.
4.  **Execute:** The code runs in a safe local environment.
5.  **Visualize:** If the user asks for a plot, the agent generates the Matplotlib code and renders the image.

---

## 🏗️ System Architecture

The project follows a **Cyclic State Graph** where the agent can loop back to correct itself if code execution fails.

```mermaid
flowchart TD
    %% --- TITLE NODE (Workaround for Big Bold Text) ---
    %% We use a node with invisible background/border to act as a Title
    GraphTitle["AI DATA ANALYST AGENT"]:::titleStyle
    GraphTitle --> Start

    %% --- STYLE DEFINITIONS ---
    classDef titleStyle fill:none,stroke:none,font-size:28px,font-weight:bold,color:black;
    classDef user fill:#FFD700,stroke:#333,stroke-width:2px,color:black;
    classDef cleaner fill:#E1F5FE,stroke:#0277BD,stroke-width:2px,color:black;
    classDef context fill:#E8F5E9,stroke:#2E7D32,stroke-width:2px,color:black;
    classDef exec fill:#FFF3E0,stroke:#EF6C00,stroke-width:2px,color:black;
    classDef output fill:#F3E5F5,stroke:#7B1FA2,stroke-width:2px,color:black;
    
    %% AGENT STYLE (Highlighted Hexagon)
    classDef agent fill:#FF5722,stroke:#BF360C,stroke-width:4px,color:white,font-weight:bold,rx:10,ry:10;

    %% --- USER INPUT ---
    UserInput[/"User Input:<br/>1. File (CSV/Excel)<br/>2. Analysis Query"/]:::user
    Start((Start)) --> UserInput
    
    %% --- NODE 1: CLEANER ---
    subgraph NODE_1 [Node 1: Smart Data Cleaner]
        direction TB
        UserInput --> LoadData[Load Data into Pandas]:::cleaner
        LoadData --> CheckNulls{"Has Missing<br/>Values?"}:::cleaner
        CheckNulls -- No --> CleanedDF[Return Clean DF]:::cleaner
        CheckNulls -- Yes --> ColLoop{"Iterate<br/>Columns"}:::cleaner
        
        %% Numeric Logic
        ColLoop -- Numeric --> Shapiro{"Check Normality<br/>(Shapiro-Wilk)"}:::cleaner
        Shapiro -- "p > 0.05 (Normal)" --> FillMean["Fill NaNs<br/>with MEAN"]:::cleaner
        Shapiro -- "p <= 0.05 (Skewed)" --> FillMedian["Fill NaNs<br/>with MEDIAN"]:::cleaner
        
        %% Categorical Logic
        ColLoop -- Categorical --> CardCheck{"Cardinality<br/>Check"}:::cleaner
        CardCheck -- Low Cardinality --> FillMode["Fill NaNs<br/>with MODE"]:::cleaner
        
        FillMean & FillMedian & FillMode --> DropRem["Drop Remaining<br/>Dirty Rows"]:::cleaner
        DropRem --> CleanedDF
    end

    %% --- NODE 2: SELECTOR ---
    subgraph NODE_2 [Node 2: AI Analyst Selector]
        direction TB
        CleanedDF --> ContextGen[Context Generation]:::context
        
        %% Context details
        ContextGen -- Inject --> Schema["DF Schema:<br/>Col Names & Types"]:::context
        ContextGen -- Inject --> ToolsDef["Tool Definitions:<br/>Arguments & Descriptions"]:::context
        ContextGen -- Inject --> UQ[User Query]:::context
        
        %% LLM 1: SELECTOR AGENT (Hexagon)
        UQ & Schema & ToolsDef --> LLM_Selector{{ fa:fa-robot LLM: Selector Agent }}:::agent
        
        LLM_Selector --> JSONParse[JSON Output Parser]:::context
        JSONParse --> AI_Plan["Output:<br/>Selected Tool + Column Args"]:::context
    end

    %% --- NODE 3: EXECUTOR ---
    subgraph NODE_3 [Node 3: Execution Engine]
        direction TB
        
        %% LLM 2: EXECUTION AGENT (Hexagon)
        AI_Plan --> LLM_Exec{{ fa:fa-robot LLM: Execution Agent }}:::agent
        
        LLM_Exec --> ToolMap{"Map String<br/>to Function"}:::exec
        
        ToolMap -- plot_correlation_heatmap --> Func1[Execute: sns.heatmap]:::exec
        ToolMap -- plot_distribution --> Func2[Execute: sns.histplot]:::exec
        ToolMap -- plot_bar_chart --> Func3[Execute: sns.countplot]:::exec
        ToolMap -- plot_boxplot --> Func4[Execute: sns.boxplot]:::exec
        ToolMap -- plot_scatter --> Func5[Execute: sns.scatterplot]:::exec
        
        Func1 & Func2 & Func3 & Func4 & Func5 --> Render[Render Plot]:::exec
    end

    %% --- END ---
    Render --> FinalOutput[/"Display Visualization<br/>to User"/]:::output
    FinalOutput --> End((End))

    %% Edge Connections between Subgraphs
    CleanedDF -.-> |"Pass State: df"| ContextGen
    AI_Plan -.-> |"Pass State: tool_name, args"| LLM_Exec
    
    %% Hide the link between Title and Start to make it look like a Header
    linkStyle 0 stroke-width:0px,fill:none;
```

## 🔑 Steps Followed
1.  **Environment Setup** – Configured `dotenv` for API keys and initialized the Google Gemini model.
2.  **DataFrame Loading** – Implemented Pandas logic to read tabular data and infer data types.
3.  **Agent Initialization** – Built a **LangGraph StateGraph** to manage the conversation history and current scratchpad (code drafts).
4.  **Prompt Engineering** – Designed a system prompt that forces the LLM to output only executable Python code, strictly adhering to the column names provided.
5.  **Execution Sandbox** – Created a `PythonREPL` tool node that runs the generated code and captures `stdout` or `stderr`.
6.  **Error Handling** – Implemented a feedback loop: if `stderr` is detected, it is fed back to the LLM as a new prompt ("You got this error, fix it.").

---

## 📊 Example Queries

**Descriptive Statistics:**
> *"What is the average age of customers in the dataset?"*
> *"Show me the summary statistics for the 'Revenue' column."*

**Complex Filtering:**
> *"Filter for sales in 'New York' where the profit margin is above 20%."*

**Visualization:**
> *"Plot a bar chart showing total sales by Product Category."*
> *"Generate a heatmap showing the correlation between all numerical columns."*

---

## 🛠️ Tech Stack

This project is built using the following technologies and frameworks:

* **Python**: The core programming language.
* **LangGraph**: For creating the cyclic, stateful agent workflow.
* **LangChain**: For prompt templates and tool bindings.
* **Google Gemini 2.5**: The high-performance LLM used for code generation.
* **Pandas**: For high-performance data manipulation and analysis.
* **Matplotlib / Seaborn**: For static data visualization.
* **Jupyter Notebook**: The interactive development environment.

---

## 👨‍💻 Author
**Sagar Sidhwa**
* **AI / ML Engineer**
* **Education:** MS in CS (AI Track) — Binghamton University
* *Focusing on ML and end-to-end real-world projects. Open to collaboration!*
