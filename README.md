import csv
import sqlite3
import os

# Lấy tên file CSV và tên bảng từ tên file CSV
filename = 'data.csv'
table_name = os.path.splitext(filename)[0]

# Kết nối đến cơ sở dữ liệu SQLite
conn = sqlite3.connect('mydatabase.db')
cursor = conn.cursor()

# Kiểm tra xem bảng đã tồn tại trong cơ sở dữ liệu SQLite chưa
cursor.execute("SELECT count(name) FROM sqlite_master WHERE type='table' AND name=?", (table_name,))
if cursor.fetchone()[0] == 1:
    print(f"Table '{table_name}' already exists, skipping table creation")
else:
    # Tạo bảng trong cơ sở dữ liệu
    with open(filename, 'r') as csvfile:
        csvreader = csv.reader(csvfile)
        header = next(csvreader)
        columns = ', '.join([f"{c} TEXT" for c in header])
        cursor.execute(f"CREATE TABLE {table_name} ({columns})")
        print(f"Table '{table_name}' created")

# Đọc dữ liệu từ file CSV và chèn vào bảng trong cơ sở dữ liệu
with open(filename, 'r') as csvfile:
    csvreader = csv.reader(csvfile)
    next(csvreader)  # bỏ qua hàng tiêu đề
    for row in csvreader:
        cursor.execute(f'INSERT INTO {table_name} VALUES ({",".join(["?"] * len(row))})', row)

# Lưu lại các thay đổi và đóng kết nối đến cơ sở dữ liệu
conn.commit()
conn.close()
