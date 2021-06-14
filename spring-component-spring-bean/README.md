# Spring Bean, Spring Component
## 1. Spring Bean
Ở phần này, chúng ta sẽ tìm hiểu về POJO, Java Bean, Spring Bean. Từ đó sẽ thấy được sự giống và khác nhau của chúng
### 1.1. POJO (Plain Old Java Object)
- POJO là một class mà không chịu sự chi phối của bất kì framework cụ thể nào. 
- Một POJO không có quy tắc đặt tên cụ thể cho các thuộc tính và phương thức của nó. 
```java
    public class UserPojo {
        public int id;
        public String userName;
        private String passWord;
        public UserPojo(int id, String userName, String passWord) {
            this.id = id;
            this.userName = userName;
            this.passWord = passWord;
        }
        public String id() {
            return this.id;
        }
        public String getUsername() {
            return this.userName;
        }
    }
```
- Bên trên là một ví dụ về POJO. UserPojo có thể sử dụng ở bất kỳ một chương trình, framework nào. Nhưng nó thật sự không có một quy tắc nào để định nghĩa constructor, class state, modifier (public, private, protected, default). Do vậy UserPojo class có thể gặp một số rắc rối khi sử dụng trong các framework.
### 1.2 Java Bean
- JavaBean là những lớp trong java được thiết kế đặc biệt nhằm tăng tính sử dụng lại, hướng đối tượng. 
- Thực tế chúng là những lớp đóng gói nhiều đối tượng vào trong 1 đối tượng đơn (gọi là bean). 
- Các lớp JavaBean về cơ bản vẫn là POJO nhưng cần thoả mã các yêu cầu sau:
  - **Access levels**: các thuộc tính trong class đều được đặt là private và chúng chỉ được truy xuất thông qua getter, setter method.
  - **Method name**: getter và setter method tuân thủ theo quy tắc getX và setX (X là tên thuộc tính). Trong trường hợp thuộc tính có kiểu dữ liệu boolean thì có thể đặt là isX.
  - **Default Constructor**: Luôn tồn tại một constructor mặc định không chứa tham số đầu vào.
  - **Serializable**: implement Serializable interface.
```java
    public class UserBean implements Serializable {
        private int id;
        private String userName;
        private String passWord;
        public UserBean() {
        }
        public UserBean(int id, String userName, String passWord) {
            this.id = id;
            this.userName = userName;
            this.passWord = passWord;
        }
        public String getId() {
            return id;
        }
        public void setId(int id) {
            this.id = id;
        }
        public String getUserName() {
            return userName;
        }
        public void setUserName(String userName) {
            this.userName = userName;
        }
        public String getPassWord() {
            return passWord;
        }
        public void setPassWord(String passWord) {
            this.passWord = passWord;
        }
    }
```
- JavaBean có thể hữu dụng khi sử dụng trong các framework ngày nay, tuy nhiên việc sử dụng nó cũng có một số hạn chế nhất định mà chúng ta sẽ phải chấp nhận:
    - **Mutability**: các JavaBean sẽ không thể đạt được tính chất immutable vì chúng phải định nghĩa các setter method.
    - **Boilerplate**: Việc triển khai getter, setter method cho toàn bộ các thuộc tính trong class đôi khi không cần thiết.
    - **No-Agrs Constructor**: Chúng ta thường không cung cấp no-args constructor vì có một số thuộc tính bắt buộc phải có giá trị sau khi khởi tạo, chúng ta sẽ không thể đặt được mục đích này khi sử dụng JavaBean.

### 1.3 Spring Bean
- Spring Beans chính là những Java Object mà từ đó tạo nên khung sườn của một Spring application.
- Spring Bean không có gì đặc biệt, bất kỳ object nào trong Spring framework mà chúng ta khởi tạo thông qua Spring container được gọi là Spring Bean. 
- Spring cung cấp 3 cách để cấu hình Bean:
    - **Annotation Based Configuration** – Bằng việc sử dụng `@Service` hoặc `@Component` annotations. Chi tiết về Scope có thể được cung cấp bởi `@Scope` annotation.
    - **XML Based Configuration** – Bằng việc tạo file XML Spring Configuration để config beans.
    - **Java Based Configuration** – Đươc tích hợp từ Spring 3.0, chúng ta có thể configure Spring beans sử dụng chương trình java. Một vài annotations quan trọng được sủ dụng cho config dựa trên java là `@Configuration`, `@ComponentScan` và `@Bean`.
```java
    @Service
    public class UserBean  {
        private String name;
        public UserBean() {
        }
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
    }
```
- Trên đây là ví dụ về cấu hình một Bean với **Annotation Based Configuration**. Đây cũng là cách được sử dụng phổ biến. 

## 2. Spring Component
`@Component` là một Annotation đánh dấu trên các Class để giúp Spring biết nó là một `Bean`.

>> Ví dụ:
```java
    @Component
    class UserService {
        ...
    }

    @Controller
    class UserController {
        private final UserService userService;
        @Autowired
        public UserController(UserService userService) {
            this.userService = userService;
        }
    }
```

Trong ví dụ trên, `UserService` class được chú thích với `@Component` do vậy nó sẽ được Spring khởi tạo và đăng ký với `Application context`. Khi `UserController` khai báo `UserService` là một dependency mà nó cần sử dụng, Spring sẽ tìm kiếm và tiêm một `UserService` instance đã được khởi tạo từ trước vào `UserController` để sử dụng.
## Tham khảo

[baeldung.com](https://www.baeldung.com)

[loda.me](https://loda.me/)
