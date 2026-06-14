# DustSwap_daily
# HƯỚNG DẪN SỬ DỤNG TOOL DUSTSWAP BOT V3.9 (MULTI-THREADS)

1. YÊU CẦU HỆ THỐNG
- Cài đặt Node.js phiên bản mới nhất (v18 trở lên).
- Cài đặt các thư viện cần thiết bằng lệnh:
  npm install 

2. CÁC FILE ĐẦU VÀO (INPUT) - Cần chuẩn bị trước khi chạy
- privatekey.txt: Chứa danh sách Private Key ví (Mạng Base). Mỗi ví 1 dòng.
- proxy.txt: Chứa danh sách Proxy (HTTP/HTTPS). Mỗi dòng 1 proxy. 
             Định dạng: http://user:pass@ip:port hoặc http://ip:port
- user_agents.txt: Chứa danh sách User-Agent trình duyệt. Mỗi dòng 1 chuỗi.
- config.json: File cấu hình thông số chạy Tool.

3. CẤU HÌNH FILE config.json
{
  "ref_code": "DUST-KZSE2",       // Mã mời để nhận 500 Point bonus
  "threads": 5,                   // Số luồng chạy cùng lúc (Tùy cấu hình máy)
  "delay_between_actions": [3, 5], // Khoảng cách giữa các bước (Login, Checkin, Spin) - Giây
  "delay_between_accounts": [10, 30], // Khoảng cách giữa các tài khoản - Giây
  "rpc_url": "https://mainnet.base.org", // URL RPC mạng Base (Nên dùng Alchemy/QuickNode)
  "recipient": "0x72Bd4b89fFb1e1f48253b7a7a65739ff1E696442" // Địa chỉ ví nhận Check-in
}

4. CÁC FILE ĐẦU RA (OUTPUT) - Tool tự động tạo
- license.txt: Lưu License Key của bạn sau lần nhập đầu tiên.
- index.txt: Lưu số thứ tự ví đang chạy. Nếu Tool tắt đột ngột, lần sau sẽ chạy tiếp từ ví này.
- profiles.json: Lưu trữ toàn bộ dữ liệu ví (Địa chỉ, Point, Rank, Trạng thái Ref/Checkin).

5. QUY TRÌNH HOẠT ĐỘNG CỦA TOOL
Bước 1: Xác minh License & HWID thiết bị.
Bước 2: Khởi động đa luồng theo cấu hình.
Bước 3: Thực hiện luồng nhiệm vụ cho từng ví:
   -> Login (Lấy thông tin ví)
   -> Bind Ref (Nhập mã mời để lấy 500 Point)
   -> Check-in On-chain (Gửi giao dịch 0 ETH để kích hoạt tài khoản)
   -> Spin (Tự động quay thưởng tối đa 3 lần nếu có vé)
   -> Final Summary (Cập nhật Point và Rank chính xác nhất)
Bước 4: Lưu kết quả vào profiles.json và chuyển ví.
Bước 5: Sau khi chạy hết danh sách, Tool sẽ chờ đến 7:00 AM (Giờ VN) để tự động reset và chạy lại vòng mới.

6. LƯU Ý QUAN TRỌNG
- Phí Gas: Các ví BẮT BUỘC phải có một ít ETH mạng Base (khoảng 0.1$ - 0.5$ là đủ dùng lâu dài) để thực hiện Check-in và Spin.
- Proxy: Nếu Proxy lỗi, Tool sẽ tự động xoay vòng lấy Proxy tiếp theo để thử lại, đảm bảo không bị Unknown IP.
- Anti-Bot: Tool đã tích hợp sẵn cơ chế Delay ngẫu nhiên và ẩn các lỗi hệ thống để tránh bị dự án quét.

7. CÁCH CHẠY TOOL
Mở Terminal/Command Prompt tại thư mục Tool và gõ lệnh:
node index.js
==========================================================================
