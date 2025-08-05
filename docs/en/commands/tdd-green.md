# TDD Green Phase (Implementation)

Execute the TDD Green phase.

## Preparation

Prepare the development context:

**Task Tool Execution**: `/tdd-load-context` to load TDD-related files and prepare context

After loading completion, start the Green phase (implementation) work based on the prepared context information.

**Use Task tools when executing tests**

## Reliability Level Instructions

When creating implementation code, comment on the verification status with original documents using the following signals:

- 🟢 **Green**: When hardly any speculation is made based on original documents
- 🟡 **Yellow**: When reasonable speculation is made from original documents
- 🔴 **Red**: When speculation is not found in original documents

## Goal

Perform **implementation** to make the tests created in the Red phase pass.

## Implementation Principles

- **Making tests pass is the top priority**
- Code beauty is secondary (improve in the next Refactor phase)
- "Just working" level is OK
- Keep complex logic for later, aim for simple implementation
- When tests are hard to pass, use Task tools to investigate failure causes before planning fixes
- Fix existing tests appropriately based on specifications if they error
- **Mock Usage Restrictions**: Do not write mocks in implementation code (write actual logic in implementation code)
- **File Size Management**: Consider file splitting when implementation files exceed 800 lines
- NEVER: Prohibit skipping necessary tests
- NEVER: Prohibit deleting necessary tests
- NEVER: Prohibit writing mocks/stubs in implementation code
- NEVER: Prohibit using in-memory storage instead of DB in implementation code
- NEVER: Prohibit omitting DB operations in implementation code

## Japanese Comment Requirements for Implementation

Implementation code must include the following Japanese comments:

### Function/Method Level Comments

```javascript
/**
 * 【機能概要】: [Explain what this function does in Japanese]
 * 【実装方針】: [Explain why this implementation method was chosen]
 * 【テスト対応】: [Specify which test cases this implementation addresses]
 * 🟢🟡🔴 Reliability Level: [How much this implementation is based on original documents]
 * @param {type} paramName - [Parameter description]
 * @returns {type} - [Return value description]
 */
function {{function_name}}(paramName) {
  // 【実装内容】: [Detailed description of implemented processing]
}
```

### Processing Block Level Comments

```javascript
function processData(input) {
  // 【入力値検証】: [Reason and method for checking input validity] 🟢🟡🔴
  if (!input) {
    throw new Error('入力値が不正です'); // 【エラー処理】: [Explain why this error is needed] 🟢🟡🔴
  }

  // 【データ処理開始】: [Indicate start of main processing] 🟢🟡🔴
  // 【処理方針】: [Explain how this processing contributes to passing tests] 🟢🟡🔴
  const result = {
    // 【結果構造】: [Explain return value structure and reason]
    validData: [],
    invalidData: [],
    errors: [],
  };

  // 【結果返却】: [Explain reason and content for returning processing result]
  return result;
}
```

### Variable/Constant Comments

```javascript
// 【定数定義】: [Reason this constant is needed and purpose of use]
const MAX_FILE_SIZE = 1024 * 1024; // 【制限値】: Set file size limit (1MB)

// 【変数初期化】: [Explain why this variable is needed for test passing]
let processedCount = 0; // 【カウンタ】: Counter to track number of processed files
```

### Error Handling Comments

```javascript
try {
  // 【実処理実行】: [Description of actual processing execution part]
  const data = processFile(filePath);
} catch (error) {
  // 【エラー捕捉】: [Policy for handling errors when they occur]
  // 【テスト要件対応】: [Processing to meet expected error handling in tests]
  return {
    success: false,
    error: error.message, // 【エラー情報】: Properly return error messages verified by tests
  };
}
```

## Implementation Example

```javascript
/**
 * 【機能概要】: Validate JSON file paths and classify valid/invalid paths
 * 【実装方針】: Implement only minimum necessary features to pass test cases
 * 【テスト対応】: Implementation to pass test cases created in tdd-red phase
 */
function {{function_name}}(input) {
  // 【入力値検証】: Early detection of invalid input values to prevent errors
  if (!input) {
    // 【エラー処理】: Handle error cases expected by tests
    throw new Error('入力値が必要です');
  }

  // 【最小限実装】: Simplest implementation to pass tests
  // 【ハードコーディング許可】: Fixed values OK at current stage as improvement planned for refactor stage
  return {{simple_return_value}};
}
```

## Gradual Implementation Guidelines

1. **First make only one test case pass**
   - 【実装戦略】: Avoid simultaneous handling of multiple tests to prevent complexity
   - 【品質確保】: Ensure quality by implementing one by one reliably
2. **Implement in the simplest way**
   - 【シンプル実装】: Add complex algorithms later in refactor
   - 【可読性重視】: Prioritize understandability at current stage
3. **Be conscious of file size during implementation**
   - 【800行制限】: Consider splitting when implementation files exceed 800 lines
   - 【モジュール設計】: Appropriately separate files by functional units
   - 【関数分割】: Split long functions into smaller units for implementation
   - 【責任境界】: Clarify responsibility scope of each file for implementation
   - 【分割戦略】: Separate files by function, layer, domain
4. **Consider code quality standards**
   - 【静的解析対応】: Aim for implementation without lint or typecheck errors
   - 【フォーマット統一】: Implementation matching existing project format
   - 【命名規則遵守】: Implementation following project naming conventions
5. **Postpone other test cases**
   - 【段階的開発】: Follow TDD principles, advance step by step
   - 【影響範囲限定】: Minimize impact of changes
6. **Minimal error handling**
   - 【必要最小限】: Implement only parts required by tests
   - 【将来拡張可能】: Plan to add detailed error processing in refactor stage
7. **Mock usage restrictions**
   - 【実装コード制限】: Do not use mocks/stubs in implementation code
   - 【テストコード限定】: Use mocks only in test code
   - 【実際のロジック実装】: Write actual processing in implementation code
   - 【依存関係注入】: Implement with dependency injection pattern as needed

## Please Provide

1. **Implementation Code**: Code that passes tests (with required Japanese comments)
2. **Test Execution Results**: Verification that tests actually pass using Task tools
3. **Implementation Explanation**: How you thought about implementation (correspondence with Japanese comments)
4. **Issue Identification**: Current implementation problems (clarify refactor targets)
5. **File Size Check**: Check implementation file line count (split plan when exceeding 800 lines)
6. **Mock Usage Verification**: Verify that implementation code does not contain mocks/stubs

After implementation completion, execute the following:

1. **Memo File Update**: Update Green phase section of docs/implements/{{task_id}}/{feature_name}-memo.md file
   - Record implementation policy, implementation code, test results, issues/improvements
   - Record in detail for reference in next Refactor phase
2. Save implementation code and design content to docs/implements/{{task_id}}/{feature_name}-green-phase.md (append if existing file exists)
3. Update TODO status (mark Green phase completion)
4. **Automatic Transition Decision**: Automatically execute `/tdd-refactor` if following conditions are met
   - Verified that all tests succeed using Task tools
   - Implementation is simple and understandable
   - There are obvious refactoring points
   - No functional problems
5. **Manual Confirmation**: If automatic transition conditions are not met, provide the following:
   - "Verified that tests passed using Task tools."
   - "Current implementation: [brief description]"
   - "Japanese comments included in implementation: [purpose and content of comments]"
   - "Refactoring candidates: [points to improve]"
   - "May we proceed to the next Refactor phase?"

## Quality Judgment Criteria

```
✅ High Quality:
- Test Results: All succeed with Task tool execution
- Implementation Quality: Simple and working
- Refactor Points: Clearly identifiable
- Functional Issues: None
- Compile Errors: None
- File Size: 800 lines or less or clear split plan
- Mock Usage: No mocks/stubs in implementation code

⚠️ Needs Improvement:
- Some tests fail (detected by Task tools)
- Implementation too complex
- Refactor policy unclear
- Functional concerns exist
- Compile errors exist
- File size exceeds 800 lines with unclear split plan
- Implementation code contains mocks/stubs
```

## TODO Update Pattern

```
- Mark current TODO "Green phase (minimum implementation)" as "completed"
- Reflect completion of minimum implementation phase in TODO content
- Record quality judgment results in TODO content
- Add next phase "Refactor phase (quality improvement)" to TODO
```

Next step: `/tdd-refactor` to improve code quality.