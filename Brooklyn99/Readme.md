<div align="center">
    <h1>👮‍♂️ TryHackMe Brooklyn Nine Nine Writeup 🔫</h1>
</div>
<p align="center">
  <img src="Images/meme2.jpg" alt="HoangVu" width="350"/>
</p>

## 🚀 1. Khởi động target

![Start taget](Images/1.png)

## 🔍 2. Recon

- Taget là một trang web tĩnh chỉ bao gồm poster của show `Brooklyn Nine Nine` (Good show, highly recommend :)))

![Hoangdeptrai](Images/2.png)

- Sử dụng `nmap` quét cổng.

![Hoangdeptrai](Images/3.png)

- Đáng chú ý ở đây ngoài port `80` cho http và `22` cho ssh thì port `21` cho `ftp` cũng đang mở.
- Port `21` cho phép đăng nhập `ftp` bằng `anonymous` và còn tiết lộ một file tên `note_to_jake.txt`.

- Tìm thư mục ẩn cũng không phát hiện gì đặc biệt

![Hoangdeptrai](Images/6.png)

- Đoc mã nguồn trang web, phát hiện một đoạn note gợi ý về `steganography`.

![Hoangdeptrai](Images/4.png)

## 🔑3. Khai thác sâu hơn

- Đăng nhập thành công và tải file `note_to_jake.txt` qua `ftp`.

![Hoangdeptrai](Images/5.png)

- Đây là đoạn note của Amy cho Jake về việc mật khẩu của ông nầy quá yếu => có thể bruteforce ??.

- Vì ta được gợi ý ở trên có liên quan đến `steganography` mà trang web trên chỉ có đúng một hình ảnh là poster kể trên, thử tải về và tìm tin giấu trong ảnh.

![Hoangdeptrai](Images/7.png)

- Có vẻ nó cần mật khẩu để giải mã. 

![Hoangdeptrai](Images/8.png)

- Sử dụng `stegcracker` để crack password

```bash
stegcracker brooklyn99.jpg /usr/share/wordlists/rockyou.txt
```

![Hoangdeptrai](Images/9.png)

- Crack thành công, đọc file `brooklyn99.jpg.out` để xem kết quả.

![Hoangdeptrai](Images/10.png)

=> Thành công lấy được mật khẩu của Holt.


</div>
<p align="center">
  <img src="Images/meme4.png" alt="HoangVu" width="300"/>
</p>

- Tuy nhiên đừng quên lời nhắc của Amy phía trên, ta có thể khai thác thêm mật khảu của Jake bằng cách brute force.

- Có thể đoán được đây là mật khẩu `ssh` vì vậy vét cạn thông qua dịch vụ này sử dụng `hydra`:

```bash
hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://10.10.243.174 -V 
```

![Hoangdeptrai](Images/12.png)

=> Thành công lấy được mật khẩu của cả Jake.

</div>
<p align="center">
  <img src="Images/meme1.jpg" alt="HoangVu" width="250"/>
</p>

- Đăng nhập `ssh` thành công thông qua tài khoản captain Holt.

![Hoangdeptrai](Images/11.png)

- Thành công lấy được user flag 🚩🚩🚩.

![Hoangdeptrai](Images/13.png)

![Hoangdeptrai](Images/14.png)

- Nâng quyền root để đọc được flag thứ hai. Kiểm tra các lệnh có thể chạy với root mà không cần mật khẩu bằng `sudo -l`

![Hoangdeptrai](Images/15.png)

- Có thể chạy nano, thử kiếm tra trên https://gtfobins.github.io/ xem có lệnh leo quyền không.

![Hoangdeptrai](Images/16.png)

- Làm theo hướng dẫn, chạy `sudo nano`, sau đó `ctrl+R và ctrl+x` rồi nhập lệnh `reset; sh 1>&0 2>&0`.

![Hoangdeptrai](Images/17.png)

- Thành công vào được root bên trong `nano`.

![Hoangdeptrai](Images/18.png)

- Cuối cùng là đọc file `root.txt` và hoàn thành root flag 🚩🚩🚩

![Hoangdeptrai](Images/19.png)

- Hoàn thành bài lab 🔥🔥🔥

![Hoangdeptrai](Images/20.png)

</div>
<p align="center">
  <img src="Images/meme3.jpg" alt="HoangVu" width="450"/>
</p>
