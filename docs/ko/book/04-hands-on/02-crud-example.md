# 4.2 CRUD 작업 구현 예제

## 학습 목표

이 장에서는 더 실제적인 CRUD(생성, 읽기, 업데이트, 삭제) 작업 구현을 통해 다음을 배우게 됩니다:

- 여러 기능 통합을 위한 AITDD 활용
- 데이터 관리 로직 설계 및 구현
- 실제에 가까운 애플리케이션 개발 경험
- 감각 코딩과의 차이점 실감

## 프로젝트 개요: 간단한 작업 관리 시스템

### 구현할 기능

간단한 메모리 기반 작업 관리 시스템을 만듭니다:

```typescript
interface Task {
  id: string;
  title: string;
  description: string;
  completed: boolean;
  createdAt: Date;
  updatedAt: Date;
}
```

**CRUD 작업**:
- **생성(Create)**: 새 작업 생성
- **읽기(Read)**: 작업 검색(전체, 단일, 조건 검색)
- **업데이트(Update)**: 작업 업데이트
- **삭제(Delete)**: 작업 삭제

### 기술적 복잡도 수준

이전 장의 계산기 함수와 비교하여 다음과 같은 복잡성이 추가됩니다:
- 데이터 지속성(메모리 기반)
- 다중 엔터티 작업
- 검증의 조합
- 다양한 에러 처리

## 실습

### 단계 1: TODO 작성과 기능 분해

AITDD의 중요한 교훈은 **약 3개 기능의 통합이 실질적인 한계**라는 것입니다. 따라서 기능을 적절한 세분성으로 분해합니다.

```markdown
# TODO: 작업 관리 시스템 CRUD 구현

## 1단계: 기초 구현
- [ ] Task 인터페이스 정의
- [ ] TaskManager 클래스의 기본 구조
- [ ] 메모리 저장소 구현

## 2단계: 기본 CRUD (최대 3개 기능)
- [ ] Create: 새 작업 생성
- [ ] Read: 모든 작업 가져오기
- [ ] Read: ID로 단일 작업 가져오기

## 3단계: 확장 CRUD (나머지 기능)
- [ ] Update: 작업 업데이트
- [ ] Delete: 작업 삭제
- [ ] Read: 조건 검색(완료 상태 등)

## 기술 요구사항
- TypeScript 사용
- Jest로 테스트
- 메모리 기반 데이터 관리
- 적절한 에러 처리
```

**핵심 포인트**:
- 단계별로 통합 복잡성 제어
- 각 단계는 독립적으로 테스트 가능
- 점진적인 기능 추가 가능

### 단계 2: 1단계 구현

#### 사양 작성

**AI에게 보낼 프롬프트**:
```
작업 관리 시스템의 기초 부분에 대한 사양을 작성해주세요.

요구사항:
- Task 인터페이스 정의
- TaskManager 클래스 설계
- 메모리 기반 저장 기능
- TypeScript를 사용한 타입 안전성

다음 TODO를 참고하세요:
[1단계 TODO 붙여넣기]
```

**생성된 사양 예시**:
```markdown
# 작업 관리 시스템 기초 사양

## Task 인터페이스
```typescript
interface Task {
  id: string;          // UUID 형식의 고유 ID
  title: string;       // 작업 제목(1-100자)
  description: string; // 작업 설명(0-500자)
  completed: boolean;  // 완료 상태
  createdAt: Date;     // 생성 날짜
  updatedAt: Date;     // 업데이트 날짜
}
```

## TaskManager 클래스
- 메모리 기반 작업 관리
- 작업 지속성(배열)
- 자동 ID 생성
- 기본 검증

## 에러 처리
- 작업을 찾을 수 없음: TaskNotFoundError
- 검증 실패: ValidationError
- 잘못된 ID 형식: InvalidIdError
```

#### 테스트 케이스 작성

**AI 프롬프트**:
```
다음 사양을 기반으로 기초 부분의 테스트 케이스를 작성해주세요:

[기초 사양 붙여넣기]

테스트 관점:
- 인터페이스 타입 검사
- TaskManager 초기화
- 메모리 저장소 작업 검증
- 에러 클래스 정의 검증
```

#### Red-Green-Refactor-Validation 실행

AI에게 단계별 구현을 요청합니다:

1. **Red**: 테스트 실패 확인
2. **Green**: 기초 클래스의 최소 구현
3. **Refactor**: 설계 개선
4. **Validation**: 기초 유효성 확인

### 단계 3: 2단계 구현(기본 CRUD)

#### 통합 시 중요 사항

감각 코딩과의 중요한 차이점은 **구조화된 접근법**이 필요하다는 것입니다:

**AITDD 방법**:
```
1. 명확한 사양 정의
2. 포괄적인 테스트 설계
3. 점진적 구현
4. 품질 검사
→ 쉬운 통합, 안정적인 품질
```

**감각 코딩 방법(피해야 할)**:
```
1. AI에게 충동적인 구현 요청
2. 나중에 테스트 추가
3. 통합 중 문제 발견
4. 수동 수정
→ 3개 기능 통합에서 무너짐
```

#### Create 기능 구현

**사양**:
```markdown
## 작업 생성 기능

### 메서드: createTask(taskData)
- 매개변수: { title: string, description: string }
- 반환값: 생성된 Task
- 검증:
  - title은 필수, 1-100자
  - description은 0-500자
- 자동 설정: id, createdAt, updatedAt, completed=false
```

**AI 프롬프트 예시**:
```
다음 사양으로 createTask 메서드의 테스트 케이스와 구현을 작성해주세요:

[사양 붙여넣기]

요구사항:
1. 포괄적인 테스트 케이스(정상, 비정상, 경계값)
2. Red-Green-Refactor-Validation 사이클로 구현
3. 기존 기초 코드와의 일관성 보장
4. TypeScript 타입 안전성 활용
```

#### Read 기능 구현

**전체 가져오기와 단일 가져오기**를 동시에 구현합니다:

**사양**:
```markdown
## 작업 검색 기능

### getAllTasks(): Task[]
- 배열로 모든 작업 반환
- 비어있으면 빈 배열
- 생성 날짜 내림차순 정렬

### getTaskById(id: string): Task
- ID로 작업 검색
- 찾을 수 없으면: TaskNotFoundError
- 잘못된 ID 형식: InvalidIdError
```

### 단계 4: 3단계 구현(확장 CRUD)

#### Update 기능

**부분 업데이트 지원**:

```typescript
interface TaskUpdateData {
  title?: string;
  description?: string;
  completed?: boolean;
}

updateTask(id: string, updateData: TaskUpdateData): Task
```

#### Delete 기능

**논리적 vs 물리적 삭제** 고려를 포함한 구현:

```typescript
deleteTask(id: string): boolean  // 물리적 삭제
// 또는
softDeleteTask(id: string): Task  // 논리적 삭제
```

#### 조건부 검색 기능

**필터링 기능**:

```typescript
interface TaskFilter {
  completed?: boolean;
  titleContains?: string;
  createdAfter?: Date;
}

searchTasks(filter: TaskFilter): Task[]
```

### 단계 5: 통합 테스트와 최종 검토

#### 통합 시나리오 테스트

실제 사용 사례를 가정한 테스트:

```typescript
describe('작업 관리 통합 시나리오', () => {
  test('일반적인 작업 관리 흐름', async () => {
    const manager = new TaskManager();
    
    // 1. 작업 생성
    const task1 = manager.createTask({
      title: '프로젝트 계획',
      description: '요구사항 정의와 설계'
    });
    
    // 2. 작업 가져오기 및 확인
    const allTasks = manager.getAllTasks();
    expect(allTasks).toHaveLength(1);
    
    // 3. 작업 업데이트
    const updatedTask = manager.updateTask(task1.id, {
      completed: true
    });
    expect(updatedTask.completed).toBe(true);
    
    // 4. 검색 기능
    const completedTasks = manager.searchTasks({
      completed: true
    });
    expect(completedTasks).toHaveLength(1);
    
    // 5. 작업 삭제
    const deleted = manager.deleteTask(task1.id);
    expect(deleted).toBe(true);
    expect(manager.getAllTasks()).toHaveLength(0);
  });
});
```

#### 성능 테스트

**대용량 데이터로 작업 검증**:

```typescript
describe('성능 테스트', () => {
  test('1000개 작업 작업', () => {
    const manager = new TaskManager();
    
    // 1000개 작업 생성
    const startTime = Date.now();
    for (let i = 0; i < 1000; i++) {
      manager.createTask({
        title: `작업 ${i}`,
        description: `설명 ${i}`
      });
    }
    const createTime = Date.now() - startTime;
    
    // 검색 성능
    const searchStart = Date.now();
    const results = manager.searchTasks({
      titleContains: '작업 1'
    });
    const searchTime = Date.now() - searchStart;
    
    expect(createTime).toBeLessThan(1000); // 1초 이내
    expect(searchTime).toBeLessThan(100);  // 100ms 이내
  });
});
```

## 문제 해결

### 일반적인 문제와 해결책

#### 문제 1: AI 생성 코드 일관성

**증상**:
- 기존 코드의 의도하지 않은 수정
- 인터페이스 불일치
- 다른 네이밍 규칙

**해결책**:
```
개선된 프롬프트 예시:
"기존 코드를 변경하지 않고 다음 인터페이스에 따라 새 메서드만 추가해주세요:
[기존 인터페이스를 명확하게 명시]"
```

#### 문제 2: 불충분한 테스트 품질

**증상**:
- 비정상 케이스 테스트 부족
- 경계값 테스트 누락
- 통합 테스트 없음

**해결책**:
```
검토 체크리스트:
- [ ] 모든 정상, 비정상, 경계 케이스를 포함
- [ ] 에러 메시지 검증
- [ ] 실제 사용 사례로 테스트
```

#### 문제 3: 통합 복잡성

**증상**:
- 3개 이상 기능 통합 시 무너짐
- 어려운 디버깅
- 테스트가 통과하지 않음

**해결책**:
```
점진적 통합 접근법:
1. 한 번에 하나의 기능을 완전히 구현
2. 2개 기능의 조합 테스트
3. 3번째 기능 추가 시 신중히 진행
4. 문제 발생 시 기능 분할
```

## AI 프롬프트 모범 사례

### 효과적인 프롬프트 설계

**1. 명확한 컨텍스트**:
```
좋은 예시:
"작업 관리 시스템의 CRUD 작업에서 다음 사양에 따라 createTask 메서드를 구현해주세요.
기존 Task 인터페이스와 TaskManager 클래스에 추가하여 구현하고,
메모리 기반 저장소를 사용합니다.

[기존 코드]
[상세 사양]
[예상 출력 형식]"
```

**2. 명시적 제약사항**:
```
제약사항 예시:
- "기존 코드를 수정하지 마세요"
- "TypeScript 타입 안전성을 최대화하세요"
- "적절한 에러 처리를 구현하세요"
- "테스트 우선으로 구현하세요"
```

**3. 예상 품질 지정**:
```
품질 요구사항 예시:
- "프로덕션 수준의 품질로 구현하세요"
- "가독성과 유지보수성을 우선시하세요"
- "성능을 고려하여 구현하세요"
- "적절한 주석을 포함하세요"
```

### 프롬프트 템플릿

CRUD 작업별 프롬프트 템플릿:

```
### CRUD 구현 프롬프트 템플릿

**기본 정보**:
- 기능: [Create/Read/Update/Delete] 작업
- 대상 엔터티: [엔터티 이름]
- 구현 언어: TypeScript
- 테스트 프레임워크: Jest

**기존 컨텍스트**:
[기존 인터페이스/클래스 정의]

**구현 요구사항**:
[구체적인 사양]

**제약사항**:
- 기존 코드 수정 없음
- 타입 안전성 최대화
- 에러 처리 필수

**출력 요구사항**:
1. 테스트 케이스(정상/비정상/경계)
2. 구현 코드
3. 사용 예시
4. 주의사항과 제한사항
```

## 품질 관리 포인트

### 코드 검토 체크리스트

**기능적 품질**:
- [ ] 모든 사양 요구사항 충족
- [ ] 적절한 에러 처리
- [ ] 경계에서의 올바른 동작
- [ ] 허용 범위 내의 성능

**기술적 품질**:
- [ ] TypeScript 타입 안전성 보장
- [ ] 네이밍 규칙 준수
- [ ] 적절한 추상화 수준
- [ ] DRY 원칙 준수

**테스트 품질**:
- [ ] 충분한 테스트 커버리지
- [ ] 테스트가 이해하기 쉬움
- [ ] 적절한 모의 사용
- [ ] 포괄적인 통합 테스트

**유지보수성**:
- [ ] 코드가 읽기 쉬움
- [ ] 구조가 쉬운 변경 허용
- [ ] 적절한 문서화
- [ ] 확장성을 고려한 설계

## 실습에서의 학습 효과

### AITDD 강점(경험하게 될 것)

**개발 속도**:
- 전통적인 CRUD 구현: 1-2일
- AITDD 사용: 1시간 이내
- **20-48배 효율성 향상** 경험

**품질 안정성**:
- 테스트 우선을 통한 품질 보증
- 리팩토링 단계에서의 최적화
- Validation 단계에서의 포괄적 품질 검사

**학습 효과**:
- AI 협업 개발 기술
- 효과적인 프롬프트 설계 능력
- 품질 관리에 대한 민감도 향상

### 감각 코딩과의 명확한 차이

**통합의 용이성**:
- 감각 코딩: 3개 기능 통합에서 무너짐
- AITDD: 점진적 통합으로 안정적

**디버깅 효율성**:
- 감각 코딩: 같은 문제의 반복
- AITDD: 체계적인 문제 해결

**장기적 유지보수성**:
- 감각 코딩: 나중에 수정 필요
- AITDD: 지속적인 개발 가능

## 다음 장을 위한 준비

이번 CRUD 구현 경험을 통해 다음 기술을 습득하게 됩니다:

1. **다중 기능 통합 관리**
2. **점진적 기능 개발**
3. **품질 관리의 중요성 이해**
4. **실용적인 AI 프롬프트 설계 기술**

다음 장에서는 이러한 기술을 사용하여 더 실용적인 개발 시나리오인 API 개발에 도전합니다. 외부 종속성과 비동기 처리 같은 새로운 복잡성을 AITDD가 어떻게 처리하는지 배우게 됩니다.

## 정리

CRUD 작업 구현을 통해 우리가 배운 것:

**프로세스의 중요성**:
- 구조화된 접근법의 가치
- 점진적 기능 추가의 효과
- 품질 관리의 자동화

**AI 활용 팁**:
- 명확한 컨텍스트 제공
- 적절한 제약사항 지정
- 지속적인 프롬프트 개선

**실용적 기술**:
- 다중 기능 통합 기법
- 테스트 주도 설계 사고
- 검토와 품질 관리

이러한 기초가 있으면 더 복잡한 실제 개발 프로젝트에서도 AITDD를 효과적으로 활용할 수 있습니다.