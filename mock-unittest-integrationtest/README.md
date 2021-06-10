# Mock, Unit test, Integration test
## 1. Mock
### 1.1. Khái niệm
- `Mock Object` (MO) là một đối tượng ảo, mô phỏng các tính chất và hành vi giống hệt như đối tượng thực. Được truyền vào bên trong khối, mã để kiểm tra tính đúng đắn của các hoạt động bên trong.
### 1.2. Đặc điểm
- Đơn giản hơn đối tượng thực nhưng vẫn giữ được sự tương tác với các đối tượng khác.
- Không lặp lại nội dung đối tượng thực.
- Cho phép thiết lập các trạng thái riêng trợ giúp cho việc thực hiện unit test.
### 1.3. Trường hợp sử dụng
- Đối tượng thật có những hành vi không đoán trước được.
- Đối tượng thật khó cài đặt.
- Đối tượng thật chậm.
- Đối tượng thật có / là giao diện người dùng.
- Đối tượng thật không tồn tại.
## 2. Unit Test
### 2.1. Khái niệm
- Là 1 loại kiểm thử phần mềm, trong đó các đơn vị đơn lẻ của phần mềm được kiểm thử.
- Unit ở đây có thể hiểu là function, proceduce, method, class.
### 2.2. Đặc điểm
- Chạy nhanh, tự động.
- Chạy độc lập giữa các test case, không phụ thuộc vào kiểm thử.
- Test case đơn giản, dễ đọc, dễ bảo trì.
- Phản ánh đúng hoạt động của module.
- Đóng vai trò như những người sử dụng đầu tiên của hệ thống.
### 2.3. Trạng thái
Unit Test có 3 trạng thái cơ bản:
- `Fail` (trạng thái lỗi)
- `Ignore` (tạm ngừng thực hiện)
- `Pass` (trạng thái làm việc)
### 2.4. Lợi ích
- Tạo ra môi trường lý tưởng để kiểm tra bất kỳ đoạn code nào, có khả năng thăm dò và phát hiện lỗi chính xác, duy trì sự ổn định của toàn bộ PM và giúp tiết kiệm thời gian so với công việc gỡ rối truyền thống.
- Phát hiện các thuật toán thực thi không hiệu quả, các thủ tục chạy vượt quá giới hạn thời gian.
- Phát hiện các vấn đề về thiết kế, xử lý hệ thống, thậm chí các mô hình thiết kế.
- Phát hiện các lỗi nghiêm trọng có thể xảy ra trong những tình huống rất hẹp.
- Tạo hàng rào an toàn cho các khối mã: Bất kỳ sự thay đổi nào cũng có thể tác động đến hàng rào này và thông báo những nguy hiểm tiềm tàng.

## 3. Intergration Test
### 3.1. Khái niệm
`Intergration Test` hay còn gọi là kiểm thử tích hợp, là 1 giai đoạn trong kiểm thử phần mềm, nơi mà các module được tích hợp lại và kiểm thử theo nhóm.
### 3.2. Intergration Test là cần thiết
Mặc dù mỗi module đều được unit test nhưng các lỗi vẫn còn tồn tại với các lý do khác nhau:
- Một Module nói chung được thiết kế bởi một lập trình viên có hiểu biết và logic lập trình có thể khác với các lập trình viên khác. Kiểm thử tích hợp là cần thiết để đảm bảo tính hợp nhất của phần mềm.
- Tại thời điểm phát triển module sẽ có một số thay đổi theo yêu cầu khách hang, những thay đổi này có thể không nhận được unit test từ trước đó.
- Giao diện và cơ sở dữ liệu của các module có thể chưa hoàn chỉnh khi được ghép lại.
- Khi tích hợp hệ thống các module có thể không tương thích với cấu hình chung của hệ thống
- Thiếu các xử lý ngoại lệ có thể xảy ra