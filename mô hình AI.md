[AI_Application_Time_Series_Forecasting_Report.md](https://github.com/user-attachments/files/21667147/AI_Application_Time_Series_Forecasting_Report.md)

# **Báo Cáo: Dự Báo Chuỗi Thời Gian Sử Dụng AI**

## **1. Giới thiệu về AI và ứng dụng trong dự báo chuỗi thời gian**

**Trí tuệ nhân tạo (AI)** là một lĩnh vực trong khoa học máy tính nghiên cứu về việc tạo ra các hệ thống có khả năng học hỏi và thực hiện các nhiệm vụ thông minh như con người. AI có thể được áp dụng trong nhiều lĩnh vực, từ xử lý ngôn ngữ tự nhiên, nhận dạng hình ảnh, cho đến phân tích và dự báo dữ liệu.

Một trong những ứng dụng phổ biến của AI là **dự báo chuỗi thời gian**, giúp dự đoán các giá trị trong tương lai dựa trên dữ liệu đã có. Dự báo chuỗi thời gian có thể được ứng dụng trong nhiều lĩnh vực như:

- **Kinh tế**: Dự báo giá cổ phiếu, tỷ giá tiền tệ.
- **Môi trường**: Dự báo nhiệt độ, lượng mưa, tốc độ gió.
- **Y tế**: Dự báo số lượng bệnh nhân, nhu cầu thuốc.
- **Giao thông**: Dự báo lưu lượng giao thông.

Trong báo cáo này, chúng ta sẽ tìm hiểu cách xây dựng một ứng dụng AI dự báo chuỗi thời gian bằng cách sử dụng các mô hình học sâu như **LSTM** (Long Short-Term Memory) và ứng dụng **Streamlit** để triển khai giao diện người dùng.

## **2. Các mô hình AI phổ biến trong dự báo chuỗi thời gian**

### **ARIMA (AutoRegressive Integrated Moving Average)**
ARIMA là một trong những mô hình thống kê đơn giản và phổ biến nhất trong dự báo chuỗi thời gian. ARIMA được sử dụng khi dữ liệu không có tính mùa vụ mạnh, chỉ có các yếu tố như xu hướng (trend) và sự thay đổi ngẫu nhiên.

- **Ưu điểm**: Dễ hiểu và triển khai.
- **Nhược điểm**: Không xử lý được dữ liệu phi tuyến tính và dữ liệu có mùa vụ mạnh.

### **LSTM (Long Short-Term Memory)**
LSTM là một loại mạng nơ-ron hồi tiếp (RNN) mạnh mẽ, có khả năng học các mẫu dài hạn trong chuỗi thời gian. Đây là mô hình phổ biến cho các dữ liệu chuỗi thời gian phức tạp.

- **Ưu điểm**: Xử lý tốt các chuỗi dài và dữ liệu phi tuyến tính.
- **Nhược điểm**: Yêu cầu dữ liệu huấn luyện lớn và tài nguyên tính toán mạnh mẽ.

### **Prophet**
Prophet là một mô hình dự báo chuỗi thời gian do Facebook phát triển, phù hợp cho dữ liệu có tính mùa vụ mạnh. Mô hình này dễ sử dụng và có khả năng xử lý các chuỗi thời gian với nhiều yếu tố như xu hướng, mùa vụ và ngày lễ.

- **Ưu điểm**: Dễ sử dụng và triển khai cho các dữ liệu có mùa vụ.
- **Nhược điểm**: Ít linh hoạt hơn so với các mô hình học sâu như LSTM.

### **XGBoost**
XGBoost là một thuật toán cây quyết định học máy mạnh mẽ, có thể xử lý cả dữ liệu tuyến tính và phi tuyến tính. Mặc dù không phải là mô hình chuỗi thời gian, nhưng XGBoost có thể được sử dụng hiệu quả khi kết hợp với các đặc trưng của chuỗi thời gian.

## **3. Quy trình xây dựng ứng dụng AI dự báo chuỗi thời gian**

### **Bước 1: Thu thập dữ liệu**
Dữ liệu là yếu tố quan trọng nhất trong bất kỳ dự án AI nào. Đối với bài toán dự báo chuỗi thời gian, dữ liệu có thể là giá cổ phiếu, nhiệt độ, tốc độ gió, hoặc bất kỳ thông tin nào thay đổi theo thời gian. Dữ liệu có thể được thu thập từ các nguồn công khai như API tài chính, dữ liệu thời tiết, hoặc từ các hệ thống quản lý.

Ví dụ: Sử dụng API **yfinance** để thu thập dữ liệu cổ phiếu hoặc **OpenWeather** cho dữ liệu thời tiết.

```python
import yfinance as yf

# Lấy dữ liệu cổ phiếu của Apple trong 5 năm
data = yf.download('AAPL', start='2016-01-01', end='2021-01-01')
```

### **Bước 2: Tiền xử lý dữ liệu**
Tiền xử lý dữ liệu bao gồm các công việc như:
- Điền giá trị thiếu (missing values).
- Chuẩn hóa dữ liệu (scaling) để các giá trị không bị lệch do sự khác biệt về đơn vị.
- Chuyển đổi dữ liệu thành dạng phù hợp với mô hình, như chuỗi thời gian với các bước trễ (lag steps).

Ví dụ:

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
scaled_data = scaler.fit_transform(data[['Close']])
```

### **Bước 3: Xây dựng mô hình**
Chúng ta sẽ chọn mô hình phù hợp với dữ liệu và yêu cầu. Ví dụ, nếu chúng ta sử dụng **LSTM**, mô hình sẽ được xây dựng như sau:

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import LSTM, Dense

model = Sequential()
model.add(LSTM(50, activation='relu', input_shape=(X_train.shape[1], X_train.shape[2])))
model.add(Dense(1))
model.compile(optimizer='adam', loss='mean_squared_error')

model.fit(X_train, y_train, epochs=100, batch_size=32)
```

### **Bước 4: Đánh giá mô hình**
Sau khi huấn luyện mô hình, chúng ta cần đánh giá mô hình với dữ liệu kiểm tra và tính toán các chỉ số như **MAE** (Mean Absolute Error), **RMSE** (Root Mean Squared Error) để xem mô hình có dự báo chính xác hay không.

### **Bước 5: Triển khai ứng dụng với Streamlit**
Streamlit là một thư viện Python đơn giản giúp xây dựng giao diện người dùng cho ứng dụng AI. Dưới đây là ví dụ về cách triển khai ứng dụng dự báo chuỗi thời gian sử dụng Streamlit:

```python
import streamlit as st
import matplotlib.pyplot as plt

# Tải dữ liệu
uploaded_file = st.file_uploader("Tải lên tệp CSV", type="csv")
if uploaded_file is not None:
    data = pd.read_csv(uploaded_file)
    st.write(data)

    # Dự báo
    forecast = model.predict(data)

    # Hiển thị đồ thị
    plt.plot(forecast)
    st.pyplot()
```

## **4. Các mô hình GitHub tham khảo**

### **1. [Time-Series-Forecasting-LSTM-Streamlit](https://github.com/harshitv804/Time-Series-Forecasting-LSTM-Streamlit)**
Ứng dụng sử dụng **LSTM** và **Streamlit** để dự báo chuỗi thời gian, có thể tải dữ liệu từ tệp CSV và hiển thị đồ thị dự báo.

### **2. [Multi-step Time Series Forecasting](https://github.com/00ber/multi-step-time-series-forecasting)**
Dự báo nhiều bước thời gian (multi-step forecasting) sử dụng **LSTM** cho dữ liệu thời tiết theo giờ tại Washington DC.

## **5. Kết luận và hướng phát triển**

Dự báo chuỗi thời gian sử dụng AI, đặc biệt là với các mô hình như **LSTM**, là một công cụ mạnh mẽ trong nhiều lĩnh vực. Tuy nhiên, để đạt được hiệu quả cao, cần:
- Dữ liệu chất lượng tốt.
- Mô hình phù hợp với đặc điểm dữ liệu.
- Tinh chỉnh siêu tham số và tối ưu hóa mô hình.

Hướng phát triển:
- Mở rộng ứng dụng AI trong các lĩnh vực khác như tài chính, y tế, và sản xuất.
- Nâng cao hiệu suất mô hình bằng các kỹ thuật học sâu khác như **Transformer**.
