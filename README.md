import pandas as pd
import zipfile
import xml.etree.ElementTree as ET
import re
import os

# Khởi tạo một tập hợp để lưu trữ các bảng đã xuất hiện
seen_tables = set()

# Khởi tạo một DataFrame để lưu trữ các bảng từ FROM hoặc JOIN
from_table_df = pd.DataFrame(columns=['File Name', 'Table'])

# Khởi tạo một DataFrame để lưu trữ các bảng được tạo mới
create_table_df = pd.DataFrame(columns=['File Name', 'Table'])

# Lọc các bảng theo thư viện
library = ("IDALPUB1", "ETLPUB01", "CFCPUB01", "CFCPUB", "CFCSTUDY")

# Đường dẫn đến file .egp
egp_file = 'C:/DANH BUI/DATAMART/15. CR_CUSTOMER/Cr_customer_newcore.egp'

# Mở file .egp và đọc nội dung
with zipfile.ZipFile(egp_file, 'r') as zip_file:
    # Lấy danh sách các file trong .egp
    file_list = zip_file.namelist()

    # Tìm các file program có đuôi .sas
    program_files = [f for f in file_list if f.endswith('.sas')]

    # Duyệt qua từng file program và tìm các bảng
    for program_file in program_files:
        egp_file_name = os.path.basename(egp_file)
        with zip_file.open(program_file) as f:
            # Đọc nội dung file program
            program_content = f.read().decode('utf-8')

            # Tìm tất cả các lệnh FROM hoặc JOIN bằng regex
            from_tables = re.findall(r'(?i)\b(?:from|join)\b\s+(\S+)', program_content)

            # Tìm tất cả các bảng được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
            create_tables = re.findall(r'(?si)\bproc\s+sql\b.*?\b(?:create|table)\b\s+(\S+)', program_content)

            # Tìm tất cả các bảng được tạo ra thông qua lệnh OUTPUT trong các PROC khác
            output_tables = re.findall(r'(?si)\bproc\s+(?!sql)\w*\b.*?\b(?:create|table|output)\b\s+(\S+)', program_content)

            # Lọc các bảng theo thư viện được chỉ định từ đầu
            from_tables = [table.upper().strip('[]') for table in from_tables if table.split('.')[0].upper() in library]
            from_tables = [re.sub(r'[\'\"\(\)|*\/to;:]', '', table) for table in from_tables]
            create_tables = [table.upper().strip('[]') for table in create_tables if
                             table.split('.')[0].upper() in library]
            create_tables = [re.sub(r'[\'\"\(\)|*\/to;:]', '', table) for table in create_tables]
            output_tables = [table.upper().strip('[]') for table in output_tables if
                             table.split('.')[0].upper() in library]
            output_tables = [re.sub(r'[\'\"\(\)|*\/to;:]', '', table) for table in output_tables]

            # Kết hợp danh sách các bảng được tạo ra bằng lệnh CREATE và OUTPUT
            new_tables = list(set(create_tables + output_tables))

            # Thêm các bảng tạo mới vào danh sách đã xuất hiện
            for table in new_tables:
                if table not in seen_tables:
                    create_table_df = create_table_df.append({'Table': table, 'File Name': egp_file_name}, ignore_index=True)
                    seen_tables.add(table)

            # Loại bỏ các bảng được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL hoặc đã xuất hiện trước đó
            from_tables = [table for table in from_tables if table not in new_tables and table not in seen_tables]

            # Thêm các bảng FROM hoặc JOIN được sử dụng vào danh sách đã xuất hithiện
            for table in from_tables:
                from_table_df = from_table_df.append({'Table': table, 'File Name': egp_file_name}, ignore_index=True)
                seen_tables.add(table)

# Ghi danh sách các bảng đã xuất hiện vào file Excel
with pd.ExcelWriter('table_list.xlsx') as writer:
    from_table_df.to_excel(writer, sheet_name='From Tables', index=False)
    create_table_df.to_excel(writer, sheet_name='Create Tables', index=False)
