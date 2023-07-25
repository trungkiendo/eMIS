import openpyxl
import os

# Lấy danh sách tất cả các tệp .xlsm trong thư mục
file_list = os.listdir(".")

# Lặp qua tất cả các tệp .xlsm
for file in file_list:

    # Nếu tệp là một tệp .xlsm, hãy mở nó
    if file.endswith(".xlsm"):

        # Mở sổ làm việc
        wb = openpyxl.load_workbook(file)

        # Lấy bảng SAS_Add_in
        table = wb.get_named_range("SAS_Add_in")

        # Xuất bảng ra tệp văn bản
        with open(file + "_SAS_Add_in.txt", "w") as f:
            f.write(table.value)

        # Lấy tất cả các trang tính
        sheets = wb.get_sheet_names()

        # Lặp qua tất cả các trang tính
        for sheet in sheets:

            # Lấy tất cả các mã VBA trên trang tính
            vba_code = wb[sheet].vba_code

            # Tìm tất cả các đường dẫn trong mã VBA
            paths = re.findall(r"(?<=path\(").+?(?=\))", vba_code)

            # In tất cả các đường dẫn
            for path in paths:
                print(path)

# Đóng tất cả các sổ làm việc
openpyxl.Workbook.close_all()
