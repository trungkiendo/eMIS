# Khởi tạo mảng chứa nhiều mảng chuỗi
arr = [["apple", "banana", "orange"], ["banana", "orange", "pear"], ["orange", "pear", "apple"], ["apple", "banana", "orange"]]

# Khởi tạo một đối tượng set để lưu trữ các từ không trùng lặp
unique_words = set()

# Duyệt qua từng mảng con trong mảng chính
for subarr in arr:
    # Nối các từ trong mảng con lại thành một chuỗi, cách nhau bởi dấu cách
    # Sau đó, sử dụng hàm split() để tách chuỗi thành các từ riêng lẻ
    words = subarr.split()
    
    # Thêm các từ vào đối tượng set unique_words để loại bỏ các từ trùng lặp
    unique_words.update(words)
    
# Duyệt qua các từ trong đối tượng set unique_words và in ra chúng thành một cột
for word in unique_words:
    print(word)
