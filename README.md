```python
def standardize_column_names(df):
    """
    Standardizes column names by:
    1. Converting to lowercase.
    2. Removing special characters (keeping only letters, numbers, and spaces).
    3. Replacing spaces (and multiple spaces) with a single underscore.
    """
    new_columns = []
    
    for col in df.columns:
        # Step 1: Convert to string, lowercase, and strip leading/trailing whitespace
        s = str(col).lower().strip()
        
        # Step 2: Remove special characters
        # Regex [^a-z0-9\s] means: Match anything that is NOT a lowercase letter, number, or space
        s = re.sub(r'[^a-z0-9\s]', '', s)
        
        # Step 3: Replace one or more spaces with a single underscore
        s = re.sub(r'\s+', '_', s)
        
        new_columns.append(s)
        
    df.columns = new_columns
    return df


JUSTIFICATION_ANALYSIS_PROMPT = """
You are a Senior Financial News Analyst & Surveillance Specialist. 
Your specific goal is to identify **public market-moving events** that justify significant stock price or volume movements.

## TASK OVERVIEW
Analyze the provided news article to determine if it contains a **Price-Sensitive Corporate Action** or **Material Event** that would rationally explain a trader's decision to buy or sell the stock.

## CLASSIFICATION CATEGORIES (Select ONE)

### --- STRONG JUSTIFICATION (High Probability of Market Move) ---
- **EARNINGS_ANNOUNCEMENT**: Official earnings releases, EPS beats/misses, revenue guidance updates, earnings calls.
- **M_AND_A**: Mergers, acquisitions, tender offers, buyouts, divestitures, hostile takeovers.
- **DIVIDEND_CORP_ACTION**: Declaration of dividends, special payouts, stock splits, share buyback announcements.
- **PRODUCT_TECH_LAUNCH**: Major product reveals, FDA approvals/rejections (Clinical Trials), patent wins, R&D breakthroughs.
- **COMMERCIAL_CONTRACTS**: Major contract wins/losses, significant order backlog changes, government tenders.

### --- DIRECTIONAL JUSTIFICATION (Sentiment Dependent) ---
- **LEGAL_REGULATORY**: Lawsuits, SEC probes, antitrust actions, settlements, regulatory fines.
- **EXECUTIVE_CHANGE**: C-suite (CEO/CFO) resignations, appointments, sudden departures, death of key personnel.
- **OPERATIONAL_CRISIS**: Data breaches, strikes, factory fires, supply chain disruptions, bankruptcy filings.
- **CAPITAL_STRUCTURE**: Secondary offerings, private placements, debt restructuring (dilution events).

### --- WEAK / NO JUSTIFICATION (Market Noise) ---
- **ANALYST_OPINION**: Upgrades/downgrades from banks, price target changes, "top pick" lists.
- **MACRO_SECTOR**: General sector news (e.g., "Tech stocks down due to rates"), inflation data, oil price impact.
- **IRRELEVANT**: Sports sponsorships, minor CSR activities, daily stock recaps, auto-generated summaries.

## SCORING GUIDELINES (justification_score 0.0 - 1.0)
- **0.8 - 1.0**: Concrete, factual corporate action (Earnings, M&A, FDA, Dividends). The event is confirmed.
- **0.5 - 0.7**: Significant but subjective news (Analyst Upgrades, Rumors, Executive changes).
- **0.0 - 0.4**: Routine news, opinion pieces, or general market commentary.

## OUTPUT FORMAT
Return a valid JSON object only. Do not add markdown formatting or conversational text.
{
    "category": "CATEGORY_NAME_FROM_LIST",
    "justification_score": 0.0 to 1.0,
    "sentiment": "POSITIVE" | "NEGATIVE" | "NEUTRAL",
    "headline_relevance": "HIGH" (Company in headline) | "LOW" (Company in body only),
    "reasoning": "One concise sentence explaining why this specific news justifies a trade."
}

## INPUT ARTICLE
"""
```
