# ~/.claude 全局配置说明

> 此文件仅供人工阅读，不被 Claude Code 自动加载。

---

## 目录结构

```
~/.claude/
├── CLAUDE.md                  # 入口文件，Claude Code 自动加载
├── global-rules.md            # 规则索引（通过 @ 引用 rules/ 下各文件）
├── automation-commands.md     # 可用 slash 命令说明
├── rules/                     # 按主题拆分的规则文件
│   ├── interaction.md         # 语言设置、执行优先规则、提问/犯错处理
│   ├── coding.md              # 代码规范 + 格式规则 + Karpathy 11 条准则
│   ├── git.md                 # Git 提交规则
│   ├── service.md             # 服务管理规则
│   ├── data.md                # 测试数据 + 生产数据保护
│   ├── docs.md                # 文档生成规则
│   ├── config.md              # 本地配置管理（local.toml）
│   ├── model.md               # 模型分配 + 成本优化
│   └── brain-selection.md     # 左脑 Claude vs 右脑 Codex 选择规则
├── projects/                  # 各项目的 memory 文件
└── README.md                  # 本文件（仅供参考）
```

---

## 加载机制

```
Claude Code 启动
  └── 自动读取 ~/.claude/CLAUDE.md
        ├── @automation-commands.md
        └── @global-rules.md
              ├── @rules/interaction.md
              ├── @rules/coding.md
              ├── @rules/git.md
              ├── @rules/service.md
              ├── @rules/data.md
              ├── @rules/docs.md
              ├── @rules/config.md
              ├── @rules/model.md
              └── @rules/brain-selection.md
  └── 自动读取 项目目录/CLAUDE.md（叠加，不覆盖全局）
```

---

## 配置优先级

```
项目子目录 CLAUDE.md  >  项目根目录 CLAUDE.md  >  全局 ~/.claude/CLAUDE.md
```

---

## 规则文件说明

| 文件 | 内容 |
|------|------|
| `rules/interaction.md` | 中文回复、执行前确认、thinking 级别、提问/犯错/搜索规则 |
| `rules/coding.md` | 禁用 emoji、英文命名、Node v22、格式规则、Karpathy 11 条 |
| `rules/git.md` | 只 commit 不 push |
| `rules/service.md` | make restart、不在 Claude 终端启动服务 |
| `rules/data.md` | test_ 前缀、生产数据事务保护 |
| `rules/docs.md` | 文档存 docs/、更新 README.md 和 CLAUDE.md |
| `rules/config.md` | local.toml 统一管理凭证，不硬编码 |
| `rules/model.md` | opusplan 模式、Haiku 子代理、成本控制 |
| `rules/brain-selection.md` | 左脑 Claude vs 右脑 Codex 选择规则 |

---

## 修改规则

直接编辑 `rules/` 下对应文件，**新对话后生效**（已打开的对话不会重新加载）。

---

## 项目级配置

在项目根目录创建 `CLAUDE.md`，只写项目特有规则，全局规则自动生效无需重复引用。

**双工具项目（同时使用 Claude + Codex）推荐结构：**

```
project/
├── CLAUDE.md              # 左脑入口：@.project/shared.md @.project/left-brain.md
├── AGENTS.md              # 右脑入口：完整内容（Codex 不支持 @ 引用）
└── .project/
    ├── shared.md          # 唯一来源：全局共享规则 + 双脑选择指南
    ├── left-brain.md      # Claude 专属：分析/审查/架构
    └── right-brain.md     # Codex 专属：生成/实现/执行
```

**单工具项目（仅 Claude）：**

```markdown
# 项目 CLAUDE.md 示例

## 技术栈
- Go 1.22 + Gin
- PostgreSQL 16

## 项目特定规则
- API 响应统一用 pkg/response 包
- 数据库操作必须在 service 层，不能在 handler 直接查询
```
