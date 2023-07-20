        # Loại bỏ các bảng mới được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
        from_tables = [table for table in from_tables if table not in create_tables]

        # Loại bỏ các bảng mới được tạo ra thông qua lệnh OUTPUT trong các PROC khác
        output_tables = re.findall(r'(?si)\bproc\s+(?!sql)\w*\b.*?\boutput\b\s+(\S+)', program_content)
        from_tables = [table for table in from_tables if table not in output_tables]

        # Lọc các bảng theo thư viện được chỉ định từ đầu
        from_tables = [table.upper().strip('[]') for table in from_tables if table.split('.')[0].upper() in library]
