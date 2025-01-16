
# JUnit 5 (Jupiter API) Testing with ContactManager

This project demonstrates how to use **JUnit 5** for unit testing a simple `ContactManager` Java class. It provides examples of different types of tests, including standard tests, parameterized tests, nested tests, and lifecycle callbacks.

---

## Project Structure

The test class is organized under the `test.java` package and depends on a main class in the `main.java` package. The tests are written for a `ContactManager` class responsible for managing contacts with attributes like `firstName`, `lastName`, and `phoneNumber`.

---

## Features of the Test Class

### 1. **Lifecycle Methods**
- `@BeforeAll` and `@AfterAll`: Execute code once before and after all tests.
- `@BeforeEach` and `@AfterEach`: Execute code before and after each test.

### 2. **Standard Unit Tests**
- Test methods are annotated with `@Test` and have custom display names using `@DisplayName`.
- Example:
  ```java
  @Test
  @DisplayName("Should Create Contact")
  public void shouldCreateContact() {
      contactManager.addContact("John", "Doe", "0123456789");
      Assertions.assertFalse(contactManager.getAllContacts().isEmpty());
      Assertions.assertEquals(1, contactManager.getAllContacts().size());
  }
  ```

### 3. **Parameterized Tests**
- Tests are repeated with multiple sets of inputs.
- Types used:
  - `@MethodSource`: Provides data from a static method.
  - `@CsvFileSource`: Reads data from a CSV file.

- Example with `@MethodSource`:
  ```java
  @DisplayName("Phone Number should match the required Format")
  @ParameterizedTest
  @MethodSource("phoneNumberList")
  public void shouldTestPhoneNumberFormatUsingMethodSource(String phoneNumber) {
      contactManager.addContact("John", "Doe", phoneNumber);
      Assertions.assertFalse(contactManager.getAllContacts().isEmpty());
  }

  private static List<String> phoneNumberList() {
      return Arrays.asList("0123456789", "0123456798", "0123456897");
  }
  ```

### 4. **Nested Tests**
- Organizes related test cases in logical groups.
- Example:
  ```java
  @Nested
  class RepeatedTests {
      @DisplayName("Repeat Contact Creation Test 5 Times")
      @RepeatedTest(5)
      public void shouldTestContactCreationRepeatedly() {
          contactManager.addContact("John", "Doe", "0123456789");
          Assertions.assertEquals(1, contactManager.getAllContacts().size());
      }
  }
  ```

### 5. **Disabled Tests**
- Tests can be temporarily disabled with `@Disabled`.
- Example:
  ```java
  @Test
  @DisplayName("Test Should Be Disabled")
  @Disabled
  public void shouldBeDisabled() {
      throw new RuntimeException("Test Should Not be executed");
  }
  ```

---

## Testing Features Demonstrated

1. **Validation of Input Data**:
   - Ensures `firstName`, `lastName`, and `phoneNumber` are non-null.
   - Throws `RuntimeException` for invalid inputs.

2. **Stream Operations**:
   - Verifies contact existence using Java Streams.
   - Example:
     ```java
     Assertions.assertTrue(contactManager.getAllContacts().stream()
             .anyMatch(contact -> contact.getFirstName().equals("John")));
     ```

3. **Parameterized Data Testing**:
   - Validates multiple phone numbers from a CSV file.

4. **JUnit Lifecycle Annotations**:
   - Demonstrates efficient resource setup and teardown for tests.

---

## How to Run the Tests

1. Clone the repository.
2. Ensure you have Java and JUnit 5 configured in your IDE or build tool (e.g., Maven/Gradle).
3. Run the test class `ContactManagerTest` using your preferred method (IDE or command-line).

---

## Example CSV File (`data.csv`)

Ensure a file named `data.csv` exists in the `resources` directory with content like:
```csv
0123456789
0123456123
0123456456
```

---

## Key Annotations Used

- `@Test`: Marks a test method.
- `@BeforeAll` / `@AfterAll`: Executes code once before/after all tests.
- `@BeforeEach` / `@AfterEach`: Executes code before/after each test.
- `@DisplayName`: Adds descriptive names for tests.
- `@ParameterizedTest`: Executes tests with different parameters.
- `@Nested`: Groups related tests.
- `@Disabled`: Disables specific tests.

---

## Output Examples

1. Successful test:
   ```
   Instantiating Contact Manager
   Should Create Contact
   Should Execute After Each Test...
   ```

2. Failed test:
   ```
   java.lang.RuntimeException: First Name cannot be null
   ```

---

## Conclusion

This project serves as an educational resource for writing comprehensive unit tests in JUnit 5. By using parameterized tests, nested tests, and lifecycle methods, it ensures thorough and maintainable test coverage for the `ContactManager` class.
