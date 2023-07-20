# Tìm tất cả các bảng được tạo ra thông qua lệnh CREATE, TABLE, DATA hoặc SET trong PROC SQL hoặc DATA step
create_tables = re.findall(r'(?si)\b(?:proc\s+sql|data)\b.*?\b(?:create|table|data|set)\b\s+(\S+)', program_content)

# Tìm tất cả các bảng được tạo ra thông qua lệnh OUTPUT hoặc SET trong các PROC khác
output_tables = re.findall(r'(?si)\bproc\s+(?!sql)\w*\b.*?\b(?:create|table|output|set)\b\s+(\S+)', program_content)

# Kết hợp danh sách các bảng được tạo ra
create_output_tables = create_tables + output_tables

# Lọc các bảng theo thư viện được chỉ định từ đầu
create_output_tables = [table.upper().strip('[]') for table in create_output_tables]
create_output_tables = [re.sub(r'[\'\"\(\)|*\/to;:]', '', table) for table in create_output_tables]
create_output_tables = [table for table in create_output_tables if table.split('.')[0].upper() in library]
create_output_tables = list(set(create_output_tables))

# Thêm các bảng tạo mới vào danh sách đã xuất hiện
for table in create_output_tables:
    if table not in seen_tables:
        create_output_table_df = create_output_table_df.append({'Table': table, 'File Name': egp_file_name}, ignore_index=True)
        seen_tables.add(table)
