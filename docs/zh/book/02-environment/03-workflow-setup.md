# 2.3 开发环境与工作流构建

本章将说明如何构建有效实践AITDD的开发环境和工作流。通过不仅准备工具，而且将整个开发过程体系化，实现一致性的高质量开发。

## AITDD实际导入经历与方法论演变

### 导入时间线

#### 从2025年初开始的全面努力
**契机：**
- **Claude Sonnet 3.5** 和 **DeepSeek R1蒸馏模型** 的出现
- 确信某种程度的实现可以通过AI实现
- 感受到传统手动编码的局限性

**约5-6个月实践经验的积累：**
- 从最初的试错发展为系统化方法
- 发现提示设计优化模式
- 从失败案例中学习并建立最佳实践

### 方法论演变过程

#### 第1阶段：实时编码（初期方法）
**特征：**
- 与AI进行实时对话的同时创建代码
- 对小规模功能实现有效
- 通过即时反馈快速修正

**有效的场景：**
- 单个文件内的小规模修改
- 快速创建原型
- 为技术调研创建样本代码

**发现的局限性：**
- 大规模开发中结构容易崩溃
- 复杂依赖关系的管理困难
- 难以保持质量一致性

#### 第2阶段：与TDD结合（当前方法）
**问题认识：**
- 实时编码难以进行大规模开发
- 缺乏质量保证机制
- 需要保持设计一致性的机制

**采用的解决方案：**
- 与 **TDD（测试驱动开发）** 结合
- 建立 **Red-Green-Refactor-Validation** 循环
- 构建系统化工作流

**当前方法的特征：**
```
演变前（实时编码）:
需求 → 直接实现 → 运行确认 → 修正 → 完成

演变后（AITDD）:
需求 → TODO创建 → Red → Green → Refactor → Validation → 完成
         ↑                                          ↓
         ←←←←←←← 反馈循环 ←←←←←←←←←←
```

### 实际开发工作流构建经验

#### 项目结构优化过程
**初期课题：**
- 文件构成在各项目间不一致
- TODO粒度不合适
- Git历史难以追踪

**改善后的结构：**
```
project-root/
├── todo.md                    # 中心任务管理
├── docs/                      # 设计文档体系化
│   ├── requirements.md        # 明确的需求定义
│   ├── architecture.md        # 架构设计
│   └── api-spec.md           # 详细的API规范
├── src/                       # 按功能明确分离
├── tests/                     # 测试代码体系化
└── scripts/                   # 自动化脚本
```

#### TODO管理的演变过程

**初期问题：**
- TODO粒度过大（如"系统整体实现"等）
- 依赖关系不明确
- 进度难以追踪

**当前优化的方法：**

**发现合适的粒度：**
```markdown
# 最优粒度（30分钟～1小时）
- [x] 用户注册API实现
- [x] 密码验证功能
- [ ] JWT认证中间件
- [ ] 添加登录功能测试

# 应避免的粒度
❌ 系统整体实现（过大）
❌ 变量名变更（过小）
```

**实践性顺序执行策略：**
1. **从TODO列表顶部开始按顺序处理**
2. **一个项目完全完成后再进行下一个**
3. **有依赖关系的项目调整顺序**
4. **分割为30分钟～1小时可完成的单位**

### Git工作流的实践运用

#### AITDD特化的分支策略

**实际采用的策略：**
```bash
# 每个TODO项目的分支创建模式
git checkout -b feature/user-registration    # TODO: 用户注册API
git checkout -b feature/auth-middleware      # TODO: 认证中间件
git checkout -b feature/password-validation  # TODO: 密码验证
```

**AITDD循环对应提交策略：**
```bash
# Red阶段（创建失败测试）
git add tests/user-registration.test.js
git commit -m "Red: Add failing tests for user registration"

# Green阶段（通过测试的最小实现）
git add src/controllers/user.js
git commit -m "Green: Implement basic user registration"

# Refactor阶段（代码改善）
git add src/controllers/user.js src/models/user.js
git commit -m "Refactor: Extract user validation logic"

# Validation阶段（最终验证和文档化）
git add docs/api-spec.md
git commit -m "Validation: Complete user registration with docs"
```

#### 失败时恢复策略的实践

**实际运用中的判断基准：**
```bash
# 模式1: 轻微修正可对应
if [ "与期待的差异" == "小" ]; then
    # 通过提示调整重新执行
    echo "详细化提示后重试"
fi

# 模式2: 需要大幅修正
if [ "与期待的差异" == "大" ]; then
    git reset --hard HEAD~1  # 回到前一状态
    echo "重新审视提示后重新执行"
fi

# 模式3: 多次失败
if [ "失败次数" -gt 3 ]; then
    git reset --hard <last_known_good_commit>
    echo "根本性重新审视方法"
fi
```

**实际恢复模式例子：**
```
状况: 用户认证API实现中错误处理不当
判断: 3次修正尝试无改善
对应: git reset --hard HEAD~4 回到Red阶段
重新执行: 用更详细的提示重新开始
结果: 完成期待的实现
```

### 从实践中获得的重要教训

#### 成功要因分析

**1. 阶段性方法的效果：**
- 通过小步骤确实的进步
- 各阶段的质量确认
- 失败时影响范围的限定

**2. 文档化的重要性：**
- 需求定义的明确化直接关系到AI输出质量
- API规范书成为测试设计的指针
- 进度的可视化有助于维持动机

**3. 提示优化的累积效果：**
- 通过复用相同模式提高效率
- 通过失败案例分析提高精度
- 领域特化知识积累

#### 常见问题与对处法

**问题1: AI输出质量不稳定**
```
症状: 同样的提示在不同日子得到不同结果
原因: 提示的模糊性、语境不足
对处: 添加更具体的技术约束和示例

改善例子:
"创建API" 
↓
"用Express.js + Mongoose创建POST /api/users API:
- 请求: {name: string, email: string}
- 验证: email格式检查、name必须
- 响应: 201返回创建的用户信息
- 错误: 400(验证), 409(重复), 500(服务器)"
```

**问题2: 大规模项目中的迷失状态**
```
症状: 不知道当前工作位置
原因: TODO管理不完善、进度追踪缺失
对处: 明确的进度显示和下一个行动的明示

改善例子:
## 当前实现状况 (2025-06-21)
- [x] 用户管理功能 (完成)
- [ ] **认证功能 (实现中: JWT middleware创建阶段)**
- [ ] 权限管理功能 (未着手)

### 下一个行动
1. JWT签名验证的实现
2. 令牌刷新功能的添加
3. 登出功能的实现
```

**问题3: 测试与代码的不一致**
```
症状: 测试通过但实际动作与期待不同
原因: 测试设计不完善、需求理解偏差
对处: 创建更现实的测试用例

改善例子:
# 不充分的测试
test('should create user', () => {
  expect(user).toBeDefined();
});

# 改善的测试
test('should create user with valid email and return 201', async () => {
  const userData = { name: 'John', email: 'john@example.com' };
  const response = await request(app)
    .post('/api/users')
    .send(userData)
    .expect(201);
  
  expect(response.body.user.email).toBe(userData.email);
  expect(response.body.user.password).toBeUndefined(); // 不包含密码
});
```

## AITDD开发工作流全貌

### 基本开发流程
```
TODO列表创建 → 项目选择 → AITDD执行 → 审查 → 下一个项目
     ↑                                            ↓
     ←←←←←←←←←← 根据需要调整 ←←←←←←←←←←←←←←
```

### AITDD执行的详细循环
```
Red (测试创建) → Green (实现) → Refactor (改善) → Validation (验证)
      ↑                                                    ↓
      ←←←←←←←←←←←←← 反馈循环 ←←←←←←←←←←←←←←
```

## 项目结构设计

### 推荐目录结构

```
project-root/
├── todo.md                    # 主要TODO列表
├── docs/                      # 项目文档
│   ├── requirements.md        # 需求定义
│   ├── architecture.md        # 架构设计
│   └── api-spec.md           # API规范书
├── src/                       # 源代码
│   ├── models/               # 数据模型
│   ├── controllers/          # 控制器
│   ├── services/             # 业务逻辑
│   └── utils/                # 实用工具
├── tests/                     # 测试代码
│   ├── unit/                 # 单元测试
│   ├── integration/          # 集成测试
│   └── fixtures/             # 测试数据
├── scripts/                   # 开发用脚本
└── README.md                 # 项目概要
```

### TODO列表的创建和管理

#### TODO列表的基本格式

**todo.md 的例子：**
```markdown
# 项目TODO列表

## 当前实现对象
- [ ] 用户注册功能的实现

## 已完成
- [x] 项目初期设置
- [x] 数据库连接设置

## 未着手（优先度顺序）
1. [ ] 用户认证功能
   - [ ] 密码哈希化
   - [ ] JWT令牌生成
   - [ ] 登录API

2. [ ] 用户管理功能
   - [ ] 个人资料更新API
   - [ ] 用户删除API
   - [ ] 用户列表API

3. [ ] 安全强化
   - [ ] 速率限制实现
   - [ ] 输入值验证强化
   - [ ] CORS设置

## 今后考虑
- [ ] 性能优化
- [ ] 部署自动化
```

#### 有效TODO粒度的设置

**合适粒度的例子：**
- ✅ `用户注册API的实现`（30分钟～1小时）
- ✅ `密码验证功能`（30分钟～1小时）
- ✅ `JWT认证中间件`（30分钟～1小时）

**应避免的粒度：**
- ❌ `系统整体的实现`（过大）
- ❌ `变量名的变更`（过小）

### TODO的执行策略

#### 顺序执行方法
```markdown
执行方针:
1. 从TODO列表顶部开始按顺序处理
2. 一个项目完全完成后再进行下一个
3. 有依赖关系的项目调整顺序
4. 分割为30分钟～1小时可完成的单位
```

#### 依赖关系的管理
```markdown
依赖关系的例子:
- 用户模型 → 用户注册API → 用户认证
- 数据库设计 → 迁移 → API实现
- 基本功能 → 错误处理 → 安全强化
```

## Git工作流的设置

### AITDD向分支策略

#### 基本的分支模型
```bash
main                    # 生产环境用
├── develop            # 开发集成用
└── feature/todo-item  # 各TODO项目用
```

#### 分支创建的实例
```bash
# 按TODO项目创建分支
git checkout -b feature/user-registration
git checkout -b feature/user-authentication
git checkout -b feature/password-validation

# 按功能群汇总的情况
git checkout -b feature/user-management
git checkout -b feature/security-enhancement
```

### 提交策略

#### 对应AITDD循环的提交
```bash
# Red阶段（测试创建）
git add tests/
git commit -m "Red: Add tests for user registration"

# Green阶段（实现）
git add src/
git commit -m "Green: Implement user registration functionality"

# Refactor阶段（改善）
git add src/
git commit -m "Refactor: Improve user registration code structure"

# Validation阶段（验证）
git add .
git commit -m "Validation: Complete user registration with documentation"
```

#### 失败时的恢复策略
```bash
# AI没有产生期待结果的情况
git reset --hard HEAD~1  # 取消最后的提交
# 或者
git reset --hard <commit-hash>  # 返回到特定提交

# 调整提示后重新执行
# 成功后进行新的提交
```

## 实践的Next Steps

### 环境构建完成后的行动指针

1. **向第3章的迁移准备**
   - AITDD过程的详细理解
   - Red-Green-Refactor-Validation循环的掌握
   - 实际开发流程的体验

2. **最初项目计划**
   - 小规模样本项目的设计
   - 明确的功能需求定义
   - 可实现范围内的TODO列表创建

3. **持续改善的准备**
   - 提示设计技能的提高
   - 与AI的有效对话模式掌握
   - 审查和质量管理的实践

### 成功的重要要点

#### 采用阶段性方法
- **从小开始**: 从简单功能开始
- **逐渐扩展**: 积累成功体验
- **从失败中学习**: 不畏惧git reset进行试错

#### 对质量的持续注意
- **测试优先**: 必须从测试开始写
- **审查的习惯化**: AI生成的代码也必须确认
- **文档化的实践**: 适当记录实现内容

#### 团队实践准备
- **构建共同理解**: 与团队成员的认识统一
- **工具统一**: 在相同开发环境中工作
- **知识共享**: 成功・失败案例的共享

## 环境构建的完成确认

### 最终检查列表

- [ ] **基本工具**
  - [ ] Claude Sonnet 4 (Claude Code) 可利用
  - [ ] VS Code 已适当设置
  - [ ] Git仓库已初始化

- [ ] **项目结构**
  - [ ] 推荐目录结构已创建
  - [ ] todo.md 文件已准备
  - [ ] 基本设置文件已配置

- [ ] **开发工作流**
  - [ ] Git分支策略已决定
  - [ ] 提交规则已定义
  - [ ] 测试环境已确认运行

- [ ] **文档化・监控**
  - [ ] README.md 已创建
  - [ ] 日志设置已完成
  - [ ] 调试环境已准备

### 运行确认测试

```bash
# 基本运行确认
npm test                     # 测试执行
npm run test:coverage        # 覆盖率确认
git status                   # Git状态确认
git log --oneline -5         # 最近的提交确认

# Claude Code连接确认
# 用VS Code打开项目
# 确认Claude Code插件正常运作
# 向AI请求创建简单测试用例并确认运行
```

### 故障排除

#### 常见问题和解决方法

**无法连接Claude Code**
```bash
# 认证确认
# Pro计划有效性确认
# 网络设置确认
```

**测试环境发生错误**
```bash
# 重新安装依赖关系
npm install

# 清除包缓存
npm cache clean --force
```

**Git操作错误**
```bash
# 重新设置认证信息
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## 总结

在第2章中，我们学习了实践AITDD的综合性开发环境构建方法。重要要点如下：

### 主要成果
1. **工具设置**: 以Claude Sonnet 4为中心的开发环境构建
2. **工作流设计**: 从TODO管理到Git流程的系统化过程
3. **质量管理基础**: 测试环境、日志、调试功能的整备

### 向下一章的准备
环境构建完成后，终于要学习AITDD的实际过程。在第3章"AITDD过程详细"中，将掌握Red-Green-Refactor-Validation循环的具体实践方法。

**学习要点:**
- 各阶段的具体工作内容
- 与AI的有效对话方法
- 质量管理和审查技术

环境已整备，现在已准备好实际与AI协作进行软件开发。在下一章体验AITDD的真正价值吧。