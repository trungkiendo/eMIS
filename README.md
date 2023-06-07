# Đặt tag cho dòng đầu tiên
first_item = treeview.identify_row(0)
treeview.item(first_item, tags=('test_tag',))

# Cấu hình màu nền cho tag
treeview.tag_configure('test_tag', background='red')
