```python
import pandas as pd
import numpy as np

def get_balanced_dataset(df, col_name, target_total, random_state=42):
    """
    Returns a dataframe with exactly 'target_total' rows, balanced across the categorical column.
    Uses 'head()' instead of 'apply()' to avoid Pandas 2.2+ warnings and improve performance.
    """
    
    # 0. Safety Check
    if target_total >= len(df):
        return df.copy()

    # 1. Shuffle the data first
    # This ensures that when we take the 'head' (first N rows), they are random.
    shuffled_df = df.sample(frac=1, random_state=random_state)
    
    # 2. Calculate base samples per category
    num_categories = df[col_name].nunique()
    base_n = target_total // num_categories
    
    # 3. Take base samples using head()
    # head(n) takes the first n rows. If a group has fewer than n, it takes them all.
    sampled_df = shuffled_df.groupby(col_name).head(base_n)
    
    # 4. Fill the gap to reach exactly target_total
    current_count = len(sampled_df)
    needed = target_total - current_count
    
    if needed > 0:
        # Identify rows we haven't picked yet
        # We use the index to find the difference
        remaining_pool = df.drop(sampled_df.index)
        
        # Sample the needed amount from the rest
        remainder_df = remaining_pool.sample(n=needed, random_state=random_state)
        
        # Combine
        final_df = pd.concat([sampled_df, remainder_df])
    else:
        final_df = sampled_df

    # 5. Final Shuffle to mix the categories together
    return final_df.sample(frac=1, random_state=random_state).reset_index(drop=True)
```
