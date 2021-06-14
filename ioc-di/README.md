# IOC Containers, DI
## 1. IOC Containers 
- **IoC (Inversion of Control):** là một nguyên tắc trong kỹ thuật phần mềm, nó chuyển quyền kiểm soát object hoặc các phần của chương trình sang một Container hoặc một Framework.
- **IoC Container** kế thừa những đặc trưng của **IoC**. Trong Spring, `ApplicationContext` là phương thức đại diện cho **IoC Containers** 
- **IoC Container** sẽ tạo ra các đối tượng, nối chúng lại với nhau, cấu hình chúng, và quản lý vòng đời của chúng từ khi tạo ra đến khi bị hủy.
- **IoC Container** sử dụng **DI (Dependency Injection)** để quản lý các thành phần tạo nên một ứng dụng. Những đối tượng này được gọi là Spring Bean.
## 2. DI (Dependency Injection)
### 2.1. Khái niệm
**Dependency Injection** là một design pattern cho phép loại bỏ các phần phụ thuộc được mã hóa cứng và làm cho ứng dụng có thể mở rộng và dễ bảo trì. Chúng ta có thể thực hiện chèn Dependency trong cấu trúc code để chuyển dependency resolution từ compile-time qua runtime.

Hiểu một cách đơn giản: 
  - Các class không giao tiếp trực tiếp với nhau mà giao tiếp thông qua Interface. Một class cấp cao hơn sẽ gọi class cấp thấp hơn thông qua interface.
  - Việc khởi tạo các module cấp thấp sẽ do IOC Container thực hiện. 
  - DI được dùng để làm giảm sự phụ thuộc giữa các module, dễ dàng hơn trong việc thay đổi module, bảo trì code và testing.
### 2.2. Các dạng của DI
DI có 3 dạng chính là: 
- **Constructor Injection:** Các dependency sẽ được container truyền vào (inject vào) 1 class thông qua constructor của class đó. Đây là cách thông dụng nhất.
```java  
    @Repository
    public class userRepository {
        public userRepository(){
            System.out.println("userRepository no-arg constructor called");
        }
    }
```
```  java
    @Service
    public class userService {
        @Autowired
        public userService(userRepository repository) {
            System.out.println("userService arg constructor called");
            this.repository = repository;
        }
    }
  ```
- **Setter Injection:** Các dependency sẽ được truyền vào 1 class thông qua các hàm Setter.
``` java 
    @Service
    public class userService {
        public userService(userRepository repository) {
            this.repository = repository;
        }
        
        @Autowired
        public void setRepository(userRepository repository) {
            System.out.println("userService setter called");
            this.repository = repository;
        }
 ```
- **Filed-Based Injection:**
```java
 @Service
    public class userService {
       @Autowrite
       private userRepository repository;
    }
```

## Tham khảo
[https://toidicodedao.com/2015/11/03/dependency-injection-va-inversion-of-control-phan-1-dinh-nghia/](https://toidicodedao.com/2015/11/03/dependency-injection-va-inversion-of-control-phan-1-dinh-nghia/)

[https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)

[https://www.amitph.com/spring-constructor-injection-example/](https://www.amitph.com/spring-constructor-injection-example/)
