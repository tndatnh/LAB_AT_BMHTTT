# LAB 4 - KHẢO SÁT VÀ ĐÁNH GIÁ BỀ MẶT MẠNG BẰNG NMAP

## 1. Thông tin sinh viên

- Họ và tên: Nguyễn Hoàng Tiến Đạt
- MSSV: 1150080011
- Lớp: 11_ĐH_CNPM1
- Video: https://www.youtube.com/@tienatnguyenhoang7765

## 2. Môi trường

- Host: Windows 11 64-bit
- VMware Workstation
- Network: VMware Host-Only, `192.168.187.0/24`
- Kali Linux: `192.168.187.128`
- Metasploitable 2: `192.168.187.129`
- VMnet1: `192.168.187.1`

## 3. Nội dung thực hành

| STT | Nội dung | Lệnh |
|---|---|---|
| 1 | Kiểm tra IP | `ip -br addr` / `ifconfig` |
| 2 | Host Discovery | `sudo nmap -sn 192.168.187.0/24` |
| 3 | SYN Scan | `sudo nmap -sS 192.168.187.129` |
| 4 | Service Version | `sudo nmap -sV 192.168.187.129` |
| 5 | Aggressive Scan | `sudo nmap -A 192.168.187.129` |
| 6 | Kiểm tra MS17-010 | `sudo nmap -p 445 --script smb-vuln-ms17-010 192.168.187.129` |
| 7 | Xuất kết quả | `sudo nmap -sV 192.168.187.129 -oN ketqua.txt -oX ketqua.xml` |

## 4. Kiểm tra kết nối

Kali và Metasploitable 2 được đặt trong cùng mạng Host-Only.  
Kiểm tra kết nối bằng `ping`, kết quả đạt 0% packet loss.

## 5. Cấu trúc bài nộp

```text
LAB_AT_BMHTTT/
└── LAB4/
    ├── README.md
    ├── 11_DH_CNPM1-LAB4_1150080011-NguyenHoangTienDat.docx
   
