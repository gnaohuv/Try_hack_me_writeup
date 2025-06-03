<div align="center">
    <h1>🪨 TryHackMe Whiterose Writeup 🧱 </h1>
</div>


## 🚀1. Khởi động target
![Start target](Images/1.png)

## 🔍2. Recon

```bash
rustscan -a 10.10.190.125 --ulimit 5000
```
![Rustscan](Images/2.png)

- Sử dụng `rustscan` quét cổng, nhận được một số cổng mở.

![Rustscan](Images/3.png)

- Thêm vào đó, dựa trên các gợi ý của để bài, truy cập tới port `4040`.

![Hoang](Images/4.png)

- Có gì đó trên port 9009, truy cập trực tiếp trên browser bị từ chối.

![Hoang](Images/5.png)

![Hoang](Images/6.png)

## 🔑3. Khai thác

- Truy cập bằng `nc`, nhận được một giao diện như sau.

![Hoang](Images/7.png)

- Nhập thử một số lệnh cơ bản, tuy nhiên dịch vụ này có vẻ liên quan đến việc lấy lại `certificate` và `private key`.

![Hoang](Images/8.png)

- Có vẻ ta có thể đăng nhập thông qua port `54321` sử dụng lệnh 
```bash
socat stdio ssl:10.10.190.125:54321,cert=cert,key=key,verify=0
```

- Nhưng trước tiên cần có `cert` và `key`.

![Hoang](Images/9.png)

- Thành công lấy được `private key` và `certificate` từ giao diện kể trên.

![Hoang](Images/10.png)

![Hoang](Images/11.png)

![Hoang](Images/12.png)

- Sau khi đã có đầy đủ thông tin, đăng nhập thành công.

![Hoang](Images/13.png)

- Từ đây, ta có thể lấy được mật khẩu `SSH` của user `Barney`

```
Password hint: d1ad7c0a3805955a35eb260dab4180dd (user = 'Barney Rubble')
```

![Hoang](Images/14.png)

- Đăng nhập và thành công lấy được flag đầu tiên. 🚩🚩🚩

![Hoang](Images/15.png)

![Hoang](Images/16.png)

- Kiểm tra `sudo -l`, ta được run file `/usr/bin/certutil` qua root mà không cần mật khẩu

![Hoang](Images/17.png)

- File này cho phép ta lấy được `cert` và `key` của user `fred`, tương tự như với `barney`.

![Hoang](Images/18.png)

![Hoang](Images/19.png)

- socat đến port `54321`, thành công lấy được mật khẩu của `fred`

![Hoang](Images/20.png)

![Hoang](Images/21.png)


- Đăng nhập thành công qua SSH, lấy được flag tiếp theo

![Hoang](Images/22.png)

![Hoang](Images/23.png)

![Hoang](Images/24.png)

- Tiếp tục kiểm tra `sudo -l` trên tài khoản này.

![Hoang](Images/25.png)

- Ta có quyền truy cập vào các file pass.txt dưới dạng base64 và base32.

![Hoang](Images/27.png)

- Lấy được mật khẩu dưới dạng này, tiến hành giải mã, thu được một mã hash

![Hoang](Images/28.png)

![Hoang](Images/29.png)

- Đây là mã hash dạng MD5

![Hoang](Images/30.png)

- Crack hash thành công, thu được mật khẩu `flintstonesvitamins`.

![Hoang](Images/31.png)

- Chuyển sang tài khoản root thành công và lấy được flag.

![Hoang](Images/32.png)

=> Hoàn thành bài lab 🔥🔥🔥

![Hoang](Images/33.png)