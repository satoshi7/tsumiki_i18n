# 기여 가이드

Tsumiki 프로젝트에 기여해 주셔서 감사합니다! 이 가이드는 프로젝트에 기여하는 방법을 설명합니다.

## 개발 환경 설정

### 필요한 환경

- Node.js 18.0.0 이상
- pnpm 10.13.1 이상

### 설정 단계

1. 저장소를 포크하고 클론합니다:

```bash
git clone https://github.com/YOUR_USERNAME/tsumiki.git
cd tsumiki
```

2. 의존성을 설치합니다:

```bash
pnpm install
```

3. pre-commit 훅을 설정합니다:

```bash
pnpm prepare
```

## 개발 워크플로우

### 브랜치 전략

- `main` 브랜치: 안정 버전 코드
- 기능 개발: `feature/기능명`
- 버그 수정: `bugfix/버그명`
- 핫픽스: `hotfix/수정내용`

### 개발 절차

1. 새 브랜치를 생성합니다:

```bash
git checkout -b feature/your-feature-name
```

2. 코드를 변경합니다

3. 코드 품질 검사를 실행합니다:

```bash
# 타입 검사
pnpm typecheck

# 코드 검사
pnpm check

# 자동 수정
pnpm fix

# 기밀 정보 검사
pnpm secretlint
```

4. 빌드 테스트를 실행합니다:

```bash
pnpm build:run
```

5. 변경사항을 커밋합니다:

```bash
git add .
git commit -m "feat: 새 기능 추가"
```

## 커밋 메시지 규약

[Conventional Commits](https://www.conventionalcommits.org/) 형식을 사용하세요:

- `feat:` 새 기능
- `fix:` 버그 수정
- `docs:` 문서 변경
- `style:` 코드 스타일 변경 (기능에 영향 없음)
- `refactor:` 리팩토링
- `test:` 테스트 추가·수정
- `chore:` 빌드 프로세스나 도구 변경

예시:
```
feat: add new install command for .sh files
fix: resolve path handling issue in install command
docs: update README with new command examples
```

## 코드 품질 기준

### 자동 검사

Pre-commit 훅에서 다음이 자동 실행됩니다:

- **secretlint**: 기밀 정보 (API 키, 패스워드 등) 혼입 검사
- **typecheck**: TypeScript 타입 검사
- **fix**: Biome을 통한 코드 자동 수정

### 수동 검사

변경 전에 다음 명령어를 실행하세요:

```bash
# 모든 검사 실행
pnpm typecheck && pnpm check && pnpm secretlint

# 코드 자동 수정
pnpm fix
```

## 프로젝트 구조

```
tsumiki/
├── src/
│   ├── cli.ts              # CLI 진입점
│   └── commands/
│       └── install.tsx     # 설치 명령어 UI 구현
├── commands/               # 명령어 템플릿 (.md, .sh)
├── dist/                  # 빌드 출력
├── package.json
├── CLAUDE.md              # 프로젝트 지시서
└── README.md
```

## 풀 리퀘스트

### 풀 리퀘스트 생성

1. 변경사항을 푸시합니다:

```bash
git push origin feature/your-feature-name
```

2. GitHub에서 풀 리퀘스트를 생성합니다

3. 풀 리퀘스트 템플릿에 따라 설명을 작성합니다

### 풀 리퀘스트 요구사항

- [ ] 변경 내용이 명확히 설명되어 있음
- [ ] 관련 Issue가 연결되어 있음 (해당하는 경우)
- [ ] 코드 품질 검사가 통과함
- [ ] 빌드가 성공함
- [ ] 기밀 정보가 포함되지 않음

## Issue 보고

버그 보고와 기능 요청은 [Issues](https://github.com/classmethod/tsumiki/issues)에서 받습니다.

### 버그 보고

다음 정보를 포함해 주세요:

- 재현 단계
- 예상되는 동작
- 실제 동작
- 환경 정보 (OS, Node.js 버전 등)
- 오류 메시지 (해당하는 경우)

### 기능 요청

다음 정보를 포함해 주세요:

- 제안하는 기능의 설명
- 사용 사례
- 예상되는 이익
- 구현 방안 (있다면)

## 보안

보안 관련 문제를 발견한 경우, 공개 Issue가 아닌 비공개로 보고해 주세요.

## 라이선스

이 프로젝트는 MIT 라이선스 하에 공개됩니다. 기여할 때 이 라이선스에 동의한 것으로 간주됩니다.

## 질문 & 지원

- [Issues](https://github.com/classmethod/tsumiki/issues) - 버그 보고, 기능 요청
- [Discussions](https://github.com/classmethod/tsumiki/discussions) - 질문, 토론

여러분의 기여를 기다리고 있습니다!