# BabyDroid
## Tổng quan
#### Giới thiệu
Đây là challenge CTF thuộc chủ đề RE trong cuộc thi HCMUS-CTF 2022 tổ chức bởi câu lạc bộ `Computer Security Club - University of Science, VNU - HCM`.
#### Nội dung
Đề thi CTF này không có gì đáng kể ngoại trừ file `babydroid.apk`.
#### Công cụ
- [Jadx](https://github.com/skylot/jadx)
## Challenge
#### Decompile
Mở Jadx lên và mở file `babydroid.apk` lên và bạn sẽ được xem cây source code ở panel bên trái màn hình. Mình mở package `com.example.android` và có thể thấy liền ngay class `FlagValidator` chính là target mình cần để tâm

![Decompiled APK](https://i.imgur.com/OVVHVr4.png)

###### FlagValidator
```java
public class FlagValidator {
    public static boolean checkFlag(Context ctx, String flag) {
        String result = Helper.retriever();
        if (flag.startsWith("HCMUS-CTF{") && flag.charAt(19) == '_' && flag.length() == 37 && flag.toLowerCase().substring(10).startsWith("this_is_") && flag.charAt(((int) (MagicNum.obtainY() * Math.pow(MagicNum.obtainX(), MagicNum.obtainY()))) + 2) == flag.charAt(((int) Math.pow(Math.pow(2.0d, 2.0d), 2.0d)) + 3) && new StringBuilder(flag).reverse().toString().toLowerCase().substring(1).startsWith(ctx.getString(R.string.last_part)) && new StringBuilder(flag).reverse().toString().charAt(0) == '}' && Helper.ran(flag.toUpperCase().substring((MagicNum.obtainY() * MagicNum.obtainX() * MagicNum.obtainY()) + 2, (int) (Math.pow(MagicNum.obtainZ(), MagicNum.obtainX()) + 1.0d))).equals("ERNYYL") && flag.toLowerCase().charAt(18) == 'a' && flag.charAt(18) == flag.charAt(28) && flag.toUpperCase().charAt(27) == flag.toUpperCase().charAt(28) + 1) {
            return flag.substring(10, flag.length() - 1).matches(result);
        }
        return false;
    }
}
```
###### MagicNum
Hàm `checkFlag` ở trên có sử dụng class `MagicNum` sau:
```java
public class MagicNum {
    public static int obtainX() {
        return 2;
    }

    public static int obtainY() {
        return 3;
    }

    public static int obtainZ() {
        return 5;
    }
}
```
Mình dùng một text editor để find & replace các hàm `obtain()` để đơn giản hoá hàm `checkFlag`
###### Context
Hàm `checkFlag` có đoạn code `ctx.getString(R.string.last_part)` mà sau khi google thì mình tìm ra được nằm trong mục `Resources` như sau:

![Screenshot of JADX](https://i.imgur.com/BHhW13v.png)
###### Helper
Hàm `checkFlag` ở trên có sử dụng class `Helper` sau:
```java
public class Helper {
    public static String ran(String s) {
        ///...
    }

    public static String retriever() {
        String str;
        StringBuilder sb;
        String r = "";
        boolean upper = true;
        for (int i = 0; i < 26; i++) {
            if (upper) {
                sb = new StringBuilder();
                sb.append(r);
                str = "[A-Z_]";
            } else {
                sb = new StringBuilder();
                sb.append(r);
                str = "[a-z_]";
            }
            sb.append(str);
            r = sb.toString();
            upper = !upper;
        }
        return r;
    }
}
```
Thấy hàm đơn giản và không cần đầu vào, mình copy-paste hàm vào một Java compiler online và nó cho ra kết quả `[A_Z_][a_z_][A_Z_][a_z_][A_Z_][a_z_]...`. Đây là một Regular Expression, mỗi kí tự sẽ thay phiên nhau viết hoa và viết thường, không đổi nếu là gạch dưới
###### Tạo lại Flag
Với các kết quả ở trên, ta có thể đơn giản hoá hàm `checkFlag` như sau:
```java
if (
   flag.startsWith("HCMUS-CTF{") 
&& flag.charAt(19) == '_' 
&& flag.length() == 37 
&& flag.toLowerCase().substring(10).startsWith("this_is_") 
&& flag.charAt(((int) (26) == flag.charAt(((int) 19) 
&& new StringBuilder(flag).reverse().toString().toLowerCase().substring(1).startsWith("ver_sic") 
&& new StringBuilder(flag).reverse().toString().charAt(0) == '}' 
&& Helper.ran(flag.toUpperCase().substring(20, 26)).equals("ERNYYL") 
&& flag.toLowerCase().charAt(18) == 'a' 
&& flag.charAt(18) == flag.charAt(28) 
&& flag.toUpperCase().charAt(27) == flag.toUpperCase().charAt(28) + 1
) {
        return flag.substring(10, flag.length() - 1).matches(result);
}
```
Tại đây thì bạn có thể đoán các hàm bằng tên của chúng mà không cần biết Java, hoặc bạn cũng có thể google :))
#### Flag
```HCMUS-CTF{ThIs_iS_A_ReAlLy_bAsIc_rEv}```
## Tác giả
Write-up by `First Time?` team
