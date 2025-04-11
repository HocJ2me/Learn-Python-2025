# Bài 3: Tích Hợp Mô Hình Nhận Diện Người Bằng YOLO (Ultralytics YOLOv8)

## Mục tiêu
- Làm quen với việc tải và sử dụng mô hình YOLOv8 thông qua thư viện ultralytics.
- Xử lý hình ảnh và vẽ bounding box xung quanh đối tượng "person".

## Hướng dẫn

### 1. Cài đặt thư viện bổ sung
Cập nhật file **requirements.txt** thêm:
```txt
ultralytics
numpy
```
Sau đó chạy:
```bash
pip install -r requirements.txt
```

### 2. Tạo module nhận diện (detector)
Tạo thư mục `detectors` và file **ultralytics_detector.py**:
```python
# detectors/ultralytics_detector.py
import cv2
from ultralytics import YOLO

# Tải mô hình YOLOv8n (nếu chưa có, model sẽ tự tải xuống)
model = YOLO("yolov8n.pt")

def detect_people(frame):
    """
    Sử dụng YOLOv8 để phát hiện người trong frame.
    Vẽ bounding box cho đối tượng 'person' với độ tin cậy > 0.5.
    """
    results = model(frame)[0]
    if results.boxes is not None:
        for box in results.boxes:
            x1, y1, x2, y2 = box.xyxy[0].tolist()
            class_id = int(box.cls[0].item())
            confidence = box.conf[0].item()
            # Với dataset COCO, class id 0 là 'person'
            if class_id == 0 and confidence > 0.5:
                cv2.rectangle(frame, (int(x1), int(y1)), (int(x2), int(y2)), (0, 255, 0), 2)
                label = f"Person: {confidence:.2f}"
                cv2.putText(frame, label, (int(x1), int(y1)-10),
                            cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0,255,0), 2)
    return frame
```

### 3. Cập nhật hàm generator trong `app.py`
Chỉnh sửa hàm `gen(stream)` để tích hợp nhận diện:
```python
from detectors.ultralytics_detector import detect_people

def gen(stream):
    while True:
        frame = stream.get_frame()
        if frame is None:
            break
        # Phát hiện người trong frame
        frame = detect_people(frame)
        ret, jpeg = cv2.imencode('.jpg', frame)
        if not ret:
            continue
        yield (b'--frame\r\n'
               b'Content-Type: image/jpeg\r\n\r\n' + jpeg.tobytes() + b'\r\n')
```

### 4. Kiểm tra
- Chạy ứng dụng với lệnh: `python app.py`
- Mở trang video stream để kiểm tra bounding box xuất hiện trên người.

## Bài tập
- Quan sát kết quả và chỉnh sửa threshold (ngưỡng độ tin cậy) nếu cần.
- Thử hiển thị thêm thông tin, ví dụ: số lượng người phát hiện.
```
