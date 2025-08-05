# 5.2 AI 추론의 가시화 기술

## 들어가며

AI 생성 코드의 품질 관리에서 가장 중요한 것은 AI가 어느 부분을 "추측"했는지 명확히 파악하는 것입니다. 신호등 시스템을 이용한 AI 추론의 가시화 기술로 효율적인 검토와 높은 품질 보증을 실현할 수 있습니다.

## 신호등 시스템의 이론적 배경

### 과제 특정

AI 생성 코드는 다음과 같은 특성이 있습니다:

- **자동 완성의 광범위성**: AI는 명시되지 않은 부분도 자동으로 보완
- **그럴듯함의 함정**: 생성 내용이 타당해 보이지만 실제 의도와 다를 수 있음
- **추측 근거의 불명확성**: 어떤 정보를 기반으로 생성되었는지 불명확

### 해결 접근법

신호등 시스템은 AI 생성 내용을 다음 기준으로 분류하여 검토 우선순위를 명확히 합니다:

```
🟢 초록불 → 🟡 노란불 → 🔴 빨간불
   안전       주의        위험
```

## 신호등 시스템의 상세 정의

### 🟢 초록불(높은 확신도·안전)

**정의:** 참조한 원본 파일에서 명확히 추측할 수 있는 내용

**특징:**
- 원래 지시나 사양서에 명기된 내용에 기반한 생성
- 기존 코드 패턴을 따른 구현
- 명확한 근거가 있는 구현 판단

**구체적 예시:**
```javascript
// 사양서에 "사용자 ID는 필수"라고 명기된 경우
function validateUser(userId) {
  if (!userId) {  // 🟢 사양서에서 명확히 도출
    throw new Error('User ID is required');
  }
}
```

**검토 우선순위:** 낮음
**확인 포인트:** 구현의 정확성, 성능 영향

### 🟡 노란불(중간 확신도·주의)

**정의:** 참조한 원본 파일에는 없지만 타당하다고 생각되는 내용

**특징:**
- AI의 합리적인 추측에 의한 보완
- 일반적인 베스트 프랙티스에 기반한 구현
- 도메인 지식을 활용한 추론

**구체적 예시:**
```javascript
// 사양서에 세부 사항이 없는 경우의 에러 핸들링
function processData(data) {
  try {
    return transform(data);
  } catch (error) {  // 🟡 일반적이지만 확인 필요
    console.error('Data processing failed:', error);
    return null;
  }
}
```

**검토 우선순위:** 높음
**확인 포인트:** 추측의 타당성, 비즈니스 요구사항과의 정합성

### 🔴 빨간불(판단 필요·위험)

**정의:** 참조한 원본 파일에 없고 직접 추측도 할 수 없는 내용

**특징:**
- AI의 독자 판단에 의한 생성
- 조직 고유의 관습이나 규칙 가정
- 명확한 근거 없는 구현 선택

**구체적 예시:**
```javascript
// 조직의 로그 형식에 대한 정보가 없는 경우
function logUserAction(action) {
  // 🔴 로그 형식은 조직 고유, 확인 필요
  logger.info(`[AUDIT] User performed: ${action} at ${new Date().toISOString()}`);
}
```

**검토 우선순위:** 최고
**확인 포인트:** 조직 규칙과의 정합성, 보안 영향

## 구현 방법과 TODO 파일 형식

### 표준 TODO 파일 형식

```markdown
## [단계명]결과 TODO

### 🟢 높은 확신도 항목
- [ ] [utils.js](./src/utils.js)의 타입 정의가 사양서와 일치하는지 확인
- [ ] [validation.js](./src/validation.js)의 필수 항목 체크 구현 확인

### 🟡 중간 확신도 항목
- [ ] [error-handler.js](./src/error-handler.js)의 에러 응답 형식의 타당성 확인
- [ ] [config.js](./src/config.js)의 기본값 설정의 조직 정책 적합성

### 🔴 판단 필요 항목
- [ ] 상세 확인: [logger.js](./src/logger.js)의 로그 출력 형식이 조직 표준 준수
- [ ] 상세 확인: [auth.js](./src/auth.js)의 세션 관리 방식 선택 근거
```

### 프롬프트에서의 지시 방법

**기본 지시 템플릿:**
```markdown
## AI 추론 가시화 지시

다음 작업을 실행하고, 생성 내용을 신호등 시스템으로 분류해주세요:

**작업 내용:**
[구체적인 태스크 내용]

**분류 기준:**
- 🟢 초록불: 참조 파일([파일명])에서 명확히 도출 가능
- 🟡 노란불: 합리적 추측이지만 참조 파일에 명기 없음
- 🔴 빨간불: 독자 판단에 의한 생성(조직 고유 내용 등)

**출력 파일:** `./todos/[단계명]-inference-check.md`

**출력 형식:**
각 생성 항목에 신호등 마크를 부여하고, TODO 형식으로 체크 항목 작성
```

### 참조원 파일 관리 방법

**파일 관계성 기록:**
```markdown
## 참조 파일 관리

**주요 참조(Primary References):**
- [`requirements.md`](./docs/requirements.md) - 기본 요구사항 정의
- [`api-spec.yaml`](./docs/api-spec.yaml) - API 사양

**보조 참조(Secondary References):**
- [`existing-code/`](./src/existing/) - 기존 구현 패턴
- [`config-samples/`](./config/) - 설정 파일 예시

**외부 참조(External References):**
- 기술 문서(프레임워크 공식)
- 업계 표준(RFC, W3C 등)

**추론 근거 추적:**
- 🟢항목 → 주요 참조에 명기
- 🟡항목 → 보조 참조＋일반 지식
- 🔴항목 → 근거 불명·독자 판단
```

## 체크 우선순위 설정

### 우선순위 매트릭스

| 신호 | 영향도·높음 | 영향도·중간 | 영향도·낮음 |
|------|-----------|-----------|-----------|
| 🔴 빨간불 | **최우선** | 높은 우선 | 중간 우선 |
| 🟡 노란불 | 높은 우선 | 중간 우선 | 낮은 우선 |
| 🟢 초록불 | 중간 우선 | 낮은 우선 | **나중에** |

### 영향도 평가 기준

**영향도·높음:**
- 보안과 관련된 구현
- 데이터 정합성에 영향
- 시스템 전체 동작에 영향

**영향도·중간:**
- 특정 기능 동작에 영향
- 사용자 경험에 영향
- 성능에 영향

**영향도·낮음:**
- 로그 출력이나 코멘트
- 내부적인 변수명
- 보조적인 기능

### 실천적인 체크 순서

1. **🔴×영향도·높음** - 즉시 확인·수정
2. **🔴×영향도·중간** 및 **🟡×영향도·높음** - 다음 작업 시작 전 확인
3. **기타 🔴항목** - 구현 완료 전 반드시 확인
4. **🟡항목** - 검토 시 확인
5. **🟢항목** - 최종 체크 시 확인

## 실제 운영 예시와 케이스 스터디

### 케이스 스터디 1: REST API 구현

**시나리오:** 사용자 관리 API 구현
**참조 파일:** `user-api-spec.yaml`, `existing-user-model.js`

**AI 생성 결과의 분류:**

```javascript
// 🟢 API 사양서에 명기된 엔드포인트
app.post('/api/users', async (req, res) => {
  
  // 🟡 일반적인 검증이지만 사양서에 세부사항 없음
  if (!req.body.email || !req.body.password) {
    return res.status(400).json({ error: 'Email and password required' });
  }
  
  // 🔴 해시화 알고리즘이 조직 고유 정책에 의존
  const hashedPassword = bcrypt.hashSync(req.body.password, 12);
  
  // 🟢 기존 모델 패턴을 따른 구현
  const user = new User({
    email: req.body.email,
    password: hashedPassword
  });
});
```

**생성된 TODO:**
```markdown
## API 구현 결과 TODO

### 🟢 높은 확신도 항목
- [ ] [user-controller.js](./src/controllers/user.js)의 엔드포인트 정의가 사양서와 일치
- [ ] [user-model.js](./src/models/user.js)의 기존 패턴 준수 확인

### 🟡 중간 확신도 항목
- [ ] [validation.js](./src/middleware/validation.js)의 에러 메시지 형식의 타당성
- [ ] [user-controller.js](./src/controllers/user.js)의 상태 코드 선택 확인

### 🔴 판단 필요 항목
- [ ] 상세 확인: [auth.js](./src/utils/auth.js)의 bcrypt 솔트 라운드 수가 조직 정책 준수
```

### 케이스 스터디 2: 테스트 케이스 생성

**시나리오:** 위 API의 테스트 케이스 작성
**참조 파일:** `user-api-spec.yaml`, `existing-test-patterns.js`

**분류 결과:**
```javascript
describe('User API', () => {
  // 🟢 사양서에 명기된 테스트 케이스
  it('should create user with valid email and password', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', password: 'password123' });
    
    expect(response.status).toBe(201);  // 🟢 사양서대로
  });
  
  // 🟡 일반적인 엣지 케이스(사양서에 명기 없음)
  it('should reject invalid email format', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'invalid-email', password: 'password123' });
    
    expect(response.status).toBe(400);  // 🟡 추측에 의함
  });
  
  // 🔴 조직 고유 보안 요구사항에 의한 추측
  it('should enforce password complexity requirements', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', password: '123' });
    
    expect(response.status).toBe(400);  // 🔴 조직 정책 의존
  });
});
```

## 효과 측정과 개선 방법

### 효과 측정 지표

**정량 지표:**
- 검토 시간 단축률
- 버그 발견률 향상
- 수정 횟수 감소

**정성 지표:**
- 검토의 효율성 향상
- 중요한 문제의 놓침 방지
- 개발자의 안심감 향상

### 운영 데이터 예시

```markdown
## 신호등 시스템 도입 효과(1개월간)

**기존 검토:**
- 평균 검토 시간: 45분/기능
- 버그 발견률: 약 60%
- 검토 놓침: 월 3-4건

**신호등 시스템 도입 후:**
- 평균 검토 시간: 25분/기능(44% 단축)
- 버그 발견률: 약 85%(25% 향상)
- 검토 놓침: 월 1건 이하

**🔴항목의 전형적인 문제:**
- 보안 관련: 40%
- 조직 정책 위반: 35%
- 설정·환경 의존: 25%
```

### 지속적 개선 접근법

**1. 분류 정확도 향상:**
```markdown
## 분류 기준 개선 로그

**1-2주차:**
- 문제: 로그 형식의 분류가 모호
- 개선: 조직 고유 항목의 체크리스트 작성

**3-4주차:**
- 문제: 에러 핸들링의 분류가 불안정
- 개선: 에러 핸들링 패턴의 명문화

**2개월차:**
- 문제: 새 기술 스택에서의 분류 곤란
- 개선: 기술 스택별 가이드라인 작성
```

**2. 프롬프트 최적화:**
```markdown
## 프롬프트 개선 사이클

**개선 전 과제:**
- 🔴항목의 검출률이 70% 정도
- 분류에 일관성이 없음

**개선 내용:**
- 조직 고유 항목의 명확한 리스트 제공
- 판단 기준의 구체적 예시를 풍부하게 추가

**개선 후 효과:**
- 🔴항목의 검출률이 90% 이상으로 향상
- 분류의 일관성이 대폭 개선
```

## 실천적인 도입 단계

### Step 1: 기본 시스템 구축

```bash
# 프로젝트 구조 준비
mkdir -p todos
mkdir -p docs/inference-guides

# 기본 템플릿 작성
cat > docs/inference-guides/classification-template.md << 'EOF'
## AI 추론 분류 템플릿

### 🟢 초록불 판정 기준
- 참조 파일 "[파일명]"에 명기된 내용
- 기존 코드의 확립된 패턴을 따르는 구현

### 🟡 노란불 판정 기준  
- 일반적인 베스트 프랙티스에 기반한 구현
- 기술적으로 타당하지만 참조 파일에 명기 없음

### 🔴 빨간불 판정 기준
- 조직 고유의 정책이나 관습에 의존
- 명확한 근거 없는 독자 판단
EOF
```

### Step 2: 조직 고유 규칙의 명문화

```markdown
## 조직 고유 체크포인트

**보안 관련:**
- [ ] 패스워드 해시화 알고리즘과 강도
- [ ] 세션 관리 방식
- [ ] API 인증 방식

**로그·감사 관련:**
- [ ] 로그 출력 형식과 레벨
- [ ] 감사 로그의 출력 항목
- [ ] 로그 보존 기간과 로테이션

**코딩 규약:**
- [ ] 명명 규칙(변수, 함수, 클래스)
- [ ] 에러 핸들링 패턴
- [ ] 코멘트 기술 규칙
```

### Step 3: 팀 운영 확립

```markdown
## 팀 운영 규칙

**분류 작업의 담당:**
- AI 실행자가 초기 분류
- 검토자가 분류의 타당성 확인

**체크 작업의 분담:**
- 🔴항목: 시니어 엔지니어가 확인
- 🟡항목: 팀 내 페어 리뷰  
- 🟢항목: 자동 테스트＋가벼운 확인

**지식 축적:**
- 주 1회 분류 기준 재검토 회의
- 오분류 패턴 공유
- 개선 사례 문서화
```

## 문제 해결

### 자주 발생하는 문제와 대처법

**문제 1: 분류가 일관되지 않음**

```markdown
**증상:** 같은 내용이라도 분류 결과가 다름
**원인:** 분류 기준이 모호, 프롬프트 지시가 불명확
**대처법:**
1. 조직 고유 항목의 더 구체적인 리스트 작성
2. 과거 분류 예시를 프롬프트에 포함
3. 판단 망설임 사항의 기록과 기준화
```

**문제 2: 🔴항목 놓침**

```markdown
**증상:** 중요한 조직 고유 항목이 🟡이나 🟢로 분류됨
**원인:** AI가 조직의 컨텍스트를 이해하지 못함
**대처법:**
1. 조직 고유 항목의 명시적인 리스트 제공
2. "의심스러운 경우는 🔴로 분류" 원칙 설정
3. 검토자에 의한 분류 타당성 체크
```

**문제 3: TODO 항목이 너무 많음**

```markdown
**증상:** 생성되는 TODO 항목 수가 실행 가능한 범위를 초과
**원인:** 분류가 너무 세밀, 영향도 평가가 관대
**대처법:**
1. 유사 항목의 그룹화
2. 영향도 평가의 엄격화
3. "중요도×긴급도" 매트릭스 도입
```

## 실천 연습

### 연습 1: 분류 기준 작성

다음 코드 스니펫을 신호등 시스템으로 분류해주세요:

```javascript
function authenticateUser(username, password) {
  // 케이스 1: 사용자 존재 체크
  const user = await User.findOne({ username });
  if (!user) {
    return { success: false, message: 'User not found' };
  }
  
  // 케이스 2: 패스워드 대조
  const isValid = await bcrypt.compare(password, user.hashedPassword);
  if (!isValid) {
    return { success: false, message: 'Invalid password' };
  }
  
  // 케이스 3: JWT 토큰 생성
  const token = jwt.sign(
    { userId: user.id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
  
  // 케이스 4: 로그 출력
  console.log(`User ${username} authenticated successfully at ${new Date().toISOString()}`);
  
  return { success: true, token };
}
```

**참조 파일:** `auth-spec.md`(기본 인증 플로우만 기재)

### 연습 2: TODO 항목의 우선순위 설정

위 분류 결과를 바탕으로 TODO 항목을 우선순위대로 정렬해주세요.

## 정리

AI 추론의 가시화 기술로 다음 효과를 실현할 수 있습니다:

1. **효율적인 검토**: 중요한 부분에 집중한 품질 체크
2. **리스크 경감**: 고위험 추측 부분의 확실한 발견
3. **지식 축적**: 조직 고유 판단 기준의 명문화와 공유
4. **지속 개선**: 데이터에 기반한 분류 기준과 프롬프트 최적화

신호등 시스템은 AITDD에서 인간 요소와 AI 지원의 균형을 맞추는 중요한 기술입니다. 다음 섹션에서는 이러한 기술을 활용한 지속적 개선과 프롬프트 최적화에 대해 학습합니다.