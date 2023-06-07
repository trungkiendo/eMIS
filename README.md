import tkinter as tk
from tkinter import ttk

# Tạo đối tượng Tkinter
root = tk.Tk()
root.geometry('400x300')

# Tạo cây dữ liệu Treeview
treeview = ttk.Treeview(root, columns=('Name', 'Age', 'Gender', 'Color'))

# Thêm tiêu đề cho các cột
treeview.heading('#0', text='ID', anchor='center')
treeview.heading('Name', text='Name', anchor='center')
treeview.heading('Age', text='Age', anchor='center')
treeview.heading('Gender', text='Gender', anchor='center')
treeview.heading('Color', text='Color', anchor='center')

# Thêm dữ liệu vào cây dữ liệu
data = [
    ('001', 'Alice', 25, 'Female', 'Red'),
    ('002', 'Bob', 30, 'Male', 'Green'),
    ('003', 'Charlie', 35, 'Male', 'Blue'),
    ('004', 'David', 40, 'Male', 'Red'),
    ('005', 'Eve', 45, 'Female', 'Green'),
]

for i, row in enumerate(data):
    item = treeview.insert('', 'end', text=i+1, values=row)
    # Đặt tag cho dòng là tên màu trong cột "Color"
    color = row[-1]
    if color == 'Red':
        tag_name = 'red'
    elif color == 'Green':
        tag_name = 'green'
    elif color == 'Blue':
        tag_name = 'blue'
    else:
        tag_name = ''
    treeview.tag_configure(tag_name, background=color)
    treeview.item(item, tags=(tag_name,))

# Đặt cây dữ liệu vào giao diện
treeview.pack(fill='both', expand=True)

# Khởi chạy giao diện
root.mainloop()
