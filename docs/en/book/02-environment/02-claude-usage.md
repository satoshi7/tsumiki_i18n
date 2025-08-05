# 2.2 How to Utilize Claude Sonnet 4

This section explains effective methods for utilizing Claude Sonnet 4, which is at the core of AITDD. Rather than simply having AI write code, we'll learn how humans and AI can collaborate to develop high-quality software.

## Features and Strengths of Claude Sonnet 4

### Position in AITDD
- **Executor of Red-Green-Refactor-Validation cycle**
- **Consistently handles from design to testing and implementation**
- **Balances high-quality code generation with quality checking**

### Reasons for Selection
- **Accessibility**: Freely usable through Claude Code
- **Coding Performance**: Stable performance at a sufficient level
- **Cost Efficiency**: Reasonable cost level ($20/month)
- **AITDD Suitability**: Optimal for trial-focused development style
- **Integration**: Excellent collaboration with VS Code environment

## Basic Usage of Claude Code

### Startup and Basic Operations

1. **Starting Claude Code**
   ```bash
   # Start Claude Code within VS Code
   # Or access via browser version of Claude
   ```

2. **Project Integration**
   - Specify project directory
   - Recognize file structure
   - Understand existing code

### Basic Dialogue Patterns in AITDD

#### 1. Goal Setting Phase
```
You: "I want to implement CRUD operations for user management functionality. Please create a TODO list first."

Claude: "I'll create a TODO list for user management functionality:
1. Define user model
2. Create test cases for user creation
3. Implement user creation functionality
..."
```

#### 2. Test Creation Phase
```
You: "Please create test cases for the first item in the TODO."

Claude: "I'll create test cases for the User Model:
```javascript
describe('User Model', () => {
  test('should create user with valid data', () => {
    // Test code
  });
});
```"
```

#### 3. Implementation Phase
```
You: "Please implement code to make this test pass."

Claude: "I'll implement a User Model to make the test pass:
```javascript
class User {
  constructor(name, email) {
    // Implementation code
  }
}
```"
```

## Effective Prompt Design

### Basic Principles of Prompt Design

#### 1. Clear Goal Setting
**Good Example:**
```
"I want to implement a user registration API (POST /users).
- With validation functionality
- Including error handling
- Want to proceed test-first"
```

**Bad Example:**
```
"Create user functionality"
```

#### 2. Providing Context
```
"Current project configuration:
- Express.js + MongoDB
- Jest for testing
- Existing User model available

New functionality to add:
- User profile update API"
```

#### 3. Explicit Constraints
```
"Constraints:
- Maintain compatibility with existing APIs
- Security-conscious implementation
- Performance requirement: Response within 1 second"
```

### Iterative Process of Prompt Optimization

#### Step 1: Initial Execution
1. **Create prompt**
2. **Request AI execution**
3. **Evaluate results**

#### Step 2: Evaluation and Improvement
1. **Identify gaps from expectations**
2. **Analyze prompt issues**
3. **Design improved prompt**

#### Step 3: Re-execution
1. **Execute with improved prompt**
2. **Check degree of improvement**
3. **Further adjust if necessary**

### Practical Prompt Templates

#### Template for Feature Implementation
```
【Implementation Request】
Feature: [Specific feature name]
Tech Stack: [List of used technologies]
Requirements:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

Constraints:
- [Constraint 1]
- [Constraint 2]

Expected Deliverables:
- Test cases
- Implementation code
- Documentation (if needed)
```

#### Template for Debugging
```
【Debug Request】
Problem: [Specific problem description]
Error Message: [Actual error]
Reproduction Steps:
1. [Step 1]
2. [Step 2]
3. [Step 3]

Related Code: [Code with issues]
Expected Behavior: [Intended behavior]
```

## Review and Quality Management

### Human Review Points

#### 1. Specification Compliance Check
- **Design Intent Reflection**: Is the planned functionality correctly implemented?
- **Requirements Coverage**: Are all requirements satisfied?
- **Constraint Adherence**: Are set constraints followed?

#### 2. Review Priority Order
1. **Specifications**: Compliance with requirements is most important
2. **Test Cases**: Appropriate coverage of specifications
3. **Implementation Code**: Code quality and specification compliance

#### 3. Review Checklist
- [ ] Are functional requirements satisfied?
- [ ] Is error handling appropriate?
- [ ] Are security requirements considered?
- [ ] Are performance requirements met?
- [ ] Is test coverage sufficient?
- [ ] Is code readability and maintainability good?

### Handling Cases When AI Doesn't Produce Expected Results

#### Fallback Strategy

**Basic Response Flow:**
1. **git reset**: Return to previous state
2. **Prompt adjustment**: Clarify and detail instructions
3. **Re-execution**: Retry with same tool (Claude Sonnet 4)
4. **Evaluation**: Check degree of improvement

**Timing for git reset:**
- When final code significantly deviates from expectations
- When judging rewriting is faster than correction requests
- When multiple correction attempts show no improvement

#### Prompt Adjustment Techniques

**Improving Specificity:**
```
# Before improvement
"Fix this code"

# After improvement
"Fix the following issues in this code:
1. Validation errors are not handled properly
2. Return value type differs from specification
3. Edge case tests are insufficient"
```

**Adding Context:**
```
# Before improvement
"Create an API"

# After improvement
"Create RESTful API using Express.js:
- Endpoint: POST /api/users
- Request format: JSON
- Response format: JSON
- Use existing User model
- MongoDB Atlas already connected"
```

## Recording for Continuous Improvement

### Recording Success Patterns
```markdown
## Success Case Record

### Date: 2025-06-21
### Task: User authentication API implementation
### Prompt Used:
[Specific prompt content]

### Result:
- Implementation completed as expected in one try
- Tests 100% passed

### Learning:
- Specific library specification is effective for authentication systems
- Important to specify security requirements in advance
```

### Analyzing Failure Patterns
```markdown
## Improvement Case Record

### Date: 2025-06-21
### Task: Complex query optimization
### Problem:
- Initial implementation didn't meet performance requirements
- No improvement after 3 correction attempts

### Solution:
- Return to initial state with git reset
- Specify performance requirements numerically in prompt
- Provide reference implementation examples

### Learning:
- Specify performance requirements quantitatively
- Break complex tasks into smaller parts
```

## Using Tools Other Than Claude Sonnet 4

### Detailed Collaboration with Gemini (for Research)

#### Use Cases and Strengths of Gemini
**Use Cases:**
- Research on new libraries
- Reading large amounts of technical documentation
- Research tasks requiring long context
- Information integration from multiple sources

**Gemini's Unique Strengths:**
- **Long Context**: Can process large amounts of information at once
- **Information Gathering Ability**: Effectively integrates information from multiple sources
- **Research Specialization**: Excellent performance in deep-diving technical information

#### Practical Collaboration Workflow

**Basic Collaboration Pattern:**
```
1. Identify research topic → Information gathering by Gemini
2. Organize and summarize information → Analysis by Gemini
3. Create implementation plan → Provide information to Claude Sonnet 4
4. Execute AITDD → Consistent implementation by Claude Sonnet 4
```

**Specific Collaboration Examples:**

**Example 1: Introducing New Framework**
```
Gemini:
"Research new features of Next.js 14 and organize migration 
methods from existing Express.js applications"

↓ Provide research results to Claude Sonnet 4

Claude Sonnet 4:
"Based on Gemini's research results, create a TODO list for 
gradual migration plan and implement the first feature with AITDD"
```

**Example 2: Deep-dive Technical Specification Research**
```
Gemini:
"Research security best practices and implementation patterns 
for combining OAuth 2.0 and JWT authentication"

↓ Organize security requirements and provide to Claude Sonnet 4

Claude Sonnet 4:
"Based on research results, create test cases for secure 
authentication system first, then implement using AITDD methodology"
```

#### Decision Criteria for Tool Selection

**When to use Gemini:**
- [ ] Initial research on new technologies/libraries
- [ ] Need to compare multiple options
- [ ] Need to read long technical documents
- [ ] Need to organize complex requirements
- [ ] Need to research precedent cases

**When to use Claude Sonnet 4:**
- [ ] Specific implementation work
- [ ] Test case creation
- [ ] Code review and quality checking
- [ ] Debugging and troubleshooting
- [ ] Refactoring work

### Practical Operational Know-how

#### Advanced Prompt Design Techniques

**Context Continuation Technique:**
```
# Session start
"Please remember the following project configuration:
- Express.js + MongoDB + Jest
- User authentication functionality implemented
- Current goal: Add user profile management functionality"

# Reference in continuing session
"Based on the project configuration I mentioned earlier, 
please create test cases for profile update API"
```

**Gradual Refinement Technique:**
```
# Phase 1: Overview level
"Please consider overall design of user management system"

# Phase 2: Feature level
"From the previous design, create detailed specifications for profile update functionality"

# Phase 3: Implementation level
"Based on specifications, implement test cases and API endpoints"
```

#### Advanced Error Response Strategies

**Pattern Analysis for Prompt Adjustment:**

**Pattern 1: Failure Due to Lack of Specificity**
```
# Failure example
"Create an API"
→ Implementation very different from expectations

# Success example
"Create POST /api/users/profile API with Express.js:
- Request: {name, email, bio}
- Validation: email format, name required
- Response: updated user information
- Error handling: 400, 401, 500 support"
```

**Pattern 2: Failure Due to Unspecified Technical Constraints**
```
# Failure example
"Write database operation code"
→ Implementation with unused ORM

# Success example
"Implement User schema update operation using Mongoose 7.x:
- Use existing User model
- Use findByIdAndUpdate method
- Proper handling of validation errors"
```

**Practical Checklist for Prompt Adjustment:**
- [ ] Specify technology stack used
- [ ] Concrete specification of input/output formats
- [ ] Instructions to consider error cases
- [ ] Ensure consistency with existing code
- [ ] Specify performance requirements
- [ ] Instructions for security considerations

#### Recording Methods for Continuous Improvement

**Templating Success Patterns:**
```markdown
## Prompt Template: API Implementation

### Basic Format
"Implement [HTTP method] [endpoint] API with [framework name]:
- Request format: [details]
- Response format: [details]
- Validation: [requirements]
- Error handling: [corresponding status codes]
- Use existing [model name] model"

### Application Example
[Specific usage example]

### Expected Results
[Success output pattern]
```

**Recording Analysis of Failure Patterns:**
```markdown
## Improvement Record: [Date]

### Problematic Prompt
[Original prompt]

### Problems Occurred
- [Specific problem 1]
- [Specific problem 2]

### Improved Prompt
[Modified prompt]

### Improvement Points
- [Improvement point 1]
- [Improvement point 2]

### Future Application Guidelines
[How to apply to other cases]
```

### Detailed Comparison with Other AI Tools

**Why Consolidate to Claude Sonnet 4:**

**1. Importance of Consistency**
- Unified approach by same tool
- Learned optimizations have cumulative effect
- Accumulation of responses to tool-specific quirks and limitations

**2. Maximizing Learning Efficiency**
- Efficiency improvement by mastering one tool
- Deepening know-how in prompt design
- Accumulation of error patterns and solutions

**3. Simplifying Cost Management**
- Single tool easier to manage than multiple tools
- Simplified budget planning
- Centralized usage monitoring

**4. Simplicity of Fallback Strategy**
- Can avoid complex decision logic
- No need to decide "which tool to retry with"
- Enables quick problem resolution

**Benefits of Tool Integration:**
```
Item                        Integrated Approach    Multiple Tools Approach
─────────────────────────────────────────────────────────────────────────────
Learning Cost               Low                    High
Prompt Optimization Efficiency High                Low
Cost Management Complexity  Low                    High
Fallback Decision          Simple                 Complex
Knowledge Accumulation Efficiency High            Distributed
─────────────────────────────────────────────────────────────────────────────
Overall Development Efficiency Optimized          Inefficient
```

### Future Response to AI Tool Environment

#### Response Policy to New Technologies
**Systematizing Evaluation Criteria:**
- **Performance Evaluation**: Performance comparison in existing workflows
- **Cost Analysis**: Total cost of ownership evaluation (including learning costs)
- **Integration Evaluation**: Compatibility with current development environment
- **Migration Cost**: Cost estimation for tool changes

**Gradual Introduction Approach:**
1. **Information Gathering Period**: 3-6 months observation period
2. **Small-scale Testing**: Trial on non-critical projects
3. **Comparative Evaluation**: Quantitative performance and efficiency comparison
4. **Gradual Migration**: Cautious migration after confirming clear advantages

**Quantifying Decisions:**
```
Threshold for new tool adoption:
- Performance improvement: 20% or more
- Cost reduction: 15% or more
- Learning cost: Within 2 weeks
- Integration cost: 50% or less of current tools
```

## Next Steps

Once you understand how to utilize Claude Sonnet 4, let's build a comprehensive development environment for practicing AITDD in the next chapter "2.3 Development Environment and Workflow Construction". We'll establish systematic development processes from TODO management to Git workflows.