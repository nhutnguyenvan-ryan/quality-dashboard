# ShiftIQ – Workforce Scheduling

Ứng dụng tối ưu lịch làm việc (HC Order) theo ngày, giờ, ca.

---

## 🚀 Hướng dẫn Deploy lên Render.com (miễn phí)

### Bước 1 — Tạo Google OAuth App

1. Vào [https://console.cloud.google.com](https://console.cloud.google.com)
2. Tạo project mới (hoặc dùng project có sẵn)
3. **APIs & Services → OAuth consent screen**
   - User type: External → Create
   - Điền App name: `ShiftIQ`, email hỗ trợ → Save
4. **APIs & Services → Credentials → Create Credentials → OAuth client ID**
   - Application type: **Web application**
   - Authorized redirect URIs: `https://TEN-APP-CUA-BAN.onrender.com/auth/google/callback`
   - *(Điền tạm, cập nhật lại sau khi có URL Render)*
5. Copy **Client ID** và **Client Secret** — dùng ở Bước 4

---

### Bước 2 — Push lên GitHub

```bash
# Trong thư mục shiftiq/
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/USERNAME/shiftiq.git
git push -u origin main
```

---

### Bước 3 — Tạo Web Service trên Render

1. Vào [https://render.com](https://render.com) → **New → Web Service**
2. Kết nối GitHub repo `shiftiq`
3. Cấu hình:
   - **Name**: `shiftiq` (hoặc tên bạn muốn)
   - **Runtime**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Plan**: Free
4. Bấm **Create Web Service** — Render sẽ deploy lần đầu

> URL app sẽ là: `https://shiftiq.onrender.com` (hoặc tên bạn đặt)

---

### Bước 4 — Tạo PostgreSQL Database (miễn phí)

1. Render Dashboard → **New → PostgreSQL**
2. **Name**: `shiftiq-db`, **Plan**: Free → Create
3. Sau khi tạo xong, vào **shiftiq-db → Info → Internal Database URL** → copy

---

### Bước 5 — Cấu hình Environment Variables

Vào Web Service `shiftiq` → **Environment** → thêm các biến:

| Key | Value |
|-----|-------|
| `GOOGLE_CLIENT_ID` | Client ID từ Bước 1 |
| `GOOGLE_CLIENT_SECRET` | Client Secret từ Bước 1 |
| `GOOGLE_CALLBACK_URL` | `https://TEN-APP.onrender.com/auth/google/callback` |
| `SESSION_SECRET` | Chuỗi ngẫu nhiên dài (vd: `shiftiq-abc123xyz456`) |
| `OWNER_EMAIL` | Gmail của bạn (tài khoản owner) |
| `DATABASE_URL` | Internal Database URL từ Bước 4 |

Sau khi lưu, Render tự **redeploy**.

---

### Bước 6 — Cập nhật Google OAuth Redirect URI

Quay lại Google Cloud Console → Credentials → OAuth Client ID → chỉnh **Authorized redirect URIs** thành URL thật:
```
https://TEN-APP.onrender.com/auth/google/callback
```

---

### ✅ Xong! Truy cập app

```
https://TEN-APP.onrender.com
```

Đăng nhập bằng Gmail đã đặt `OWNER_EMAIL` → role **OWNER**.

---

## 💻 Chạy local (development)

```bash
npm install
cp .env.example .env
# Điền các giá trị vào .env
node server.js
# Mở http://localhost:3000
```

---

## 📁 Cấu trúc project

```
shiftiq/
├── public/
│   ├── index.html      # Giao diện chính
│   ├── app.js          # Logic frontend + optimizer
│   └── style.css       # Styles
├── server.js           # Express server + API + OAuth
├── package.json
├── render.yaml         # Render deploy config
├── .env.example        # Template biến môi trường
└── .gitignore
```

---

## ⚠️ Lưu ý Free Tier Render

- Web Service free sẽ **sleep sau 15 phút** không có request → lần đầu truy cập chậm ~30 giây
- PostgreSQL free tồn tại **90 ngày** rồi bị xóa — export data định kỳ
- Để tránh sleep: dùng [UptimeRobot](https://uptimerobot.com) ping URL mỗi 10 phút (miễn phí)
