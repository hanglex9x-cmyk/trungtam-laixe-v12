# Trung Tâm Đào Tạo Lái Xe — V12.2 Final 24 Module
Kiểu 1: Google Sheet + Apps Script + GitHub. Dữ liệu nằm trong Google của khách, không phí duy trì.

## File bàn giao
- `Code.gs` — backend Apps Script (27 sheet, 12 cảnh báo, Có Bằng + Zalo, thu nhiều lần, phân quyền, backup)
- `index.html` — giao diện 1 file (Tailwind CDN + JS vanilla), chạy được cả trên Apps Script lẫn GitHub Pages
- `README.md` — file này
- `HuongDanSuDung.md`, `CanhBao.md` — xem mục tương ứng bên dưới gộp trong README này

## BƯỚC 1 — Tạo Sheet MỚI (bắt buộc ID riêng)
1. Vào drive.google.com → Mới → Google Sheets → đặt tên `QuanLy_TrungTamLaiXe_[TenKhach]`
2. Copy ID trong URL: `docs.google.com/spreadsheets/d/<ID_NAY>/edit`
3. TUYỆT ĐỐI không dùng ID của dự án/khách khác.

## BƯỚC 2 — Dán Code.gs
1. Trong Sheet: Tiện ích mở rộng → Apps Script
2. Xóa code mẫu, dán toàn bộ `Code.gs`
3. Thay dòng đầu: `const SHEET_ID = 'ID_USER_NHAP'` → ID ở Bước 1
4. Lưu → chọn hàm `initSheets` → Run → cấp quyền lần đầu → tạo đủ 27 sheet + header
5. (Demo bán hàng) chạy thêm `seedDemoData` để có số liệu mẫu K51/K52/K53
6. (Tự động) chạy `taoTrigger` 1 lần: backup 2h sáng + quét Zalo 7 ngày/6 tháng lúc 8h

## BƯỚC 3 — Deploy Web App
1. Deploy → New deployment → Web app
2. Execute as: Me • Who has access: Anyone with the link
3. Deploy → copy URL `.../exec`
4. Nếu sửa code sau này: Deploy → Manage deployments → sửa version → New version (URL giữ nguyên)

## BƯỚC 4 — Nối index.html
- Cách A (GitHub Pages): tạo repo GitHub RIÊNG cho khách này (hỏi tên repo trước khi đẩy), commit `index.html`, bật Pages. Mở file, thay `API_URL = 'https://script.google.com/macros/s/DEPLOY_ID/exec'` bằng URL /exec ở Bước 3.
- Cách B (chỉ Apps Script): trong project Apps Script tạo thêm file HTML tên đúng `index`, dán toàn bộ index.html. Lúc này mở trực tiếp URL /exec là thấy giao diện, không cần GitHub.

## BƯỚC 5 — Zalo OA (Module Có Bằng)
1. Trung tâm tạo Zalo OA + lấy access token
2. Dán token vào Sheet `CauHinh`: khóa `ZaloOAToken`
3. Học viên phải có SDT trong Sheet `HocVien`
4. Chưa có token/SĐT: hệ thống KHÔNG bịa “đã gửi” — ghi `Chua gui` + log trong `LichSu`, giao diện báo rõ.
5. 3 kịch bản tự động: 0 ngày chúc mừng + xin 5 sao, 7 ngày giới thiệu bạn bè tặng 500k, 6 tháng nhắc nâng hạng.

## Sử dụng hằng ngày
- Có bằng: mở tab “✅ HV Có Bằng + Zalo” → nhập MaHV + Ngày có bằng + Số bằng → nút XÁC NHẬN. HV tự chuyển XONG xanh lá, vào danh sách có bằng theo nhóm tháng.
- Thu tiền nhiều lần: Sheet HoaDon (Tong/DaThu/ConLai) + ChiTietThu (MaPhieu dạng HD001-L1, L2…), hệ thống chặn thu vượt.
- Đổi màu: tab Cài đặt → Color Picker → lưu vào CauHinh của chính dự án này.
- Phân quyền: Sheet PhanQuyen (Email | VaiTro: Sep/QL/KT/GV/HV). Mọi doPost đều ghi LichSu.

## Cảnh báo giới hạn Kiểu 1 (CanhBao)
- Phù hợp <1.000 records chính, vài người dùng đồng thời. Sheet chậm dần khi >10.000 dòng/sheet.
- Apps Script có quota UrlFetch/execution hằng ngày của Google; Zalo OA phụ thuộc token + chính sách Zalo.
- Khi vượt ngưỡng: nâng cấp Supabase (15–30tr) hoặc MySQL/VPS (20–50tr), giữ nguyên giao diện, chỉ thay API.
- Backup: `autoBackup()` copy Sheet lúc 2h sáng khi đã chạy `taoTrigger()`. Vẫn nên tải 1 bản Sheet về máy mỗi tháng.
