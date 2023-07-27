import win32com.client

# Mở Excel
xl = win32com.client.Dispatch("Excel.Application")
xl.Visible = False

# Mở workbook chứa add-in
wb = xl.Workbooks.Open(r"C:\path\to\your\file.xlsm", ReadOnly=True)

# Lấy danh sách các add-in
add_ins = wb.AddIns2

# Lặp qua các add-in để tìm add-in của table SAS
for add_in in add_ins:
    if add_in.Name == "SAS":
        # Lấy bảng chứa dữ liệu
        table = add_in.InstalledRunTime.Table

        # Lấy tên các cột
        headers = [col.Name for col in table.Columns]

        # Lấy dữ liệu từ các hàng
        data = []
        for row in table.Rows:
            data.append([row.Columns(i).Value for i in range(1, len(headers) + 1)])

        # Xuất dữ liệu
        print("Column headers: ", headers)
        print("Data: ", data)

# Đóng workbook
wb.Close(False)

# Đóng Excel
xl.Quit()
