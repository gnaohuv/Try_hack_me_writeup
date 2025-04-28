# 🌹 TryHackMe Whiterose Writeup 🌹

<div align="right">
    <em>📌 By: Hoàng Vũ :).</em>
</div>

## 1. Khởi động taget
![Start taget](Images/Start_machine.png)
## 2. Recon
- Sử dụng nmap quét cổng, thấy 2 cổng mở là `22` và `80`
    - Cổng `22` mở cho dịch vụ `SSH`
    - Cổng `80` chạy máy chủ web ` nginx 1.14.0 (Ubuntu)` qua http
