# Java Interview Questions

This collection of Java interview questions covers fundamental and advanced topics, including object-oriented programming, collections, streams, exception handling, and database integration. These questions are suitable for assessing candidates for roles involving HR and workforce management systems like elementsuite, which rely on Java for backend development, data processing, and integration with relational databases.

## Basic Java Questions

1. **Write a Java method to calculate the total hours worked by an employee based on a list of shift start and end times (using `LocalDateTime`).**
    - **Tests**: Understanding of Java’s `LocalDateTime`, loops, and basic arithmetic.
    - **Example Answer**:
      ```java
      import java.time.LocalDateTime;
      import java.time.Duration;
      import java.util.List;
 
      public class ShiftCalculator {
          public static long calculateTotalHours(List<LocalDateTime[]> shifts) {
              long totalHours = 0;
              for (LocalDateTime[] shift : shifts) {
                  totalHours += Duration.between(shift[0], shift[1]).toHours();
              }
              return totalHours;
          }
      }
      ```

2. **Explain the difference between `ArrayList` and `LinkedList` in Java’s Collections Framework.**
    - **Tests**: Knowledge of Java collections and their performance characteristics.
    - **Expected Answer**:
        - `ArrayList` uses a dynamic array, offering fast random access (`O(1)`) but slow insertions/deletions in the middle (`O(n)`).
        - `LinkedList` uses a doubly-linked list, with fast insertions/deletions (`O(1)` if the position is known) but slow random access (`O(n)`).
        - Use `ArrayList` for frequent reads, `LinkedList` for frequent modifications.

3. **What is the purpose of the `final` keyword in Java? Provide an example.**
    - **Tests**: Understanding of immutability and `final` usage.
    - **Example Answer**:
        - `final` prevents modification of variables, methods, or classes.
        - Example:
          ```java
          public class Employee {
              final int employeeId = 1001; // Cannot be reassigned
              final void printDetails() { // Cannot be overridden
                  System.out.println("Employee ID: " + employeeId);
              }
          }
          ```

4. **Write a Java method to validate an employee’s email address using a regular expression.**
    - **Tests**: String manipulation and regex usage.
    - **Example Answer**:
      ```java
      import java.util.regex.Pattern;
 
      public class EmailValidator {
          public static boolean isValidEmail(String email) {
              String regex = "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$";
              return Pattern.matches(regex, email);
          }
      }
      ```

## Intermediate Java Questions

5. **Write a Java program to convert a `Stream` of employee names into an `ArrayList` and sort it alphabetically.**
    - **Tests**: Stream API and collections (relates to your earlier question on stream-to-ArrayList conversion).
    - **Example Answer**:
      ```java
      import java.util.ArrayList;
      import java.util.stream.Collectors;
      import java.util.stream.Stream;
 
      public class EmployeeSorter {
          public static ArrayList<String> sortEmployeeNames(Stream<String> nameStream) {
              return nameStream
                  .sorted()
                  .collect(Collectors.toCollection(ArrayList::new));
          }
 
          public static void main(String[] args) {
              Stream<String> names = Stream.of("John", "Alice", "Bob");
              ArrayList<String> sortedNames = sortEmployeeNames(names);
              System.out.println(sortedNames); // [Alice, Bob, John]
          }
      }
      ```

6. **Explain the concept of inheritance in Java with an example relevant to an HR system.**
    - **Tests**: Object-oriented programming (OOP) principles.
    - **Example Answer**:
        - Inheritance allows a class to inherit properties and methods from another class.
        - Example:
          ```java
          public class Employee {
              protected int id;
              protected String name;
              public Employee(int id, String name) {
                  this.id = id;
                  this.name = name;
              }
              public void printDetails() {
                  System.out.println("ID: " + id + ", Name: " + name);
              }
          }
   
          public class Manager extends Employee {
              private String department;
              public Manager(int id, String name, String department) {
                  super(id, name);
                  this.department = department;
              }
              @Override
              public void printDetails() {
                  System.out.println("ID: " + id + ", Name: " + name + ", Dept: " + department);
              }
          }
          ```

7. **How does exception handling work in Java? Write a method that handles a potential `SQLException` when querying an employee database.**
    - **Tests**: Exception handling and JDBC basics.
    - **Example Answer**:
      ```java
      import java.sql.Connection;
      import java.sql.PreparedStatement;
      import java.sql.ResultSet;
      import java.sql.SQLException;
 
      public class EmployeeDAO {
          public String getEmployeeName(Connection conn, int employeeId) throws SQLException {
              String name = null;
              String query = "SELECT first_name FROM EMPLOYEE WHERE employee_id = ?";
              try (PreparedStatement stmt = conn.prepareStatement(query)) {
                  stmt.setInt(1, employeeId);
                  ResultSet rs = stmt.executeQuery();
                  if (rs.next()) {
                      name = rs.getString("first_name");
                  }
              } catch (SQLException e) {
                  System.err.println("Database error: " + e.getMessage());
                  throw e;
              }
              return name;
          }
      }
      ```

8. **Write a Java method to group employees by department using a `Map` from a list of `Employee` objects.**
    - **Tests**: Collections framework and stream grouping.
    - **Example Answer**:
      ```java
      import java.util.List;
      import java.util.Map;
      import java.util.stream.Collectors;
 
      public class EmployeeGrouper {
          public static Map<String, List<Employee>> groupByDepartment(List<Employee> employees) {
              return employees.stream()
                  .collect(Collectors.groupingBy(Employee::getDepartment));
          }
      }
 
      class Employee {
          private String name;
          private String department;
          public Employee(String name, String department) {
              this.name = name;
              this.department = department;
          }
          public String getDepartment() {
              return department;
          }
      }
      ```

## Advanced Java Questions

9. **Write a Java method to process a large dataset of employee shifts concurrently using the `ExecutorService` to calculate total hours worked per employee.**
    - **Tests**: Multithreading and concurrent processing.
    - **Example Answer**:
      ```java
      import java.time.Duration;
      import java.time.LocalDateTime;
      import java.util.List;
      import java.util.Map;
      import java.util.concurrent.ExecutorService;
      import java.util.concurrent.Executors;
      import java.util.concurrent.ConcurrentHashMap;
 
      public class ShiftProcessor {
          public static Map<Integer, Long> calculateHoursConcurrently(List<Shift> shifts) {
              ConcurrentHashMap<Integer, Long> hoursByEmployee = new ConcurrentHashMap<>();
              ExecutorService executor = Executors.newFixedThreadPool(4);
              shifts.forEach(shift -> executor.submit(() -> {
                  long hours = Duration.between(shift.getStartTime(), shift.getEndTime()).toHours();
                  hoursByEmployee.merge(shift.getEmployeeId(), hours, Long::sum);
              }));
              executor.shutdown();
              return hoursByEmployee;
          }
      }
 
      class Shift {
          private int employeeId;
          private LocalDateTime startTime;
          private LocalDateTime endTime;
          public Shift(int employeeId, LocalDateTime startTime, LocalDateTime endTime) {
              this.employeeId = employeeId;
              this.startTime = startTime;
              this.endTime = endTime;
          }
          public int getEmployeeId() { return employeeId; }
          public LocalDateTime getStartTime() { return startTime; }
          public LocalDateTime getEndTime() { return endTime; }
      }
      ```

10. **Explain the difference between `HashMap`, `TreeMap`, and `ConcurrentHashMap`. When would you use each in an HR system?**
    - **Tests**: Advanced collections and concurrency knowledge.
    - **Expected Answer**:
        - `HashMap`: Non-synchronized, fast for single-threaded access; use for in-memory caching of employee data.
        - `TreeMap`: Maintains sorted order, slower due to red-black tree; use for ordered reports (e.g., employees by hire date).
        - `ConcurrentHashMap`: Thread-safe, supports concurrent reads/writes; use for shared data in multi-threaded shift processing.
        - Example: Use `ConcurrentHashMap` in a real-time dashboard for employee schedules.

11. **Write a Java method to integrate with a REST API to fetch employee data and convert the JSON response to a list of `Employee` objects.**
    - **Tests**: HTTP client usage and JSON parsing.
    - **Example Answer**:
      ```java
      import com.fasterxml.jackson.databind.ObjectMapper;
      import java.net.http.HttpClient;
      import java.net.http.HttpRequest;
      import java.net.http.HttpResponse;
      import java.net.URI;
      import java.util.List;

      public class EmployeeApiClient {
          public static List<Employee> fetchEmployees(String apiUrl) throws Exception {
              HttpClient client = HttpClient.newHttpClient();
              HttpRequest request = HttpRequest.newBuilder()
                  .uri(URI.create(apiUrl))
                  .GET()
                  .build();
              HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
              ObjectMapper mapper = new ObjectMapper();
              return mapper.readValue(response.body(), mapper.getTypeFactory().constructCollectionType(List.class, Employee.class));
          }
      }

      class Employee {
          private int id;
          private String name;
          // Getters, setters, and constructor
      }
      ```

12. **How would you implement a custom exception for invalid employee data in an HR system?**
    - **Tests**: Custom exception handling and OOP design.
    - **Example Answer**:
      ```java
      public class InvalidEmployeeDataException extends Exception {
          public InvalidEmployeeDataException(String message) {
              super(message);
          }
      }

      public class EmployeeValidator {
          public static void validateEmployee(Employee employee) throws InvalidEmployeeDataException {
              if (employee.getName() == null || employee.getName().isEmpty()) {
                  throw new InvalidEmployeeDataException("Employee name cannot be empty");
              }
              if (employee.getId() <= 0) {
                  throw new InvalidEmployeeDataException("Employee ID must be positive");
              }
          }
      }
      ```

## Scenario-Based Questions

13. **In an HR system like elementsuite, design a Java class to manage employee schedules, ensuring thread safety for concurrent updates.**
    - **Tests**: Thread safety, OOP, and collections.
    - **Example Answer**:
      ```java
      import java.time.LocalDateTime;
      import java.util.concurrent.ConcurrentHashMap;

      public class ScheduleManager {
          private final ConcurrentHashMap<Integer, Schedule> schedules = new ConcurrentHashMap<>();

          public void addShift(int employeeId, LocalDateTime start, LocalDateTime end) {
              schedules.computeIfAbsent(employeeId, k -> new Schedule()).addShift(start, end);
          }

          public Schedule getSchedule(int employeeId) {
              return schedules.getOrDefault(employeeId, new Schedule());
          }
      }

      class Schedule {
          private final List<Shift> shifts = Collections.synchronizedList(new ArrayList<>());
          public void addShift(LocalDateTime start, LocalDateTime end) {
              shifts.add(new Shift(start, end));
          }
          public List<Shift> getShifts() {
              return new ArrayList<>(shifts);
          }
      }
      ```

14. **Write a Java method to process a CSV file containing employee data and insert valid records into a database using JDBC.**
    - **Tests**: File handling, JDBC, and error handling.
    - **Example Answer**:
      ```java
      import java.io.BufferedReader;
      import java.io.FileReader;
      import java.sql.Connection;
      import java.sql.PreparedStatement;

      public class EmployeeImporter {
          public static void importEmployees(Connection conn, String csvFile) throws Exception {
              String sql = "INSERT INTO EMPLOYEE (employee_id, first_name, last_name) VALUES (?, ?, ?)";
              try (BufferedReader br = new BufferedReader(new FileReader(csvFile));
                   PreparedStatement stmt = conn.prepareStatement(sql)) {
                  String line;
                  while ((line = br.readLine()) != null) {
                      String[] data = line.split(",");
                      stmt.setInt(1, Integer.parseInt(data[0]));
                      stmt.setString(2, data[1]);
                      stmt.setString(3, data[2]);
                      stmt.executeUpdate();
                  }
              } catch (Exception e) {
                  throw new RuntimeException("Failed to import employees: " + e.getMessage());
              }
          }
      }
      ```

15. **How would you optimize a Java application that processes large employee datasets (e.g., shift calculations) to improve performance?**
    - **Tests**: Performance optimization and best practices.
    - **Expected Answer**:
        - Use `Stream` API with parallel processing for large datasets.
        - Leverage `ConcurrentHashMap` for thread-safe data aggregation.
        - Minimize database calls by batching inserts/updates.
        - Use connection pooling for JDBC operations.
        - Profile with tools like VisualVM to identify bottlenecks.
        - Example optimization:
          ```java
          List<Shift> shifts = // large dataset
          Map<Integer, Long> hours = shifts.parallelStream()
              .collect(Collectors.groupingBy(
                  Shift::getEmployeeId,
                  Collectors.summingLong(s -> Duration.between(s.getStartTime(), s.getEndTime()).toHours())
              ));
          ```

## Notes
- These questions assume Java 17 or later, commonly used in enterprise systems like elementsuite for backend processing and database integration.
- Candidates should explain their code, focusing on readability, maintainability, and performance considerations.
- For advanced roles, consider asking about Spring Framework, JPA, or microservices to assess enterprise-level Java expertise.