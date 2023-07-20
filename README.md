import pandas as pd

# Khởi tạo một DataFrame để lưu trữ các bảng
table_df = pd.DataFrame(columns=['Table', 'File'])

        
        # Thêm các bảng vào DataFrame
        for table in tables:
            table_df = table_df.append({'Table': table, 'File': program_file}, ignore_index=True)

# Lưu DataFrame vào file Excel
table_df.to_excel('table_list.xlsx', index=False)
