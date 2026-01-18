```python
import pandas as pd
import numpy as np

def get_balanced_dataset(df, col_name, target_total, random_state=42):
    """
    Returns a dataframe with exactly 'target_total' rows, balanced across the categorical column.
    
    Parameters:
    - df: The source dataframe.
    - col_name: The name of the categorical column.
    - target_total: The exact number of rows desired in the output.
    - random_state: Seed for reproducibility.
    """
    
    # 0. Safety Check: If target is larger than data, return everything
    if target_total >= len(df):
        return df.copy()

    # 1. Calculate base samples per category
    num_categories = df[col_name].nunique()
    base_n = target_total // num_categories
    
    # 2. Define stratified sampler
    def sampler(group):
        # Take base_n, or all items if the category is small
        return group.sample(n=min(len(group), base_n), random_state=random_state)

    # 3. Take the base samples
    sampled_df = df.groupby(col_name, group_keys=False).apply(sampler)
    
    # 4. Fill the gap to reach exactly target_total
    current_count = len(sampled_df)
    needed = target_total - current_count
    
    if needed > 0:
        # Get rows that weren't picked in the first pass
        # Note: This relies on unique indices in your dataframe
        remaining_pool = df.drop(sampled_df.index)
        
        # Sample the remainder from the general pool
        remainder_df = remaining_pool.sample(n=needed, random_state=random_state)
        
        # Combine
        final_df = pd.concat([sampled_df, remainder_df])
    else:
        final_df = sampled_df

    # 5. Final Shuffle
    return final_df.sample(frac=1, random_state=random_state).reset_index(drop=True)
```
