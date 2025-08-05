# Tsumiki - AI 기반 개발 프레임워크

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

🌐 언어: [English](../en/README.md) | [日本語](../ja/README.md) | [中文](../zh/README.md) | **한국어**

Tsumiki는 AI를 활용하여 요구사항 정의부터 구현까지 효율적인 개발 프로세스를 제공하는 AI 기반 개발 프레임워크입니다.

## 설치

Tsumiki를 사용하려면 다음 npx 명령으로 설치하세요:

```bash
npx tsumiki install
```

이 명령은 Tsumiki의 Claude Code 슬래시 명령을 `.claude/commands/`에 설치합니다.

## 개요

Tsumiki는 두 가지 명령 세트로 구성됩니다:

- **kairo** - 요구사항부터 구현까지의 포괄적인 개발 흐름
- **tdd** - 개별 테스트 주도 개발(TDD) 실행

### Kairo 명령

Kairo는 요구사항 정의부터 구현까지의 개발 프로세스를 자동화하고 지원합니다:

1. **요구사항 정의** - 개요에서 상세한 요구사항 문서 생성
2. **설계** - 기술 설계 문서 자동 생성
3. **작업 분할** - 구현 작업을 적절히 분할하고 순서 지정
4. **TDD 구현** - 테스트 주도 개발을 통한 고품질 구현

## 사용 가능한 명령

### Kairo 명령 (포괄적 개발 흐름)
- `kairo-requirements` - 요구사항 정의
- `kairo-design` - 설계 문서 생성
- `kairo-tasks` - 작업 분할
- `kairo-implement` - 구현 실행

### TDD 명령 (개별 실행)
- `tdd-requirements` - TDD 요구사항 정의
- `tdd-testcases` - 테스트 케이스 생성
- `tdd-red` - 테스트 구현 (Red)
- `tdd-green` - 최소 구현 (Green)
- `tdd-refactor` - 리팩토링
- `tdd-verify-complete` - TDD 완료 확인

### 리버스 엔지니어링 명령
- `rev-tasks` - 기존 코드에서 작업 목록 생성
- `rev-design` - 기존 코드에서 설계 문서 생성
- `rev-specs` - 기존 코드에서 테스트 사양 생성
- `rev-requirements` - 기존 코드에서 요구사항 문서 생성

## 빠른 시작

### 포괄적 개발 흐름

```bash
# 1. 요구사항 정의
/kairo-requirements

# 2. 설계
/kairo-design

# 3. 작업 분할
/kairo-tasks

# 4. 구현
/kairo-implement
```

### 개별 TDD 프로세스

```bash
/tdd-requirements
/tdd-testcases
/tdd-red
/tdd-green
/tdd-refactor
/tdd-verify-complete
```

### 리버스 엔지니어링

```bash
# 1. 기존 코드에서 작업 구조 분석
/rev-tasks

# 2. 설계 문서 생성 (작업 분석 후 권장)
/rev-design

# 3. 테스트 사양 생성 (설계 문서 후 권장)
/rev-specs

# 4. 요구사항 문서 생성 (모든 분석 완료 후 권장)
/rev-requirements
```

### 개발 환경 정리

```bash
# 개발 환경 정리
/clear
```

## 상세 문서

- [매뉴얼](./MANUAL.md) - 자세한 사용법, 디렉토리 구조, 워크플로 예제
- [기여 가이드](./CONTRIBUTING.md) - 프로젝트 기여 방법
- [Claude.md](./CLAUDE.md) - Claude Code 사용 가이드

## 라이선스

MIT 라이선스 - 자세한 내용은 [LICENSE](../../LICENSE)를 참조하세요.

## 커뮤니티

- [GitHub Issues](https://github.com/classmethod/tsumiki/issues)
- [토론](https://github.com/classmethod/tsumiki/discussions)