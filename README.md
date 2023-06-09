import csv

filename = 'data.csv'
with open(filename, 'r') as csvfile:
    csvreader = csv.reader(csvfile)
    sheetname = next(csvreader)[0]
    print(f"Sheetname of {filename}: {sheetname}")
