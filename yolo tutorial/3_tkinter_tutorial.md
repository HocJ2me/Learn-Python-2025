# Tutorial 3: Xây dựng GUI với Tkinter

## Mục tiêu
- Hiểu cấu trúc cơ bản của Tkinter  
- Tạo window, label, button  
- Hiển thị video camera + YOLO trong GUI  

## Chuẩn bị
- Python (Tkinter tích hợp sẵn)  
- (Tuỳ chọn) `pip install pillow` để xử lý ảnh  

## Khởi tạo giao diện cơ bản
```python
import tkinter as tk

root = tk.Tk()
root.title("Camera App")
root.geometry("800x600")

lbl = tk.Label(root, text="Chưa có ảnh")
lbl.pack(pady=10)

btn = tk.Button(root, text="Bật camera")
btn.pack()

root.mainloop()
```

## Hiển thị khung hình camera
```python
import cv2
from PIL import Image, ImageTk

def start_camera():
    cap = cv2.VideoCapture(0)

    def update():
        ret, frame = cap.read()
        if ret:
            img = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
            im_pil = Image.fromarray(img)
            imgtk = ImageTk.PhotoImage(image=im_pil)
            lbl.imgtk = imgtk
            lbl.configure(image=imgtk)
        lbl.after(10, update)

    update()

btn.config(command=start_camera)
```

## Thêm YOLO vào GUI
```python
from ultralytics import YOLO

model = YOLO('yolov8n.pt')

def start_camera():
    cap = cv2.VideoCapture(0)

    def update():
        ret, frame = cap.read()
        if ret:
            # Phát hiện như trong tutorial 2
            results = model(frame, stream=True)
            # Vẽ hộp lên frame...
            # Chuyển sang ImageTk để hiển thị
            img = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
            im_pil = Image.fromarray(img)
            imgtk = ImageTk.PhotoImage(image=im_pil)
            lbl.imgtk = imgtk
            lbl.configure(image=imgtk)
        lbl.after(10, update)

    update()
```

## Bố cục nâng cao
- Thêm nút “Chụp ảnh” để lưu file  
- Dropdown (ComboBox) chọn model  
- Status bar hiển thị FPS  
