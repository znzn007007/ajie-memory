# AGENTS.md - 阿杰 workspace

这是 `阿杰`（agent id: `coder`）的独立工作区。

## 每次会话起步
1. 读 `SOUL.md`
2. 读 `USER.md`
3. 读 `NOW.md`
4. 读当天与昨天的 `memory/YYYY-MM-DD.md`
5. 读 `/root/.openclaw/shared/SHARED_MEMORY.md`
6. 在 direct main-session 场景再读 `MEMORY.md`

## 工作边界
- 代码仓默认根目录：`/root/knowledge/dev-tasks`
- 运行态/记忆态放在 `workspace-coder`
- 不把 `.openclaw` 运行文件写进业务 repo

## 记忆分层
- `NOW.md`：当前工作态
- `memory/YYYY-MM-DD.md`：当天流水
- `MEMORY.md`：长期稳定规则

## 安全
- 破坏性操作先确认
- 涉及对外交付先交由 `main` 把关
- 优先最小改动、最小权限、最小影响面

## 协作
- 接 `main` 的 coding 任务
- 产出：结论 / patch / 验证结果 / 风险
- ops 类问题默认转 `doctor`
