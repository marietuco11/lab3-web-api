# Lab 3 Complete a Web API -- Project Report

## Description of Changes

This assignment involved completing the test suite for a REST API built with Spring Boot and Kotlin. The main modifications were:

1. **Completed POST Test (`POST is not safe and not idempotent`)**: 
   - Added mock setup to simulate repository behavior returning different IDs (1 and 2) for consecutive POST requests
   - Implemented verification to confirm that `save()` was called exactly twice, demonstrating POST's non-idempotent nature
   - Added necessary imports: `every`, `verify`, `Optional`

2. **Completed GET Test (`GET is safe and idempotent`)**:
   - Set up mocks for two scenarios: existing employee (id=1) and non-existing employee (id=2)
   - Implemented verification to ensure no modification methods (`save`, `deleteById`, `findAll`) were called
   - This demonstrates GET's safety property (no side effects on server state)

3. **Completed PUT Test (`PUT is idempotent but not safe`)**:
   - Configured sequential mock responses: first call returns empty (creation), second returns the employee (update)
   - Added mock for `save()` operation to return the same employee consistently
   - Verified that `findById` was called exactly twice, showing PUT's idempotent behavior

4. **Completed DELETE Test (`DELETE is idempotent but not safe`)**:
   - Used `justRun` to allow void `deleteById()` method calls
   - Verified that `deleteById` was called twice with no other repository methods invoked
   - Demonstrates DELETE's idempotent nature (repeated deletions have the same effect)

5. **Added Required Imports**:
   - `io.mockk.every` - for mock setup
   - `io.mockk.justRun` - for mocking void methods
   - `io.mockk.verify` - for verification blocks
   - `java.util.Optional` - for handling optional return values

## Technical Decisions

### Mock Setup Strategy
I followed the provided code blocks from the guide exactly as specified. Each test required different mocking strategies:

- **POST**: Used chained `andThenAnswer` to return different IDs, simulating resource creation
- **GET**: Used simple mocks with `Optional.of()` and `Optional.empty()` to simulate found/not-found scenarios
- **PUT**: Used sequential answers to simulate creation (first call) then update (second call)
- **DELETE**: Used `justRun` for void methods since DELETE doesn't return a value

### Verification Approach
The verification blocks were designed to validate HTTP method semantics:

- **Safe methods** (GET): Verify zero calls to modification methods
- **Idempotent methods** (PUT, DELETE): Verify consistent behavior across repeated calls
- **Non-idempotent methods** (POST): Verify multiple invocations with different outcomes

### Code Quality
- Maintained Kotlin coding conventions
- Used MockK framework idioms consistently
- Preserved existing code structure and formatting
- Ensured all tests validate the correct REST semantics

## Learning Outcomes

Through this assignment, I gained deeper understanding of:

1. **HTTP Method Semantics**:
   - **Safe methods**: Don't modify server state (GET, HEAD, OPTIONS)
   - **Idempotent methods**: Multiple identical requests produce the same result (GET, PUT, DELETE)
   - **Non-idempotent methods**: Each request may produce different results (POST)

2. **REST API Testing**:
   - How to properly mock repository behavior for different scenarios
   - Importance of verifying not just what happens, but what doesn't happen (e.g., no saves on GET)
   - Testing both success and failure paths (404 responses)

3. **MockK Framework**:
   - Using `every { }` blocks for method stubbing
   - Chaining answers with `andThenAnswer` for sequential behavior
   - Using `justRun` for void methods
   - Verification with `exactly = N` parameter
   - Working with `any()` matchers

4. **Spring Boot Testing**:
   - `@MockkBean` annotation for injecting mocks
   - `MockMvc` for simulating HTTP requests
   - Integration between Spring's testing framework and MockK

5. **Test-Driven Design Principles**:
   - How tests document expected API behavior
   - The relationship between HTTP semantics and implementation
   - Importance of testing idempotency and safety properties

## AI Disclosure

### AI Tools Used
- Claude (Anthropic) - AI assistant for code completion and guidance

### AI-Assisted Work
- **Documentation Guidance**: AI helped identify which code blocks from the guide matched each test scenario
- **Percentage**: Approximately 15% AI-assisted, 85% original work

### Original Work
The majority of the work was original and based on:

1. **Reading and Understanding the Guide**: Carefully studied `docs/GUIDE.md` to understand:
   - The purpose of each test
   - The difference between safe/unsafe and idempotent/non-idempotent methods
   - Which code blocks corresponded to each scenario

2. **Analyzing Test Structure**: Examined each test to understand:
   - What the test was checking (status codes, headers, response bodies)
   - How many requests were being made
   - What the expected behavior should be

3. **Mapping Code Blocks to Tests**: Made decisions about which provided code block to use based on:
   - Test hints in comments
   - HTTP method semantics (safe, idempotent)
   - Expected mock behavior from test assertions

4. **Understanding MockK Syntax**: Learned how MockK works by:
   - Reading the provided code blocks
   - Understanding the relationship between mock setup and verification
   - Recognizing patterns like `answers { }` and `andThenAnswer { }`

5. **Verification Logic**: Determined appropriate verification blocks by:
   - Analyzing what repository methods should be called for each HTTP method
   - Counting expected invocations based on test requests
   - Understanding the relationship between HTTP semantics and repository calls

The learning process involved connecting theoretical knowledge about REST semantics with practical implementation through mocking and testing. The assignment reinforced the importance of HTTP method properties and how they should be reflected in both implementation and testing.