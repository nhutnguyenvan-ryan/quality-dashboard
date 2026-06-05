# Quality Performance Dashboard

## Cấu trúc thư mục
```
quality-performance-dashboard/
├── server.js           ← Node.js server (Render chạy file này)
├── package.json        ← Khai báo dependencies
├── .gitignore          ← Bỏ qua node_modules
└── public/
    ├── index.html      ← Dashboard chính
    └── staff_template.csv
```

---

## ✅ Hướng dẫn deploy lên GitHub + Render (Web Service)

### Bước 1 — Tạo GitHub Repository

1. Vào https://github.com → Đăng nhập
2. Click **"New repository"** (nút dấu `+` góc trên phải)
3. Đặt tên: `quality-performance-dashboard`
4. Chọn **Public** (bắt buộc để Render free tier nhận diện)
5. Click **"Create repository"**

### Bước 2 — Upload file lên GitHub

Cách đơn giản nhất (không cần Git):

1. Trong repo vừa tạo, click **"uploading an existing file"**
2. Kéo thả toàn bộ file/thư mục vào (giữ nguyên cấu trúc):
   ```
   server.js
   package.json
   .gitignore
   public/index.html
   public/staff_template.csv
   ```
   > ⚠️ Lưu ý: GitHub không cho upload thư mục rỗng. Cần upload `public/index.html` bằng cách tạo thư mục `public` thủ công:
   > - Click **"Create new file"**
   > - Đặt tên: `public/index.html`
   > - Dán nội dung file index.html vào
   > - Commit

3. Lặp lại tương tự cho các file còn lại
4. Click **"Commit changes"**

### Bước 3 — Deploy trên Render

1. Vào https://render.com → Đăng ký bằng GitHub
2. Click **"New +"** → chọn **"Web Service"**
3. Chọn repo `quality-performance-dashboard`
4. Cấu hình:

   | Trường | Giá trị |
   |--------|---------|
   | **Name** | quality-dashboard *(tùy ý)* |
   | **Region** | Singapore (gần nhất) |
   | **Branch** | main |
   | **Runtime** | Node |
   | **Build Command** | `npm install` |
   | **Start Command** | `node server.js` |
   | **Instance Type** | **Free** |

5. Click **"Create Web Service"**
6. Đợi 2–3 phút build xong → nhận URL dạng:
   ```
   https://quality-dashboard-xxxx.onrender.com
   ```

---

## ⚠️ Lưu ý Render Free Tier

- Server sẽ **"ngủ"** sau 15 phút không có request → lần đầu truy cập sau đó mất ~30 giây để khởi động lại
- Nếu cần luôn online, dùng gói **Starter** ($7/tháng) hoặc ping định kỳ bằng UptimeRobot (miễn phí)

---

## Cách dùng sau khi deploy

### Cập nhật data module
1. Export Google Sheets **"For Slide"** → CSV
2. Trên dashboard → **"Cập nhật data"** → chọn file CSV

### Cập nhật data nhân sự
1. Điền dữ liệu vào file `staff_template.csv` theo đúng format
2. Trang **"Theo nhân sự"** → **"Upload staff CSV"**
