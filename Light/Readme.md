<div align="center">
    <h1>💡 TryHackMe Light Writeup 🗄️</h1>
</div>

## 🚀 1. Khởi động taget

![Start taget](Images/1.png)

## 🔍2. Recon

- Như thường lệ, quét `nmap` để phát hiện các cổng mở.

![Nmap](Images/4.png)

- Phát hiện cổng `22` cho SSH đang được mở.

![Nmap](Images/2.png)

- Dựa vào gợi ý của room, sử dụng `nc` truy cập với port `1337` và username là `smokey`.

![Nmap](Images/3.png)

- Ta nhận được một password, tuy nhiên dùng thử password này với `ssh` thì không truy cập được.

![Nmap](Images/5.png)

- Có lẽ mục tiêu khai thác tập trung vào việc sử dụng nc trên port `1337`.

## 🔑3. Khai thác

- Nhập thử các kí tự khác, nhận thấy có vẻ đầu vào giúp giao tiếp với database với một câu truy vấn SQL nào đó. Câu truy vấn có vẻ dạng : 
```
SELECT * FROM users WHERE username = '{user_input}' LIMIT 30; 
```

![hoang](Images/6.png)

- Bộ lọc lọc một số kí tự kết thúc sớm câu truy vấn, ví dụ `#`.

![hoang](Images/7.png)

- Và có vẻ lọc cả một số keyword nguy hiểm như `Union` hay `Select`.

![hoang](Images/8.png)

- Sử dụng một số cách như thay đổi các viết hoa và thường để bypass và thành công,

![hoang](Images/9.png)


- Sử dụng một số câu lệnh để khai thác thông tin qua lỗ hổng SQL injection này.

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md

```sql
' UniOn SeLeCt @@version '
' UniOn SeLeCt version() '
' UniOn SeLeCt sqlite_version() '
```

![hoang](Images/10.png)

- Db được sử dụng là `SqlLite`, phiên bản `3.31.1`.

![hoang](Images/12.png)

- Thu thập thông tin về các bảng.

![hoang](Images/13.png)

- Từ thông tin bản, tìm được các username là `TryHackMeAdmin` và `Flag`

![hoang](Images/14.png)


![hoang](Images/15.png)

- Tìm được flag 🚩🚩🚩 và trả lời các câu hỏi.

![hoang](Images/16.png)

![hoang](Images/17.png)

![hoang](Images/18.png)

- Hoàn thành bài lab 🔥🔥🔥

![hoang](Images/19.png)

