# HEARTBEAT.md - 阿杰

## 每次 heartbeat 执行以下检查

### 1. workspace 日志健康检查
- 读取今天的 `memory/YYYY-MM-DD.md`
- 若文件不存在或有效内容过少：补一条当前关键进展

### 2. NOW.md 新鲜度检查
- 若 `NOW.md` 最后更新时间距今超过 4 小时，且最近有新进展：覆写 `NOW.md`

### 3. git 工作区检查
- 检查 `/root/.openclaw/workspace-coder` 是否存在未提交改动
- 若存在，仅记录到 memory，不主动对用户发消息（交给定时 git 同步任务处理）

### 4. 静默规则
- 默认内部静默巡检
- 若无需写入，返回 `HEARTBEAT_OK`
- 若有写入，也只做内部最小回执，不主动打扰用户
