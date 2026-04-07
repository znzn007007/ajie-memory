# TOOLS.md - 阿杰 local notes

## Repo Root
- `/root/knowledge/dev-tasks`：默认代码仓根目录

## Agent Paths
- workspace: `/root/.openclaw/workspace-coder`
- agentDir: `/root/.openclaw/agents/coder/agent`

## Collaboration
- `main`：总控 / 对话入口 / 任务派发
- `doctor`：运维与系统问题

## GCP / sub2api-prod 约定
- 涉及 `sub2api-prod` 的 GCP 操作，先确认固定三元组：
  - project: `stalwart-elixir-490811-q6`
  - zone: `us-west1-b`
  - SSH 用户优先：`znzn007007`
- 必须先做“最小登录验证”，再做正式操作：
  - `gcloud compute ssh znzn007007@sub2api-prod --zone us-west1-b --project stalwart-elixir-490811-q6 --command='whoami && hostname'`
- 不要直接依赖 `gcloud compute ssh sub2api-prod ...` 的默认登录用户推断；当前环境可能会错误落到 `root@<ip>`。
- 若登录失败，先排查顺序：
  1. `gcloud auth list`
  2. `gcloud config list`
  3. `gcloud compute ssh ... --dry-run`（确认实际 SSH 用户）
  4. `gcloud compute ssh ... --troubleshoot`
- 对 `sub2api-prod` 的 systemd 巡检优先命令：
  - `systemctl status <name>.timer --no-pager -l`
  - `systemctl status <name>.service --no-pager -l`
  - `systemctl list-timers <name>.timer --all --no-pager`
  - `journalctl -u <name>.service -n 20 --no-pager`
