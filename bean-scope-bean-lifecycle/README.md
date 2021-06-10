# Bean Scopes, Bean Life Cycle
## 1. Bean Scopes
Có 5 sopce mặc định đã được định nghĩa:
-	**Singleton**: Chỉ duy nhất một instance của bean sẽ được tạo cho mỗi container. Đây là scope mặc định cho spring bean. Khi sử dụng scope này cần chắc chắn rằng các bean không có các biến/thuộc tính được share.
-	**Prototype**: Một instance của bean sẽ được tạo cho mỗi lần được yêu cầu(request)
-	**Request**: nó dùng cho ứng dụng web, một instance của bean sẽ được tạo cho mỗi HTTP request.
-	**Session**: Mỗi instance của bean sẽ được tạo cho mỗi HTTP Session
-	**Global-Session**: Được sử dụng để tạo global sesion bean cho các ứng dụng Portlet.
```java
    @RestController
    @Scope("propertype")
    public class HomeController{
    ...
    }
```
## 2. Bean Life Cycle
**Spring Boot** từ thời điểm bắt đầu chạy tới khi shutdown được biểu diễn như hình dưới đây
![alt](https://loda.me/assets/static/2.2b6d00f.f8f825a.jpg)
- Khi **IoC Container** (`ApplicationContext`) tìm thấy một **Bean** cần quản lý, nó sẽ khởi tạo bằng `Constructor`
- Inject dependencies vào **Bean** bằng **Setter**, và thực hiện các quá trình cài đặt khác vào Bean như `setBeanName`, `setBeanClassLoader`, v.v..
- Hàm đánh dấu `@PostConstruct` được gọi
- Tiền xử lý sau khi` @PostConstruct` được gọi.
- **Bean** sẵn sàng để hoạt động
- Nếu **IoC Container** không quản lý bean nữa hoặc bị shutdown nó sẽ gọi hàm `@PreDestroy` trong **Bean**
- Xóa **Bean**.
  

```java
    @Component
    public class User {
    
        @PostConstruct
        public void postConstruct(){
            System.out.println("\t>> Đối tượng User sau khi khởi tạo xong sẽ chạy hàm này");
        }
        
        @PreDestroy
        public void preDestroy(){
         System.out.println("\t>> Đối tượng User trước khi bị destroy thì chạy hàm này");
        }
    }
```

## Tham khảo
[https://www.tutorialspoint.com/spring/spring_bean_scopes.htm](https://www.tutorialspoint.com/spring/spring_bean_scopes.htm)

[https://loda.me/spring-boot-3-spring-bean-life-cycle-post-construct-va-pre-destroy-loda1557583753982/](https://loda.me/spring-boot-3-spring-bean-life-cycle-post-construct-va-pre-destroy-loda1557583753982/)
