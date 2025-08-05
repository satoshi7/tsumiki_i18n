# TDD Refactor Phase (Code Improvement)

Execute the TDD Refactor phase.

## Preparation

Prepare the development context:

**Task Tool Execution**: `/tdd-load-context` to load TDD-related files and prepare context

After loading completion, start Refactor phase (code improvement) work based on the prepared context information.

## Reliability Level Instructions

When refactoring, comment on the verification status with source materials using the following signals:

- 🟢 **Green Signal**: When referencing source materials with almost no speculation
- 🟡 **Yellow Signal**: When making reasonable speculation from source materials
- 🔴 **Red Signal**: When speculation is not found in source materials

## Goal

Improve the code implemented in the Green phase from the following perspectives. **Tests must continue to pass** is the prerequisite.

## Improvement Perspectives

### 1. Readability Improvement

- Improve variable names and function names
- Enrich Japanese comments
- Make code structure more understandable

### 2. Eliminate Duplicate Code (DRY Principle)

- Commonize similar processing
- Extract constants
- Create helper functions

### 3. Design Improvement

- Apply single responsibility principle
- Organize dependencies
- Consider modularization

- NEVER: Write mocks/stubs in implementation code
- NEVER: Use in-memory storage instead of DB in implementation code

### 4. File Size Optimization

- Split/optimize to keep file size under 500 lines
- Split large files by function
- Set appropriate module boundaries

### 5. Code Quality Assurance

- Resolve lint errors
- Resolve typecheck errors
- Unify formatting
- Clear static analysis tool checks

### 6. Security Review

- Detect and fix implementations that could lead to vulnerabilities
- Strengthen input validation
- Verify SQL injection countermeasures
- Verify XSS (Cross-Site Scripting) countermeasures
- Verify CSRF (Cross-Site Request Forgery) countermeasures
- Avoid data leakage risks
- Proper implementation of authentication/authorization

### 7. Performance Review

- Analyze computational complexity of algorithms
- Optimize memory usage
- Remove unnecessary processing
- Consider cache strategies
- Optimize database queries
- Improve loop processing efficiency
- Proper implementation of asynchronous processing

### 8. Enhance Error Handling

- Input validation
- Appropriate error messages
- Improve exception handling

## Enhanced Japanese Comment Requirements for Refactoring

During refactoring, improve existing Japanese comments and add new comments:

### Improved Function/Method Comments

```javascript
/**
 * 【機能概要】: [Detailed description of functionality after refactoring]
 * 【改善内容】: [Explain what improvements were made]
 * 【設計方針】: [Reason why chose this design]
 * 【パフォーマンス】: [Performance considerations]
 * 【保守性】: [Improvements for maintainability]
 * 🟢🟡🔴 Reliability Level: [How much this improvement is based on source materials]
 * @param {type} paramName - [Detailed parameter description and constraints]
 * @returns {type} - [Detailed return value description and guarantees]
 */
function improvedFunction(paramName) {
  // 【実装詳細】: [Content and reason for improved implementation]
}
```

### Helper Function/Utility Comments

```javascript
/**
 * 【ヘルパー関数】: [Role of this function and reason for creation]
 * 【再利用性】: [What scenarios can this be reused in]
 * 【単一責任】: [Range of responsibility this function handles]
 */
function helperFunction(input) {
  // 【処理効率化】: [Improvements for processing efficiency] 🟢🟡🔴
  // 【可読性向上】: [Mechanisms to improve code readability] 🟢🟡🔴
}
```

### Constant/Configuration Value Comments

```javascript
// 【設定定数】: [Role of this constant and reason for setting] 🟢🟡🔴
// 【調整可能性】: [Possibility and method for future adjustments] 🟢🟡🔴
const IMPROVED_CONSTANT = 100; // 【最適化済み】: Optimized based on performance tests 🟢🟡🔴

// 【設定オブジェクト】: [Reason for grouping settings and management policy]
const CONFIG = {
  // 【各設定項目】: [Meaning and impact range of each setting value]
  maxRetries: 3, // 【リトライ回数】: Appropriate count based on operational experience
  timeout: 5000, // 【タイムアウト】: Time setting considering usability
};
```

### Error Handling Improvement Comments

```javascript
try {
  // 【安全な処理実行】: [Possibility of exceptions and countermeasures]
  const result = riskyOperation();
} catch (error) {
  // 【詳細エラー処理】: [Appropriate handling according to error type]
  // 【ユーザビリティ】: [User-friendly error handling]
  if (error.code === 'SPECIFIC_ERROR') {
    // 【特定エラー対応】: [Reason for handling this specific error]
    return handleSpecificError(error);
  }
  // 【一般エラー対応】: [Safe handling of unexpected errors]
  return handleGenericError(error);
}
```

## Refactoring Procedures

1. **Verify all current tests pass**
   - 【品質保証】: Operation verification before refactoring
   - 【安全性確保】: Prevent functional breakdown due to changes
   - 【実行方法】: Use Task tools to execute tests and analyze results in detail
2. **Code/Test Exclusion Check**
   - 【.gitignore確認】: Check if code files that should be checked are not excluded
   - 【テスト除外確認】: Check if tests are disabled with `describe.skip`, `it.skip`, `test.skip`, etc.
   - 【jest設定確認】: Check if test files are excluded in `jest.config.js` or `testPathIgnorePatterns` in `package.json`
   - 【実行対象確認】: Check if tests and code that should be executed are properly included
3. **Cleanup Development-Generated Files**
   - 【不要ファイル検出】: Detect and delete temporary files created during development
   - 【対象ファイルパターン】: Check files matching the following patterns
     - `debug-*.js`, `debug-*.ts`: Debug scripts
     - `test-*.js`, `test-*.ts`, `temp-*.js`: Temporary test files
     - `*.tmp`, `*.temp`, `*.bak`, `*.orig`: Temporary/backup files
     - `*~`, `.DS_Store`: Editor/system generated files
     - `test-output-*`, `*.test-output`: Test output files
   - 【安全確認】: Check file contents before deletion to ensure no important code is included
   - 【選択的削除】: Delete only files determined unnecessary, keep necessary files
   - 【削除ログ】: Record deleted files and deletion reasons as log
   - 【実行手順】:
     1. Detect files with `find . -name "debug-*" -o -name "test-*" -o -name "temp-*" -o -name "*.tmp" -o -name "*.temp" -o -name "*.bak" -o -name "*.orig" -o -name "*~" -o -name ".DS_Store" | grep -v node_modules`
     2. Check contents of each file with Read tool
     3. Delete files determined unnecessary and record deletion reason
4. **Conduct Security Review**
   - 【脆弱性検査】: Identify security holes in entire code
   - 【入力検証確認】: Verify defense against invalid input values
   - 【セキュリティガイドライン適用】: Apply industry standard security best practices
5. **Conduct Performance Review**
   - 【計算量解析】: Evaluate time/space complexity of algorithms
   - 【ボトルネック特定】: Identify problem areas in processing speed and memory usage
   - 【最適化戦略】: Plan specific performance improvement measures
6. **Apply small improvements one by one**
   - 【段階的改善】: Safe changes with limited impact scope
   - 【トレーサビリティ】: Ensure traceability of changes
7. **Execute tests after each improvement**
   - 【継続的検証】: Operation verification with each improvement
   - 【早期発見】: Early detection and correction of problems
   - 【実行方法】: Use Task tools to execute tests and verify impact of improvements
8. **Immediately revert if tests fail**
   - 【迅速復旧】: Quick response when problems occur
   - 【安定性維持】: Maintain stable system state

## Notes

- **Do not make functional changes** (adding new features is NG)
- **Fix immediately if tests stop passing**
- **Do not make large changes at once**
- **Also improve Japanese comment quality**
- **Use Task tools when executing tests for quality verification**

## Please Provide

1. **Security Review Results**: Presence/absence of vulnerabilities and countermeasures
2. **Performance Review Results**: Analysis of performance issues and improvement measures
3. **Improved Code**: Code after refactoring (with enhanced Japanese comments)
4. **Improvement Point Explanation**: What and how was improved (including security/performance perspectives)
5. **Test Execution Results**: Verification that all tests continue to pass using Task tools
6. **Quality Assessment**: Current code quality level (including security/performance evaluation)
7. **Comment Improvement Details**: How Japanese comments were enhanced

## Refactoring Example

```javascript
// Before: Hard coding
function add(a, b) {
  return 5; // Just working implementation
}

// After: Proper implementation (with improved Japanese comments)
/**
 * 【機能概要】: Add two numbers and return the result
 * 【改善内容】: Removed hard coding and implemented actual addition processing
 * 【設計方針】: Design emphasizing input validation and type safety
 * 【エラー処理】: Implemented proper exception handling for invalid inputs
 */
function add(firstNumber, secondNumber) {
  // 【入力値検証】: Early detection of non-numeric inputs to prevent errors
  // 【型安全性】: Runtime verification combined with TypeScript type checking
  if (typeof firstNumber !== 'number' || typeof secondNumber !== 'number') {
    // 【ユーザビリティ】: Provide clear error messages for developers
    throw new Error('Arguments must be numbers');
  }

  // 【メイン処理】: Simple and reliable addition processing
  // 【パフォーマンス】: Efficient implementation avoiding unnecessary processing
  return firstNumber + secondNumber;
}
```

After refactoring completion, execute the following:

1. **Final Memo File Update**: Update Refactor phase section and overview of docs/implements/{{task_id}}/{feature_name}-memo.md file
   - Record improvement details, security review results, performance review results
   - Record final code and quality assessment
   - Update current phase in overview section to "completed"
2. Save refactoring details and design improvements to docs/implements/{{task_id}}/{feature_name}-refactor-phase.md (append if existing file exists)
3. Update TODO status (mark Refactor phase completion)
4. **Quality Assessment**: Assess refactoring results quality by the following criteria
   - Test Results: All tests continue to succeed
   - Security: No major vulnerabilities found
   - Performance: No major performance issues found
   - Refactor Quality: Goals achieved
   - Code Quality: Improved to appropriate level
5. **Next Step Display**: Regardless of assessment results, display next recommended command
   - "Next recommended step: `/tdd-verify-complete` to execute completeness verification."

## Quality Assessment Criteria

```
✅ High Quality:
- Test Results: All continue to succeed with Task tool execution
- Security: No major vulnerabilities
- Performance: No major performance issues
- Refactor Quality: Goals achieved
- Code Quality: Appropriate level
- Documentation: Complete

⚠️ Needs Improvement:
- Some tests fail (detected by Task tools)
- Security vulnerabilities found
- Performance issues found
- Refactor goals not achieved
- Quality improvement insufficient
- Documentation incomplete
```

## TODO Update Pattern

```
- Mark current TODO "Refactor phase (quality improvement)" as "completed"
- Reflect completion of refactoring phase in TODO content
- Record quality assessment results in TODO content
- Add next phase "Completeness verification" to TODO
- Add new TODOs for areas requiring improvement if any
```