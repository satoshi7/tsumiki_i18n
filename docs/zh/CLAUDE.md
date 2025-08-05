# CLAUDE.md

此文件为Claude Code (claude.ai/code) 在处理此存储库中的代码时提供指导。

## 概述

Tsumiki是一个为AI驱动开发框架提供命令模板的CLI工具。此项目是一个使用TypeScript + React和Ink构建的CLI应用程序，将Claude Code命令模板安装到用户的`.claude/commands/`目录中。

## 开发命令

```bash
# 开发环境
pnpm install                # 安装依赖项

# 构建
pnpm build                  # 构建项目并将commands目录复制到dist/
pnpm build:run              # 构建后运行CLI（用于测试）

# 代码质量
pnpm check                  # 使用Biome进行代码检查
pnpm fix                    # 使用Biome自动修复
pnpm typecheck              # TypeScript类型检查（使用tsgo）
pnpm secretlint             # 检查敏感信息

# pre-commit钩子
pnpm prepare                # 设置simple-git-hooks
```

## 项目结构

- **`src/cli.ts`**: CLI入口点，使用commander定义命令
- **`src/commands/install.tsx`**: 使用React + Ink实现的安装命令UI
- **`commands/`**: Tsumiki的AI开发框架Claude Code命令模板（`.md`和`.sh`文件）
- **`dist/`**: 构建输出，模板被复制到`dist/commands/`

## 技术栈

- **CLI框架**: Commander.js
- **UI框架**: React + Ink（CLI中的React渲染）
- **构建工具**: tsup（基于TypeScript + ESBuild）
- **代码质量**: Biome（检查器和格式化器）
- **TypeScript**: tsgo（快速类型检查）
- **包管理器**: pnpm

## 构建过程

在构建期间（`pnpm build`），执行以下过程：
1. 清理`dist`目录
2. 创建`dist/commands`目录
3. 将`commands/`中的`.md`和`.sh`文件复制到`dist/commands/`
4. 使用tsup以ESM和CJS两种格式构建TypeScript代码

## 安装行为

`tsumiki install`命令执行以下操作：
1. 在当前目录中创建`.claude/commands/`目录
2. 从构建的`dist/commands/`复制所有`.md`和`.sh`文件
3. 使用React + Ink显示进度和文件列表

## 质量管理

Pre-commit钩子自动执行以下操作：
- `pnpm secretlint`: 检查敏感信息
- `pnpm typecheck`: 类型检查
- `pnpm fix`: 自动代码修复

在提交代码更改之前，始终运行`pnpm check`和`pnpm typecheck`。