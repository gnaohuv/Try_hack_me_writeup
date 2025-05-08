<div align="center">
    <h1>🏔️ TryHackMe Lian_yu Writeup 🌊</h1>
</div>

## 🚀 1. Khởi động taget

![Start taget](Images/1.Taget.png)

- Phần cuối bài sẽ sử dụng địa chỉ sau (vì phải khởi động lại :(( )

![Start taget](Images/8.png)

## 🔍 2. Recon

- Sử dụng `nmap` quét mục tiêu

![Nmap](Images/2.namp.png)

- Kết quả trả về một số cổng mở, ngoài các cổng thường thấy như `21`, `22`, `80` thì còn có cổng `111` mở dịch vụ `RPC (Remote Procedure Call)`. Tuy nhiên ở bước này chưa thấy điều gì đặc biệt để khai thác.

- Truy cập vào trang web với giao diện `Arrowverse` (superhero universe ???)

![HoangVu](Images/3.arrow.png)

- Giao diện trang web khá đơn giản, chỉ là lời gới thiệu về địa danh `Lian Yu` được nhắc tới và những thứ tác giả dựa trên để tạo ra room này. Mã nguồn cũng không có gì đặc biệt.

- Bước tiếp theo, dùng `gobuster` quét vì khả năng cao sẽ có thư mục ẩn.

![HoangVu](Images/4.gobuster_island.png)

- Tìm được một thư mục với tên `island`. Chuyển hướng tới `island`, tuy vậy trang Web đang bị thiếu mất `Code Word` nào đó.

![HoangVu](Images/9.png)

- Xem mã nguồn thì `code word` này bị ẩn đi vì để màu là trắng, trùng với nền.

![HoangVu](Images/10.png)

- Tìm được `vigilante`, có vẻ là tên user hoặc mật khẩu nào đó.

![HoangVu](Images/5.vogi.png)

- Tuy nhiên với những thông tin tìm được, vẫn chưa thể khai thác thêm điều gì cũng như chưa submit thành công các câu hỏi, vậy nên tiếp tục quét thư mục ẩn.

![HoangVu](Images/6.2100.png)

- Phát hiện thư mục `2100`, submit thành công câu hỏi thứ nhất.

![HoangVu](Images/6.5.png)

- Chuyển hướng sang `2100`, tìm được một video nhưng không thể xem được.

![HoangVu](Images/11.png)

- Xem qua mã nguồn thì phát hiện một gợi ý liên quan đến `.ticket`, có vẻ là file ta cần tìm

![HoangVu](Images/12.png)

- Tiếp tục sử dụng `gobuster` để quét file ẩn với đuôi là `.ticket` qua lệnh sau:

```bash
gobuster dir -u 10.10.14.242/island/2100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .ticket
```

- Tìm được một file ẩn là `green_arrow.ticket`, submit thành công answer 2.

![HoangVu](Images/30.png)

- Truy cập file, nhận được một đoạn mã `RTy8yhBQdscX`

![HoangVu](Images/7.green_arrow.png)

- Sau khi tìm hiểu thì đây là đoạn mã `base58`, sử dụng `Cyberchef` để dịch lại.

![HoangVu](Images/7.base58.png)

- Được một đoạn mã là `!#th3h00d`, thử submit câu hỏi thứ 3 và thành công thì có vẻ đây là mật khẩu `FTP`

![HoangVu](Images/31.png)

- Tuy nhiên chưa tìm được tên user ứng với mật khẩu này. Sau một hồi thử thì có vẻ nó là `vigilante` mà ta tìm được ở trên.

## 🔑3. Khai thác sâu hơn

![HoangVu](Images/13.png)

- Đăng nhập, tìm được 3 file ảnh. Tải về và cùng xem có gì ẩn bên trong không.

- Vì thu được file dưới dạng `png` và `jpg` nên tôi thử sử dụng `steghide` để trích xuất dữ liệu ẩn trong các file ảnh này.

```bash
steghide extract -sf Leave_me_alone.png
```

- Tuy nhiên các file `png` thì không đúng định dạng được hỗ trợ còn file `aa.jpg` thì cần password

![HoangVu](Images/14.png)

- Đang không biết phải làm sao thì tôi kiểm tra thử `Hex table` của ảnh `Leave_me_alone.png`.

![HoangVu](Images/15.png)

- Phần Header có vẻ có gì đó hơi sai sai. Tìm kiếm trên `google`, tôi tìm được định dạng đúng của header này.

![HoangVu](Images/17.png)

- Sử dụng `hexedit` và chỉnh lại cho đúng

```bash
hexedit Leave_me_alone.png
```

![HoangVu](Images/16.png)

- Cuối cùng ta xem được ảnh hoàn chỉnh

![HoangVu](Images/18.png)

- Hóa ra `password` chính là `password` :(((

<p align="center">
  <img src="Images/password.jpg" alt="HoangVu" width="200"/>
</p>

- Có vẻ đây là mật khẩu của file `aa.jpg`, giải mã thử.

![HoangVu](Images/19.png)

- Thành công thu được file `ss.zip`, giải nén file này thu được 2 file là `passwd.txt` và `shado` (đoạn này tôi làm lại 2 lần nên file bị overwrite :( )

- File `passwd.txt` chứa một đoạn note mà tôi cũng chưa biết nó có s nghĩa gì :)))
![HoangVu](Images/21.png)

- File `shado` chưa mốt đoạn text, sau khi submit thử câu hỏi thì đó có vẻ là mật khẩu `SSH`.
![HoangVu](Images/20.png)

![HoangVu](Images/32.png)

- Tuy vậy chưa biết `username` ứng với mật khẩu này.

![HoangVu](Images/22.png)

- Theo gợi ý từ `passwd.txt` thử với `username` `Oliver`, tuy vậy có vẻ là không chính xác.

- Sau một hồi không biết username này là gì, thì tôi quay lại đăng nhập bằng `FTP` và phát hiện một `user` bên cạnh `vigilante` là `slade`.

![HoangVu](Images/23.png)

- Đăng nhập thử thì boommm💥, thành công.

![HoangVu](Images/24.png)

- Sau khi đăng nhập thành công, việc cần làm là kiếm flag 🚩, đầu tiên là `user.txt`

![HoangVu](Images/25.png)

![HoangVu](Images/33.png)

- Tiếp theo là root flag 🚩

- Như thường lệ, `sudo -l` xem có gì đặc biệt. Phát hiện `pkexec` có thể được chạy không password với quyền root.

- `pkexec` là một lệnh trên Linux dùng để chạy chương trình với quyền root — tương tự như `sudo`, nhưng tuân theo chính sách quyền kiểm soát của PolicyKit.

![HoangVu](Images/26.png)

- Tìm kiếm xem có thể leo quyền với lệnh này không trên https://gtfobins.github.io/

![HoangVu](Images/27.png)

- Chạy lệnh 

```bash
sudo pkexec /bin/sh
```

![HoangVu](Images/28.png)

- Thành công vào được root, thành công đọc được `root.txt`.

![HoangVu](Images/29.png)

![HoangVu](Images/34.png)

=> Hoàn thành bài lab 🔥🔥🔥

![HoangVu](Images/completed.png)