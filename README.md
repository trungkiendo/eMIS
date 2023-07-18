import zipfile
import xml.etree.ElementTree as ET
import re

# Đường dẫn đến file .egp
egp_file = 'path/to/your/egp/file.egp'

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
            
            # In ra tất cả các bảng
            for table in from_tables:
                print(f"Table {table} is used in {program_file}")
