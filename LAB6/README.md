
---

# LAB 6 - XÁC THỰC ACTIVE DIRECTORY VÀ CHÍNH SÁCH TRUY CẬP TRÊN pfSense

### 1. Thông tin sinh viên
- Họ và tên: Nguyễn Hoàng Tiến Đạt
- MSSV: 1150080011
- Lớp: 11_ĐH_CNPM1
- Video: https://www.youtube.com/@tienatnguyenhoang7765

### 2. Môi trường
- Host: Windows 11 64-bit
- VMware Workstation
- pfSense-Firewall:
  - WAN (`em0`): NAT (VMnet8)
  - MGMT (`em1`): `192.168.187.254/24` (VMnet1)
  - LAN_DOMAIN (`em2`): `10.6.0.1/24` (LAN Segment: `lab6-lan`)
  - DMZ (`em3`): `172.16.6.1/24` (LAN Segment: `lab6-dmz`)
- DC01: Windows Server 2025 (`10.6.0.10/24`, Gateway `10.6.0.1`)
- Domain: `lab6.test`

### 3. Nội dung thực hành
- Tạo 4 card mạng cho pfSense trên VMware và gán IP từng interface qua console.
- Cài đặt Active Directory Domain Services trên Windows Server 2025, tạo domain `lab6.test`.
- Tạo OU `LAB6-Users` và 3 tài khoản: `sv_a`, `sv_b`, `svc_pfsense_ldap`.
- Tạo rule Pass trên pfSense cổng LAN_DOMAIN để Windows Server ping thông gateway `10.6.0.1`.
- Cấu hình Authentication Server trên pfSense trỏ về DC qua LDAP (port 389, Base DN `DC=lab6,DC=test`).
- Thiết kế các rule cho Kịch bản 1 đến 5 (chặn web theo IP Alias, đặt lịch Schedule, cô lập DMZ).

### 4. Kiểm tra kết nối
- Máy thật vào được WebGUI pfSense qua IP `https://192.168.187.254`.
- pfSense và Windows Server kết nối thông mạng với nhau, kiểm tra ping đạt 0% packet loss.
- Đã lưu cấu hình LDAP Server `LAB6-AD` trên WebGUI pfSense.

### 5. Cấu trúc bài nộp
```text
LAB_AT_BMHTTT/
└── LAB6/
    ├── README.md
    └── 11_DH_CNPM1-Lab6_1150080011-NguyenHoangTienDat.docx
```