LAB 5 - CẤU HÌNH TƯỜNG LỬA pfSense
1. Thông tin sinh viên
Họ và tên: Nguyễn Hoàng Tiến Đạt
MSSV: 1150080011
Lớp: 11_ĐH_CNPM1
Video: https://www.youtube.com/@tienatnguyenhoang7765
2. Môi trường
Host: Windows 11 64-bit
Ảo hóa: VMware Workstation
pfSense CE 2.7.2:
WAN (em0): DHCP (VMware NAT / VMnet8)
LAN (em1): 10.0.0.1/8 (LAN Segment: LAN-Lab)
DMZ (em2): 172.16.0.1/16 (LAN Segment: DMZ-Lab)
Domain Controller / Client: Windows Server 2025 (10.0.0.2/8, Gateway 10.0.0.1, DNS 10.0.0.2)
3. Nội dung thực hành
Cấu hình IP LAN 10.0.0.1/8 trên console pfSense, tắt DHCP server.
Cấu hình IP tĩnh 10.0.0.2/8 trên Windows Server 2025, truy cập WebGUI pfSense qua https://10.0.0.1.
Kích hoạt interface DMZ trên pfSense với IP 172.16.0.1/16.
Chuẩn hóa ruleset LAN: Tắt 2 rule mặc định (Default allow LAN to any IPv4 và IPv6).
Tình huống 1 (đang thực hiện): Tạo rule Block ICMP trên tab LAN để chặn ping ra ngoài.
4. Kết quả kiểm thử
Windows Server 2025 ping thông 10.0.0.1 và đăng nhập WebGUI pfSense thành công.
Cổng DMZ được kích hoạt và nhận diện đúng trên hệ thống.
Đã disable thành công 2 rule mặc định và tạo xong rule Block ICMP đầu tiên trên tab LAN.
5. Cấu trúc bài nộp
LAB_AT_BMHTTT/LAB3/(README.md và 11_DH_CNPM1-Lab3_1150080011-NguyenHoangTienDat.docx)
