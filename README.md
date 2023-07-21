import os

# Khởi tạo một tập hợp để lưu trữ các bảng đã xuất hiện
seen_tables = set()

# Khởi tạo một DataFrame để lưu trữ các bảng từ FROM hoặc JOIN
from_table_df = pd.DataFrame(columns=['File Name', 'Table'])

# Khởi tạo một DataFrame để lưu trữ các bảng được tạo mới hoặc xuất ra
create_output_table_df = pd.DataFrame(columns=['File Name', 'Table'])

# Lọc các bảng theo thư viện
library = ("IDALPUB1", "ETLPUB01", "CFCPUB01", "CFCPUB", "CFCSTUDY")

# Đường dẫn đến thư mục chứa các tệp .epg
folder_path = 'C:/DANH BUI/DATAMART/15. CR_CUSTOMER'

# Mở từng tệp .epg trong thư mục và tìm các bảng
for file_name in os.listdir(folder_path):
    if file_name.endswith('.epg'):
        file_path = os.path.join(folder_path, file_name)
        
        # Thực hiện các thao tác trên từng tệp .epg như trong code ban đầu
        with zipfile.ZipFile(file_path, 'r') as zip_file:
            # Lấy danh sách các file trong .epg
            file_list = zip_file.namelist()

            # Tìm các file program có đuôi .sas
            program_files = [f for f in file_list if f.endswith('.sas')]

            # Duyệt qua từng file program và tìm các bảng
            for program_file in program_files:
                egp_file_name = os.path.basename(file_path)
                with zip_file.open(program_file) as f:
                    # Đọc nội dung file program
                    program_content = f.read().decode('utf-8')

                    # Tìm tất cả các lệnh FROM hoặc JOIN bằng regex
                    from_tables = re.findall(r'(?i)\b(?:from|join)\b\s+(\S+)', program_content)

                    # Tìm tất cả các bảng được tạo ra thông qua lệnh CREATE hoặc TABLE trong PROC SQL
                    create_tables = re.findall(r'(?si)\bproc\s+sql\b.*?\b(?:create|table)\b\s+(\S+)', program_content)

                    # Tìm tất cả các bảng được tạo ra thông qua lệnh OUTPUT trong các PROC khác
                    output_tables = re.findall(r'(?si)\bproc\s+(?!sql)\w*\b.*?\b(?:create|table|output)\b\s+(\S+)', program_content)

                    # Kết hợp danh sách các bảng được tạo ra bằng lệnh CREATE và OUTPUT
                    create_output_tables = create_tables + output_tables

                    # Lấy tất cả các bảng trong danh sách create_output_tables
                    create_output_table_list = []
                    for tables in create_output_tables:
                        create_output_table_list.extend(tables.split())

                    # Lọc các bảng theo thư viện được chỉ định từ đầu
                    create_output_table_list = [table.upper().strip('[]') for table in create_output_table_list if table.split('.')[0].upper() in library]
                    create_output_table_list = [re.sub(r'[\'\"\(\)|*\/to;:]', '', table) for table in create_output_table_list]

                    # Thêm các bảng tạo mới hoặc xuất ra vào danh sách đã xuất hiện
                    for table in create_output_table_list:
                        if table not in seen_tables:
                        create_output_table_df = create_output_table_df.append({'Table': table, 'File Name': egp_file_name}, ignore_index=True)
                        seen_tables.add(table)

                    # Tìm các bảng từ lệnh FROM hoặc JOIN
                    from_tables = re.findall(r'(?i)\b(?:from|join)\b\s+(\S+)', program_content)

                    # Lọc các bảng theo thư viện được chỉ định từ đầu
                    from_tables = [table.upper().strip('[]') for table in from_tables]
                    from_tables = [re.sub(r'[\'\"\(\)|*\/to;:]', '', table) for table in from_tables]
                    from_tables = [table for table in from_tables if table.split('.')[0].upper() in library]
                    from_tables = list(set(from_tables))

                    # Thêm các bảng từ lệnh FROM hoặc JOIN vào danh sách từ bảng đã xuất hiện và loại bỏ các bảng trong danh sách tạo mới hoặc xuất ra
                    for table in from_tables:
                        if table not in seen_tables:
                            if table not in create_output_table_list:
                                from_table_df = from_table_df.append({'Table': table, 'File Name': egp_file_name}, ignore_index=True)
                                seen_tables.add(table)

        # Xóa các bảng trùng lặp trong DataFrame create_output_table_df
        create_output_table_df.drop_duplicates(inplace=True)

        # Xuất kết quả vào file Excel
        with pd.ExcelWriter('Table_List.xlsx', mode='a') as writer:
            create_output_table_df.to_excel(writer, sheet_name='Created/Output Tables', index=False)
            from_table_df.to_excel(writer, sheet_name='From/Join Tables', index=False)

        # Reset các DataFrame và tập hợp để sử dụng cho tệp .epg tiếp theo
        seen_tables = set()
        from_table_df = pd.DataFrame(columns=['File Name', 'Table'])
        create_output_table_df = pd.DataFrame(columns=['File Name', 'Table'])
