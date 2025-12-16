import openpyxl

def get_cell_value_from_all_sheets(file_path, row_num, col_letter):
    """
    Extracts the value of a specific cell (e.g., 'B17') from every sheet in an Excel file.
    
    Args:
        file_path (str): Path to the .xlsx file.
        row_num (int): The row number (e.g., 17).
        col_letter (str): The column letter (e.g., 'B').
        
    Returns:
        dict: A dictionary where keys are sheet names and values are the cell values.
    """
    results = {}
    
    try:
        # 1. Load the workbook (data_only=True gets the value, not the formula)
        workbook = openpyxl.load_workbook(file_path, data_only=True)
        
        # 2. Get all sheet names
        sheet_names = workbook.sheetnames
        print(f"Found {len(sheet_names)} sheets: {sheet_names}")
        
        # 3. Iterate through each sheet
        for name in sheet_names:
            sheet = workbook[name]
            
            # Construct the coordinate (e.g., "B" + "17" = "B17")
            coordinate = f"{col_letter}{row_num}"
            
            # Get the value
            cell_value = sheet[coordinate].value
            results[name] = cell_value
            
    except Exception as e:
        print(f"Error: {e}")
        return None

    return results

# --- Example Usage ---

# Define your inputs here
file_name = 'your_data.xlsx'
target_row = 17
target_col = 'B'

# Call the function
extracted_data = get_cell_value_from_all_sheets(file_name, target_row, target_col)

# Print the results
if extracted_data:
    print("\n--- Extracted Values ---")
    for sheet, val in extracted_data.items():
        print(f"Sheet '{sheet}': {val}")
