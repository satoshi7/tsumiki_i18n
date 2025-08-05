# 3.3 Test Case Creation

## Importance of Test Case Creation

In AITDD, test cases are crucial elements that determine the quality of implementation. Since the quality of AI-generated code heavily depends on the comprehensiveness and accuracy of test cases, it's essential to design comprehensive test cases at this stage.

## Test Case Design Principles

### 1. Ensuring Comprehensiveness

#### Functional Coverage
- **Normal cases**: All expected behaviors
- **Error cases**: Error handling and validation
- **Boundary values**: Input boundary conditions
- **Edge cases**: Special conditions or exceptional situations

#### Test Level Coverage
- **Unit tests**: Testing individual functions and methods
- **Integration tests**: Testing component interactions
- **End-to-end tests**: Complete execution of user scenarios

### 2. Clear and Specific Expected Values

```markdown
❌ Bad example: "An error should occur"
✅ Good example: "Status code 400 and error message 'Email already exists' should be returned"
```

### 3. Independence and Reproducibility
- Each test case can be executed independently
- No dependency on test execution order
- No dependency on external environment

## Standard Format for Test Case Documentation

### Basic Template

```markdown
# [Feature Name] Test Case Specification

## Test Overview
- **Target Feature**: Name of the feature to test
- **Test Purpose**: What to verify
- **Prerequisites**: Prerequisites for test execution

## Test Case List

### TC001: [Test Case Name]
- **Category**: Normal/Error/Boundary
- **Purpose**: Content to verify in this test
- **Prerequisites**: State before test execution
- **Test Data**: Details of input data
- **Execution Steps**: 
  1. Specific step 1
  2. Specific step 2
- **Expected Results**: 
  - Details of expected behavior
  - Expected output values
- **Post-conditions**: Expected state after test execution
```

### Concrete Test Case Examples

#### Example: User Registration Feature Test Cases

```markdown
# User Registration Feature Test Case Specification

## Test Overview
- **Target Feature**: User Registration API (POST /api/users)
- **Test Purpose**: Verify all patterns of new user registration
- **Prerequisites**: Database in initial state, API server running

## Test Case List

### TC001: Normal User Registration
- **Category**: Normal
- **Purpose**: Verify new user registration with valid data
- **Prerequisites**: test@example.com is not registered
- **Test Data**: 
  ```json
  {
    "email": "test@example.com",
    "password": "SecurePass123!",
    "password_confirmation": "SecurePass123!"
  }
  ```
- **Execution Steps**: 
  1. Send test data to POST /api/users
  2. Check response
  3. Check database state
- **Expected Results**: 
  - Status code: 201
  - Response: 
    ```json
    {
      "id": any positive integer,
      "email": "test@example.com",
      "created_at": "datetime (ISO8601 format)"
    }
    ```
  - Database: New record created in users table
  - Password is hashed and stored
- **Post-conditions**: User is successfully registered and can log in

### TC002: Email Address Duplication Error
- **Category**: Error
- **Purpose**: Verify error handling when registering with existing email
- **Prerequisites**: test@example.com is already registered
- **Test Data**: 
  ```json
  {
    "email": "test@example.com",
    "password": "AnotherPass456!",
    "password_confirmation": "AnotherPass456!"
  }
  ```
- **Execution Steps**: 
  1. Send test data to POST /api/users
  2. Check response
  3. Check database state
- **Expected Results**: 
  - Status code: 400
  - Response: 
    ```json
    {
      "error": "validation_failed",
      "details": [
        {
          "field": "email",
          "message": "Email already exists"
        }
      ]
    }
    ```
  - Database: No new record created
- **Post-conditions**: No impact on existing user data

### TC003: Password Mismatch Error
- **Category**: Error
- **Purpose**: Verify error handling when password and confirmation don't match
- **Prerequisites**: Use new email address
- **Test Data**: 
  ```json
  {
    "email": "new@example.com",
    "password": "SecurePass123!",
    "password_confirmation": "DifferentPass456!"
  }
  ```
- **Expected Results**: 
  - Status code: 400
  - Error message: "Password confirmation does not match"

### TC004: Invalid Email Address Format
- **Category**: Error/Boundary
- **Purpose**: Verify email address format validation
- **Test Data Set**:
  - "invalid-email" (no @)
  - "test@" (no domain)
  - "@example.com" (no local part)
  - "test..test@example.com" (consecutive dots)
- **Expected Results**: All should result in 400 error

### TC005: Insufficient Password Strength
- **Category**: Error/Boundary
- **Purpose**: Verify password strength validation
- **Test Data Set**:
  - "short" (less than 8 characters)
  - "onlylowercase" (lowercase only)
  - "ONLYUPPERCASE" (uppercase only)
  - "12345678" (numbers only)
  - "NoSymbol123" (no symbols)
- **Expected Results**: All should result in 400 error

### TC006: Missing Required Fields
- **Category**: Error
- **Purpose**: Verify validation of required fields
- **Test Data Set**:
  - Missing email
  - Missing password
  - Missing password_confirmation
  - Empty string cases
  - null cases
- **Expected Results**: All should result in 400 error

### TC007: Boundary Test - Email Address Length
- **Category**: Boundary
- **Purpose**: Verify email address character limit
- **Test Data**:
  - 254 characters (maximum allowed)
  - 255 characters (exceeds limit)
- **Expected Results**: 
  - 254 characters: Successful registration
  - 255 characters: 400 error

### TC008: Rate Limiting Test
- **Category**: Non-functional
- **Purpose**: Verify rate limiting for concurrent registrations
- **Execution Steps**: Send large number of requests in short time
- **Expected Results**: 429 error when limit exceeded

### TC009: Database Connection Error
- **Category**: Error/Infrastructure
- **Purpose**: Verify behavior during database failure
- **Prerequisites**: Database unavailable
- **Expected Results**: 500 error and error log output

### TC010: CSRF Token Verification
- **Category**: Security
- **Purpose**: Verify CSRF attack prevention
- **Test Data**: No CSRF token or invalid token
- **Expected Results**: 403 error
```

## Test Case Creation Workflow

### 1. Test Case Extraction from Specifications

```markdown
Specification items → Corresponding test cases

■ Functional Requirements
- Basic functions → Normal test cases
- Validation → Error test cases
- Input restrictions → Boundary test cases

■ Non-functional Requirements
- Performance → Load test cases
- Security → Security test cases
- Availability → Failure test cases
```

### 2. Test Case Design Steps

#### Step 1: Organize Test Perspectives
```markdown
## Test Perspective List

### Functional Perspective
- [ ] Normal input behavior
- [ ] Input value validation
- [ ] Error handling
- [ ] Data persistence

### Data Perspective
- [ ] Boundary values (minimum, maximum)
- [ ] Special characters/multilingual
- [ ] NULL/empty strings
- [ ] Invalid formats

### State Perspective
- [ ] Initial state
- [ ] Data existing state
- [ ] Error state
- [ ] Limited state

### Environment Perspective
- [ ] Normal environment
- [ ] High load environment
- [ ] Failure environment
```

#### Step 2: Create Test Case Matrix

| Function | Normal | Error | Boundary | Security | Performance |
|----------|--------|-------|----------|----------|-------------|
| User Registration | TC001 | TC002-006 | TC007 | TC010 | TC008 |
| Validation | - | TC002-006 | TC004,005,007 | - | - |
| Data Storage | TC001 | TC009 | - | - | - |

#### Step 3: Create Detailed Test Cases
- Expand each cell content into detailed test cases
- Break down into executable specific steps
- Clearly define expected results

### 3. AI-Assisted Test Case Support

#### Areas Where AI Can Be Utilized
- **Coverage checking**: Identify missing test cases
- **Test data generation**: Suggest boundary values and error values
- **Expected value calculation**: Calculate complex computation results
- **Test case structuring**: Unify format

#### Areas Requiring Human Judgment
- **Business requirement understanding**: Domain-specific requirements
- **Risk assessment**: Impact and importance evaluation
- **Test prioritization**: Execution order and resource allocation
- **Quality standards**: Acceptance criteria setting

## Test Case Quality Checkpoints

### 1. Completeness Verification

#### Functional Coverage
```markdown
## Coverage Checklist

### API Specification Items
- [ ] Test cases for all endpoints
- [ ] Test cases for all parameters
- [ ] Test cases for all response patterns

### Error Handling
- [ ] Test cases for all error codes
- [ ] Test cases for all validation rules
- [ ] Test cases for all exception patterns
```

#### Business Rule Coverage
```markdown
### Business Rule Verification
- [ ] Test cases for all business flows
- [ ] Test cases for all business exceptions
- [ ] Test cases for all permission patterns
```

### 2. Executability Verification

#### Test Data Preparability
- Can necessary test data be prepared?
- Can external dependent services be mocked?
- Can tests be executed in test environment?

#### Expected Result Verifiability
- Can expected results be objectively judged?
- Are necessary tools and methods for verification available?
- Manual verification methods for parts difficult to automate

### 3. Maintainability Verification

#### Test Case Independence
- Each test case can be executed independently
- No dependency on test execution order
- Parallel execution possible

#### Response to Changes
- Clear impact scope when specifications change
- Easy modification of test cases
- Simple test data management

## Human Review Points

### Review Perspectives

#### 1. Consistency with Business Requirements
- [ ] Are user stories appropriately tested?
- [ ] Are business rules correctly reflected?
- [ ] Are edge cases valid from business perspective?

#### 2. Risk-Based Priority
- [ ] Are there sufficient test cases for high-risk functions?
- [ ] Are important business flows covered?
- [ ] Are security requirements appropriately tested?

#### 3. Test Efficiency
- [ ] Is the number of test cases appropriate (not too many/few)?
- [ ] Are there duplicate test cases?
- [ ] Is separation of automatable parts and manual tests appropriate?

### Review Process

#### Step 1: Initial Review
- Verify consistency with specifications
- Basic coverage check
- Point out obvious gaps or issues

#### Step 2: Detailed Review
- Verify validity of each test case
- Verify accuracy of expected results
- Verify executability

#### Step 3: Final Approval
- Overall quality verification
- Verify test execution plan
- Determine progression to next phase

## Test Case Creation Best Practices

### 1. Gradual Refinement

```markdown
Stage 1: Overview Level
"Test normal and error cases for user registration"

Stage 2: Function Level
"Successful registration with valid data"
"Failed registration with invalid data"

Stage 3: Detailed Level
"TC001: Normal user registration"
"TC002: Email address duplication error"
```

### 2. Strategic Test Data Design

#### Data Pattern Systematization
```markdown
## Basic Data Set
- Normal data: Common valid values
- Boundary data: Limit values (minimum/maximum)
- Error data: Invalid/incorrect values
- Special data: Special characters/multilingual/NULL
```

#### Reusable Test Data
- Define commonly used test data
- Manage test data variations
- Automate data creation

### 3. Precise Definition of Expected Results

#### Specific Expected Values
```markdown
❌ "Should error"
✅ "HTTP 400 + {"error": "validation_failed", "field": "email"}"

❌ "Should register successfully"
✅ "HTTP 201 + User ID returned + DB record created"
```

#### Verifiable Conditions
- Specific values or formats of output
- Database state changes
- Log output content
- Impact on external systems

## Common Problems and Solutions

### Problem 1: Inappropriate Test Case Granularity

**Symptoms**: 
- One test case testing multiple functions
- Conversely, too detailed causing high management cost

**Solutions**: 
- One test case = one verification perspective
- Group by business value units

### Problem 2: Ambiguous Expected Results

**Symptoms**: 
- "Should work normally", "Should error", etc.
- Unclear judgment criteria

**Solutions**: 
- Specify concrete values and states
- Consider automated test judgment conditions

### Problem 3: Missing Test Cases

**Symptoms**: 
- Edge cases not considered
- Insufficient error patterns

**Solutions**: 
- Systematic verification with checklists
- Use equivalence class partitioning and boundary value analysis

### Problem 4: Insufficient Maintainability

**Symptoms**: 
- Difficult to modify test cases when specifications change
- Complex test data management

**Solutions**: 
- Modularized design
- Reusable test data design

## Preparation for Next Steps

Once test case creation is complete, proceed to the [Red-Green-Refactor-Validation cycle](./04-rgr-validation-cycle.md).

### Deliverable Verification
- [ ] testcases.md is created in detail
- [ ] Test cases correspond to all specification items
- [ ] Expected results are specifically defined
- [ ] Human review is completed
- [ ] Test data can be prepared

### Quality Checklist
- [ ] **Comprehensiveness**: Normal, error, and boundary cases are covered
- [ ] **Clarity**: Expected results are specific and verifiable
- [ ] **Independence**: Each test case can be executed independently
- [ ] **Feasibility**: Executable in test environment
- [ ] **Maintainability**: Structure that easily responds to specification changes

Proper test case creation establishes the foundation for AI to generate high-quality code. The next chapter will detail the implementation cycle based on these test cases.