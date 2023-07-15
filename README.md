import zipfile
from lxml import etree

# Đường dẫn đến file SAS .egp
egp_file = "path/to/your/sas_project.egp"

# Giải nén file SAS .egp và lấy danh sách các file .xml trong thư mục "Project"
with zipfile.ZipFile(egp_file) as zf:
    xml_files = zf.namelist()
    xml_files = [f for f in xml_files if f.startswith("Project/") and f.endswith(".xml")]

# Tìm kiếm tên các bảng trong các file .xml tương ứng và in ra
for xml_file in xml_files:
    with zipfile.ZipFile(egp_file).open(xml_file) as f:
        xml_data = f.read()
        root = etree.fromstring(xml_data)
        for query in root.xpath("//Tree[@Type='query']"):
            table_names = query.xpath(".//Table[@Type='table' or @Type='view']/@Name")
            for table_name in table_names:
                print(table_name)
