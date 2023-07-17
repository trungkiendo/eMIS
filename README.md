import zipfile
import xml.etree.ElementTree as ET

# Đường dẫn đến file .egp
egp_file = 'path/to/your/egp/file.egp'

# Mở file .egp và đọc nội dung
with zipfile.ZipFile(egp_file, 'r') as zip_file:
    # Lấy danh sách các file trong .egp
    file_list = zip_file.namelist()
    
    # Lọc ra các file có phần mở rộng là ".egp"
    egp_files = [f for f in file_list if f.endswith('.egp')]
    
    # Duyệt qua từng file .egp để lấy danh sách các bảng
    for egp_file in egp_files:
        with zip_file.open(egp_file) as f:
            # Đọc nội dung file .egp dưới dạng string
            egp_content = f.read().decode('utf-8')
            
            # Phân tích cấu trúc XML của file .egp
            root = ET.fromstring(egp_content)
            
            # Tìm tất cả các bảng trong file .egp
            for table in root.findall('.//table'):
                table_name = table.get('name')
                print(f"Table name: {table_name}")
