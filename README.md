# Các mảng ban đầu
arr1 = ['apple', 'banana', 'orange', 'pear', 'cherry']
arr2 = ['apple', 'pear', 'grape', 'kiwi', 'orange']
arr3 = ['cherry', 'kiwi', 'mango', 'peach']
arr4 = ['orange', 'pear', 'apple']

# Tạo một tập hợp rỗng để lưu các phần tử không bị trùng
unique_set = set()

# Duyệt qua từng mảng và từng phần tử trong mỗi mảng, nếu phần tử đó chưa có trong tập hợp thì thêm nó vào tập hợp
for item in arr1 + arr2 + arr3 + arr4:
    if item not in unique_set:
        unique_set.add(item)

# Chuyển tập hợp sang mảng để lấy danh sách các phần tử không bị trùng
unique_arr = list(unique_set)

# In mảng mới
print(unique_arr)
