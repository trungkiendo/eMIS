import openpyxl
import sqlite3

# Open the Excel file
workbook = openpyxl.load_workbook('example.xlsx')

# Select the worksheet
worksheet = workbook.active

# Connect to the SQLite database
conn = sqlite3.connect('example.db')

# Iterate over the rows in the worksheet and insert the data into the database
for row in worksheet.iter_rows(values_only=True):
    cursor = conn.cursor()
    cursor.execute("INSERT INTO my_table (col1, col2, col3) VALUES (?, ?, ?)", row)
    conn.commit()

# Close the database connection
conn.close()
