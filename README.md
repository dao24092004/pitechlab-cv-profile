# PITECH LAB — Dynamic Portfolio (CV Profile Platform)

Nền tảng portfolio động cho tổ chức **PITECH LAB / INNOTECH SOLUTIONS** — mỗi nhân sự có một trang hồ sơ riêng, quản trị viên nhập liệu và duyệt kích hoạt, khách truy cập chat và gọi video trực tiếp ngay trên trang.

**Stack:** React 19 · TypeScript · Vite 6 · Tailwind CSS · Framer Motion · three.js · STOMP/SockJS · WebRTC

---

## 📚 Tài liệu bàn giao

Bộ tài liệu đặc tả & hướng dẫn nằm trong [`docs/`](./docs/):

| # | Tài liệu | Nội dung |
|---|---|---|
| 00 | [docs/README.md](./docs/README.md) | Mục lục, tóm tắt hệ thống, bản đồ tài liệu ↔ mã nguồn |
| 01 | [Tổng quan hệ thống](./docs/01-TONG-QUAN-HET-THONG.md) | Kiến trúc, công nghệ, mô hình dữ liệu, phân quyền, luồng dữ liệu |
| 02 | [Đặc tả chức năng](./docs/02-DAC-TA-CHUC-NANG.md) | 21 tác vụ người dùng, quy tắc nghiệp vụ, ma trận truy vết |
| 03 | [Hướng dẫn cài đặt & sử dụng](./docs/03-HUONG-DAN-CAI-DAT-VA-SU-DUNG.md) | Cài đặt, cấu hình, deploy, hướng dẫn theo vai trò, xử lý sự cố |
| 04 | [Kiểm tra & bàn giao](./docs/04-KIEM-TRA-VA-BAN-GIAO.md) | Checklist nghiệm thu, 31 khiếm khuyết, kế hoạch khắc phục |
| 05 | [Phụ lục kỹ thuật](./docs/05-PHU-LUC-KY-THUAT.md) | Dependency, endpoint REST, topic STOMP, biến môi trường, quy trình phát triển |

---

## ⚡ Chạy nhanh (5 phút)

### 1. Yêu cầu

- Node.js ≥ 18 (khuyến nghị 20.x / 22.x)
- npm ≥ 9
- Backend API đang chạy (xem [mục 3 của tài liệu 03](./docs/03-HUONG-DAN-CAI-DAT-VA-SU-DUNG.md))

### 2. Cài đặt

```bash
git clone https://github.com/dao24092004/pitechlab-cv-profile.git
cd pitechlab-cv-profile
npm install
```

### 3. Cấu hình

Tạo file `.env.local` ở thư mục gốc:

```bash
cp .env.example .env.local
```

```ini
VITE_API_URL=http://localhost:8080
VITE_WS_URL=http://localhost:8080/ws
```

### 4. Chạy

```bash
npm run dev
```

Mở trình duyệt: **http://localhost:3000**

### 5. Build production

```bash
npm run build     # → dist/
npm run preview   # xem trước bản build
```

---

## 🗺️ Điều hướng nhanh theo vai trò

| Tôi là… | Đi tới đâu |
|---|---|
| **Người dùng cuối** | [Tài liệu 03 — mục 6.1](./docs/03-HUONG-DAN-CAI-DAT-VA-SU-DUNG.md) |
| **Quản trị viên** | [Tài liệu 03 — mục 6.2](./docs/03-HUONG-DAN-CAI-DAT-VA-SU-DUNG.md) |
| **Nhân sự (User)** | [Tài liệu 03 — mục 6.3](./docs/03-HUONG-DAN-CAI-DAT-VA-SU-DUNG.md) |
| **Người triển khai** | [Tài liệu 03 — mục 5](./docs/03-HUONG-DAN-CAI-DAT-VA-SU-DUNG.md) |
| **Người gặp sự cố** | [Tài liệu 03 — mục 7](./docs/03-HUONG-DAN-CAI-DAT-VA-SU-DUNG.md) |
| **Lập trình viên mới** | [Tài liệu 01](./docs/01-TONG-QUAN-HET-THONG.md) → [Tài liệu 05](./docs/05-PHU-LUC-KY-THUAT.md) |
| **Người nghiệm thu** | [Tài liệu 04](./docs/04-KIEM-TRA-VA-BAN-GIAO.md) |

---

## ✨ Tính năng chính

### Trang portfolio công khai (`/`)

- Hero với nền 3D `three.js`, avatar, họ tên, chức danh, kỹ năng, kinh nghiệm
- Section: Lộ trình nghề nghiệp · Dự án (so le) · Học vấn · Kỹ năng · Nghiên cứu · Thành tựu
- Đa ngôn ngữ Việt / Anh
- Chế độ sáng / tối
- **Xuất CV** ra PDF theo mẫu A4 (không cần backend)
- Trang cá nhân theo đường dẫn tổ chức: `/#/view/{rCode}/{lCode}/{dCode}/{profileId}`

### Bảng điều khiển quản trị (`/admin`)

12 tab: chat · profile · account · organization · projects · skills · experience · education · publications · events · cv-import · profile-list

- Quản lý toàn bộ hồ sơ, công khai (`activate`) hoặc xóa
- Cây tổ chức 3 cấp: **Vùng → Chi nhánh → Phòng ban**, sinh link chia sẻ tự động
- Nhập CV (`.pdf` / `.docx`) để tạo hồ sơ tự động bằng AI
- Chat realtime với khách truy cập

### Bảng điều khiển cá nhân (`/user`)

9 tab: chat · profile · change-password · projects · skills · experience · education · publications · events

- Tự quản lý hồ sơ của mình
- Xem & sao chép link chia sẻ
- Đổi mật khẩu

### Chat & gọi video (realtime)

- Chat qua **STOMP over SockJS** — khách ẩn danh, Admin phản hồi
- Gọi video **WebRTC peer-to-peer** — STOMP chỉ làm kênh báo hiệu
- Tự đồng bộ hồ sơ khi Admin cập nhật (`/topic/public/updates`)

---

## 📁 Cấu trúc mã nguồn

```
pitechlab-cv-profile/
├── App.tsx              # Định tuyến + ProtectedRoute
├── index.tsx            # Entry point
├── index.html           # Shell HTML (Tailwind CDN, importmap)
├── index.css            # Tailwind directives + utility 3D
├── vite.config.ts       # Cấu hình Vite, alias '@'
├── tailwind.config.js   # Theme
├── pages/               # Home · Admin · User · Login
├── components/          # 18 component tái sử dụng
├── services/api.ts      # Axios + portfolioService/adminService/userService
├── types/index.ts       # Interface TypeScript
├── utils/translations.ts# Bản dịch VI/EN
└── docs/                # 📚 Bộ tài liệu bàn giao
```

---

## 🔌 Tích hợp backend

Toàn bộ dữ liệu đến từ backend qua REST (`{VITE_API_URL}/api/v1`) và STOMP (`{VITE_WS_URL}`).

| Nhóm | Endpoint chính |
|---|---|
| Portfolio | `GET /portfolio`, `GET /admin/portfolio/{id}`, `GET /view/{r}/{l}/{d}/{id}` |
| Xác thực | `POST /auth/login`, `/auth/register`, `/auth/change-password` |
| Quản trị | `GET /admin/list?page=&size=`, `PUT /admin/profile`, `POST /admin/profile/{id}/activate` |
| Tổ chức | `GET/POST/PUT/DELETE /org/{regions,locals,departments}` |
| Người dùng | `GET/PUT /user/profile`, `GET/POST/PUT/DELETE /user/{projects,skills,...}` |
| Tệp tin | `POST /upload`, `POST /admin/import-cv` |

> Danh mục đầy đủ (46 endpoint + 7 topic STOMP) tại [Tài liệu 05 — mục 2 & 3](./docs/05-PHU-LUC-KY-THUAT.md).

---

## ⚠️ Lưu ý trước khi triển khai production

Đã ghi nhận 31 điểm cần xử lý trong [Tài liệu 04](./docs/04-KIEM-TRA-VA-BAN-GIAO.md). Ba điểm quan trọng nhất:

1. **🔴 Tailwind chưa được build** — thiếu dependency `tailwindcss` và `postcss.config.js`; toàn bộ style phụ thuộc CDN. Cần chuẩn hóa trước khi lên production. Chi tiết: [tài liệu 03 — mục 7.2](./docs/03-HUONG-DAN-CAI-DAT-VA-SU-DUNG.md).
2. **🔴 Không có xử lý 401 tập trung** — token hết hạn sẽ khiến người dùng bị "kẹt" trên màn hình lỗi.
3. **🟠 Cần TURN server cho gọi video** — hiện chỉ dùng STUN công cộng, hai bên cùng NAT ngăn chặn sẽ không kết nối được.

---

## 📄 Giấy phép & liên kết

- Mã nguồn: [github.com/dao24092004/pitechlab-cv-profile](https://github.com/dao24092004/pitechlab-cv-profile)
- Triển khai: [frontendcv.vercel.app](https://frontendcv.vercel.app)
- AI Studio app: [ai.studio/apps/drive/1UcWhDkru60nn-4OY0BjpOAtvnoL1h3Dj](https://ai.studio/apps/drive/1UcWhDkru60nn-4OY0BjpOAtvnoL1h3Dj)

> Repo gốc được tạo từ Google AI Studio. Lịch sử commit có dòng *"Initial clean commit (no secrets)"* — hãy rà soát lịch sử trước khi công khai rộng rãi.
