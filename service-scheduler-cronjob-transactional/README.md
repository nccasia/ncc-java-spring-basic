# Service, Scheduler/Cronjob, Transactional
## 1. Service
### 1.1. Service là gì?
`Service` là một lớp trong kiến trúc của Spring Boot, thực hiện các nghiệp vụ logic.
### 1.2. `@Service` Annotation
- `@Service` annotation để đánh dấu một class thuộc về tầng service nắm giữ code xử lý business.
```java
    @Target({ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Component
    public @interface Service {

    /**
    * The value may indicate a suggestion for a logical component name,
    * to be turned into a Spring bean in case of an autodetected component.
    * @return the suggested component name, if any (or empty String otherwise)
    */
    @AliasFor(annotation = Component.class)
    String value() default "";

    }
```
- Những Class được đánh dấu là `@Service` sẽ được đăng ký với `ApplicationContext ` bởi vì chúng được đánh dấu `@Component` annotation.

## 2. Scheduler / Cronjob
### Scheduler
`@Scheduled` là một Annotation sử dụng để cấu hình một lịch trình (schedule), nó được gắn trên một phương thức, và phương thức này sẽ được chạy theo lịch được cấu hình bởi `@Scheduled`.

Để bật tính năng Scheduler, cần đặt `@EnableScheduling` annotaiton trong Config.
|Thuộc tính|Mô tả|
|----------|:---:|
| cron     | Là một biểu thức cron, nó chứa 6 trường "giây, phút, giờ, ngày trong tháng, tháng, ngày của tuần". Giúp chỉ định một lịch trình phức tạp. `@return` trả về một biểu thức cron, chứa thông tin lịch trình.|
| zone     | Một múi giờ mà biểu thức cron sẽ được dùng. Theo mặc định, thuộc tính này là String rỗng (nghĩa là múi giờ địa phương của máy chủ sẽ được sử dụng).|
| fixedDelay| Thực thi các phương thức được chú thích (annotated), sau khi hoàn thành nghỉ một khoảng thời gian cố định theo mili giây, sau đó thực thi lượt tiếp theo. `@return` thời gian nghỉ theo mili giây.|
| fixedDelayString |  Thực thi các phương thức được chú thích (annotated), sau khi hoàn thành nghỉ một khoảng thời gian cố định theo mili giây, sau đó thực thi lượt tiếp theo. `@return` thời gian nghỉ theo mili giây.|
| fixedRate|	Thực thi phương thức được chú thích (annotated), với một khoảng thời gian cố định giữa các lần gọi. `@return` khoảng thời gian cố định giữa các lần gọi tính theo mili giây.|
|fixedRateString | Thực thi phương thức được chú thích (annotated), với một khoảng thời gian cố định giữa các lần gọi. `@return` khoảng thời gian cố định giữa các lần gọi tính theo mili giây.|
|initialDelay| Khoảng thời gian tính bằng mili giây tạm dừng trước khi thực thi lần đầu tiên, sử dụng cùng với fixedRate() hoặc fixedDelay(). `@return` trả về thời gian trễ cho lần gọi đầu tiên|
|initialDelayString|	Khoảng thời gian tính bằng mili giây tạm dừng trước khi thực thi lần đầu tiên, sử dụng cùng với fixedRate() hoặc fixedDelay(). `@return` trả về thời gian trễ cho lần gọi đầu tiên.|

### Cronjob
`@Schedule` cho phép  thiết lập các lịch trình phức tạp, chẳng hạn thực thi nhiệm vụ vào tất cả các ngày thứ hai lúc 12 giờ trưa. Để thiết lập các lịch trình phức tạp bạn cần sử dụng thuộc tính `cron`.

`Cron` có 6 trường:
```
second, minute, hour, day of month, month, day(s) of week
```
>Ví dụ:
```
"0 0 * * * *" // Đầu giờ của tất cả các giờ của tất cả các ngày.
 
"*/10 * * * * *" // Mỗi 10 giây (số giây chia hết cho 10).
 
"0 0 8-10 * * *" // 8, 9 và 10 giờ các ngày
 
"0 0/30 8-10 * * *" // 8:00, 8:30, 9:00, 9:30 và 10 tất cả các ngày
 
"0 0 9-17 * * MON-FRI" // 9, .. 17 giờ các ngày thứ 2 tới thứ 6 (monday & friday)
 
"0 0 0 25 12 ?" // Tất cả các ngày giáng sinh, nửa đêm.

```

|   Ký hiệu    |	Ý nghĩa |
|--------------|:----------:|
| * 	       | Khớp với bất kỳ|
| */X          | Tất cả các X (Chia hết cho X)|
| ?            | ("không chỉ định giá trị") - nó hữu ích khi cần chỉ định gì đó trong một trong 2 trường (field), trong đó một trường cho phép còn trường kia thì không. Chẳng hạn, nếu ta muốn ngày thứ 10 trong tháng, nhưng không quan tâm đó là ngày thứ mấy trong tuần, tôi cần đặt "10" vào trường day-of-month, và đặt "?" vào trường day-of-week.|

### Ví dụ
> Dùng Scheduler và Cronjob để Bot thông báo giờ mỗi `n` phút và 12h trưa mỗi ngày

>Table message
```sql
    create table message
    (
        id      serial       not null,
        sender  varchar(50)  not null,
        message varchar(255) not null
    );
```
>Chat.java
```java
    @Entity
    @Table(name = "message", schema = "public")
    @Getter
    @Setter
    @NoArgsConstructor
    @ToString
    public class Chat {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private int id;
        @Column(name = "sender")
        private String sender;
        @Column(name = "message")
        private String message;

        public Chat(String sender, String message) {
            this.sender = sender;
            this.message = message;
        }
    }
```
>chatRepository.java
```java
    @Repository(value = "chatRepository")
    public interface chatRepository extends JpaRepository<Chat, Integer> {
    }
```
>index.html
```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>Welcome Page</title>
        <link rel="stylesheet" th:href="@{/css/css.css}">
    </head>
    <body>
    <div class="wrapper fadeInDown">
        <div id="formContent">
            <!-- Tabs Titles -->
            <h2 class="active">HOME</h2>
            <h2 class="inactive underlineHover" th:if="${#request.userPrincipal == null}"><a href="/login">Sign
                In</a></h2>
            <h2 class="inactive underlineHover" th:if="${#request.userPrincipal == null}"><a href="/signup">Sign
                Up</a></h2>
            <h2 class="inactive underlineHover" th:if="${#request.userPrincipal != null}"><a href="/user">User</a></h2>
            <h2 class="inactive underlineHover" th:if="${#request.userPrincipal != null}"><a href="/logout">Log
                Out</a></h2>

            <div class="list" th:if="${#request.userPrincipal != null}">
                <form th:action="@{/}" method="post">
                    <input type="text" name="message">
                    <input type="submit" value="Send">
                </form>
            </div>
            <th:block th:each="chat : ${chatList}">
                <div class="list1" th:class="${chat.id} % 2 == 0 ? list2 : list1">
                    <a th:href="@{'/user/' + ${chat.sender}}">
                        <b th:utext="${chat.getSender()}"></b>
                    </a>
                    <b>: </b>
                    <span th:utext="${chat.getMessage()}"></span>
                </div>
            </th:block>
            <div id="formFooter">
                Hi, <span th:text="${#request.userPrincipal.getName()}" th:if="${#request.userPrincipal != null}"></span>
                <span th:if="${#request.userPrincipal == null}">Guest</span> !
            </div>
        </div>
    </div>
    </body>
    </html>
```
>Scheduler.java
```java
    @Component
    @RequiredArgsConstructor
    public class Scheduler {
        private final chatRepository chatRepository;
        @Scheduled(fixedDelay = 1 * 1000 * 60)
        public void postTime(){
            chatRepository.save(new Chat("Bot", "Bây giờ là: " + java.time.LocalDateTime.now()));
        }
        @Scheduled(cron = "0 0 12 * * ?")
        public void postHaveLaunch(){
            chatRepository.save(new Chat("Bot", "Đi ăn trưa thôi :)))"));
        }
    }
```
## 3. Transactional
### 3.1. Giới thiệu chung
`Transaction` quản lý những thay đổi mà bạn thực hiện trong một hoặc nhiều hệ thống, nó có thể database, message brokers, hoặc bất kỳ loại hệ thống phần mềm nào khác. Mục tiêu chính của giao dịch là cung cấp các đặc điểm `ACID` để đảm bảo tính nhất quán và hợp lệ của dữ liệu của bạn.
#### ACID transactions
Vốn dĩ một transaction được đặc trưng bởi 4 yếu tố (thường được gọi là ACID):
- `Atomicity` quy định rằng tất cả các hoạt động của transaction hoặc là thực thi thành công hết hoặc là không có bất cứ hành động nào được thực khi có bất kỳ một hoạt động thực thi không thành công.
- `Consistency` nghĩa là tất cả các ràng buộc toàn vẹn dữ liệu(constraints, key, data types, Trigger, Check) phải được thực thi thành công cho mọi transaction phát sinh xuống database, nhầm đảm bảo tính đúng đắn của dữ liệu.
- `Isolation` đảm bảo các transaction xảy ra xen kẽ sẽ không làm ảnh hưởng đến tính nhất quán của dữ liệu. Các thay đổi dữ liệu bên trong mỗi transaction sẽ được cô lập, các transaction khác sẽ không thể nhìn thấy cho đến khi nó được đồng bộ xuống database. 
- `Durability` đảm bảo một transaction thực thi thành công thì tất cả những thay đổi trong transaction phải được đồng bộ xuống database kể cả khi hệ thống xảy ra lỗi hoặc bị mất điện. 

### 3.2 Transaction với Spring boot
#### `@Transaction` annotation
- `@Transactional` annotation sẽ nói với Spring rằng method cần được thực thi bên trong một transaction. Khi ta sử dụng method đó, Spring sẽ tạo ra một proxy object bao bọc object chứa method và cung cấp các đoạn mã cần thiết để bắt đầu một transaction.
- Mặc định, proxy sẽ start một transaction trước khi có một yêu cầu đến method được chú thích với `@Transactional` annotation. Sau khi method thực thi xong, proxy sẽ commit hoặc rollback transaction nếu có một `RuntimeException` hoặc `Error` xảy ra trong quá trình thực thi. Mọi thứ xảy ra ở giữa, chỉ là các đoạn mã code thực thi logic business do chính chúng ta viết. 
- `@Transactional` annottion còn hỗ trợ cho chúng ta tuỳ biến một các hành vi của một transaction thông qua một số thuộc tính quan trọng như `propagation`, `readOnly`, `rollbackFor`, `noRollbackFor`.
#### Transaction Propagation
Spring cung cấp 7 tuỳ biến cho Propagation trong @Transactional annotation như sau:
- `REQUIRED`: Nói với Spring rằng nếu có một transaction đang hoạt động thì nó sẽ sử dụng chung, nếu không có transaction nào đang hoạt động, method được gọi sẽ tạo một transaction mới. Đây là giá trị mặc định của Propagation.
- `SUPPORTS`: Chỉ đơn giản là sử dụng lại transaction hiện đang hoạt động. Nếu không thì method được gọi sẽ thực thi mà không được đặt trong một transactional context nào
- `MANDATORY`: yêu cầu phải có một transaction đang hoạt động trước khi gọi, nếu không method được gọi sẽ ném ra một exception.
- `NEVER`: sẽ ném một exception nếu method được gọi trong một transaction hoạt động
- `NESTED`: Method được gọi sẽ tạo một transaction mới nếu không có transaction nào đang hoạt động. Nếu method được gọi với một transaction đang hoạt động Spring sẽ tạo một savepoint và rollback tại đây nếu có Exception xảy ra. 

#### Handling Exceptions
  Spring proxy sẽ tự động rollback transaction nếu có một RuntimeException xảy ra. Chúng ta có thể tùy biến bằng cách sử dụng thuộc tính `rollbackFor` và `noRollbackFor` của `@Transactional` annotation.
 - `rollbackFor` cho phép bạn cung cấp một mảng các Exception class mà transaction sẽ bị rollback nếu chúng xảy ra. 
 - `noRollbackFor` được dùng để chỉ định một mảng các Exception class mà transaction sẽ không rollback khi chúng xảy ra.

> Ví dụ
```java
    @Service
    public class userServiceImpl implements userService {
        @Autowired
        private userRepository userRepository;
        @Override
        @Transactional
        public void updateMail(Long id, String mail) {
            User user = userRepository.findById(id).get();
            user.setMail(name);
        }
    }
```


## Tham Khảo

[https://openplanning.net/11131/chay-cac-nhiem-vu-nen-theo-lich-trinh-trong-spring](https://openplanning.net/11131/chay-cac-nhiem-vu-nen-theo-lich-trinh-trong-spring)

[https://www.baeldung.com/transaction-configuration-with-jpa-and-spring](https://www.baeldung.com/transaction-configuration-with-jpa-and-spring)

[https://thorben-janssen.com/transactions-spring-data-jpa/](https://thorben-janssen.com/transactions-spring-data-jpa/)
