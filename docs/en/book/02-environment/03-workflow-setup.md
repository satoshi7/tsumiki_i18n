# 2.3 Development Environment and Workflow Construction

This chapter explains how to build a development environment and workflow for effectively practicing AITDD. By systematizing not only tool preparation but the entire development process, we achieve consistent, high-quality development.

## Actual Introduction History and Methodology Evolution of AITDD

### Introduction Timeline

#### Serious Efforts Starting from Early 2025
**Catalyst:**
- The emergence of **Claude Sonnet 3.5** and **DeepSeek R1's distilled model**
- Gained confidence that substantial implementation was achievable with AI
- Felt the limitations of traditional manual coding

**Accumulation of about 5-6 months of practical experience:**
- Evolution from initial trial and error to systematic methodology
- Discovery of prompt design optimization patterns
- Establishment of best practices through learning from failure cases

### Methodology Evolution Process

#### Phase 1: Live Coding (Initial Approach)
**Characteristics:**
- Creating code through real-time dialogue with AI
- Effective for small-scale feature implementation
- Rapid fixes through immediate feedback

**Effective scenarios:**
- Small-scale modifications within a single file
- Rapid prototype creation
- Sample code creation for technical investigation

**Discovered limitations:**
- Structure tends to break down in large-scale development
- Difficult to manage complex dependencies
- Difficult to maintain quality consistency

#### Phase 2: Combination with TDD (Current Methodology)
**Problem recognition:**
- Large-scale development difficult with live coding
- Lack of quality assurance mechanisms
- Need for mechanisms to maintain design consistency

**Adopted solutions:**
- Combination with **TDD (Test-Driven Development)**
- Establishment of **Red-Green-Refactor-Validation** cycle
- Construction of systematic workflow

**Current methodology characteristics:**
```
Before evolution (Live coding):
Requirements → Direct implementation → Operation check → Fix → Complete

After evolution (AITDD):
Requirements → TODO creation → Red → Green → Refactor → Validation → Complete
         ↑                                          ↓
         ←←←←←←← Feedback loop ←←←←←←←←←←
```

### Practical Development Workflow Construction Experience

#### Project Structure Optimization Process
**Initial challenges:**
- File structure varied inconsistently across projects
- TODO granularity was inappropriate
- Git history was difficult to track

**Improved structure:**
```
project-root/
├── todo.md                    # Central task management
├── docs/                      # Systematic design documents
│   ├── requirements.md        # Clear requirements definition
│   ├── architecture.md        # Architecture design
│   └── api-spec.md           # Detailed API specifications
├── src/                       # Clear separation by functionality
├── tests/                     # Systematic test code
└── scripts/                   # Automation scripts
```

#### TODO Management Evolution Process

**Initial problems:**
- TODO granularity too large ("System-wide implementation", etc.)
- Dependencies unclear
- Progress difficult to track

**Current optimized approach:**

**Discovery of appropriate granularity:**
```markdown
# Optimal granularity (30 minutes to 1 hour)
- [x] User registration API implementation
- [x] Password validation functionality
- [ ] JWT authentication middleware
- [ ] Add login functionality tests

# Granularity to avoid
❌ System-wide implementation (too large)
❌ Variable name changes (too small)
```

**Practical sequential execution strategy:**
1. **Process TODO list items from top to bottom**
2. **Completely finish one item before moving to the next**
3. **Adjust order for items with dependencies**
4. **Split into units completable in 30 minutes to 1 hour**

### Practical Git Workflow Operations

#### AITDD-Specialized Branch Strategy

**Actually adopted strategy:**
```bash
# Branch creation pattern for each TODO item
git checkout -b feature/user-registration    # TODO: User registration API
git checkout -b feature/auth-middleware      # TODO: Authentication middleware
git checkout -b feature/password-validation  # TODO: Password validation
```

**AITDD cycle-corresponding commit strategy:**
```bash
# Red phase (create failing tests)
git add tests/user-registration.test.js
git commit -m "Red: Add failing tests for user registration"

# Green phase (minimal implementation to pass tests)
git add src/controllers/user.js
git commit -m "Green: Implement basic user registration"

# Refactor phase (code improvement)
git add src/controllers/user.js src/models/user.js
git commit -m "Refactor: Extract user validation logic"

# Validation phase (final verification and documentation)
git add docs/api-spec.md
git commit -m "Validation: Complete user registration with docs"
```

#### Practical Failure Recovery Strategies

**Practical operation decision criteria:**
```bash
# Pattern 1: Minor fixes can handle the issue
if [ "deviation from expectation" == "small" ]; then
    # Re-run with prompt adjustment
    echo "Retry with more detailed prompt"
fi

# Pattern 2: Major fixes needed
if [ "deviation from expectation" == "large" ]; then
    git reset --hard HEAD~1  # Return to previous state
    echo "Re-run after prompt review"
fi

# Pattern 3: Multiple failures
if [ "failure count" -gt 3 ]; then
    git reset --hard <last_known_good_commit>
    echo "Fundamentally review approach"
fi
```

**Actual recovery pattern example:**
```
Situation: User authentication API implementation with inappropriate error handling
Decision: No improvement after 3 fix attempts
Action: git reset --hard HEAD~4 to return to Red phase
Re-execution: Restart with more detailed prompt
Result: Expected implementation completed
```

### Important Lessons from Practice

#### Success Factor Analysis

**1. Effectiveness of gradual approach:**
- Steady progress in small steps
- Quality verification at each stage
- Limited impact scope during failures

**2. Importance of documentation:**
- Clear requirements definition directly affects AI output quality
- API specifications serve as guidelines for test design
- Progress visualization contributes to motivation maintenance

**3. Cumulative effect of prompt optimization:**
- Efficiency improvement through reuse of same patterns
- Accuracy improvement through failure case analysis
- Domain-specific knowledge accumulation

#### Common Problems and Solutions

**Problem 1: Unstable AI output quality**
```
Symptom: Different results for same prompt on different days
Cause: Prompt ambiguity, insufficient context
Solution: Add more specific technical constraints and examples

Improvement example:
"Create an API" 
↓
"Create POST /api/users API with Express.js + Mongoose:
- Request: {name: string, email: string}
- Validation: email format check, name required
- Response: 201 with created user info
- Errors: 400(validation), 409(duplicate), 500(server)"
```

**Problem 2: Getting lost in large-scale projects**
```
Symptom: Unable to understand current work position
Cause: Poor TODO management, lack of progress tracking
Solution: Clear progress display and next action specification

Improvement example:
## Current Implementation Status (2025-06-21)
- [x] User management functionality (Complete)
- [ ] **Authentication functionality (In progress: JWT middleware creation stage)**
- [ ] Permission management functionality (Not started)

### Next Actions
1. Implement JWT signature verification
2. Add token refresh functionality
3. Implement logout functionality
```

**Problem 3: Test and code inconsistencies**
```
Symptom: Tests pass but actual behavior differs from expectations
Cause: Poor test design, requirement understanding gaps
Solution: Create more realistic test cases

Improvement example:
# Insufficient test
test('should create user', () => {
  expect(user).toBeDefined();
});

# Improved test
test('should create user with valid email and return 201', async () => {
  const userData = { name: 'John', email: 'john@example.com' };
  const response = await request(app)
    .post('/api/users')
    .send(userData)
    .expect(201);
  
  expect(response.body.user.email).toBe(userData.email);
  expect(response.body.user.password).toBeUndefined(); // Password not included
});
```

## AITDD Development Workflow Overview

### Basic Development Flow
```
TODO list creation → Item selection → AITDD execution → Review → Next item
     ↑                                            ↓
     ←←←←←←←←←← Adjust as needed ←←←←←←←←←←←←←←
```

### Detailed AITDD Execution Cycle
```
Red (Test creation) → Green (Implementation) → Refactor (Improvement) → Validation (Verification)
      ↑                                                    ↓
      ←←←←←←←←←←←←← Feedback loop ←←←←←←←←←←←←←←
```

## Project Structure Design

### Recommended Directory Structure

```
project-root/
├── todo.md                    # Main TODO list
├── docs/                      # Project documents
│   ├── requirements.md        # Requirements definition
│   ├── architecture.md        # Architecture design
│   └── api-spec.md           # API specifications
├── src/                       # Source code
│   ├── models/               # Data models
│   ├── controllers/          # Controllers
│   ├── services/             # Business logic
│   └── utils/                # Utilities
├── tests/                     # Test code
│   ├── unit/                 # Unit tests
│   ├── integration/          # Integration tests
│   └── fixtures/             # Test data
├── scripts/                   # Development scripts
└── README.md                 # Project overview
```

### TODO List Creation and Management

#### Basic TODO List Format

**Example todo.md:**
```markdown
# Project TODO List

## Current Implementation Target
- [ ] User registration functionality implementation

## Completed
- [x] Project initial setup
- [x] Database connection setup

## Not Started (Priority order)
1. [ ] User authentication functionality
   - [ ] Password hashing
   - [ ] JWT token generation
   - [ ] Login API

2. [ ] User management functionality
   - [ ] Profile update API
   - [ ] User deletion API
   - [ ] User list API

3. [ ] Security enhancement
   - [ ] Rate limiting implementation
   - [ ] Input validation enhancement
   - [ ] CORS configuration

## Future consideration
- [ ] Performance optimization
- [ ] Deployment automation
```

#### Setting Effective TODO Granularity

**Appropriate granularity examples:**
- ✅ `User registration API implementation` (30 minutes to 1 hour)
- ✅ `Password validation functionality` (30 minutes to 1 hour)
- ✅ `JWT authentication middleware` (30 minutes to 1 hour)

**Granularity to avoid:**
- ❌ `System-wide implementation` (too large)
- ❌ `Variable name changes` (too small)

### TODO Execution Strategy

#### Sequential Execution Approach
```markdown
Execution policy:
1. Process TODO list items from top to bottom
2. Completely finish one item before moving to next
3. Adjust order for items with dependencies
4. Split into units completable in 30 minutes to 1 hour
```

#### Dependency Management
```markdown
Dependency examples:
- User model → User registration API → User authentication
- Database design → Migration → API implementation
- Basic functionality → Error handling → Security enhancement
```

## Git Workflow Setup

### AITDD-oriented Branch Strategy

#### Basic Branch Model
```bash
main                    # Production environment
├── develop            # Development integration
└── feature/todo-item  # For each TODO item
```

#### Branch Creation Examples
```bash
# Create branch for each TODO item
git checkout -b feature/user-registration
git checkout -b feature/user-authentication
git checkout -b feature/password-validation

# When grouping by functionality
git checkout -b feature/user-management
git checkout -b feature/security-enhancement
```

### Commit Strategy

#### Commits Corresponding to AITDD Cycle
```bash
# Red phase (test creation)
git add tests/
git commit -m "Red: Add tests for user registration"

# Green phase (implementation)
git add src/
git commit -m "Green: Implement user registration functionality"

# Refactor phase (improvement)
git add src/
git commit -m "Refactor: Improve user registration code structure"

# Validation phase (verification)
git add .
git commit -m "Validation: Complete user registration with documentation"
```

#### Failure Recovery Strategy
```bash
# When AI doesn't produce expected results
git reset --hard HEAD~1  # Cancel last commit
# or
git reset --hard <commit-hash>  # Return to specific commit

# Adjust prompt and re-execute
# New commit after success
```

## Next Steps for Practice

### Action Guidelines After Environment Setup Completion

1. **Preparation for Chapter 3 transition**
   - Detailed understanding of AITDD process
   - Mastery of Red-Green-Refactor-Validation cycle
   - Experience with actual development flow

2. **First project planning**
   - Design small-scale sample project
   - Define clear functional requirements
   - Create TODO list within implementable scope

3. **Preparation for continuous improvement**
   - Improve prompt design skills
   - Master effective dialogue patterns with AI
   - Practice review and quality management

### Important Points for Success

#### Adopting Gradual Approach
- **Start small**: Begin with simple functionality
- **Gradually expand**: Build successful experiences
- **Learn from failures**: Don't fear git reset and try iteratively

#### Continuous Attention to Quality
- **Test first**: Always start by writing tests
- **Habitualizing reviews**: Always check AI-generated code
- **Practice documentation**: Properly record implementation content

#### Team Practice Preparation
- **Build common understanding**: Align with team members
- **Unify tools**: Work with same development environment
- **Share knowledge**: Share success and failure cases

## Environment Setup Completion Verification

### Final Checklist

- [ ] **Basic Tools**
  - [ ] Claude Sonnet 4 (Claude Code) available
  - [ ] VS Code properly configured
  - [ ] Git repository initialized

- [ ] **Project Structure**
  - [ ] Recommended directory structure created
  - [ ] todo.md file prepared
  - [ ] Basic configuration files placed

- [ ] **Development Workflow**
  - [ ] Git branch strategy decided
  - [ ] Commit rules defined
  - [ ] Test environment verified

- [ ] **Documentation & Monitoring**
  - [ ] README.md created
  - [ ] Log configuration completed
  - [ ] Debug environment prepared

### Operation Verification Test

```bash
# Basic operation verification
npm test                     # Run tests
npm run test:coverage        # Check coverage
git status                   # Check Git status
git log --oneline -5         # Check recent commits

# Claude Code integration verification
# Open project in VS Code
# Verify Claude Code plugin works normally
# Request AI to create simple test case and verify operation
```

### Troubleshooting

#### Common Problems and Solutions

**Cannot connect to Claude Code**
```bash
# Check authentication
# Check Pro plan validity
# Check network settings
```

**Errors in test environment**
```bash
# Reinstall dependencies
npm install

# Clear package cache
npm cache clean --force
```

**Git operation errors**
```bash
# Reset authentication information
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Summary

In Chapter 2, we learned comprehensive development environment construction methods for practicing AITDD. Key points are as follows:

### Main Achievements
1. **Tool setup**: Development environment construction centered on Claude Sonnet 4
2. **Workflow design**: Systematic process from TODO management to Git flow
3. **Quality management foundation**: Setup of test environment, logs, and debug functionality

### Preparation for Next Chapter
Once environment construction is complete, we will learn the actual AITDD process. In Chapter 3 "AITDD Process Details", we will master specific practical methods of the Red-Green-Refactor-Validation cycle.

**Learning points:**
- Specific work content for each phase
- Effective dialogue methods with AI
- Quality management and review techniques

Now that the environment is ready, we are prepared to actually perform software development in collaboration with AI. Let's experience the true value of AITDD in the next chapter.