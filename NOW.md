# NOW.md — 阿杰 当前工作态

## 当前优先级（进行中）
- CPA Codex quota monitor 已在 sub2api-prod 部署完成并挂 systemd timer 持续运行
- 当前阈值下监控链路正常，但未命中告警条件
- sing-box 已部署到 sub2api-prod 并配置真实 VLESS/Reality 节点
- CPA 单账号灰度已挂上：codex-everettehagenesga@outlook.com-plus.json 走 socks5://127.0.0.1:1080
- 代理链已验证：sing-box 出口 IP 为 107.150.100.225
- 待补充：真实 Codex 业务请求闭环验证（带 API key 的联调）

## 2026-04-07 当天进展
- [13:10 UTC] 完成 CPA Codex quota monitor 远端部署与定时化：
  - 代码目录：/home/znzn007007/cpa-codex-quota-monitor
  - 配置文件：/home/znzn007007/cpa-codex-quota-monitor/config.json（真实敏感值仅保留远端本地）
  - 服务：/etc/systemd/system/cpa-codex-quota-monitor.service
  - 定时：/etc/systemd/system/cpa-codex-quota-monitor.timer
  - 运行策略：OnBootSec=2min, OnUnitActiveSec=10min
  - 验证：手动触发 service 成功，最新聚合 total=32 / usable=12 / avg5=58.17 / avg7=63.5
  - 飞书链路：真实 webhook secret 发送测试成功
- [15:32 UTC] 在 GCP sub2api-prod 上成功部署 sing-box 代理框架：
  - 二进制：/opt/sing-box/sing-box (v1.13.6)
  - 配置：/opt/sing-box/config.json（初始 outbound: direct）
  - 服务：/etc/systemd/system/sing-box.service
  - 本机监听：socks5(127.0.0.1:1080) / http(127.0.0.1:7890)
  - 验证：curl 通过 socks5/http 均可访问外网，出口 IP 为 104.196.245.182（直连）
- [15:46 UTC] sing-box 配置真实 VLESS/Reality 节点并重启：
  - 节点信息：vless://09388f5c-559f-4b57-b408-7b619987c59c@107.150.100.225:443...
  - 出口验证：curl --socks5 127.0.0.1:1080 返回 IP 为 107.150.100.225
  - sing-box 日志显示 outbound/vless[node-233boy-reality] 正常工作
- [15:46 UTC] CPA 单账号灰度配置：
  - 目标 auth：/opt/cliproxy/pgstore/auths/codex-everettehagenesga@outlook.com-plus.json
  - 已添加字段：proxy_url: "socks5://127.0.0.1:1080"
  - CPA 服务已重启：cliproxyapi.service active (running)
  - CPA 已加载 32 条 auth，灰度账号已指定走本地代理
- [15:57 UTC] 灰度配置验证：
  - 目标 auth 文件已确认包含 proxy_url
  - sing-box 出口已切换到代理节点
  - CPA 服务状态正常
  - 说明：灰度接入本身已生效，缺一次真实 Codex 业务请求闭环验证

## 2026-04-03~2026-04-06 历史进展（参考）
- 2026-04-03 11:55 UTC：完成 GCP sub2api-prod 上的 CLIProxyAPI（CPA）部署：
  - 实例：sub2api-prod (zone: us-west1-b, project: stalwart-elixir-490811-q6)
  - 部署形态：二进制 + systemd（服务：cliproxyapi.service）
  - PostgreSQL：复用现有实例，新建独立 schema cliproxy
  - 配置文件：/opt/cliproxy/.env、/opt/cliproxy/config.yaml
  - 管理密钥：已生成并通过 Management API 验证
  - Codex auth：已迁移 10 条 OAuth 记录到 cliproxy.auth_store
- 2026-04-03 10:47 UTC：完成 sub2api 首 Token 高延迟事故源码分析与原因报告
  - 根因：WSv2 链路 + previous_response_id 恢复 + store=false strict 模式导致首 Token 叠高
  - 修复建议：改 store_disabled_conn_mode=adaptive、提高 TTFT 权重、缩短 sticky TTL
  - 提供 4 套配置方案（保守/激进/局部）

## 当前阻塞
- 无硬阻塞

## 下一步
- 观察 quota monitor 定时运行是否稳定，必要时再调阈值/日报策略
- 监控灰度账号表现：对比代理组 vs 直连组的 TTFT、错误率、超时率
- 需要时补充真实 Codex 请求闭环验证（带 API key 联调）
- 运维/部署类问题优先交由 `doctor`

---
*最后更新：2026-04-07 13:11 UTC*
