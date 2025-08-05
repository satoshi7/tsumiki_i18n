# 5.2 AI 推理可视化技术

## 引言

在 AI 生成代码的质量管理中，最重要的是明确了解 AI 对哪些部分进行了"推测"。使用信号灯系统的 AI 推理可视化技术，可以实现高效的审查和高质量保证。

## 信号灯系统的理论背景

### 问题识别

AI 生成代码具有以下特性：

- **自动补全的广泛性**：AI 自动补全未明确说明的部分
- **似是而非的陷阱**：生成内容看起来合理，但可能与实际意图不同
- **推测依据的不明确性**：不清楚基于什么信息生成

### 解决方法

信号灯系统按以下标准对 AI 生成内容进行分类，明确审查优先级：

```
🟢 绿灯 → 🟡 黄灯 → 🔴 红灯
  安全     注意      危险
```

## 信号灯系统的详细定义

### 🟢 绿灯（高置信度·安全）

**定义：** 可以从参考的源文件中明确推测的内容

**特征：**
- 基于原始指示或规范书中明确记载的内容生成
- 遵循现有代码模式的实现
- 有明确依据的实现判断

**具体示例：**
```javascript
// 当规范书明确记载"用户ID是必需的"时
function validateUser(userId) {
  if (!userId) {  // 🟢 从规范书明确推导
    throw new Error('User ID is required');
  }
}
```

**审查优先级：** 低
**确认要点：** 实现的准确性、性能影响

### 🟡 黄灯（中置信度·注意）

**定义：** 不在参考的源文件中，但认为合理的内容

**特征：**
- 基于 AI 合理推测的补全
- 基于一般最佳实践的实现
- 利用领域知识的推理

**具体示例：**
```javascript
// 规范书中没有详细说明时的错误处理
function processData(data) {
  try {
    return transform(data);
  } catch (error) {  // 🟡 一般性但需要确认
    console.error('Data processing failed:', error);
    return null;
  }
}
```

**审查优先级：** 高
**确认要点：** 推测的合理性、与业务需求的一致性

### 🔴 红灯（需要判断·危险）

**定义：** 不在参考的源文件中，且无法直接推测的内容

**特征：**
- 基于 AI 独立判断的生成
- 对组织特有习惯或规则的假设
- 没有明确依据的实现选择

**具体示例：**
```javascript
// 没有关于组织日志格式信息时
function logUserAction(action) {
  // 🔴 日志格式是组织特有的，需要确认
  logger.info(`[AUDIT] User performed: ${action} at ${new Date().toISOString()}`);
}
```

**审查优先级：** 最高
**确认要点：** 与组织规则的一致性、安全影响

## 实现方法和 TODO 文件格式

### 标准 TODO 文件格式

```markdown
## [步骤名]结果 TODO

### 🟢 高置信度项目
- [ ] 确认 [utils.js](./src/utils.js) 的类型定义与规范书一致
- [ ] 检查 [validation.js](./src/validation.js) 的必需项检查实现

### 🟡 中置信度项目
- [ ] 验证 [error-handler.js](./src/error-handler.js) 的错误响应格式的合理性
- [ ] 检查 [config.js](./src/config.js) 的默认值设置的组织政策符合性

### 🔴 需要判断的项目
- [ ] 详细确认：[logger.js](./src/logger.js) 的日志输出格式符合组织标准
- [ ] 详细确认：[auth.js](./src/auth.js) 的会话管理方式选择依据
```

### 在提示中的指示方法

**基本指示模板：**
```markdown
## AI 推理可视化指示

执行以下工作，并使用信号灯系统对生成内容进行分类：

**工作内容：**
[具体任务内容]

**分类标准：**
- 🟢 绿灯：可以从参考文件（[文件名]）明确推导
- 🟡 黄灯：合理推测但参考文件中未明确记载
- 🔴 红灯：基于独立判断的生成（组织特有内容等）

**输出文件：** `./todos/[步骤名]-inference-check.md`

**输出格式：**
为每个生成项目赋予信号灯标记，并以 TODO 格式创建检查项目
```

### 参考源文件的管理方法

**文件关系记录：**
```markdown
## 参考文件管理

**主要参考（Primary References）：**
- [`requirements.md`](./docs/requirements.md) - 基本需求定义
- [`api-spec.yaml`](./docs/api-spec.yaml) - API 规范

**辅助参考（Secondary References）：**
- [`existing-code/`](./src/existing/) - 现有实现模式
- [`config-samples/`](./config/) - 配置文件示例

**外部参考（External References）：**
- 技术文档（框架官方）
- 行业标准（RFC、W3C 等）

**推理依据追踪：**
- 🟢项目 → 在主要参考中明确记载
- 🟡项目 → 辅助参考＋一般知识
- 🔴项目 → 依据不明·独立判断
```

## 检查优先级的设定

### 优先级矩阵

| 信号 | 影响度·高 | 影响度·中 | 影响度·低 |
|------|-----------|-----------|-----------|
| 🔴 红灯 | **最优先** | 高优先 | 中优先 |
| 🟡 黄灯 | 高优先 | 中优先 | 低优先 |
| 🟢 绿灯 | 中优先 | 低优先 | **推迟** |

### 影响度评估标准

**影响度·高：**
- 涉及安全的实现
- 影响数据完整性
- 影响整个系统运行

**影响度·中：**
- 影响特定功能运行
- 影响用户体验
- 影响性能

**影响度·低：**
- 日志输出和注释
- 内部变量名
- 辅助功能

### 实践性的检查顺序

1. **🔴×影响度·高** - 立即确认·修正
2. **🔴×影响度·中** 以及 **🟡×影响度·高** - 下次工作开始前确认
3. **其他🔴项目** - 实现完成前必须确认
4. **🟡项目** - 审查时确认
5. **🟢项目** - 最终检查时确认

## 实际运营示例和案例研究

### 案例研究1：REST API 实现

**场景：** 用户管理 API 的实现
**参考文件：** `user-api-spec.yaml`、`existing-user-model.js`

**AI 生成结果的分类：**

```javascript
// 🟢 API 规范书中明确记载的端点
app.post('/api/users', async (req, res) => {
  
  // 🟡 一般性验证但规范书中没有详细说明
  if (!req.body.email || !req.body.password) {
    return res.status(400).json({ error: 'Email and password required' });
  }
  
  // 🔴 哈希算法依赖组织特有政策
  const hashedPassword = bcrypt.hashSync(req.body.password, 12);
  
  // 🟢 遵循现有模型模式的实现
  const user = new User({
    email: req.body.email,
    password: hashedPassword
  });
});
```

**生成的 TODO：**
```markdown
## API 实现结果 TODO

### 🟢 高置信度项目
- [ ] [user-controller.js](./src/controllers/user.js) 的端点定义与规范书一致
- [ ] [user-model.js](./src/models/user.js) 的现有模式遵循确认

### 🟡 中置信度项目
- [ ] [validation.js](./src/middleware/validation.js) 的错误消息格式的合理性
- [ ] [user-controller.js](./src/controllers/user.js) 的状态码选择确认

### 🔴 需要判断的项目
- [ ] 详细确认：[auth.js](./src/utils/auth.js) 的 bcrypt 盐轮数符合组织政策
```

### 案例研究2：测试用例生成

**场景：** 为上述 API 创建测试用例
**参考文件：** `user-api-spec.yaml`、`existing-test-patterns.js`

**分类结果：**
```javascript
describe('User API', () => {
  // 🟢 规范书中明确记载的测试用例
  it('should create user with valid email and password', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', password: 'password123' });
    
    expect(response.status).toBe(201);  // 🟢 符合规范书
  });
  
  // 🟡 一般性边缘情况（规范书中未明确记载）
  it('should reject invalid email format', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'invalid-email', password: 'password123' });
    
    expect(response.status).toBe(400);  // 🟡 基于推测
  });
  
  // 🔴 基于组织特有安全要求的推测
  it('should enforce password complexity requirements', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', password: '123' });
    
    expect(response.status).toBe(400);  // 🔴 依赖组织政策
  });
});
```

## 效果测量和改进方法

### 效果测量指标

**定量指标：**
- 审查时间缩短率
- 错误发现率提升
- 修订次数减少

**定性指标：**
- 审查效率提升
- 防止遗漏重要问题
- 开发者信心提升

### 运营数据示例

```markdown
## 信号灯系统实施效果（1个月）

**传统审查：**
- 平均审查时间：45分钟/功能
- 错误发现率：约60%
- 审查遗漏：每月3-4件

**信号灯系统实施后：**
- 平均审查时间：25分钟/功能（缩短44%）
- 错误发现率：约85%（提升25%）
- 审查遗漏：每月1件以下

**🔴项目的典型问题：**
- 安全相关：40%
- 组织政策违反：35%
- 配置·环境依赖：25%
```

### 持续改进方法

**1. 分类精度的提升：**
```markdown
## 分类标准改进日志

**第1-2周：**
- 问题：日志格式的分类模糊
- 改进：创建组织特有项目的检查清单

**第3-4周：**
- 问题：错误处理的分类不稳定
- 改进：明文化错误处理模式

**第2个月：**
- 问题：新技术栈的分类困难
- 改进：创建技术栈别指南
```

**2. 提示优化：**
```markdown
## 提示改进周期

**改进前的问题：**
- 🔴项目的检测率约70%
- 分类缺乏一致性

**改进内容：**
- 提供组织特有项目的明确列表
- 大量添加判断标准的具体示例

**改进后的效果：**
- 🔴项目的检测率提升到90%以上
- 分类的一致性大幅改善
```

## 实践性的实施步骤

### 步骤1：构建基本系统

```bash
# 准备项目结构
mkdir -p todos
mkdir -p docs/inference-guides

# 创建基本模板
cat > docs/inference-guides/classification-template.md << 'EOF'
## AI 推理分类模板

### 🟢 绿灯的判定标准
- 参考文件"[文件名]"中明确记载的内容
- 遵循现有代码的既定模式的实现

### 🟡 黄灯的判定标准  
- 基于一般最佳实践的实现
- 技术上合理但参考文件中未明确记载

### 🔴 红灯的判定标准
- 依赖于组织特有的政策或习惯
- 没有明确依据的独立判断
EOF
```

### 步骤2：明文化组织特有规则

```markdown
## 组织特有检查点

**安全相关：**
- [ ] 密码哈希算法和强度
- [ ] 会话管理方式
- [ ] API 认证方式

**日志·审计相关：**
- [ ] 日志输出格式和级别
- [ ] 审计日志的输出项目
- [ ] 日志保存期限和轮换

**编码规范：**
- [ ] 命名规则（变量、函数、类）
- [ ] 错误处理模式
- [ ] 注释编写规则
```

### 步骤3：建立团队运营

```markdown
## 团队运营规则

**分类工作的负责：**
- AI 执行者进行初始分类
- 审查者确认分类的合理性

**检查工作的分工：**
- 🔴项目：高级工程师确认
- 🟡项目：团队内配对审查  
- 🟢项目：自动测试＋轻微确认

**知识的积累：**
- 每周1次的分类标准审查会议
- 错误分类模式的共享
- 改进案例的文档化
```

## 故障排除

### 常见问题和对策

**问题1：分类不一致**

```markdown
**症状：** 相似内容的分类结果不同
**原因：** 分类标准模糊、提示指示不明确
**对策：**
1. 创建更具体的组织特有项目列表
2. 在提示中包含过去的分类示例
3. 记录和标准化判断困难事项
```

**问题2：遗漏🔴项目**

```markdown
**症状：** 重要的组织特有项目被分类为🟡或🟢
**原因：** AI 不理解组织的上下文
**对策：**
1. 提供组织特有项目的明确列表
2. 设定"有疑问时分类为🔴"的原则
3. 审查者检查分类合理性
```

**问题3：TODO 项目过多**

```markdown
**症状：** 生成的 TODO 项目数超出可执行范围
**原因：** 分类过于细致、影响度评估过宽
**对策：**
1. 类似项目的分组
2. 严格化影响度评估
3. 引入"重要度×紧急度"矩阵
```

## 实践练习

### 练习1：创建分类标准

请使用信号灯系统对以下代码片段进行分类：

```javascript
function authenticateUser(username, password) {
  // 情况1：用户存在检查
  const user = await User.findOne({ username });
  if (!user) {
    return { success: false, message: 'User not found' };
  }
  
  // 情况2：密码验证
  const isValid = await bcrypt.compare(password, user.hashedPassword);
  if (!isValid) {
    return { success: false, message: 'Invalid password' };
  }
  
  // 情况3：JWT 令牌生成
  const token = jwt.sign(
    { userId: user.id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
  
  // 情况4：日志输出
  console.log(`User ${username} authenticated successfully at ${new Date().toISOString()}`);
  
  return { success: true, token };
}
```

**参考文件：** `auth-spec.md`（仅记载基本认证流程）

### 练习2：设定 TODO 项目的优先级

基于上述分类结果，按优先级顺序排列 TODO 项目。

## 总结

通过 AI 推理的可视化技术，可以实现以下效果：

1. **高效审查**：集中于重要部分的质量检查
2. **风险缓解**：可靠地发现高风险推测部分
3. **知识积累**：明文化和共享组织特有的判断标准
4. **持续改进**：基于数据的分类标准和提示优化

信号灯系统是 AITDD 中平衡人为因素和 AI 辅助的重要技术。在下一节中，我们将学习利用这些技术的持续改进和提示优化。