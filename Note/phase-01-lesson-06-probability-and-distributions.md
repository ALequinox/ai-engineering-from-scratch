# 第 1 阶段 · 第 6 课：概率与概率分布

- 学习日期：2026-09-30
- 原课程：`phases/01-math-foundations/06-probability-and-distributions/docs/en.md`
- 课后测验：3/3；上节课热身：2/2。
- 本课目标：理解事件与分布，计算条件概率、期望和方差，并将 logits 转换为稳定的类别概率和交叉熵损失。

## 1. 样本空间、事件与条件概率

样本空间是所有可能结果的集合；事件是其中一部分结果。公平骰子的样本空间为 `{1,2,3,4,5,6}`，偶数事件为 `{2,4,6}`，概率是 `3/6=1/2`。

条件概率把范围缩小到条件已经发生后的可能结果：

```text
P(A | B) = P(A 且 B) / P(B)，其中 P(B)>0
```

课堂例子：已知掷出偶数，剩下 `{2,4,6}`；其中大于 3 的结果为 `{4,6}`，所以条件概率是 `2/3`；小于 3 的结果只有 `{2}`，概率为 `1/3`。第一次计算“大于 3”时误用了原样本空间，答为 `1/6`，已订正。

两件事独立时，知道其中一件不会改变另一件的概率：`P(A 且 B)=P(A)P(B)`。两次公平抛硬币都为正面的概率是 `1/2×1/2=1/4`。

## 2. 离散 PMF 与连续 PDF

**概率质量函数（PMF）**给离散结果分配单点概率，例如公平骰子的 `P(X=2)=1/6`。所有可能结果的概率之和为 1。

**概率密度函数（PDF）**描述连续变量。密度在某一点的值不是那个点的概率；区间概率是区间下方的面积。所有可能值上的总面积为 1。

```text
X 在 [0,2] 连续均匀分布：区间内密度为 1/2。
P(0≤X≤1) = 区间宽度 1 × 密度 1/2 = 1/2。
P(X=1) = 0，因为单个点的宽度为 0。
```

密度数值可以大于 1，仍不代表概率超过 1。

## 3. 常见分布与手写函数

- **伯努利分布**：两个结果，`P(X=1)=p`、`P(X=0)=1−p`。课堂上 p=0.8 时答出 `P(0)=0.2`；p=0.7 的代码计算结果为 0.3。
- **类别分布**：多个互斥类别，各概率非负且总和为 1。`[0.1,0.7,0.2]` 中类别编号 1 的概率是 0.7。
- **连续均匀分布**：在 `[a,b]` 内的密度为 `1/(b−a)`。
- **正态分布**：钟形曲线，均值 μ 控制中心，标准差 σ 控制宽度。标准正态在 x=0 的密度约 0.398942，在 x=3 处约 0.004432。
- **泊松分布**：用于固定区间内的事件计数；参数 λ 是平均次数。λ=2 时零次事件的概率是 `exp(−2)≈0.135335`。

课堂使用标准库运行了简化实现；其中伯努利对非法 p 报错，对非法结果 k 返回 0；泊松对非法 k/λ 报错：

```python
import math

def bernoulli_pmf(k, p):
    if not 0 <= p <= 1:
        raise ValueError("p 必须在 0 到 1 之间")
    if k == 1:
        return p
    if k == 0:
        return 1 - p
    return 0.0

def categorical_pmf(k, probs):
    return probs[k] if 0 <= k < len(probs) else 0.0

def uniform_pdf(x, a, b):
    if b <= a:
        raise ValueError("b 必须大于 a")
    return 1 / (b - a) if a <= x <= b else 0.0

def poisson_pmf(k, lam):
    if k < 0 or int(k) != k or lam < 0:
        raise ValueError("参数无效")
    if lam == 0:
        return 1.0 if k == 0 else 0.0
    return lam**k * math.exp(-lam) / math.factorial(k)

def normal_pdf(x, mu, sigma):
    if sigma <= 0:
        raise ValueError("标准差必须大于 0")
    scale = 1 / (sigma * math.sqrt(2 * math.pi))
    distance = (x - mu) / sigma
    return scale * math.exp(-0.5 * distance**2)
```

类别分布的示意函数假设 `probs` 本身合法；完整实现还应检查各项非负且总和接近 1。

## 4. 期望、方差、联合与边缘分布

期望是按概率加权的平均；方差是与均值的平方距离再取期望：

```text
E[X] = Σ x·P(X=x)
Var(X) = E[(X−E[X])²]
```

公平骰子的期望是 `(1+2+3+4+5+6)/6=3.5`，即使实际不会掷出 3.5。`X∈{1,3}` 且各有 0.5 概率时，期望 2、方差 1。课堂初次计算方差答为 0.5，只算了一个结果的贡献；两项各贡献 0.5，合计 1。`X∈{0,4}` 且各有 0.5 概率时，每个平方差为 4，方差为 4。

```python
def expected_value(values, probabilities):
    return sum(x * p for x, p in zip(values, probabilities))

def variance(values, probabilities):
    mu = expected_value(values, probabilities)
    return sum(p * (x - mu) ** 2
               for x, p in zip(values, probabilities))
```

联合分布描述多个变量同时取值。例：下雨且不带伞的概率为 0.05，下雨且带伞为 0.45；只看天气的边缘概率时，把带伞与否加总，得到 `P(下雨)=0.05+0.45=0.50`。

## 5. 抽样与中心极限定理

按类别分布抽样时，先把概率累加成区间边界，再生成一个 0 到 1 的随机数，选择它落入的类别。对 `[0.5,0.3,0.2]`，边界为 `0.5、0.8、1.0`；随机数 0.65 选类别 1，0.95 选类别 2。

在适当独立性和有限方差等条件下，许多同分布样本的平均值的分布逐渐接近正态分布。每次掷 30 枚公平骰子求平均，会比单次掷骰更集中在 3.5 附近。使用固定随机种子分别模拟 10,000 组：

```text
每组 1 枚：均值 3.5408，方差 2.9279。
每组 30 枚：均值 3.4986，方差 0.0986。
```

中心极限定理有适用条件，不能说任何原始分布都无条件收敛到正态。样本均值方差在独立同分布、有限方差条件下等于单个样本方差除以样本量。

## 6. Log 概率、softmax 与交叉熵

多个小概率连乘可能最终发生浮点下溢。取 log 后，乘法变成加法：`log(p₁p₂…pₙ)=Σlog(pᵢ)`。在 `(0,1]` 内，概率越大，log 概率越接近 0；例如 −2 比 −5 对应更高概率。

模型输出的原始分数称为 logits。softmax 将其转成类别概率：

```text
softmax(zᵢ) = exp(zᵢ) / Σexp(zⱼ)
```

所有 logits 减去同一个最大值，不改变结果，却能避免巨大指数溢出。例如 `[1000,1001,1002] → [-2,-1,0]`；注意 `exp(102)` 在常见 float64 下不会溢出，课堂因此使用 1000 以上的示例。

```python
def softmax(logits):
    largest = max(logits)
    shifted = [z - largest for z in logits]
    exps = [math.exp(z) for z in shifted]
    total = sum(exps)
    return [value / total for value in exps]

def log_softmax(logits):
    largest = max(logits)
    shifted = [z - largest for z in logits]
    log_total = largest + math.log(sum(math.exp(z) for z in shifted))
    return [z - log_total for z in logits]

def cross_entropy_loss(logits, target_index):
    return -log_softmax(logits)[target_index]
```

课堂最初误认为平移后的 0 对应最小概率；比较 `exp(−2)≈0.135`、`exp(−1)≈0.368`、`exp(0)=1` 后已订正：原始分数越大，softmax 概率越大。

手写运行结果：

```text
softmax([1000,1001,1002]) = [0.090031,0.244728,0.665241]，总和 1。
log-softmax = [−2.407606,−1.407606,−0.407606]。
正确类别编号 0：交叉熵 2.407606。
正确类别编号 2：交叉熵 0.407606。
```

单个正确类别的交叉熵 `−log(P(正确类别))`：模型越不相信正确类别，损失越高。PyTorch 对相同 logits 的 `softmax` 与 `torch.nn.functional.cross_entropy` 实际输出与手写结果一致。PyTorch 的交叉熵函数接收原始 logits，不要先把 softmax 概率传入。

## 7. 本课测验与完成范围

2026-09-30 课程 post 测验 **3/3**：

1. softmax 减去最大 logit：选择 B，保持结果且避免指数溢出。
2. 正确类别的交叉熵：选择 B，预测概率越低损失越大。
3. 语言模型使用 log 概率：选择 A，避免长期小概率连乘下溢，将乘法变为加法。

已完成：中文互动计算、阅读与预测手写 PMF/PDF、期望/方差、softmax 与交叉熵代码；导师运行标准库示例、中心极限定理抽样模拟及 PyTorch 对照。学习者尚未独立编写这些函数。

待实践：独立实现输入验证完整的分布函数；用样本检验概率和为 1、期望与方差；用 PyTorch 验证多个正确类别的交叉熵；比较不同抽样次数的样本均值分布。以上未记为已完成。

下一课：贝叶斯定理与统计思维（Bayes' Theorem & Statistical Thinking）。
