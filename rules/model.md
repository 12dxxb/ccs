# 模型与成本规则

## 模型分配（opusplan 模式）
- 全局模型设为 `opusplan`：Plan 模式自动用 Opus，执行阶段自动用 Sonnet
- 子代理模型设为 `haiku`（通过 CLAUDE_CODE_SUBAGENT_MODEL 配置）
- Opus：架构设计、需求分析、方案评估（Plan 模式自动切换）
- Sonnet：代码编写、修改、调试、测试（执行阶段自动切换）
- Haiku：所有子代理任务（文件搜索、代码搜索、状态检查、日志查看等）
- 强制切换：`/model opus` 或 `/model sonnet`

## 成本优化
- MAX_THINKING_TOKENS=10000，日常任务 medium，复杂问题 high
- autocompact 设为 50%，长对话自动压缩防止 token 累积
- 活跃 MCP 控制在 5 个以内，优先使用 CLI 工具（gh, aws, gcloud）替代 MCP
- 复杂任务先进 Plan 模式（Shift+Tab）再实现
- 方向错误时立即 Escape 停止，不浪费 token
- 不相关任务之间用 `/clear` 重置上下文，用 `/rewind` 回退而非重新解释
