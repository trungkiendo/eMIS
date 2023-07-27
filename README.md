import win32com.client

# Mở Excel
xl = win32com.client.Dispatch("Excel.Application")
xl.Visible = True

# Mở workbook chứa add-in
wb = xl.Workbooks.Open(r"C:\path\to\your\file.xlsm")

# Lấy dữ liệu từ add-in
data = xl.Run("SASMacroName")

# Xuất dữ liệu ra tệp văn bản
with open("output.txt", "w") as f:
    for row in data:
        f.write('\t'.join([str(cell) for cell in row]) + '\n')

# Đóng workbook
wb.Close(False)

# Đóng Excel
xl.Quit()
