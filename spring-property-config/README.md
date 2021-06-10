# Spring property, Spring config
## 1. Spring property
`@PropertySource` annotation dùng để khai báo file config. Nếu không có annotation này, Spring sẽ sử dụng file mặc định (application.properties trong resource)
```java
    @PropertySource("classpath:config.properties")
```
Sử dụng lặp lại annotation này để đọc nhiều file cùng một lúc
```java
    @PropertySources({ 
        @PropertySource("classpath:config1.properties"), 
        @PropertySource("classpath:config2.properties")
    })
```
`@Value` là một annotation có nhiệm vụ inject các giá trị trong file properties vào trong các thuộc tính của Spring
```java
    @Value( "${message}" )
    private String message;
```
Ngoài ra, chúng ra có thể sử dụng `Environment` lấy giá trị trong các file properties
```java
    @Autowired
    private Environment env;
    ...
    System.out.println(env.getProperty("message"));
```

## 2. Spring config
### 2.1. `@Configuration` và `@Bean`
Ở bài 1 [Spring Bean, Spring Component](https://github.com/tienthanh0601/spring-boot/tree/master/spring-component-spring-bean) đã có nhắc tới **Java Based Configuration**. Chúng ta sẽ tìm hiểu kỹ hơn về nó.
- `@Configuration` là một Annotation đánh dấu trên một Class cho phép Spring Boot biết được đây là nơi định nghĩa ra các Bean.
- `@Bean` là một Annotation được đánh dấu trên các method cho phép Spring Boot biết được đây là Bean và sẽ thực hiện đưa Bean này vào Context.
- `@Bean` sẽ nằm trong các class có đánh dấu `@Configuration`.
- Ví dụ:

```java
  class UserService {
      @Autowired
      private PasswordEncoder passwordEncoder 

      public void changePassword(User user, String password) {
          user.setPassword(passwordEncoder.encode(password));
          repo.save(user);
      }
  } 

  @Configuration
  class PasswordEncoderConfiguration {
      @Bean
      public PasswordEncoder passwordEncoder () {
          return new BCryptPasswordEncoder();
      }
  }
```

Như vậy,ta thấy được `PasswordEncoder` là một object được quản lý bởi `Context` của Spring, dù chúng ta không sử dụng `@Component`
### `@Configuration` và `@Bean` hoạt động ra sao?
Ở lần đầu khởi chạy, ngoài việc tìm `@Component`, Spring còn đi tìm các class được đánh dấu `@Configuration`.
- Đi tìm class có đánh dấu `@Configuration`
- Tạo ra đối tượng từ class có đánh dấu `@Configuration`
- Tìm các method có đánh dấu `@Bean` trong đối tượng vừa tạo
- Thực hiện gọi các method có đánh dấu `@Bean` và đưa vào `Context`

### Bổ sung: `@Component` vs `@Bean`
`@Component` là một class-level annotation. Nó yêu cầu Spring sử dụng class này để tạo một bean nếu có nơi nào khác sử dụng class này như một dependency. Việc khởi tạo class hoàn toàn do Spring kiểm soát và chỉ một bean được tạo cho mỗi class.

`@Bean` là một method-level annotation. Nghĩa là, `@Bean` sẽ được đặt trên một method, và method này trả về một Object và sẽ được đăng ký và quản lý bởi `Contex`. `@Bean` trong trường hợp đang sử dụng các class thư viện bên thứ ba, lúc này bạn không thể khai báo `@Component` trên những class này mà bắt buộc bạn phải tạo một method trả về các instance của chúng. 
### 2.2. Không chỉ là cấu hình Bean
Trong thực tế, việc sử dụng `@Configuration` là hết sức cần thiết, và nó đóng vai trò là nơi cấu hình cho toàn bộ ứng dụng của bạn. Một ứng dụng sẽ có nhiều class chứa `@Configuration` và mỗi class sẽ đảm nhận cấu hình một bộ phận gì đó trong ứng dụng.
```java
    @Configuration
    @EnableWebSecurity
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                    .antMatchers("/", "/home").permitAll()
                    .anyRequest().authenticated()
                    .and()
                .formLogin()
                    .loginPage("/login")
                    .permitAll()
                    .and()
                .logout()
                    .permitAll();
        }
```
Ví dụ trên là đoạn code cấu hình cho **Spring Security** mà ta sẽ tìm hiểu ở phần sau.
## Tham khảo
[baeldung.com](https://www.baeldung.com)

[loda.me](https://loda.me/)