# Lấy tên file CSV và tên bảng từ tên sheetname của file CSV
filename = 'data.csv'
with open(filename, 'r') as csvfile:
    csvreader = csv.reader(csvfile)
    table_name = next(csvreader)[0]
