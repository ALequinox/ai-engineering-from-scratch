# 第 13 阶段 · 第 22 课：Agent Skills 的可移植契约与运行时边界

- 学习日期：2026-09-30
- 原课程：[Agent Skills: Portable Contract and Runtime Boundary](../phases/13-tools-and-protocols/22-skills-and-agent-sdks/docs/en.md)
- 学习目标：区分技能包的可移植结构、主机发现与调用、工具权限和最终验证。
- 本课结果：创建并验证了最小技能；在 Codex 中安装、显式调用并卸载完整评审包。课程演示与 19 个测试通过；六题测验 2/6。

## 1. Agent Skill 是一个包，不是一段保存的提示词

Agent Skill 的可移植单位是**目录**，入口是 `SKILL.md`。入口文件由 YAML frontmatter 和 Markdown 操作说明组成；目录还可包含 `references/`、`scripts/`、`assets/`。只复制入口文件而漏掉它引用的资源，会得到一个不完整的包。

本课创建了 [`my-first-skill/SKILL.md`](../agent-skills-first-run/my-first-skill/SKILL.md)。它的 `name` 是 `my-first-skill`，与目录名一致；`description` 同时写清任务和触发条件：用户要把粗略会议记录整理成决策记录时使用。

```yaml
---
name: my-first-skill
description: Turn rough meeting notes into a compact decision record when the user asks to capture a technical decision.
---
```

可移植的必填字段是 `name` 和 `description`。`name` 应使用稳定的短横线命名形式并匹配目录名，避免发现和打包时出现两个身份。正文负责说明步骤、分支、失败处理及输出。`license`、`compatibility`、`metadata` 等是可选字段；`allowed-tools` 的主机支持仍需验证。某主机独有的 `user-invocable` 等字段只是该主机的扩展，不能据此推断其他主机也实现同样行为。

## 2. 从发现到验证是不同阶段

1. **发现**：主机在配置的位置找候选目录。
2. **校验**：检查名称、必填字段、正文及包结构。
3. **编入目录**：向模型提供简短的名称和描述等路由信息。
4. **选择**：判断当前请求是否适合这个技能。
5. **激活**：把选中技能的 `SKILL.md` 正文载入上下文。
6. **披露资源**：任务需要时才读对应参考文件或素材。
7. **执行**：通过主机提供的工具运行脚本或访问外部能力。
8. **验证**：检查真实输出、路径和退出码，而不只相信模型的自述。

**发现不等于激活；激活不等于获得权限；执行成功也不自动证明结果正确。** 技能可以描述如何运行脚本，但真正的文件、网络和工具权限仍由主机决定。

## 3. 相邻机制怎样分工

- **一次性提示词**：只服务当前交互，不需要包的生命周期。
- **`AGENTS.md`**：在仓库工作期间持续适用的约定，例如本仓库的课程格式与提交规则。
- **Agent Skill**：针对某类任务可重复使用的判断流程，例如整理决策记录或评审技能包。
- **普通代码**：稳定、确定性的转换或校验，例如检查 frontmatter。
- **MCP 工具**：提供带输入契约的外部数据或操作能力，例如查询拉取请求 API。
- **Hook**：在主机定义的事件发生时运行，例如每次工具调用后必须检查的逻辑。
- **Subagent**：把有边界的工作放进独立上下文执行。
- **Plugin**：分发更大的主机扩展；它可以包含技能，但不等于技能的可移植契约。

一个任务可能组合多种机制。例如“按固定方法评审拉取请求，同时查询远端数据”：方法放在 Skill，远端查询交给 MCP 工具；本仓库的通用规则继续由 `AGENTS.md` 提供。

## 4. Build It：校验结构，再选择机制

课程的 `code/main.py` 用 Python 标准库实现 frontmatter 解析、技能文本校验和机制选择。校验应先发现结构性错误，再讨论更深的内容；报告返回具体问题代码，而不是只给一个布尔值。`TaskShape` 和 `select_primitives` 根据任务是否需要模型判断、事件触发、外部能力、隔离上下文或确定性处理来选机制。

从课程目录运行：

```bash
cd phases/13-tools-and-protocols/22-skills-and-agent-sdks
python3 code/main.py
python3 -m unittest discover -s code/tests -v
```

课堂记录：演示退出码 0，19 个测试通过。最小技能的源代码校验与**已安装**评审包内的 `check_skill.py` 校验均返回 `valid: true`、`errors: []`、退出码 0。

## 5. Use It：真实主机检查了什么

实验工作区为 `agent-skills-first-run`，即 `TARGET_ROOT`。评审包安装到该工作区的项目范围，包含入口、参考文件、脚本和素材；`SKILL_ROOT` 必须指向**已安装**的 `skill-contract-reviewer/SKILL.md` 所在目录，不能用课程源码目录冒充。

运行包内脚本时，目标和脚本都解析为绝对路径：

```bash
python3 "$SKILL_ROOT/scripts/check_skill.py" "$TARGET_ROOT/my-first-skill"
```

新 Codex 会话发现并显式调用了评审技能。主机报告确认：目标包有效、零结构错误、选择机制为 **Agent Skill**，理由是“把会议记录整理成决策记录”属于可重复、需要判断的流程。报告还给出解析后的脚本路径、目标路径、`cwd`、完整 `argv` 和退出码；精确证据见[学习进度](../AGENT-SKILLS-LEARNING.md)。主机报告的 `cwd` 是仓库根目录，而非实验目录，绝对路径使执行目标仍然明确。

随后仅卸载 `skill-contract-reviewer`。安装器列表变为空，技能目录消失；新的 Codex 会话中它不再出现，显式调用也提示不可用。学习者创建的 `my-first-skill` 保留下来。

## 6. 本课易错点与复习

本课测验为 **2/6**。需要重点复习三点：

1. `name` 匹配目录名解决**包身份**，不保证每个主机使用相同的调用语法。
2. 主机独有字段只在对应适配器中有意义，不能自动升级为可移植规范。
3. `AGENTS.md` 适合仓库范围的常驻约定；罕见的专项任务流程更适合 Skill。任何指令文件都不能借此绕过主机权限或沙箱。

复习时自问：一个技能包通过结构校验后，是否已经获得脚本执行权？如果一个工作流程既有评审方法又要访问远端 API，各部分分别由谁提供？

下一课：[13/24 技能发现与渐进式披露](phase-13-lesson-24-skill-discovery-and-progressive-disclosure.md)。
