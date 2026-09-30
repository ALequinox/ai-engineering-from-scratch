# 第 1 阶段 · 第 5 课：链式法则与自动微分

- 学习日期：2026-09-29
- 原课程：`phases/01-math-foundations/05-chain-rule-and-autodiff/docs/en.md`
- 课后测验：3/3；上节课热身 2/2。
- 目标：理解计算图、反向模式自动微分、梯度累积和梯度检查，并运行手写引擎与 XOR 小网络。

## 1. 链式法则

复合函数的导数沿每一段相乘：

```text
y=f(g(x))：dy/dx = (dy/dg)×(dg/dx)
y=sin(x²)
内层 a=x²，da/dx=2x
外层 y=sin(a)，dy/da=cos(a)
dy/dx = cos(x²)×2x
```

在 x=2 时，a=4，`da/dx=4`，所以导数为 `4cos(4)`。课堂回答正确识别 4 是 x→a 的内层导数。

## 2. 计算图与反向传播

计算图中节点代表值或运算，前向计算从输入流向输出，反向传播把梯度从输出传回输入。

```text
x1=2 ─┐
      × → a=6 → +1 → b=7 → ReLU → y=7
x2=3 ─┘
```

反向计算：ReLU 在正输入处导数为 1，加法输入的导数为 1，乘法 `a=x1×x2` 对 x1 的局部导数是 x2，对 x2 的局部导数是 x1。因此 `dy/dx1=3`、`dy/dx2=2`。

反向模式从输出开始，按拓扑依赖关系逆序传播。一个节点要先收到下游所有贡献，再把完整梯度传给更早的节点。课堂最初不清楚拓扑排序的目的，随后通过依赖顺序解释。

反向模式适合很多输入参数、一个输出损失的神经网络；前向模式适合少量输入、很多输出。对偶数可在前向模式中携带数值和导数：

```text
(a,a′)×(b,b′) = (ab, a′b + ab′)
```

## 3. Value 自动微分节点

简化的 `Value` 对象保存数值、梯度、父节点和局部反向函数：

```python
class Value:
    def __init__(self, data, children=(), op=''):
        self.data = float(data)
        self.grad = 0.0
        self._backward = lambda: None
        self._prev = set(children)
        self._op = op
```

加法与乘法的反向规则：

```python
def add_backward(left, right, out):
    left.grad += out.grad
    right.grad += out.grad

def multiply_backward(left, right, out):
    left.grad += right.data * out.grad
    right.grad += left.data * out.grad
```

乘法来自乘积求导。若 `out.grad=2`、left=2、right=3，则 left 收到 6，right 收到 4。ReLU 正区间传递梯度，负区间传 0；课程实现将零点导数设为 0。

使用 `+=` 而非 `=` 是因为一个节点可能被多个后续运算使用，需要把每条路径的梯度贡献相加。例如 `y=a×a`，a=4，两条乘法输入路径各贡献 4，合计 `dy/da=8`。

## 4. 拓扑排序与 backward

从最终输出递归访问所有依赖，先把输入节点追加，再把最终节点追加，得到拓扑顺序。反转后即可从输出向输入传播：

```python
topo = []
visited = set()

def build_topo(v):
    if v not in visited:
        visited.add(v)
        for child in v._prev:
            build_topo(child)
        topo.append(v)

build_topo(output)
output.grad = 1.0
for node in reversed(topo):
    node._backward()
```

把输出梯度设为 1，因为 `d(output)/d(output)=1`。训练每个新批次前要将参数 `.grad` 清零；否则新梯度会与前一批次残留梯度累加。课程代码对参数显式重置。

## 5. 常见反向规则

课程 `Value` 类还用已有运算组合实现减法和除法，或直接记录幂、指数、对数、tanh 的局部导数：

```text
x^n：n×x^(n−1)
exp(x)：exp(x)
log(x)：1/x
tanh(x)：1−tanh(x)^2
ReLU：输入为正时 1，否则 0
```

每个新运算都必须为前向数值和局部反向梯度定义规则。

## 6. 梯度检查

梯度检查比较自动微分结果和中央差分近似：

```text
数值梯度 ≈ [f(x+h)−f(x−h)]/(2h)
差异 = |自动梯度−数值梯度|
```

较大差异可能意味着反向规则实现错误；很小的误差常由有限差分近似和浮点精度造成。适合在添加新运算、调试不收敛时使用，正式训练中逐参数检查太慢。

## 7. 源码实际运行

运行命令：

```bash
python3 phases/01-math-foundations/05-chain-rule-and-autodiff/code/autodiff.py
```

结果摘要：

```text
基础图 y=relu(x1*x2+1)：y=7，dy/dx1=3，dy/dx2=2
x^3 在 x=2：y=8，dy/dx=12
复杂 ReLU 图：f=4，df/da=−3，df/db=2，df/dc=1
单神经元示例：预激活为 −1.4，ReLU 关闭，梯度为 0
5 个表达式的自动微分/数值差分比较均通过，最大误差约 1.17e−9
XOR 训练损失：step 0 为 4.1491，step 99 为 0.1783
四个 XOR 输入预测符号都正确
与 PyTorch 对照：两边梯度均为 dy/dx1=3、dy/dx2=2
脚本结束：All demos passed.
```

XOR 模型用 2 个输入、4 个隐藏神经元、1 个输出，tanh 激活，100 轮梯度下降。它说明自动微分引擎能支持小型网络训练；这只是教学规模的实现。

## 8. 本课测验与学习者表现

热身两题均正确：梯度是偏导向量；导数表示局部变化率。

课程 post 测验 3/3：

1. 为什么用反向模式？选择 B：很多输入、一个输出时，一次反传可得到全部参数梯度。
2. 为什么梯度用 `+=`？选择 A：多条计算路径的贡献要相加。
3. 什么是梯度检查？选择 B：把自动梯度与数值差分比较，验证反向计算。

学习者正确算出 `sin(x²)` 在 x=2 时的内层导数、简单计算图的 x1/x2 梯度、乘法梯度缩放和共享节点梯度累积；也能说明新批次不清零会混入上一轮梯度。

## 9. 已完成与待实践

已完成：交互推导、读取 Value 节点和基本反向规则、解释拓扑排序和梯度累积、运行自动微分完整演示、通过课程测验。

待实践：独立从空文件实现 Value 类；新增 tanh 或幂运算并做梯度检查；尝试从头训练 XOR 并检查梯度清零。课堂运行的是仓库已有实现，学习者尚未独立编写该引擎。

下一课：概率与概率分布（Probability & Distributions）。
