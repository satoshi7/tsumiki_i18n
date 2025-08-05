# 4.2 CRUD 操作实现示例

## 学习目标

在本章中，您将通过实现更实际的 CRUD（创建、读取、更新、删除）操作来学习以下内容：

- 利用 AITDD 集成多个功能
- 设计和实现数据管理逻辑
- 体验接近真实的应用程序开发
- 感受与感觉编码的区别

## 项目概述：简单任务管理系统

### 要实现的功能

创建一个简单的基于内存的任务管理系统：

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

**CRUD 操作**：
- **创建（Create）**：创建新任务
- **读取（Read）**：检索任务（全部、单个、条件搜索）
- **更新（Update）**：更新任务
- **删除（Delete）**：删除任务

### 技术复杂度

与上一章的计算器函数相比，增加了以下复杂性：
- 数据持久化（基于内存）
- 多实体操作
- 验证组合
- 多样化的错误处理

## 动手实践

### 第1步：TODO 创建和功能分解

AITDD 的一个重要教训是**集成约3个功能是实际限制**。因此，我们将功能分解为适当的粒度。

```markdown
# TODO: 任务管理系统 CRUD 实现

## 第1阶段：基础实现
- [ ] Task 接口定义
- [ ] TaskManager 类的基本结构
- [ ] 内存存储实现

## 第2阶段：基本 CRUD（最多3个功能）
- [ ] Create：创建新任务
- [ ] Read：获取所有任务
- [ ] Read：按ID获取单个任务

## 第3阶段：扩展 CRUD（剩余功能）
- [ ] Update：更新任务
- [ ] Delete：删除任务
- [ ] Read：条件搜索（按完成状态等）

## 技术要求
- 使用 TypeScript
- 使用 Jest 测试
- 基于内存的数据管理
- 适当的错误处理
```

**关键点**：
- 通过分阶段控制集成复杂性
- 每个阶段都可以独立测试
- 可以逐步添加功能

### 第2步：第1阶段实现

#### 规范创建

**给 AI 的提示**：
```
请为任务管理系统的基础部分创建规范。

要求：
- Task 接口定义
- TaskManager 类设计
- 基于内存的存储功能
- TypeScript 类型安全

请参考以下 TODO：
[粘贴第1阶段 TODO]
```

**生成的规范示例**：
```markdown
# 任务管理系统基础规范

## Task 接口
```typescript
interface Task {
  id: string;          // UUID 格式的唯一ID
  title: string;       // 任务标题（1-100个字符）
  description: string; // 任务描述（0-500个字符）
  completed: boolean;  // 完成状态
  createdAt: Date;     // 创建日期
  updatedAt: Date;     // 更新日期
}
```

## TaskManager 类
- 基于内存的任务管理
- 任务持久化（数组）
- 自动ID生成
- 基本验证

## 错误处理
- 任务未找到：TaskNotFoundError
- 验证失败：ValidationError
- 无效ID格式：InvalidIdError
```

#### 测试用例创建

**AI 提示**：
```
请基于以下规范创建基础部分的测试用例：

[粘贴基础规范]

测试视角：
- 接口类型检查
- TaskManager 初始化
- 内存存储操作验证
- 错误类定义验证
```

#### 执行 Red-Green-Refactor-Validation

向 AI 请求逐步实现：

1. **Red**：确认测试失败
2. **Green**：基础类的最小实现
3. **Refactor**：设计改进
4. **Validation**：基础有效性确认

### 第3步：第2阶段实现（基本 CRUD）

#### 集成的重要注意事项

与感觉编码的重要区别是需要**结构化方法**：

**AITDD 方法**：
```
1. 明确的规范定义
2. 全面的测试设计
3. 渐进式实现
4. 质量检查
→ 轻松集成，稳定质量
```

**感觉编码方法（要避免）**：
```
1. 向 AI 提出冲动的实现请求
2. 后添加测试
3. 集成时发现问题
4. 手动修复
→ 3个功能集成就会崩溃
```

#### Create 功能实现

**规范**：
```markdown
## 任务创建功能

### 方法：createTask(taskData)
- 参数：{ title: string, description: string }
- 返回值：创建的 Task
- 验证：
  - title 必填，1-100个字符
  - description 0-500个字符
- 自动设置：id、createdAt、updatedAt、completed=false
```

**AI 提示示例**：
```
请使用以下规范创建 createTask 方法的测试用例和实现：

[粘贴规范]

要求：
1. 全面的测试用例（正常、异常、边界值）
2. 使用 Red-Green-Refactor-Validation 循环实现
3. 确保与现有基础代码的一致性
4. 利用 TypeScript 类型安全
```

#### Read 功能实现

同时实现**获取全部和获取单个**：

**规范**：
```markdown
## 任务检索功能

### getAllTasks(): Task[]
- 返回数组中的所有任务
- 如果为空则返回空数组
- 按创建日期降序排序

### getTaskById(id: string): Task
- 按ID搜索任务
- 如果未找到：TaskNotFoundError
- 无效ID格式：InvalidIdError
```

### 第4步：第3阶段实现（扩展 CRUD）

#### Update 功能

**支持部分更新**：

```typescript
interface TaskUpdateData {
  title?: string;
  description?: string;
  completed?: boolean;
}

updateTask(id: string, updateData: TaskUpdateData): Task
```

#### Delete 功能

包括**逻辑删除与物理删除**考虑的实现：

```typescript
deleteTask(id: string): boolean  // 物理删除
// 或
softDeleteTask(id: string): Task  // 逻辑删除
```

#### 条件搜索功能

**过滤功能**：

```typescript
interface TaskFilter {
  completed?: boolean;
  titleContains?: string;
  createdAfter?: Date;
}

searchTasks(filter: TaskFilter): Task[]
```

### 第5步：集成测试和最终审查

#### 集成场景测试

假设实际使用案例的测试：

```typescript
describe('任务管理集成场景', () => {
  test('常见任务管理流程', async () => {
    const manager = new TaskManager();
    
    // 1. 创建任务
    const task1 = manager.createTask({
      title: '项目规划',
      description: '需求定义和设计'
    });
    
    // 2. 获取并验证任务
    const allTasks = manager.getAllTasks();
    expect(allTasks).toHaveLength(1);
    
    // 3. 更新任务
    const updatedTask = manager.updateTask(task1.id, {
      completed: true
    });
    expect(updatedTask.completed).toBe(true);
    
    // 4. 搜索功能
    const completedTasks = manager.searchTasks({
      completed: true
    });
    expect(completedTasks).toHaveLength(1);
    
    // 5. 删除任务
    const deleted = manager.deleteTask(task1.id);
    expect(deleted).toBe(true);
    expect(manager.getAllTasks()).toHaveLength(0);
  });
});
```

#### 性能测试

**大数据操作验证**：

```typescript
describe('性能测试', () => {
  test('1000个任务操作', () => {
    const manager = new TaskManager();
    
    // 创建1000个任务
    const startTime = Date.now();
    for (let i = 0; i < 1000; i++) {
      manager.createTask({
        title: `任务 ${i}`,
        description: `描述 ${i}`
      });
    }
    const createTime = Date.now() - startTime;
    
    // 搜索性能
    const searchStart = Date.now();
    const results = manager.searchTasks({
      titleContains: '任务 1'
    });
    const searchTime = Date.now() - searchStart;
    
    expect(createTime).toBeLessThan(1000); // 1秒内
    expect(searchTime).toBeLessThan(100);  // 100毫秒内
  });
});
```

## 故障排除

### 常见问题和解决方案

#### 问题1：AI生成代码的一致性

**症状**：
- 对现有代码的意外修改
- 接口不一致
- 不同的命名约定

**解决方案**：
```
改进的提示示例：
"请在不更改任何现有代码的情况下，仅根据以下接口添加新方法：
[明确说明现有接口]"
```

#### 问题2：测试质量不足

**症状**：
- 缺少异常用例测试
- 缺少边界值测试
- 没有集成测试

**解决方案**：
```
审查清单：
- [ ] 涵盖所有正常、异常和边界用例
- [ ] 验证错误消息
- [ ] 使用实际用例进行测试
```

#### 问题3：集成复杂性

**症状**：
- 3个以上功能集成时崩溃
- 调试困难
- 测试不通过

**解决方案**：
```
渐进集成方法：
1. 一次完全实现一个功能
2. 测试2个功能的组合
3. 添加第3个功能时要小心
4. 如果出现问题，拆分功能
```

## AI 提示最佳实践

### 有效的提示设计

**1. 清晰的上下文**：
```
好的示例：
"在任务管理系统的 CRUD 操作中，请根据以下规范实现 createTask 方法。
通过添加到现有的 Task 接口和 TaskManager 类来实现，
使用基于内存的存储。

[现有代码]
[详细规范]
[预期输出格式]"
```

**2. 明确的约束**：
```
约束示例：
- "不要修改现有代码"
- "最大化 TypeScript 类型安全"
- "实现适当的错误处理"
- "先实现测试"
```

**3. 指定预期质量**：
```
质量要求示例：
- "以生产级质量实现"
- "优先考虑可读性和可维护性"
- "考虑性能实现"
- "包含适当的注释"
```

### 提示模板

CRUD 操作特定的提示模板：

```
### CRUD 实现提示模板

**基本信息**：
- 功能：[Create/Read/Update/Delete] 操作
- 目标实体：[实体名称]
- 实现语言：TypeScript
- 测试框架：Jest

**现有上下文**：
[现有接口/类定义]

**实现要求**：
[具体规范]

**约束**：
- 不修改现有代码
- 最大化类型安全
- 需要错误处理

**输出要求**：
1. 测试用例（正常/异常/边界）
2. 实现代码
3. 使用示例
4. 注意事项和限制
```

## 质量控制要点

### 代码审查清单

**功能质量**：
- [ ] 满足所有规范要求
- [ ] 适当的错误处理
- [ ] 边界时的正确行为
- [ ] 性能在可接受范围内

**技术质量**：
- [ ] 确保 TypeScript 类型安全
- [ ] 遵循命名约定
- [ ] 适当的抽象级别
- [ ] 遵守 DRY 原则

**测试质量**：
- [ ] 充分的测试覆盖率
- [ ] 测试易于理解
- [ ] 适当使用模拟
- [ ] 全面的集成测试

**可维护性**：
- [ ] 代码可读
- [ ] 结构允许轻松更改
- [ ] 适当的文档
- [ ] 设计考虑可扩展性

## 实践中的学习效果

### AITDD 优势（您将体验到的）

**开发速度**：
- 传统 CRUD 实现：1-2天
- 使用 AITDD：1小时内
- 体验**20-48倍的效率提升**

**质量稳定性**：
- 通过测试优先保证质量
- 在重构阶段进行优化
- 在验证步骤中进行全面质量检查

**学习效果**：
- AI 协作开发技能
- 有效的提示设计能力
- 提高对质量控制的敏感性

### 与感觉编码的明显区别

**集成的便利性**：
- 感觉编码：3个功能集成时崩溃
- AITDD：通过渐进集成保持稳定

**调试效率**：
- 感觉编码：重复相同的问题
- AITDD：系统化的问题解决

**长期可维护性**：
- 感觉编码：需要后续修复
- AITDD：实现持续开发

## 为下一章做准备

通过这次 CRUD 实现体验，您将获得以下技能：

1. **多功能集成管理**
2. **渐进式功能开发**
3. **理解质量控制的重要性**
4. **实用的 AI 提示设计技能**

在下一章中，我们将使用这些技能来应对更实际的开发场景：API 开发。您将学习 AITDD 如何处理外部依赖和异步处理等新复杂性。

## 总结

通过实现 CRUD 操作，我们学到了：

**过程的重要性**：
- 结构化方法的价值
- 渐进式功能添加的效果
- 质量控制的自动化

**AI 利用技巧**：
- 提供清晰的上下文
- 指定适当的约束
- 持续的提示改进

**实用技能**：
- 多功能集成技术
- 测试驱动的设计思维
- 审查和质量控制

有了这些基础，您将能够在更复杂的实际开发项目中有效地利用 AITDD。