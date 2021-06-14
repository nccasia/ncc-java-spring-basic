# DTO
## 1. Kiến trúc Source Code và Kiến trúc dữ liệu
- Spring Boot đều tuân theo 2 mô hình cơ bản:
  - Mô hình MVC
  - Mô hình 3 lớp (3 tier)

![alt](https://images.viblo.asia/fdbe3b44-aa91-4a88-9202-814c56ef9178.png)
Sơ đồ trên dùng để tổ chức Source Code trong chương trình. Nhờ đó chúng ta chia thành các Controller, Service, Repository tương ứng với các layer. Tuy nhiên, nếu xét về mặt tổ chức data, thì sơ đồ sẽ trở thành như sau.
![alt](https://www.petrikainulainen.net/wp-content/uploads/spring-web-app-architecture.png)

- Mô hình này cũng gồm có 3 lớp, trong đó tên các layer được đổi thành các thành phần tương ứng trong Spring Boot.

- Theo đó, tương ứng với từng layer thì data sẽ có dạng khác nhau. Nói cách khác, mỗi layer chỉ nên xử lý một số loại data nhất định. Mỗi dạng data sẽ có nhiệm vụ, mục đích khác nhau. Tất nhiên trong code cũng được chia ra tương ứng.

- Ví dụ trong sơ đồ thì `Controller` không nên đụng tới data dạng `Domain Model` hoặc `Entity`, chỉ được phép nhận và trả về `DTO`.

## 2. Data
### 2.1. Các dạng Data
Theo sơ đồ trên, data trong ứng dụng Spring Boot chia thành 2 loại chính:
- `Public`: nghĩa là để trao đổi, chia sẻ với bên ngoài qua REST API hoặc giao tiếp với các service khác trong microservice. Data lúc này ở dạng DTO.
- `Private`: các data dùng trong nội bộ ứng dụng, bên ngoài không nên biết. Data lúc này nằm trong các Domain model hoặc Entity.
  
Từ 2 loại public và private trên, chúng ta có 3 dạng data:

- `DTO (Data transfer object)`: là các class đóng gói data để chuyển giữa client - server hoặc giữa các service trong microservice. Mục đích tạo ra `DTO` là để giảm bớt lượng info không cần thiết phải chuyển đi, và cũng tăng cường độ bảo mật.
- `Domain Model`: là các class đại diện cho các domain, hiểu là các đối tượng thuộc business như Client, Report, Department,... chẳng hạn. Trong ứng dụng thực, các class đại diện cho kết quả tính toán, các class làm tham số đầu vào cho service tính toán,... được coi là `Domain model`.
- `Entity`: cũng là `Domain Model` nhưng tương ứng với table trong DB, có thể map vào DB được. Lưu ý chỉ có entity mới có thể đại diện cho data trong DB.
  
Các dạng data có hậu tố tương ứng, trừ entity. Ví dụ entity `User` không có hậu tố, nếu là domain model thì là `UserModel`, hoặc với DTO thì là `UserDto`,...

### 2.2. Nguyên tắc chọn Data tương ứng với Layer
- `Web layer`: chỉ nên xử lý `DTO`, đồng nghĩa với việc các `Controller` chỉ nên nhận và trả về dữ liệu là `DTO`.
- `Service layer`: nhận vào `DTO` (từ controller gửi qua) hoặc `Domain model` (từ các service nội bộ khác). Dữ liệu được xử lý (có thể tương tác với DB), cuối cùng được `Service` trả về Web layer dưới dạng `DTO`.
- `Repository layer`: chỉ thao tác trên `Entity`, vì đó là đối tượng thích hợp, có thể mapping vào DB.

## 3. Ví dụ
Như ví dụ ở phần [Spring Security](../spring-security/README.md), khi tạo `Restful API` để get thông tin của User, ta đã trả về tất cả dữ liệu trên DB kể cả `password`. Điều đó là hoàn toàn không cần thiết. Do vậy, một DTO được ra.
>userDTO.java
```java
    public class userDTO {
        private int Id;
        private String userName;
        private String eMail;
        private String role;

        public userDTO() {
        }

        public userDTO(User user) {
            this.Id = user.getId();
            this.userName = user.getUsername();
            this.eMail = user.getEmail();
            this.role = user.getRole().getName();
        }

        public int getId() {
            return Id;
        }

        public void setId(int id) {
            Id = id;
        }

        public String getUserName() {
            return userName;
        }

        public void setUserName(String userName) {
            this.userName = userName;
        }

        public String geteMail() {
            return eMail;
        }

        public void seteMail(String eMail) {
            this.eMail = eMail;
        }

        public String getRole() {
            return role;
        }

        public void setRole(String role) {
            this.role = role;
        }
    }
```

>RestAPI.java
```java
    @GetMapping(value = "/admin/user")
    public List<userDTO> getAllUser() {
        List<userDTO> userDTOList = new ArrayList<>();
        userDTOList = userService.findAll().stream().map(user -> new userDTO(user)).collect(Collectors.toList());
        return userDTOList;
    }

    @GetMapping(value = "/admin/user/{id}")
    public userDTO getUser(@PathVariable("id") int Id) {
        return new userDTO(userService.findById(Id));
    }
```

`ModelMapper` là công cụ hữu ích để chuyển đổi giữa các Object.

```java
    ModelMapper mapper = new ModelMapper();
    UserDTO userDTO = mapper.map(user, UserDTO.class);
```
## Tham khảo

[https://viblo.asia/p/entity-domain-model-va-dto-sao-nhieu-qua-vay-YWOZroMPlQ0](https://viblo.asia/p/entity-domain-model-va-dto-sao-nhieu-qua-vay-YWOZroMPlQ0)

[https://www.petrikainulainen.net/software-development/design/understanding-spring-web-application-architecture-the-classic-way/](https://www.petrikainulainen.net/software-development/design/understanding-spring-web-application-architecture-the-classic-way/)