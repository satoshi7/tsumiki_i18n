# 4.1 第一个AITDD项目

## 学习目标

通过本章的第一个AITDD项目，您将学到：

- 理解AITDD流程的整体脉络
- 实践从TODO创建到实现完成的完整步骤
- 体验AI辅助与人工审查的适当平衡
- 通过小规模功能感受AITDD的效果

## 项目选择标准

### 适合第一个项目的特征

**实现范围**：
- 可在30分钟至1小时内完成的小规模功能
- 具有单一职责的简单处理
- 外部依赖少、独立性高的功能

**技术复杂度**：
- 使用熟悉的技术栈
- 能够想象实现方法的内容
- 相对容易调试的处理

**学习效果**：
- 能够体验AITDD的各个步骤
- 容易获得成功体验的内容
- 可作为后续扩展基础的功能

### 推荐项目示例

**1. 简单计算功能**
```
示例：含税价格计算器
- 输入：商品价格、税率
- 输出：含税价格、税额
- 处理：基本数值计算
```

**2. 数据转换功能**
```
示例：CSV数据格式转换
- 输入：CSV字符串
- 输出：JSON格式数据
- 处理：解析和转换逻辑
```

**3. 验证功能**
```
示例：邮箱地址验证
- 输入：字符串
- 输出：验证结果（布尔值）
- 处理：使用正则表达式进行格式检查
```

## 实践练习：含税价格计算器

在这个示例中，我们将通过实现含税价格计算器来学习AITDD流程。

### 步骤1：TODO创建

在项目中创建todo.md文件，明确要实现的功能。

```markdown
# TODO: 实现含税价格计算器

## 要实现的功能
- [ ] 根据商品价格和税率计算含税价格的函数
- [ ] 计算税额的函数
- [ ] 输入值验证
- [ ] 错误处理（负值、null值等）

## 完成标准
- 正常值的计算能够正确工作
- 对异常值能够进行适当的错误处理
- 达到100%的测试覆盖率
```

**要点**：
- 具体分解功能
- 明确完成标准
- 调整粒度以在30分钟至1小时内完成

### 步骤2：规格创建

基于TODO与AI协作创建详细规格。

**给AI的提示示例**：
```
请根据以下TODO创建详细的规格说明书：

[粘贴todo.md的内容]

规格说明书中请包含以下内容：
- 函数签名（参数、返回值）
- 输入值约束
- 错误条件和应对
- 详细的计算逻辑
```

**期望的规格说明书（示例）**：
```markdown
# 含税价格计算器 规格说明书

## 函数规格

### calculateTaxIncludedPrice(price: number, taxRate: number): number
- 根据商品价格和税率计算含税价格
- 参数：
  - price: 商品价格（0以上的数值）
  - taxRate: 税率（0以上1以下的小数，例：0.1 = 10%）
- 返回值：含税价格（四舍五入到小数点后一位）

### calculateTax(price: number, taxRate: number): number
- 计算税额
- 参数与返回值同上
- 返回值：税额（四舍五入到小数点后一位）

## 错误处理
- 负价格：Error("价格必须为0或以上")
- 税率超出范围：Error("税率必须在0到1之间")
- null/undefined：Error("请输入有效数字")
```

**人工审查检查点**：
- [ ] TODO的意图是否正确反映
- [ ] 函数的责任范围是否适当
- [ ] 错误条件是否全面
- [ ] 内容是否可实现

### 步骤3：测试用例创建

基于规格说明书让AI创建测试用例。

**给AI的提示示例**：
```
请根据以下规格说明书创建全面的测试用例：

[粘贴规格说明书的内容]

请从以下角度创建测试用例：
- 正常情况：一般值的动作确认
- 边界值：0、1等边界的动作确认
- 异常情况：错误条件的动作确认
- 测试框架：使用Jest
```

**生成的测试用例示例**：
```typescript
import { calculateTaxIncludedPrice, calculateTax } from './taxCalculator';

describe('含税价格计算器', () => {
  describe('calculateTaxIncludedPrice', () => {
    // 正常情况
    test('一般的含税价格计算', () => {
      expect(calculateTaxIncludedPrice(1000, 0.1)).toBe(1100);
      expect(calculateTaxIncludedPrice(150, 0.08)).toBe(162);
    });

    // 边界值
    test('价格为0的情况', () => {
      expect(calculateTaxIncludedPrice(0, 0.1)).toBe(0);
    });

    test('税率为0的情况', () => {
      expect(calculateTaxIncludedPrice(1000, 0)).toBe(1000);
    });

    // 异常情况
    test('负价格报错', () => {
      expect(() => calculateTaxIncludedPrice(-100, 0.1))
        .toThrow('价格必须为0或以上');
    });

    test('税率超出范围报错', () => {
      expect(() => calculateTaxIncludedPrice(1000, -0.1))
        .toThrow('税率必须在0到1之间');
      expect(() => calculateTaxIncludedPrice(1000, 1.1))
        .toThrow('税率必须在0到1之间');
    });
  });

  describe('calculateTax', () => {
    test('税额计算', () => {
      expect(calculateTax(1000, 0.1)).toBe(100);
      expect(calculateTax(150, 0.08)).toBe(12);
    });
  });
});
```

**人工审查检查点**：
- [ ] 是否涵盖所有规格要求
- [ ] 异常情况测试是否适当
- [ ] 测试期望值是否正确
- [ ] 是否有需要追加的测试用例

### 步骤4：Red-Green-Refactor-Validation循环

#### 4.1 Red（测试失败）

运行测试确认预期的失败。

```bash
npm test
```

预期输出：
```
FAIL  src/taxCalculator.test.ts
  ✕ calculateTaxIncludedPrice is not defined
  ✕ calculateTax is not defined
```

#### 4.2 Green（最小实现）

向AI请求最小实现。

**给AI的提示示例**：
```
请创建能通过以下测试用例的最小实现：

[粘贴测试用例]

要求：
- 优先通过测试
- 不要过度复杂化
- 适当实现错误处理
```

**生成的实现示例**：
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
    throw new Error('价格必须为0或以上');
  }
  if (taxRate < 0 || taxRate > 1) {
    throw new Error('税率必须在0到1之间');
  }
}
```

运行测试：
```bash
npm test
# 确认所有测试通过
```

#### 4.3 Refactor（重构）

向AI请求代码改进。

**给AI的提示示例**：
```
请重构以下代码：

[粘贴实现代码]

改进点：
- 提高代码可读性
- 分离函数职责
- 优化性能
- 增强TypeScript类型安全性
```

#### 4.4 Validation（验证）

让AI验证实现的合理性。

**给AI的提示示例**：
```
请全面验证以下实现：

[粘贴重构后的代码]
[也粘贴测试用例]

验证观点：
- 与规格要求的一致性
- 代码质量（可读性、可维护性）
- 测试覆盖率的合理性
- 潜在问题点
- 性能方面的担忧
```

### 步骤5：最终审查

**人工审查检查点**：

**功能方面**：
- [ ] 所有测试都通过
- [ ] 满足规格要求
- [ ] 错误处理适当

**代码质量**：
- [ ] 可读性高
- [ ] 有适当的函数分离
- [ ] 遵循命名规范
- [ ] TypeScript类型定义适当

**可维护性**：
- [ ] 易于扩展的结构
- [ ] 测试易于维护
- [ ] 文档适当

## 反思与学习要点

### 成功模式

**遵守流程**：
- 不跳过任何步骤执行
- 确实进行人工审查
- 不盲目接受AI输出

**适当的AI活用**：
- 使用清晰具体的提示
- 提供充分的上下文
- 指定期望的输出格式

### 常见失败模式及对策

**失败1：提示模糊**
```
❌ 坏例子："创建一个税计算函数"
✅ 好例子："根据商品价格和税率计算含税价格的函数，请按照以下规格创建：[详细规格]"
```

**失败2：省略人工审查**
```
❌ 问题：直接使用AI的输出
✅ 对策：必须确认与规格的一致性
```

**失败3：测试设计不充分**
```
❌ 问题：只有正常情况的测试
✅ 对策：包括边界值、异常情况的全面测试
```

### 为下一步做准备

从这个初始项目中应该获得的感觉：
- 与AI协作开发的节奏
- 质量管理的重要性
- 提示设计的基础
- 理解审查要点

下一章中，我们将通过实现更复杂的CRUD操作来培养AITDD的应用能力。

## 总结

在第一个AITDD项目中重视以下几点：

1. **获得成功体验**：即使规模小也要体验完整流程
2. **建立基本习惯**：理解各步骤的重要性
3. **培养AI活用感**：适当提示设计的基础
4. **形成质量意识**：感受人工审查的价值

有了这些基础，即使在更复杂的项目中也能有效地活用AITDD。