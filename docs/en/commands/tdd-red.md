# TDD Red Phase (Write Failing Tests)

Execute the TDD Red phase.

## Preparation

Prepare the development context:

**Task Tool Execution**: `/tdd-load-context` to load TDD-related files and prepare context

After loading completion, start Red phase (failing test creation) work based on the prepared context information.

## Target Test Case

**【Target Test Case】**: {{test_case_name}}

## Test Case Addition Target Count

**Test Case Addition Target Count**: 10 or more (if less than 10 available test cases, add all)

Please select and implement 10 or more test cases from unimplemented test cases. If there are less than 10 available test cases, target all available test cases for implementation.
If test cases are already implemented, add tests from the test cases written in the test case definition.

## Reliability Level Instructions

When creating test code, comment on the verification status with source materials (requirements definition, existing implementation, library documentation, etc.) using the following signals:

- 🟢 **Green Signal**: When referencing source materials with almost no speculation
- 🟡 **Yellow Signal**: When making reasonable speculation from source materials
- 🔴 **Red Signal**: When speculation is not found in source materials

## Requirements

- **Language/Framework**: {{language_framework}}
- Tests must be created in a failing state
- Test names should be descriptive in Japanese
- Clearly describe assertions (expected value verification)
- Create by calling functions/methods not yet implemented

## Test Code Creation Guidelines

- Structure conscious of Given-When-Then pattern
- Test data preparation (Given)
- Actual processing execution (When)
- Result verification (Then)

## Required Japanese Comment Requirements

Test code must include the following Japanese comments:

### Comments at Test Case Start

```javascript
describe('{{feature_name}}', () => {
  test('{{test_case_name}}', () => {
    // 【テスト目的】: [Clearly state in Japanese what this test verifies]
    // 【テスト内容】: [Explain specifically what kind of processing to test]
    // 【期待される動作】: [Explain results when operating normally]
    // 🟢🟡🔴 Reliability Level: [How much this test content is based on source materials]

    // 【テストデータ準備】: [Reason why this data is prepared]
    // 【初期条件設定】: [Explain state before test execution]
    const input = {{test_input}};

    // 【実際の処理実行】: [Explain which function/method to call]
    // 【処理内容】: [Explain content of executed processing in Japanese]
    const result = {{function_name}}(input);

    // 【結果検証】: [Specifically explain what to verify]
    // 【期待値確認】: [Explain expected results and reasons]
    expect(result).toBe({{expected_output}}); // 【確認内容】: [Specific items verified by this check] 🟢🟡🔴
  });
});
```

### Setup/Cleanup Comments (when necessary)

```javascript
beforeEach(() => {
  // 【テスト前準備】: [Explain preparation work done before each test execution]
  // 【環境初期化】: [Reason and method for making test environment clean]
});

afterEach(() => {
  // 【テスト後処理】: [Explain cleanup work done after each test execution]
  // 【状態復元】: [Reason for restoring state to not affect next test]
});
```

### Comments for Each Expect Statement

Each expect statement must have Japanese comments:

```javascript
expect(result.property).toBe(expectedValue); // 【確認内容】: [Specific items and reasons verified by this check]
expect(result.array).toHaveLength(3); // 【確認内容】: [Reason for verifying array length matches expected value]
expect(result.errors).toContain('error message'); // 【確認内容】: [Reason for verifying specific error message is included]
```

## Example of Test Code to Create

```javascript
// Test file: {{test_file_name}}
describe('{{feature_name}}', () => {
  beforeEach(() => {
    // 【テスト前準備】: Initialize test environment before each test execution to ensure consistent test conditions
    // 【環境初期化】: Clean reset file system state to avoid influence from previous tests
  });

  afterEach(() => {
    // 【テスト後処理】: Delete temporary files and directories created after test execution
    // 【状態復元】: Return system to original state to not affect next test
  });

  test('{{test_case_name}}', () => {
    // 【テスト目的】: {{test_purpose}}
    // 【テスト内容】: {{test_description}}
    // 【期待される動作】: {{expected_behavior}}
    // 🟢🟡🔴 Reliability Level: [How much this test content is based on source materials]

    // 【テストデータ準備】: {{test_data_reason}}
    // 【初期条件設定】: {{initial_condition}}
    const input = {{test_input}};

    // 【実際の処理実行】: {{function_description}}
    // 【処理内容】: {{process_description}}
    const result = {{function_name}}(input);

    // 【結果検証】: {{verification_description}}
    // 【期待値確認】: {{expected_result_reason}}
    expect(result).toBe({{expected_output}}); // 【確認内容】: {{specific_verification_point}}
  });
});
```

## Please Provide

1. **Test Code**: In executable format with required Japanese comments
2. **Test Execution Command**: How to execute
3. **Expected Failure Message**: What kind of error should occur
4. **Comment Explanation**: Intent and purpose of each Japanese comment

After test code creation, execute the following:

1. **Memo File Creation/Update**: Create or append Red phase content to docs/implements/{{task_id}}/{feature_name}-memo.md file
   - If existing memo file exists, update Red phase section
   - If memo file doesn't exist, create new one
2. Save test code design content to docs/implements/{{task_id}}/{feature_name}-red-phase.md (append if existing file exists)
3. Update TODO status (mark Red phase completion)
4. **Quality Assessment**: Assess test code quality by the following criteria
   - Test execution: Executable and confirmed to fail
   - Expected values: Clear and specific
   - Assertions: Appropriate
   - Implementation policy: Clear
5. **Next Step Display**: Regardless of assessment results, display next recommended command
   - "Next recommended step: `/tdd-green` to start Green phase (minimum implementation)."

## TDD Memo File Format

Format for docs/implements/{{task_id}}/{feature_name}-memo.md file:

```markdown
# TDD Development Memo: {feature_name}

## Overview

- Feature name: [feature name]
- Development start: [date/time]
- Current phase: [Red/Green/Refactor]

## Related Files

- Original task file: `docs/tasks/{task file path}.md`
- Requirements definition: `docs/implements/{{task_id}}/{feature_name}-requirements.md`
- Test case definition: `docs/implements/{{task_id}}/{feature_name}-testcases.md`
- Implementation file: `[implementation file path]`
- Test file: `[test file path]`

## Red Phase (Failing Test Creation)

### Creation Date/Time

[date/time]

### Test Cases

[Overview of created test cases]

### Test Code

[Actual test code]

### Expected Failure

[What kind of failure is expected]

### Requirements for Next Phase

[Content to be implemented in Green phase]

## Green Phase (Minimum Implementation)

### Implementation Date/Time

[date/time]

### Implementation Policy

[Minimum implementation policy]

### Implementation Code

[Actual implementation code]

### Test Results

[Results of tests passing]

### Issues/Improvements

[Points to improve in Refactor phase]

## Refactor Phase (Quality Improvement)

### Refactor Date/Time

[date/time]

### Improvement Content

[Specific improvement content]

### Security Review

[Security verification results]

### Performance Review

[Performance verification results]

### Final Code

[Code after refactoring]

### Quality Assessment

[Final quality assessment]
```

## Quality Assessment Criteria

```
✅ High Quality:
- Test execution: Success (confirmed to fail)
- Expected values: Clear and specific
- Assertions: Appropriate
- Implementation policy: Clear

⚠️ Needs Improvement:
- Tests cannot be executed
- Expected values are ambiguous
- Implementation approach unclear
- Complex test cases
```

## TODO Update Pattern

```
- Mark current TODO "Red phase (failing test creation)" as "completed"
- Reflect completion of failing test creation phase in TODO content
- Record quality assessment results in TODO content
- Add next phase "Green phase (minimum implementation)" to TODO
```

Next step: `/tdd-green` to perform minimum implementation to make tests pass.