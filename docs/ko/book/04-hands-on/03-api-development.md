# 4.3 API 개발 실습

## 학습 목표

이 장에서는 RESTful API 개발을 통해 AITDD의 실제 웹 애플리케이션 개발 적용 방법을 배웁니다:

- 외부 의존성을 포함한 구현에서의 AITDD 활용
- 비동기 처리와 에러 핸들링 구현
- HTTP 상태 코드와 응답 설계
- API 문서 자동 생성
- 실제 프로덕션 환경에 가까운 개발 경험

## 프로젝트 개요: 작업 관리 API

이전 장에서 개발한 작업 관리 시스템을 기반으로 Express.js를 사용한 RESTful API를 구축합니다.

### API 사양 개요

```http
GET    /api/tasks       # 모든 작업 가져오기
GET    /api/tasks/:id   # 특정 작업 가져오기
POST   /api/tasks       # 새 작업 생성
PUT    /api/tasks/:id   # 작업 업데이트
DELETE /api/tasks/:id   # 작업 삭제
```

### 기술 스택

- **웹 프레임워크**: Express.js
- **언어**: TypeScript
- **테스트**: Jest + Supertest
- **검증**: express-validator
- **문서**: OpenAPI (Swagger)

## 새로운 기술적 복잡성

API 개발은 이전 장의 CRUD에 더해 다음 요소들이 추가됩니다:

**HTTP 관련**:
- 요청/응답 처리
- 상태 코드 관리
- 헤더 처리
- 라우팅 설계

**비동기 처리**:
- Promise/async-await
- 에러 핸들링
- 타임아웃 처리

**검증**:
- 요청 데이터 검증
- 응답 형식 통일
- 에러 응답 표준화

## 실습

### 단계 1: TODO 작성과 API 설계

AITDD를 사용한 API 개발에서도 **3개 기능 정도의 통합 한계**는 동일하게 적용됩니다. 따라서 엔드포인트를 적절히 분할합니다.

```markdown
# TODO: 작업 관리 API 구현

## 1단계: 기반 구축
- [ ] Express.js 프로젝트 설정
- [ ] TypeScript 설정
- [ ] 기본 미들웨어 설정
- [ ] 에러 핸들링 미들웨어

## 2단계: 기본 API (3개 엔드포인트)
- [ ] GET /api/tasks - 모든 작업 가져오기
- [ ] GET /api/tasks/:id - 단일 작업 가져오기
- [ ] POST /api/tasks - 작업 생성

## 3단계: 확장 API (나머지 엔드포인트)
- [ ] PUT /api/tasks/:id - 작업 업데이트
- [ ] DELETE /api/tasks/:id - 작업 삭제
- [ ] GET /api/tasks?search=xxx - 조건 검색

## 4단계: 품질 향상
- [ ] OpenAPI 사양서 생성
- [ ] 입력 검증 강화
- [ ] 보안 헤더 추가
- [ ] 성능 최적화
```

### 단계 2: 1단계 - 기반 구축

#### 사양 작성

**AI 프롬프트**:
```
Express.js와 TypeScript를 사용한 API 프로젝트의 기반 설정 사양을 작성해주세요.

요구사항:
- TypeScript 환경에서의 Express.js 설정
- 기본 미들웨어(CORS, JSON 파싱 등)
- 구조화된 에러 핸들링
- 개발/프로덕션 환경 설정 분리
- 테스트 환경 설정

다음 TODO를 참고해주세요:
[1단계 TODO 붙여넣기]
```

**생성된 사양 예시**:
````markdown
# 작업 관리 API 기반 사양

## 프로젝트 구조
```
src/
├── app.ts              # Express 애플리케이션 설정
├── server.ts           # 서버 시작 진입점
├── routes/             # 라우트 정의
├── controllers/        # 비즈니스 로직
├── middleware/         # 커스텀 미들웨어
├── types/              # TypeScript 타입 정의
└── tests/              # 테스트 파일
```

## Express 설정
- CORS 지원
- JSON/URL 인코딩 파싱
- 요청 로깅
- 보안 헤더
- 에러 핸들링 미들웨어

## 에러 처리 표준화
```typescript
interface APIError {
  message: string;
  code: string;
  statusCode: number;
  details?: any;
}
```

## 응답 형식 통일
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

#### 테스트 케이스 작성과 Red-Green-Refactor-Validation

**API 기반 테스트**:
```typescript
describe('API 기반 테스트', () => {
  test('서버 시작 확인', async () => {
    const response = await request(app).get('/health');
    expect(response.status).toBe(200);
    expect(response.body.success).toBe(true);
  });

  test('CORS 설정 확인', async () => {
    const response = await request(app)
      .options('/api/tasks')
      .set('Origin', 'http://localhost:3000')
      .set('Access-Control-Request-Method', 'GET');
    
    expect(response.headers['access-control-allow-origin']).toBeDefined();
  });

  test('JSON 파싱 확인', async () => {
    const response = await request(app)
      .post('/api/test')
      .send({ test: 'data' })
      .set('Content-Type', 'application/json');
    
    expect(response.status).not.toBe(400); // JSON 파싱 에러가 아님
  });

  test('에러 핸들링 확인', async () => {
    const response = await request(app).get('/api/nonexistent');
    expect(response.status).toBe(404);
    expect(response.body.success).toBe(false);
    expect(response.body.error).toBeDefined();
  });
});
````

### 단계 3: 2단계 - 기본 API 구현

#### GET /api/tasks 구현

**사양**:
```markdown
## 모든 작업 가져오기 API

### 엔드포인트
GET /api/tasks

### 응답 (성공 시)
```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "title": "작업 제목",
      "description": "작업 설명",
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

### 상태 코드
- 200: 정상 가져오기(빈 배열 포함)
- 500: 서버 에러


**AI 프롬프트 예시**:
```
다음 사양에 기반하여 GET /api/tasks 엔드포인트를 구현해주세요:

[사양 붙여넣기]

요구사항:
1. Express.js 라우터 사용
2. 이전 장에서 생성한 TaskManager 클래스 활용
3. 에러 핸들링을 적절히 구현
4. TypeScript 타입 안전성 보장
5. Supertest를 사용한 통합 테스트 작성

기존 코드:
[TaskManager 클래스 코드 붙여넣기]
[API 기반 코드 붙여넣기]
```

**예상 구현**:
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

#### GET /api/tasks/:id 구현

**추가 사양**:
```markdown
## 단일 작업 가져오기 API

### 엔드포인트
GET /api/tasks/:id

### 매개변수
- id: 작업 ID(UUID 형식)

### 상태 코드
- 200: 정상 가져오기
- 400: 잘못된 ID 형식
- 404: 작업을 찾을 수 없음
- 500: 서버 에러
```

#### POST /api/tasks 구현

**추가 사양**:
````markdown
## 작업 생성 API

### 엔드포인트
POST /api/tasks

### 요청 본문
```json
{
  "title": "작업 제목",
  "description": "작업 설명"
}
```

### 검증
- title: 필수, 1-100자
- description: 선택, 0-500자

### 상태 코드
- 201: 생성 성공
- 400: 검증 에러
- 500: 서버 에러
````

### 단계 4: 3단계 - 확장 API 구현

#### 입력 검증 강화

**express-validator 활용**:

```typescript
// middleware/validation.ts
import { body, param, validationResult } from 'express-validator';
import { Request, Response, NextFunction } from 'express';

export const createTaskValidation = [
  body('title')
    .notEmpty()
    .withMessage('제목은 필수입니다')
    .isLength({ min: 1, max: 100 })
    .withMessage('제목은 1-100자여야 합니다'),
  
  body('description')
    .optional()
    .isLength({ max: 500 })
    .withMessage('설명은 500자 이하여야 합니다'),
];

export const taskIdValidation = [
  param('id')
    .isUUID()
    .withMessage('유효한 UUID 형식의 ID를 지정해주세요'),
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
        message: '검증 에러',
        code: 'VALIDATION_ERROR',
        statusCode: 400,
        details: errors.array()
      }
    });
  }
  next();
};
```

#### PUT /api/tasks/:id 구현

**부분 업데이트 지원**:
```typescript
export const updateTaskValidation = [
  ...taskIdValidation,
  body('title')
    .optional()
    .isLength({ min: 1, max: 100 })
    .withMessage('제목은 1-100자여야 합니다'),
  
  body('description')
    .optional()
    .isLength({ max: 500 })
    .withMessage('설명은 500자 이하여야 합니다'),
  
  body('completed')
    .optional()
    .isBoolean()
    .withMessage('completed는 불린값이어야 합니다'),
];
```

#### DELETE /api/tasks/:id 구현

**소프트 삭제 고려**:
```typescript
deleteTask = async (req: Request, res: Response, next: NextFunction) => {
  try {
    const { id } = req.params;
    const deleted = this.taskManager.deleteTask(id);
    
    if (!deleted) {
      return res.status(404).json({
        success: false,
        error: {
          message: '작업을 찾을 수 없습니다',
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

### 단계 5: 4단계 - 품질 향상

#### OpenAPI 사양서 생성

**AI 프롬프트 예시**:
```
다음 API 엔드포인트에서 OpenAPI 3.0 사양서를 생성해주세요:

[구현한 엔드포인트 목록]
[응답 형식 정의]
[에러 응답 정의]

요구사항:
1. Swagger UI에서 표시 가능한 형식
2. 모든 엔드포인트의 상세 문서
3. 요청/응답 예시 포함
4. 에러 코드 설명 포함
```

**생성된 OpenAPI 사양 예시**:
```yaml
openapi: 3.0.0
info:
  title: 작업 관리 API
  version: 1.0.0
  description: 간단한 작업 관리 시스템의 RESTful API

paths:
  /api/tasks:
    get:
      summary: 모든 작업 가져오기
      responses:
        '200':
          description: 성공
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskListResponse'
    
    post:
      summary: 작업 생성
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateTaskRequest'
      responses:
        '201':
          description: 생성 성공
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskResponse'
        '400':
          description: 검증 에러
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

## 복잡한 문제 대처법

### 수동 구현으로의 전환 판단

API 개발에서 다음의 경우 수동 구현을 고려합니다:

**구현 방법을 상상할 수 없는 경우**:
- "커스텀 미들웨어의 복잡한 인증 로직"
- "WebSocket과의 통합 처리"
- "복잡한 데이터베이스 최적화"

**성능 대응이 필요한 경우**:
- "대량 요청 처리 최적화"
- "메모리 사용량 최적화"
- "응답 시간 단축"

### 수동 구현 시의 AI 활용

완전 수동이 아닌 다음과 같이 AI를 활용:
```typescript
// 구현 이미지가 가능한 부분은 AI에 요청
const generateResponseHelper = (data: any, meta: any) => {
  // 이 부분은 AI로 생성 가능
  return {
    success: true,
    data,
    meta: {
      timestamp: new Date().toISOString(),
      ...meta
    }
  };
};

// 복잡한 로직은 수동 구현
const complexAuthMiddleware = (req, res, next) => {
  // 복잡한 인증 로직은 수동으로 구현
  // 단, 부분적으로 AI 보완 활용
};
```

## API 개발 특유의 테스트 전략

### 통합 테스트 패턴

**엔드투엔드 테스트**:
```typescript
describe('API 통합 테스트', () => {
  test('작업 관리 플로우', async () => {
    // 1. 작업 생성
    const createResponse = await request(app)
      .post('/api/tasks')
      .send({
        title: '테스트 작업',
        description: '테스트용 작업입니다'
      });
    
    expect(createResponse.status).toBe(201);
    const taskId = createResponse.body.data.id;

    // 2. 작업 가져오기 확인
    const getResponse = await request(app)
      .get(`/api/tasks/${taskId}`);
    
    expect(getResponse.status).toBe(200);
    expect(getResponse.body.data.title).toBe('테스트 작업');

    // 3. 작업 업데이트
    const updateResponse = await request(app)
      .put(`/api/tasks/${taskId}`)
      .send({
        completed: true
      });
    
    expect(updateResponse.status).toBe(200);
    expect(updateResponse.body.data.completed).toBe(true);

    // 4. 작업 삭제
    const deleteResponse = await request(app)
      .delete(`/api/tasks/${taskId}`);
    
    expect(deleteResponse.status).toBe(204);

    // 5. 삭제 확인
    const getAfterDeleteResponse = await request(app)
      .get(`/api/tasks/${taskId}`);
    
    expect(getAfterDeleteResponse.status).toBe(404);
  });
});
```

### 성능 테스트

**부하 테스트**:
```typescript
describe('성능 테스트', () => {
  test('동시 요청 처리', async () => {
    const requests = Array.from({ length: 100 }, (_, i) =>
      request(app)
        .post('/api/tasks')
        .send({
          title: `병렬 작업 ${i}`,
          description: '병렬 처리 테스트'
        })
    );

    const startTime = Date.now();
    const responses = await Promise.all(requests);
    const endTime = Date.now();

    responses.forEach(response => {
      expect(response.status).toBe(201);
    });

    expect(endTime - startTime).toBeLessThan(5000); // 5초 이내
  });
});
```

## 에러 핸들링 베스트 프랙티스

### 포괄적 에러 처리

```typescript
// middleware/errorHandler.ts
export const errorHandler = (
  error: any,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  // 로그 출력
  console.error('API Error:', {
    error: error.message,
    stack: error.stack,
    method: req.method,
    url: req.url,
    body: req.body,
    timestamp: new Date().toISOString()
  });

  // 에러 타입별 처리
  if (error.name === 'TaskNotFoundError') {
    return res.status(404).json({
      success: false,
      error: {
        message: '작업을 찾을 수 없습니다',
        code: 'TASK_NOT_FOUND',
        statusCode: 404
      }
    });
  }

  if (error.name === 'ValidationError') {
    return res.status(400).json({
      success: false,
      error: {
        message: '검증 에러',
        code: 'VALIDATION_ERROR',
        statusCode: 400,
        details: error.details
      }
    });
  }

  // 알 수 없는 에러
  res.status(500).json({
    success: false,
    error: {
      message: '서버 내부 에러가 발생했습니다',
      code: 'INTERNAL_SERVER_ERROR',
      statusCode: 500
    }
  });
};
```

## 실습에서의 학습 효과

### API 개발에서의 AITDD 효과

**개발 속도 실감**:
- 전통적인 API 개발: 며칠~1주일
- AITDD 사용: 몇 시간
- **대폭적인 효율화** 실현

**품질 안정성**:
- 포괄적 테스트에 의한 품질 보증
- 에러 핸들링의 표준화
- API 문서의 자동 생성

**실천적 기술 습득**:
- 웹 개발에서의 효과적인 AI 활용
- 복잡한 통합 처리 대응
- 프로덕션 품질의 구현

### 기존 개발과의 차이

**설계 단계**:
- 기존: 상세 설계에 시간 투입
- AITDD: AI와 협력하여 단계적으로 설계

**구현 단계**:
- 기존: 수동으로 상세 구현
- AITDD: AI 생성 코드의 품질 관리

**테스트 단계**:
- 기존: 구현 후 테스트 작성
- AITDD: 테스트 우선 접근법

## 다음 장을 위한 준비

이번 API 개발 경험으로 다음이 습득됩니다:

1. **웹 개발에서의 AI 활용 기술**
2. **복잡한 통합 처리 관리 방법**
3. **프로덕션 품질의 구현 기법**
4. **에러 핸들링과 디버깅 기술**

다음 장에서는 이러한 구현 과정에서 발생하는 에러와 문제에 대한 구체적인 대처법을 학습합니다.

## 정리

API 개발을 통해 다음을 습득했습니다:

**기술적 성장**:
- RESTful API 설계 실습
- 비동기 처리와 에러 핸들링
- TypeScript로 타입 안전한 구현
- 포괄적인 테스트 전략

**AITDD 활용 기술**:
- 복잡한 구현에서의 AI 협조
- 단계적 기능 추가 관리
- 품질 관리의 자동화
- 문서 생성 활용

**실천적 개발력**:
- 프로덕션 품질의 구현
- 성능을 고려한 설계
- 보안을 고려한 구현
- 운영을 고려한 설계

이러한 경험으로 실제 제품 개발에서도 AITDD를 효과적으로 활용할 수 있는 기반이 마련됩니다.