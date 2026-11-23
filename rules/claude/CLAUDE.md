# Claude Code 工作流（与 Cursor 独立）

> **隔离说明**：Claude 配置在 `~/.claude/`，Cursor 在 `~/.cursor/`。二者共用工作区 `.Ai_C/` 目录结构，但 **hook 状态文件互不共享**（`.claude-workflow-state.json` vs `.cursor-workflow-state.json`）。

## 计划工作流

1. **plan-create**：改业务前先 Write `.Ai_C/plans/YYYY-MM-DD_HHmmss-<topic>.md`  
   详见 `~/.claude/skills/plan-create/SKILL.md`
2. **用户确认**后（确认 / ok / 可以 / yes / 执行 / 开始 / 行）才可改业务文件  
   硬门禁：`~/.claude/hooks/workflow-guard.mjs`（via `~/.claude/settings.json`）
3. **plan-finish**：完工后只写 `.Ai_C/docs/plan-timeline.md` 与 `project-tech.md`  
   详见 `~/.claude/skills/plan-finish/SKILL.md`

## Unity 架构

- 规则：`~/.claude/rules/unity-architecture.mdc`（`**/*.cs` 时生效）

## 通用规范

1. 全程中文；文件引用格式 `[文件名](相对路径)`
2. 【业务代码】：业务逻辑、UI 脚本、数据配置、实体结构；纯注释/排版/文案微调/工具脚本豁免
3. 新需求 / 超出原计划范围 → 重新 plan-create，旧确认失效
4. 编码：`m_` 前缀私有成员；单文件单一职责；#region 分块
5. 运行安全：GetComponent 后空引用判断；Update 内禁止资源加载/循环查找/字符串拼接/new 集合
6. GC：循环内禁止临时 new 数组/List/字符串；高频运算优先 struct
