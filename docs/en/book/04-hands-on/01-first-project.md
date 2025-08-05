# 4.1 Your First AITDD Project

## Learning Objectives

Through this chapter's first AITDD project, you will learn:

- Understanding the entire AITDD process flow
- Practicing the complete steps from TODO creation to implementation
- Experiencing the proper balance between AI assistance and human review
- Realizing the effectiveness of AITDD with a small-scale feature

## Project Selection Criteria

### Characteristics Suitable for Your First Project

**Implementation Scope**:
- Small-scale features completable in 30-60 minutes
- Simple processes with single responsibility
- Highly independent features with minimal external dependencies

**Technical Complexity**:
- Using familiar technology stacks
- Content where implementation methods can be visualized
- Processes that are relatively easy to debug

**Learning Effectiveness**:
- Allows experiencing each step of AITDD
- Content that facilitates success experiences
- Features that can serve as expandable foundations

### Recommended Project Examples

**1. Simple Calculation Functions**
```
Example: Tax-inclusive Price Calculator
- Input: Product price, tax rate
- Output: Tax-inclusive price, tax amount
- Process: Basic numerical calculations
```

**2. Data Conversion Functions**
```
Example: CSV Data Format Conversion
- Input: CSV string
- Output: JSON format data
- Process: Parsing and conversion logic
```

**3. Validation Functions**
```
Example: Email Address Validation
- Input: String
- Output: Validation result (boolean)
- Process: Format checking using regular expressions
```

## Hands-on Practice: Tax-inclusive Price Calculator

In this example, we'll learn the AITDD process through implementing a tax-inclusive price calculator.

### Step 1: TODO Creation

Create a todo.md file in your project to clarify the features to implement.

```markdown
# TODO: Implement Tax-inclusive Price Calculator

## Features to Implement
- [ ] Function to calculate tax-inclusive price from product price and tax rate
- [ ] Function to calculate tax amount
- [ ] Input value validation
- [ ] Error handling (negative values, null values, etc.)

## Completion Criteria
- Calculations work correctly with normal values
- Appropriate error handling for abnormal values
- Achieve 100% test coverage
```

**Key Points**:
- Break down features specifically
- Clarify completion criteria
- Adjust granularity to complete in 30-60 minutes

### Step 2: Specification Creation

Create detailed specifications in collaboration with AI based on the TODO.

**Example Prompt to AI**:
```
Please create a detailed specification document from the following TODO:

[Paste todo.md content]

Include the following in the specification:
- Function signatures (parameters, return values)
- Input value constraints
- Error conditions and responses
- Detailed calculation logic
```

**Expected Specification Document (Example)**:
```markdown
# Tax-inclusive Price Calculator Specification

## Function Specifications

### calculateTaxIncludedPrice(price: number, taxRate: number): number
- Calculates tax-inclusive price from product price and tax rate
- Parameters:
  - price: Product price (numeric value ≥ 0)
  - taxRate: Tax rate (decimal between 0 and 1, e.g., 0.1 = 10%)
- Return value: Tax-inclusive price (rounded to first decimal place)

### calculateTax(price: number, taxRate: number): number
- Calculates tax amount
- Parameters and return value same as above
- Return value: Tax amount (rounded to first decimal place)

## Error Handling
- Negative price: Error("Price must be 0 or greater")
- Tax rate out of range: Error("Tax rate must be between 0 and 1")
- null/undefined: Error("Please enter valid numbers")
```

**Human Review Checkpoints**:
- [ ] Is the TODO intent correctly reflected?
- [ ] Is the function's responsibility scope appropriate?
- [ ] Are error conditions comprehensive?
- [ ] Is the content implementable?

### Step 3: Test Case Creation

Have AI create test cases based on the specification.

**Example Prompt to AI**:
```
Please create comprehensive test cases based on the following specification:

[Paste specification content]

Create test cases from the following perspectives:
- Normal cases: Operation verification with typical values
- Boundary values: Operation verification at boundaries like 0, 1
- Abnormal cases: Operation verification for error conditions
- Test framework: Use Jest
```

**Generated Test Case Example**:
```typescript
import { calculateTaxIncludedPrice, calculateTax } from './taxCalculator';

describe('Tax-inclusive Price Calculator', () => {
  describe('calculateTaxIncludedPrice', () => {
    // Normal cases
    test('general tax-inclusive price calculation', () => {
      expect(calculateTaxIncludedPrice(1000, 0.1)).toBe(1100);
      expect(calculateTaxIncludedPrice(150, 0.08)).toBe(162);
    });

    // Boundary values
    test('when price is 0', () => {
      expect(calculateTaxIncludedPrice(0, 0.1)).toBe(0);
    });

    test('when tax rate is 0', () => {
      expect(calculateTaxIncludedPrice(1000, 0)).toBe(1000);
    });

    // Abnormal cases
    test('error with negative price', () => {
      expect(() => calculateTaxIncludedPrice(-100, 0.1))
        .toThrow('Price must be 0 or greater');
    });

    test('error when tax rate out of range', () => {
      expect(() => calculateTaxIncludedPrice(1000, -0.1))
        .toThrow('Tax rate must be between 0 and 1');
      expect(() => calculateTaxIncludedPrice(1000, 1.1))
        .toThrow('Tax rate must be between 0 and 1');
    });
  });

  describe('calculateTax', () => {
    test('tax amount calculation', () => {
      expect(calculateTax(1000, 0.1)).toBe(100);
      expect(calculateTax(150, 0.08)).toBe(12);
    });
  });
});
```

**Human Review Checkpoints**:
- [ ] Does it cover all specification requirements?
- [ ] Are abnormal case tests appropriate?
- [ ] Are test expected values correct?
- [ ] Are there additional test cases needed?

### Step 4: Red-Green-Refactor-Validation Cycle

#### 4.1 Red (Test Failure)

Run tests to confirm expected failures.

```bash
npm test
```

Expected output:
```
FAIL  src/taxCalculator.test.ts
  ✕ calculateTaxIncludedPrice is not defined
  ✕ calculateTax is not defined
```

#### 4.2 Green (Minimal Implementation)

Request minimal implementation from AI.

**Example Prompt to AI**:
```
Please create minimal implementation to pass the following test cases:

[Paste test cases]

Requirements:
- Prioritize passing tests
- Don't overcomplicate
- Implement error handling appropriately
```

**Generated Implementation Example**:
```typescript
export function calculateTaxIncludedPrice(price: number, taxRate: number): number {
  validateInputs(price, taxRate);
  const tax = calculateTax(price, taxRate);
  return Math.round((price + tax) * 10) / 10;
}

export function calculateTax(price: number, taxRate: number): number {
  validateInputs(price, taxRate);
  return Math.round((price * taxRate) * 10) / 10;
}

function validateInputs(price: number, taxRate: number): void {
  if (price < 0) {
    throw new Error('Price must be 0 or greater');
  }
  if (taxRate < 0 || taxRate > 1) {
    throw new Error('Tax rate must be between 0 and 1');
  }
}
```

Run tests:
```bash
npm test
# Confirm all tests pass
```

#### 4.3 Refactor

Request code improvements from AI.

**Example Prompt to AI**:
```
Please refactor the following code:

[Paste implementation code]

Improvements:
- Improve code readability
- Separate function responsibilities
- Optimize performance
- Enhance TypeScript type safety
```

#### 4.4 Validation

Have AI validate implementation appropriateness.

**Example Prompt to AI**:
```
Please comprehensively validate the following implementation:

[Paste refactored code]
[Also paste test cases]

Validation perspectives:
- Consistency with specification requirements
- Code quality (readability, maintainability)
- Test coverage appropriateness
- Potential issues
- Performance concerns
```

### Step 5: Final Review

**Human Review Checkpoints**:

**Functionality**:
- [ ] All tests pass
- [ ] Specification requirements are met
- [ ] Error handling is appropriate

**Code Quality**:
- [ ] High readability
- [ ] Appropriate function separation
- [ ] Follows naming conventions
- [ ] TypeScript type definitions are appropriate

**Maintainability**:
- [ ] Extensible structure
- [ ] Tests are maintainable
- [ ] Documentation is appropriate

## Reflection and Learning Points

### Success Patterns

**Process Adherence**:
- Execute each step without skipping
- Conduct human reviews thoroughly
- Don't blindly accept AI output

**Appropriate AI Utilization**:
- Use clear and specific prompts
- Provide sufficient context
- Specify expected output format

### Common Failure Patterns and Countermeasures

**Failure 1: Ambiguous Prompts**
```
❌ Bad example: "Create a tax calculation function"
✅ Good example: "Create a function to calculate tax-inclusive price from product price and tax rate according to the following specification: [detailed specification]"
```

**Failure 2: Skipping Human Review**
```
❌ Problem: Using AI output as-is
✅ Solution: Always verify consistency with specifications
```

**Failure 3: Insufficient Test Design**
```
❌ Problem: Only normal case tests
✅ Solution: Comprehensive tests including boundary values and abnormal cases
```

### Preparation for Next Steps

Key insights to gain from this first project:
- Rhythm of collaborative development with AI
- Importance of quality control
- Basics of prompt design
- Understanding review points

In the next chapter, we'll develop application skills for AITDD through implementing more complex CRUD operations.

## Summary

In your first AITDD project, focus on:

1. **Gaining Success Experience**: Experience the complete process even at small scale
2. **Establishing Basic Habits**: Understand the importance of each step
3. **Developing AI Utilization Sense**: Foundation of appropriate prompt design
4. **Cultivating Quality Awareness**: Realize the value of human review

With these foundations, you'll be able to effectively utilize AITDD even in more complex projects.