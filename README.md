# Hướng dẫn Deploy lên Render

## Cấu trúc file
```
quality-dashboard/
├── index.html          ← Dashboard chính
├── staff_template.csv  ← Template CSV nhân sự (tham khảo)
└── README.md           ← File này
```

---

## Deploy trên Render (Static Site) — Miễn phí

### Bước 1: Tạo GitHub repo
1. Vào https://github.com/new
2. Tạo repo mới (ví dụ: `quality-dashboard`)
3. Upload toàn bộ file trong thư mục này lên repo

### Bước 2: Deploy trên Render
1. Vào https://render.com → Đăng ký/Đăng nhập
2. Click **"New +"** → chọn **"Static Site"**
3. Kết nối GitHub repo vừa tạo
4. Cấu hình:
   - **Name**: quality-dashboard (hoặc tên bất kỳ)
   - **Branch**: main
   - **Root Directory**: *(để trống)*
   - **Build Command**: *(để trống)*
   - **Publish Directory**: `.` (dấu chấm)
5. Click **"Create Static Site"**
6. Đợi ~1-2 phút → nhận URL dạng `https://quality-dashboard-xxxx.onrender.com`

---

## Cách sử dụng Dashboard

### Cập nhật data (Module report)
1. Export Google Sheets sheet **"For Slide"** → **File → Download → CSV**
2. Trên dashboard, click nút **"Cập nhật data"** (góc trên phải)
3. Chọn file CSV vừa download
4. Dashboard tự động reload với data mới

### Xem báo cáo nhân sự
1. Chuẩn bị file CSV theo format mẫu trong `staff_template.csv`
   - Cột bắt buộc: `Agent Name`, `Module`, `QA Score`, `Total Errors`
2. Vào trang **"Theo nhân sự"** (sidebar trái)
3. Click **"Upload staff CSV"**
4. Chọn file CSV nhân sự

---

## Format CSV nhân sự (bắt buộc có các cột)
| Cột | Bắt buộc | Mô tả |
|-----|----------|-------|
| Agent Name | ✅ | Tên nhân sự |
| Module | ✅ | Tên module (Livestream, Listing, ...) |
| Queue | ✅ | Tên queue |
| Week | ✅ | Tuần (W21, W22, ...) |
| QA Score | ✅ | Điểm QA (%) |
| Total Errors | ✅ | Số lỗi |
| Tenure/Newbie | | Loại nhân sự |
| Notes | | Ghi chú |

---

## Kết nối Google Sheets (nâng cao)

Nếu muốn dashboard tự động lấy data từ Sheets mà không cần upload CSV:

1. Google Sheets → **File → Share → Publish to web**
2. Chọn sheet → Format **CSV** → **Publish**
3. Copy URL dạng: `https://docs.google.com/spreadsheets/d/.../pub?gid=...&single=true&output=csv`
4. Trong `index.html`, tìm dòng `function triggerUpload()` và thêm fetch tự động

---

## Lưu ý
- Dashboard chạy hoàn toàn trên browser, không có backend → data không lưu trên server
- Mỗi lần mở lại sẽ hiển thị data mẫu, cần upload lại CSV
- Để lưu data lâu dài: có thể dùng Render + backend (Node.js) hoặc lưu lên Google Sheets
