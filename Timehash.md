# Timehash
## Tổng quan
#### Giới thiệu
Đây là challenge CTF thuộc chủ đề Forensics trong cuộc thi HCMUS-CTF 2022 tổ chức bởi câu lạc bộ `Computer Security Club - University of Science, VNU - HCM`.
#### Nội dung
Đề thi CTF đơn giản yêu cầu bạn tìm mã PIN để app `timehash` chạy đúng.
#### Công cụ
- IDA Pro
## Challenge
#### Decompile executable
Mình mở IDA Pro lên và mở file `timehash` do BTC cung cấp.
IDA decompile hàm `main` cho mình như sau:
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  unsigned int v4; // [rsp+1Ch] [rbp-14h]
  __int64 v5; // [rsp+28h] [rbp-8h]

  if ( argc != 2 )
  {
    puts("Wrong");
    exit(1);
  }
  v5 = (int)time(0LL);
  v4 = 573785173;
  while ( v4 > 0x10000000 )
  {
    v4 = SeriousHash(v4);
    sleep(0x15180u);
    if ( v5 + 86400 > (unsigned __int64)(int)time(0LL) )
      exit(2);
  }
  if ( atoi(argv[1]) == v4 )
    return puts("Congratulations");
  else
    return puts("Wrong");
}
```
Mình thấy chương trình sẽ in ra **Congratulations** nếu như `atoi(argv[1]) == v4` tức là input của mình giống với biến `v4`, ngoài ra mình có thể thấy biến `v5` có làm một chuyện gì đó liên quan đến check thời gian, nhưng vì nó không đụng đến `v4` nên mình có thể xoá nó đi
#### Create flag
Mình tạo một file c mới và copy những gì cần thiết vào (những macro mình copy từ file `defs.h` của IDA):
```c
#include <stdint.h>
#include <stdio.h>
#define _WORD  uint16_t
#define LAST_IND(x,part_type)    (sizeof(x)/sizeof(part_type) - 1)
#if defined(__BYTE_ORDER) && __BYTE_ORDER == __BIG_ENDIAN
#  define LOW_IND(x,part_type)   LAST_IND(x,part_type)
#  define HIGH_IND(x,part_type)  0
#else
#  define HIGH_IND(x,part_type)  LAST_IND(x,part_type)
#  define LOW_IND(x,part_type)   0
#endif
#define WORDn(x, n)   (*((_WORD*)&(x)+n))
#define HIWORD(x)  WORDn(x,HIGH_IND(x,_WORD))

unsigned int SeriousHash(unsigned int a1)
{
  unsigned int v2; // [rsp+10h] [rbp-4h]
  unsigned int v3; // [rsp+10h] [rbp-4h]
  unsigned int v4; // [rsp+10h] [rbp-4h]
  unsigned int v5; // [rsp+10h] [rbp-4h]

  v2 = 73244475 * ((73244475 * (a1 ^ HIWORD(a1))) ^ ((73244475 * (a1 ^ HIWORD(a1))) >> 16));
  v3 = (8 * (HIWORD(v2) ^ v2)) ^ HIWORD(v2) ^ v2;
  v4 = (16 * ((v3 >> 5) + v3)) ^ ((v3 >> 5) + v3);
  v5 = (((v4 >> 17) + v4) << 25) ^ ((v4 >> 17) + v4);
  return (v5 >> 6) + v5;
}

void main()
{
  unsigned int v4; 
  v4 = 573785173;
  while ( v4 > 0x10000000 )
  {
    v4 = SeriousHash(v4);
  }
  printf("%u\n", v4);
}
```
Mình compile và chạy thì mình lấy được kết quả `v4 == 96521168` 
#### Flag
```HCMUS-CTF{96521168}```
## Tác giả
Write-up by `First Time?` team
