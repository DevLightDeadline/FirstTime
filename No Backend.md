# No Backend
## Tổng quan
#### Giới thiệu
Đây là challenge CTF thuộc chủ đề Web trong cuộc thi HCMUS-CTF 2022 tổ chức bởi câu lạc bộ `Computer Security Club - University of Science, VNU - HCM`.
#### Nội dung
Đề thi CTF nói rằng một bạn tạo một chatbot tại trang [103.245.250.31:32323](http://103.245.250.31:32323/), tuy nhiên backend đã bị xoá.
#### Công cụ
- Browser có DevTool
## Challenge
#### Làm quen với trang
Khi truy cập vào trang, mình được dẫn ngay đến trang đăng nhập `#/auth/login`. Mình mở DevTool lên và mở tab Network và nhập ngẫu nhiên vào form login lẫn signup, đúng thật là không có backend do mình không thấy có traffic nào cả.
Mình mở sang tab Sources, có vẻ website được xây dựng bằng [Vue.js](https://vuejs.org/) và sử dụng [Webpack](https://webpack.js.org/). May mắn thay mình có thể xem được source code gốc của app.
Sau khi xem các code trong `main.js` cũng như các component Router và Store thì mình biết được rằng chatbot nằm ở `#/dashboard`, tuy nhiên để có thể truy cập thì mình cần thoả điều kiện 
```js
isAuthenticated && getCookie("token")
```
#### Get Flag
###### Cookie
Mình mở tab Consoles và nhập dòng lệnh sau để set cookie `token`
```js
document.cookie = 'token=chuc_ban_co_mot_ngay_vui_ve'
```
Như thế thì `getCookie("token") == true`
###### isAuthenticated
Biến isAuthenticated được lưu trong `store.getters` và mình google thì để thay đổi mình cần dùng `store.commit`. Trước tiên mình sẽ lấy được store bằng dòng lệnh sau:
```js
document.getElementById('app').__vue__.$store
```
Trong `$store.commit` mình thấy có hàm `setUser` và mình nghĩ ngay đây là target của mình. Mình nhập dòng lệnh sau:
```js
document.getElementById('app').__vue__.$store.commit("setUser", true)
```
###### Chatbot
Sau khi set được 2 biến mình load link `#/dashboard` và mình đã vào được giao diện chat. Mình nhắn một cái gì đó có chữ `flag` thì chatbot sẽ reply flag cho mình
#### Flag
```HCMUS-CTF{w0w_4uth3ntication_bYP4ss_s000_h4rd}```
## Tác giả
Write-up by `First Time?` team

Write-up này được viết khi cái trang Vue không còn phản hồi nữa nên có thể mình bỏ bớt bước nào đó nha :(