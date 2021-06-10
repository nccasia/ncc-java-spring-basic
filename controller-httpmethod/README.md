# Controller, HTTP Methods (POST, GET, PUT, DELETE)
## 1. `@Controller` và `@RestController`
`@Controller` thường hay được sử dụng cho Spring Controller truyền thống hay được sử dụng trong các phiên bản Spring từ 4.0 trở xuống.

`@RestController` được giới thiệu từ phiên bản Spring 4.0 để đơn giản hóa việc tạo ra các RESTful web service. Nó là sự kết hợp của annotaiton `@Controller` và `@ResponseBody`.

Để sử dụng được 2 annotation này thì cần thêm đoạn code sau vào `pom.xml`
```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```

### `@Controller`
Các controler truyền thống thường được đánh dấu với annotation `@Controller`.
```java
    @Controller
    public class Controller{
        @GetMapping("/login")
        public String loginPage() {
            return "login";
        }
    }
```

### `@RestController`
`@RestController` là một phiên bản đặc biệt của controller, nó được kết hợp bởi `@Controller` và `@ResponseBody` giúp cho việc xây dựng các RESTful API được dễ dàng hơn, đơn giản hơn.
```java
    @RestController
    public class RESTfulAPI{
        @Autowired
        private userRepository userRepository;
        @GetMapping(value = "/getall")
        public List<User> getUser() {
            return userRepository.findAll();
        }
    }
```

## 2. HTTP Methods (POST, GET, PUT, DELETE)
### 2.1 `POST` và `@PostMapping`
`POST` là một HTTP Methods, nó có chức năng là gửi các biểu mẫu HTTP (ví dụ như đăng kí, đăng nhập...)

`@PostMapping` đánh dấu hàm xử lý `POST` request trong Controller

```java
    @RestController
    public class Restful{
        @GetMapping("/post")
        public ResponseEntity<String> post() {
            return new ResponseEntity<String>("POST Response", HttpStatus.OK);
        }
    }
```
### 2.2 `GET` và `@GetMapping`
`GET` được sử dụng để lấy lại thông tin từ Server đã cung cấp bởi sử dụng một URI đã cung cấp. Các yêu cầu sử dụng GET chỉ nhận dữ liệu và không có ảnh hưởng gì tới dữ liệu.

`@GetMapping` đánh dấu hàm xử lý `GET` request trong Controller

```java
    @RestController
    public class Restful{
        @GetMapping("/get")
        public ResponseEntity<String> get() {
            return new ResponseEntity<String>("GET Response", HttpStatus.OK);
        }
    }
```
### 2.3 `PUT` và `@PutMapping`
`PUT` Thay đổi tất cả các đại diện hiện tại của nguồn mục tiêu với nội dung được tải lên.

`@PutMapping` đánh dấu hàm xử lý `PUT` request trong Controller

```java
    @RestController
    public class Restful{
        @PutMapping("/put")
        public ResponseEntity<String> put() {
            return new ResponseEntity<String>("PUT Response", HttpStatus.OK);
        }
    }
```

### 2.4 `DELETE` và `@DeleteMapping`
`DELETE` Gỡ bỏ tất cả các đại diện hiện tại của nguồn mục tiêu bởi URI.

`@DeleteMapping` đánh dấu hàm xử lý `DELETE` request trong Controller
```java
    @RestController
    public class Restful{
        @DeleteMapping("/del")
        public ResponseEntity<String> del() {
            return new ResponseEntity<String>("DELETE Response", HttpStatus.OK);
        }
    }
```

## Tham Khảo

[https://javarevisited.blogspot.com/2017/08/difference-between-restcontroller-and-controller-annotations-spring-mvc-rest.html](https://javarevisited.blogspot.com/2017/08/difference-between-restcontroller-and-controller-annotations-spring-mvc-rest.html#axzz6wR1pGy2i)

[https://kipalog.com/posts/Spring-Boot--10--RequestMapping--PostMapping--ModelAttribute--RequestParam-Web-To-Do-Thymeleaf](https://kipalog.com/posts/Spring-Boot--10--RequestMapping--PostMapping--ModelAttribute--RequestParam-Web-To-Do-Thymeleaf)

[https://vi.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP_Request_methods](https://vi.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP_Request_methods)

[https://www.baeldung.com/spring-new-requestmapping-shortcuts](https://www.baeldung.com/spring-new-requestmapping-shortcuts)
