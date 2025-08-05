# 4.4 Error Handling and Debugging

## Learning Objectives

In this chapter, you will learn methods for handling various errors that occur during AITDD development and effective debugging techniques:

- Understanding error patterns specific to AI-generated code
- Identifying and fixing prompt-caused errors
- Establishing efficient debugging processes
- Deciding when to switch to manual implementation and practicing it
- Best practices for error prevention

## Error Classification and Response Strategies

In AITDD development, different types of errors occur compared to traditional development. It's important to classify these appropriately and apply suitable countermeasures for each.

### Basic Error Classification

**1. Prompt-Caused Errors**
- Due to ambiguous instructions
- Due to unclear requirements
- Due to insufficient context

**2. AI Implementation-Caused Errors**
- Bugs in AI-generated code
- Compatibility issues with existing code
- Performance issues

**3. Integration-Caused Errors**
- Issues during multi-feature integration
- Interface mismatches
- Dependency issues

**4. Traditional Errors**
- General programming errors
- Environment configuration issues
- External dependency issues

## Practical Debugging Process

### Step 1: Comprehensive Error Information Collection

When an error occurs, first systematically collect information.

**Information to Collect**:
```markdown
## Error Information Collection Checklist

### Basic Information
- [ ] Error message (complete version)
- [ ] Stack trace
- [ ] Occurrence timing
- [ ] Execution environment

### Context Information
- [ ] Executed command
- [ ] Input data
- [ ] Expected behavior
- [ ] Actual behavior

### AI-Related Information
- [ ] Prompt used
- [ ] Relevant portion of AI-generated code
- [ ] Related existing code

### Execution Environment
- [ ] OS/Browser information
- [ ] Node.js/TypeScript version
- [ ] State of dependency packages
```

**Example of Information Collection**:
```
Error occurrence record example:

Error Message:
TypeError: Cannot read property 'map' of undefined
    at getAllTasks (TaskController.ts:15)
    at Router.handle (express/lib/router/layer.js:95)

Occurrence Timing:
During test execution of GET /api/tasks endpoint

Input Data:
GET http://localhost:3000/api/tasks

Expected Behavior:
Response including empty array is returned

Actual Behavior:
Error calling map on undefined

Prompt Used:
"Please implement an API endpoint that calls the getAllTasks method of the TaskManager class and returns a response"
```

### Step 2: Root Cause Analysis with AI

Pass all collected information to AI and request error cause analysis.

**Effective Analysis Prompt**:
```
Please analyze the cause of the following error:

## Error Information
[Paste all collected error information]

## Related Code
[Complete source code where error occurred]

## Analysis Requirements
1. Direct cause of the error
2. Root cause (design/implementation issues)
3. Possibility of prompt-caused issue
4. Suggested fix approach
5. Prevention measures for similar errors

Please provide as specific and practical an analysis as possible.
```

**AI Analysis Example**:
```
## Error Analysis Results

### Direct Cause
taskManager.getAllTasks() is returning undefined in TaskController.getAllTasks method

### Root Cause
1. Issue with TaskManager class instantiation
2. Task array not initialized in getAllTasks method implementation

### Prompt-Caused Possibility
The instruction "use existing TaskManager class" was ambiguous,
possibly implemented without confirming actual class definition

### Fix Approach
1. Verify TaskManager class initialization
2. Validate getAllTasks method return value
3. Add null/undefined checks

### Prevention Measures
1. Include existing code details in prompt
2. Enhance return value type safety
3. Add null/undefined cases in unit tests
```

### Step 3: Incremental Investigation and Hypothesis Verification

Based on AI analysis, investigate the problem incrementally.

**Investigation Procedure Example**:
```typescript
// 1. TaskManager class state verification
describe('TaskManager Debugging', () => {
  test('TaskManager instantiation check', () => {
    const manager = new TaskManager();
    console.log('TaskManager instance:', manager);
    expect(manager).toBeDefined();
  });

  test('getAllTasks return value check', () => {
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    console.log('getAllTasks result:', result);
    console.log('result type:', typeof result);
    expect(result).toBeDefined();
  });

  test('Task array initial state check', () => {
    const manager = new TaskManager();
    const tasks = manager.getAllTasks();
    expect(Array.isArray(tasks)).toBe(true);
    console.log('Initial tasks array:', tasks);
  });
});
```

**Debug Execution**:
```bash
npm test -- --verbose TaskManager\ Debugging
```

### Step 4: Fix Implementation with AI Collaboration

Feed back investigation results to AI and request fix implementation.

**Fix Request Prompt**:
```
Debug investigation revealed the following:

## Investigation Results
[Debug test output results]

## Identified Issues
1. Improper array initialization in TaskManager class
2. getAllTasks method return value is undefined

## Fix Requirements
Please implement fixes that satisfy:
1. Array is properly initialized
2. Type safety is ensured
3. null/undefined checks are added
4. Existing tests pass
5. New test cases are also added

Please provide fixed code and explanation of fix reasons.
```

## Identifying and Fixing Prompt-Caused Errors

### Criteria for Determining Prompt Issues

**Determination by Frequency**:
```
Similar error pattern occurrence:
- 1st time: Process as implementation error
- 2nd time: Consider possibility of prompt issue
- 3rd time: Prompt fix mandatory
```

**Signs of Typical Prompt Issues**:
- Different implementations generated for same request
- Output significantly different from expectations
- Unintended modification of existing code
- Implementation beyond instruction scope

### Prompt Issue Fix Process

#### 1. Prompt Diagnosis

**AI Diagnosis Request**:
```
Please analyze the following prompt and point out issues:

## Used Prompt
[Problematic prompt]

## Expected Result
[Expected output]

## Actual Result
[Actual output]

## Diagnosis Requirements
1. Ambiguous parts of prompt
2. Missing information
3. Potentially misleading expressions
4. Improvement suggestions
```

#### 2. Creating Prompt Improvements

**Example of Improved Prompt**:
```
Before improvement (problematic prompt):
"Please create an API endpoint using the TaskManager class"

After improvement:
"Please implement the GET /api/tasks endpoint using the following existing TaskManager class.

Existing code:
[Complete TaskManager class code]

Requirements:
1. Do not modify existing code at all
2. Use Express.js Router
3. Response format: { success: boolean, data: Task[] }
4. Include error handling
5. Ensure TypeScript type safety

Output format:
- routes/tasks.ts file
- controllers/TaskController.ts file
- Corresponding test files"
```

#### 3. Improved Prompt Verification

**Verification Process**:
```typescript
describe('Prompt Improvement Verification', () => {
  test('Problem reproduction with pre-improvement prompt', async () => {
    // Request implementation with problematic prompt
    // Confirm expected problem occurs
  });

  test('Solution confirmation with improved prompt', async () => {
    // Request implementation with improved prompt
    // Confirm problem is resolved
  });

  test('Side effect check of improved prompt', async () => {
    // Confirm no new problems caused by improvement
  });
});
```

## Deciding When to Switch to Manual Implementation

### Timing Decision for Switching

**Pre-Implementation Decision**:
```markdown
## Manual Implementation Consideration Checklist

### Presence of Implementation Image
- [ ] Concrete implementation steps come to mind
- [ ] Technologies/libraries to use are clear
- [ ] Implementation pitfalls can be predicted

### Technical Complexity
- [ ] Performance optimization needed
- [ ] Complex algorithms required
- [ ] Deep domain knowledge required

### Difficulty Explaining to AI
- [ ] Cannot clearly verbalize requirements
- [ ] Prompt becomes abnormally long
- [ ] Difficult to explain prerequisite knowledge
```

**Mid-Implementation Switch Decision**:
```
Switch decision criteria:
1. Same error occurs 3+ times
2. Not resolved even with prompt fixes
3. Debug time exceeds implementation time
4. AI-generated code quality is inconsistent
```

### Effective Manual Implementation Approach

#### Gradual AI Integration Strategy

**Partial AI utilization instead of fully manual**:
```typescript
// 1. Manual implementation for complex logic parts
const complexAlgorithm = (data: any[]) => {
  // Implement manually (parts difficult to explain to AI)
  let result = [];
  for (let i = 0; i < data.length; i++) {
    // Complex calculation logic
  }
  return result;
};

// 2. Request AI for boilerplate code
const generateApiResponse = (data: any) => {
  // This part can be generated by AI
  return {
    success: true,
    data,
    meta: {
      timestamp: new Date().toISOString(),
      count: Array.isArray(data) ? data.length : 1
    }
  };
};

// 3. Combine for final implementation
export const processData = async (inputData: any[]) => {
  try {
    const processedData = complexAlgorithm(inputData); // Manual part
    return generateApiResponse(processedData); // AI-generated part
  } catch (error) {
    // Error handling can also be AI-assisted
  }
};
```

#### AI Support Utilization in Manual Implementation

**1. IDE Autocomplete Utilization**:
```typescript
// Actively use AI autocomplete in VS Code etc.
const taskManager = new TaskManager();
// ↑ Utilize where AI autocomplete is effective
```

**2. Partial Code Generation Requests**:
```
Example AI request for parts with clear implementation image:

"Please create a validation function based on the following type definition:

interface TaskInput {
  title: string;
  description?: string;
}

Requirements:
- title is required, 1-100 characters
- description is optional, 0-500 characters
- Specific error messages for invalid cases
- Functions as TypeScript type guard
```

#### Continuing Quality Process

**Validation step is mandatory even for manual implementation**:
```
Manual implementation Validation checklist:
1. Consistency with specification requirements
2. Type safety assurance
3. Appropriate error handling
4. Test coverage confirmation
5. Performance adequacy
6. Code readability/maintainability
```

## Best Practices for Error Prevention

### Improving Prompt Design

**1. Context Clarification**:
```
Good prompt example:

"Please add new functionality to the following existing system:

Existing code:
[Paste all related code]

New feature requirements:
[Specific and clear requirements]

Constraints:
- Existing code modification prohibited
- Ensure TypeScript type safety
- Error handling mandatory

Expected output:
- Implementation code
- Test code
- Usage examples
- Important notes"
```

**2. Incremental Implementation Instructions**:
```
Incremental implementation of complex features:

"Please implement the following incrementally:

Step 1: Interface design
Step 2: Basic implementation
Step 3: Error handling
Step 4: Test creation

Please proceed while confirming at each step."
```

### Strengthening Test Strategy

**1. Tests Specific to AI-Generated Code**:
```typescript
describe('AI-Generated Code Verification Tests', () => {
  test('Confirm no unintended existing code modifications', () => {
    // Test that important existing functions are not changed
    const originalFunction = require('./legacy/original-module');
    expect(originalFunction.criticalMethod).toBeDefined();
    expect(typeof originalFunction.criticalMethod).toBe('function');
  });

  test('Confirm inferred implementation scope', () => {
    // Confirm AI's inferred implementations are within requirements
    const implementation = new FeatureImplementation();
    expect(implementation.getImplementedFeatures())
      .toEqual(expect.arrayContaining(REQUIRED_FEATURES));
  });

  test('Type safety confirmation', () => {
    // Confirm TypeScript type checking functions correctly
    // Test that no compilation errors occur
  });
});
```

**2. Strengthening Integration Tests**:
```typescript
describe('Feature Integration Tests', () => {
  test('Regression check with 3-feature integration', async () => {
    // Test that 3 features work normally when combined
    const feature1 = await executeFeature1();
    const feature2 = await executeFeature2(feature1.result);
    const feature3 = await executeFeature3(feature2.result);
    
    expect(feature3.result).toMatchExpectedOutput();
  });
});
```

### Continuous Improvement Process

**1. Error Pattern Accumulation**:
```markdown
## Error Pattern Management

### High-Frequency Errors
1. undefined/null access errors
   - Cause: Insufficient initialization in AI-generated code
   - Countermeasure: Explicit initialization instructions

2. Type mismatch errors
   - Cause: Insufficient type information in prompt
   - Countermeasure: Explicitly provide type definitions

3. Existing code modification errors
   - Cause: Insufficient "modification prohibited" instructions
   - Countermeasure: Specific constraint instructions
```

**2. Prompt Template Improvement**:
```
Improved prompt template:

### Basic Template
```
Feature: [Feature name]
Implementation target: [Specific implementation content]

Existing code:
[All related code]

Requirements:
[Clear and specific requirements]

Constraints:
- Existing code modification prohibited
- [Other constraints]

Output requirements:
- [Expected output format]

Quality requirements:
- TypeScript type safety
- Error handling
- Including test code
```

## Practical Debugging Techniques

### Log-Based Debugging

**Effective Log Output**:
```typescript
// Debug log helper
class DebugLogger {
  static log(context: string, data: any) {
    if (process.env.NODE_ENV === 'development') {
      console.log(`[${context}]`, {
        timestamp: new Date().toISOString(),
        data: JSON.stringify(data, null, 2)
      });
    }
  }

  static error(context: string, error: any) {
    console.error(`[ERROR:${context}]`, {
      message: error.message,
      stack: error.stack,
      timestamp: new Date().toISOString()
    });
  }
}

// Usage example
export const taskController = {
  getAllTasks: async (req, res, next) => {
    try {
      DebugLogger.log('TaskController.getAllTasks', 'Starting execution');
      
      const tasks = taskManager.getAllTasks();
      DebugLogger.log('TaskController.getAllTasks', { tasksCount: tasks?.length });
      
      const response = generateResponse(tasks);
      DebugLogger.log('TaskController.getAllTasks', { response });
      
      res.json(response);
    } catch (error) {
      DebugLogger.error('TaskController.getAllTasks', error);
      next(error);
    }
  }
};
```

### Test-Driven Debugging

**Working Backwards from Failed Tests**:
```typescript
describe('Bug Reproduction Tests', () => {
  test('Null error reproduction under specific conditions', () => {
    // Identify minimum conditions for bug occurrence
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    
    // Error should occur at this point
    expect(() => result.map(x => x.id)).toThrow();
  });

  test('Post-fix behavior confirmation', () => {
    // Test expected behavior after fix
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    
    expect(Array.isArray(result)).toBe(true);
    expect(() => result.map(x => x.id)).not.toThrow();
  });
});
```

## Summary

In this chapter, we learned comprehensive error handling and debugging techniques in AITDD development:

**Key Learning Outcomes**:
- Appropriate error classification and countermeasures
- Efficient debugging process using AI
- Identifying and fixing prompt-caused errors
- Decision-making for switching to manual implementation and AI integration strategy
- Best practices for error prevention

**Practical Skills**:
- Comprehensive information gathering techniques
- Error analysis in collaboration with AI
- Incremental problem-solving approach
- Continuous improvement of quality management

**Preparation for Next Chapter**:
With these skills, you are prepared to learn more advanced AITDD techniques and optimization technologies. We will proceed to prompt design and AI utilization optimization.

Through the AITDD practical hands-on series, you have acquired a full range of techniques from basics to applications. In the next section, we will further refine these techniques and learn advanced methods for utilizing them in actual production environments.