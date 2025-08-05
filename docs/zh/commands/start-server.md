# 开发服务器启动・管理

用于启动和管理开发环境服务器的命令。

## 服务器启动确认・管理

开发开始前确认服务器状态，必要时启动：

```bash
# 确认现有Vite服务器
ps aux | grep -E "vite.*--port 3000" | grep -v grep

# 如果服务器未启动则新启动
if ! ps aux | grep -E "vite.*--port 3000" | grep -v grep > /dev/null; then
  echo "服务器未启动。启动开发服务器..."
  npm run dev &
  echo "服务器启动中... 等待5秒"
  sleep 5
else
  echo "发现现有服务器。将继续使用。"
  ps aux | grep -E "vite.*--port 3000" | grep -v grep | awk '{print "PID: " $2 " - Vite服务器已运行中"}'
fi

# 服务器动作确认
echo "服务器动作确认中..."
curl -s http://localhost:3000 > /dev/null && echo "✅ 服务器正常运行" || echo "⚠️ 无法连接服务器"
```

## 服务器管理命令

### 服务器状态确认

```bash
# 确认当前运行中的服务器进程
ps aux | grep -E "vite.*--port 3000" | grep -v grep

# 端口使用状况确认
lsof -i :3000
```

### 服务器停止

```bash
# Vite服务器停止
pkill -f "vite.*--port 3000"

# 强制停止（上述方法无效时）
ps aux | grep -E "vite.*--port 3000" | grep -v grep | awk '{print $2}' | xargs kill -9
```

### 服务器重启

```bash
# 服务器停止
pkill -f "vite.*--port 3000"

# 稍等片刻
sleep 2

# 服务器重启
npm run dev &

# 启动确认
sleep 5
curl -s http://localhost:3000 > /dev/null && echo "✅ 服务器正常运行" || echo "⚠️ 无法连接服务器"
```

## 使用场景

- TDD开发开始前的环境准备
- 服务器停止时的恢复
- 需要服务器状态确认时
- 开发环境设置时

## 注意事项

- 如果端口3000被其他进程使用，请终止相关进程
- 服务器启动后，可通过浏览器访问 http://localhost:3000 进行动作确认
- 作业结束时建议适当停止后台启动的服务器