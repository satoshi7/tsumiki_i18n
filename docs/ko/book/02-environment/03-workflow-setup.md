# 2.3 개발 환경과 워크플로 구축

AITDD를 효과적으로 실천하기 위한 개발 환경과 워크플로 구축 방법에 대해 설명합니다. 도구 준비뿐만 아니라 개발 프로세스 전체를 체계화함으로써 일관성 있는 고품질 개발을 실현합니다.

## AITDD 도입의 실제 경위와 방법론 진화

### 도입 타임라인

#### 2025년 초부터의 본격적인 노력
**계기:**
- **Claude Sonnet 3.5**와 **DeepSeek R1 증류 모델**의 등장
- 어느 정도의 구현이 AI로 실현 가능하다는 확신을 얻음
- 기존 수동 코딩의 한계를 느낌

**약 5-6개월의 실천 경험 축적:**
- 초기 시행착오에서 체계적 방법론으로의 발전
- 프롬프트 설계 최적화 패턴 발견
- 실패 사례로부터의 학습과 모범 사례 확립

### 방법론 진화 과정

#### 1단계: 라이브 코딩 (초기 접근법)
**특징:**
- AI와 실시간 대화하며 코드 작성
- 소규모 기능 구현에 효과적
- 즉시 피드백을 통한 신속한 수정

**효과적이었던 장면:**
- 단일 파일 내 소규모 수정
- 프로토타입의 신속한 작성
- 기술 조사용 샘플 코드 작성

**한계 발견:**
- 대규모 개발에서 구조가 붕괴되기 쉬움
- 복잡한 의존 관계 관리가 어려움
- 품질의 일관성을 유지하기 어려움

#### 2단계: TDD와의 결합 (현재 방법론)
**과제 인식:**
- 라이브 코딩으로는 대규모 개발이 어려움
- 품질 보증 메커니즘이 부족
- 설계 일관성을 유지하는 구조가 필요

**채택한 해결책:**
- **TDD(테스트 주도 개발)**와의 결합
- **Red-Green-Refactor-Validation** 사이클 확립
- 체계적 워크플로 구축

**현재 방법론의 특징:**
```
진화 전 (라이브 코딩):
요구사항 → 직접 구현 → 동작 확인 → 수정 → 완료

진화 후 (AITDD):
요구사항 → TODO 작성 → Red → Green → Refactor → Validation → 완료
         ↑                                          ↓
         ←←←←←←← 피드백 루프 ←←←←←←←←←←
```

### 실제 개발 워크플로 구축 경험

#### 프로젝트 구조 최적화 과정
**초기 과제:**
- 파일 구성이 프로젝트마다 제각각
- TODO 세분화가 적절하지 않음
- Git 이력 추적이 곤란

**개선 후 구조:**
```
project-root/
├── todo.md                    # 중심적 작업 관리
├── docs/                      # 설계 문서 체계화
│   ├── requirements.md        # 명확한 요구사항 정의
│   ├── architecture.md        # 아키텍처 설계
│   └── api-spec.md           # 상세한 API 사양
├── src/                       # 기능별 명확한 분리
├── tests/                     # 테스트 코드 체계화
└── scripts/                   # 자동화 스크립트
```

#### TODO 관리의 진화 과정

**초기 문제:**
- TODO 세분화가 너무 큼 ("시스템 전체 구현" 등)
- 의존 관계가 불명확
- 진행 상황이 추적 곤란

**현재의 최적화된 접근법:**

**적절한 세분화 발견:**
```markdown
# 최적 세분화 (30분~1시간)
- [x] 사용자 등록 API 구현
- [x] 패스워드 검증 기능
- [ ] JWT 인증 미들웨어
- [ ] 로그인 기능 테스트 추가

# 피해야 할 세분화
❌ 시스템 전체 구현 (너무 큼)
❌ 변수명 변경 (너무 작음)
```

**실용적 순차 실행 전략:**
1. **TODO 리스트를 위에서부터 순서대로 처리**
2. **하나의 항목을 완전히 완료한 후 다음으로**
3. **의존 관계가 있는 항목은 순서 조정**
4. **30분~1시간에 완료 가능한 단위로 분할**

### Git 워크플로의 실용적 운용

#### AITDD 특화 브랜치 전략

**실제로 채택한 전략:**
```bash
# TODO 항목별 브랜치 생성 패턴
git checkout -b feature/user-registration    # TODO: 사용자 등록 API
git checkout -b feature/auth-middleware      # TODO: 인증 미들웨어
git checkout -b feature/password-validation  # TODO: 패스워드 검증
```

**AITDD 사이클 대응 커밋 전략:**
```bash
# Red 단계 (실패하는 테스트 작성)
git add tests/user-registration.test.js
git commit -m "Red: Add failing tests for user registration"

# Green 단계 (테스트를 통과시키는 최소 구현)
git add src/controllers/user.js
git commit -m "Green: Implement basic user registration"

# Refactor 단계 (코드 개선)
git add src/controllers/user.js src/models/user.js
git commit -m "Refactor: Extract user validation logic"

# Validation 단계 (최종 검증과 문서화)
git add docs/api-spec.md
git commit -m "Validation: Complete user registration with docs"
```

#### 실패 시 복구 전략의 실천

**실제 운용에서의 판단 기준:**
```bash
# 패턴 1: 경미한 수정으로 대응 가능
if [ "기대와의 차이" == "작음" ]; then
    # 프롬프트 조정으로 재실행
    echo "프롬프트를 상세화하여 재시도"
fi

# 패턴 2: 대폭적인 수정이 필요
if [ "기대와의 차이" == "큼" ]; then
    git reset --hard HEAD~1  # 이전 상태로 되돌림
    echo "프롬프트 재검토 후 재실행"
fi

# 패턴 3: 여러 번의 실패
if [ "실패 횟수" -gt 3 ]; then
    git reset --hard <last_known_good_commit>
    echo "접근법을 근본적으로 재검토"
fi
```

**실제 복구 패턴 예:**
```
상황: 사용자 인증 API 구현에서 에러 핸들링이 부적절
판단: 3회 수정 시도로 개선되지 않음
대응: git reset --hard HEAD~4로 Red 단계까지 되돌림
재실행: 더 상세한 프롬프트로 재시작
결과: 기대한 대로의 구현 완료
```

### 실천에서 얻은 중요한 교훈

#### 성공 요인 분석

**1. 단계적 접근법의 효과:**
- 작은 단계에서의 확실한 진보
- 각 단계에서의 품질 확인
- 실패 시 영향 범위의 한정

**2. 문서화의 중요성:**
- 요구사항 정의의 명확화가 AI 출력 품질에 직결
- API 사양서가 테스트 설계의 지침이 됨
- 진행 상황의 시각화가 동기 유지에 기여

**3. 프롬프트 최적화의 누적 효과:**
- 동일 패턴 재사용으로 효율성 향상
- 실패 사례 분석으로 정확도 향상
- 도메인 특화 지식 축적

#### 자주 발생하는 문제와 대처법

**문제 1: AI 출력 품질이 불안정**
```
증상: 같은 프롬프트로도 날에 따라 결과가 다름
원인: 프롬프트의 모호성, 문맥 부족
대처: 더 구체적인 기술 제약과 예시 추가

개선 예:
"API를 만들어" 
↓
"Express.js + Mongoose로 POST /api/users API를 작성:
- 요청: {name: string, email: string}
- 검증: email 형식 체크, name 필수
- 응답: 201로 생성된 사용자 정보
- 에러: 400(검증), 409(중복), 500(서버)"
```

**문제 2: 대규모 프로젝트에서의 방향감각 상실**
```
증상: 현재 작업 위치를 알 수 없게 됨
원인: TODO 관리 미비, 진행 상황 추적 결여
대처: 명확한 진행 상황 표시와 다음 액션 명시

개선 예:
## 현재 구현 상황 (2025-06-21)
- [x] 사용자 관리 기능 (완료)
- [ ] **인증 기능 (구현 중: JWT middleware 작성 단계)**
- [ ] 권한 관리 기능 (미착수)

### 다음 액션
1. JWT 서명 검증 구현
2. 토큰 갱신 기능 추가
3. 로그아웃 기능 구현
```

**문제 3: 테스트와 코드의 불일치**
```
증상: 테스트는 통과하지만 실제 동작이 기대와 다름
원인: 테스트 설계 미비, 요구사항 이해 차이
대처: 더 현실적인 테스트 케이스 작성

개선 예:
# 불충분한 테스트
test('should create user', () => {
  expect(user).toBeDefined();
});

# 개선된 테스트
test('should create user with valid email and return 201', async () => {
  const userData = { name: 'John', email: 'john@example.com' };
  const response = await request(app)
    .post('/api/users')
    .send(userData)
    .expect(201);
  
  expect(response.body.user.email).toBe(userData.email);
  expect(response.body.user.password).toBeUndefined(); // 패스워드는 포함하지 않음
});
```

## AITDD 개발 워크플로 전체상

### 기본적인 개발 플로우
```
TODO 리스트 작성 → 항목 선택 → AITDD 실행 → 리뷰 → 다음 항목
     ↑                                            ↓
     ←←←←←←←←←← 필요에 따라 조정 ←←←←←←←←←←←←←←
```

### AITDD 실행의 상세 사이클
```
Red (테스트 작성) → Green (구현) → Refactor (개선) → Validation (검증)
      ↑                                                    ↓
      ←←←←←←←←←←←←← 피드백 루프 ←←←←←←←←←←←←←←
```

## 프로젝트 구조 설계

### 권장 디렉터리 구조

```
project-root/
├── todo.md                    # 메인 TODO 리스트
├── docs/                      # 프로젝트 문서
│   ├── requirements.md        # 요구사항 정의
│   ├── architecture.md        # 아키텍처 설계
│   └── api-spec.md           # API 사양서
├── src/                       # 소스 코드
│   ├── models/               # 데이터 모델
│   ├── controllers/          # 컨트롤러
│   ├── services/             # 비즈니스 로직
│   └── utils/                # 유틸리티
├── tests/                     # 테스트 코드
│   ├── unit/                 # 단위 테스트
│   ├── integration/          # 통합 테스트
│   └── fixtures/             # 테스트 데이터
├── scripts/                   # 개발용 스크립트
└── README.md                 # 프로젝트 개요
```

### TODO 리스트 작성과 관리

#### TODO 리스트의 기본 형식

**todo.md 예:**
```markdown
# 프로젝트 TODO 리스트

## 현재 구현 대상
- [ ] 사용자 등록 기능 구현

## 완료
- [x] 프로젝트 초기 설정
- [x] 데이터베이스 연결 설정

## 미착수 (우선순위 순)
1. [ ] 사용자 인증 기능
   - [ ] 패스워드 해싱
   - [ ] JWT 토큰 생성
   - [ ] 로그인 API

2. [ ] 사용자 관리 기능
   - [ ] 프로필 업데이트 API
   - [ ] 사용자 삭제 API
   - [ ] 사용자 목록 API

3. [ ] 보안 강화
   - [ ] 속도 제한 구현
   - [ ] 입력값 검증 강화
   - [ ] CORS 설정

## 향후 검토
- [ ] 성능 최적화
- [ ] 배포 자동화
```

#### 효과적인 TODO 세분화 설정

**적절한 세분화 예:**
- ✅ `사용자 등록 API 구현` (30분~1시간)
- ✅ `패스워드 검증 기능` (30분~1시간)
- ✅ `JWT 인증 미들웨어` (30분~1시간)

**피해야 할 세분화:**
- ❌ `시스템 전체 구현` (너무 큼)
- ❌ `변수명 변경` (너무 작음)

### TODO 실행 전략

#### 순차 실행 접근법
```markdown
실행 방침:
1. TODO 리스트를 위에서부터 순서대로 처리
2. 하나의 항목을 완전히 완료한 후 다음으로
3. 의존 관계가 있는 항목은 순서 조정
4. 30분~1시간에 완료 가능한 단위로 분할
```

#### 의존 관계 관리
```markdown
의존 관계 예:
- 사용자 모델 → 사용자 등록 API → 사용자 인증
- 데이터베이스 설계 → 마이그레이션 → API 구현
- 기본 기능 → 에러 핸들링 → 보안 강화
```

## Git 워크플로 설정

### AITDD 지향 브랜치 전략

#### 기본적인 브랜치 모델
```bash
main                    # 프로덕션 환경용
├── develop            # 개발 통합용
└── feature/todo-item  # 각 TODO 항목용
```

#### 브랜치 생성 실례
```bash
# TODO 항목별로 브랜치 생성
git checkout -b feature/user-registration
git checkout -b feature/user-authentication
git checkout -b feature/password-validation

# 기능군으로 묶는 경우
git checkout -b feature/user-management
git checkout -b feature/security-enhancement
```

### 커밋 전략

#### AITDD 사이클에 대응하는 커밋
```bash
# Red 단계 (테스트 작성)
git add tests/
git commit -m "Red: Add tests for user registration"

# Green 단계 (구현)
git add src/
git commit -m "Green: Implement user registration functionality"

# Refactor 단계 (개선)
git add src/
git commit -m "Refactor: Improve user registration code structure"

# Validation 단계 (검증)
git add .
git commit -m "Validation: Complete user registration with documentation"
```

#### 실패 시 복구 전략
```bash
# AI가 기대한 결과를 내지 않는 경우
git reset --hard HEAD~1  # 마지막 커밋 취소
# 또는
git reset --hard <commit-hash>  # 특정 커밋까지 되돌림

# 프롬프트를 조정하여 재실행
# 성공하면 새로운 커밋
```

## 실천을 위한 Next Steps

### 환경 구축 완료 후 행동 지침

1. **3장으로의 이전 준비**
   - AITDD 프로세스의 상세 이해
   - Red-Green-Refactor-Validation 사이클 습득
   - 실제 개발 플로우 체험

2. **첫 번째 프로젝트 계획**
   - 소규모 샘플 프로젝트 설계
   - 명확한 기능 요구사항 정의
   - 구현 가능한 범위의 TODO 리스트 작성

3. **지속적 개선 준비**
   - 프롬프트 설계 스킬 향상
   - AI와의 효과적인 대화 패턴 습득
   - 리뷰와 품질 관리 실천

### 성공을 위한 중요 포인트

#### 단계적 접근법 채택
- **작게 시작**: 단순한 기능부터 시작
- **점진적 확장**: 성공 경험을 축적
- **실패에서 배우기**: git reset을 두려워하지 말고 시행착오

#### 품질에 대한 지속적 주의
- **테스트 퍼스트**: 반드시 테스트부터 작성
- **리뷰 습관화**: AI 생성 코드도 반드시 확인
- **문서화 실천**: 구현 내용을 적절히 기록

#### 팀 실천 준비
- **공통 이해 구축**: 팀 멤버와의 인식 맞춤
- **도구 통일**: 같은 개발 환경에서 작업
- **지식 공유**: 성공・실패 사례 공유

## 환경 구축 완료 확인

### 최종 체크리스트

- [ ] **기본 도구**
  - [ ] Claude Sonnet 4 (Claude Code) 이용 가능
  - [ ] VS Code가 적절히 설정됨
  - [ ] Git 리포지토리가 초기화됨

- [ ] **프로젝트 구조**
  - [ ] 권장 디렉터리 구조가 생성됨
  - [ ] todo.md 파일이 준비됨
  - [ ] 기본 설정 파일이 배치됨

- [ ] **개발 워크플로**
  - [ ] Git 브랜치 전략이 결정됨
  - [ ] 커밋 규칙이 정의됨
  - [ ] 테스트 환경이 동작 확인됨

- [ ] **문서화・모니터링**
  - [ ] README.md가 작성됨
  - [ ] 로그 설정이 완료됨
  - [ ] 디버그 환경이 준비됨

### 동작 확인 테스트

```bash
# 기본적인 동작 확인
npm test                     # 테스트 실행
npm run test:coverage        # 커버리지 확인
git status                   # Git 상태 확인
git log --oneline -5         # 최근 커밋 확인

# Claude Code와의 연동 확인
# VS Code로 프로젝트 열기
# Claude Code 플러그인이 정상 동작하는지 확인
# 간단한 테스트 케이스 작성을 AI에 의뢰하여 동작 확인
```

### 트러블슈팅

#### 자주 발생하는 문제와 해결책

**Claude Code에 연결할 수 없음**
```bash
# 인증 확인
# Pro 플랜 유효성 확인
# 네트워크 설정 확인
```

**테스트 환경에서 에러 발생**
```bash
# 의존성 재설치
npm install

# 패키지 캐시 클리어
npm cache clean --force
```

**Git 조작에서 에러**
```bash
# 인증 정보 재설정
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## 정리

2장에서는 AITDD를 실천하기 위한 포괄적인 개발 환경 구축 방법을 학습했습니다. 중요한 포인트는 다음과 같습니다:

### 주요 성과
1. **도구 설정**: Claude Sonnet 4를 중심으로 한 개발 환경 구축
2. **워크플로 설계**: TODO 관리에서 Git 플로우까지의 체계적 프로세스
3. **품질 관리 기반**: 테스트 환경, 로그, 디버그 기능 정비

### 다음 장으로의 준비
환경 구축이 완료되면 드디어 AITDD의 실제 프로세스를 학습합니다. 3장 "AITDD 프로세스 상세"에서는 Red-Green-Refactor-Validation 사이클의 구체적인 실천 방법을 습득합니다.

**학습 포인트:**
- 각 단계의 구체적인 작업 내용
- AI와의 효과적인 대화 방법
- 품질 관리와 리뷰 기법

환경이 정비된 지금, 실제로 AI와 협조하여 소프트웨어 개발을 수행할 준비가 완료되었습니다. 다음 장에서 AITDD의 진가를 체험해 봅시다.