# My AI Engineering Path
<!-- Managed by the ai-engineering-from-scratch learning skills.
     Repo: https://github.com/rohitg00/ai-engineering-from-scratch -->

## Mission
ship an AI product and change careers

希望完成的作品：An agent, a trained model, a RAG product。
教学语言：中文；必要时附英文术语。每天学习 2 小时，每周约 14 小时。
学习笔记：每课完成后，自动在当前项目的 `Note/` 文件夹生成一篇中文 Markdown 笔记（不存在则新建目录），命名为 `phase-NN-lesson-MM-英文课名.md`。记录本课概念、逐步计算示例、代码与实际运行情况、易错点、测验结果和后续练习；区分已完成与待练习内容。重学时更新对应笔记，保留历史测验记录，并在进度日志中链接笔记。

## Placement
- Date: 2026-09-28
- Score: 0/10（数学与统计 0/2；经典机器学习 0/2；深度学习 0/2；NLP 与 Transformer 0/2；AI 应用 0/2）
- Entry point: Phase 1: Math Foundations
- Pace: ~14 hours/week
- 测验记录：Q1–Q10 均回答“不知道”；Q4 曾反馈选项显示问题，已提供重答机会，未收到修订答案。

## Path
| Phase | Name | Status | Est. hours |
|-------|------|--------|------------|
| 0 | Setup & Tooling | Skip | -- |
| 1 | Math Foundations | Do | 23 |
| 2 | ML Fundamentals | Do | 21 |
| 3 | Deep Learning Core | Do | 15 |
| 4 | Computer Vision | Do | 27 |
| 5 | NLP | Do | 30 |
| 6 | Speech & Audio | Do | 18 |
| 7 | Transformers Deep Dive | Do | 14 |
| 8 | Generative AI | Do | 14 |
| 9 | Reinforcement Learning | Do | 13 |
| 10 | LLMs from Scratch | Do | 26 |
| 11 | LLM Engineering | Do | 19 |
| 12 | Multimodal AI | Do | 65 |
| 13 | Tools & Protocols | Do | 43 |
| 14 | Agent Engineering | Do | 55 |
| 15 | Autonomous Systems | Do | 20 |
| 16 | Multi-Agent & Swarms | Do | 28 |
| 17 | Infrastructure & Production | Do | 32 |
| 18 | Ethics, Safety & Alignment | Do | 31 |
| 19 | Capstone Projects | Do | 620 |

预计总计：~1114 小时，19 个阶段（按 ROADMAP.md 各阶段标题逐项求和）。按每周 14 小时约需 79.6 周；实际随练习与项目范围调整。这不是转行时间承诺。
Phase 0 按定位规则标为 Skip，不代表已验证工具环境；首次实践时仍需检查必要工具。

## Progress log
| Date | Lesson | Quiz | Note |
|------|--------|------|------|
| 2026-09-28 | 01-math-foundations/01-linear-algebra-intuition | 3/3 | 已完成中文互动讲解；能计算点积、投影并解释秩与 LoRA；已纠正“正交=不相关”的表述及列表与数组乘法区别。导师运行了手写点积与投影；NumPy 未安装，仅推演，尚未独立编写完整实现。[学习笔记](Note/phase-01-lesson-01-linear-algebra-intuition.md) |
| 2026-09-28 | 01-math-foundations/02-vectors-matrices-operations | 1/2 | 能手算矩阵乘法、转置和前向层；偏置曾误作统一加数，已纠正。行列式为零误选单位变换，需复习。导师运行手写层，NumPy 仅推演，尚未独立完成 Matrix 类。[学习笔记](Note/phase-01-lesson-02-vectors-matrices-operations.md) |
| 2026-09-29 | 01-math-foundations/03-matrix-transformations | 3/3 | 能解释特征值、特征分解与 PCA 方向选择；旋转方向、反射轴与负数分量相加曾出错，纠正后答对。导师运行特征值与分解验证，NumPy 仅阅读，尚未独立实现。[学习笔记](Note/phase-01-lesson-03-matrix-transformations.md) |

| 2026-09-29 | 01-math-foundations/04-calculus-for-ml | 3/3 | 能计算一维更新、批次梯度、链式法则与线性回归梯度；海森矩阵混合正负曲率起初误判为局部最小，经过提示已订正为鞍点。中央差分、数值梯度与批次回归由导师实跑；尚未独立实现优化器。[学习笔记](Note/phase-01-lesson-04-calculus-for-ml.md) |
| 2026-09-29 | 01-math-foundations/05-chain-rule-and-autodiff | 3/3 | 能解释链式法则、反向模式和梯度累积；拓扑排序作用起初不清楚，经计算图解释后继续作答正确。导师运行仓库自动微分实现，含梯度检查、XOR 和 PyTorch 对照；尚未独立实现引擎。[学习笔记](Note/phase-01-lesson-05-chain-rule-and-autodiff.md) |
| 2026-09-30 | 01-math-foundations/06-probability-and-distributions | 3/3 | 能区分 PMF/PDF、计算条件概率、期望与方差，并解释稳定 softmax、交叉熵和对数概率；条件概率分母、方差求和及 softmax 排序曾出错，已订正。导师运行手写函数、抽样模拟和 PyTorch 对照；尚未独立实现。[学习笔记](Note/phase-01-lesson-06-probability-and-distributions.md) |
| 2026-09-30 | 01-math-foundations/07-bayes-theorem | 3/3 | 能用基准率解释阳性后约 0.98% 的患病概率、计算垃圾邮件后验与 Beta 顺序更新；健康人误报率起初误用 0.99，垃圾邮件后验方向起初判断偏低，均已订正。导师运行标准库分类器及已安装的 scikit-learn 对照；尚未独立编写分类器。[学习笔记](Note/phase-01-lesson-07-bayes-theorem.md) |
| 2026-09-30 | 01-math-foundations/08-optimization | 3/3 | 能计算梯度下降、动量和 Adam 首步，解释小批次、余弦退火与鞍点；题设学习率 1 曾误代为 0.1，已订正。导师运行手写优化器与 PyTorch 对照；尚未独立实现。[学习笔记](Note/phase-01-lesson-08-optimization.md) |
| 2026-10-01 | 01-math-foundations/09-information-theory | 3/3 | 能计算熵、交叉熵、KL、困惑度与互信息；熵的加权求和、KL 变化方向及 P=Q 时 KL 为零曾答错，均已订正。写出困惑度的 Python 表达式；导师运行手写程序与 NumPy 对照，尚未独立实现完整函数。[学习笔记](Note/phase-01-lesson-09-information-theory.md) |

## Review queue

- 01-math-foundations/02-vectors-matrices-operations（2026-09-28，1/2；2026-09-29 已复习通过）：复习行列式为 0、信息丢失与不可逆的关系；对照单位矩阵的行列式为 1。[笔记](Note/phase-01-lesson-02-vectors-matrices-operations.md)。

  - 复习证据：正确回答行列式为 0 时不可逆，并区分行列式为 1 与单位矩阵；保留原测验记录，后续间隔回忆。
