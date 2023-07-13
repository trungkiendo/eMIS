import os
import datetime

# Tìm ngày hôm nay
today = datetime.datetime.now().strftime('%Y-%m-%d')

# Liệt kê tất cả các file .xlsx và .xls trong thư mục report và daily tạo trong ngày hôm nay
report_dir = 'path/to/report/folder'
daily_dir = 'path/to/daily/folder'
file_paths = [os.path.join(report_dir, f) for f in os.listdir(report_dir) if (f.endswith('.xlsx') or f.endswith('.xls')) and os.path.getctime(os.path.join(report_dir, f)).strftime('%Y-%m-%d') == today] + [os.path.join(daily_dir, f) for f in os.listdir(daily_dir) if (f.endswith('.xlsx') or f.endswith('.xls')) and os.path.getctime(os.path.join(daily_dir, f)).strftime('%Y-%m-%d') == today]

# In ra danh sách các file tìm được
print("Danh sách các file .xlsx và .xls trong thư mục report và daily tạo trong ngày hôm nay:")
for file_path in file_paths:
    print(file_path)
