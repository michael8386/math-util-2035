# 📋 BÁO CÁO TIẾN ĐỘ & TÀI LIỆU REVIEW DỰ ÁN (PULL REQUEST REVIEW GUIDE)

> **Môn học:** SWP391 – Software Development Project  
> **Dự án:** EquiFlow — Racehorse Training & Stable Management System (Hệ thống Quản lý Huấn luyện & Chuồng trại Ngựa đua)  
> **Mã dự án (Project Code):** `RACEHORSE_TRAINING` (Project ID: 2)  
> **Sinh viên thực hiện:** Mai Quang Minh & Nhóm phát triển EquiFlow  
> **Kính gửi:** Thầy GIÁO LÀNG (Giảng viên Hướng dẫn môn học SWP391)  
> **Thời điểm cập nhật:** Tháng 10/2026  

---

## 📌 Lời Mở Đầu & Mục Đích Tạo PR

Kính chào Thầy GIÁO LÀNG,

File tài liệu này được đính kèm trực tiếp trong Pull Request gửi đến Thầy nhằm:
1. **Báo cáo bức tranh tổng thể về tiến độ dự án** sau khi hoàn thành giai đoạn cốt lõi (**Sprint 1, Sprint 2 & Sprint 3** tương ứng với **Flow 1, Flow 2 & Flow 3**).
2. **Cung cấp hướng dẫn chi tiết để Thầy có thể chạy thử (Quickstart Demo)** và kiểm tra đánh giá cả về giao diện (Frontend) lẫn nghiệp vụ hệ thống (Backend).
3. **Cung cấp tài khoản test, dữ liệu mẫu chuẩn (Canonical Horses)** và kịch bản demo mẫu giúp Thầy kiểm tra nhanh mà không mất nhiều thời gian cài đặt.
4. **Trình bày các vấn đề kỹ thuật / nghiệp vụ trọng tâm** và kính xin ý kiến nhận xét, góp ý và định hướng của Thầy cho các phân hệ tiếp theo (**Flow 4: Chuồng trại, Flow 5: Thi đấu, Flow 6: Trí tuệ nhân tạo AI**).

---

## 👥 1. Thông Tin Nhóm & Phân Công Nhiệm Vụ

| STT | Họ và Tên | Mã Sinh Viên | Vai Trò Chính | Phân Công Trách Nhiệm |
|:---:|---|:---:|:---:|---|
| 1 | **Mai Quang Minh** | *(Leader)* | Leader / Fullstack | Quản lý kiến trúc, Flow 1 (Auth, Hồ sơ ngựa), Flow 2 (Giáo án huấn luyện), tích hợp FE & BE |
| 2 | **** | **** | Core Developer | Phát triển phân hệ Y tế (Flow 3: Medical Record, Medical Lock, 2D Injury Map), tối ưu hóa API & DB |
| 3 | *(Thành viên 3)* | *(MSSV)* | Frontend / UI-UX | Xây dựng giao diện, CSS Modules, Design Tokens |
| 4 | *(Thành viên 4)* | *(MSSV)* | Backend / QA | Audit Trail, kiểm thử API, validation |
| 5 | *(Thành viên 5)* | *(MSSV)* | Tester / BA | Đặc tả yêu cầu, kiểm thử E2E Playwright, dữ liệu mẫu |

* **Kho mã nguồn Frontend:** [Racehorse_Training_Management_System_MT_FE](https://github.com/MichaelTran1226/Racehorse_Training_Management_System_MT_FE)
* **Kho mã nguồn Backend:** [Racehorse_Training_Management_System_MT_BE](https://github.com/MichaelTran1226/Racehorse_Training_Management_System_MT_BE)
* **Bảng tiến độ GitHub Project:** [EquiFlow Project Board](https://github.com/users/MichaelTran1226/projects/2)

---

## 🏗️ 2. Kiến Trúc Hệ Thống & Tech Stack Đã Được Phê Duyệt

Dự án tuân thủ nghiêm ngặt các quyết định kiến trúc (`DECISIONS.md` / `TECH_STACK.md`), đảm bảo mã nguồn tinh gọn, hiệu năng cao và tuân theo tiêu chuẩn Enterprise:

### 2.1. Phân hệ Frontend (FE)
- **Framework & Runtime:** React 19.2 + Vite 8.3 + TypeScript Strict (`~6.0.2`, target ES2022).
- **Routing:** `react-router-dom` v7.
- **Phong cách giao diện (UI/UX Styling):** Pure CSS + CSS Modules, triệt để sử dụng bảng mã token dùng chung (`tokens.css`). **Không sử dụng Tailwind CSS** hay các UI kit cồng kềnh bên ngoài nhằm tối ưu hóa độ sâu kiến trúc và khả năng kiểm soát CSS thuần của sinh viên.
- **Tiêu chuẩn giao diện:** 100% tiếng Anh (English Invariant) cho toàn bộ nhãn, thông báo lỗi, toast notification và dialog.
- **Chế độ chạy linh hoạt:** Hỗ trợ cả **Live API Mode** (kết nối NestJS BE) và **Mock Mode** (chạy độc lập qua bộ nhớ đệm giả lập trình duyệt, phục vụ demo tức thì).

### 2.2. Phân hệ Backend (BE)
- **Framework:** NestJS 11.x trên nền Node.js LTS (>= 20.19.0), TypeScript strict decorators.
- **Cơ sở dữ liệu & ORM:** PostgreSQL kết hợp Prisma ORM 6.x (hỗ trợ cả Supabase Cloud và PostgreSQL Local).
- **Xác thực & Phân quyền (Auth & RBAC):** 
  - JWT Stateless: Access Token có thời hạn 15 phút (lưu in-memory), Refresh Token 7 ngày (lưu HttpOnly Cookie).
  - RBAC nghiêm ngặt với 5 vai trò hệ thống, kiểm soát qua `JwtAuthGuard` và `RolesGuard`.
- **API Specification:** Chuẩn RESTful, tiền tố `/api`, tài liệu tương tác Swagger UI tại `/api/docs`.
- **Kiểm thử tự động:** Jest + Supertest với độ bao phủ (coverage) cao cho các nghiệp vụ lõi (Business Rules, Medical Lock, RBAC).

---

## 🎯 3. Tiến Độ Thực Hiện Các Luồng Nghiệp Vụ (Business Flows)

Hệ thống được thiết kế xoay quanh 6 luồng nghiệp vụ chính. Hiện tại, nhóm đã hoàn thành toàn diện **Flow 1, 2, 3**:

```
+---------------------------------------------------------------------------------------+
|                                    HỆ THỐNG EQUIFLOW                                  |
+---------------------------------------------------------------------------------------+
|  [FLOW 1: HỒ SƠ NGỰA]       -->   [FLOW 2: GIÁO ÁN HUẤN LUYỆN]  --> [FLOW 3: Y TẾ NGỰA] |
|  - Định danh RFID microchip       - Lập kế hoạch theo giai đoạn      - Bệnh án & đơn thuốc |
|  - Vòng đời 7 trạng thái          - Chặn bài tập (Medical Lock)      - Bản đồ chấn thương 2D |
|  - Chuyển nhượng sở hữu           - Đánh giá thể lực sau tập         - Khóa huấn luyện khẩn |
|  (Trạng thái: HOÀN THÀNH)         (Trạng thái: HOÀN THÀNH)          (Trạng thái: HOÀN THÀNH) |
+---------------------------------------------------------------------------------------+
|  [FLOW 4: CHUỒNG TRẠI]      -->   [FLOW 5: GIẢI ĐUA & THÀNH TÍCH] --> [FLOW 6: AI INSIGHTS]|
|  - Sơ đồ Stall & Dinh dưỡng       - Đăng ký tham gia giải            - Phân tích tải trọng  |
|  - Checklist công việc Groom      - Ghi nhận thành tích, giải thưởng - Dự báo nguy cơ chấn thương|
|  (Kế hoạch Sprint 4)              (Kế hoạch Sprint 5)                (Kế hoạch Sprint 6)     |
+---------------------------------------------------------------------------------------+
```

### Chi tiết các luồng đã hoàn thành (Ready for Defense):

### ✅ Flow 1: Quản lý Hồ sơ & Định danh Ngựa (Horse Profile & Governance)
- **Định danh tiêu chuẩn:** Áp dụng chuẩn mã RFID `^RFID-[A-Z0-9-]{4,28}$` và mã microchip độc nhất.
- **Vòng đời trạng thái (7 trạng thái):** `ACTIVE`, `IN_TRAINING`, `UNDER_OBSERVATION`, `INJURED`, `ISOLATED`, `RESTING`, `RETIRED`.
- **Thẩm quyền Club Manager:** Đăng ký ngựa mới, điều chuyển quyền sở hữu cho Chủ ngựa (`HORSE_OWNER`), vô hiệu hóa/cho ngựa nghỉ hưu an toàn dữ liệu (Soft-delete/Deactivate).

### ✅ Flow 2: Lập & Thực hiện Giáo án Huấn luyện (Training Plan & Workouts)
- **Wizard lập giáo án:** Huấn luyện viên trưởng (`HEAD_TRAINER`) xây dựng bài tập chi tiết theo từng cự ly, khối lượng, tốc độ mục tiêu và loại mặt sân (cỏ, cát, hỗn hợp).
- **Bất biến quy tắc y tế (`RULE-MED-01`):** Hệ thống tự động kiểm tra trạng thái sức khỏe của ngựa trước khi áp dụng giáo án. Nếu ngựa đang có lệnh **Khóa Huấn Luyện (Medical Lock)** từ Bác sĩ Thú y, hệ thống **chặn tuyệt đối** thao tác xếp lịch bài tập nặng hoặc thi đấu.

### ✅ Flow 3: Quản lý Y tế & Xử lý Chấn thương (Veterinary & Injury Management)
- **Bệnh án điện tử:** Bác sĩ thú y (`VETERINARIAN`) quản lý lịch sử khám, chẩn đoán lâm sàng, phác đồ điều trị và kê đơn thuốc.
- **Lệnh Khóa Huấn Luyện khẩn cấp (Emergency Medical Lock):** Thiết lập khóa với quyền ưu tiên cao nhất toàn hệ thống, tự động đồng bộ sang phân hệ huấn luyện (đã xử lý triệt để lỗi trùng lặp dữ liệu Idempotency).
- **Mô hình giải phẫu cơ xương 2D (2D Musculoskeletal Injury Map - SC-3.05):** Cho phép bác sĩ chuyển đổi lớp hiển thị Cơ (Muscle Layer) và Xương (Skeleton Layer), đánh dấu điểm chấn thương (Pins) trực quan trên mô hình giải phẫu, theo dõi tiến trình hồi phục theo từng giai đoạn và hiển thị thumbnail thu nhỏ trên thẻ bệnh án.

---

## ⚡ 4. Hướng Dẫn Cài Đặt & Chạy Thử Nhanh (Quickstart Guide)

Thầy có thể lựa chọn 1 trong 2 cách sau để kiểm tra dự án:

### 🌟 Cách 1: Chạy Frontend ở chế độ MOCK (Khuyên dùng để Thầy test giao diện ngay lập tức)
> *Chế độ này không yêu cầu cài đặt PostgreSQL hay khởi động Backend, dữ liệu mẫu đã được đồng bộ sẵn trong bộ nhớ.*

1. Mở thư mục Frontend:
   ```bash
   cd Racehorse_Training_Management_System_MT_FE
   npm install
   ```
2. Kiểm tra file `.env` đã có cấu hình:
   ```env
   VITE_USE_MOCK=true
   ```
3. Khởi chạy:
   ```bash
   npm run dev
   ```
4. Truy cập trình duyệt: **`http://localhost:5173`**

---

### 🚀 Cách 2: Chạy Fullstack (Cả Backend NestJS + Frontend React)

#### Bước 1: Khởi động Backend
```bash
cd Racehorse_Training_Management_System_MT_BE
npm install

# Cấu hình file .env:
# DATABASE_URL="postgresql://postgres:password@localhost:5432/equiflow_db?schema=public"
# DIRECT_URL="postgresql://postgres:password@localhost:5432/equiflow_db?schema=public"
# JWT_SECRET="equiflow_jwt_super_secret_key_2026"
# JWT_REFRESH_SECRET="equiflow_jwt_refresh_secret_key_2026"
# PORT=3000

# Sinh Prisma Client, khởi tạo CSDL và gieo dữ liệu mẫu:
npx prisma db push
npm run db:seed

# Chạy server ở chế độ phát triển:
npm run start:dev
```
- API Backend hoạt động tại: **`http://localhost:3000/api`**
- Tài liệu API Swagger UI xem tại: **`http://localhost:3000/api/docs`**

#### Bước 2: Khởi động Frontend
```bash
cd Racehorse_Training_Management_System_MT_FE
# Trong file .env, chuyển sang dùng Live API:
# VITE_API_URL=http://localhost:3000/api
# VITE_USE_MOCK=false

npm run dev
```
- Ứng dụng web mở tại: **`http://localhost:5173`**

---

## 🔑 5. Danh Sách Tài Khoản Kiểm Thử (Demo Accounts)

Hệ thống đã gieo sẵn (Seed) các tài khoản tương ứng với đầy đủ 5 vai trò theo bảng phân quyền (RBAC).

> **Mật khẩu dùng chung cho tất cả các tài khoản mẫu:** `EquiFlow@2026`  
> *(Hoặc tài khoản cá nhân của sinh viên: `Minhmaiki@gmail.com` / Mật khẩu: `123456`)*

| STT | Vai trò (Role) | Email Đăng Nhập | Mật Khẩu | Chức năng & Phạm vi kiểm tra chính |
|:---:|---|---|:---:|---|
| 1 | **Club Manager** | `manager@gmail.com` | `EquiFlow@2026` | Quản lý danh sách ngựa, chuyển nhượng quyền sở hữu, quản lý nhân viên, xem Audit Log |
| 2 | **Head Trainer** | `trainer@gmail.com` | `EquiFlow@2026` | Lập giáo án huấn luyện, kiểm tra cơ chế chặn Medical Lock, phân công bài tập |
| 3 | **Veterinarian** | `vet@gmail.com` | `EquiFlow@2026` | Hồ sơ bệnh án, kích hoạt/hủy Khóa Y Tế, thao tác Bản đồ chấn thương 2D |
| 4 | **Groom** | `groom@gmail.com` | `EquiFlow@2026` | Sơ đồ phân bổ chuồng trại (Stall Grid), danh sách công việc chăm sóc |
| 5 | **Horse Owner** | `owner@gmail.com` | `EquiFlow@2026` | Xem hồ sơ và tiến độ huấn luyện của ngựa thuộc sở hữu riêng |

---

## 🐎 6. Bộ 3 Chiến Mã Mẫu Dùng Để Kiểm Thử Kịch Bản (Canonical Demo Horses)

Để việc kiểm tra logic nghiệp vụ giữa các màn hình đạt tính nhất quán cao nhất, hệ thống đã thiết lập sẵn 3 chú ngựa chuẩn đại diện cho 3 kịch bản:

```
[HR-000001: Thunderbolt Swift]  ---> Trạng thái: ACTIVE / Không khóa y tế
                                     ==> Dùng để test Tạo Giáo Án Bình Thường (Flow 2)

[HR-000002: Northern Dancer]    ---> Trạng thái: INJURED / ĐANG BỊ MEDICAL LOCK
                                     ==> Dùng để test Cơ Chế Chặn Khóa Y Tế (RULE-MED-01)

[HR-000003: Shadowfax Wonder]   ---> Trạng thái: UNDER_OBSERVATION / Có vết thương
                                     ==> Dùng để test Bản Đồ Giải Phẫu Chấn Thương 2D (Flow 3)
```

1. **`HR-000001` — Thunderbolt Swift:** 
   - **Tình trạng:** Khỏe mạnh (`ACTIVE`), `isMedicalLocked: false`.
   - **Kịch bản test:** Huấn luyện viên trưởng có thể tạo mới và áp dụng giáo án huấn luyện bình thường mà không bị cảnh báo.
2. **`HR-000002` — Northern Dancer Legacy:**
   - **Tình trạng:** Chấn thương gân (`INJURED`), **`isMedicalLocked: true`**.
   - **Kịch bản test:** Huấn luyện viên trưởng khi vào lập giáo án và chọn ngựa này sẽ bị hệ thống **hiển thị cảnh báo đỏ và lập tức chặn** xếp lịch bài tập nặng, thể hiện đúng tính toàn vẹn của quy tắc `RULE-MED-01`.
3. **`HR-000003` — Shadowfax Wonder:**
   - **Tình trạng:** Đang hồi phục (`UNDER_OBSERVATION`), có ghi nhận chấn thương mô cơ và khớp.
   - **Kịch bản test:** Mở màn hình Bệnh án (`/health/records`) -> Nhấp vào tab Bản đồ chấn thương (`Injury Map`) -> Quan sát mô hình 2D hai lớp Cơ/Xương, các điểm ghim chấn thương và thanh tiến độ phục hồi.

---

## 🧪 7. Bằng Chứng Đảm Bảo Chất Lượng (Quality Gate & Test Evidence)

Nhóm đã thực hiện kiểm thử tự động toàn diện trước khi gửi PR này:

- **Backend Unit & Service Tests:**
  - Lệnh thực thi: `npm test`
  - Kết quả: **16/16 Test Suites Passed**, **108/108 Unit Tests Passed (100% Green)**.
- **Backend Build & Typecheck:**
  - Lệnh thực thi: `npm run build` (`nest build`)
  - Kết quả: Biên dịch thành công, không có lỗi kiểu dữ liệu (Exit code 0).
- **Frontend Typecheck & Production Build:**
  - Lệnh thực thi: `npm run build` (`tsc --noEmit && vite build`)
  - Kết quả: Biên dịch thành công trong ~315ms, 0 lỗi TypeScript (Exit code 0).
- **Frontend Linter:**
  - Lệnh thực thi: `npm run lint`
  - Kết quả: Hoàn toàn sạch linter warnings/errors.
- **Quy trình Git:** Toàn bộ tính năng được tách thành các nhánh riêng biệt (`feat/...`, `fix/...`) và mở từng PR độc lập có liên kết mã task rõ ràng.

---

## ❓ 8. Những Điểm Nhóm Kính Xin Ý Kiến Hướng Dẫn Của Thầy

Trong quá trình phân tích và hiện thực dự án, nhóm có một số thắc mắc và kính mong nhận được góp ý chuyên môn từ Thầy để chuẩn bị tốt nhất cho giai đoạn tiếp theo:

1. **Về Cơ Chế Khóa Y Tế (`RULE-MED-01`):**
   - Hiện tại hệ thống cho phép Bác sĩ Thú y kích hoạt và mở Khóa Huấn Luyện đơn phương dựa trên chẩn đoán y khoa. Thầy đánh giá quy trình này đã tối ưu chưa, hay cần bổ sung thêm bước phê duyệt xác nhận (Two-man rule) từ Quản lý Câu lạc bộ (`Club Manager`) trước khi mở khóa?
2. **Về Phạm Vi Triển Khai Flow 4 (Chuồng trại) & Flow 5 (Giải đua):**
   - Để đảm bảo đồ án vừa vặn với thời lượng môn học SWP391 mà không bị loãng phạm vi, Thầy khuyến nghị nhóm nên tập trung sâu vào **Checklist chăm sóc chuồng trại theo ca của Groom + Theo dõi định mức tiêu hao thức ăn**, hay nên mở rộng thêm phần quản lý mua bán vật tư tồn kho phức tạp?
3. **Về Phương Án Triển Khai Flow 6 (Trí Tuệ Nhân Tạo AI):**
   - Theo định hướng kiến trúc (`Flow6_AI_Architecture_Blueprint.md`), nhóm dự kiến triển khai tính năng:
     - Gợi ý giáo án dựa trên thuật toán phân tích tải trọng bài tập (Training Load Calculation).
     - Trợ lý ảo AI tóm tắt hồ sơ sức khỏe và giải đáp tra cứu dinh dưỡng ngựa.
   - Thầy có lời khuyên nào để phần AI này gây được ấn tượng tốt nhất với Hội đồng chấm điểm mà vẫn đảm bảo tính khả thi cao khi demo bảo vệ?
4. **Về Phong Cách Mã Nguồn & Cấu Trúc Dự Án:**
   - Kính mong Thầy xem qua cấu trúc thư mục của FE (Feature-based: `src/features/...`) và BE (Modular Domain-driven: `src/...`) xem đã đạt chuẩn Enterprise theo yêu cầu của Bộ môn Kỹ thuật Phần mềm chưa ạ.


## P/s
Nhóm chúng em bị vướng mắc khi trong việc chia task và các member không follow đúng workflow, đồng thời hiện project bị lỗi vặt cũng như bắt đầu đi sai định hướng ban đầu, mong Thầy cho chúng em lời khuyên.

---

## 🙏 Lời Cảm Ơn

Nhóm chúng em xin chân thành cảm ơn Thầy đã dành thời gian quý báu để xem qua mã nguồn và tài liệu này. Mọi ý kiến đóng góp của Thầy sẽ là định hướng vô cùng quan trọng giúp nhóm chúng em hoàn thiện sản phẩm một cách chỉn chu và đạt kết quả tốt nhất trong kỳ đồ án SWP391!

*Trân trọng,*  
**Sinh viên Mai Quang Minh, Trần Bùi Đạt Thành & Nhóm phát triển Dự án TMEC**
maiquangminh1711@gmail.com