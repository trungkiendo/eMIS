# Khởi tạo mảng chứa 7 mảng con
arr = [["apple", "banana", "orange"], ["banana", "orange", "pear"], ["orange", "pear", "apple"], ["apple", "banana", "orange"], ["pear", "orange", "banana"], ["banana", "apple", "pear"], ["orange", "pear", "banana"]]

# Khởi tạo một đối tượng set để lưu trữ các từ không trùng lặp
unique_words = set()

# Duyệt qua từng mảng con trong mảng chính
for subarr in arr:
    # Duyệt qua từng từ trong mảng con và thêm nó vào đối tượng set unique_words
    for word in subarr:
        unique_words.add(word)
    
# Duyệt qua các từ trong đối tượng set unique_words và in ra chúng thành một cột
for word in unique_words:
    print(word)
