![image](https://github.com/user-attachments/assets/c276d5a6-14e5-4d20-ab16-db816edf8760)# Bài 4: Xây Dựng Giao Diện Web Hiện Đại Với Lưới 2 Cột (2xn)

## Mục tiêu
- Thiết kế giao diện web hiện đại dùng Bootstrap.
- Hiển thị video theo lưới 2 cột với thông tin chi tiết bên dưới mỗi ô (vị trí ATM, trạng thái cảnh báo).

## Hướng dẫn

### 1. Tạo cấu trúc thư mục cho template và CSS
- Tạo thư mục `templates`.
- Tạo thư mục `static/css`.

### 2. Tạo file `templates/index.html`
```html
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Giám Sát ATM</title>
  <!-- Bootstrap CSS -->
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <!-- CSS Tùy chỉnh -->
  <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
</head>
<body>
  <div class="container-fluid">
    <div class="row">
      <!-- Cột hiển thị video (chiếm 9/12) -->
      <div class="col-md-9">
        <h1 class="mt-4 mb-4 text-center">Giám Sát ATM</h1>
        <div class="row">
          {% for i in range(num_streams) %}
          <div class="col-md-6 mb-4">
            <div class="card video-card">
              <img src="{{ url_for('video_feed', stream_id=i) }}" class="card-img-top" alt="Video Stream {{ i }}">
              <div class="card-body p-2">
                <h5 class="card-title">Vị trí: ATM {{ i+1 }}</h5>
                <p class="card-text">
                  Trạng thái: <span class="alert-state">Bình thường</span>
                </p>
              </div>
            </div>
          </div>
          {% endfor %}
        </div>
      </div>
      <!-- Cột điều khiển (chiếm 3/12) -->
      <div class="col-md-3">
        <div class="card control-panel-panel mt-4 mb-4">
          <div class="card-header bg-primary text-white">
            <h4 class="mb-0">Điều Khiển Thiết Bị</h4>
          </div>
          <div class="card-body">
            <!-- Nhóm Điều Khiển Đèn -->
            <div class="control-section mb-3">
              <h5>Đèn</h5>
              <div class="btn-group btn-group-toggle d-flex" data-toggle="buttons">
                <a href="#" class="btn btn-success flex-fill">Bật đèn</a>
                <a href="#" class="btn btn-danger flex-fill">Tắt đèn</a>
              </div>
            </div>
            <hr>
            <!-- Nhóm Điều Khiển Còi -->
            <div class="control-section">
              <h5>Còi</h5>
              <div class="btn-group btn-group-toggle d-flex" data-toggle="buttons">
                <a href="#" class="btn btn-success flex-fill">Bật còi</a>
                <a href="#" class="btn btn-danger flex-fill">Tắt còi</a>
              </div>
            </div>
          </div>
        </div>
      </div> <!-- /cột điều khiển -->
    </div> <!-- /row -->
  </div> <!-- /container-fluid -->

  <!-- Bootstrap JS và phụ thuộc -->
  <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"></script>
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</body>
</html>
```

### 3. Tạo file CSS `static/css/style.css`
```css
/* Phông nền và font chữ chung */
body {
  background-color: #f8f9fa;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

/* Style cho card video */
.video-card {
  box-shadow: 0 2px 6px rgba(0,0,0,0.15);
  border-radius: 8px;
  overflow: hidden;
}
.video-card img {
  width: 100%;
  height: auto;
  max-height: 600px;
  object-fit: cover;
}
.video-card .card-body {
  padding: 0.5rem 1rem;
}

/* Style cho khung điều khiển */
.control-panel-panel {
  box-shadow: 0 2px 6px rgba(0,0,0,0.15);
  border-radius: 8px;
}
.control-section h5 {
  font-weight: bold;
  margin-bottom: 0.5rem;
}
.btn-group-toggle .btn {
  margin: 0 2px;
}

/* Style cho trạng thái cảnh báo bên dưới video */
.alert-state {
  font-weight: bold;
  color: #007bff;
}
```

## Bài tập cho học sinh:
- Thêm thông tin thời gian nhận diện hay hiển thị tên khu vực ATM.
- Tùy chỉnh lại màu sắc và bố cục giao diện theo ý tưởng riêng.
