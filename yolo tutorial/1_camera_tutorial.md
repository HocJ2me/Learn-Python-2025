# Tutorial 1: Đọc Camera và Hiển Thị Hình Ảnh với OpenCV

## Mục tiêu
- Kết nối tới webcam (hoặc camera IP)  
- Hiển thị khung hình real‑time  
- Xử lý phím điều khiển để thoát  

## Chuẩn bị môi trường
```bash
# Python 3.x
pip install opencv-python
```
- Đảm bảo driver camera đã được cài

## Cấu trúc chương trình
1. Khởi tạo `VideoCapture`  
2. Vòng lặp đọc khung hình  
3. Hiển thị với `imshow`  
4. Bắt phím để thoát (`'q'`)  

## Mã nguồn
```python
import cv2

def main():
    cap = cv2.VideoCapture(0)  # 0 = thiết bị mặc định
    if not cap.isOpened():
        print("Không mở được camera")
        return

    while True:
        ret, frame = cap.read()
        if not ret:
            break

        cv2.imshow("Camera", frame)
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

if __name__ == "__main__":
    main()
```