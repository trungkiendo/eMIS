# Tìm các file program có đuôi .sas hoặc .py
program_files = [f for f in file_list if f.endswith('.sas') or f.endswith('.py')]

# Duyệt qua từng file program và tìm các đoạn code
for program_file in program_files:
    with zip_file.open(program_file) as f:
        # Đọc nội dung file program
        program_content = f.read().decode('utf-8')
        
        # Tìm các đoạn code trong file program bằng regex
        code_blocks = re.findall(r'(?s)\bdata\b.*?\b;|(?s)\bproc\b.*?\b;|\bdef\b.*?:\n.*?(?=^\S|\Z)', program_content)
        
        # Tìm tất cả các bảng được sử dụng trong các lệnh FROM hoặc JOIN
        tables = set()
        for code_block in code_blocks:
            # Tìm các tên bảng được sử dụng trong lệnh FROM hoặc JOIN bằng regex
            from_tables = re.findall(r'\bfrom\b\s+(\S+)', code_block, re.IGNORECASE)
            join_tables = re.findall(r'\bjoin\b\s+(\S+)', code_block, re.IGNORECASE)
            tables.update(from_tables)
            tables.update(join_tables)
            
            # Loại trừ các bảng mới được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
            create_tables = re.findall(r'(?s)\bproc sql\b.*?\bcreate\b.*?\btable\b\s+(\S+)', code_block, re.IGNORECASE)
            tables.difference_update(create_tables)
            
            # Loại trừ các bảng mới được tạo ra thông qua lệnh OUTPUT trong các PROC khác
            output_tables = re.findall(r'(?s)\bproc\b(?! sql)\w*\b.*?\boutput\b\s+(\S+)', code_block, re.IGNORECASE)
            tables.difference_update(output_tables)
        
        # In ra tất cả các bảng
        for table in tables:
            print(f"Table {table} is used in {program_file}")
