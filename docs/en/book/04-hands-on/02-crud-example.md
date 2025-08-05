# 4.2 CRUD Operations Implementation Example

## Learning Objectives

In this chapter, you will learn the following through implementing more practical CRUD (Create, Read, Update, Delete) operations:

- Utilizing AITDD for integrating multiple features
- Designing and implementing data management logic
- Experiencing near-real application development
- Feeling the difference from vibe coding

## Project Overview: Simple Task Management System

### Features to Implement

Create a simple memory-based task management system:

```typescript
interface Task {
  id: string;
  title: string;
  description: string;
  completed: boolean;
  createdAt: Date;
  updatedAt: Date;
}
```

**CRUD Operations**:
- **Create**: Create new tasks
- **Read**: Retrieve tasks (all, single, conditional search)
- **Update**: Update tasks
- **Delete**: Delete tasks

### Technical Complexity Level

Compared to the calculator function from the previous chapter, the following complexities are added:
- Data persistence (memory-based)
- Multiple entity operations
- Combination of validations
- Diversified error handling

## Hands-on Practice

### Step 1: TODO Creation and Feature Decomposition

An important lesson from AITDD is that **integration of about 3 features is the practical limit**. Therefore, we decompose features into appropriate granularities.

```markdown
# TODO: Task Management System CRUD Implementation

## Phase 1: Foundation Implementation
- [ ] Task interface definition
- [ ] Basic structure of TaskManager class
- [ ] Memory storage implementation

## Phase 2: Basic CRUD (up to 3 features)
- [ ] Create: Create new task
- [ ] Read: Get all tasks
- [ ] Read: Get single by ID

## Phase 3: Extended CRUD (remaining features)
- [ ] Update: Update task
- [ ] Delete: Delete task
- [ ] Read: Conditional search (by completion status, etc.)

## Technical Requirements
- Use TypeScript
- Test with Jest
- Memory-based data management
- Appropriate error handling
```

**Key Points**:
- Control integration complexity through phasing
- Each phase can be tested independently
- Gradual feature addition is possible

### Step 2: Phase 1 Implementation

#### Specification Creation

**Prompt to AI**:
```
Please create specifications for the foundation part of the task management system.

Requirements:
- Task interface definition
- TaskManager class design
- Memory-based storage functionality
- Type safety with TypeScript

Please refer to the following TODO:
[Paste Phase 1 TODO]
```

**Generated Specification Example**:
```markdown
# Task Management System Foundation Specification

## Task Interface
```typescript
interface Task {
  id: string;          // Unique ID in UUID format
  title: string;       // Task title (1-100 characters)
  description: string; // Task description (0-500 characters)
  completed: boolean;  // Completion status
  createdAt: Date;     // Creation date
  updatedAt: Date;     // Update date
}
```

## TaskManager Class
- Memory-based task management
- Task persistence (array)
- Automatic ID generation
- Basic validation

## Error Handling
- Task not found: TaskNotFoundError
- Validation failure: ValidationError
- Invalid ID format: InvalidIdError
```

#### Test Case Creation

**AI Prompt**:
```
Please create test cases for the foundation part based on the following specification:

[Paste foundation specification]

Test perspectives:
- Interface type checking
- TaskManager initialization
- Memory storage operation verification
- Error class definition verification
```

#### Red-Green-Refactor-Validation Execution

Request step-by-step implementation from AI:

1. **Red**: Confirm test failure
2. **Green**: Minimal implementation of foundation classes
3. **Refactor**: Design improvement
4. **Validation**: Foundation validity confirmation

### Step 3: Phase 2 Implementation (Basic CRUD)

#### Important Notes for Integration

An important difference from vibe coding is the need for a **structured approach**:

**AITDD Method**:
```
1. Clear specification definition
2. Comprehensive test design
3. Gradual implementation
4. Quality check
→ Easy integration, stable quality
```

**Vibe Coding Method (to avoid)**:
```
1. Impulsive implementation request to AI
2. Tests added later
3. Problems found during integration
4. Manual fixes
→ Breaks down with 3-feature integration
```

#### Create Feature Implementation

**Specification**:
```markdown
## Task Creation Feature

### Method: createTask(taskData)
- Parameter: { title: string, description: string }
- Return value: Created Task
- Validation:
  - title is required, 1-100 characters
  - description is 0-500 characters
- Auto-set: id, createdAt, updatedAt, completed=false
```

**Example AI Prompt**:
```
Please create test cases and implementation for the createTask method with the following specification:

[Paste specification]

Requirements:
1. Comprehensive test cases (normal, abnormal, boundary values)
2. Implement with Red-Green-Refactor-Validation cycle
3. Ensure consistency with existing foundation code
4. Utilize TypeScript type safety
```

#### Read Feature Implementation

Implement **get all and get single** simultaneously:

**Specification**:
```markdown
## Task Retrieval Feature

### getAllTasks(): Task[]
- Returns all tasks in an array
- Empty array if empty
- Sort by creation date descending

### getTaskById(id: string): Task
- Search task by ID
- If not found: TaskNotFoundError
- Invalid ID format: InvalidIdError
```

### Step 4: Phase 3 Implementation (Extended CRUD)

#### Update Feature

**Support partial updates**:

```typescript
interface TaskUpdateData {
  title?: string;
  description?: string;
  completed?: boolean;
}

updateTask(id: string, updateData: TaskUpdateData): Task
```

#### Delete Feature

Implementation including **logical vs physical deletion** consideration:

```typescript
deleteTask(id: string): boolean  // Physical deletion
// or
softDeleteTask(id: string): Task  // Logical deletion
```

#### Conditional Search Feature

**Filtering functionality**:

```typescript
interface TaskFilter {
  completed?: boolean;
  titleContains?: string;
  createdAfter?: Date;
}

searchTasks(filter: TaskFilter): Task[]
```

### Step 5: Integration Testing and Final Review

#### Integration Scenario Testing

Tests assuming actual use cases:

```typescript
describe('Task Management Integration Scenario', () => {
  test('Common task management flow', async () => {
    const manager = new TaskManager();
    
    // 1. Create task
    const task1 = manager.createTask({
      title: 'Project Planning',
      description: 'Requirements definition and design'
    });
    
    // 2. Get and verify task
    const allTasks = manager.getAllTasks();
    expect(allTasks).toHaveLength(1);
    
    // 3. Update task
    const updatedTask = manager.updateTask(task1.id, {
      completed: true
    });
    expect(updatedTask.completed).toBe(true);
    
    // 4. Search feature
    const completedTasks = manager.searchTasks({
      completed: true
    });
    expect(completedTasks).toHaveLength(1);
    
    // 5. Delete task
    const deleted = manager.deleteTask(task1.id);
    expect(deleted).toBe(true);
    expect(manager.getAllTasks()).toHaveLength(0);
  });
});
```

#### Performance Testing

**Operation verification with large data**:

```typescript
describe('Performance Test', () => {
  test('1000 task operations', () => {
    const manager = new TaskManager();
    
    // Create 1000 tasks
    const startTime = Date.now();
    for (let i = 0; i < 1000; i++) {
      manager.createTask({
        title: `Task ${i}`,
        description: `Description ${i}`
      });
    }
    const createTime = Date.now() - startTime;
    
    // Search performance
    const searchStart = Date.now();
    const results = manager.searchTasks({
      titleContains: 'Task 1'
    });
    const searchTime = Date.now() - searchStart;
    
    expect(createTime).toBeLessThan(1000); // Within 1 second
    expect(searchTime).toBeLessThan(100);  // Within 100ms
  });
});
```

## Troubleshooting

### Common Problems and Solutions

#### Problem 1: AI-Generated Code Consistency

**Symptoms**:
- Unintended modifications to existing code
- Interface inconsistencies
- Different naming conventions

**Solution**:
```
Improved prompt example:
"Without changing any existing code, please add only new methods according to the following interfaces:
[Clearly state existing interfaces]"
```

#### Problem 2: Insufficient Test Quality

**Symptoms**:
- Lack of abnormal case tests
- Missing boundary value tests
- No integration tests

**Solution**:
```
Review checklist:
- [ ] Covers all normal, abnormal, and boundary cases
- [ ] Verifies error messages
- [ ] Tests with actual use cases
```

#### Problem 3: Integration Complexity

**Symptoms**:
- Breaks down with 3+ feature integration
- Difficult debugging
- Tests don't pass

**Solution**:
```
Gradual integration approach:
1. Completely implement one feature at a time
2. Test combinations of 2 features
3. Carefully proceed when adding 3rd feature
4. Split features if problems occur
```

## AI Prompt Best Practices

### Effective Prompt Design

**1. Clear Context**:
```
Good example:
"In the CRUD operations of the task management system, please implement the createTask method according to the following specification.
Implement by adding to the existing Task interface and TaskManager class,
using memory-based storage.

[Existing code]
[Detailed specification]
[Expected output format]"
```

**2. Explicit Constraints**:
```
Constraint examples:
- "Do not modify existing code"
- "Maximize TypeScript type safety"
- "Implement appropriate error handling"
- "Implement test-first"
```

**3. Specify Expected Quality**:
```
Quality requirement examples:
- "Implement with production-level quality"
- "Prioritize readability and maintainability"
- "Implement with performance considerations"
- "Include appropriate comments"
```

### Prompt Template

CRUD operation-specific prompt template:

```
### CRUD Implementation Prompt Template

**Basic Information**:
- Feature: [Create/Read/Update/Delete] operation
- Target entity: [Entity name]
- Implementation language: TypeScript
- Test framework: Jest

**Existing Context**:
[Existing interface/class definitions]

**Implementation Requirements**:
[Specific specifications]

**Constraints**:
- No modification to existing code
- Maximize type safety
- Error handling required

**Output Requirements**:
1. Test cases (normal/abnormal/boundary)
2. Implementation code
3. Usage examples
4. Notes and limitations
```

## Quality Control Points

### Code Review Checklist

**Functional Quality**:
- [ ] Meets all specification requirements
- [ ] Appropriate error handling
- [ ] Correct behavior at boundaries
- [ ] Performance within acceptable range

**Technical Quality**:
- [ ] TypeScript type safety ensured
- [ ] Follows naming conventions
- [ ] Appropriate abstraction level
- [ ] DRY principle observed

**Test Quality**:
- [ ] Sufficient test coverage
- [ ] Tests are understandable
- [ ] Appropriate use of mocks
- [ ] Comprehensive integration tests

**Maintainability**:
- [ ] Code is readable
- [ ] Structure allows easy changes
- [ ] Appropriate documentation
- [ ] Design considers extensibility

## Learning Effects in Practice

### AITDD Strengths (What You'll Experience)

**Development Speed**:
- Traditional CRUD implementation: 1-2 days
- With AITDD: Under 1 hour
- Experience **20-48x efficiency improvement**

**Quality Stability**:
- Quality assurance through test-first
- Optimization during refactoring phase
- Comprehensive quality check in Validation step

**Learning Effects**:
- AI collaborative development skills
- Effective prompt design ability
- Improved sensitivity to quality control

### Clear Difference from Vibe Coding

**Ease of Integration**:
- Vibe coding: Breaks down with 3-feature integration
- AITDD: Stable with gradual integration

**Debugging Efficiency**:
- Vibe coding: Repetition of same problems
- AITDD: Systematic problem solving

**Long-term Maintainability**:
- Vibe coding: Requires later fixes
- AITDD: Enables continuous development

## Preparation for Next Chapter

Through this CRUD implementation experience, you'll acquire the following skills:

1. **Multi-feature integration management**
2. **Gradual feature development**
3. **Understanding importance of quality control**
4. **Practical AI prompt design skills**

In the next chapter, we'll use these skills to tackle a more practical development scenario: API development. You'll learn how AITDD can handle new complexities such as external dependencies and asynchronous processing.

## Summary

Through implementing CRUD operations, we learned:

**Process Importance**:
- Value of structured approach
- Effect of gradual feature addition
- Automation of quality control

**AI Utilization Tips**:
- Providing clear context
- Specifying appropriate constraints
- Continuous prompt improvement

**Practical Skills**:
- Multi-feature integration techniques
- Test-driven design thinking
- Review and quality control

With these foundations, you'll be able to effectively utilize AITDD even in more complex real development projects.