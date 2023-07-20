# Đường dẫn đến file SAS .egp
# egp_file = "D:/Kien/Card/ISH_card.egp"
import zipfile
import xml.etree.ElementTree as ET
import re
import pandas as pd

# Khởi tạo một DataFrame để lưu trữ các bảng
table_df = pd.DataFrame(columns=['Table', 'File'])

# Đường dẫn đến file .egp
egp_file = 'C:/DANH BUI/DATAMART/1. CR application/CR APPLICATION.egp'
# Mở file .egp và đọc nội dung
with zipfile.ZipFile(egp_file, 'r') as zip_file:
    # Lấy danh sách các file trong .egp
    file_list = zip_file.namelist()

    # Tìm các file program có đuôi .sas
    program_files = [f for f in file_list if f.endswith('.sas')]

    # Duyệt qua từng file program và tìm các bảng
    for program_file in program_files:
        with zip_file.open(program_file) as f:
            # Đọc nội dung file program
            program_content = f.read().decode('utf-8')

            # Tìm tất cả các lệnh FROM hoặc JOIN bằng regex
            from_tables = re.findall(r'(?i)\b(?:from|join)\b\s+(\S+)', program_content)

            # Tìm tất cả các bảng được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
            create_tables = re.findall(r'(?si)\bproc\s+sql\b.*?\b(?:create|table)\b\s+(\S+)', program_content)

            # Loại bỏ các bảng mới được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
            from_tables = [table for table in from_tables if table not in create_tables]

            # Loại bỏ các bảng mới được tạo ra thông qua lệnh OUTPUT trong các PROC khác
            output_tables = re.findall(r'(?si)\bproc\s+(?!sql)\w*\b.*?\boutput\b\s+(\S+)', program_content)
            from_tables = [table for table in from_tables if table not in output_tables]

            # Tìm tất cả các bảng được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
            create_tables = re.findall(r'(?si)\bproc\s+sql\b.*?\b(?:create|table)\b\s+(\S+)', program_content)

            # Loại bỏ các bảng mới đượ;c tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
            from_tables = [table for table in from_tables if table not in create_tables]
            seen_table = set()
            unique_table = []
            library = ("IDALPUB1","ETLPUB01","CFCPUB01","CFCPUB","ETLPUB01")
            # In ra tất cả các bảng
            # print(from_tables)
            # for subarr in from_tables:
                # Duyệt qua từng từ trong mảng con và thêm nó vào đối tượng set unique_words
                # for word in subarr:
                # seen_table.add(subarr)
                    # Duyệt qua các từ trong đối tượng set unique_words và in ra chúng thành một cột
            # for word in seen_table:
            # print(seen_table)
            for table in from_tables:
                # print("Hi "+table.upper())
                index = table.find(".")
                lib_table = table[:index]
                # print(lib_table)
                if lib_table.upper() in library:
                    # seen_table.add(table.upper())
                    if table.upper() not in seen_table:
                        unique_table.append(table.upper())
                        seen_table.add(table.upper())
