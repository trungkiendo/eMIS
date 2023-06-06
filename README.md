import pandas as pd
import sqlite3

# đọc dữ liệu từ file1.xlsx và lưu vào dataframe1
dataframe1 = pd.read_excel('file1.xlsx')

# đọc dữ liệu từ file2.xlsx và lưu vào dataframe2
dataframe2 = pd.read_excel('file2.xlsx')

# kết nối với cơ sở dữ liệu SQLite
conn = sqlite3.connect('database.db')

# tạo đối tượng cursor
cursor = conn.cursor()

# cập nhật bảng table1 từ khung dữ liệu dataframe1
dataframe1.to_sql('table1', conn, if_exists='replace', index=False)

# cập nhật bảng table2 từ khung dữ liệu dataframe2
dataframe2.to_sql('table2', conn, if_exists='replace', index=False)

# đóng kết nối với cơ sở dữ liệu SQLite
conn.close()
