LAB 8 - TẤN CÔNG DNS CỤC BỘ: DNS SPOOFING VÀ DNS CACHE POISONING VỚI pfSense

1. Thông tin sinh viên
Họ và tên: Nguyễn Hoàng Tiến Đạt
MSSV: 1150080011
Lớp: 11_ĐH_CNPM1
Video: https://www.youtube.com/@tienatnguyenhoang7765

2. Môi trường
Host: Windows 11 64-bit
VMware Workstation 17 Pro
pfSense-Firewall:
WAN (em0): NAT (VMnet8)
MGMT (em1): 192.168.187.130/24 (VMnet1)
LAN_DOMAIN (em2): 10.6.0.1/24 (LAN Segment: lan-domain)
DMZ (em3): 172.16.6.1/24 (LAN Segment: dmz-net)
DNSLAB (em4): 10.9.0.1/24 (LAN Segment: dns-lab)
DNS-SERVER & VICTIM (Ubuntu Server - WEB-PROXY):
DNS Resolver: 10.9.0.10/24 (Gateway 10.9.0.1)
Victim Client: 10.9.0.100/24 (Gateway 10.9.0.1)
Domain thử nghiệm: lab9.test, www.external.lab9.test
ATTACKER (Kali Linux):
IP: 10.9.0.200/24 (Gateway 10.9.0.1, LAN Segment: dns-lab)
Công cụ: Scapy, Wireshark, python3-http.server (port 80)
DMZ-SERVER (Ubuntu Server - Ubuntu 64-bit):
Web Server: 172.16.6.10/24 (Nginx port 80)
Delayed Authoritative DNS: 172.16.6.20/24 (auth_dns_delayed.py delay 1.5s, Gateway 172.16.6.1)

3. Nội dung thực hành
Thêm card mạng thứ 5 (em4) trên pfSense gán cho interface DNSLAB với IP 10.9.0.1/24 thuộc LAN Segment dns-lab.
Thiết lập 5 firewall rules trên pfSense để cô lập mạng DNSLAB: cho phép UDP/TCP 53 tới 172.16.6.20, cho phép TCP 80 tới 172.16.6.10, chặn truy cập tới LAN-DOMAIN 10.6.0.0/24, chặn tới MGMT 192.168.187.0/24 và chặn tất cả lưu lượng còn lại.
Cấu hình authoritative DNS server có độ trễ 1.5s (auth_dns_delayed.py) và Nginx web server trên phân vùng DMZ.
Thiết lập dịch vụ recursive caching DNS resolver trên máy DNS-SERVER lắng nghe tại 10.9.0.10 cổng 53.
Thực hiện Baseline: kiểm tra phân giải tên miền www.external.lab9.test trả về IP gốc 172.16.6.10 với Query time ~1500ms.
Thực hiện Task 1: mô phỏng chuyển hướng cục bộ bằng file /etc/hosts trên Victim trỏ www.lab9.test về 10.9.0.200 (Kali fake web).
Thực hiện Task 2: tấn công Direct DNS Spoofing bằng Scapy trên Kali mạo danh DNS-SERVER trả lời trực tiếp cho Victim.
Thực hiện Task 3: tấn công DNS Cache Poisoning mạo danh Authoritative server (172.16.6.20) gửi phản hồi giả tới resolver (10.9.0.10), đầu độc cache với IP 10.9.0.200 và TTL 120s.
Khôi phục hệ thống: khởi động lại dịch vụ DNS resolver để làm sạch cache, xác nhận kết quả trả về đúng 172.16.6.10.

4. Kiểm tra kết nối
Máy thật truy cập WebGUI pfSense thành công qua IP https://192.168.187.130 (hoặc https://172.16.6.1).
pfSense kiểm tra ping thông suốt tới tất cả các máy ảo trong lab: 172.16.6.10 (DMZ), 10.9.0.10 (DNS-SERVER), 10.9.0.200 (Kali) đều đạt 0% packet loss.
Baseline phân giải tên miền www.external.lab9.test thành công trả về 172.16.6.10.
Task 1 kiểm tra curl trỏ về web giả mạo 10.9.0.200 thành công, trong khi dig vẫn trả về 172.16.6.10.
Task 3 kiểm tra đầu độc cache thành công: dig lần 1 nhận 10.9.0.200, dig lần 2 nhận ngay 10.9.0.200 từ cache với thời gian đáp ứng 1ms.
Khôi phục thành công: sau khi xóa cache, kết quả phân giải trở lại chính xác 172.16.6.10 với thời gian truy vấn 1505ms.

5. Cấu trúc bài nộp
LAB_AT_BMHTTT/
└── LAB8/
    ├── README.md
    └── 11_DH_CNPM1-Lab8_1150080011-NguyenHoangTienDat.docx
