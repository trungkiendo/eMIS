
            # Tìm tất cả các bảng được sử dụng trong các lệnh FROM hoặc JOIN
            tables = set()
            for code_block in code_blocks:
                # Tìm các tên bảng được sử dụng trong lệnh FROM hoặc JOIN bằng regex
                from_tables = re.findall(r'\bfrom\b\s+(\S+)', code_block, re.IGNORECASE)
                join_tables = re.findall(r'\bjoin\b\s+(\S+)', code_block, re.IGNORECASE)
                tables.update(from_tables)
                tables.update(join_tables)
            
            # In ra tất cả các bảng
            for table in tables:
                print(f"Table {table} is used in {program_file}")
