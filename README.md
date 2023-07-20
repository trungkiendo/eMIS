# Tạo danh sách bảng tạo mới
new_tables = list(set(create_tables))

# Xuất ra danh sách bảng tạo mới
for table in new_tables:
    if table not in seen_tables:
        print(table)
        seen_tables.add(table)
