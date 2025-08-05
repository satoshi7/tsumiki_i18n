# Tsumiki - AI驱动开发框架

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

🌐 语言: [English](../en/README.md) | [日本語](../ja/README.md) | **中文** | [한국어](../ko/README.md)

Tsumiki是一个AI驱动的开发框架，利用AI提供从需求定义到实现的高效开发流程。

## 安装

要使用Tsumiki，请使用以下npx命令安装：

```bash
npx tsumiki install
```

此命令会将Tsumiki的Claude Code斜线命令安装到`.claude/commands/`。

## 概述

Tsumiki由两个命令集组成：

- **kairo** - 从需求到实现的综合开发流程
- **tdd** - 独立的测试驱动开发（TDD）执行

### Kairo命令

Kairo自动化并支持从需求定义到实现的开发过程：

1. **需求定义** - 从概述生成详细的需求文档
2. **设计** - 自动生成技术设计文档
3. **任务分解** - 适当地划分和排序实现任务
4. **TDD实现** - 通过测试驱动开发实现高质量代码

## 可用命令

### Kairo命令（综合开发流程）
- `kairo-requirements` - 需求定义
- `kairo-design` - 设计文档生成
- `kairo-tasks` - 任务分解
- `kairo-implement` - 实现执行

### TDD命令（独立执行）
- `tdd-requirements` - TDD需求定义
- `tdd-testcases` - 测试用例创建
- `tdd-red` - 测试实现（红）
- `tdd-green` - 最小实现（绿）
- `tdd-refactor` - 重构
- `tdd-verify-complete` - TDD完成验证

### 逆向工程命令
- `rev-tasks` - 从现有代码生成任务列表
- `rev-design` - 从现有代码生成设计文档
- `rev-specs` - 从现有代码生成测试规范
- `rev-requirements` - 从现有代码生成需求文档

## 快速开始

### 综合开发流程

```bash
# 1. 需求定义
/kairo-requirements

# 2. 设计
/kairo-design

# 3. 任务分解
/kairo-tasks

# 4. 实现
/kairo-implement
```

### 独立TDD流程

```bash
/tdd-requirements
/tdd-testcases
/tdd-red
/tdd-green
/tdd-refactor
/tdd-verify-complete
```

### 逆向工程

```bash
# 1. 从现有代码分析任务结构
/rev-tasks

# 2. 生成设计文档（建议在任务分析后）
/rev-design

# 3. 生成测试规范（建议在设计文档后）
/rev-specs

# 4. 生成需求文档（建议在所有分析完成后）
/rev-requirements
```

### 开发环境清理

```bash
# 清理开发环境
/clear
```

## 详细文档

- [手册](./MANUAL.md) - 详细使用方法、目录结构、工作流示例
- [贡献指南](./CONTRIBUTING.md) - 如何为项目做贡献
- [Claude.md](./CLAUDE.md) - Claude Code使用指南

## 许可证

MIT许可证 - 详见 [LICENSE](../../LICENSE)。

## 社区

- [GitHub Issues](https://github.com/classmethod/tsumiki/issues)
- [讨论](https://github.com/classmethod/tsumiki/discussions)