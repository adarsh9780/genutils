This is **significantly more complicated**, but also **much more valuable and engaging**.

You have effectively moved the goalpost from building a "Smart Filing Cabinet" (the previous idea) to building a **"Junior Financial Analyst"**.

Here is the breakdown of why the complexity has increased and how to structure this new challenge so it remains achievable for a hackathon.

### ⚖️ Complexity Comparison

| Feature | Previous Idea (News Sorter) | New Idea (Market Intelligence Agent) | Complexity Delta |
| :--- | :--- | :--- | :--- |
| **Input** | JSON list (Text only) | SQLite Database (Text + Time-series Data) | ⬆️ **Medium** (Requires SQL Tools) |
| **Core Task** | Categorization | Retrieval + Summarization + Analysis + Chat | ⬆️ **High** (Multi-agent coordination) |
| **Reasoning** | "What is this article about?" | "What happened to Company X's price when this news came out?" | ⬆️ **Very High** (Cross-modal reasoning) |
| **Interaction** | Batch Process (Input $\to$ Output) | Conversational (Chat loop with memory) | ⬆️ **High** (Managing chat history state) |

---

### 🚀 Revised Problem Statement: "The AI Market Analyst"

Since the complexity is higher, you should frame the problem as building an **Agentic Workflow** that acts as a researcher.

#### 1. The Mission
Build a **Market Intelligence Agent** using **LangGraph** that interacts with a structured database (SQLite). The agent must serve as a comprehensive analyst for 20 specific companies.

#### 2. The Agent Capabilities (The "Tools")
The agent needs to orchestrate the following abilities:
1.  **The SQL Retriever:** Convert user questions ("How did NVDA perform in March?") into SQL queries to fetch price or article data.
2.  **The Synthesizer:** Fetch raw news articles for a specific period and generate a concise summary.
3.  **The Theme Engine:**
    * *Mode A (Auto):* Read the retrieved articles and cluster them into emerging themes (e.g., "Supply Chain Issues", "CEO Scandal").
    * *Mode B (User):* Classify articles into user-provided buckets (e.g., "ESG", "Financials").
4.  **The Chatter:** Answer follow-up questions about the data (e.g., "Was the stock price volatile during the CEO scandal?").

#### 3. Why LangGraph is Critical Here
This is no longer a linear chain. It requires a **Router Architecture**:
* **User Query:** "Tell me about Tesla's news last month." $\rightarrow$ **Route to News Agent**.
* **User Query:** "What was the closing price on the 15th?" $\rightarrow$ **Route to SQL/Price Agent**.
* **User Query:** "Analyze the themes." $\rightarrow$ **Route to Clustering Node**.

---

### 🏆 Revised Evaluation Criteria (Weighted)

Since the scope is broader, the judging criteria must reward **integration** (making the tools work together) rather than just pure classification accuracy.

| Category | Weight | Focus Area |
| :--- | :--- | :--- |
| **1. Agent Orchestration (LangGraph)** | **40%** | How well does the supervisor routing work? Does it know when to query SQL vs. when to summarize text? |
| **2. Data Interaction (Tool Use)** | **20%** | Effectiveness of the SQL generation. Does it handle query errors (e.g., retrying if the SQL fails)? |
| **3. Insight Quality** | **20%** | Can the agent actually connect dots? (e.g., "The stock dropped 5% on the same day this negative article was published.") |
| **4. User Experience (Chat)** | **20%** | Does the agent maintain context? (If I ask "summarize Apple," then ask "what about the price?", does it know I still mean Apple?) |

---

### ⚠️ A Critical Implementation Warning

**Don't make them do advanced correlation math.**
Correlating stock price movements with news sentiment is a PhD-level Data Science problem.
* **Hackathon Expectation:** The agent should simply **display** the data side-by-side or answer factual questions.
    * *Good:* "On May 12th, the stock was $150. On that day, there was an article titled 'Earnings Miss'."
    * *Too Hard:* "This specific article caused a 2.3% drop with a p-value of 0.05."

### 🛠 Tech Stack Recommendation
To make this achievable in a hackathon timeframe (usually 24-48 hours), explicitly suggest they use:

1.  **LangChain SQLDatabase Toolkit:** For easy natural language to SQL conversion.
2.  **LangGraph `MessageGraph`:** To handle the chat history state.
3.  **SQLite:** Keep the schema simple (as you described).



**Would you like me to write the SQL Schema (CREATE TABLE statements) for the `articles` and `price` tables to ensure the participants have clean data to work with?**
