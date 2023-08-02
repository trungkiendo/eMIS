import win32com.client
from openpyxl import load_workbook

# Khởi tạo ứng dụng SAS và kết nối
sas = win32com.client.Dispatch('SAS.Application')
sas.Visible = False

# Thực hiện mã SAS để refresh các bảng
sas_code = """
libname mylib 'path/to/my/lib';
proc sql;
    create table mytable as
    select * from mylib.mydata;
quit;
"""

sas.Submit(sas_code)

# Lấy danh sách tên các bảng được refresh
table_names = sas.Workspace('results').ListTables()

# Mở tập tin Excel và ghi tên các bảng vào sheet
workbook = load_workbook('ten_file_excel.xlsm')
worksheet = workbook['Sheet1']

row = 1
for table_name in table_names:
    worksheet.cell(row=row, column=1, value=table_name)
    row += 1

# Lưu tập tin Excel
workbook.save('ten_file_excel.xlsm')

# Đóng kết nối SAS và tập tin Excel
sas.Quit()
