# CLAUDE.md

이 파일은 이 저장소의 코드를 작업할 때 Claude Code (claude.ai/code)에 대한 지침을 제공합니다.

## 개요

Tsumiki는 AI 주도 개발 프레임워크를 위한 명령어 템플릿을 제공하는 CLI 도구입니다. 이 프로젝트는 Ink를 사용한 TypeScript + React로 구성된 CLI 애플리케이션으로, Claude Code 명령어 템플릿을 사용자의 `.claude/commands/` 디렉토리에 설치합니다.

## 개발 명령어

```bash
# 개발 환경
pnpm install                # 의존성 설치

# 빌드
pnpm build                  # 프로젝트를 빌드하고 commands 디렉토리를 dist/에 복사
pnpm build:run              # 빌드 후 CLI 실행 (테스트용)

# 코드 품질
pnpm check                  # Biome으로 코드 검사
pnpm fix                    # Biome으로 자동 수정
pnpm typecheck              # TypeScript 타입 검사 (tsgo 사용)
pnpm secretlint             # 시크릿 정보 검사

# pre-commit 훅
pnpm prepare                # simple-git-hooks 설정
```

## 프로젝트 구조

- **`src/cli.ts`**: CLI 진입점, commander를 사용한 명령어 정의
- **`src/commands/install.tsx`**: React + Ink을 사용한 설치 명령어 UI 구현
- **`commands/`**: Tsumiki의 AI 개발 프레임워크용 Claude Code 명령어 템플릿 (`.md`와 `.sh` 파일)
- **`dist/`**: 빌드 출력, 템플릿이 `dist/commands/`에 복사됨

## 기술 스택

- **CLI 프레임워크**: Commander.js
- **UI 프레임워크**: React + Ink (CLI에서의 React 렌더링)
- **빌드 도구**: tsup (TypeScript + ESBuild 기반)
- **코드 품질**: Biome (린터 및 포매터)
- **TypeScript**: tsgo (고속 타입 검사)
- **패키지 매니저**: pnpm

## 빌드 프로세스

빌드 시 (`pnpm build`) 다음 과정이 실행됩니다:
1. `dist` 디렉토리 정리
2. `dist/commands` 디렉토리 생성
3. `commands/` 내의 `.md`와 `.sh` 파일을 `dist/commands/`에 복사
4. tsup으로 TypeScript 코드를 ESM과 CJS 양쪽 형식으로 빌드

## 설치 동작

`tsumiki install` 명령어는 다음을 실행합니다:
1. 현재 디렉토리에 `.claude/commands/` 디렉토리 생성
2. 빌드된 `dist/commands/`에서 모든 `.md`와 `.sh` 파일 복사
3. React + Ink으로 진행 상황과 파일 목록 표시

## 품질 관리

Pre-commit 훅에서 다음이 자동 실행됩니다:
- `pnpm secretlint`: 기밀 정보 검사
- `pnpm typecheck`: 타입 검사
- `pnpm fix`: 코드 자동 수정

코드 수정 시 반드시 `pnpm check`와 `pnpm typecheck`를 실행한 후 커밋하세요.