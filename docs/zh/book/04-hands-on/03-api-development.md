# 4.3 API 开发实践

## 学习目标

在本章中，您将通过 RESTful API 的开发学习如何将 AITDD 应用于实际的 Web 应用程序开发：

- 在包含外部依赖的实现中利用 AITDD
- 实现异步处理和错误处理
- HTTP 状态码和响应设计
- 自动生成 API 文档
- 接近实际生产环境的开发体验

## 项目概述：任务管理 API

基于前一章开发的任务管理系统，我们将使用 Express.js 构建 RESTful API。

### API 规范概述

```http
GET    /api/tasks       # 获取所有任务
GET    /api/tasks/:id   # 获取特定任务
POST   /api/tasks       # 创建新任务
PUT    /api/tasks/:id   # 更新任务
DELETE /api/tasks/:id   # 删除任务
```

### 技术栈

- **Web 框架**: Express.js
- **语言**: TypeScript
- **测试**: Jest + Supertest
- **验证**: express-validator
- **文档**: OpenAPI (Swagger)

## 新的技术复杂性

API 开发在前一章的 CRUD 基础上增加了以下要素：

**HTTP 相关**：
- 请求/响应处理
- 状态码管理
- 头部处理
- 路由设计

**异步处理**：
- Promise/async-await
- 错误处理
- 超时处理

**验证**：
- 请求数据验证
- 响应格式统一
- 错误响应标准化

## 动手实践

### 第1步：TODO 创建和 API 设计

在使用 AITDD 进行 API 开发时，**约3个功能的集成限制**同样适用。因此，我们适当地划分端点。

```markdown
# TODO: 任务管理 API 实现

## 第1阶段：基础构建
- [ ] Express.js 项目设置
- [ ] TypeScript 配置
- [ ] 基本中间件配置
- [ ] 错误处理中间件

## 第2阶段：基本 API（3个端点）
- [ ] GET /api/tasks - 获取所有任务
- [ ] GET /api/tasks/:id - 获取单个任务
- [ ] POST /api/tasks - 创建任务

## 第3阶段：扩展 API（剩余端点）
- [ ] PUT /api/tasks/:id - 更新任务
- [ ] DELETE /api/tasks/:id - 删除任务
- [ ] GET /api/tasks?search=xxx - 条件搜索

## 第4阶段：质量提升
- [ ] OpenAPI 规范生成
- [ ] 输入验证增强
- [ ] 安全头部添加
- [ ] 性能优化
```

### 第2步：第1阶段 - 基础构建

#### 规范创建

**AI 提示**：
```
请创建使用 Express.js 和 TypeScript 的 API 项目基础设置的规范。

要求：
- TypeScript 环境中的 Express.js 设置
- 基本中间件（CORS、JSON 解析等）
- 结构化的错误处理
- 开发/生产环境配置分离
- 测试环境设置

请参考以下 TODO：
[粘贴第1阶段 TODO]
```

**生成的规范示例**：
````markdown
# 任务管理 API 基础规范

## 项目结构
```
src/
├── app.ts              # Express 应用程序配置
├── server.ts           # 服务器启动入口点
├── routes/             # 路由定义
├── controllers/        # 业务逻辑
├── middleware/         # 自定义中间件
├── types/              # TypeScript 类型定义
└── tests/              # 测试文件
```

## Express 配置
- CORS 支持
- JSON/URL 编码解析
- 请求日志
- 安全头部
- 错误处理中间件

## 错误处理标准化
```typescript
interface APIError {
  message: string;
  code: string;
  statusCode: number;
  details?: any;
}
```

## 响应格式统一
```typescript
interface APIResponse<T> {
  success: boolean;
  data?: T;
  error?: APIError;
  meta?: {
    timestamp: string;
    requestId: string;
  };
}
```
```

#### 测试用例创建和 Red-Green-Refactor-Validation

**API 基础测试**：
```typescript
describe('API 基础测试', () => {
  test('服务器启动确认', async () => {
    const response = await request(app).get('/health');
    expect(response.status).toBe(200);
    expect(response.body.success).toBe(true);
  });

  test('CORS 配置确认', async () => {
    const response = await request(app)
      .options('/api/tasks')
      .set('Origin', 'http://localhost:3000')
      .set('Access-Control-Request-Method', 'GET');
    
    expect(response.headers['access-control-allow-origin']).toBeDefined();
  });

  test('JSON 解析确认', async () => {
    const response = await request(app)
      .post('/api/test')
      .send({ test: 'data' })
      .set('Content-Type', 'application/json');
    
    expect(response.status).not.toBe(400); // 不是 JSON 解析错误
  });

  test('错误处理确认', async () => {
    const response = await request(app).get('/api/nonexistent');
    expect(response.status).toBe(404);
    expect(response.body.success).toBe(false);
    expect(response.body.error).toBeDefined();
  });
});
````

### 第3步：第2阶段 - 基本 API 实现

#### GET /api/tasks 的实现

**规范**：
```markdown
## 获取所有任务 API

### 端点
GET /api/tasks

### 响应（成功时）
```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "title": "任务标题",
      "description": "任务描述",
      "completed": false,
      "createdAt": "2025-06-21T10:00:00Z",
      "updatedAt": "2025-06-21T10:00:00Z"
    }
  ],
  "meta": {
    "timestamp": "2025-06-21T10:00:00Z",
    "requestId": "req-123"
  }
}
```

### 状态码
- 200: 正常获取（包括空数组）
- 500: 服务器错误


**AI 提示示例**：
```
请基于以下规范实现 GET /api/tasks 端点：

[粘贴规范]

要求：
1. 使用 Express.js 路由器
2. 利用前一章创建的 TaskManager 类
3. 适当实现错误处理
4. 确保 TypeScript 类型安全
5. 使用 Supertest 创建集成测试

现有代码：
[粘贴 TaskManager 类代码]
[粘贴 API 基础代码]
```

**预期实现**：
```typescript
// routes/tasks.ts
import { Router } from 'express';
import { TaskController } from '../controllers/TaskController';

const router = Router();
const taskController = new TaskController();

router.get('/', taskController.getAllTasks);

export default router;

// controllers/TaskController.ts
import { Request, Response, NextFunction } from 'express';
import { TaskManager } from '../services/TaskManager';
import { APIResponse } from '../types/api';

export class TaskController {
  private taskManager = new TaskManager();

  getAllTasks = async (req: Request, res: Response, next: NextFunction) => {
    try {
      const tasks = this.taskManager.getAllTasks();
      
      const response: APIResponse<typeof tasks> = {
        success: true,
        data: tasks,
        meta: {
          timestamp: new Date().toISOString(),
          requestId: req.headers['x-request-id'] as string || 'unknown'
        }
      };

      res.status(200).json(response);
    } catch (error) {
      next(error);
    }
  };
}
```

#### GET /api/tasks/:id 的实现

**追加规范**：
```markdown
## 获取单个任务 API

### 端点
GET /api/tasks/:id

### 参数
- id: 任务 ID（UUID 格式）

### 状态码
- 200: 正常获取
- 400: 无效的 ID 格式
- 404: 找不到任务
- 500: 服务器错误
```

#### POST /api/tasks 的实现

**追加规范**：
````markdown
## 创建任务 API

### 端点
POST /api/tasks

### 请求体
```json
{
  "title": "任务标题",
  "description": "任务描述"
}
```

### 验证
- title: 必填，1-100个字符
- description: 可选，0-500个字符

### 状态码
- 201: 创建成功
- 400: 验证错误
- 500: 服务器错误
````

### 第4步：第3阶段 - 扩展 API 实现

#### 输入验证增强

**使用 express-validator**：

```typescript
// middleware/validation.ts
import { body, param, validationResult } from 'express-validator';
import { Request, Response, NextFunction } from 'express';

export const createTaskValidation = [
  body('title')
    .notEmpty()
    .withMessage('标题是必填项')
    .isLength({ min: 1, max: 100 })
    .withMessage('标题必须为 1-100 个字符'),
  
  body('description')
    .optional()
    .isLength({ max: 500 })
    .withMessage('描述必须为 500 个字符或更少'),
];

export const taskIdValidation = [
  param('id')
    .isUUID()
    .withMessage('请指定有效的 UUID 格式 ID'),
];

export const handleValidationErrors = (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({
      success: false,
      error: {
        message: '验证错误',
        code: 'VALIDATION_ERROR',
        statusCode: 400,
        details: errors.array()
      }
    });
  }
  next();
};
```

#### PUT /api/tasks/:id 实现

**部分更新支持**：
```typescript
export const updateTaskValidation = [
  ...taskIdValidation,
  body('title')
    .optional()
    .isLength({ min: 1, max: 100 })
    .withMessage('标题必须为 1-100 个字符'),
  
  body('description')
    .optional()
    .isLength({ max: 500 })
    .withMessage('描述必须为 500 个字符或更少'),
  
  body('completed')
    .optional()
    .isBoolean()
    .withMessage('completed 必须是布尔值'),
];
```

#### DELETE /api/tasks/:id 实现

**软删除考虑**：
```typescript
deleteTask = async (req: Request, res: Response, next: NextFunction) => {
  try {
    const { id } = req.params;
    const deleted = this.taskManager.deleteTask(id);
    
    if (!deleted) {
      return res.status(404).json({
        success: false,
        error: {
          message: '找不到任务',
          code: 'TASK_NOT_FOUND',
          statusCode: 404
        }
      });
    }

    res.status(204).send(); // No Content
  } catch (error) {
    next(error);
  }
};
```

### 第5步：第4阶段 - 质量提升

#### OpenAPI 规范生成

**AI 提示示例**：
```
请从以下 API 端点生成 OpenAPI 3.0 规范：

[实现的端点列表]
[响应格式定义]
[错误响应定义]

要求：
1. 可在 Swagger UI 中显示的格式
2. 所有端点的详细文档
3. 包含请求/响应示例
4. 包含错误代码说明
```

**生成的 OpenAPI 规范示例**：
```yaml
openapi: 3.0.0
info:
  title: 任务管理 API
  version: 1.0.0
  description: 简单任务管理系统的 RESTful API

paths:
  /api/tasks:
    get:
      summary: 获取所有任务
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskListResponse'
    
    post:
      summary: 创建任务
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateTaskRequest'
      responses:
        '201':
          description: 创建成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskResponse'
        '400':
          description: 验证错误
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: string
          format: uuid
        title:
          type: string
          minLength: 1
          maxLength: 100
        description:
          type: string
          maxLength: 500
        completed:
          type: boolean
        createdAt:
          type: string
          format: date-time
        updatedAt:
          type: string
          format: date-time
```

## 处理复杂问题

### 决定切换到手动实现

在 API 开发中，在以下情况下考虑手动实现：

**当无法想象实现方法时**：
- "自定义中间件中的复杂认证逻辑"
- "与 WebSocket 的集成处理"
- "复杂的数据库优化"

**当需要性能优化时**：
- "大量请求的处理优化"
- "内存使用量优化"
- "响应时间缩短"

### 在手动实现中使用 AI

不是完全手动，而是以下方式利用 AI：
```typescript
// 可以想象的部分请 AI 实现
const generateResponseHelper = (data: any, meta: any) => {
  // 这部分可以由 AI 生成
  return {
    success: true,
    data,
    meta: {
      timestamp: new Date().toISOString(),
      ...meta
    }
  };
};

// 手动实现复杂逻辑
const complexAuthMiddleware = (req, res, next) => {
  // 手动实现复杂的认证逻辑
  // 但是，部分利用 AI 辅助
};
```

## API 开发特有的测试策略

### 集成测试模式

**端到端测试**：
```typescript
describe('API 集成测试', () => {
  test('任务管理流程', async () => {
    // 1. 创建任务
    const createResponse = await request(app)
      .post('/api/tasks')
      .send({
        title: '测试任务',
        description: '这是一个测试任务'
      });
    
    expect(createResponse.status).toBe(201);
    const taskId = createResponse.body.data.id;

    // 2. 确认任务获取
    const getResponse = await request(app)
      .get(`/api/tasks/${taskId}`);
    
    expect(getResponse.status).toBe(200);
    expect(getResponse.body.data.title).toBe('测试任务');

    // 3. 更新任务
    const updateResponse = await request(app)
      .put(`/api/tasks/${taskId}`)
      .send({
        completed: true
      });
    
    expect(updateResponse.status).toBe(200);
    expect(updateResponse.body.data.completed).toBe(true);

    // 4. 删除任务
    const deleteResponse = await request(app)
      .delete(`/api/tasks/${taskId}`);
    
    expect(deleteResponse.status).toBe(204);

    // 5. 确认删除
    const getAfterDeleteResponse = await request(app)
      .get(`/api/tasks/${taskId}`);
    
    expect(getAfterDeleteResponse.status).toBe(404);
  });
});
```

### 性能测试

**负载测试**：
```typescript
describe('性能测试', () => {
  test('并发请求处理', async () => {
    const requests = Array.from({ length: 100 }, (_, i) =>
      request(app)
        .post('/api/tasks')
        .send({
          title: `并发任务 ${i}`,
          description: '并发处理测试'
        })
    );

    const startTime = Date.now();
    const responses = await Promise.all(requests);
    const endTime = Date.now();

    responses.forEach(response => {
      expect(response.status).toBe(201);
    });

    expect(endTime - startTime).toBeLessThan(5000); // 5秒内
  });
});
```

## 错误处理最佳实践

### 全面的错误处理

```typescript
// middleware/errorHandler.ts
export const errorHandler = (
  error: any,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  // 日志输出
  console.error('API Error:', {
    error: error.message,
    stack: error.stack,
    method: req.method,
    url: req.url,
    body: req.body,
    timestamp: new Date().toISOString()
  });

  // 按错误类型处理
  if (error.name === 'TaskNotFoundError') {
    return res.status(404).json({
      success: false,
      error: {
        message: '找不到任务',
        code: 'TASK_NOT_FOUND',
        statusCode: 404
      }
    });
  }

  if (error.name === 'ValidationError') {
    return res.status(400).json({
      success: false,
      error: {
        message: '验证错误',
        code: 'VALIDATION_ERROR',
        statusCode: 400,
        details: error.details
      }
    });
  }

  // 未知错误
  res.status(500).json({
    success: false,
    error: {
      message: '发生内部服务器错误',
      code: 'INTERNAL_SERVER_ERROR',
      statusCode: 500
    }
  });
};
```

## 实践中的学习效果

### API 开发中的 AITDD 效果

**开发速度体验**：
- 传统 API 开发：几天到1周
- 使用 AITDD：几小时
- 实现**显著的效率提升**

**质量稳定性**：
- 通过全面测试保证质量
- 错误处理的标准化
- API 文档的自动生成

**实践技能的获得**：
- Web 开发中的有效 AI 利用
- 处理复杂集成处理
- 生产质量的实现

### 与传统开发的区别

**设计阶段**：
- 传统：花时间进行详细设计
- AITDD：与 AI 合作逐步设计

**实现阶段**：
- 传统：手动详细实现
- AITDD：AI 生成代码的质量管理

**测试阶段**：
- 传统：实现后创建测试
- AITDD：测试优先方法

## 为下一章做准备

通过这次 API 开发体验，您将获得：

1. **Web 开发中的 AI 利用技术**
2. **管理复杂集成处理的方法**
3. **生产质量的实现技术**
4. **错误处理和调试技术**

在下一章中，我们将学习在这些实现过程中发生的错误和故障的具体对策。

## 总结

通过 API 开发，我们获得了以下内容：

**技术成长**：
- RESTful API 设计实践
- 异步处理和错误处理
- 使用 TypeScript 的类型安全实现
- 全面的测试策略

**AITDD 利用技术**：
- 复杂实现中的 AI 协作
- 渐进式功能添加的管理
- 质量管理的自动化
- 文档生成的利用

**实践开发能力**：
- 生产质量的实现
- 考虑性能的设计
- 考虑安全的实现
- 考虑运营的设计

这些经验为在实际产品开发中有效利用 AITDD 奠定了基础。