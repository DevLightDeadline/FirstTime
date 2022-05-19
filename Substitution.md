# Substitution
## Tổng quan
#### Giới thiệu
Đây là challenge CTF thuộc chủ đề Crypto trong cuộc thi HCMUS-CTF 2022 tổ chức bởi câu lạc bộ `Computer Security Club - University of Science, VNU - HCM`.
#### Nội dung
Đề thi CTF cho mình một file `msg_enc.txt` đã được mã hoá vào `chal.py` là source code dùng để mã hoá file gốc.
#### Công cụ
- Python
## Challenge
#### Decrypt
###### Code
Đây là một loại mã hoá đơn giản, substitution đúng như tên của challenge. Mình chỉnh code gốc của BTC để thuận tiện cho việc giải mã:
```python
from fnmatch import translate
from random import shuffle

msg = ''
with open('msg_enc.txt', 'r') as file:
    msg = file.read()

ALPHABET = b'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
SUB_ALPHABET = list(b'ABCDEFGHIJKLMNOPQRSTUVWXYZ')
translate_dict = {}
for u, v in zip(ALPHABET, SUB_ALPHABET):
    translate_dict[u] = v
    
print(msg.translate(translate_dict))
```
###### Substitute
Nếu bạn nhìn vào cuối file `msg_enc.txt` bạn sẽ thấy một chuỗi trông rất giống format `HCMUS-CTF{`, điều này giúp chúng ta substitute các kí tự `C`,`F`,`H`,`M`,`S`,`T`,`U`.

Sau khi thay thể các kí tự trên, sẽ có sự xuất hiện nhiều lần của cụm từ `THK` làm mình nghĩ ngay đến `THE` và thay thế `K` thành `E`. Chữ E là chữ cái thông dụng nhất trong từ điển tiếng anh và từ bước này thì các cụm từ đã hiện rõ ra rất gần với từ gốc tiếng anh của chúng mà bạn có thể đoán được.
Cuối cùng sub list sẽ là `APUHERMICWELVLNKSFYTGBDOXT`
#### Flag
```HCMUS-CTF{NHANLUN_LIKES_TO_PLAY_CRYPTOGRAM}```
## Tác giả
Write-up by `First Time?` team

Write-up này được viết khi mình không còn giữ file `msg_enc.txt` gốc nữa nên không thể ghi rõ từng bước được :<
