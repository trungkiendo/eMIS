
            # Xử lý tên table để bỏ các ký tự đặc biệt
            from_tables = [re.sub(r'[^\w\s]', '', table) for table in from_tables]
