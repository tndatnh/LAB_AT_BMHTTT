LAB 10 - TẤN CÔNG ĐẦU ĐỘC DNS CACHE TỪ XA: KAMINSKY ATTACK VỚI pfSense

1. Thông tin sinh viên
Họ và tên: Nguyễn Hoàng Tiến Đạt
MSSV: 1150080011
Lớp: 11_ĐH_CNPM1
Video: https://www.youtube.com/@tienatnguyenhoang7765

2. Môi trường
Host: Windows 11 64-bit, VMware Workstation 17 Pro
pfSense-Firewall: MGMT 192.168.187.254, DNSLAB (em4) 10.9.0.1/24, ATTACKNET (em5) 10.10.0.1/24, DMZ (em3) 172.16.6.1/24
DNS-SERVER / VICTIM (WEB-PROXY): 10.9.0.10/24 (Gateway 10.9.0.1), domain example.lab10.test
ATTACKER (Kali Linux): 10.10.0.200/24 (Gateway 10.10.0.1, LAN Segment: attack-net, Promiscuous Mode: Deny)
DMZ-SERVER (Ubuntu 64-bit): Web 172.16.6.10/24, Auth DNS Relay 172.16.6.20/24 (Gateway 172.16.6.1)

3. Nội dung thực hành
Tạo card mạng ATTACKNET (em5) trên pfSense gán IP 10.10.0.1/24, chuyển Kali sang phân vùng attack-net và tắt chế độ nghe lén promiscuous.
Cấu hình rule pfSense cho phép Kali gửi port 53 tới resolver 10.9.0.10 và port 5300 tới race relay 172.16.6.20, chặn toàn bộ ra ngoài.
Chạy authoritative DNS relay (auth_dns_relay_lab10.py) trên Ubuntu DMZ mở cửa sổ chạy đua TXID qua port 5300.
Chạy resolver mô phỏng (vuln_resolver_lab10.py) trên DNS-SERVER với cổng nguồn cố định 33333 để tái hiện lỗ hổng Kaminsky.
Thực hiện Baseline: phân giải tên miền www.example.lab10.test
 trả về web gốc 172.16.6.10.
Chứng minh tính Remote: bật tcpdump trên Kali xác nhận không bắt được gói tin giữa resolver và authoritative server.
Thực hiện tấn công Kaminsky: Kali chạy evil DNS, fake web và kaminsky_remote_lab10.py gửi truy vấn tên miền con ngẫu nhiên để đầu độc bản ghi NS và glue A.
Kiểm tra kết quả: truy vấn tên miền trên Victim bị trỏ sang IP 10.10.0.200 của hacker và mở ra trang web giả mạo.
Khôi phục và gia cố: tắt resolver mô phỏng, chạy lại BIND9 với tính năng ngẫu nhiên hóa cổng nguồn UDP để chống tấn công.

4. Kiểm tra kết nối
Máy thật truy cập WebGUI pfSense thành công qua IP https://192.168.187.254
.
pfSense kết nối thông suốt với Kali và các máy chủ theo đúng các rule được cấp phép.
Baseline phân giải tên miền www.example.lab10.test
 trả về đúng IP web gốc 172.16.6.10.
Kali bắt gói ghi nhận 0 packets captured, chứng minh attacker không thể nghe lén lưu lượng DNS nội bộ.
Tấn công Kaminsky thành công: Kali đoán trúng TXID, resolver ghi nhận log HIT và CACHE nạp NS độc hại.
Victim bị đầu độc thành công: dig trả về 10.10.0.200 và curl hiển thị giao diện fake web của attacker.
Khôi phục thành công: BIND9 hoạt động lại bình thường và tự động đổi cổng nguồn UDP liên tục.

5. Cấu trúc bài nộp LAB_AT_BMHTTT/ └── LAB10/ ├── README.md └── 11_DH_CNPM1-Lab10_1150080011-NguyenHoangTienDat.docx
