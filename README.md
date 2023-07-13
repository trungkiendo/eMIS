import win32com.client as win32

# Tạo đối tượng Excel và mở tệp .xlsm
excel = win32.Dispatch('Excel.Application')
workbook = excel.Workbooks.Open(r'C:\path\to\your\file.xlsm')

# Lấy danh sách các bảng đã được add-in
addin_tables = []
for ws in workbook.Worksheets:
    if ws.CodeName.startswith("SASx") and ws.Visible == 2:
        addin_tables.append(ws.Name)

# In ra danh sách các bảng đã được add-in
print("Các bảng SAS đã được add-in trong tệp Excel:")
for table in addin_tables:
    print(table)

# Đóng tệp Excel
workbook.Close()
excel.Quit()
