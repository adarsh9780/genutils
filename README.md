Here is a structured, professional, and exciting problem statement framework. You can copy-paste this directly into your hackathon landing page or slide deck.

I have structured this as a **"Mission Brief"** to make it engaging for developers.

-----

### 🚀 Hackathon Challenge: The Agentic Newsroom

#### 1\. The Background

In the age of information overload, media companies and financial analysts are drowning in a firehose of news. Traditional keyword filtering ("if text contains 'Bitcoin'") is brittle and misses nuance. Simple LLM calls are often inconsistent.

We need a system that doesn't just "read" news but "thinks" about where it belongs—handling ambiguity, adhering to strict user taxonomies, or dynamically discovering new trends on the fly.

#### 2\. The Mission

Build an **Intelligent News Categorization Agent** using **LangGraph**. Your system must ingest a stream of news articles and route them into categories with high precision.

The system must operate in two distinct modes:

1.  **The Librarian (User-Defined):** The user provides a strict list of themes (e.g., "Mergers", "Politics", "Cybersecurity"). The agent must categorize articles *only* into these buckets or flag them as "Uncategorized."
2.  **The Explorer (System-Generated):** The user provides no themes. The agent must analyze a batch of articles, identify the emerging topics (clustering), and tag the articles accordingly.

#### 3\. Core Technical Requirements

Participants must demonstrate the use of **LangGraph** to manage the workflow. A simple linear LLM chain (Input $\rightarrow$ Output) is **not** sufficient. We expect to see:

  * **State Management:** Passing article data, current tags, and confidence scores between nodes.
  * **Cyclic Logic (Reflection):** If the LLM is unsure (low confidence), the graph should loop back to a "Critic Node" or "Search Node" to gather more context before deciding.
  * **Human-in-the-Loop (Bonus):** If an article is ambiguous, the system pauses and asks the user for help, then remembers that decision.

#### 4\. The User Stories

  * *As a Financial Analyst,* I want to upload 50 URLs and have them sorted strictly into ["Bullish", "Bearish", "Neutral"] so I can quantify market sentiment.
  * *As a News Editor,* I want to feed in the morning wire and have the system tell *me* what the top 5 trending topics are today (Dynamic Theme Generation).

#### 5\. Judging Focus

Your solution will be evaluated on:

  * **Architecture:** How well did you use LangGraph nodes and edges? (Did you use conditional edges?)
  * **Accuracy:** How well does it handle nuances? (e.g., Distinguishing "Apple" the fruit from "Apple" the tech giant).
  * **Robustness:** Can it handle articles that don't fit any category?

-----

### 🛠 Suggested Input Data (For Participants)

To make the problem concrete, define the input format they should expect:

**Input Format (JSON):**

```json
{
  "mode": "user_defined", // or "system_generated"
  "target_themes": ["Politics", "Technology", "Sports"], // Empty if mode is system_generated
  "articles": [
    {
      "id": "101",
      "headline": "Tech giant faces antitrust lawsuit in EU",
      "content": "..."
    },
    {
      "id": "102",
      "headline": "Olimpics opening ceremony draws millions",
      "content": "..."
    }
  ]
}
```

**Expected Output Format:**

```json
{
  "results": [
    {
      "id": "101",
      "category": "Technology",
      "confidence": 0.95,
      "reasoning": "Article discusses regulation of a tech company."
    }
  ]
}
```

-----

### 💡 Pro Tip for the Organizer

To ensure the hackathon goes smoothly, I recommend providing a "Boilerplate" repo.

**Would you like me to generate a `graph.py` skeleton code that sets up the basic State Schema and Nodes (Classifier, Generator) so participants don't start from zero?**
