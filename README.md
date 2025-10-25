[![Build Status](../../actions/workflows/CI.yml/badge.svg)](../../actions/workflows/CI.yml)

# Web Engineering 2025-2026 / Lab 3: Complete a Web API

A minimal Spring Boot + Kotlin REST API demonstrating HTTP method semantics (safety and idempotency) through comprehensive testing. This project implements a complete test suite for a basic Employee management API.

## Tech Stack
- **Spring Boot** 3.5.3
- **Kotlin** 2.2.10
- **Java** 21 (toolchain)
- **Gradle** 9.0.0
- **MockK** for mocking
- **Spring MockMvc** for testing

## Prerequisites
- Java 21 or higher
- Git

## Quick Start
```bash
# Clone the repository
git clone <repository-url>
cd lab-3-restful-ws

# Build the project
./gradlew clean build

# Run tests
./gradlew test

# Run the application
./gradlew bootRun
# Default: http://localhost:8080
```

## Project Structure
```
src/
├── main/kotlin/es/unizar/webeng/lab3/
│   ├── Application.kt          # Spring Boot application entry point
│   ├── Controller.kt           # REST controller with CRUD endpoints
│   ├── Employee.kt             # Employee entity
│   └── EmployeeRepository.kt   # JPA repository
└── test/kotlin/es/unizar/webeng/lab3/
    └── ControllerTests.kt      # Complete test suite

```

## API Endpoints

The application exposes the following REST endpoints:

- `POST /employees` - Create a new employee (Not safe, Not idempotent)
- `GET /employees/{id}` - Retrieve an employee (Safe, Idempotent)
- `PUT /employees/{id}` - Create or update an employee (Not safe, Idempotent)
- `DELETE /employees/{id}` - Delete an employee (Not safe, Idempotent)

## Assignment Completion

This project successfully implements all required tests demonstrating:

### HTTP Method Properties

1. **POST Test** - Demonstrates that POST is neither safe nor idempotent:
   - Each request creates a new resource with a different ID
   - Verifies that `save()` is called multiple times with different results

2. **GET Test** - Demonstrates that GET is both safe and idempotent:
   - Multiple identical requests return the same result
   - No repository modification methods are called
   - Properly handles 404 for non-existent resources

3. **PUT Test** - Demonstrates that PUT is idempotent but not safe:
   - First request creates the resource (201 Created)
   - Subsequent identical requests update with the same result (200 OK)
   - Verifies `findById()` is called to check existence

4. **DELETE Test** - Demonstrates that DELETE is idempotent but not safe:
   - Multiple delete requests have the same effect
   - Verifies `deleteById()` is called correctly
   - No other repository methods are invoked

## Testing

### Run All Tests
```bash
./gradlew test
```

### Run with Coverage
```bash
./gradlew test jacocoTestReport
```

### Test Output
All tests validate:
- Correct HTTP status codes
- Proper response headers (Location, Content-Location)
- Expected JSON response bodies
- Repository method invocation counts
- Safety and idempotency properties

## Code Quality and Formatting

This project uses **Ktlint** for code formatting:
```bash
# Format code
./gradlew ktlintFormat

# Check formatting
./gradlew ktlintCheck
```

**Important**: If Ktlint modifies your code during formatting, the build will fail. Always run `ktlintFormat` before building.

## Implementation Details

### Mocking Strategy
Tests use MockK to simulate repository behavior:
- `every { }` blocks for method stubbing
- `andThenAnswer { }` for sequential responses
- `justRun { }` for void methods
- `verify(exactly = N)` for invocation counting

### Test Scenarios Covered
- Resource creation with unique IDs
- Successful resource retrieval
- 404 responses for non-existent resources
- Resource creation vs. update (PUT)
- Idempotent deletion
- Method safety verification

## Key Concepts Demonstrated

### Safe Methods
Methods that don't alter server state:
- **GET** - Only reads data, no side effects

### Idempotent Methods
Methods where multiple identical requests have the same effect:
- **GET** - Always returns the same data
- **PUT** - Creates once, then updates to same state
- **DELETE** - Deletes once, subsequent calls have no additional effect

### Non-Idempotent Methods
Methods where each request may produce different results:
- **POST** - Each call creates a new resource with a new ID

## Lessons Learned

1. **REST Semantics**: Deep understanding of HTTP method properties and their practical implications
2. **Test-Driven Design**: How tests document and enforce expected API behavior
3. **Mocking Frameworks**: Effective use of MockK for isolating units under test
4. **Spring Testing**: Integration between Spring's test framework and MockK

## CI/CD

The project includes GitHub Actions workflow that:
- Builds the project
- Runs all tests
- Validates code formatting
- Generates test reports

Check the build status badge at the top of this README.

## Documentation

For detailed assignment instructions and code block explanations, see:
- `docs/GUIDE.md` - Complete assignment guide
- `REPORT.md` - Project completion report

## Author

Completed as part of Web Engineering 2025-2026 course assignment.

## License

Educational project - see course materials for details.