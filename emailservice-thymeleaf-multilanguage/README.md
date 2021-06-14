# Email Service, Thymeleaf, Multilanguage
## 1. Email Service
### 1.1. Giới thiệu
Spring Framework cung cấp cho bạn một API để gửi email, nó bao gồm một vài interface và một vài lớp, tất cả nằm trong 2 package `org.springframework.mail` & `org.springframework.mail.javamail`.
### 1.2. Cài đặt
Để sử dụng Spring Mail trong ứng dụng Spring Boot, hãy thêm phụ thuộc sau vào `pom.xml`:
```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-mail</artifactId>
    </dependency>
```
### 1.3. Các Class và Interface
Hình ảnh dưới đây là các Interface và Class chính của Spring Mail

![alt](https://s1.o7planning.com/vi/11145/images/20740934.png "Email Service")

>>MIME (Multi-Purpose Internet Mail Extensions): (Phần mở rộng đa mục đích cho Internet Mail) Là một phần mở rộng của giao thức Internet Email ban đầu. Nó cho phép gửi các Email có đính kèm các kiểu dữ liệu khác nhau trên Internet như audio, video, image,.., và hỗ trợ các Email có định dạng HTML.

| Class/Interface       | Mô tả |
| ----------------------|:-------------:|
| MailSender            | Đây là một interface ở mức cao nhất (top-level), nó cung cấp các chức năng để gửi một email đơn giản.|
| JavaMailSender        | Đây là interface con của MailSender, nó hỗ trợ các tin nhắn kiểu MIME, nó thường đươc sử dụng với lớp MimeMessageHelper để tạo ra MimeMessage.|
| JavaMailSenderImpl    | Là một lớp thực hiện interface JavaMailSender. Nó hỗ trợ gửi các tin nhắn MimeMessage và SimpleMailMessage. |
| MailMessage           | Là một interface đại diện cho một tin nhắn (message) đơn giản. Nó bao gồm các thông tin cơ bản của một email như người gửi, người nhận, tiêu đề (subject) và nội tin nhắn.|
| SimpleMailMessage     | Đây là một lớp thực hiện (implements) interface MailMessage, được sử dụng để tạo một tin nhắn (message) đơn giản. |
| MimeMailMessage       |Đây là một lớp thực hiện interface MailMessage, được sử dụng để tạo ra một tin nhắn hỗ trợ MIME.|
| MimeMessagePreparator |Interface này cung cấp phương thức callback được gọi khi chuẩn bị một tin nhắn MIME.|
| MimeMessageHelper |Là một lớp trợ giúp để tạo ra một tin nhắn MIME, nó hỗ trợ image, và các tập tin đính kèm, và tạo ra các tin nhắn kiểu HTML.|
### 1.4 Ví dụ
>> Tạo một Spring Bean để cấu hình 
```java
    @Bean
    public JavaMailSender getJavaMailSender(){
        JavaMailSenderImpl mailSender = new JavaMailSenderImpl();
        mailSender.setHost("smtp.gmail.com");
        mailSender.setPort(587);

        mailSender.setUsername("your-email");
        mailSender.setPassword("your-password");

        Properties props = mailSender.getJavaMailProperties();
        props.put("mail.transport.protocol", "smtp");
        props.put("mail.smtp.auth", "true");
        props.put("mail.smtp.starttls.enable", "true");
        props.put("mail.debug", "true");

        return mailSender;
    }
```
>> Tạo một RestController để gửi Email
```java
    @RestController
    public class restAPI {
        @Autowired
        public JavaMailSender mailSender;
        @RequestMapping("/sendmail")
        public String sendEmail() throws MessagingException {
            MimeMessage message = mailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message, true, "utf-8");

            String htmlMsg = "<h3>Im testing send a HTML email</h3>"
                    + "<img src='https://ncc.asia/images/logo/logo.png'>";
            message.setContent(htmlMsg, "text/html");

            FileSystemResource file = new FileSystemResource(new File("test.txt"));
            helper.addAttachment("Demo Mail", file);

            helper.setTo("mail@mail.com");
            helper.setSubject("Demo Send Email");

            mailSender.send(message);
            return "Email Sent Sucessfully!";
        }
    }
```
>> Truy cập localhost:8080/sendmail và đây là kết quả

![alt](https://i.ibb.co/jfQ4yc9/demo.png "Demo Email Service")

## 2. Thymeleaf
### 2.1. Thymeleaf là gì?
Thymeleaf là một Java template engine dùng để xử lý và tạo HTML, XML, Javascript, CSS và text.
Mục tiêu chính của thymeleaf là mang lại các template tự nhiên, đồng nhất, đơn giản (nature templates) cho công việc phát triển.
### 2.2. Lợi ích
- Với thymeleaf, ta chỉ cần sử dụng file HTML là có thể hiển thị tất cả mọi thứ (không cần jsp …).
- Thymealeaf sẽ tham gia vào renderd các file HTML dưới dạng các thuộc tính trong các thẻ HTML –> do đó ta không cần phải thêm bất kỳ thẻ non-HTML nào.
- Vì là HTML nên ta có thể xem các file view mà không cần khởi chạy server.
- Thymeleaf hỗ trợ cơ chế cache, do đó ta có thể cache dữ liệu hoặc custom để hiển thị view khi có thay đổi mà không cần restart server.
### 2.3. Cài đặt
Để sử dụng Thymeleaf, hãy thêm phụ thuộc sau vào `pom.xml`:
```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
```
### 2.4. Cú pháp & Cách hoạt động
- Cú pháp của Thymeleaf sẽ là một attributes (Thuộc tính) của thẻ HTML và bắt đầu bằng chữ `th:`
- Ví dụ, để truyền dữu liệu từ biến `name` vào trong một thẻ `H1` của HTML, ta thực hiện như sau:
```html
    <H1 th:utext="${name}"></h1>
```
thì khi render ra sẽ có dạng
```html
    <h1>Thành Phạm</h1>
```
Như đã thấy thì thẻ `th:utext="${name}` đã biến mất và giá trị biến `name` = `Thành Phạm` đã được truyền vào. Đây là cách **Thymeleaf** hoạt động.
- Vậy làm sao để truyền `Thành Phạm` vào biến `name`? Chúng ta sẽ tạo một Web Controller sử dụng anotation `Controller`
```java
    @Controller
    public class controller {
        @GetMapping("/")
        public String getIndex(Model model){
            model.addAttribute("name", "Thành Phạm");
            return "index";
        }
    }
```
- Như ví dụ trên, ta thấy xuất hiện `Model`. `Model` là đối tượng lưu giữ thông tin và được sử dụng bởi Template Engine để generate ra webpage. Có thể hiểu nó là `Context` của Thymeleaf
- Model lưu giữ thông tin dưới dạng key-value.
- Trong Thymeleaf, để lấy giá trị của `Model`, chúng ta sẽ sử dụng `Thymeleaf Standard Expression`
  - `${...}`: giá trị của một biến
  - `*{...}`: giá trị của một biến được chỉ định
  - `#{...}`: giá trị của một message
  - `@{...}`: đường dẫn URL theo context của server

## 3. Multilanguage
Spring cung cấp các hỗ trợ mở rộng cho quốc tế hóa (Internationalization) (i18n) thông qua việc sử dụng `Spring Interceptor`, `Locale Resolvers` và `Resource Bundles` cho các địa phương khác nhau.
Để hiểu rõ hơn thì chúng ta sẽ theo dõi ví dụ.
- Đầu tiên, chúng ta sẽ tạo 2 file properties cho 2 loại ngôn ngữ là Tiếng Anh và Tiếng Việt. Các file này sẽ được load và quản lý bởi `messageSource`.
```
    ## i18n/lang_en.properties
    hello = Hello
    name = Thanh Pham
```
```
    ## i18n/lang_vi.properties
    hello = Xin Chào
    name = Thành Phạm
```
- Sau đó, chúng ta phải khai báo 2 Bean là `localeResolver` và `messageSource`:
  - `localeResolver`: lấy thông tin địa phương(locale) của người dùng, thông qua session hoặc cookies
  - `messageSource`: tải nội dung các file `.properties`
```java
    @Bean
    public LocaleResolver localeResolver()  {
        CookieLocaleResolver resolver= new CookieLocaleResolver();
        return resolver;
    }
```
```java
    @Bean
    public MessageSource messageSource()  {
        ReloadableResourceBundleMessageSource messageResource= new ReloadableResourceBundleMessageSource();
        messageResource.setBasename("classpath:i18n/lang");
        messageResource.setDefaultEncoding("UTF-8");
        return messageResource;
    }
```
- `addInterceptors` sẽ xác định ngôn ngữ thay đổi sẽ được hiển thị qua param name nào trên trình duyệt. Ở đây sẽ là `?lang=en`
```java
    public void addInterceptors(InterceptorRegistry registry) {
        LocaleChangeInterceptor localeInterceptor = new LocaleChangeInterceptor();
        localeInterceptor.setParamName("lang");
        registry.addInterceptor(localeInterceptor).addPathPatterns("/*");
    }
```
Cuối cùng, ta thêm thẻ sau vào file `html` mà đã tạo ở phần Thymeleaf:
```html
  <h1 th:text="#{hello}">, </h1><h1 th:text="#{name}"></h1>
```

OUTPUT:
```
  [localhost:8080/?lang=en] Hello, Thanh Pham
  [localhost:8080/?lang=vi] Xin chào, Thành Phạm
```

## Tham khảo
[https://openplanning.net/11145/spring-email](https://openplanning.net/11145/spring-email)

[https://www.baeldung.com/spring-email](https://www.baeldung.com/spring-email)

[https://loda.me/spring-boot-9-giai-thich-cach-thymeleaf-van-hanh-expression-demo-full-loda1558267496214/](https://loda.me/spring-boot-9-giai-thich-cach-thymeleaf-van-hanh-expression-demo-full-loda1558267496214/)

[https://stackjava.com/spring/code-vi-du-spring-boot-da-ngon-ngu-internationalization-i18n.html](https://stackjava.com/spring/code-vi-du-spring-boot-da-ngon-ngu-internationalization-i18n.html)