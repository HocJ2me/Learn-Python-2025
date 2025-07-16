# Bài 2: Xây Dựng Ứng Dụng Streaming Video Đơn Giản

## Mục tiêu
- Sử dụng OpenCV để mở camera (hoặc stream từ URL) và gửi frame qua HTTP dưới dạng MJPEG.
- Tạo một route trong Flask để trả về video stream.

## Hướng dẫn

### 1. Cài đặt thư viện
Cập nhật file **requirements.txt**:
```txt
Flask
opencv-python
```
Sau đó chạy:
```bash
pip install -r requirements.txt
```

### 2. Tạo module Video Stream
Tạo thư mục `video` và tạo file **video_stream.py** với nội dung:
```python
# video/video_stream.py
import cv2

class VideoStream:
    def __init__(self, src=0):
        self.src = src
        self.cap = cv2.VideoCapture(src)

    def __del__(self):
        if self.cap.isOpened():
            self.cap.release()

    def get_frame(self):
        try:
            ret, frame = self.cap.read()
            if not ret:
                return None
            return frame
        except Exception as e:
            print("Lỗi khi đọc frame:", e)
            return None
```

### 3. Tạo route streaming video trong Flask
Cập nhật file **app.py** (hoặc tạo file mới nếu dự án bắt đầu từ Bài 1):
```python
# app.py
from flask import Flask, Response
from video.video_stream import VideoStream
import cv2

app = Flask(__name__)

# Khởi tạo đối tượng VideoStream với camera mặc định (index 0)
stream = VideoStream(0)

def gen(stream):
    """Đọc frame và trả về dữ liệu MJPEG."""
    while True:
        frame = stream.get_frame()
        if frame is None:
            break
        ret, jpeg = cv2.imencode('.jpg', frame)
        if not ret:
            continue
        yield (b'--frame\r\n'
               b'Content-Type: image/jpeg\r\n\r\n' + jpeg.tobytes() + b'\r\n')

@app.route('/')
def index():
    return "Trang chủ, hãy truy cập /video_feed để xem video stream."

@app.route('/video_feed')
def video_feed():
    return Response(gen(stream),
                    mimetype='multipart/x-mixed-replace; boundary=frame')

if __name__ == '__main__':
    app.run(debug=True)
```

### 4. Kiểm tra
- Chạy lệnh: `python app.py`
- Truy cập [http://127.0.0.1:5000/video_feed](http://127.0.0.1:5000/video_feed) để xem video stream.

## Bài tập
- Thử thay đổi nguồn video từ index 0 sang URL RTSP (nếu có).
```


