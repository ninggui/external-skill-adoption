# 外部技能评估

![GitHub stars](https://img.shields.io/github/stars/ninggui/external-skill-adoption)
![License](https://img.shields.io/github/license/ninggui/external-skill-adoption)
[![SkillHub](https://img.shields.io/badge/SkillHub-在线安装-blue)](https://skillhub.cn/skills/external-skill-adoption)

评估外部插件/技能并决定是否采纳：搜索、比选、试装、兼容性实测。

## 这是什么

一个可复用的 AI Agent 技能（Skill），来自真实业务场景沉淀，含完整执行流程、避坑清单与验证步骤。

## 快速使用

将本仓库放入 Agent 技能目录后，用对应触发词调用（见 SKILL.md），Agent 会自动加载并执行完整流程。

## 核心能力

| 能力 | 说明 |
|------|------|
| SkillHub/ClawHub 搜索比选 |
| 生态兼容性判断（OpenClaw/Claude/Hermes） |
| 安装后实测验证 |
| 隐私与安全扫描 |

## 使用方式（安装）

- **Hermes**: 放入 `skills/` 目录
- **Claude**: 放入 `~/.claude/skills/`
- **其他 Agent**: 按对应 SKILL.md 格式放入技能目录
- **SkillHub 一键安装**: https://skillhub.cn/skills/external-skill-adoption

## 优势

- 避免"装了一堆用不上"
- 方法论可迁移，脚本绑定生态如实标注
- 含 10 维评分 rubric

## 内容结构

- `SKILL.md` — 核心技能定义（触发条件、执行流程、避坑清单）
- `references/` — 可选参考文件

## 许可

MIT
