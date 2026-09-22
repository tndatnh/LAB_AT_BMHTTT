LAB 3 - An toàn thông tin

Họ tên: Nguyễn Hoàng Tiến Đạt
MSSV: 1150080011
Lớp: 11CNPM1

Môi trường dùng:

Chạy trên VMware Workstation Pro 26H1, VM Windows 11 25H2 (build 26200.9445,
đã update KB5124008). Trước khi bắt đầu có tạo snapshot LAB3_CLEAN_20260914 

Các công cụ cài theo đúng version đề bài yêu cầu:
- Sysmon 15.22
- Autoruns 14.3
- Process Explorer 17.14
- Wireshark 4.6.8 (kèm Npcap)
- Python 3.14.7

Gói dữ liệu LAB3_Threats_Assets.zip 

Dựng môi trường:

Network Adapter để Host-only theo yêu cầu đề 

Baseline - lấy thông tin OS, trạng thái Defender, firewall, network config, danh sách process đang chạy

Các tình huống đã làm:

- Baseline: xong
- TH1 - Risk register + phân loại nguồn đe dọa: xong
- TH2 - EICAR test Defender: xong
- TH3 - Tấn công mật khẩu, log đăng nhập: xong

Kết quả từng phần:

TH1: Làm risk register với 4 tài sản chính (tài khoản, dữ liệu C:\LAB3, dịch vụ
giả lập, email/người dùng), map đủ Asset → Vulnerability → Threat → Risk → Control.
5 tình huống phân loại xong, có giải thích lý do chọn từng nhóm. PASS.

TH2: Defender chặn file EICAR ngay lúc ghi, thấy trong Protection history lẫn
Get-MpThreatDetection, không có đụng gì tới việc tắt bảo vệ hay tạo exclusion.
RealTimeProtectionEnabled = True suốt. PASS.

TH3: Sinh được cả 3 loại event 4624 (login đúng), 4625 (login sai, mình cố tình
gõ sai 2 lần), 4648 (do dùng runas với credential khác). Sau khi đổi mật khẩu thử
lại mật khẩu cũ thì fail hẳn (4625), mật khẩu mới thì login được bình thường. PASS.

Lỗi gặp phải trong lúc làm:

Lỗi lớn nhất là ở bước cài Python/Wireshark - vì VM để Host-only nên không có mạng,
chạy lệnh Test-NetConnection bị báo 'Name resolution failed'. Sau đó chuyển sang
NAT, shut down hẳn VM (không phải restart Windows suông) rồi mở lại thì mới nhận
được IP mới, ping google.com được.

Xong vụ mạng thì tới lượt winget báo lỗi:
'Failed when searching source: msstore - 0x8a15005e: The server certificate did
not match any of the expected values'
Máy vẫn tìm thấy đúng gói ở nguồn "winget" nên mình thêm cờ --source winget vào
lệnh cài là chạy được, không cần sửa gì thêm.

Sau khi cài xong hết công cụ thì đổi Network Adapter về lại Host-only trước
khi bắt đầu baseline, tránh traffic thử nghiệm sau này bị lẫn ra ngoài mạng thật.

