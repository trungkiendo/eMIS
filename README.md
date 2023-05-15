# Import các thư viện cần thiết
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, confusion_matrix, classification_report

# Đọc dữ liệu vào DataFrame
df = pd.read_csv('data.csv')

# Chuẩn bị dữ liệu
X = df[['income', 'credit_history', 'previous_loans', 'purpose']]
X = pd.get_dummies(X)
y = df['fraud']

# Chia dữ liệu thành tập huấn luyện và tập kiểm tra
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Khởi tạo mô hình Logistic Regression
model = LogisticRegression()

# Huấn luyện mô hình trên tập huấn luyện
model.fit(X_train, y_train)

# Dự đoán trên tập kiểm tra
y_pred = model.predict(X_test)

# Đánh giá độ chính xác của mô hình
accuracy = accuracy_score(y_test, y_pred)
print('Accuracy:', accuracy)

# Tính toán confusion matrix, precision, recall và F1-score
cm = confusion_matrix(y_test, y_pred)
print('Confusion matrix:\n', cm)

report = classification_report(y_test, y_pred)
print('Classification report:\n', report)
