# Tutorial 2: Tích hợp YOLOv8 vào Python

## Mục tiêu
- Cài đặt và chạy YOLO để phát hiện đối tượng  
- Vẽ bounding box và nhãn lên khung hình  

## Chuẩn bị
```bash
# Python ≥3.8
pip install torch torchvision ultralytics opencv-python
```

## Tải model YOLO
```python
from ultralytics import YOLO

# Tải model pre-trained (nhỏ nhất yolov8n)
model = YOLO('yolov8n.pt')
```

## Thử trên ảnh tĩnh
```python
results = model('test.jpg')
results.show()  # hiển thị kết quả với hộp và nhãn
```

## Kết hợp với camera
```python
import cv2
from ultralytics import YOLO

model = YOLO('yolov8n.pt')
cap = cv2.VideoCapture(0)

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # Phát hiện
    results = model(frame, stream=True)
    for r in results:
        boxes = r.boxes.xyxy.cpu().numpy()
        confs = r.boxes.conf.cpu().numpy()
        cls_ids = r.boxes.cls.cpu().numpy()
        for box, conf, cls in zip(boxes, confs, cls_ids):
            x1, y1, x2, y2 = map(int, box)
            label = f"{model.names[int(cls)]} {conf:.2f}"
            cv2.rectangle(frame, (x1, y1), (x2, y2), (0,255,0), 2)
            cv2.putText(frame, label, (x1, y1-10),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0,255,0), 1)

    cv2.imshow("YOLO Detection", frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```