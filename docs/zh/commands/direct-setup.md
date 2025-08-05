# direct-setup

## 目的

执行DIRECT任务的设置作业。基于设计文档进行环境构建、设置文件创建、依赖关系安装等。

## 前提条件

- 已提供任务ID
- 存在相关设计文档
- 已准备必要的权限和环境

## 执行内容

1. **设计文档确认**
   - 确认 `docs/design/{要求名}/architecture.md`
   - 确认 `docs/design/{要求名}/database-schema.sql`
   - 确认其他相关设计文档

2. **设置作业的执行**
   - 环境变量设置
   - 设置文件的创建・更新
   - 依赖关系的安装
   - 数据库的初始化
   - 服务的启动设置
   - 权限设置

3. **作业记录的创建**
   - 执行命令的记录
   - 更改设置的记录
   - 遇到问题和解决方法的记录

## 输出目标

作业记录在 `docs/implements/{TASK-ID}/` 目录中作为以下文件创建：
- `setup-report.md`: 设置作业执行记录

## 输出格式示例

````markdown
# {TASK-ID} 设置作业执行

## 作业概要

- **任务ID**: {TASK-ID}
- **作业内容**: {设置作业概要}
- **执行日期**: {执行日期}
- **执行者**: {执行者}

## 设计文档参照

- **参照文档**: {参照的设计文档列表}
- **相关要求**: {REQ-XXX, REQ-YYY}

## 执行的作业

### 1. 环境变量的设置

```bash
# 执行的命令
export NODE_ENV=development
export DATABASE_URL=postgresql://localhost:5432/mydb
```
````

**设置内容**:

- NODE_ENV: 设置为开发环境
- DATABASE_URL: PostgreSQL数据库的URL

### 2. 设置文件的创建

**创建文件**: `config/database.json`

```json
{
  "development": {
    "host": "localhost",
    "port": 5432,
    "database": "mydb"
  }
}
```

### 3. 依赖关系的安装

```bash
# 执行的命令
npm install express pg
```

**安装内容**:

- express: Web框架
- pg: PostgreSQL客户端

### 4. 数据库的初始化

```bash
# 执行的命令
createdb mydb
psql -d mydb -f database-schema.sql
```

**执行内容**:

- 数据库创建
- 模式的应用

## 作业结果

- [ ] 环境变量设置完成
- [ ] 设置文件创建完成
- [ ] 依赖关系安装完成
- [ ] 数据库初始化完成
- [ ] 服务启动设置完成

## 遇到的问题和解决方法

### 问题1: {问题概要}

- **发生状况**: {问题发生的状况}
- **错误消息**: {错误消息}
- **解决方法**: {解决方法}

## 下一步骤

- 执行 `direct-verify.md` 确认设置
- 根据需要进行设置调整

```

## 执行后确认
- 确认 `docs/implements/{TASK-ID}/setup-report.md` 文件已创建
- 确认设置已正确应用
- 确认下一步骤（direct-verify）的准备已完成

## 目录创建

执行前请创建必要的目录：
```bash
mkdir -p docs/implements/{TASK-ID}
```
```