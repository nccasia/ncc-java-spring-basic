# Authentication vs Authorization
Phần này chỉ nói về `Authentication` và `Authorization`, làm cơ sở cho **Spring Security**

| Authentication      | Authorization |
| --------------------|:-------------:|
| Authentication xác nhận danh tính của bạn để cấp quyền truy cập vào hệ thống.| Authorization xác định xem bạn có được phép truy cập tài nguyên không.|
| Đây là quá trình xác nhận thông tin đăng nhập để có quyền truy cập của người dùng.| Đó là quá trình xác minh xem có cho phép truy cập hay không.|
| Nó quyết định liệu người dùng có phải là những gì anh ta tuyên bố hay không.| Nó xác định những gì người dùng có thể và không thể truy cập.|
| Authentication thường yêu cầu tên người dùng và mật khẩu.| Các yếu tố xác thực cần thiết để authorization có thể khác nhau, tùy thuộc vào mức độ bảo mật.|
| Authentication là bước đầu tiên của authorization vì vậy luôn luôn đến trước.| Authorization được thực hiện sau khi authentication thành công.|
| Ví dụ, sinh viên của một trường đại học cụ thể được yêu cầu tự xác thực trước khi truy cập vào liên kết sinh viên của trang web chính thức của trường đại học. Điều này được gọi là authentication.| Ví dụ, authorization xác định chính xác thông tin nào sinh viên được phép truy cập trên trang web của trường đại học sau khi authentication thành công.|
## Tham khảo
[http://www.differencebetween.net/technology/difference-between-authentication-and-authorization/](http://www.differencebetween.net/technology/difference-between-authentication-and-authorization/)
