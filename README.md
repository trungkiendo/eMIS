# Tạo danh sách các thư viện được chỉ định
library = ['IDALPUB1', 'ETLPUB01', 'CFCPUB01', 'CFCPUB', 'ETLPUB01']

# Duyệt qua từng file program và tìm các bảng
for program_file in program_files:
    with zip_file.open(program_file) as f:
        # Đọc nội dung file program
        program_content = f.read().decode('utf-8')

        # Tìm tất cả các lệnh FROM hoặc JOIN bằng regex
        from_tables = re.findall(r'(?i)\b(?:from|join)\b\s+(\S+)', program_content)

        # Tìm tất cả các bảng được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
        create_tables = re.findall(r'(?si)\bproc\s+sql\b.*?\b(?:create|table)\b\s+(\S+)', program_content)

        # Tìm tất cả các bảng được tạo ra thông qua lệnh OUTPUT trong các PROC khác
        output_tables = re.findall(r'(?si)\bproc\s+(?!sql)\w*\b.*?\boutput\b\s+(\S+)', program_content)

        # Lọc các bảng theo thư viện được chỉ định từ đầu
        from_tables = [table.upper().strip('[]') for table in from_tables if table.split('.')[0].upper() in library]
        create_tables = [table.upper().strip('[]') for table in create_tables if table.split('.')[0].upper() in library]
        output_tables = [table.upper().strip('[]') for table in output_tables if table.split('.')[0].upper() in library]

        # Loại bỏ các bảng được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
        from_tables = [table for table in from_tables if table not in create_tables]

        # Loại bỏ các bảng được tạo ra thông qua lệnh OUTPUT trong các PROC khác
        from_tables = [table for table in from_tables if table not in output_tables]

        # In ra tất cả các bảng
        for table in from_tables:
            if table not in seen_tables:
                table_df = table_df.append({'Table': table, 'File': program_file}, ignore_index=True)
                seen_tables.add(table)
