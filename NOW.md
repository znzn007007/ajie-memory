# NOW.md — 阿杰 当前工作态

## 当前优先级（进行中）
- 已完成 `coder` agent phase-1 基础工作区搭建
- 已配置 HEARTBEAT.md 监控机制（30 分钟巡检，静默模式）
- 已固定代码仓根目录：`/root/knowledge/dev-tasks`
- Telegram 入口通过主实例 agents.list 接入

## 2026-04-03 当天进展
- [08:40 UTC~09:04 UTC] 在 GCP sub2api-prod 上成功部署 CLIProxyAPI（CPA）：
  - 实例：sub2api-prod (zone: us-west1-b, project: stalwart-elixir-490811-q6)
  - 部署形态：二进制 + systemd（服务：cliproxyapi.service）
  - PostgreSQL：复用现有实例，新建独立 schema cliproxy
  - 配置文件：/opt/cliproxy/.env、/opt/cliproxy/config.yaml
  - API 密钥：已生成并通过 curl /v1/models 验证（200 OK）
  - 管理密钥：已生成并写入 config_store
  - 远程管理：已启用（allow-remote: true）
  - 官方 WebUI：本地验证 /management.html 正常（200 OK）
  - Cloudflare Tunnel：由用户自行配置（指向 http://127.0.0.1:8317）
- [10:47 UTC] 完成 sub2api 首 Token 高延迟事故源码分析与原因报告（via 子代理深查）：
  - 根因：WSv2 链路 + previous_response_id 恢复 + store=false strict 模式导致的首 Token 叠高
  - 源码证据链路：handler → scheduler → Forward → WSv2 acquire/dial/retry
  - 默认配置：responses_websockets_v2=true、store_disabled_conn_mode=strict、TTFT 权重=0.5
  - 修复建议：改 store_disabled_conn_mode=adaptive、提高 TTFT 权重、缩短 sticky TTL
- [07:50 UTC] 完成 sub2api 事故修复方案（纯配置级）：提供 4 套配置方案（保守/激进/局部），按优先级排序：
  - 方案 1（推荐）：最小配置改法 - store_disabled_conn_mode: adaptive + sticky_ttl: 600 + ttft: 1.2
  - 方案 2：保留 WSv2 偏性能优化 - prewarm_generate_enabled: true + min_idle: 8 + max_idle: 16
  - 方案 3：临时回退 HTTP 做对照实验 - force_http: true
  - 方案 4：按账号分层配置 - 对慢账号单独 force_http 或关闭 WSv2
- [10:48 UTC] Heartbeat：更新 NOW.md 新鲜度、检查 workspace 日志、记录 git 工作区状态
- [10:10 UTC] Heartbeat：NOW.md 新鲜度更新（记录 CLIProxyAPI 部署进展），git 工作区存在未提交改动（已记录，不主动发消息）
- [11:10 UTC] Codex auth 迁移完成：
  - 从 sub2api.public.accounts 迁移 Codex OAuth 认证到 CPA cliproxy.auth_store
  - 成功迁移 10 条 Codex auth 记录（自动去重，每个 email 保留最新一条）
  - CPA 服务已重启，模型列表验证通过（/v1/models 返回 13 个模型）
  - 迁移格式：按 CPA CodexTokenStorage 源码结构组装（id_token、access_token、refresh_token、email、type=codex、expired）
  - 目标文件名：codex-<email>.json
- [12:10 UTC~21:05 UTC] 确认 sub2api-prod 上可用 Docker Compose 环境（方案 B 环境调研）：
  - 核实远端环境：Docker / docker-compose 可用，3000 端口空闲
  - 核实宿主机资源：PostgreSQL (5432) 和 Redis (6379) 已在运行
  - 核实仓库架构：支持 SQLite / MySQL / PostgreSQL，默认 compose 为 new-api+postgres+redis 三件套
  - 输出部署草案：复用宿主机 PG/Redis，只起 new-api 容器，提供 .env 模板和 compose 配置
  - 输出内容：提供 docker-compose.yml 配置方案（服务定义、卷挂载、网络隔离）、提供 .env 模板（包含数据库连接、服务配置）

## 2026-03-30~2026-04-01 历史进展（参考）
- 2026-03-30 14:49 UTC：完成 GCP 机器 sub2api 性能排查（via gcloud ssh）：机器资源宽裕、服务运行稳定、延迟来自上游/代理链路而非本地瓶颈，建议先排查账号质量/上游路由后再考虑扩容
- 2026-03-31 14:49 UTC：完成 GCP sub2api 机器性能与错误排查（14:49 UTC）：确认慢请求主要原因在上游链路与无效账号 failover，而非机器资源或本地前置逻辑；建议优先清理坏账号、优化账号选择策略
- 2026-04-01 16:30 UTC：完成 instance-20260320-121921 的 PostgreSQL 安装配置（16:30 UTC）

## 当前阻塞
- 无硬阻塞

## 下一步
- 接收 `main` 派发的新 coding 任务
- 遵循破坏性操作先确认原则
- 运维/部署类问题优先交由 `doctor`

---
*最后更新：2026-04-03 14:20 UTC*
