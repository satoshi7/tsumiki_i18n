# 개발 서버 시작・관리

개발 환경의 서버를 시작・관리하는 명령어입니다.

## 서버 시작 확인・관리

개발 시작 전에 서버 상태를 확인하고, 필요에 따라 시작합니다：

```bash
# 기존 Vite 서버 확인
ps aux | grep -E "vite.*--port 3000" | grep -v grep

# 서버가 시작되지 않은 경우 신규 시작
if ! ps aux | grep -E "vite.*--port 3000" | grep -v grep > /dev/null; then
  echo "서버가 시작되지 않았습니다. 개발 서버를 시작합니다..."
  npm run dev &
  echo "서버 시작 중... 5초 대기합니다"
  sleep 5
else
  echo "기존 서버를 찾았습니다. 그대로 이용합니다."
  ps aux | grep -E "vite.*--port 3000" | grep -v grep | awk '{print "PID: " $2 " - Vite서버가 이미 시작 중"}'
fi

# 서버 동작 확인
echo "서버 동작 확인 중..."
curl -s http://localhost:3000 > /dev/null && echo "✅ 서버는 정상적으로 동작하고 있습니다" || echo "⚠️ 서버에 연결할 수 없습니다"
```

## 서버 관리 명령어

### 서버 상태 확인

```bash
# 현재 동작 중인 서버 프로세스 확인
ps aux | grep -E "vite.*--port 3000" | grep -v grep

# 포트 사용 상황 확인
lsof -i :3000
```

### 서버 정지

```bash
# Vite 서버 정지
pkill -f "vite.*--port 3000"

# 강제 정지 (위 방법으로 정지되지 않는 경우)
ps aux | grep -E "vite.*--port 3000" | grep -v grep | awk '{print $2}' | xargs kill -9
```

### 서버 재시작

```bash
# 서버 정지
pkill -f "vite.*--port 3000"

# 잠시 대기
sleep 2

# 서버 재시작
npm run dev &

# 시작 확인
sleep 5
curl -s http://localhost:3000 > /dev/null && echo "✅ 서버는 정상적으로 동작하고 있습니다" || echo "⚠️ 서버에 연결할 수 없습니다"
```

## 사용 장면

- TDD 개발 시작 전 환경 준비
- 서버가 정지된 경우 복구
- 서버 상태 확인이 필요한 경우
- 개발 환경 설정 시

## 주의사항

- 포트 3000이 다른 프로세스에 사용되고 있는 경우, 해당 프로세스를 종료해 주세요
- 서버 시작 후, 브라우저에서 http://localhost:3000 에 접근하여 동작 확인할 수 있습니다
- 백그라운드에서 시작한 서버는 작업 종료 시 적절히 정지하는 것을 권장합니다