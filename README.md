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
            
            # In ra các đoạn code
            for code_block in code_blocks:
                print(f"Code block in {program_file}:")
                print(code_block)
