# Convergent Code Review · 收敛式代码审核

以真实缺陷和有效证据为核心，按变更风险安排审核，让修复与复审能够及时结束。

这是从 [Claude Code 团队的审核文章](https://claude.dev/blog/solving-code-review-at-anthropic/) 提炼并独立编写的中文 Agent Skill。适用于 Codex 及其他支持 Agent Skills 的工具，专注代码审核与流程改进，可单独使用。

## 如何工作

1. 明确本次变更、实际候选、相关项目规则和已有结论。
2. 按风险选择检查深度，读取能证明行为的有效证据。
3. 对疑似缺陷核对触发条件和后果，减少误报及风格噪声。
4. 修复后复核问题、差异和受影响行为；旧证据仍适用时复用。
5. 满足条件立即结束；默认最多三轮，仍有阻断项时报告待决问题。

三轮是上限，不是必跑次数。审核通过也不能替代项目要求的 CI、人工批准或发布授权。

用户要求改进团队流程时，还可采用 [流程落地参考](references/adoption.md)：项目审核规则、检查分工、合并与发布条件、恢复安排，以及用真实故障和误报评估审核者。

## 安装

```bash
npx skills add lulu233camellia/convergent-code-review --skill convergent-code-review -g -a codex
```

也可以把完整仓库克隆到 Skill 目录；以下示例要求目标目录尚不存在：

```bash
git clone https://github.com/lulu233camellia/convergent-code-review.git ~/.codex/skills/convergent-code-review
```

其他兼容工具使用其自己的 Skill 目录。安装后若未被识别，重新打开会话或重载宿主。代理、GitHub 与 CI 能力由宿主提供，本仓库没有后台服务或自动安装脚本。

## 使用示例

```text
使用 $convergent-code-review 审核当前差异，先读取已有测试结果；这次只输出结论。
```

```text
使用 $convergent-code-review 复核这些修复，沿用上一轮的问题记录，只扩大有具体依据的检查范围。
```

```text
使用 $convergent-code-review 分析仓库为什么反复审核，给出适合本项目的 REVIEW.md 和 CI 分工建议。
```

与多 Agent 开发流程同时使用时，共享当前问题记录、候选和审核轮数，避免两个流程各自启动完整复审。它不依赖 `multi-agent-dev`，也不承担需求规划和应用开发编排。

## 文件

```text
convergent-code-review/
├── SKILL.md                     # 风险、证据、核验与结束规则
├── agents/openai.yaml           # Codex 界面元数据
├── references/review-state.md   # 问题、证据与结论模板
├── references/adoption.md       # 团队流程落地与改进
├── references/source.md         # 来源及适配说明
├── README.md
└── LICENSE
```

## 验证范围

发布前检查 Skill 结构、相对引用、界面元数据和公开文件内容。本仓库尚无真实项目的提速、缺陷检出或回滚率基准；结构检查不证明审核效果。

规则由执行 Agent 遵循，不是运行时硬性限流器，也不会自动配置 GitHub 保护规则。参考 [来源说明](references/source.md) 了解与文章团队实践的区别。

## License

本仓库原创内容采用 [MIT](LICENSE)。来源文章保留原有权利。本项目与 Anthropic、Claude Code 和 OpenAI 没有官方关联。
