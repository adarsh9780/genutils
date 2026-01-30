```python
import pandas as pd
import sqlite3
import os

def csv_to_sqlite(csv_file, db_name, table_name, mode='overwrite'):
    """
    Converts a CSV file to a SQLite table with flexible handling for 
    appending or overwriting data.
    """
    # 1. Load the CSV data
    if not os.path.exists(csv_file):
        print(f"Error: File '{csv_file}' not found.")
        return

    df = pd.read_csv(csv_file)

    # 2. Connect to (or create) the database
    conn = sqlite3.connect(db_name)
    
    # 3. Handle 'append' logic vs 'overwrite'
    if mode == 'append':
        # Check if table exists to validate columns
        cursor = conn.cursor()
        cursor.execute(f"SELECT name FROM sqlite_master WHERE type='table' AND name='{table_name}';")
        
        if cursor.fetchone():
            # Get existing table columns
            existing_cols = pd.read_sql(f"SELECT * FROM {table_name} LIMIT 0", conn).columns.tolist()
            
            # Identify columns to drop from the CSV
            extra_cols = [col for col in df.columns if col not in existing_cols]
            
            if extra_cols:
                print(f"Warning: The following columns were dropped to match the database schema: {extra_cols}")
                df = df.drop(columns=extra_cols)
            
            # Ensure we aren't missing columns required by the DB (optional but helpful)
            missing_cols = [col for col in existing_cols if col not in df.columns]
            if missing_cols:
                print(f"Note: CSV is missing columns {missing_cols}. These will be NULL in the DB.")

        if_exists_param = 'append'
    else:
        # Default is overwrite
        if_exists_param = 'replace'

    # 4. Write to the database
    try:
        df.to_sql(table_name, conn, if_exists=if_exists_param, index=False)
        print(f"Success: Data {mode}ed to table '{table_name}' in '{db_name}'.")
    except Exception as e:
        print(f"An error occurred: {e}")
    finally:
        conn.close()

# --- Example Usage ---
# csv_to_sqlite('data.csv', 'my_database.db', 'users', mode='overwrite')
# csv_to_sqlite('new_data.csv', 'my_database.db', 'users', mode='append')```
