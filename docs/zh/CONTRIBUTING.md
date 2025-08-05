# 贡献指南

感谢您对Tsumiki项目的贡献！本指南将说明如何为项目做出贡献。

## 开发环境设置

### 必需环境

- Node.js 18.0.0或更高版本
- pnpm 10.13.1或更高版本

### 设置步骤

1. Fork并克隆仓库：

```bash
git clone https://github.com/YOUR_USERNAME/tsumiki.git
cd tsumiki
```

2. 安装依赖项：

```bash
pnpm install
```

3. 设置pre-commit钩子：

```bash
pnpm prepare
```

## 开发工作流程

### 分支策略

- `main`分支：稳定版代码
- 功能开发：`feature/功能名称`
- 错误修复：`bugfix/错误名称`
- 热修复：`hotfix/修复内容`

### 开发流程

1. 创建新分支：

```bash
git checkout -b feature/your-feature-name
```

2. 进行代码更改

3. 运行代码质量检查：

```bash
# 类型检查
pnpm typecheck

# 代码检查
pnpm check

# 自动修复
pnpm fix

# 机密信息检查
pnpm secretlint
```

4. 运行构建测试：

```bash
pnpm build:run
```

5. 提交更改：

```bash
git add .
git commit -m "feat: 添加新功能"
```

## 提交信息约定

请使用[Conventional Commits](https://www.conventionalcommits.org/)格式：

- `feat:` 新功能
- `fix:` 错误修复
- `docs:` 文档更改
- `style:` 代码样式更改（不影响功能）
- `refactor:` 重构
- `test:` 测试添加·修改
- `chore:` 构建过程或工具更改

示例：
```
feat: add new install command for .sh files
fix: resolve path handling issue in install command
docs: update README with new command examples
```

## 代码质量标准

### 自动检查

Pre-commit钩子会自动运行以下检查：

- **secretlint**: 机密信息（API密钥、密码等）混入检查
- **typecheck**: TypeScript类型检查
- **fix**: Biome自动代码修复

### 手动检查

更改前请运行以下命令：

```bash
# 运行所有检查
pnpm typecheck && pnpm check && pnpm secretlint

# 代码自动修复
pnpm fix
```

## 项目结构

```
tsumiki/
├── src/
│   ├── cli.ts              # CLI入口点
│   └── commands/
│       └── install.tsx     # 安装命令UI实现
├── commands/               # 命令模板（.md, .sh）
├── dist/                  # 构建输出
├── package.json
├── CLAUDE.md              # 项目指导文档
└── README.md
```

## 拉取请求

### 创建拉取请求

1. 推送更改：

```bash
git push origin feature/your-feature-name
```

2. 在GitHub上创建拉取请求

3. 按照拉取请求模板填写说明

### 拉取请求要求

- [ ] 更改内容已清楚说明
- [ ] 已链接相关Issue（如适用）
- [ ] 代码质量检查通过
- [ ] 构建成功
- [ ] 不包含机密信息

## Issue报告

错误报告和功能请求在[Issues](https://github.com/classmethod/tsumiki/issues)中接受。

### 错误报告

请包含以下信息：

- 重现步骤
- 预期行为
- 实际行为
- 环境信息（操作系统、Node.js版本等）
- 错误消息（如适用）

### 功能请求

请包含以下信息：

- 建议功能的说明
- 使用场景
- 预期收益
- 实现方案（如有）

## 安全

如果发现安全相关问题，请私下报告，而不是通过公开Issue。

## 许可证

本项目在MIT许可证下发布。贡献时，您同意此许可证。

## 问题与支持

- [Issues](https://github.com/classmethod/tsumiki/issues) - 错误报告、功能请求
- [Discussions](https://github.com/classmethod/tsumiki/discussions) - 问题、讨论

我们期待您的贡献！