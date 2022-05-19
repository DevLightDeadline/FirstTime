# Super Secret
## Tổng quan
#### Giới thiệu
Đây là challenge CTF thuộc chủ đề Misc trong cuộc thi HCMUS-CTF 2022 tổ chức bởi câu lạc bộ `Computer Security Club - University of Science, VNU - HCM`.
#### Nội dung
Đề thi CTF nói rằng một thành viên câu lạc bộ đã để lộ một bí mật trong Discord của cuộc thi. Đây là gợi ý để tìm ra flag của chall này.
#### Công cụ
- Discord
## Challenge
#### Rickrolling
Do tác giả của chall là `Fluoxetine#3023`nên việc đầu tiên mình tìm đến là xem Profile của tác giả trên Discord. Khi mở profile lên nó hiện ra một đường link [pastebin](https://pastebin.com/LsNtJMke) khá thú vị. Truy cập link lên bạn sẽ thấy một paste được đặt tên là flag và có nội dung như sau:

```HCMUS-CTF{https://www.youtube.com/watch?v=dQw4w9WgXcQ}```

Nếu bạn có kinh nghiệm với Rickroll thì bạn cũng có thể đã nhận ra id `dQw4w9WgXcQ` rồi :)). Nhưng để chắc ăn thì mình cũng submit flag này vì format flag là hợp lệ (`HCMUS-CTF{...}`) và hiển nhiên đó là flag sai.
#### Secret
Sau một hồi đi xem Profile của các admin thì mình tìm hướng tiếp cận khác.
Đề thi chall được viết bằng tiếng anh, và trong đề mình có để ý đến cụm từ **secret**. Mình nhập cụm từ này vào hộp search của Discord và nó trả về một kết quả:

![Screenshot of an Discord message](https://i.imgur.com/yWElwFw.png)

Đọc vào mình không thấy chữ *secret* xuất hiện ở đâu cả, điều đó giúp mình nhận ra nó nằm trong tên của file ảnh.
Nhấp vào ảnh và chọn `Open original` mình có thể thấy tên file là 
```secret-is-c291872ada763ed9a480eca240552890.png```
#### Flag
```HCMUS-CTF{c291872ada763ed9a480eca240552890}```
## Tác giả
Write-up by `First Time?` team
