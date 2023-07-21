import os
import zipfile
import pandas as pd
import re

# Khởi tạo một tập hợp để lưu trữ các bảng đã xuất hiện
seen_tables = set()

# Khởi tạo một DataFrame để lưu trữ các bảng từ FROM hoặc JOIN
from_table_df = pd.DataFrame(columns=['File Name', 'Table'])

# Khởi tạo một DataFrame để lưu trữ các bảng được tạo mới hoặc xuất ra
create_output_table_df = pd.DataFrame(columns=['File Name', 'Table'])

# Lọc các bảng theo thư viện
library = ("IDALPUB1", "ETLPUB01", "CFCPUB01", "CFCPUB", "CFCSTUDY")

# Đường dẫn đến thư mục chứa các tệp .epg
folder_path = 'C:/epg_files/'

# Khởi tạo một DataFrame để lưu trữ kết quả cho tất cả các tệp .epg
all_tables_df = pd.DataFrame(columns=['File Name', 'Table', 'Type'])

# Mở từng tệp .epg trong thư mục và tìm các bảng
for file_name in os.listdir(folder_path):
    if file_name.endswith('.epg'):
        file_path = os.path.join(folder_path, file_name)
        
        # Thực hiện các thao tác trên từng tệp .epg như trong code ban đầu
        with zipfile.ZipFile(file_path, 'r') as zip_file:
            # Lấy danh sách các file trong .epg
            file_list = zip_file.namelist()

            # Tìm các file program có đuôi .sas
            program_files = [f for f in file_list if f.endswith('.sas')]

            # Duyệt qua từng file program và tìm các bảng
            for program_file in program_files:
                egp_file_name = os.path.basename(file_path)
                with zip_file.open(program_file) as f:
                    # Đọc nội dung file program
                    program_content = f.read().decode('utf-8')

                    # Tìm tất cả các lệnh FROM hoặc JOIN bằng regex
                    from_tables = re.findall(r'(?i)\b(?:from|join)\b\s+(\S+)', program_content)

                    # Tìm tất cả các bảng được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
                    create_tables = re.findall(r'(?si)\bproc\s+sql\b.*?\b(?:create|table)\b\s+(\S+)', program_content)

                    # Tìm tất cả các bảng được tạo ra thông qua lệnh OUTPUT trong các PROC khác
                    output_tables = re.findall(r'(?si)\bproc\s+(?!sql)\w*\b.*?\b(?:create|table|output)\b\s+(\S+)', program_content)

                    # Kết hợp danh sách các bảng được tạo ra bằng lệnh CREATE và OUTPUT
                    create_output_tables = create_tables + output_tables

                    # Lấy tất cả các bảng trong danh sách create_output_tables
                    create_output_table_list = []
                    for tables in create_output_tables:
                        create_output_table_list.extend(tables.split())

                    # Lọc các bảng theo thư viện được chỉ định từ đầu
                    create_output_tables_filtered = [table for table in create_output_table_list if any(lib in table for lib in library)]

                    # Lưu các bảng từ FROM hoặc JOIN vào from_table_df
                    for table in from_tables:
                        if any(lib in table for lib in library):
                            from_table_df = from_table_df.append({'File Name': egp_file_name, 'Table': table, 'Type': 'FROM/JOIN'}, ignore_index=True)

                    # Lưu các bảng được tạo mới hoặc xuất ra vàocreate_output_table_df
                    for table in create_output_tables_filtered:
                        if any(lib in table for lib in library):
                            create_output_table_df = create_output_table_df.append({'File Name': egp_file_name, 'Table': table}, ignore_index=True)

        # Lưu tất cả các bảng được tìm thấy trong tệp .epg vào all_tables_df
        all_tables = list(set(from_tables + create_output_tables_filtered))
        for table in all_tables:
            if any(lib in table for lib in library):
                all_tables_df = all_tables_df.append({'File Name': egp_file_name, 'Table': table, 'Type': 'FROM/JOIN' if table in from_tables else 'CREATE/OUTPUT'}, ignore_index=True)

# Ghi all_tables_df ra file Excel với 2 sheet riêng biệt
with pd.ExcelWriter('all_tables.xlsx') as writer:
    all_tables_df[all_tables_df['Type'] == 'FROM/JOIN'].to_excel(writer, sheet_name='FROM/JOIN')
    all_tables_df[all_tables_df['Type'] == 'CREATE/OUTPUT'].to_excel(writer, sheet_name='CREATE/OUTPUT')
