# Clean Code: A Handbook of Agile Software Craftsmanship

## Table of Contents

- [Introduction](#introduction)
- [Chapter 1: Clean Code](#chapter-1-clean-code)
- [Chapter 2: Meaningful Names](#chapter-2-meaningful-names)
- [Chapter 3: Functions](#chapter-3-functions)
- [Chapter 4: Comments](#chapter-4-comments)
- [Chapter 5: Formatting](#chapter-5-formatting)
- [Chapter 6: Objects and Data Structures](#chapter-6-objects-and-data-structures)
- [Chapter 7: Error Handling](#chapter-7-error-handling)
- [Chapter 8: Boundaries](#chapter-8-boundaries)
- [Chapter 9: Unit Tests](#chapter-9-unit-tests)
- [Chapter 10: Classes](#chapter-10-classes)
- [Chapter 11: Systems](#chapter-11-systems)
- [Chapter 12: Emergence](#chapter-12-emergence)
- [Conclusion](#conclusion)
- [Shortcuts](#shortcuts)
- [Further Reading](#further-reading)

---

## Introduction

"Clean Code" by Robert C. Martin (Uncle Bob) is a seminal work that emphasizes the importance of writing code that is not only functional but also readable and maintainable. This guide summarizes the key principles and practices outlined in the book, providing developers with actionable advice to improve code quality.

---

## Chapter 1: Clean Code

- **Definition of Clean Code:**
  - Simple and direct.
  - Reads like well-written prose.
  - Focuses on one thing at a time.
  - Contains minimal duplication.
  - Expresses the intent of the programmer clearly.

- **Importance:**
  - Enhances maintainability.
  - Reduces the cost of change.
  - Improves team collaboration.

**Example:**

```java
// Clean Code Example
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

This code is simple, direct, and expresses the programmer's intent clearly.

---

## Chapter 2: Meaningful Names

- **Use Intention-Revealing Names:**
  - Choose names that answer the "why" and "what" questions.

  **Bad:**

  ```java
  int d; // elapsed time in days
  ```

  **Good:**

  ```java
  int elapsedTimeInDays;
  ```

- **Avoid Disinformation:**
  - Don't use names that could be misleading.

  **Bad:**

  ```java
  int accountList; // It's actually a single account, not a list
  ```

  **Good:**

  ```java
  int account;
  ```

- **Make Meaningful Distinctions:**
  - Don't differentiate names with numbers or irrelevant details.

  **Bad:**

  ```java
  getData();
  getData1();
  ```

  **Good:**

  ```java
  getCustomerData();
  getOrderData();
  ```

- **Use Pronounceable Names:**
  - Easier to communicate about the code.

  **Bad:**

  ```java
  class DtaRcrd102 {
      private Date genymdhms;
      private Date modymdhms;
      // ...
  }
  ```

  **Good:**

  ```java
  class CustomerRecord {
      private Date generationTimestamp;
      private Date modificationTimestamp;
      // ...
  }
  ```

- **Use Searchable Names:**
  - Single-letter names are hard to find in code.

  **Bad:**

  ```java
  for (int i = 0; i < 10; i++) {
      // ...
  }
  ```

  **Good:**

  ```java
  for (int userIndex = 0; userIndex < maxUsers; userIndex++) {
      // ...
  }
  ```

- **Avoid Encodings:**
  - Don't add unnecessary prefixes or Hungarian notation.

  **Bad:**

  ```java
  String strName;
  int iCount;
  ```

  **Good:**

  ```java
  String name;
  int count;
  ```

---

## Chapter 3: Functions

- **Small Functions:**
  - Functions should be small, ideally no more than 20 lines.
  - Do one thing and do it well.

  **Example:**

  ```java
  // Good: Small, focused function
  public void renderOrderPage() {
      retrieveOrder();
      calculatePrices();
      generatePage();
  }
  ```

- **Single Responsibility:**
  - Each function should have one responsibility or reason to change.

- **Descriptive Names:**
  - Function names should clearly indicate their purpose.
  - Use verb phrases for functions.

  **Bad:**

  ```java
  void process();
  ```

  **Good:**

  ```java
  void calculateInvoiceTotal();
  ```

- **Function Arguments:**
  - Fewer arguments are better; aim for zero to two.
  - Avoid flag arguments.

  **Bad:**

  ```java
  void saveUser(String name, boolean isAdmin);
  ```

  **Good:**

  ```java
  void saveUser(User user);
  ```

- **Avoid Side Effects:**
  - Functions should not alter the state of external variables unexpectedly.

  **Bad:**

  ```java
  int calculateTotalPrice(Order order) {
      // Modifies global discount variable
      discount = 0.1;
      return order.getSubtotal() - (order.getSubtotal() * discount);
  }
  ```

  **Good:**

  ```java
  int calculateTotalPrice(Order order, double discountRate) {
      return order.getSubtotal() - (order.getSubtotal() * discountRate);
  }
  ```

- **Use Exceptions for Error Handling:**
  - Don't return error codes; throw exceptions instead.

  **Bad:**

  ```java
  int deleteFile(String filePath) {
      if (fileExists(filePath)) {
          // Delete file
          return 0; // Success
      } else {
          return -1; // Error code
      }
  }
  ```

  **Good:**

  ```java
  void deleteFile(String filePath) throws FileNotFoundException {
      if (!fileExists(filePath)) {
          throw new FileNotFoundException("File not found: " + filePath);
      }
      // Delete file
  }
  ```

- **Don't Repeat Yourself (DRY):**
  - Avoid code duplication by abstracting common logic into functions.

  **Bad:**

  ```java
  void createAdminUser(String name, String email) {
      // Validation logic
      if (name == null || name.isEmpty()) {
          throw new IllegalArgumentException("Name is required");
      }
      if (email == null || email.isEmpty()) {
          throw new IllegalArgumentException("Email is required");
      }
      // Create admin user
  }

  void createRegularUser(String name, String email) {
      // Same validation logic duplicated
      if (name == null || name.isEmpty()) {
          throw new IllegalArgumentException("Name is required");
      }
      if (email == null || email.isEmpty()) {
          throw new IllegalArgumentException("Email is required");
      }
      // Create regular user
  }
  ```

  **Good:**

  ```java
  void validateUserInput(String name, String email) {
      if (name == null || name.isEmpty()) {
          throw new IllegalArgumentException("Name is required");
      }
      if (email == null || email.isEmpty()) {
          throw new IllegalArgumentException("Email is required");
      }
  }

  void createAdminUser(String name, String email) {
      validateUserInput(name, email);
      // Create admin user
  }

  void createRegularUser(String name, String email) {
      validateUserInput(name, email);
      // Create regular user
  }
  ```

---

## Chapter 4: Comments

- **Comments Are a Failure to Express:**
  - Code should be self-explanatory.
  - Use comments to explain "why," not "what."

- **Good Comments:**

  - **Legal Comments:**

    ```java
    /**
     * Copyright (C) 2023 Acme Corp.
     * All rights reserved.
     */
    ```

  - **Informative Comments:**

    ```java
    // Use binary search since the list is sorted
    int index = Collections.binarySearch(sortedList, key);
    ```

  - **Explanation of Intent:**

    ```java
    // We subtract one to convert from 1-based indexing to 0-based indexing
    int arrayIndex = userInput - 1;
    ```

- **Bad Comments:**

  - **Redundant Comments:**

    ```java
    // Increment i by 1
    i++;
    ```

  - **Misleading Comments:**

    ```java
    // This method is deprecated
    void newMethod() {
        // Actually, this method is not deprecated
    }
    ```

  - **Noise Comments:**

    ```java
    // Default constructor
    public MyClass() {
    }
    ```

- **Avoid Commented-Out Code:**

  ```java
  // Old implementation
  // int calculateSum(int a, int b) {
  //     return a + b;
  // }

  // New implementation
  int calculateSum(int a, int b) {
      return Math.addExact(a, b);
  }
  ```

  Instead, remove the commented-out code entirely.

---

## Chapter 5: Formatting

- **Consistent Formatting:**
  - Use a consistent style guide.
  - Improves readability and collaboration.

- **Vertical Formatting:**

  **Bad:**

  ```java
  public class User { public String name; public String email; public String getName() { return name; } public String getEmail() { return email; } }
  ```

  **Good:**

  ```java
  public class User {
      private String name;
      private String email;

      public String getName() {
          return name;
      }

      public String getEmail() {
          return email;
      }
  }
  ```

- **Horizontal Formatting:**

  **Bad:**

  ```java
  public void calculateTotalPrice(Order order,double discountRate){return order.getSubtotal()-(order.getSubtotal()*discountRate);}
  ```

  **Good:**

  ```java
  public void calculateTotalPrice(Order order, double discountRate) {
      return order.getSubtotal() - (order.getSubtotal() * discountRate);
  }
  ```

- **Indentation and Bracing:**

  **Bad:**

  ```java
  if (isValid){
  doSomething();
  }else{
  doSomethingElse();
  }
  ```

  **Good:**

  ```java
  if (isValid) {
      doSomething();
  } else {
      doSomethingElse();
  }
  ```

---

## Chapter 6: Objects and Data Structures

- **Encapsulation:**
  - Hide internal representation and expose operations.

  **Bad:**

  ```java
  public class Point {
      public double x;
      public double y;
  }
  ```

  **Good:**

  ```java
  public class Point {
      private double x;
      private double y;

      public Point(double x, double y) {
          this.x = x;
          this.y = y;
      }

      public double getX() {
          return x;
      }

      public double getY() {
          return y;
      }

      public void move(double deltaX, double deltaY) {
          this.x += deltaX;
          this.y += deltaY;
      }
  }
  ```

- **Data/Object Anti-Symmetry:**
  - Objects hide data and expose behavior.
  - Data structures expose data and have minimal behavior.

- **The Law of Demeter:**
  - A method should only call methods of:
    - Itself.
    - Its parameters.
    - Any objects it creates.
    - Its direct components.

  **Bad (Violates Law of Demeter):**

  ```java
  // Method in class A
  void processOrder(Order order) {
      Customer customer = order.getCustomer();
      Address address = customer.getAddress();
      String street = address.getStreet();
      // ...
  }
  ```

  **Good:**

  ```java
  // Method in class A
  void processOrder(Order order) {
      String street = order.getDeliveryStreet();
      // ...
  }

  // In class Order
  public String getDeliveryStreet() {
      return customer.getAddress().getStreet();
  }
  ```

---

## Chapter 7: Error Handling

- **Use Exceptions, Not Return Codes:**
  - Throw exceptions to signal errors.
  - Makes error handling explicit.

  **Bad:**

  ```java
  int withdraw(double amount) {
      if (amount > balance) {
          return -1; // Error code for insufficient funds
      }
      balance -= amount;
      return 0; // Success
  }
  ```

  **Good:**

  ```java
  void withdraw(double amount) throws InsufficientFundsException {
      if (amount > balance) {
          throw new InsufficientFundsException();
      }
      balance -= amount;
  }
  ```

- **Define Exception Classes:**
  - Use specific exceptions for different error types.

  ```java
  public class InsufficientFundsException extends Exception {
      public InsufficientFundsException() {
          super("Insufficient funds in account");
      }
  }
  ```

- **Don't Ignore Exceptions:**

  **Bad:**

  ```java
  try {
      // Code that may throw IOException
  } catch (IOException e) {
      // Do nothing
  }
  ```

  **Good:**

  ```java
  try {
      // Code that may throw IOException
  } catch (IOException e) {
      logger.error("Failed to read file", e);
      throw new FileReadException("Could not read file", e);
  }
  ```

- **Provide Context:**
  - Include messages and context when throwing exceptions.

  ```java
  if (orderId == null) {
      throw new IllegalArgumentException("Order ID cannot be null");
  }
  ```

---

## Chapter 8: Boundaries

- **Encapsulate External Libraries:**
  - Create a boundary layer between your code and third-party code.

  **Example:**

  ```java
  // Third-party API
  Map<String, Object> response = externalService.call(parameters);

  // Encapsulated in your code
  User user = userService.getUser(userId);
  ```

- **Use Dependency Injection:**
  - Inject dependencies rather than hard-coding them.

  ```java
  public class UserService {
      private ExternalService externalService;

      public UserService(ExternalService externalService) {
          this.externalService = externalService;
      }
  }
  ```

- **Write Integration Tests:**
  - Ensure that interactions with external systems work as expected.

  ```java
  @Test
  public void testExternalServiceIntegration() {
      // Setup test with mock external service
      // Verify that your code handles responses correctly
  }
  ```

---

## Chapter 9: Unit Tests

- **Importance of Testing:**
  - Tests are as important as production code.
  - They provide a safety net for changes.

- **Clean Tests:**
  - Tests should be readable and maintainable.
  - Follow the same clean code principles.

- **Structure of a Test:**

  ```java
  @Test
  public void testCalculateTotalPrice() {
      // Arrange
      Order order = new Order();
      order.addItem(new Item("Apple", 1.00), 3);
      order.addItem(new Item("Banana", 0.50), 2);

      // Act
      double totalPrice = order.calculateTotalPrice();

      // Assert
      assertEquals(4.00, totalPrice, 0.001);
  }
  ```

- **Test Coverage:**
  - Aim for high coverage but focus on meaningful tests.

- **First Principles (F.I.R.S.T):**
  - **Fast:** Tests should run quickly.
  - **Independent:** Tests should not depend on each other.
  - **Repeatable:** Tests should yield the same results every time.
  - **Self-Validating:** Tests should have a clear pass/fail outcome.
  - **Timely:** Write tests before production code.

---

## Chapter 10: Classes

- **Small Classes:**
  - Classes should be small and focused on a single responsibility.

  **Bad:**

  ```java
  public class GodClass {
      void handleUserInput() { /* ... */ }
      void renderUI() { /* ... */ }
      void processData() { /* ... */ }
      void manageDatabaseConnection() { /* ... */ }
  }
  ```

  **Good:**

  - Separate classes for each responsibility:

    ```java
    public class InputHandler { /* ... */ }
    public class Renderer { /* ... */ }
    public class DataProcessor { /* ... */ }
    public class DatabaseManager { /* ... */ }
    ```

- **Single Responsibility Principle (SRP):**
  - A class should have only one reason to change.

- **Cohesion:**
  - Methods within a class should be related.
  - High cohesion means better maintainability.

- **Open/Closed Principle:**
  - Classes should be open for extension but closed for modification.

  **Example:**

  ```java
  public abstract class Shape {
      public abstract double area();
  }

  public class Circle extends Shape {
      private double radius;

      @Override
      public double area() {
          return Math.PI * radius * radius;
      }
  }

  public class Square extends Shape {
      private double side;

      @Override
      public double area() {
          return side * side;
      }
  }
  ```

---

## Chapter 11: Systems

- **Separate Construction from Use:**
  - Build systems by assembling components.
  - Use factories or dependency injection.

  **Example with Dependency Injection:**

  ```java
  public class Application {
      public static void main(String[] args) {
          Service service = new ServiceImpl();
          Controller controller = new Controller(service);
          controller.start();
      }
  }
  ```

- **Use Facades:**
  - Simplify complex systems with a simplified interface.

  **Example:**

  ```java
  public class DatabaseFacade {
      public void saveOrder(Order order) {
          // Complex database operations hidden behind simple method
      }
  }
  ```

- **Prefer Plain Old Java Objects (POJOs):**
  - Keep components decoupled from frameworks.

  **Bad:**

  ```java
  public class UserEntity extends HibernateEntity {
      // Tightly coupled with Hibernate
  }
  ```

  **Good:**

  ```java
  public class User {
      // Plain Java object
  }
  ```

---

## Chapter 12: Emergence

- **Simple Design:**
  - The best designs emerge from applying simple rules consistently.

- **Four Rules of Simple Design (Beck):**
  1. **Passes all tests.**
  2. **Minimizes duplication.**
  3. **Maximizes clarity.**
  4. **Keeps the code minimal.**

- **Refactoring:**
  - Continuously improve code through small, incremental changes.

  **Example:**

  - Identify duplicated code.
  - Extract methods or classes.
  - Simplify complex methods.

---

## Conclusion

Writing clean code is an ongoing discipline that requires attention to detail and a commitment to excellence. By applying the principles outlined in this guide, developers can create codebases that are easier to understand, maintain, and extend. Clean code leads to better software quality, happier teams, and more successful projects.

---

## Shortcuts

- **DRY:** Don't Repeat Yourself
- **SRP:** Single Responsibility Principle
- **KISS:** Keep It Simple, Stupid
- **YAGNI:** You Aren't Gonna Need It
- **F.I.R.S.T:** Fast, Independent, Repeatable, Self-Validating, Timely
- **SOLID Principles:**
  - **S**ingle Responsibility Principle
  - **O**pen/Closed Principle
  - **L**iskov Substitution Principle
  - **I**nterface Segregation Principle
  - **D**ependency Inversion Principle

---

# Short Summary

- **Meaningful Names:** Use clear, descriptive names for variables, functions, and classes.
- **Small Functions:** Keep functions short and focused on a single task.
- **Comments:** Write comments that add value and explain "why," not "what."
- **Formatting:** Consistent code formatting enhances readability.
- **Error Handling:** Use exceptions appropriately and provide context.
- **Unit Tests:** Write clean, effective tests that follow the F.I.R.S.T principles.
- **Classes:** Apply the Single Responsibility Principle to classes.
- **Systems:** Separate concerns and use design patterns to manage complexity.
- **Refactoring:** Continuously improve your codebase through refactoring.
- **Clean Code Culture:** Cultivate a team environment that values and practices clean code principles.
