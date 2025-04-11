# Bài 1: Tạo Ứng Dụng Flask Đơn Giản ("Hello World")

## Mục tiêu
- Hiểu cách khởi tạo một ứng dụng web đơn giản bằng Flask.
- Làm quen với khái niệm route và cách hiển thị một trang web tĩnh.

## Hướng dẫn

### 1. Cài đặt môi trường
- Cài đặt Python 3 (khuyến nghị Python 3.7+).
- Tạo một thư mục dự án, ví dụ: `ATM_PYTHON_Basic`.

### 2. Tạo file requirements.txt
Tạo file **requirements.txt** với nội dung:
```txt
Flask
```
Cài đặt thư viện bằng lệnh:

```bash
pip install -r requirements.txt
```
### 3. Tạo file app.py
Tạo file app.py với nội dung sau:

```python
# app.py
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def index():
    return "Hello, World! Đây là ứng dụng Flask đầu tiên của em."

if __name__ == '__main__':
    app.run(debug=True)
```
### 4. Chạy và kiểm tra
Chạy lệnh: python app.py

Mở trình duyệt và truy cập http://127.0.0.1:5000

### Bài tập 
Tùy chỉnh thông điệp hiển thị trên trang chủ.
Thêm một route mới, ví dụ /about hiển thị thông tin cá nhân.


