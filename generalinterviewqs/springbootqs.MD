# Spring Boot Interview Questions

This collection of Spring Boot interview questions covers fundamental and advanced topics, including configuration, REST APIs, data access, security, and performance optimization. These questions are suitable for assessing candidates for roles involving HR and workforce management systems like elementsuite, which likely use Spring Boot for backend development, API services, and database integration.

## Basic Spring Boot Questions

1. **What is Spring Boot, and how does it simplify development compared to traditional Spring?**
    - **Tests**: Understanding of Spring Boot’s core features and benefits.
    - **Expected Answer**:
        - Spring Boot is a framework that simplifies Spring application development with auto-configuration, starter dependencies, and embedded servers.
        - It reduces boilerplate code, provides production-ready features (e.g., monitoring via Actuator), and simplifies dependency management compared to traditional Spring’s manual configuration.
        - Example: Spring Boot auto-configures a DataSource for database connectivity based on classpath dependencies.

2. **Write a Spring Boot REST controller to retrieve a list of employees from a service.**
    - **Tests**: Knowledge of REST API creation and controller annotations.
    - **Example Answer**:
      ```java
      import org.springframework.web.bind.annotation.GetMapping;
      import org.springframework.web.bind.annotation.RestController;
      import java.util.List;
 
      @RestController
      public class EmployeeController {
          private final EmployeeService employeeService;
 
          public EmployeeController(EmployeeService employeeService) {
              this.employeeService = employeeService;
          }
 
          @GetMapping("/api/employees")
          public List<Employee> getAllEmployees() {
              return employeeService.getAllEmployees();
          }
      }
 
      class Employee {
          private int id;
          private String name;
          // Getters, setters, constructor
      }
      ```

3. **Explain the purpose of the `@SpringBootApplication` annotation.**
    - **Tests**: Understanding of Spring Boot’s core annotations.
    - **Expected Answer**:
        - `@SpringBootApplication` is a convenience annotation combining `@EnableAutoConfiguration`, `@ComponentScan`, and `@Configuration`.
        - It enables auto-configuration, scans for components in the package, and marks the class as a configuration source.
        - Example: Used in the main application class to bootstrap a Spring Boot app.

4. **How do you configure a database connection in Spring Boot using `application.properties`?**
    - **Tests**: Knowledge of Spring Boot configuration and properties.
    - **Example Answer**:
        - Configure in `application.properties`:
          ```properties
          spring.datasource.url=jdbc:mysql://localhost:3306/hrdb
          spring.datasource.username=root
          spring.datasource.password=password
          spring.jpa.hibernate.ddl-auto=update
          ```
        - Spring Boot auto-configures a `DataSource` and JPA based on these properties.

## Intermediate Spring Boot Questions

5. **Write a Spring Boot service to fetch employees from a database using Spring Data JPA.**
    - **Tests**: Spring Data JPA and repository usage.
    - **Example Answer**:
      ```java
      import org.springframework.stereotype.Service;
      import org.springframework.beans.factory.annotation.Autowired;
      import java.util.List;
 
      @Service
      public class EmployeeService {
          private final EmployeeRepository repository;
 
          @Autowired
          public EmployeeService(EmployeeRepository repository) {
              this.repository = repository;
          }
 
          public List<Employee> getAllEmployees() {
              return repository.findAll();
          }
      }
 
      import org.springframework.data.jpa.repository.JpaRepository;
 
      public interface EmployeeRepository extends JpaRepository<Employee, Integer> {
      }
 
      @Entity
      public class Employee {
          @Id
          private int id;
          private String name;
          // Getters, setters
      }
      ```

6. **Explain how dependency injection works in Spring Boot and provide an example.**
    - **Tests**: Understanding of dependency injection (DI) and Spring’s IoC container.
    - **Example Answer**:
        - DI allows Spring to manage and inject dependencies into beans, reducing coupling.
        - Example (from Q5):
          ```java
          @Service
          public class EmployeeService {
              private final EmployeeRepository repository;
              @Autowired
              public EmployeeService(EmployeeRepository repository) {
                  this.repository = repository;
              }
          }
          ```
        - Spring injects `EmployeeRepository` into `EmployeeService` via constructor injection.

7. **Write a Spring Boot endpoint to handle employee creation with validation.**
    - **Tests**: REST API development, validation, and error handling.
    - **Example Answer**:
      ```java
      import jakarta.validation.Valid;
      import jakarta.validation.constraints.NotBlank;
      import org.springframework.http.ResponseEntity;
      import org.springframework.web.bind.annotation.PostMapping;
      import org.springframework.web.bind.annotation.RequestBody;
      import org.springframework.web.bind.annotation.RestController;
 
      @RestController
      public class EmployeeController {
          private final EmployeeService employeeService;
 
          public EmployeeController(EmployeeService employeeService) {
              this.employeeService = employeeService;
          }
 
          @PostMapping("/api/employees")
          public ResponseEntity<Employee> createEmployee(@Valid @RequestBody EmployeeDTO employeeDTO) {
              Employee employee = employeeService.save(employeeDTO);
              return ResponseEntity.ok(employee);
          }
      }
 
      public class EmployeeDTO {
          @NotBlank(message = "Name is required")
          private String name;
          // Getters, setters
      }
      ```

8. **How does Spring Boot handle exception handling globally? Provide an example.**
    - **Tests**: Global exception handling with `@ControllerAdvice`.
    - **Example Answer**:
      ```java
      import org.springframework.http.HttpStatus;
      import org.springframework.http.ResponseEntity;
      import org.springframework.web.bind.annotation.ControllerAdvice;
      import org.springframework.web.bind.annotation.ExceptionHandler;
 
      @ControllerAdvice
      public class GlobalExceptionHandler {
          @ExceptionHandler(IllegalArgumentException.class)
          public ResponseEntity<String> handleIllegalArgument(IllegalArgumentException ex) {
              return new ResponseEntity<>("Error: " + ex.getMessage(), HttpStatus.BAD_REQUEST);
          }
      }
      ```

## Advanced Spring Boot Questions

9. **Write a Spring Boot configuration to schedule a task to archive old employee records daily.**
    - **Tests**: Task scheduling and Spring Boot configuration.
    - **Example Answer**:
      ```java
      import org.springframework.scheduling.annotation.Scheduled;
      import org.springframework.stereotype.Component;
      import org.springframework.beans.factory.annotation.Autowired;
 
      @Component
      @EnableScheduling
      public class ArchiveScheduler {
          private final EmployeeService employeeService;
 
          @Autowired
          public ArchiveScheduler(EmployeeService employeeService) {
              this.employeeService = employeeService;
          }
 
          @Scheduled(cron = "0 0 1 * * ?") // Run daily at 1 AM
          public void archiveOldEmployees() {
              employeeService.archiveEmployeesBefore(LocalDate.of(2025, 1, 1));
          }
      }
      ```

10. **Explain how Spring Security can be used to secure a REST API in Spring Boot. Provide a basic configuration.**
    - **Tests**: Spring Security knowledge and API security.
    - **Example Answer**:
        - Spring Security provides authentication and authorization for APIs.
        - Example:
          ```java
          import org.springframework.context.annotation.Bean;
          import org.springframework.context.annotation.Configuration;
          import org.springframework.security.config.annotation.web.builders.HttpSecurity;
          import org.springframework.security.web.SecurityFilterChain;
  
          @Configuration
          public class SecurityConfig {
              @Bean
              public SecurityFilterChain securityFilterChain(HttpSecurity http delineator) throws Exception {
                  http.authorizeRequests()
                      .requestMatchers("/api/employees/**").authenticated()
                      .and()
                      .httpBasic();
                  return http.build();
              }
          }
          ```
        - This configures HTTP Basic authentication for `/api/employees/**` endpoints.