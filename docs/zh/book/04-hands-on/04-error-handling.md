# 4.4 错误处理和调试

## 学习目标

在本章中，您将学习处理 AITDD 开发中发生的各种错误的方法和有效的调试技术：

- 理解 AI 生成代码特有的错误模式
- 识别和修复提示引起的错误
- 建立高效的调试流程
- 决定何时切换到手动实现及其实践
- 错误预防的最佳实践

## 错误分类和应对策略

在 AITDD 开发中，会发生与传统开发不同类型的错误。适当地分类这些错误并应用相应的对策是很重要的。

### 错误的基本分类

**1. 提示引起的错误**
- 由于指令模糊
- 由于需求不明确
- 由于上下文不足

**2. AI 实现引起的错误**
- AI 生成代码的错误
- 与现有代码的兼容性问题
- 性能问题

**3. 集成引起的错误**
- 多功能集成时的问题
- 接口不匹配
- 依赖关系问题

**4. 传统型错误**
- 一般编程错误
- 环境配置问题
- 外部依赖问题

## 实践调试流程

### 步骤1：错误信息的全面收集

当错误发生时，首先系统地收集信息。

**要收集的信息**：
```markdown
## 错误信息收集清单

### 基本信息
- [ ] 错误消息（完整版）
- [ ] 堆栈跟踪
- [ ] 发生时机
- [ ] 执行环境

### 上下文信息
- [ ] 执行的命令
- [ ] 输入数据
- [ ] 期望的行为
- [ ] 实际的行为

### AI 相关信息
- [ ] 使用的提示
- [ ] AI 生成代码的相关部分
- [ ] 相关的现有代码

### 执行环境
- [ ] 操作系统/浏览器信息
- [ ] Node.js/TypeScript 版本
- [ ] 依赖包的状态
```

**信息收集实例**：
```
错误发生时的记录示例：

错误消息：
TypeError: Cannot read property 'map' of undefined
    at getAllTasks (TaskController.ts:15)
    at Router.handle (express/lib/router/layer.js:95)

发生时机：
执行 GET /api/tasks 端点测试时

输入数据：
GET http://localhost:3000/api/tasks

期望行为：
返回包含空数组的响应

实际行为：
对 undefined 调用 map 出错

使用的提示：
"请实现调用 TaskManager 类的 getAllTasks 方法并返回响应的 API 端点"
```

### 步骤2：使用 AI 进行原因分析

将收集的所有信息传递给 AI，请求错误原因分析。

**有效的分析提示**：
```
请分析以下错误的原因：

## 错误信息
[粘贴所有收集的错误信息]

## 相关代码
[发生错误的完整源代码]

## 分析要求
1. 错误的直接原因
2. 根本原因（设计/实现问题）
3. 提示引起的可能性
4. 修复方案建议
5. 类似错误的预防措施

请尽可能提供具体和实用的分析。
```

**AI 分析示例**：
```
## 错误分析结果

### 直接原因
TaskController.getAllTasks 方法中，taskManager.getAllTasks() 返回了 undefined

### 根本原因
1. TaskManager 类实例化问题
2. getAllTasks 方法实现中任务数组未初始化

### 提示引起的可能性
"使用现有 TaskManager 类"的指令模糊，
可能在未确认实际类定义的情况下实现

### 修复方案
1. 确认 TaskManager 类初始化
2. 验证 getAllTasks 方法返回值
3. 添加 null/undefined 检查

### 预防措施
1. 在提示中包含现有代码的详细信息
2. 增强返回值的类型安全性
3. 在单元测试中添加 null/undefined 情况
```

### 步骤3：渐进式调查和假设验证

基于 AI 的分析，逐步调查问题。

**调查步骤示例**：
```typescript
// 1. TaskManager 类状态确认
describe('TaskManager 调试', () => {
  test('TaskManager 实例化确认', () => {
    const manager = new TaskManager();
    console.log('TaskManager instance:', manager);
    expect(manager).toBeDefined();
  });

  test('getAllTasks 返回值确认', () => {
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    console.log('getAllTasks result:', result);
    console.log('result type:', typeof result);
    expect(result).toBeDefined();
  });

  test('任务数组初始状态确认', () => {
    const manager = new TaskManager();
    const tasks = manager.getAllTasks();
    expect(Array.isArray(tasks)).toBe(true);
    console.log('Initial tasks array:', tasks);
  });
});
```

**调试执行**：
```bash
npm test -- --verbose TaskManager调试
```

### 步骤4：与 AI 合作的修复实现

将调查结果反馈给 AI，请求修复实现。

**修复请求提示**：
```
调试调查的结果，发现了以下问题：

## 调查结果
[调试测试的输出结果]

## 发现的问题
1. TaskManager 类的数组初始化不当
2. getAllTasks 方法的返回值是 undefined

## 修复要求
请实现满足以下条件的修复：
1. 数组被正确初始化
2. 确保类型安全性
3. 添加 null/undefined 检查
4. 现有测试通过
5. 添加新的测试用例

请提供修复后的代码和修复原因的说明。
```

## 识别和修复提示引起的错误

### 判断提示问题的标准

**通过频率判定**：
```
相似错误模式的发生情况：
- 第1次：作为实现错误处理
- 第2次：考虑提示问题的可能性
- 第3次：必须修复提示
```

**典型提示问题的迹象**：
- 相同要求生成不同实现
- 输出与期望大相径庭
- 意外修改现有代码
- 超出指令范围的实现

### 提示问题的修复流程

#### 1. 提示诊断

**AI 诊断请求**：
```
请分析以下提示并指出问题：

## 使用的提示
[有问题的提示]

## 期望的结果
[期望的输出]

## 实际的结果
[实际的输出]

## 诊断要求
1. 提示中模糊的部分
2. 缺少的信息
3. 可能引起误解的表达
4. 改进建议
```

#### 2. 创建提示改进方案

**改进提示的示例**：
```
改进前（有问题的提示）：
"请使用 TaskManager 类创建 API 端点"

改进后：
"请使用以下现有 TaskManager 类实现 GET /api/tasks 端点。

现有代码：
[TaskManager 类的完整代码]

要求：
1. 完全不要修改现有代码
2. 使用 Express.js 的 Router
3. 响应格式：{ success: boolean, data: Task[] }
4. 包含错误处理
5. 确保 TypeScript 类型安全

输出格式：
- routes/tasks.ts 文件
- controllers/TaskController.ts 文件
- 对应的测试文件"
```

#### 3. 改进提示的验证

**验证过程**：
```typescript
describe('提示改进验证', () => {
  test('使用改进前提示重现问题', async () => {
    // 使用有问题的提示请求实现
    // 确认期望的问题发生
  });

  test('使用改进后提示确认解决', async () => {
    // 使用改进的提示请求实现
    // 确认问题得到解决
  });

  test('改进提示的副作用确认', async () => {
    // 确认改进没有引起新的问题
  });
});
```

## 切换到手动实现的判断

### 切换时机的判断

**实现前的判断**：
```markdown
## 手动实现考虑清单

### 实现图像的有无
- [ ] 具体的实现步骤浮现在脑海中
- [ ] 要使用的技术/库明确
- [ ] 可以预测实现的陷阱

### 技术复杂性
- [ ] 需要性能优化
- [ ] 需要复杂的算法
- [ ] 需要深入的领域知识

### 向 AI 解释的困难度
- [ ] 无法清晰地表达需求
- [ ] 提示变得异常冗长
- [ ] 难以解释前提知识
```

**实现中的切换判断**：
```
切换判断的标准：
1. 相同错误发生3次以上
2. 修改提示也无法解决
3. 调试时间超过实现时间
4. AI 生成代码的质量不一致
```

### 有效的手动实现方法

#### 渐进式 AI 并用策略

**部分 AI 利用而非完全手动**：
```typescript
// 1. 复杂逻辑部分手动实现
const complexAlgorithm = (data: any[]) => {
  // 手动实现（难以向 AI 解释的部分）
  let result = [];
  for (let i = 0; i < data.length; i++) {
    // 复杂的计算逻辑
  }
  return result;
};

// 2. 定型代码请求 AI
const generateApiResponse = (data: any) => {
  // 这部分可以由 AI 生成
  return {
    success: true,
    data,
    meta: {
      timestamp: new Date().toISOString(),
      count: Array.isArray(data) ? data.length : 1
    }
  };
};

// 3. 组合为最终实现
export const processData = async (inputData: any[]) => {
  try {
    const processedData = complexAlgorithm(inputData); // 手动部分
    return generateApiResponse(processedData); // AI 生成部分
  } catch (error) {
    // 错误处理也可以 AI 辅助
  }
};
```

#### 手动实现中的 AI 支持利用

**1. IDE 自动完成功能的利用**：
```typescript
// 积极使用 VS Code 等的 AI 自动完成
const taskManager = new TaskManager();
// ↑ 在 AI 自动完成有效的地方利用
```

**2. 部分代码生成请求**：
```
对有明确实现图像的部分的 AI 请求示例：

"请基于以下类型定义创建验证函数：

interface TaskInput {
  title: string;
  description?: string;
}

要求：
- title 必填，1-100个字符
- description 可选，0-500个字符
- 无效时提供具体的错误消息
- 作为 TypeScript 类型守卫功能
```

#### 继续质量流程

**即使手动实现也必须进行 Validation 步骤**：
```
手动实现的 Validation 检查项目：
1. 与规范要求的一致性
2. 类型安全性的确保
3. 错误处理的适当性
4. 测试覆盖率的确认
5. 性能的合理性
6. 代码的可读性/可维护性
```

## 错误预防的最佳实践

### 改进提示设计

**1. 明确上下文**：
```
好的提示示例：

"请向以下现有系统添加新功能：

现有代码：
[粘贴所有相关代码]

新功能要求：
[具体明确的要求]

约束条件：
- 禁止修改现有代码
- 确保 TypeScript 类型安全
- 必须进行错误处理

期望的输出：
- 实现代码
- 测试代码
- 使用示例
- 注意事项"
```

**2. 渐进式实现指示**：
```
复杂功能的渐进式实现：

"请渐进式地实现以下内容：

步骤1：接口设计
步骤2：基本实现
步骤3：错误处理
步骤4：测试创建

请在每个步骤进行确认后继续。"
```

### 加强测试策略

**1. AI 生成代码特有的测试**：
```typescript
describe('AI 生成代码验证测试', () => {
  test('确认没有意外的现有代码修改', () => {
    // 测试重要的现有函数没有被更改
    const originalFunction = require('./legacy/original-module');
    expect(originalFunction.criticalMethod).toBeDefined();
    expect(typeof originalFunction.criticalMethod).toBe('function');
  });

  test('推测实现范围的确认', () => {
    // 确认 AI 推测实现的部分在要求范围内
    const implementation = new FeatureImplementation();
    expect(implementation.getImplementedFeatures())
      .toEqual(expect.arrayContaining(REQUIRED_FEATURES));
  });

  test('类型安全性确认', () => {
    // 确认 TypeScript 类型检查正常工作
    // 测试不会发生编译错误
  });
});
```

**2. 加强集成测试**：
```typescript
describe('功能集成测试', () => {
  test('3功能集成的回归确认', async () => {
    // 测试3个功能组合也能正常工作
    const feature1 = await executeFeature1();
    const feature2 = await executeFeature2(feature1.result);
    const feature3 = await executeFeature3(feature2.result);
    
    expect(feature3.result).toMatchExpectedOutput();
  });
});
```

### 持续改进流程

**1. 错误模式的积累**：
```markdown
## 错误模式管理

### 高频错误
1. undefined/null 访问错误
   - 原因：AI 生成代码中初始化不足
   - 对策：明确的初始化指示

2. 类型不匹配错误
   - 原因：提示中类型信息不足
   - 对策：明确提供类型定义

3. 现有代码修改错误
   - 原因："禁止修改"指示不彻底
   - 对策：具体的约束指示
```

**2. 提示模板的改进**：
```
改进的提示模板：

### 基本模板
```
功能: [功能名]
实现对象: [具体的实现内容]

现有代码:
[所有相关代码]

要求:
[明确具体的要求]

约束:
- 禁止修改现有代码
- [其他约束]

输出要求:
- [期望的输出格式]

质量要求:
- TypeScript 类型安全
- 错误处理
- 包含测试代码
```

## 实用调试技术

### 基于日志的调试

**有效的日志输出**：
```typescript
// 调试日志帮助器
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

// 使用示例
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

### 测试驱动调试

**从失败测试逆推**：
```typescript
describe('错误重现测试', () => {
  test('特定条件下的 null 错误重现', () => {
    // 确定错误发生的最小条件
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    
    // 此时应该发生错误
    expect(() => result.map(x => x.id)).toThrow();
  });

  test('修复后的行为确认', () => {
    // 测试修复后期望的行为
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    
    expect(Array.isArray(result)).toBe(true);
    expect(() => result.map(x => x.id)).not.toThrow();
  });
});
```

## 总结

在本章中，我们学习了 AITDD 开发中的全面错误处理和调试技术：

**主要学习成果**：
- 适当的错误分类和对策
- 利用 AI 的高效调试流程
- 识别和修复提示引起的错误
- 切换到手动实现的判断和 AI 并用策略
- 错误预防的最佳实践

**实践技能**：
- 全面的信息收集技术
- 与 AI 合作的错误分析
- 渐进式问题解决方法
- 质量管理的持续改进

**为下一章做准备**：
有了这些技能，您已准备好学习更高级的 AITDD 技术和优化技术。我们将进入提示设计和 AI 利用的优化。

通过 AITDD 实践动手系列，您已经掌握了从基础到应用的一整套技术。在下一部分中，我们将进一步完善这些技术，学习在实际生产环境中利用它们的高级方法。