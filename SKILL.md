---
name: external-skill-adoption
description: 评估外部插件/技能并决定是否采纳。触发：学习一下X插件、看哪个更有用、装GitHub skill、比选插件。
slug: external-skill-adoption
displayName: 外部技能评估
version: 1.0.0
---

# 外部插件/技能评估与采纳

用户常给一个外部工具/插件/技能（GitHub、DeepSeek Harness 生态、SkillHub），要求"学习一下、运用一下、看哪个对我更有用"。本 skill 定义完整评估闭环：定位 → 审查 → 安装 → A/B 实测 → 知识库沉淀 → 结论。

## 评估流程（编号步骤）

1. **定位本体**
   - 先 `search_files` 查本地是否已有同名目录
   - 再用 GitHub API 确认仓库本体：
     - 仓库搜索：`curl -s 'https://api.github.com/search/repositories?q=<查询>&per_page=10'`
     - README 获取：`curl -s 'https://api.github.com/repos/<owner>/<repo>/readme'` → JSON `content` 字段 base64 解码
   - **区分整合包与本体**：如 `dsh-router-jspace` 是 J-Space + dsh-router + oh-we-need 的整合 preset，不是独立插件本体。查上游 README 里的链接链回本体。

2. **判断依赖边界**（决定"能不能装"）
   - 是否绑定特定运行时（如 DSH 插件系统 dsh plugin / 注入器）？→ Hermes 装不了，只能借鉴方法论
   - 是否模型无关（打包为通用 Skill）？→ 可直接装 Hermes
   - 是模型专属还是对照组（如 V4 Pro 专属研究、Flash 为对照）？→ 影响收益预期

3. **浅克隆审查**
   - `git clone --depth 1 <url>`，看目录结构 + 脚本内容
   - 安全检查：脚本是否纯标准库？有无网络调用/写敏感路径/可疑系统命令？
   - **跑自带验证脚本**（如 `verify_suite.py`）确认包完整性，再决定安装

4. **安装到 Hermes**
   - `cp -r <repo>/<skill-dir> /home/user/skills/<name>`（同名目录先确认再覆盖）
   - **装完必须 `skill_view(name)` 确认 `readiness_status: available`，不能只 ls 目录**

5. **A/B 实测（关键步骤，验证"装了到底有没有用"）**
   - 用两个**隔离子代理**跑同一任务：对照组不加载新 skill；实验组在 context 里明确指示 `skill_view` 加载并按协议执行
   - **用户铁律：delegate_task 串行派发，禁并发**（先对照组，后实验组）
   - 任务选择：能区分普通执行与协议执行的真实任务（多文件审计 / 多步推理 / 需要状态保持），子代理能独立完成
   - 评分维度：结论数、准确性（**父代必须抽查验证，子代理自报不可信**）、是否实际验证动作、结构化程度、可落地性
   - 对照组先跑：本会话若已 skill_view 过目标 skill，"安装前"状态已被污染，必须用子代理隔离环境

6. **知识库沉淀**：学习笔记 → 飞书文档 + 多维表格（走 knowledge-base-pipeline，用户已授权不询问）

7. **结论**：对比表（维度：现在能用 / 依赖 / 收益 / 维护成本）+ 明确建议。给结论不给过程。

## 避坑
- **lark-cli 建文档**：二进制在 `/home/user/bin/lark-cli`（当前 shell PATH 可能不含）；`--content @path` **只接受相对路径**（须 cd 到工作目录用 `./xxx.md`，或 `--content -` 走 stdin）；绝对路径报 invalid_argument
- 飞书多维表格 `+record-upsert` 的 `--json` 用**中文字段名**（文档名称/关键词/备注/创建日期/文档链接/文档类型/标签）
- 实验组子代理不会主动加载 skill，必须在 context 里写明"先 skill_view(name) 再严格按其协议执行"，否则 A/B 失效
- 外部 skill 安装后不要改其内部文件（如 J-Space 有 verify_suite.py 校验 premise 逐字一致，改了会挂）

## 参考文件
- `references/jspace-dsh-2026-08.md` — J-Space / dsh-router 研究细节、安装记录、A/B 测试设计与实测结果
