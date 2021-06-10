# JMS
## 1. JMS là gì?
- **Java Message Service (JMS) API** là một phần của đặc tả kỹ thuật Java Enterprice Edition (Java EE), là một API trung gian hướng thông báo Java (MOM) để gửi tin nhắn giữa hai hoặc nhiều client.
- JMS mô tả các phương thức tạo bởi chương trình Java cho việc: tạo (Create), gửi (Send), nhận (Receive), đọc (Read) tin nhắn.
- JMS cho phép giao tiếp giữa các thành phần khác nhau của một ứng dụng phân tán được kết nối lỏng lẻo, đáng tin cậy và hỗ trợ bất đồng bộ.
- Giao tiếp giữa các client được tạo ra bởi message broker qua các tiêu chuẩn truyền tin bất đồng bộ như AMQP, MQTT.
- JMS API là triển khai để xử lý vấn đề nhà sản xuất-người tiêu dùng.
- JMS bao gồm 2 thành phần:
    - **API** : hỗ trợ chức năng cho người phát triển phần mềm.
    - **SPI** (Service Provider Interface) : cho phép các Provider tạo ra tool JMS tích hợp, định hướng cho mọi người sử dụng theo hướng chuẩn hóa.
  ![alt](https://gpcoder.com/wp-content/uploads/2020/04/jms-architecture.png?v=1589391006)
- Khi chúng ta phát triển Hệ thống nhắn tin Java với JMS API, thì chúng ta có thể triển khai cùng một ứng dụng trong bất kỳ JMS Provider software nào.
## 2. Các thành phần của JMS
- **JMS Provider**:
  - `JMS API` là một tập hợp các interface, không chứa bất kỳ implementation nào. JMS Provider là một hệ thống bên thứ ba, chịu trách nhiệm implement JMS API để cung cấp các tính năng nhắn tin cho khách hàng.
  - `JMS Provider` còn được gọi là phần mềm MOM (Message-oriented middleware hay MQ – Message Queue). JMS Provider cũng cung cấp một số thành phần UI để quản trị và kiểm soát phần mềm MOM này. 
  - `JMS Provider` được viết trên chuẩn Java do đó sẽ chạy được trên đa nền tảng.
- **JMS Client**: Là các chương trình độc lập hoặc các components (thành phần) của ứng dụng, được viết bằng Java có khả năng trao đổi message.
  - `JMS producer/ publisher`: là JMS client tạo và gửi tin nhắn.
  - `JMS consumer/ subscriber`: là JMS client nhận tin nhắn.
- **JMS Message**: Là các object, định dạng trung gian chứa data để giao tiếp giữa JMS Client và Provider.
- **Administered object**: hỗ trợ cơ chế quản lý và cấu hình cho JMS Object. Bao gồm:
  - `ConnectionFactory Object`: được sử dụng để tạo kết nối giữa ứng dụng Java và JMS Provider. Tương tự như khái niệm truy cập DataSource của kết nối dữ liệu.
  - `Destination Object`: là nơi lưu trữ cho message, là đối tượng JMS được JMS Client sử dụng để chỉ định đích của tin nhắn mà nó đang gửi và nguồn tin nhắn mà nó nhận được. Có hai loại Destination: Queue and Topic.
  - `JMS Queue`: Khu vực chứa các tin nhắn đã được gửi và đang chờ để đọc (chỉ bởi một consumer). Hàng đợi này đảm bảo các tin nhắn được nhận theo thứ tự gửi và mỗi tin nhắn chỉ được xử lý một lần.
  - `JMS Topic`: Một cơ chế phân phối để publisher gửi tin nhắn đến nhiều người đăng ký (subscriber).
## 3. Cơ chế giao tiếp
- `Asynchronous`: JMS tự động chuyển message đến người nhận khi message đến.
- `Reliable`: một message chỉ được chuyển đến đúng một người nhận mà không có cơ chế nhân bản, do vậy, tín hiệu phản hồi hoàn tất nhận message từ người nhận sẽ gây nên xóa bỏ thông tin trên middleware object.
## 4. Mô hình JMS
`P2P` (Point to Point)
![alt](https://codenotfound.com/assets/images/posts/jms/jms-point-to-point-messaging.png)
- `P2P` là mô hình sử dụng `Queue` để lưu trữ message.
- Mô hình có tính bảo mật cao do chỉ có 1 người gửi và 1 người nhận, nhưng đôi khi bị block do chờ message đến
  
`Pub / Sub` (Publisher / Subcriber)
![alt](https://codenotfound.com/assets/images/posts/jms/jms-publish-subscribe-messaging.png)
- `Pub / Sub` là mô hình sử dụng `Topic` để lưu trữ message.
- Mô hình cho phép 1 người gửi, nhiều người nhận. Bảo mật không cao nhưng thuận lợi áp dụng cho hệ thống phân tán.

Các mô hình này được gọi là **JMS Message Domain**.





## Tham Khảo

[https://gpcoder.com/6715-gioi-thieu-jms-java-message-services/](https://gpcoder.com/6715-gioi-thieu-jms-java-message-services/)