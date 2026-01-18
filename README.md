import pandas as pd
import numpy as np

# 1. Setup: Calculate targets
target_total = 1000
num_categories = df['category'].nunique()
base_n = target_total // num_categories  # Returns 28
remainder = target_total % num_categories # Returns 20

# 2. Define a sampling function
def get_stratified_sample(group):
    # If a category has fewer rows than we want, take them all
    n_to_take = min(len(group), base_n)
    return group.sample(n=n_to_take, random_state=42)

# 3. Take the base samples (approx 980 rows)
sampled_df = df.groupby('category', group_keys=False).apply(get_stratified_sample)

# 4. Fill the gap to reach exactly 1000
current_count = len(sampled_df)
needed = target_total - current_count

if needed > 0:
    # Identify rows we haven't picked yet
    remaining_pool = df.drop(sampled_df.index)
    
    # Sample the 'needed' amount from the remaining pool
    # This distributes the remainder randomly across categories
    remainder_df = remaining_pool.sample(n=needed, random_state=42)
    
    # Combine them
    final_df = pd.concat([sampled_df, remainder_df])
else:
    final_df = sampled_df

# Shuffle the final result so categories aren't clumped
final_df = final_df.sample(frac=1, random_state=42).reset_index(drop=True)

print(f"Total rows: {len(final_df)}")
print(final_df['category'].value_counts())
