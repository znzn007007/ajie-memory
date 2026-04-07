# CPA 代理灰度标准复盘模板

## 适用场景
- 在目标机器上为 CLIProxyAPI（CPA）增加本机代理能力
- 先做单账号灰度，再考虑扩大范围或正式上线
- 适用于 sing-box + CPA 的单机代理接入场景

## 标准目标
1. 先验证目标机器可操作
2. 先部署本机代理层，不先碰 CPA 全局配置
3. 先验证真实上游节点出站
4. 只改单 auth 做灰度
5. 验证 sing-box 日志与 CPA 状态
6. 观察稳定性，再决定是否扩大

---

## 一、环境最小验证
### 必查项
- instance
- project
- zone
- SSH 用户
- 最小 SSH 登录命令

### 通过标准
- 能成功登录目标机器
- 确认有 sudo 权限
- 确认 systemd 可用

---

## 二、部署本机代理层（sing-box）
### 推荐位置
- 二进制：`/opt/sing-box/sing-box`
- 配置：`/opt/sing-box/config.json`
- 服务：`/etc/systemd/system/sing-box.service`

### 推荐本机监听
- `127.0.0.1:1080` → socks5
- `127.0.0.1:7890` → http

### 原则
- 只监听本机，不暴露公网
- 先 direct 验证框架，再接真实节点
- 使用 systemd 托管

---

## 三、接入真实上游节点
### 输入要求
支持：
- `vmess://`
- `vless://`
- `trojan://`
- `ss://`
- `hysteria2://`
- `tuic://`

### 验证命令
```bash
curl --socks5 127.0.0.1:1080 https://api.ipify.org
```

### 通过标准
- 能返回预期出口 IP
- sing-box 无明显握手错误/超时错误

---

## 四、CPA 单账号灰度
### 灰度原则
- 不先改全局 `proxy-url`
- 只对一个目标 auth 增加 `proxy_url`
- 其他 auth 保持原状

### 推荐配置
```json
"proxy_url":"socks5://127.0.0.1:1080"
```

### 适用对象
- file-backed auth
- pgstore auths
- 优先单账号验证

---

## 五、重启/热加载 CPA
### 目标
- 确认 CPA 能重新加载 auth
- 确认服务正常运行

### 观察点
- `API server started successfully`
- `full client load complete`
- `file watcher started`
- `model refresh completed`

### 注意
若出现：
```text
failed to shutdown HTTP server: context deadline exceeded
```
优先判断为：
- reload / restart 期间旧 HTTP server 优雅退出超时
- 不等于代理配置错误
- 不等于服务启动失败

要以后续成功启动日志为准。

---

## 六、验证代理是否真实生效
### 核心日志证据
在 sing-box 日志中应看到：
```text
inbound/socks[socks-in]: inbound connection from 127.0.0.1:xxxxx
inbound/socks[socks-in]: inbound connection to chatgpt.com:443
outbound/vless[node-xxx]: outbound connection to chatgpt.com:443
```

### 判断逻辑
1. auth 已带 `proxy_url`
2. sing-box 有 socks 入站
3. sing-box 有对应代理出站
4. 无明显 error/timeout/handshake fail

则可判断灰度生效。

---

## 七、观测指标
### 必看
- 请求成功率
- TTFT / 首 token
- 总耗时
- timeout
- EOF
- reset
- handshake fail
- 403 / 429 / 5xx

### 快速检查命令
```bash
sudo journalctl -u sing-box -f
sudo journalctl -u cliproxyapi -f
```

```bash
sudo journalctl -u sing-box --since "15 min ago" --no-pager | egrep -i "error|fail|timeout|reset|broken|refused|handshake|closed|EOF"
sudo journalctl -u cliproxyapi --since "15 min ago" --no-pager | egrep -i "error|fail|timeout|reset|broken|EOF|proxy|socks"
```

---

## 八、常见坑
### 1. GitHub latest 下载链接不能乱猜
应先查 release API，拿准确平台包。

### 2. GCP 实例不要直接拿实例名当普通 scp hostname
优先使用：
- `gcloud compute ssh`
- `gcloud compute scp`

### 3. `/opt/cliproxy/pgstore/auths` 默认按 root 权限处理
不要假设普通用户可写。

### 4. shutdown timeout 不等于代理配置坏了
要看后续是否成功启动、auth 是否成功加载。

### 5. 单账号灰度优先于全局切换
风险更低，回滚更简单。

---

## 九、回滚方式
### 单账号回滚
删除目标 auth 的：
```json
"proxy_url":"socks5://127.0.0.1:1080"
```
或改为：
```json
"proxy_url":"direct"
```

### 代理层回滚
- 恢复 `/opt/sing-box/config.json` 旧版本
- `systemctl restart sing-box`

### CPA 层回滚
- 恢复 auth 备份文件
- `systemctl restart cliproxyapi`

---

## 十、扩大灰度前提
满足以下条件后，再考虑扩大到第二个账号或多节点：
- 代理链持续稳定
- 日志无明显错误
- 请求成功率正常
- TTFT 无明显恶化
- 无明显 403 / 429 / timeout 增长

---

## 十一、推荐目录沉淀方式
- 当天事实：`memory/YYYY-MM-DD.md`
- 长期规则：`MEMORY.md`
- 通用 SOP：`playbooks/cpa-proxy-gray.md`

---

## 十二、标准复盘口径
### 背景
为什么要做代理灰度。

### 动作
机器验证、sing-box 部署、节点接入、单 auth 灰度、CPA 重启。

### 结果
是否生效、请求是否经过代理、是否稳定。

### 问题
踩到的坑、报错、异常。

### 结论
能否扩大灰度、是否可复用、下一步建议。
