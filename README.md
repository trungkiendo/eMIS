import zipfile
import xml.etree.ElementTree as ET
import re

# Đường dẫn đến file .egp
egp_file = 'path/to/your/egp/file.egp'

# Mở file .egp và đọc nội dung
with zipfile.ZipFile(egp_file, 'r') as zip_file:
    # Lấy danh sách các file trong .egp
    file_list = zip_file.namelist()

    # Tìm các file program có đuôi .sas hoặc .py
    program_files = [f for f in file_list if f.endswith('.sas') or f.endswith('.py')]

    # Duyệt qua từng file program và tìm các đoạn code
    for program_file in program_files:
        with zip_file.open(program_file) as f:
            # Đọc nội dung file program
            program_content = f.read().decode('utf-8')
            
            # Tìm các đoạn code trong file program bằng regex
            code_blocks = re.findall(r'(?s)\bdata\b.*?\b;|(?s)\bproc\b.*?\b;|\bdef\b.*?:\n.*?(?=^\S|\Z)', program_content)
            
            # Tìm tất cả các bảng có trong các đoạn code
            tables = set()
            for code_block in code_blocks:
                table_names = re.findall(r'\btable\b\s+(\S+)', code_block)
                tables.update(table_names)
            
            # In ra tất cả các bảng
            for table in tables:
                print(f"Table {table} is used in {program_file}")
