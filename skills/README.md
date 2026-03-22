# 阿杰 skills policy

## 目标
让 `coder` 既继承全局通用能力，又能放置自己的专属 coding 能力。

## 分层规则
### 1. 全局共享能力（shared / global）
默认复用主 workspace 的通用 skills，适合所有 agent：
- 通用 OpenClaw 使用规则
- 通用浏览器 / Feishu / Google Workspace / account-switcher 等技能
- 不带强角色偏向的能力

### 2. coder 专属能力（local / workspace-coder）
放在 `/root/.openclaw/workspace-coder/skills`，只服务 `coder`：
- 仓库级 coding SOP
- patch / diff / review 规范
- 多语言项目排查模板
- repo root 约束与工作入口约束
- 不应给 `main` / `atom` 默认继承的编程专属流程

## 当前策略
- 现阶段：先复用全局 shared skills
- 后续新增 `coder` 专属 skill 时，一律优先落到本目录
- 若某个 skill 已被多个 agent 复用，再考虑提升到全局 shared

## repo root 约束
- 默认代码工作根目录：`/root/knowledge/dev-tasks`
- 默认按**仓库名**进入，而不是按模糊任务目录乱入
- 只有目录明确是单任务隔离区且没有独立仓库时，才按任务目录进入

## 标准 coding 流程
1. 识别目标仓库 / 技术栈
2. 读取关键入口（README、构建文件、主代码目录）
3. 明确任务边界与验收标准
4. 设计最小改动方案
5. 实施改动
6. 跑针对性验证（测试 / 构建 / 静态检查）
7. 输出变更摘要、风险与下一步
