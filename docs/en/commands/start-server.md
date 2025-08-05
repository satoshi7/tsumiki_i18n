# Development Server Startup & Management

Commands for starting and managing development environment servers.

## Server Startup Verification & Management

Check server status before development and start if necessary:

```bash
# Check existing Vite server
ps aux | grep -E "vite.*--port 3000" | grep -v grep

# Start new server if not running
if ! ps aux | grep -E "vite.*--port 3000" | grep -v grep > /dev/null; then
  echo "Server not running. Starting development server..."
  npm run dev &
  echo "Server starting... waiting 5 seconds"
  sleep 5
else
  echo "Found existing server. Will use it."
  ps aux | grep -E "vite.*--port 3000" | grep -v grep | awk '{print "PID: " $2 " - Vite server already running"}'
fi

# Server operation verification
echo "Checking server operation..."
curl -s http://localhost:3000 > /dev/null && echo "✅ Server is running normally" || echo "⚠️ Cannot connect to server"
```

## Server Management Commands

### Server Status Check

```bash
# Check currently running server processes
ps aux | grep -E "vite.*--port 3000" | grep -v grep

# Check port usage
lsof -i :3000
```

### Server Stop

```bash
# Stop Vite server
pkill -f "vite.*--port 3000"

# Force stop (if above doesn't work)
ps aux | grep -E "vite.*--port 3000" | grep -v grep | awk '{print $2}' | xargs kill -9
```

### Server Restart

```bash
# Stop server
pkill -f "vite.*--port 3000"

# Wait briefly
sleep 2

# Restart server
npm run dev &

# Verify startup
sleep 5
curl -s http://localhost:3000 > /dev/null && echo "✅ Server is running normally" || echo "⚠️ Cannot connect to server"
```

## Usage Scenarios

- Environment preparation before TDD development
- Recovery when server is stopped
- When server status check is needed
- During development environment setup

## Notes

- If port 3000 is used by other processes, please terminate the relevant process
- After server startup, you can verify operation by accessing http://localhost:3000 in browser
- It's recommended to properly stop background servers when work is finished