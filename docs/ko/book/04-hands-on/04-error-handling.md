# 4.4 에러 핸들링과 디버깅

## 학습 목표

이 장에서는 AITDD 개발 중 발생하는 각종 에러의 대처법과 효과적인 디버깅 기법을 습득합니다:

- AI 생성 코드 특유의 에러 패턴 이해
- 프롬프트로 인한 에러의 식별과 수정 방법
- 효율적인 디버깅 프로세스 확립
- 수동 구현으로의 전환 판단과 실습
- 에러 예방을 위한 베스트 프랙티스

## 에러 분류와 대응 전략

AITDD 개발에서는 기존 개발과는 다른 종류의 에러가 발생합니다. 이들을 적절히 분류하고 각각에 맞는 대처법을 적용하는 것이 중요합니다.

### 에러의 기본 분류

**1. 프롬프트로 인한 에러**
- 지시의 모호함으로 인한 것
- 요구사항의 불명확함으로 인한 것
- 컨텍스트 부족으로 인한 것

**2. AI 구현으로 인한 에러**
- AI 생성 코드의 버그
- 기존 코드와의 정합성 문제
- 성능 문제

**3. 통합으로 인한 에러**
- 다중 기능 통합 시 문제
- 인터페이스 불일치
- 의존성 문제

**4. 전통적 에러**
- 일반적인 프로그래밍 에러
- 환경 설정 문제
- 외부 의존성 문제

## 실천적 디버깅 프로세스

### 단계 1: 에러 정보의 포괄적 수집

에러가 발생한 경우, 먼저 정보를 체계적으로 수집합니다.

**수집할 정보**:
```markdown
## 에러 정보 수집 체크리스트

### 기본 정보
- [ ] 에러 메시지(완전판)
- [ ] 스택 트레이스
- [ ] 발생 시점
- [ ] 실행 환경

### 컨텍스트 정보
- [ ] 실행된 명령
- [ ] 입력 데이터
- [ ] 기대했던 동작
- [ ] 실제 동작

### AI 관련 정보
- [ ] 사용한 프롬프트
- [ ] AI 생성 코드의 해당 부분
- [ ] 관련 기존 코드

### 실행 환경
- [ ] OS·브라우저 정보
- [ ] Node.js/TypeScript 버전
- [ ] 의존 패키지 상태
```

**정보 수집 실례**:
```
에러 발생 시 기록 예시:

에러 메시지:
TypeError: Cannot read property 'map' of undefined
    at getAllTasks (TaskController.ts:15)
    at Router.handle (express/lib/router/layer.js:95)

발생 시점:
GET /api/tasks 엔드포인트 테스트 실행 시

입력 데이터:
GET http://localhost:3000/api/tasks

기대 동작:
빈 배열을 포함한 응답이 반환됨

실제 동작:
undefined에 대해 map을 호출하여 에러

사용 프롬프트:
"TaskManager 클래스의 getAllTasks 메서드를 호출해 응답을 반환하는 API 엔드포인트를 구현해주세요"
```

### 단계 2: AI를 통한 원인 분석

수집한 정보를 모두 AI에게 전달하고 에러 원인 분석을 요청합니다.

**효과적인 분석 프롬프트**:
```
다음 에러에 대해 원인을 분석해주세요:

## 에러 정보
[수집한 에러 정보를 모두 붙여넣기]

## 관련 코드
[에러가 발생한 소스코드 일체]

## 분석 요구
1. 에러의 직접적인 원인
2. 근본 원인(설계·구현 문제)
3. 프롬프트로 인한 가능성
4. 수정 방침 제안
5. 유사 에러의 예방책

가능한 한 구체적이고 실천적인 분석을 부탁합니다.
```

**AI에 의한 분석 예시**:
```
## 에러 분석 결과

### 직접적인 원인
TaskController.getAllTasks 메서드에서 taskManager.getAllTasks()가 undefined를 반환하고 있음

### 근본 원인
1. TaskManager 클래스의 인스턴스화 문제
2. getAllTasks 메서드 구현에서 작업 배열이 초기화되지 않음

### 프롬프트로 인한 가능성
"기존 TaskManager 클래스를 사용"이라는 지시가 모호해서,
실제 클래스 정의를 확인하지 않고 구현했을 가능성

### 수정 방침
1. TaskManager 클래스 초기화 확인
2. getAllTasks 메서드 반환값 검증
3. null/undefined 체크 추가

### 예방책
1. 기존 코드의 상세를 프롬프트에 포함
2. 반환값의 타입 안전성 강화
3. 단위 테스트에 null/undefined 케이스 추가
```

### 단계 3: 단계적 조사와 가설 검증

AI의 분석을 바탕으로 단계적으로 문제를 조사합니다.

**조사 순서 예시**:
```typescript
// 1. TaskManager 클래스 상태 확인
describe('TaskManager 디버깅', () => {
  test('TaskManager 인스턴스화 확인', () => {
    const manager = new TaskManager();
    console.log('TaskManager instance:', manager);
    expect(manager).toBeDefined();
  });

  test('getAllTasks 반환값 확인', () => {
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    console.log('getAllTasks result:', result);
    console.log('result type:', typeof result);
    expect(result).toBeDefined();
  });

  test('작업 배열 초기 상태 확인', () => {
    const manager = new TaskManager();
    const tasks = manager.getAllTasks();
    expect(Array.isArray(tasks)).toBe(true);
    console.log('Initial tasks array:', tasks);
  });
});
```

**디버그 실행**:
```bash
npm test -- --verbose TaskManager디버깅
```

### 단계 4: AI와 협력한 수정 구현

조사 결과를 AI에게 피드백하고 수정 구현을 요청합니다.

**수정 요청 프롬프트**:
```
디버그 조사 결과, 다음이 판명되었습니다:

## 조사 결과
[디버그 테스트의 출력 결과]

## 판명된 문제
1. TaskManager 클래스의 배열 초기화가 부적절
2. getAllTasks 메서드의 반환값이 undefined

## 수정 요구
다음을 만족하는 수정을 구현해주세요:
1. 배열이 적절히 초기화됨
2. 타입 안전성이 확보됨
3. null/undefined 체크가 추가됨
4. 기존 테스트가 통과함
5. 새로운 테스트 케이스도 추가

수정 후 코드와 수정 이유 설명도 부탁합니다.
```

## 프롬프트로 인한 에러의 식별과 수정

### 프롬프트 문제의 판단 기준

**빈도에 의한 판정**:
```
유사한 에러 패턴의 발생 상황:
- 1회차: 구현 에러로 처리
- 2회차: 프롬프트 문제 가능성 검토
- 3회차: 프롬프트 수정 필수
```

**전형적인 프롬프트 문제의 징후**:
- 같은 요구에 다른 구현이 생성됨
- 기대와 크게 다른 출력
- 기존 코드를 의도치 않게 수정
- 지시 범위를 초과한 구현

### 프롬프트 문제의 수정 프로세스

#### 1. 프롬프트 진단

**AI에 의한 진단 요청**:
```
다음 프롬프트를 분석하고 문제점을 지적해주세요:

## 사용한 프롬프트
[문제가 있었던 프롬프트]

## 기대한 결과
[기대했던 출력]

## 실제 결과
[실제 출력]

## 진단 요구
1. 프롬프트의 모호한 부분
2. 부족한 정보
3. 오해를 부를 가능성이 있는 표현
4. 개선 제안
```

#### 2. 프롬프트 개선안 작성

**개선 프롬프트 예시**:
```
개선 전(문제가 있던 프롬프트):
"TaskManager 클래스를 사용해서 API 엔드포인트를 만들어주세요"

개선 후:
"다음 기존 TaskManager 클래스를 사용하여 GET /api/tasks 엔드포인트를 구현해주세요.

기존 코드:
[TaskManager 클래스의 완전한 코드]

요구사항:
1. 기존 코드는 일절 변경하지 않음
2. Express.js의 Router를 사용
3. 응답 형식: { success: boolean, data: Task[] }
4. 에러 핸들링 포함
5. TypeScript 타입 안전성 확보

출력 형식:
- routes/tasks.ts 파일
- controllers/TaskController.ts 파일
- 대응하는 테스트 파일"
```

#### 3. 개선 프롬프트의 검증

**검증 프로세스**:
```typescript
describe('프롬프트 개선 검증', () => {
  test('개선 전 프롬프트로 문제 재현', async () => {
    // 문제가 있던 프롬프트로 구현 요청
    // 기대하는 문제가 발생하는지 확인
  });

  test('개선 후 프롬프트로 해결 확인', async () => {
    // 개선된 프롬프트로 구현 요청
    // 문제가 해결되는지 확인
  });

  test('개선 프롬프트의 부작용 확인', async () => {
    // 개선으로 새로운 문제가 발생하지 않는지 확인
  });
});
```

## 수동 구현으로의 전환 판단

### 전환 시점의 판단

**구현 전 판단**:
```markdown
## 수동 구현 검토 체크리스트

### 구현 이미지의 유무
- [ ] 구체적인 구현 순서가 떠오름
- [ ] 사용할 기술·라이브러리가 명확
- [ ] 구현의 함정을 예측할 수 있음

### 기술적 복잡성
- [ ] 성능 최적화가 필요
- [ ] 복잡한 알고리즘이 필요
- [ ] 깊은 도메인 지식이 필요

### AI에 대한 설명 곤란도
- [ ] 요구사항을 명확히 언어화할 수 없음
- [ ] 프롬프트가 비정상적으로 길어짐
- [ ] 전제 지식의 설명이 곤란
```

**구현 중 전환 판단**:
```
전환 판단 기준:
1. 같은 에러가 3회 이상 발생
2. 프롬프트 수정으로도 해결되지 않음
3. 디버그 시간이 구현 시간을 초과
4. AI 생성 코드의 품질이 일관되지 않음
```

### 효과적인 수동 구현 접근법

#### 단계적 AI 병용 전략

**완전 수동이 아닌 부분적 AI 활용**:
```typescript
// 1. 복잡한 로직 부분은 수동 구현
const complexAlgorithm = (data: any[]) => {
  // 수동으로 구현(AI에 설명 곤란한 부분)
  let result = [];
  for (let i = 0; i < data.length; i++) {
    // 복잡한 계산 로직
  }
  return result;
};

// 2. 정형적인 코드는 AI에 요청
const generateApiResponse = (data: any) => {
  // 이 부분은 AI로 생성 가능
  return {
    success: true,
    data,
    meta: {
      timestamp: new Date().toISOString(),
      count: Array.isArray(data) ? data.length : 1
    }
  };
};

// 3. 조합하여 최종 구현
export const processData = async (inputData: any[]) => {
  try {
    const processedData = complexAlgorithm(inputData); // 수동 부분
    return generateApiResponse(processedData); // AI 생성 부분
  } catch (error) {
    // 에러 핸들링도 AI 지원 가능
  }
};
```

#### 수동 구현에서의 AI 지원 활용

**1. IDE 보완 기능 활용**:
```typescript
// VS Code 등의 AI 보완을 적극적으로 사용
const taskManager = new TaskManager();
// ↑ 여기서 AI 보완이 효과적인 부분은 활용
```

**2. 부분적인 코드 생성 요청**:
```
명확한 구현 이미지가 있는 부분의 AI 요청 예시:

"다음 타입 정의를 기반으로 검증 함수를 만들어주세요:

interface TaskInput {
  title: string;
  description?: string;
}

요구사항:
- title은 1-100자 필수
- description은 0-500자 선택
- 부정확한 경우 구체적인 에러 메시지
- TypeScript 타입 가드로 기능
```

#### 품질 프로세스의 지속

**수동 구현에서도 Validation 단계는 필수**:
```
수동 구현의 Validation 체크 항목:
1. 사양 요구사항과의 정합성
2. 타입 안전성 확보
3. 에러 핸들링의 적절성
4. 테스트 커버리지 확인
5. 성능의 타당성
6. 코드의 가독성·유지보수성
```

## 에러 예방의 베스트 프랙티스

### 프롬프트 설계 개선

**1. 컨텍스트 명확화**:
```
좋은 프롬프트 예시:

"다음 기존 시스템에 새 기능을 추가해주세요:

기존 코드:
[관련된 모든 코드를 붙여넣기]

새 기능 요구사항:
[구체적이고 명확한 요구사항]

제약 조건:
- 기존 코드는 변경 금지
- TypeScript 타입 안전성 확보
- 에러 핸들링 필수

기대하는 출력:
- 구현 코드
- 테스트 코드
- 사용 예시
- 주의점"
```

**2. 단계적 구현 지시**:
```
복잡한 기능의 단계적 구현:

"다음을 단계적으로 구현해주세요:

단계 1: 인터페이스 설계
단계 2: 기본 구현
단계 3: 에러 핸들링
단계 4: 테스트 작성

각 단계에서 확인을 받으며 진행해주세요."
```

### 테스트 전략 강화

**1. AI 생성 코드 특유의 테스트**:
```typescript
describe('AI 생성 코드 검증 테스트', () => {
  test('의도하지 않은 기존 코드 수정이 없는지 확인', () => {
    // 기존의 중요한 함수가 변경되지 않았는지 테스트
    const originalFunction = require('./legacy/original-module');
    expect(originalFunction.criticalMethod).toBeDefined();
    expect(typeof originalFunction.criticalMethod).toBe('function');
  });

  test('추측에 의한 구현 범위 확인', () => {
    // AI가 추측으로 구현한 부분이 요구사항 내인지 확인
    const implementation = new FeatureImplementation();
    expect(implementation.getImplementedFeatures())
      .toEqual(expect.arrayContaining(REQUIRED_FEATURES));
  });

  test('타입 안전성 확인', () => {
    // TypeScript 타입 체크가 올바르게 기능하는지 확인
    // 컴파일 에러가 발생하지 않는지 테스트
  });
});
```

**2. 통합 테스트 강화**:
```typescript
describe('기능 통합 테스트', () => {
  test('3기능 통합에서의 회귀 확인', async () => {
    // 3개 기능이 조합되어도 정상 동작하는지 테스트
    const feature1 = await executeFeature1();
    const feature2 = await executeFeature2(feature1.result);
    const feature3 = await executeFeature3(feature2.result);
    
    expect(feature3.result).toMatchExpectedOutput();
  });
});
```

### 지속적 개선 프로세스

**1. 에러 패턴 축적**:
```markdown
## 에러 패턴 관리

### 발생 빈도가 높은 에러
1. undefined/null 접근 에러
   - 원인: AI 생성 코드에서의 초기화 부족
   - 대책: 명시적 초기화 지시

2. 타입 불일치 에러
   - 원인: 프롬프트에서의 타입 정보 부족
   - 대책: 타입 정의를 명시적으로 제공

3. 기존 코드 변경 에러
   - 원인: "변경 금지" 지시의 불철저
   - 대책: 구체적인 제약 지시
```

**2. 프롬프트 템플릿 개선**:
```
개선된 프롬프트 템플릿:

### 기본 템플릿
```
기능: [기능명]
구현 대상: [구체적인 구현 내용]

기존 코드:
[관련 코드 전체]

요구사항:
[명확하고 구체적인 요구사항]

제약:
- 기존 코드는 변경 금지
- [기타 제약]

출력 요구:
- [기대하는 출력 형식]

품질 요구사항:
- TypeScript 타입 안전성
- 에러 핸들링
- 테스트 코드 포함
```

## 실천적 디버깅 테크닉

### 로그 기반 디버깅

**효과적인 로그 출력**:
```typescript
// 디버그용 로그 헬퍼
class DebugLogger {
  static log(context: string, data: any) {
    if (process.env.NODE_ENV === 'development') {
      console.log(`[${context}]`, {
        timestamp: new Date().toISOString(),
        data: JSON.stringify(data, null, 2)
      });
    }
  }

  static error(context: string, error: any) {
    console.error(`[ERROR:${context}]`, {
      message: error.message,
      stack: error.stack,
      timestamp: new Date().toISOString()
    });
  }
}

// 사용 예시
export const taskController = {
  getAllTasks: async (req, res, next) => {
    try {
      DebugLogger.log('TaskController.getAllTasks', 'Starting execution');
      
      const tasks = taskManager.getAllTasks();
      DebugLogger.log('TaskController.getAllTasks', { tasksCount: tasks?.length });
      
      const response = generateResponse(tasks);
      DebugLogger.log('TaskController.getAllTasks', { response });
      
      res.json(response);
    } catch (error) {
      DebugLogger.error('TaskController.getAllTasks', error);
      next(error);
    }
  }
};
```

### 테스트 주도 디버깅

**실패 테스트로부터 역산**:
```typescript
describe('버그 재현 테스트', () => {
  test('특정 조건에서의 null 에러 재현', () => {
    // 버그가 발생하는 최소 조건 특정
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    
    // 이 시점에서 에러가 발생해야 함
    expect(() => result.map(x => x.id)).toThrow();
  });

  test('수정 후 동작 확인', () => {
    // 수정 후 기대하는 동작을 테스트
    const manager = new TaskManager();
    const result = manager.getAllTasks();
    
    expect(Array.isArray(result)).toBe(true);
    expect(() => result.map(x => x.id)).not.toThrow();
  });
});
```

## 정리

이 장에서는 AITDD 개발에서의 포괄적인 에러 핸들링과 디버깅 기법을 학습했습니다:

**주요 학습 성과**:
- 에러의 적절한 분류와 대처법
- AI 활용에 의한 효율적 디버깅 프로세스
- 프롬프트로 인한 에러의 식별과 수정
- 수동 구현으로의 전환 판단과 AI 병용 전략
- 에러 예방의 베스트 프랙티스

**실천적 기술**:
- 포괄적인 정보 수집 기법
- AI와 협력한 에러 분석
- 단계적 문제 해결 접근법
- 품질 관리의 지속적 개선

**다음 장을 위한 준비**:
이러한 기술로 더 고도의 AITDD 기법과 최적화 기술을 학습할 준비가 되었습니다. 프롬프트 설계와 AI 활용의 최적화로 진행합니다.

AITDD의 실습 시리즈를 통해 기초부터 응용까지 한 번에 기술을 습득했습니다. 다음 부에서는 이러한 기술을 더욱 다듬어 실제 프로덕션 환경에서 활용하기 위한 고급 기법을 학습해 나갑니다.