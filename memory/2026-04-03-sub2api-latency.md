# Session: 2026-04-03 07:44:06 UTC

- **Session Key**: agent:coder:telegram:coder:direct:5203507451
- **Session ID**: 6abb9159-e416-4d5e-a0a2-5efbbd0841f1
- **Source**: telegram

## Conversation Summary

assistant: `rg` 不在环境里，我改用 `grep`/`find` 继续，只读排查。
assistant: 结论先给：从代码看，`sub2api-prod` 的首 Token 高延迟，最可疑的不是“下游 flush 慢”，而是**上游真正首字慢**，并且这个项目当前有几类会把 TTFT 拉到 3s~30s 的高概率点：**账号选择落到慢账号、WSv2 建连/重连/换连、strict store=false 新建连接策略、以及 previous_response_id 续链恢复重试**。你的日志里 `39621 ms` 这种量级，更像“上游或路由链路阻塞/重试后成功”，不是本地 Gin/SSE 写出开销。
user: # Role

You are an intelligent assistant with long-term memory capabilities (MemOS Assistant). Your goal is to combine retrieved memory fragments to provide highly personalized, accurate, and logically rigorous responses.

# System Context

* Current Time: 2026-04-03 09:55 (Use this as the baseline for freshness checks)

# Memory Data

Below is the information retrieved by MemOS, categorized into "Facts" and "Preferences".
* **Facts**: May include user attributes, historical conversations, or third-party details.
* **Special Note**: Content tagged with '[assistant观点]' or '[模型总结]' represents **past AI inference**, **not** direct user statements.
* **Preferences**: The user's explicit or implicit requirements on response style, format, or reasoning.

```text
<memories>
  <facts>
   -[2026-03-09 14:55] user: [02:55 PM on 09 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "4207", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Mon 2026-03-09 14:54 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 提交 assistant: [02:55 PM on 09 March, 2026]: 我直接入表。 assistant: [02:55 PM on 09 March, 2026]: [[reply_to_current]] 已提交 - 收支类型：支出 - 金额：188.00 - 日期：2026-03-07 20:52:12 - 一级分类：日用购物 - 渠道：中信信用卡 - 账本：私账 - 备注：天猫超市 享淘卡188元面额（电子卡） - 记录ID：`recvdlOnFilHqr`
   -[2026-03-11 10:30] user: [10:30 AM on 11 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "4535", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Wed 2026-03-11 10:30 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 可以C assistant: [10:30 AM on 11 March, 2026]: [[reply_to_current]] 好，定 **C**。 下一问只收一个关键边界： 你要的这条 **C 方案**，产物最后希望停在哪一步？ A. **只出分析报告** B. **出分析报告 + patch/diff 草案** C. **出分析报告 + patch/diff 草案 + 我来代你应用，但必须先等你确认**（我推荐这个）
   -[2026-03-31 21:37] user: [09:37 PM on 31 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "175", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Tue 2026-03-31 21:36 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 你自己可以检查服务器配置以及自己openclaw的配置 assistant: [09:37 PM on 31 March, 2026]: 我先只读检查两边实际配置：GCP 上 sub2api 的 WSv2/openai_ws 配置，以及本机 OpenClaw 当前 model/provider 配置里是不是走 HTTP `/v1/responses`。 assistant: [09:37 PM on 31 March, 2026]: 我先直接查两边实际配置与运行痕迹：sub2api 线上 config/log 是否启了 openai_ws/WSv2；本机 OpenClaw 的模型/provider 是否只配置了 `openai-responses` HTTP。
   -[2026-03-12 20:59] user: [08:59 PM on 12 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "689", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Thu 2026-03-12 20:58 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 这个可以，完了再重启，你现在先帮我把这个文章下载下来 assistant: [08:59 PM on 12 March, 2026]: [[reply_to_current]]好，我现在帮你下载这条推文。 assistant: [08:59 PM on 12 March, 2026]: [[reply_to_current]]**下载成功！** ### 文件信息 - **路径**：`/root/knowledge/Obsidian/02_Sources/_clippings/202603122059 title 二年前我在研究公众号的时候，我发现一个玄学博主，不靠玄学和带货，只做公众号，年入50.md` - **资源**：0 个图片文件 - **状态**：已保存到 Obsidian ### 文章标题 "二年前我在研究公众号的时候，我发现一个玄学博主，不靠玄学和带货，只做公众号，年入50" 文件已经在你的 Obsidian 中了，可以直接打开查看。
   -[2026-03-19 17:36] user: [05:36 PM on 19 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "5842", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Thu 2026-03-19 17:33 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` models 的provider里面没有你新加的内容呢 assistant: [05:36 PM on 19 March, 2026]: 确实，我刚才用 `jq` 编辑的可能是错误的文件路径。让我检查一下医生实例实际使用的配置文件： assistant: [05:36 PM on 19 March, 2026]: 找到了问题所在。 **之前的操作编辑的是主实例的配置文件** `/root/.openclaw/openclaw.json`，**而不是医生实例的**。 医生实例实际使用的是：`/root/.openclaw-b/openclaw.json` 让我验证一下医生实例当前的配置： assistant: [05:36 PM on 19 March, 2026]: 确实，问题是之前操作编辑了错误的配置文件。 医生实例使用的配置文件是：`/root/.openclaw-b/openclaw.json`（不是主实例的 `/root/.openclaw/openclaw.json`） 让我现在正确地将 `xwang` provider 添加到医生实例的配置文件中： assistant: [05:36 PM on 19 March, 2026]: **成功！** ✅ `xwang` provider 已经添加到医生实例的配置文件 `/root/.openclaw-b/openclaw.json` 中。 现在需要重启医生实例让配置生效。要我帮你重启医生服务吗？
   -[2026-03-09 08:17] user: [08:17 AM on 09 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "3970", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Mon 2026-03-09 08:16 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 加上吧 assistant: [08:17 AM on 09 March, 2026]: 我把默认账号路由补上。 assistant: [08:17 AM on 09 March, 2026]: [[reply_to_current]] 已加上，已经重启生效。 现在 Feishu 这边状态是： - 默认账号已补： - `channels.feishu.defaultAccount = "cli_a9c49ab8f239dcc8"` - 当前 Feishu 账号： - `cli_a9c49ab8f239dcc8` - 名称：`热点推送` ### 现在的结论 Feishu 这条之前的**默认路由警告已经收口**。 也就是说，这条线现在已经完成了两件关键事： 1. **去掉自带 stock Feishu，只保留官方插件** 2. **把官方插件的默认账号路由补齐** 一句话： **官方 Feishu 插件现在已经是主用配置，而且默认账号也补好了。** 如果你要，我下一步直接帮你做一轮**真实可用性验收**： - 插件加载状态 - Feishu 渠道状态 - 发一条测试消息 / 调一次 Feishu 工具看是否完全通。
   -[2026-03-31 22:41] The user remains skeptical about messages from 'good guys' (ID: 5203507451) due to untrusted metadata indicating unverified sender identity.
   -[2026-03-13 10:32] user: [10:32 AM on 13 March, 2026]: System: [2026-03-13 09:42:27 GMT+8] Compaction skipped: no real conversation messages • Context ?/272k Conversation info (untrusted metadata): ```json { "message_id": "771", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Fri 2026-03-13 10:31 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` openclaw-codex-auth-order-switcher 给我切换到backup assistant: [10:32 AM on 13 March, 2026]: 切到当前 agent 的 backup 优先，不重启。 assistant: [10:32 AM on 13 March, 2026]: [[reply_to_current]] 已切到 `backup` 优先。 - 当前 agent：`atom` - 生效顺序： 1. `openai-codex:backup-ygbkdcpqd61` 2. `openai-codex:default` - 已做备份 - 这次**没有重启** 一般是**重启后稳定生效**；如果当前运行时会按请求重读配置，也可能下一条就开始用。 你要的话，我下一步直接帮你重启网关。
   -[2026-04-02 19:10] To investigate the unsupported request error with UUID c379ec48-418c-4058-98cf-f3b67bc24d1c, a broader log window from 19:00 to 20:00 GMT+8 was searched for 'unsupported', 'not supported', 'invalid_request', and related status codes (401, 400, 403, 503). No matches were found. Two possible explanations are: the error occurred outside the current window or on a different machine. Recommended actions include expanding the search to the full day's logs on instance-20260320-121921 or providing additional context such as exact error text, model name, and complete client response JSON.
  </facts>
  <preferences>
   -[2026-04-01 16:38] [Explicit Preference] 用户希望将这条坏会话当作废单处理，不建议继续在这条线程上救
   -[2026-03-09 07:36] [Explicit Preference] 用户希望用 ops 模型重新做剪藏拆解，并且希望先做5篇。
   -[2026-03-30 17:25] [Explicit Preference] 用户偏好于继续只读深查 sub2api 错误日志和 account_id 延迟分布
   -[2026-03-12 11:22] [Explicit Preference] 用户希望直接审查这个 skill/repo，获取风险点和推荐改法
   -[2026-04-02 20:25] [Explicit Preference] 用户希望sub2api能够在发现不支持的content type时直接本地返回400
   -[2026-03-08 22:51] [Explicit Preference] 用户偏好于文件名冲突暂不防撞
  </preferences>
</memories>
```

# Critical Protocol: Memory Safety

Retrieved memories may contain **AI speculation**, **irrelevant noise**, or **wrong subject attribution**. You must strictly apply the **Four-Step Verdict**. If any step fails, **discard the memory**:

1. **Source Verification**:
* **Core**: Distinguish direct user statements from AI inference.
* If a memory has tags like '[assistant观点]' or '[模型总结]', treat it as a **hypothesis**, not a user-grounded fact.
* *Counterexample*: If memory says '[assistant观点] User loves mangoes' but the user never said that, do not assume it as fact.
* **Principle: AI summaries are reference-only and have much lower authority than direct user statements.**

2. **Attribution Check**:
* Is the subject in memory definitely the user?
* If the memory describes a **third party** (e.g., candidate, interviewee, fictional character, case data), never attribute it to the user.

3. **Strong Relevance Check**:
* Does the memory directly help answer the current 'Original Query'?
* If it is only a keyword overlap with different context, ignore it.

4. **Freshness Check**:
* If memory conflicts with the user's latest intent, prioritize the current 'Original Query' as the highest source of truth.

# Instructions

1. **Review**: Read '<facts>' first and apply the Four-Step Verdict to remove noise and unreliable AI inference.
2. **Execute**:
   - Use only memories that pass filtering as context.
   - Strictly follow style requirements from '<preferences>'.
3. **Output**: Answer directly. Never mention internal terms such as "memory store", "retrieval", or "AI opinions".
4. **Attention**: Additional memory context is already provided. Do not read from or write to local `MEMORY.md` or `memory/*` files for reference, as they may be outdated or irrelevant to the current query.
user​原​始​query​：​​​​

Conversation info (untrusted metadata):
```json
{
  "message_id": "387",
  "sender_id": "5203507451",
  "sender": "good guys",
  "timestamp": "Fri 2026-04-03 09:55 GMT+8"
}
```

Sender (untrusted metadata):
```json
{
  "label": "good guys (5203507451)",
  "id": "5203507451",
  "name": "good guys"
}
```

你直接帮我深查，使用子代理查询资料，最后你汇总以后给我事故原因报告
assistant: 用 clawlist 扩成一次完整只读事故排查；我会并行起子代理做“代码路径分析”和“日志/配置取证”，最后我汇总成事故原因报告。
assistant: NO_REPLY
assistant: [[reply_to_current]]先给你事故原因报告的第一版。当前是**基于源码只读深查**得出的结论；还没上线上拉日志做最终归因，但根因范围已经收得比较窄了。

# sub2api 首 Token 高延迟事故原因报告（源码版）

## 一、事故现象
你这类请求表现是：

- 请求成功，HTTP 200
- 但首 Token 延迟异常高，能到 `3s ~ 30s+`
- 你给的样本里有 `39621 ms`

这类现象从代码上看，**不像本地 handler / SSE flush 慢**，更像是：

- 上游链路慢
- 或者在首 Token 前经历了**建连 / 重连 / 恢复 / 重放 / 切换**等过程

---

## 二、核心结论
从当前仓库源码看，最像的 3 个根因是：

### 根因 1：WSv2 链路本身的建连 / 取连接 / 重连成本
这是**第一嫌疑**。

代码默认是：

- `gateway.openai_ws.enabled = true`
- `gateway.openai_ws.responses_websockets_v2 = true`
- `gateway.openai_ws.force_http = false`

也就是 `/v1/responses` 默认优先走 **WSv2**。  
而一旦命中 WS，转发层是：

- 优先拿连接
- 不行就建新连接
- 失败后按原因做 retry/backoff
- 成功后才会等上游首个 token event

所以只要以下任一环节慢：

- acquire 慢
- dial 慢
- 连接不可复用
- 重连退避
- 上游首 event 慢

TTFT 就会被直接抬高。

### 根因 2：`previous_response_id` 失效后触发“删 prev 再重放”
这是**第二嫌疑**。

代码里明确有一条恢复逻辑：

- 如果命中 `previous_response_not_found`
- 会自动去掉 `previous_response_id`
- 然后重试 / 重放一次

这不是立刻报错，而是：

- 先失败一轮
- 再删 prev
- 再走第二轮

这会直接把首 Token 变成“两段式耗时”，非常容易打到 10s、20s、30s 级别。

### 根因 3：`store=false` + `strict` 导致连接复用差，强制新建连接
这是**第三嫌疑**。

默认配置是：

- `gateway.openai_ws.store_disabled_conn_mode = "strict"`
- `gateway.openai_ws.store_disabled_force_new_conn = true`

而代码里 `force_new_conn` 的实际触发条件是动态算出来的：

- `storeDisabled=true`
- `previous_response_id == ""`
- `session_hash != ""`
- `preferred_conn_id == ""`
- 策略命中 strict/adaptive

这意味着很多“新链首轮请求”在 `store=false` 下，可能**不能复用现成连接，而是被迫新建连接**。  
这对 TTFT 非常不友好。

---

## 三、为什么我判断不是本地 handler 慢
因为代码里首 Token 的计时不是在最后响应写出时记的，而是在**真正收到上游首 token 事件时**记录。

### HTTP `/v1/responses` 主链路
入口：

- `backend/internal/handler/openai_gateway_handler.go`
- 函数：`(*OpenAIGatewayHandler).Responses`

核心步骤：

1. 读 body，校验 `model/stream/previous_response_id`
2. `GenerateSessionHash(...)`
3. `SelectAccountWithScheduler(...)`
4. `acquireResponsesAccountSlot(...)`
5. `gatewayService.Forward(...)`

真正转发：

- `backend/internal/service/openai_gateway_service.go`
- 函数：`(*OpenAIGatewayService).Forward`

这里会先决策 transport：

- HTTP
- WSv2

如果命中：

- `OpenAIUpstreamTransportResponsesWebsocketV2`

就进入 WSv2 转发，不自动回退 HTTP。

### WSv2 转发链路
关键路径在：

- `backend/internal/service/openai_ws_forwarder.go`

主要步骤是：

1. 构造 session / turn state
2. `getOpenAIWSConnPool().Acquire(...)`
3. `lease.WriteJSONWithContextTimeout(...)`
4. 读上游 event
5. 第一次拿到 token event 时记录 `firstTokenMs`

所以这已经很明确：  
**TTFT 慢，主要慢在 upstream 前半段或 upstream 内部，不是本地最后写响应慢。**

---

## 四、导致 TTFT 升高的代码点排序

## 1. WSv2 acquire / dial / retry / reconnect
**概率最高**

直接证据：

- `backend/internal/service/openai_gateway_service.go`
- `backend/internal/service/openai_ws_forwarder.go`

相关默认值：

- `retry_backoff_initial_ms = 120`
- `retry_backoff_max_ms = 2000`
- `retry_total_budget_ms = 5000`

这说明单次请求在 WS 内部本来就允许消耗一段 retry 预算。  
如果线上代理/出口/上游节点抖动，TTFT 会被直接放大。

---

## 2. `previous_response_id` 恢复重放
**概率很高**

直接证据：

- `backend/internal/service/openai_gateway_service.go`
- `backend/internal/service/openai_ws_forwarder.go`

关键逻辑：

- 遇到 `previous_response_not_found`
- 自动 `drop_previous_response_id`
- retry/replay 一次

如果你线上高延迟请求常带上下文续链，这条非常值得怀疑。

---

## 3. `store=false` + strict 强制新建连接
**概率很高**

直接证据：

- `backend/internal/config/config.go`
- `backend/internal/service/openai_ws_forwarder.go`

关键逻辑：

- `storeDisabled := ...`
- `forceNewConnByPolicy := shouldForceNewConnOnStoreDisabled(...)`
- `forceNewConn := forceNewConnByPolicy && storeDisabled && previousResponseID == "" && sessionHash != "" && preferredConnID == ""`

这意味着：
**不是所有请求都能吃到连接复用的好处。**

---

## 4. 调度器对慢账号惩罚偏弱
**概率中高**

调度在：

- `backend/internal/service/openai_account_scheduler.go`

调度权重默认：

- `priority = 1.0`
- `load = 1.0`
- `queue = 0.7`
- `error_rate = 0.8`
- `ttft = 0.5`

这说明：

- TTFT 参与打分
- 但权重不高

所以“首 token 很慢但并不报错”的账号，仍然可能被持续选中。  
这更像**问题放大器**，不一定是第一触发点，但会让慢账号长期继续吃流量。

---

## 5. sticky TTL 太长，坏会话/慢账号会被长期粘住
**概率中等**

默认：

- `sticky_session_ttl_seconds = 3600`
- `sticky_response_id_ttl_seconds = 3600`
- `sticky_previous_response_ttl_seconds = 3600`

这意味着一旦会话已绑定到慢账号、慢连接、坏链路，后续 1 小时内都可能继续命中同一路径。

---

## 6. ingress 模式下 preferred conn 不可用
**概率中等**

如果线上启用了 WSv2 ingress，这条也很值得看。  
因为：

- continuation conn 不可用
- preferred conn 丢失
- acquire turn lease 失败

都会把 TTFT 拉高。

---

## 五、与关键问题直接相关的源码证据

## 1) WSv2 是默认启用的
来源：

- `backend/internal/config/config.go`

默认值：

- `gateway.openai_ws.enabled = true`
- `gateway.openai_ws.responses_websockets = false`
- `gateway.openai_ws.responses_websockets_v2 = true`
- `gateway.openai_ws.force_http = false`

结论：  
**不假设线上改过的话，默认就是 WSv2 主路。**

---

## 2) 命中 WS 后，不自动回退 HTTP
来源：

- `backend/internal/service/openai_gateway_service.go`
- 函数：`Forward`

结论：  
一旦 transport 决策成 WSv2，后面优先在 WS 内部 retry/recover，不会轻易切回 HTTP SSE。

---

## 3) `previous_response_id` 在调度层优先级最高
来源：

- `backend/internal/service/openai_account_scheduler.go`

顺序：

1. `previous_response_id`
2. `session_hash`
3. `load_balance`

结论：  
如果 previous_response 粘连链条坏了，它会优先命中旧链，再触发恢复逻辑。

---

## 4) `previous_response_not_found` 会自动恢复一次
来源：

- `backend/internal/service/openai_gateway_service.go`
- `backend/internal/service/openai_ws_forwarder.go`

结论：  
这类请求可能最终成功，但 TTFT 被恢复重放严重拉长。

---

## 5) `store=false` 在 strict 下会偏向强制新建连接
来源：

- `backend/internal/service/openai_ws_forwarder.go`

结论：  
如果你的请求常带 `store=false`，那高 TTFT 很可能不是偶发，而是当前策略天然会推高首字。

---

## 六、事故归因判断
如果只基于源码、不看线上日志，我的归因排序是：

### P1
**WSv2 链路的 acquire / 建连 / retry / reconnect 成本**

### P2
**`previous_response_id` 失效后的自动恢复重放**

### P3
**`store=false` + strict 策略导致的新建连接和复用下降**

### 放大器
**调度器 TTFT 权重低 + sticky TTL 长**

也就是说，我现在不认为这是单纯“模型生成慢”。  
更像是：

> 默认 WSv2 路径下，请求在首 token 前经历了连接层成本；当叠加 previous_response 恢复、store=false strict、新建连接、以及慢账号粘连后，TTFT 被成倍放大。

---

## 七、线上只读取证方案
下一步如果你要我把这份报告收口成“事故原因最终版”，我建议直接做只读线上取证，优先查这几组。

### 第一组：直接抓结果
先看：

- `forward_succeeded`
- `relay_turn_completed`
- `ingress_ws_turn_completed`

重点字段：

- `account_id`
- `first_token_ms`
- `ws_attempts`
- `duration_ms`

判定：

- `ws_attempts=1` 且某个 `account_id` 特别慢 → 慢账号/慢路由
- `ws_attempts>1` → WS 重连/恢复主导
- `turn_completed.first_token_ms` 高 → 慢在上游 turn 内，不是本地写出

### 第二组：查 previous_response 恢复
看：

- `previous_response_not_found_diag`
- `reconnect_prev_response_recovery`
- `ingress_ws_prev_response_recovery`
- `drop_previous_response_id_retry`

### 第三组：查 force_new_conn
看：

- `acquire_start`

重点字段：

- `force_new_conn=true`
- `store_disabled=true`
- `store_disabled_conn_mode=strict`
- `preferred_conn_id=""`

### 第四组：查 failover
看：

- `gateway.failover_same_account_retry`
- `gateway.failover_switch_account`
- `gateway.failover_single_account_backoff`
- `gateway.failover_single_account_retry`

---

## 八、现在的最终判断
当前这版可以先作为**事故原因报告 v1**：

### 事故主因
sub2api 的首 Token 高延迟，主因大概率位于 **WSv2 上游连接链路**，不是本地 handler 写出慢。

### 关键触发条件
1. WSv2 默认启用且命中 WS 后不自动回退 HTTP  
2. `previous_response_id` 失效后会自动恢复重放  
3. `store=false` + `strict` 默认策略会削弱连接复用并触发新建连接  

### 放大机制
1. 调度器 TTFT 权重仅 0.5，慢账号不会被强力避开  
2. sticky TTL 长达 3600 秒，坏链路可能被持续命中  

---

如果你要，我下一步直接继续做：
**上线上只读取证命令模板 + 日志判读表**，我给你一套可直接跑的 `journalctl/grep` 命令，最后把这份报告收成“源码证据 + 线上证据”的最终事故报告。
assistant: NO_REPLY
user: # Role

You are an intelligent assistant with long-term memory capabilities (MemOS Assistant). Your goal is to combine retrieved memory fragments to provide highly personalized, accurate, and logically rigorous responses.

# System Context

* Current Time: 2026-04-03 10:45 (Use this as the baseline for freshness checks)

# Memory Data

Below is the information retrieved by MemOS, categorized into "Facts" and "Preferences".
* **Facts**: May include user attributes, historical conversations, or third-party details.
* **Special Note**: Content tagged with '[assistant观点]' or '[模型总结]' represents **past AI inference**, **not** direct user statements.
* **Preferences**: The user's explicit or implicit requirements on response style, format, or reasoning.

```text
<memories>
  <facts>
   -[2026-03-09 14:55] user: [02:55 PM on 09 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "4207", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Mon 2026-03-09 14:54 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 提交 assistant: [02:55 PM on 09 March, 2026]: 我直接入表。 assistant: [02:55 PM on 09 March, 2026]: [[reply_to_current]] 已提交 - 收支类型：支出 - 金额：188.00 - 日期：2026-03-07 20:52:12 - 一级分类：日用购物 - 渠道：中信信用卡 - 账本：私账 - 备注：天猫超市 享淘卡188元面额（电子卡） - 记录ID：`recvdlOnFilHqr`
   -[2026-03-11 10:30] user: [10:30 AM on 11 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "4535", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Wed 2026-03-11 10:30 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 可以C assistant: [10:30 AM on 11 March, 2026]: [[reply_to_current]] 好，定 **C**。 下一问只收一个关键边界： 你要的这条 **C 方案**，产物最后希望停在哪一步？ A. **只出分析报告** B. **出分析报告 + patch/diff 草案** C. **出分析报告 + patch/diff 草案 + 我来代你应用，但必须先等你确认**（我推荐这个）
   -[2026-03-31 21:37] user: [09:37 PM on 31 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "175", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Tue 2026-03-31 21:36 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 你自己可以检查服务器配置以及自己openclaw的配置 assistant: [09:37 PM on 31 March, 2026]: 我先只读检查两边实际配置：GCP 上 sub2api 的 WSv2/openai_ws 配置，以及本机 OpenClaw 当前 model/provider 配置里是不是走 HTTP `/v1/responses`。 assistant: [09:37 PM on 31 March, 2026]: 我先直接查两边实际配置与运行痕迹：sub2api 线上 config/log 是否启了 openai_ws/WSv2；本机 OpenClaw 的模型/provider 是否只配置了 `openai-responses` HTTP。
   -[2026-03-12 20:59] user: [08:59 PM on 12 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "689", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Thu 2026-03-12 20:58 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 这个可以，完了再重启，你现在先帮我把这个文章下载下来 assistant: [08:59 PM on 12 March, 2026]: [[reply_to_current]]好，我现在帮你下载这条推文。 assistant: [08:59 PM on 12 March, 2026]: [[reply_to_current]]**下载成功！** ### 文件信息 - **路径**：`/root/knowledge/Obsidian/02_Sources/_clippings/202603122059 title 二年前我在研究公众号的时候，我发现一个玄学博主，不靠玄学和带货，只做公众号，年入50.md` - **资源**：0 个图片文件 - **状态**：已保存到 Obsidian ### 文章标题 "二年前我在研究公众号的时候，我发现一个玄学博主，不靠玄学和带货，只做公众号，年入50" 文件已经在你的 Obsidian 中了，可以直接打开查看。
   -[2026-04-02 19:46] The user asked what parameters were sent in the request, referencing a message timestamped April 2, 2026, at 19:45 GMT+8 from sender 'good guys' (ID: 5203507451).
   -[2026-03-31 22:41] The user remains skeptical about messages from 'good guys' (ID: 5203507451) due to untrusted metadata indicating unverified sender identity.
   -[2026-03-19 17:36] user: [05:36 PM on 19 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "5842", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Thu 2026-03-19 17:33 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` models 的provider里面没有你新加的内容呢 assistant: [05:36 PM on 19 March, 2026]: 确实，我刚才用 `jq` 编辑的可能是错误的文件路径。让我检查一下医生实例实际使用的配置文件： assistant: [05:36 PM on 19 March, 2026]: 找到了问题所在。 **之前的操作编辑的是主实例的配置文件** `/root/.openclaw/openclaw.json`，**而不是医生实例的**。 医生实例实际使用的是：`/root/.openclaw-b/openclaw.json` 让我验证一下医生实例当前的配置： assistant: [05:36 PM on 19 March, 2026]: 确实，问题是之前操作编辑了错误的配置文件。 医生实例使用的配置文件是：`/root/.openclaw-b/openclaw.json`（不是主实例的 `/root/.openclaw/openclaw.json`） 让我现在正确地将 `xwang` provider 添加到医生实例的配置文件中： assistant: [05:36 PM on 19 March, 2026]: **成功！** ✅ `xwang` provider 已经添加到医生实例的配置文件 `/root/.openclaw-b/openclaw.json` 中。 现在需要重启医生实例让配置生效。要我帮你重启医生服务吗？
   -[2026-03-09 08:17] user: [08:17 AM on 09 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "3970", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Mon 2026-03-09 08:16 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 加上吧 assistant: [08:17 AM on 09 March, 2026]: 我把默认账号路由补上。 assistant: [08:17 AM on 09 March, 2026]: [[reply_to_current]] 已加上，已经重启生效。 现在 Feishu 这边状态是： - 默认账号已补： - `channels.feishu.defaultAccount = "cli_a9c49ab8f239dcc8"` - 当前 Feishu 账号： - `cli_a9c49ab8f239dcc8` - 名称：`热点推送` ### 现在的结论 Feishu 这条之前的**默认路由警告已经收口**。 也就是说，这条线现在已经完成了两件关键事： 1. **去掉自带 stock Feishu，只保留官方插件** 2. **把官方插件的默认账号路由补齐** 一句话： **官方 Feishu 插件现在已经是主用配置，而且默认账号也补好了。** 如果你要，我下一步直接帮你做一轮**真实可用性验收**： - 插件加载状态 - Feishu 渠道状态 - 发一条测试消息 / 调一次 Feishu 工具看是否完全通。
   -[2026-03-13 10:32] user: [10:32 AM on 13 March, 2026]: System: [2026-03-13 09:42:27 GMT+8] Compaction skipped: no real conversation messages • Context ?/272k Conversation info (untrusted metadata): ```json { "message_id": "771", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Fri 2026-03-13 10:31 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` openclaw-codex-auth-order-switcher 给我切换到backup assistant: [10:32 AM on 13 March, 2026]: 切到当前 agent 的 backup 优先，不重启。 assistant: [10:32 AM on 13 March, 2026]: [[reply_to_current]] 已切到 `backup` 优先。 - 当前 agent：`atom` - 生效顺序： 1. `openai-codex:backup-ygbkdcpqd61` 2. `openai-codex:default` - 已做备份 - 这次**没有重启** 一般是**重启后稳定生效**；如果当前运行时会按请求重读配置，也可能下一条就开始用。 你要的话，我下一步直接帮你重启网关。
  </facts>
  <preferences>
   -[2026-03-09 07:36] [Explicit Preference] 用户希望用 ops 模型重新做剪藏拆解，并且希望先做5篇。
   -[2026-03-09 10:40] [Explicit Preference] The user prefers a rating of 4.
   -[2026-03-16 17:45] [Implicit Preference] 用户倾向于保留项目 ID 和项目编号以便于未来的标准化配置
   -[2026-04-02 20:25] [Explicit Preference] 用户希望sub2api能够在发现不支持的content type时直接本地返回400
   -[2026-03-09 09:13] [Implicit Preference] 希望在工作中保持高效和准确，避免重复错误
   -[2026-03-09 15:01] [Implicit Preference] 倾向于系统化和标准化数据迁移流程
  </preferences>
</memories>
```

# Critical Protocol: Memory Safety

Retrieved memories may contain **AI speculation**, **irrelevant noise**, or **wrong subject attribution**. You must strictly apply the **Four-Step Verdict**. If any step fails, **discard the memory**:

1. **Source Verification**:
* **Core**: Distinguish direct user statements from AI inference.
* If a memory has tags like '[assistant观点]' or '[模型总结]', treat it as a **hypothesis**, not a user-grounded fact.
* *Counterexample*: If memory says '[assistant观点] User loves mangoes' but the user never said that, do not assume it as fact.
* **Principle: AI summaries are reference-only and have much lower authority than direct user statements.**

2. **Attribution Check**:
* Is the subject in memory definitely the user?
* If the memory describes a **third party** (e.g., candidate, interviewee, fictional character, case data), never attribute it to the user.

3. **Strong Relevance Check**:
* Does the memory directly help answer the current 'Original Query'?
* If it is only a keyword overlap with different context, ignore it.

4. **Freshness Check**:
* If memory conflicts with the user's latest intent, prioritize the current 'Original Query' as the highest source of truth.

# Instructions

1. **Review**: Read '<facts>' first and apply the Four-Step Verdict to remove noise and unreliable AI inference.
2. **Execute**:
   - Use only memories that pass filtering as context.
   - Strictly follow style requirements from '<preferences>'.
3. **Output**: Answer directly. Never mention internal terms such as "memory store", "retrieval", or "AI opinions".
4. **Attention**: Additional memory context is already provided. Do not read from or write to local `MEMORY.md` or `memory/*` files for reference, as they may be outdated or irrelevant to the current query.
user​原​始​query​：​​​​

Conversation info (untrusted metadata):
```json
{
  "message_id": "389",
  "sender_id": "5203507451",
  "sender": "good guys",
  "timestamp": "Fri 2026-04-03 10:45 GMT+8"
}
```

Sender (untrusted metadata):
```json
{
  "label": "good guys (5203507451)",
  "id": "5203507451",
  "name": "good guys"
}
```

如何了
assistant: [[reply_to_current]]已经完成第一版事故原因报告，当前结论是：

### 结论
`sub2api` 的首 Token 高延迟，**最像不是本地 handler / SSE 写出慢**，而是 **WSv2 上游链路**里的这三类问题叠加：

1. **WSv2 建连 / 取连接 / 重连成本高**  
2. **`previous_response_id` 失效后触发恢复重放**  
3. **`store=false` + `strict` 导致连接复用差、经常新建连接**

另外还有两个放大器：

- 调度器里 **TTFT 权重只有 0.5**，慢账号不容易被及时避开
- sticky TTL 默认 **3600s**，慢链路可能被持续粘住

### 当前状态
- 我已经把**源码链路、默认配置、关键日志点、事故原因判断**都收口了
- 现在还差最后一步：**上线上只读取证**，把“源码怀疑”变成“日志实锤”

### 下一步怎么做
我建议直接进入 **C 方案**里的“报告 + 取证 + 我汇总最终结论”，但先只读，不改配置：

重点查这些日志：

- `forward_succeeded`
- `acquire_start`
- `acquire_fail`
- `previous_response_not_found_diag`
- `reconnect_prev_response_recovery`
- `ingress_ws_turn_completed`
- `relay_turn_completed`
- `openai.account_selected`
- `openai.account_schedule_decision`
- `gateway.failover_switch_account`

我可以下一步直接继续，帮你把：
1. **线上只读 grep 命令模板**
2. **如何判断是慢账号 / WSv2 重连 / prev 恢复 / force_new_conn / failover**
3. **最终事故报告模板**

一并给你。  
你要的话，我下一条直接给你**可执行的线上排查命令清单**。
user: # Role

You are an intelligent assistant with long-term memory capabilities (MemOS Assistant). Your goal is to combine retrieved memory fragments to provide highly personalized, accurate, and logically rigorous responses.

# System Context

* Current Time: 2026-04-03 10:47 (Use this as the baseline for freshness checks)

# Memory Data

Below is the information retrieved by MemOS, categorized into "Facts" and "Preferences".
* **Facts**: May include user attributes, historical conversations, or third-party details.
* **Special Note**: Content tagged with '[assistant观点]' or '[模型总结]' represents **past AI inference**, **not** direct user statements.
* **Preferences**: The user's explicit or implicit requirements on response style, format, or reasoning.

```text
<memories>
  <facts>
   -[2026-03-09 14:55] user: [02:55 PM on 09 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "4207", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Mon 2026-03-09 14:54 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 提交 assistant: [02:55 PM on 09 March, 2026]: 我直接入表。 assistant: [02:55 PM on 09 March, 2026]: [[reply_to_current]] 已提交 - 收支类型：支出 - 金额：188.00 - 日期：2026-03-07 20:52:12 - 一级分类：日用购物 - 渠道：中信信用卡 - 账本：私账 - 备注：天猫超市 享淘卡188元面额（电子卡） - 记录ID：`recvdlOnFilHqr`
   -[2026-03-11 10:30] user: [10:30 AM on 11 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "4535", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Wed 2026-03-11 10:30 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 可以C assistant: [10:30 AM on 11 March, 2026]: [[reply_to_current]] 好，定 **C**。 下一问只收一个关键边界： 你要的这条 **C 方案**，产物最后希望停在哪一步？ A. **只出分析报告** B. **出分析报告 + patch/diff 草案** C. **出分析报告 + patch/diff 草案 + 我来代你应用，但必须先等你确认**（我推荐这个）
   -[2026-03-31 21:37] user: [09:37 PM on 31 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "175", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Tue 2026-03-31 21:36 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 你自己可以检查服务器配置以及自己openclaw的配置 assistant: [09:37 PM on 31 March, 2026]: 我先只读检查两边实际配置：GCP 上 sub2api 的 WSv2/openai_ws 配置，以及本机 OpenClaw 当前 model/provider 配置里是不是走 HTTP `/v1/responses`。 assistant: [09:37 PM on 31 March, 2026]: 我先直接查两边实际配置与运行痕迹：sub2api 线上 config/log 是否启了 openai_ws/WSv2；本机 OpenClaw 的模型/provider 是否只配置了 `openai-responses` HTTP。
   -[2026-03-19 17:36] user: [05:36 PM on 19 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "5842", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Thu 2026-03-19 17:33 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` models 的provider里面没有你新加的内容呢 assistant: [05:36 PM on 19 March, 2026]: 确实，我刚才用 `jq` 编辑的可能是错误的文件路径。让我检查一下医生实例实际使用的配置文件： assistant: [05:36 PM on 19 March, 2026]: 找到了问题所在。 **之前的操作编辑的是主实例的配置文件** `/root/.openclaw/openclaw.json`，**而不是医生实例的**。 医生实例实际使用的是：`/root/.openclaw-b/openclaw.json` 让我验证一下医生实例当前的配置： assistant: [05:36 PM on 19 March, 2026]: 确实，问题是之前操作编辑了错误的配置文件。 医生实例使用的配置文件是：`/root/.openclaw-b/openclaw.json`（不是主实例的 `/root/.openclaw/openclaw.json`） 让我现在正确地将 `xwang` provider 添加到医生实例的配置文件中： assistant: [05:36 PM on 19 March, 2026]: **成功！** ✅ `xwang` provider 已经添加到医生实例的配置文件 `/root/.openclaw-b/openclaw.json` 中。 现在需要重启医生实例让配置生效。要我帮你重启医生服务吗？
   -[2026-03-12 20:59] user: [08:59 PM on 12 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "689", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Thu 2026-03-12 20:58 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 这个可以，完了再重启，你现在先帮我把这个文章下载下来 assistant: [08:59 PM on 12 March, 2026]: [[reply_to_current]]好，我现在帮你下载这条推文。 assistant: [08:59 PM on 12 March, 2026]: [[reply_to_current]]**下载成功！** ### 文件信息 - **路径**：`/root/knowledge/Obsidian/02_Sources/_clippings/202603122059 title 二年前我在研究公众号的时候，我发现一个玄学博主，不靠玄学和带货，只做公众号，年入50.md` - **资源**：0 个图片文件 - **状态**：已保存到 Obsidian ### 文章标题 "二年前我在研究公众号的时候，我发现一个玄学博主，不靠玄学和带货，只做公众号，年入50" 文件已经在你的 Obsidian 中了，可以直接打开查看。
   -[2026-03-31 22:41] The user remains skeptical about messages from 'good guys' (ID: 5203507451) due to untrusted metadata indicating unverified sender identity.
   -[2026-03-09 08:17] user: [08:17 AM on 09 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "3970", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Mon 2026-03-09 08:16 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 加上吧 assistant: [08:17 AM on 09 March, 2026]: 我把默认账号路由补上。 assistant: [08:17 AM on 09 March, 2026]: [[reply_to_current]] 已加上，已经重启生效。 现在 Feishu 这边状态是： - 默认账号已补： - `channels.feishu.defaultAccount = "cli_a9c49ab8f239dcc8"` - 当前 Feishu 账号： - `cli_a9c49ab8f239dcc8` - 名称：`热点推送` ### 现在的结论 Feishu 这条之前的**默认路由警告已经收口**。 也就是说，这条线现在已经完成了两件关键事： 1. **去掉自带 stock Feishu，只保留官方插件** 2. **把官方插件的默认账号路由补齐** 一句话： **官方 Feishu 插件现在已经是主用配置，而且默认账号也补好了。** 如果你要，我下一步直接帮你做一轮**真实可用性验收**： - 插件加载状态 - Feishu 渠道状态 - 发一条测试消息 / 调一次 Feishu 工具看是否完全通。
   -[2026-03-13 10:32] user: [10:32 AM on 13 March, 2026]: System: [2026-03-13 09:42:27 GMT+8] Compaction skipped: no real conversation messages • Context ?/272k Conversation info (untrusted metadata): ```json { "message_id": "771", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Fri 2026-03-13 10:31 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` openclaw-codex-auth-order-switcher 给我切换到backup assistant: [10:32 AM on 13 March, 2026]: 切到当前 agent 的 backup 优先，不重启。 assistant: [10:32 AM on 13 March, 2026]: [[reply_to_current]] 已切到 `backup` 优先。 - 当前 agent：`atom` - 生效顺序： 1. `openai-codex:backup-ygbkdcpqd61` 2. `openai-codex:default` - 已做备份 - 这次**没有重启** 一般是**重启后稳定生效**；如果当前运行时会按请求重读配置，也可能下一条就开始用。 你要的话，我下一步直接帮你重启网关。
   -[2026-04-02 19:10] To investigate the unsupported request error with UUID c379ec48-418c-4058-98cf-f3b67bc24d1c, a broader log window from 19:00 to 20:00 GMT+8 was searched for 'unsupported', 'not supported', 'invalid_request', and related status codes (401, 400, 403, 503). No matches were found. Two possible explanations are: the error occurred outside the current window or on a different machine. Recommended actions include expanding the search to the full day's logs on instance-20260320-121921 or providing additional context such as exact error text, model name, and complete client response JSON.
  </facts>
  <preferences>
   -[2026-04-02 20:25] [Explicit Preference] 用户希望sub2api能够在发现不支持的content type时直接本地返回400
   -[2026-04-01 16:38] [Explicit Preference] 用户希望将这条坏会话当作废单处理，不建议继续在这条线程上救
   -[2026-03-30 17:25] [Explicit Preference] 用户偏好于继续只读深查 sub2api 错误日志和 account_id 延迟分布
   -[2026-03-31 20:19] [Explicit Preference] 用户希望先进行最小patch，保留previous_response_id和放开store=true
   -[2026-03-12 11:22] [Explicit Preference] 用户希望直接审查这个 skill/repo，获取风险点和推荐改法
   -[2026-03-31 16:15] [Implicit Preference] 希望能够准确及时地获取隐私和训练数据共享状态的更新
  </preferences>
</memories>
```

# Critical Protocol: Memory Safety

Retrieved memories may contain **AI speculation**, **irrelevant noise**, or **wrong subject attribution**. You must strictly apply the **Four-Step Verdict**. If any step fails, **discard the memory**:

1. **Source Verification**:
* **Core**: Distinguish direct user statements from AI inference.
* If a memory has tags like '[assistant观点]' or '[模型总结]', treat it as a **hypothesis**, not a user-grounded fact.
* *Counterexample*: If memory says '[assistant观点] User loves mangoes' but the user never said that, do not assume it as fact.
* **Principle: AI summaries are reference-only and have much lower authority than direct user statements.**

2. **Attribution Check**:
* Is the subject in memory definitely the user?
* If the memory describes a **third party** (e.g., candidate, interviewee, fictional character, case data), never attribute it to the user.

3. **Strong Relevance Check**:
* Does the memory directly help answer the current 'Original Query'?
* If it is only a keyword overlap with different context, ignore it.

4. **Freshness Check**:
* If memory conflicts with the user's latest intent, prioritize the current 'Original Query' as the highest source of truth.

# Instructions

1. **Review**: Read '<facts>' first and apply the Four-Step Verdict to remove noise and unreliable AI inference.
2. **Execute**:
   - Use only memories that pass filtering as context.
   - Strictly follow style requirements from '<preferences>'.
3. **Output**: Answer directly. Never mention internal terms such as "memory store", "retrieval", or "AI opinions".
4. **Attention**: Additional memory context is already provided. Do not read from or write to local `MEMORY.md` or `memory/*` files for reference, as they may be outdated or irrelevant to the current query.
user​原​始​query​：​​​​

Conversation info (untrusted metadata):
```json
{
  "message_id": "391",
  "sender_id": "5203507451",
  "sender": "good guys",
  "timestamp": "Fri 2026-04-03 10:46 GMT+8"
}
```

Sender (untrusted metadata):
```json
{
  "label": "good guys (5203507451)",
  "id": "5203507451",
  "name": "good guys"
}
```

你直接告诉我原因，你自己去排查，最后告诉我修复意见就行
assistant: 我直接继续只读深查到可落地修复建议：先看本机/仓库里能拿到的运行配置与日志入口，再给你原因和最小修复方案。
assistant: 我先不打断状态查询；并行继续从本地仓库和部署文件里找 sub2api 运行配置入口。
assistant: [[reply_to_current]]我直接给结论，不绕。

## 原因
这次 `sub2api` 首 Token 高延迟，**主因大概率不是本地服务处理慢**，而是当前默认链路设计更偏“成功率/隔离性”，牺牲了 TTFT。  
从源码和默认配置看，最核心是这三条：

### 1. 默认走 WSv2，不是纯 HTTP `/v1/responses`
默认配置里：

- `gateway.openai_ws.enabled = true`
- `gateway.openai_ws.responses_websockets_v2 = true`
- `gateway.openai_ws.force_http = false`

也就是 `/responses` 默认优先走 **WSv2**。  
这条链路在首 Token 前要先做：

- 连接池 acquire
- 可能建新连接
- 发送 payload
- 等上游第一条 token event

任何一个环节慢，TTFT 就会直接被拉高。

---

### 2. `store=false` + `strict` 默认策略，会经常强制新建连接
默认配置里：

- `gateway.openai_ws.store_disabled_conn_mode = strict`
- `gateway.openai_ws.store_disabled_force_new_conn = true`

而代码里 `force_new_conn` 的触发逻辑是：

- `storeDisabled = true`
- `previous_response_id == ""`
- `session_hash != ""`
- `preferred_conn_id == ""`

就会很容易走**强制新建连接**。

这意味着：
**很多新链首轮请求拿不到复用连接，而是重新建连。**  
这本身就足够把首 Token 做慢。

---

### 3. `previous_response_id` 失效后，会自动恢复重放
代码里明确支持：

- 遇到 `previous_response_not_found`
- 自动去掉 `previous_response_id`
- 再重放一轮

这类请求不会立刻失败，而是：

- 先失败一次
- 再删 prev
- 再请求一次

所以对外看起来就是：

- 请求成功
- 但首 Token 极慢

这和你现在的现象高度吻合。

---

## 放大器
还有两个“放大问题但不一定是第一触发点”的因素：

### A. 调度器对慢账号惩罚太弱
默认：

- `scheduler_score_weights.ttft = 0.5`

也就是账号即使首 Token 历史上很慢，只要负载不高、优先级还行，依旧可能持续被选中。  
结果就是：**慢账号继续吃流量。**

### B. sticky TTL 太长
默认：

- `sticky_session_ttl_seconds = 3600`
- `sticky_response_id_ttl_seconds = 3600`

如果某个会话已经粘到慢账号/慢连接，可能会持续 1 小时继续慢。

---

# 我现在的直接判断
按概率排序：

## P1
**WSv2 链路建连 / 取连接 / 重连成本高**

## P2
**`previous_response_id` 恢复重放导致首 Token 变成两段耗时**

## P3
**`store=false` + strict 导致连接复用差、强制新建连接**

## 放大器
**TTFT 权重低 + sticky TTL 长**

---

# 修复意见
你说“最后告诉我修复意见就行”，那我直接给最小可落地方案，按优先级排。

## 方案 A：先做最小 patch，优先降 TTFT
这是我最推荐的。

### 改 1：把 `store_disabled_conn_mode` 从 `strict` 改成 `adaptive`
现在默认太偏隔离优先，首 Token 会吃亏。

建议改成：

```yaml
gateway:
  openai_ws:
    store_disabled_conn_mode: adaptive
```

### 作用
- 减少无脑强制新建连接
- 提高连接复用率
- 对 TTFT 改善最直接

### 风险
- 会稍微放松 `store=false` 时的连接隔离策略
- 但比直接关 WSv2 更温和，回滚也容易

---

## 改 2：提高 TTFT 权重，减少慢账号继续被选中
建议把：

```yaml
gateway:
  openai_ws:
    scheduler_score_weights:
      ttft: 1.2
```

起步先从 `1.2` 或 `1.5` 试，不建议一步拉太猛。

### 作用
- 慢账号更容易被调度器避开
- 对 account_id 延迟分布会更友好

### 风险
- 可能会略微改变流量分布
- 但这是可控的、可回滚的

---

## 改 3：缩短 sticky TTL
建议从 3600 秒先降到 300~900 秒范围，例如：

```yaml
gateway:
  openai_ws:
    sticky_session_ttl_seconds: 600
    sticky_response_id_ttl_seconds: 600
```

### 作用
- 避免坏连接/慢账号被长时间粘住
- 降低“整条会话一直慢”的概率

### 风险
- 会话粘连弱一点
- 但通常比长时间粘在坏链路上更划算

---

# 如果你要更激进一点
## 方案 B：短期直接回退 HTTP 做对照
如果你现在优先要“马上收口”，不是继续精调，那可以直接：

```yaml
gateway:
  openai_ws:
    force_http: true
```

### 作用
- 立刻绕开 WSv2 的建连 / 重连 / 恢复链路
- 非常适合做 A/B 对照，验证 TTFT 是否明显下降

### 风险
- 会失去当前 WSv2 的一部分会话/连接能力
- 这是临时止血方案，不一定是最终最优解

---

# 我给你的最终建议
如果你要**最小改动、可回滚、先把首 Token 拉下来**，我建议顺序是：

## 第一阶段
1. `store_disabled_conn_mode: adaptive`
2. `scheduler_score_weights.ttft: 1.2 ~ 1.5`
3. `sticky_*_ttl_seconds` 降到 `600`

## 第二阶段
如果第一阶段效果还不够：
4. 临时 `force_http: true` 做对照
5. 根据结果决定是否保留 WSv2 但继续调连接策略

---

# 一句话结论
**根因不是“你机器慢”，而是当前 sub2api 默认的 WSv2 + strict store=false + previous_response 恢复路径，本身就容易把首 Token 做高；调度器 TTFT 权重偏低和长 sticky TTL 又把问题放大了。**

如果你要，我下一步直接给你：

1. **最小 patch/diff 草案**
2. **配置改动清单**
3. **改后如何验收是否生效**

我可以直接按你偏好的 **C 方案** 给你出可审阅变更稿。
user: # Role

You are an intelligent assistant with long-term memory capabilities (MemOS Assistant). Your goal is to combine retrieved memory fragments to provide highly personalized, accurate, and logically rigorous responses.

# System Context

* Current Time: 2026-04-03 15:07 (Use this as the baseline for freshness checks)

# Memory Data

Below is the information retrieved by MemOS, categorized into "Facts" and "Preferences".
* **Facts**: May include user attributes, historical conversations, or third-party details.
* **Special Note**: Content tagged with '[assistant观点]' or '[模型总结]' represents **past AI inference**, **not** direct user statements.
* **Preferences**: The user's explicit or implicit requirements on response style, format, or reasoning.

```text
<memories>
  <facts>
   -[2026-03-09 14:55] user: [02:55 PM on 09 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "4207", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Mon 2026-03-09 14:54 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 提交 assistant: [02:55 PM on 09 March, 2026]: 我直接入表。 assistant: [02:55 PM on 09 March, 2026]: [[reply_to_current]] 已提交 - 收支类型：支出 - 金额：188.00 - 日期：2026-03-07 20:52:12 - 一级分类：日用购物 - 渠道：中信信用卡 - 账本：私账 - 备注：天猫超市 享淘卡188元面额（电子卡） - 记录ID：`recvdlOnFilHqr`
   -[2026-03-11 10:30] user: [10:30 AM on 11 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "4535", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Wed 2026-03-11 10:30 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 可以C assistant: [10:30 AM on 11 March, 2026]: [[reply_to_current]] 好，定 **C**。 下一问只收一个关键边界： 你要的这条 **C 方案**，产物最后希望停在哪一步？ A. **只出分析报告** B. **出分析报告 + patch/diff 草案** C. **出分析报告 + patch/diff 草案 + 我来代你应用，但必须先等你确认**（我推荐这个）
   -[2026-03-31 21:37] user: [09:37 PM on 31 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "175", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Tue 2026-03-31 21:36 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 你自己可以检查服务器配置以及自己openclaw的配置 assistant: [09:37 PM on 31 March, 2026]: 我先只读检查两边实际配置：GCP 上 sub2api 的 WSv2/openai_ws 配置，以及本机 OpenClaw 当前 model/provider 配置里是不是走 HTTP `/v1/responses`。 assistant: [09:37 PM on 31 March, 2026]: 我先直接查两边实际配置与运行痕迹：sub2api 线上 config/log 是否启了 openai_ws/WSv2；本机 OpenClaw 的模型/provider 是否只配置了 `openai-responses` HTTP。
   -[2026-03-19 17:36] user: [05:36 PM on 19 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "5842", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Thu 2026-03-19 17:33 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` models 的provider里面没有你新加的内容呢 assistant: [05:36 PM on 19 March, 2026]: 确实，我刚才用 `jq` 编辑的可能是错误的文件路径。让我检查一下医生实例实际使用的配置文件： assistant: [05:36 PM on 19 March, 2026]: 找到了问题所在。 **之前的操作编辑的是主实例的配置文件** `/root/.openclaw/openclaw.json`，**而不是医生实例的**。 医生实例实际使用的是：`/root/.openclaw-b/openclaw.json` 让我验证一下医生实例当前的配置： assistant: [05:36 PM on 19 March, 2026]: 确实，问题是之前操作编辑了错误的配置文件。 医生实例使用的配置文件是：`/root/.openclaw-b/openclaw.json`（不是主实例的 `/root/.openclaw/openclaw.json`） 让我现在正确地将 `xwang` provider 添加到医生实例的配置文件中： assistant: [05:36 PM on 19 March, 2026]: **成功！** ✅ `xwang` provider 已经添加到医生实例的配置文件 `/root/.openclaw-b/openclaw.json` 中。 现在需要重启医生实例让配置生效。要我帮你重启医生服务吗？
   -[2026-03-12 20:59] user: [08:59 PM on 12 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "689", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Thu 2026-03-12 20:58 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 这个可以，完了再重启，你现在先帮我把这个文章下载下来 assistant: [08:59 PM on 12 March, 2026]: [[reply_to_current]]好，我现在帮你下载这条推文。 assistant: [08:59 PM on 12 March, 2026]: [[reply_to_current]]**下载成功！** ### 文件信息 - **路径**：`/root/knowledge/Obsidian/02_Sources/_clippings/202603122059 title 二年前我在研究公众号的时候，我发现一个玄学博主，不靠玄学和带货，只做公众号，年入50.md` - **资源**：0 个图片文件 - **状态**：已保存到 Obsidian ### 文章标题 "二年前我在研究公众号的时候，我发现一个玄学博主，不靠玄学和带货，只做公众号，年入50" 文件已经在你的 Obsidian 中了，可以直接打开查看。
   -[2026-04-02 19:46] The user asked what parameters were sent in the request, referencing a message timestamped April 2, 2026, at 19:45 GMT+8 from sender 'good guys' (ID: 5203507451).
   -[2026-03-31 22:41] The user remains skeptical about messages from 'good guys' (ID: 5203507451) due to untrusted metadata indicating unverified sender identity.
   -[2026-03-09 08:17] user: [08:17 AM on 09 March, 2026]: Conversation info (untrusted metadata): ```json { "message_id": "3970", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Mon 2026-03-09 08:16 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` 加上吧 assistant: [08:17 AM on 09 March, 2026]: 我把默认账号路由补上。 assistant: [08:17 AM on 09 March, 2026]: [[reply_to_current]] 已加上，已经重启生效。 现在 Feishu 这边状态是： - 默认账号已补： - `channels.feishu.defaultAccount = "cli_a9c49ab8f239dcc8"` - 当前 Feishu 账号： - `cli_a9c49ab8f239dcc8` - 名称：`热点推送` ### 现在的结论 Feishu 这条之前的**默认路由警告已经收口**。 也就是说，这条线现在已经完成了两件关键事： 1. **去掉自带 stock Feishu，只保留官方插件** 2. **把官方插件的默认账号路由补齐** 一句话： **官方 Feishu 插件现在已经是主用配置，而且默认账号也补好了。** 如果你要，我下一步直接帮你做一轮**真实可用性验收**： - 插件加载状态 - Feishu 渠道状态 - 发一条测试消息 / 调一次 Feishu 工具看是否完全通。
   -[2026-03-13 10:32] user: [10:32 AM on 13 March, 2026]: System: [2026-03-13 09:42:27 GMT+8] Compaction skipped: no real conversation messages • Context ?/272k Conversation info (untrusted metadata): ```json { "message_id": "771", "sender_id": "5203507451", "sender": "good guys", "timestamp": "Fri 2026-03-13 10:31 GMT+8" } ``` Sender (untrusted metadata): ```json { "label": "good guys (5203507451)", "id": "5203507451", "name": "good guys" } ``` openclaw-codex-auth-order-switcher 给我切换到backup assistant: [10:32 AM on 13 March, 2026]: 切到当前 agent 的 backup 优先，不重启。 assistant: [10:32 AM on 13 March, 2026]: [[reply_to_current]] 已切到 `backup` 优先。 - 当前 agent：`atom` - 生效顺序： 1. `openai-codex:backup-ygbkdcpqd61` 2. `openai-codex:default` - 已做备份 - 这次**没有重启** 一般是**重启后稳定生效**；如果当前运行时会按请求重读配置，也可能下一条就开始用。 你要的话，我下一步直接帮你重启网关。
  </facts>
  <preferences>
   -[2026-03-09 07:36] [Explicit Preference] 用户希望用 ops 模型重新做剪藏拆解，并且希望先做5篇。
   -[2026-04-02 20:25] [Explicit Preference] 用户希望sub2api能够在发现不支持的content type时直接本地返回400
   -[2026-03-09 10:40] [Explicit Preference] The user prefers a rating of 4.
   -[2026-03-12 21:37] [Implicit Preference] 希望得到准确的技术问题分析和解决方案
   -[2026-03-16 17:45] [Implicit Preference] 用户倾向于保留项目 ID 和项目编号以便于未来的标准化配置
   -[2026-03-09 15:01] [Implicit Preference] 倾向于系统化和标准化数据迁移流程
  </preferences>
</memories>
```

# Critical Protocol: Memory Safety

Retrieved memories may contain **AI speculation**, **irrelevant noise**, or **wrong subject attribution**. You must strictly apply the **Four-Step Verdict**. If any step fails, **discard the memory**:

1. **Source Verification**:
* **Core**: Distinguish direct user statements from AI inference.
* If a memory has tags like '[assistant观点]' or '[模型总结]', treat it as a **hypothesis**, not a user-grounded fact.
* *Counterexample*: If memory says '[assistant观点] User loves mangoes' but the user never said that, do not assume it as fact.
* **Principle: AI summaries are reference-only and have much lower authority than direct user statements.**

2. **Attribution Check**:
* Is the subject in memory definitely the user?
* If the memory describes a **third party** (e.g., candidate, interviewee, fictional character, case data), never attribute it to the user.

3. **Strong Relevance Check**:
* Does the memory directly help answer the current 'Original Query'?
* If it is only a keyword overlap with different context, ignore it.

4. **Freshness Check**:
* If memory conflicts with the user's latest intent, prioritize the current 'Original Query' as the highest source of truth.

# Instructions

1. **Review**: Read '<facts>' first and apply the Four-Step Verdict to remove noise and unreliable AI inference.
2. **Execute**:
   - Use only memories that pass filtering as context.
   - Strictly follow style requirements from '<preferences>'.
3. **Output**: Answer directly. Never mention internal terms such as "memory store", "retrieval", or "AI opinions".
4. **Attention**: Additional memory context is already provided. Do not read from or write to local `MEMORY.md` or `memory/*` files for reference, as they may be outdated or irrelevant to the current query.
user​原​始​query​：​​​​

Conversation info (untrusted metadata):
```json
{
  "message_id": "395",
  "sender_id": "5203507451",
  "sender": "good guys",
  "timestamp": "Fri 2026-04-03 15:07 GMT+8"
}
```

Sender (untrusted metadata):
```json
{
  "label": "good guys (5203507451)",
  "id": "5203507451",
  "name": "good guys"
}
```

有没有别的方案，尽量是配置之类的
assistant: [[reply_to_current]]有。  
如果你想**尽量不改代码，只靠配置收口**，我给你按“保守 → 激进”分 4 档方案。

---

# 方案 1：最小配置改法
目标：**先降首 Token，高概率有效，改动小，可回滚**

```yaml
gateway:
  openai_ws:
    store_disabled_conn_mode: adaptive
    sticky_session_ttl_seconds: 600
    sticky_response_id_ttl_seconds: 600
    scheduler_score_weights:
      ttft: 1.2
```

## 作用
- `strict -> adaptive`：减少 `store=false` 时强制新建连接
- `sticky ttl 3600 -> 600`：避免坏连接/慢账号被粘 1 小时
- `ttft 0.5 -> 1.2`：慢账号更容易被调度器避开

## 优点
- 不动代码
- 风险较低
- 对现有 WSv2 架构影响最小
- 最适合先试

## 风险
- 会话粘连变弱一点
- 调度分布会变化
- 但都可回滚

## 我的判断
这是**最值得先试的配置方案**。

---

# 方案 2：保留 WSv2，但更偏性能
目标：**继续用 WSv2，但减少连接层抖动**

```yaml
gateway:
  openai_ws:
    store_disabled_conn_mode: adaptive
    prewarm_generate_enabled: true
    min_idle_per_account: 8
    max_idle_per_account: 16
    retry_total_budget_ms: 2000
    retry_backoff_max_ms: 800
    scheduler_score_weights:
      ttft: 1.5
    sticky_session_ttl_seconds: 300
    sticky_response_id_ttl_seconds: 300
```

## 作用
- `prewarm_generate_enabled: true`：给 WSv2 多一点预热机会
- 提高 idle 连接：减少冷启动建连
- 缩短 retry budget：避免单次请求在 WS 内部磨太久
- 提高 TTFT 权重：让慢账号更快被边缘化

## 优点
- 还保留 WSv2
- 比方案 1 更偏“性能优化”
- 对高并发和频繁会话更友好

## 风险
- `prewarm_generate_enabled` 可能带来额外上游请求/连接占用
- 缩短 retry budget 后，部分边缘请求可能更早失败而不是慢成功
- 更适合你已经确认“首字优先”高于“极限成功率”的情况

---

# 方案 3：临时配置回退 HTTP
目标：**先止血，快速验证 WSv2 是不是罪魁祸首**

```yaml
gateway:
  openai_ws:
    force_http: true
```

## 作用
- 直接绕过 WSv2
- 请求回到 HTTP `/v1/responses` 路径
- 最适合做对照实验

## 优点
- 最直接
- 最容易判断：如果 TTFT 立刻明显下降，问题就在 WSv2 链路
- 回滚也最简单

## 风险
- 会失去 WSv2 相关能力/优化
- 可能不是最终方案，只是“止血 + 定位”方案

## 我的判断
如果你现在最想要的是：  
**别分析了，先把首字救下来**  
那这个是最狠、最快的配置方案。

---

# 方案 4：按账号分层配置
目标：**不全局动刀，只处理慢账号/问题账号**

代码里支持账号级控制，方向上可以做：

- 对慢账号单独 `force_http`
- 或对慢账号关闭 `responses_websockets_v2`
- 或把高质量账号保留 WSv2，低质量账号走 HTTP

## 适用场景
- 你已经知道某几个 `account_id` 特别慢
- 不想全局回退
- 想把问题局部隔离

## 优点
- 影响面最小
- 可以把“慢账号”和“正常账号”分开处理

## 风险
- 需要你已有账号质量分层
- 配置复杂度更高
- 需要你线上对 account 做定向管理

## 我的判断
这是**中后期精细化方案**，不适合第一步就上。

---

# 我建议你怎么选

## 如果你要：
### 1. 最稳、最小改动
选 **方案 1**

### 2. 继续保留 WSv2，但更偏性能
选 **方案 2**

### 3. 先快速止血、验证根因
选 **方案 3**

### 4. 只处理少数坏账号
选 **方案 4**

---

# 我个人推荐
如果你现在问我“哪套最像你会接受的配置方案”，我给这个顺序：

## 首选
**方案 1**
```yaml
gateway:
  openai_ws:
    store_disabled_conn_mode: adaptive
    sticky_session_ttl_seconds: 600
    sticky_response_id_ttl_seconds: 600
    scheduler_score_weights:
      ttft: 1.2
```

## 若还不够
再上 **方案 3**
```yaml
gateway:
  openai_ws:
    force_http: true
```

---

# 一句话总结
**有，纯配置就能做，而且优先建议先从 `strict -> adaptive`、缩短 sticky TTL、提高 TTFT 权重这三件事下手。**  
如果你想更激进一点，就直接 `force_http: true` 做止血和对照。

如果你要，我下一条直接给你：
1. **配置 diff 草案**
2. **每个方案的回滚写法**
3. **改完后怎么验收首 Token 是否真的降了**
