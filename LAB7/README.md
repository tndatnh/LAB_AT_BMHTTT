LAB 7 - XÂY DỰNG CHÍNH SÁCH WEB PROTECTION TRÊN MÔ HÌNH pfSense VÀ WEB PROXY

Thông tin sinh viên
Họ và tên: Nguyễn Hoàng Tiến Đạt
MSSV: 1150080011
Lớp: 11_ĐH_CNPM1
Video: https://www.youtube.com/@tienatnguyenhoang7765

Môi trường
Host: Windows 11 64-bit
VMware Workstation
pfSense-Firewall:
WAN (em0): NAT (VMnet8)
MGMT (em1): 192.168.187.254/24 (VMnet1)
LAN (em2): 10.6.0.1/24 (LAN Segment: lab6-lan)
DMZ (em3): 172.16.6.1/24 (LAN Segment: lab6-dmz)
DC01: Windows Server 2025 (10.6.0.10/24, Gateway 10.6.0.1, DNS Server zone lab7.test)
DMZ-WEB: Ubuntu Server 24.04 (172.16.6.10/24, Gateway 172.16.6.1, Nginx chạy 13 site mô phỏng lab7.test)
WEB-PROXY: Ubuntu Server 24.04 (10.6.0.30/24, Gateway 10.6.0.1, DNS 10.6.0.10, Squid 3128, E2Guardian 8080, ClamAV)

Nội dung thực hành
Tạo 13 bản ghi DNS A trong zone lab7.test trên DC01 trỏ về máy chủ web DMZ 172.16.6.10.
Khởi tạo 13 virtual host Nginx trên DMZ-WEB tương ứng các nhóm web kiểm thử và tạo các file test 10MB.bin, 100MB.bin, 110MB.bin, eicar.com.txt.
Triển khai máy ảo WEB-PROXY, cài đặt và cấu hình Squid lắng nghe localhost cổng 3128, cấu hình E2Guardian lắng nghe cổng 8080.
Phân loại danh sách kiểm duyệt trên E2Guardian: đưa china, jobs, mideast, blockedgame vào danh sách chặn cứng (domainsnobypass); đưa weapons, politics, religion, streaming vào danh sách cảnh báo có mở tạm 300 giây (bypass); đưa normal, education vào danh sách ngoại lệ; tích hợp module quét virus ClamAV.
Tạo các Alias trên pfSense: LAB7_PROXY (10.6.0.30), LAB7_CLIENTS (10.6.0.10, 10.6.0.21), WEB_PORTS (80, 443).
Cấu hình tập luật Firewall LAN trên pfSense: cho phép DNS về DC01 cổng 53, cho phép Client kết nối Proxy cổng 8080, chặn trực tiếp cổng 80 và 443 từ Client để chống bypass, cho phép Proxy kết nối ra Web và DMZ.
Kiểm tra kết nối và chính sách
Khi chưa bật proxy trên trình duyệt Client, truy cập web bị pfSense chặn trực tiếp cổng 80/443.
Cấu hình proxy 10.6.0.30:8080 trên Client, truy cập normal.lab7.test thành công (HTTP 200 OK).
Truy cập china.lab7.test bị E2Guardian chặn hoàn toàn (Hard Block).
Truy cập weapons.lab7.test xuất hiện trang cảnh báo kèm tùy chọn mở tạm thời Bypass trong 5 phút.
Tải file eicar.com.txt từ download.lab7.test bị hệ thống phát hiện mã độc và chặn tải về.

Cấu trúc bài nộp LAB_AT_BMHTTT/ └── LAB7/ ├── README.md └── 11CNPM1_Lab7_1150080011_NguyenHoangTienDat.docx
