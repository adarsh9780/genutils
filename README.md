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


# ... (Your existing list) ...
- **DIVIDEND_POLICY**: Dividend declarations, special dividends, yield changes, ex-dividend dates.
- **PRODUCT_TECH_LAUNCH**: Non-pharma product launches, technology reveals, R&D breakthroughs.
```
