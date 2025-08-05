# 4.3 API Development Practice

## Learning Objectives

In this chapter, you will learn how to apply AITDD to actual web application development through RESTful API development:

- Utilizing AITDD in implementations with external dependencies
- Implementing asynchronous processing and error handling
- HTTP status codes and response design
- Automatic API documentation generation
- Development experience close to actual production environments

## Project Overview: Task Management API

We will build a RESTful API using Express.js based on the task management system developed in the previous chapter.

### API Specification Overview

```http
GET    /api/tasks       # Get all tasks
GET    /api/tasks/:id   # Get specific task
POST   /api/tasks       # Create new task
PUT    /api/tasks/:id   # Update task
DELETE /api/tasks/:id   # Delete task
```

### Technology Stack

- **Web Framework**: Express.js
- **Language**: TypeScript
- **Testing**: Jest + Supertest
- **Validation**: express-validator
- **Documentation**: OpenAPI (Swagger)

## New Technical Complexities

API development adds the following elements beyond the CRUD from the previous chapter:

**HTTP-related**:
- Request/response processing
- Status code management
- Header processing
- Routing design

**Asynchronous Processing**:
- Promise/async-await
- Error handling
- Timeout processing

**Validation**:
- Request data validation
- Response format unification
- Error response standardization

## Hands-on Practice

### Step 1: TODO Creation and API Design

Even in API development with AITDD, the **limit of about 3 feature integrations** applies similarly. Therefore, we divide endpoints appropriately.

```markdown
# TODO: Task Management API Implementation

## Phase 1: Foundation Building
- [ ] Express.js project setup
- [ ] TypeScript configuration
- [ ] Basic middleware configuration
- [ ] Error handling middleware

## Phase 2: Basic API (3 endpoints)
- [ ] GET /api/tasks - Get all tasks
- [ ] GET /api/tasks/:id - Get single task
- [ ] POST /api/tasks - Create task

## Phase 3: Extended API (remaining endpoints)
- [ ] PUT /api/tasks/:id - Update task
- [ ] DELETE /api/tasks/:id - Delete task
- [ ] GET /api/tasks?search=xxx - Conditional search

## Phase 4: Quality Improvement
- [ ] OpenAPI specification generation
- [ ] Input validation enhancement
- [ ] Security headers addition
- [ ] Performance optimization
```

### Step 2: Phase 1 - Foundation Building

#### Specification Creation

**AI Prompt**:
```
Please create specifications for the foundation setup of an API project using Express.js and TypeScript.

Requirements:
- Express.js setup in TypeScript environment
- Basic middleware (CORS, JSON parsing, etc.)
- Structured error handling
- Development/production configuration separation
- Test environment setup

Please refer to the following TODO:
[Paste Phase 1 TODO]
```

**Generated Specification Example**:
````markdown
# Task Management API Foundation Specification

## Project Structure
```
src/
├── app.ts              # Express application configuration
├── server.ts           # Server startup entry point
├── routes/             # Route definitions
├── controllers/        # Business logic
├── middleware/         # Custom middleware
├── types/              # TypeScript type definitions
└── tests/              # Test files
```

## Express Configuration
- CORS support
- JSON/URL-encoded parsing
- Request logging
- Security headers
- Error handling middleware

## Error Processing Standardization
```typescript
interface APIError {
  message: string;
  code: string;
  statusCode: number;
  details?: any;
}
```

## Response Format Unification
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

#### Test Case Creation and Red-Green-Refactor-Validation

**API Foundation Tests**:
```typescript
describe('API Foundation Tests', () => {
  test('Server startup confirmation', async () => {
    const response = await request(app).get('/health');
    expect(response.status).toBe(200);
    expect(response.body.success).toBe(true);
  });

  test('CORS configuration confirmation', async () => {
    const response = await request(app)
      .options('/api/tasks')
      .set('Origin', 'http://localhost:3000')
      .set('Access-Control-Request-Method', 'GET');
    
    expect(response.headers['access-control-allow-origin']).toBeDefined();
  });

  test('JSON parsing confirmation', async () => {
    const response = await request(app)
      .post('/api/test')
      .send({ test: 'data' })
      .set('Content-Type', 'application/json');
    
    expect(response.status).not.toBe(400); // Not a JSON parsing error
  });

  test('Error handling confirmation', async () => {
    const response = await request(app).get('/api/nonexistent');
    expect(response.status).toBe(404);
    expect(response.body.success).toBe(false);
    expect(response.body.error).toBeDefined();
  });
});
````

### Step 3: Phase 2 - Basic API Implementation

#### GET /api/tasks Implementation

**Specification**:
```markdown
## Get All Tasks API

### Endpoint
GET /api/tasks

### Response (Success)
```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "title": "Task Title",
      "description": "Task Description",
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

### Status Codes
- 200: Normal retrieval (including empty array)
- 500: Server error


**AI Prompt Example**:
```
Please implement the GET /api/tasks endpoint based on the following specification:

[Paste specification]

Requirements:
1. Use Express.js router
2. Utilize the TaskManager class created in the previous chapter
3. Implement error handling appropriately
4. Ensure TypeScript type safety
5. Create integration tests using Supertest

Existing code:
[Paste TaskManager class code]
[Paste API foundation code]
```

**Expected Implementation**:
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

#### GET /api/tasks/:id Implementation

**Additional Specification**:
```markdown
## Get Single Task API

### Endpoint
GET /api/tasks/:id

### Parameters
- id: Task ID (UUID format)

### Status Codes
- 200: Normal retrieval
- 400: Invalid ID format
- 404: Task not found
- 500: Server error
```

#### POST /api/tasks Implementation

**Additional Specification**:
````markdown
## Create Task API

### Endpoint
POST /api/tasks

### Request Body
```json
{
  "title": "Task Title",
  "description": "Task Description"
}
```

### Validation
- title: Required, 1-100 characters
- description: Optional, 0-500 characters

### Status Codes
- 201: Creation success
- 400: Validation error
- 500: Server error
````

### Step 4: Phase 3 - Extended API Implementation

#### Input Validation Enhancement

**Using express-validator**:

```typescript
// middleware/validation.ts
import { body, param, validationResult } from 'express-validator';
import { Request, Response, NextFunction } from 'express';

export const createTaskValidation = [
  body('title')
    .notEmpty()
    .withMessage('Title is required')
    .isLength({ min: 1, max: 100 })
    .withMessage('Title must be 1-100 characters'),
  
  body('description')
    .optional()
    .isLength({ max: 500 })
    .withMessage('Description must be 500 characters or less'),
];

export const taskIdValidation = [
  param('id')
    .isUUID()
    .withMessage('Please specify a valid UUID format ID'),
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
        message: 'Validation error',
        code: 'VALIDATION_ERROR',
        statusCode: 400,
        details: errors.array()
      }
    });
  }
  next();
};
```

#### PUT /api/tasks/:id Implementation

**Partial Update Support**:
```typescript
export const updateTaskValidation = [
  ...taskIdValidation,
  body('title')
    .optional()
    .isLength({ min: 1, max: 100 })
    .withMessage('Title must be 1-100 characters'),
  
  body('description')
    .optional()
    .isLength({ max: 500 })
    .withMessage('Description must be 500 characters or less'),
  
  body('completed')
    .optional()
    .isBoolean()
    .withMessage('completed must be a boolean value'),
];
```

#### DELETE /api/tasks/:id Implementation

**Soft Delete Consideration**:
```typescript
deleteTask = async (req: Request, res: Response, next: NextFunction) => {
  try {
    const { id } = req.params;
    const deleted = this.taskManager.deleteTask(id);
    
    if (!deleted) {
      return res.status(404).json({
        success: false,
        error: {
          message: 'Task not found',
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

### Step 5: Phase 4 - Quality Improvement

#### OpenAPI Specification Generation

**AI Prompt Example**:
```
Please generate an OpenAPI 3.0 specification from the following API endpoints:

[List of implemented endpoints]
[Response format definitions]
[Error response definitions]

Requirements:
1. Format displayable in Swagger UI
2. Detailed documentation for all endpoints
3. Include request/response examples
4. Include error code descriptions
```

**Generated OpenAPI Specification Example**:
```yaml
openapi: 3.0.0
info:
  title: Task Management API
  version: 1.0.0
  description: RESTful API for a simple task management system

paths:
  /api/tasks:
    get:
      summary: Get all tasks
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskListResponse'
    
    post:
      summary: Create task
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateTaskRequest'
      responses:
        '201':
          description: Creation success
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskResponse'
        '400':
          description: Validation error
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

## Handling Complex Problems

### Deciding When to Switch to Manual Implementation

In API development, consider manual implementation in the following cases:

**When you can't visualize the implementation**:
- "Complex authentication logic in custom middleware"
- "Integration processing with WebSocket"
- "Complex database optimization"

**When performance optimization is needed**:
- "Processing optimization for large request volumes"
- "Memory usage optimization"
- "Response time reduction"

### Using AI in Manual Implementation

Rather than completely manual, utilize AI in the following ways:
```typescript
// Request AI for parts you can visualize
const generateResponseHelper = (data: any, meta: any) => {
  // This part can be generated by AI
  return {
    success: true,
    data,
    meta: {
      timestamp: new Date().toISOString(),
      ...meta
    }
  };
};

// Implement complex logic manually
const complexAuthMiddleware = (req, res, next) => {
  // Implement complex authentication logic manually
  // However, utilize AI assistance partially
};
```

## API Development-Specific Test Strategy

### Integration Test Patterns

**End-to-End Tests**:
```typescript
describe('API Integration Tests', () => {
  test('Task management flow', async () => {
    // 1. Create task
    const createResponse = await request(app)
      .post('/api/tasks')
      .send({
        title: 'Test Task',
        description: 'This is a test task'
      });
    
    expect(createResponse.status).toBe(201);
    const taskId = createResponse.body.data.id;

    // 2. Confirm task retrieval
    const getResponse = await request(app)
      .get(`/api/tasks/${taskId}`);
    
    expect(getResponse.status).toBe(200);
    expect(getResponse.body.data.title).toBe('Test Task');

    // 3. Update task
    const updateResponse = await request(app)
      .put(`/api/tasks/${taskId}`)
      .send({
        completed: true
      });
    
    expect(updateResponse.status).toBe(200);
    expect(updateResponse.body.data.completed).toBe(true);

    // 4. Delete task
    const deleteResponse = await request(app)
      .delete(`/api/tasks/${taskId}`);
    
    expect(deleteResponse.status).toBe(204);

    // 5. Confirm deletion
    const getAfterDeleteResponse = await request(app)
      .get(`/api/tasks/${taskId}`);
    
    expect(getAfterDeleteResponse.status).toBe(404);
  });
});
```

### Performance Testing

**Load Testing**:
```typescript
describe('Performance Tests', () => {
  test('Concurrent request processing', async () => {
    const requests = Array.from({ length: 100 }, (_, i) =>
      request(app)
        .post('/api/tasks')
        .send({
          title: `Concurrent Task ${i}`,
          description: 'Concurrent processing test'
        })
    );

    const startTime = Date.now();
    const responses = await Promise.all(requests);
    const endTime = Date.now();

    responses.forEach(response => {
      expect(response.status).toBe(201);
    });

    expect(endTime - startTime).toBeLessThan(5000); // Within 5 seconds
  });
});
```

## Error Handling Best Practices

### Comprehensive Error Processing

```typescript
// middleware/errorHandler.ts
export const errorHandler = (
  error: any,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  // Log output
  console.error('API Error:', {
    error: error.message,
    stack: error.stack,
    method: req.method,
    url: req.url,
    body: req.body,
    timestamp: new Date().toISOString()
  });

  // Process by error type
  if (error.name === 'TaskNotFoundError') {
    return res.status(404).json({
      success: false,
      error: {
        message: 'Task not found',
        code: 'TASK_NOT_FOUND',
        statusCode: 404
      }
    });
  }

  if (error.name === 'ValidationError') {
    return res.status(400).json({
      success: false,
      error: {
        message: 'Validation error',
        code: 'VALIDATION_ERROR',
        statusCode: 400,
        details: error.details
      }
    });
  }

  // Unknown error
  res.status(500).json({
    success: false,
    error: {
      message: 'Internal server error occurred',
      code: 'INTERNAL_SERVER_ERROR',
      statusCode: 500
    }
  });
};
```

## Learning Effects in Practice

### AITDD Effects in API Development

**Development Speed Experience**:
- Traditional API development: Several days to 1 week
- Using AITDD: Several hours
- Achieving **significant efficiency improvement**

**Quality Stability**:
- Quality assurance through comprehensive testing
- Standardization of error handling
- Automatic generation of API documentation

**Acquisition of Practical Skills**:
- Effective AI utilization in web development
- Handling complex integration processing
- Production-quality implementation

### Differences from Traditional Development

**Design Phase**:
- Traditional: Spend time on detailed design
- AITDD: Design progressively in cooperation with AI

**Implementation Phase**:
- Traditional: Manual detailed implementation
- AITDD: Quality management of AI-generated code

**Testing Phase**:
- Traditional: Test creation after implementation
- AITDD: Test-first approach

## Preparation for the Next Chapter

Through this API development experience, you will acquire:

1. **AI utilization techniques in web development**
2. **Methods for managing complex integration processing**
3. **Production-quality implementation techniques**
4. **Error handling and debugging techniques**

In the next chapter, we will learn specific countermeasures for errors and troubles that occur during these implementation processes.

## Summary

Through API development, we acquired the following:

**Technical Growth**:
- Practical RESTful API design
- Asynchronous processing and error handling
- Type-safe implementation with TypeScript
- Comprehensive test strategy

**AITDD Utilization Techniques**:
- AI collaboration in complex implementations
- Management of gradual feature additions
- Automation of quality management
- Utilization of documentation generation

**Practical Development Skills**:
- Production-quality implementation
- Performance-conscious design
- Security-conscious implementation
- Operation-conscious design

These experiences establish a foundation for effectively utilizing AITDD in actual product development.