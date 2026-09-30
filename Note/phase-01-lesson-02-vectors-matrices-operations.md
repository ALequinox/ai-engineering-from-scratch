# 第 1 阶段 · 第 2 课：向量、矩阵与运算

- 学习日期：2026-09-28
- 原课程：`phases/01-math-foundations/02-vectors-matrices-operations/docs/en.md`
- 目标：理解矩阵操作，并用手写 Python 计算一个神经网络层。
- 课后测验：1/2（50%）；行列式与可逆性需要复习。

## 1. 热身回忆

点积题选择了“距离”，已纠正：点积反映对齐程度，也受长度影响，不是距离。线性相关题回答正确：`[2,1,0] = 2×[1,0,0] + [0,1,0]`。

## 2. 逐元素乘法与矩阵乘法

逐元素乘法保留各位置的贡献；点积把对应位置的乘积相加，得到一个数。

```text
[2,4,5] 与 [1,1,0]：
逐元素乘法得到 [2,4,0]
点积得到 2+4+0 = 6
```

矩阵乘法用左矩阵的行与右矩阵的列做点积。课堂手算：

```text
A = [[1,2], [3,4]]
B = [[5,6], [7,8]]
左上：1×5 + 2×7 = 19
右上：1×6 + 2×8 = 22
左下：3×5 + 4×7 = 43
右下：3×6 + 4×8 = 50
A @ B = [[19,22], [43,50]]
```

形状规则：`(m,n) @ (n,p) → (m,p)`，左矩阵列数必须等于右矩阵行数。

- `(3,2) @ (2,5)` 合法，结果为 `(3,5)`。
- `(2,3) @ (2,2)` 不合法，中间维度 3 与 2 不相等。

## 3. 三层循环如何计算矩阵乘法

Python 索引从 0 开始。`A[0][1]` 表示第一行第二个数。

```python
result = []
for i in range(len(A)):
    row = []
    for j in range(len(B[0])):
        total = 0
        for k in range(len(B)):
            total += A[i][k] * B[k][j]
        row.append(total)
    result.append(row)
```

这里假设 A、B 非空、每行长度一致，且形状允许相乘。

- `i` 选左边的行，`j` 选右边的列，`k` 累加一个点积。
- 每个新位置必须重新设置 `total = 0`。
- 如果只在所有循环外初始化一次，第二个位置会在 19 上继续累加，错误地得到 41，而不是 22。
- `row.append(total)` 收集一个数；`result.append(row)` 收集一整行。

导师实际运行了计算第一个位置的循环，输出为 19，与学习者预测一致。

## 4. 转置

转置交换行与列：原来第 i 行第 j 列的数移到第 j 行第 i 列。形状从 `(m,n)` 变为 `(n,m)`。

```text
[[1,2,3], [4,5,6]] → [[1,4], [2,5], [3,6]]
[[1,2], [3,4]] → [[1,3], [2,4]]
```

课堂阅读的类方法如下；这部分未在课堂工具调用中单独运行：

```python
    def transpose(self):
        result = []
        for j in range(self.cols):
            new_row = []
            for i in range(self.rows):
                new_row.append(self.data[i][j])
            result.append(new_row)
        return Matrix(result)
```

## 5. 单位矩阵、逆矩阵和行列式

单位矩阵是主对角线为 1、其余为 0 的方阵，与形状兼容的矩阵或向量相乘时不改变它们。

```text
I = [[1,0], [0,1]]
I @ [3,4] = [3,4]
```

逆矩阵撤销原矩阵的变换。例如横坐标乘 2、纵坐标乘 3，可以通过分别乘 1/2、1/3 撤销。

```text
S = [[2,0], [0,3]]
S⁻¹ = [[1/2,0], [0,1/3]]
S⁻¹ @ S = I
```

如果变换丢失信息，就无法唯一恢复输入。例如 `[[1,0], [0,0]]` 会把所有纵坐标变成 0，`[3,4]` 和 `[3,8]` 都变成 `[3,0]`。

对二维方阵：

```text
A = [[a,b], [c,d]]
det(A) = a×d − b×c
```

二维情况下，行列式绝对值表示面积缩放倍数；负号表示方向翻转。对于方阵：

- 行列式为 0：不可逆，也称奇异矩阵。
- 行列式不为 0：可逆；负数也可以。
- 单位矩阵的行列式为 1，不是 0。

当行列式非零，二维逆矩阵公式为：

```text
A⁻¹ = (1 / det(A)) × [[d,-b], [-c,a]]
```

课堂例子：

```text
A = [[1,2], [3,4]]
det(A) = 4−6 = −2
A⁻¹ = [[−2,1], [1.5,−0.5]]
A @ A⁻¹ = [[1,0], [0,1]]
```

## 6. 偏置与广播

偏置是加到计算结果上的调整量，在模型中通常是可学习参数。给每行加同一个偏置向量，指的是每行都加完整的向量，而不是每个元素都加同一个数。

```text
[[1,2,3], [4,5,6]] + [10,20,30]
→ [[11,22,33], [14,25,36]]
```

课堂曾将第二行答为 `[14,15,16]`，已纠正为 `[14,25,36]`。随后练习 `[2,4,6] + [1,10,100]` 回答 `[3,14,106]`，正确。

NumPy 广播从最右边对齐维度，每对维度必须相等或其中一个为 1；缺少的维度按 1 处理。

- `(2,3) + (3,) → (2,3)`：偏置应用到每行。
- `(2,1) + (2,) → (2,2)`：不是给列向量逐项加偏置的预期形状。

```text
[[9], [6]] + [-10,1]
→ [[-1,10], [-4,7]]
```

若希望得到 `[[-1],[7]]`，偏置应写成形状 `(2,1)` 的 `[[-10],[1]]`。广播并非任意修复形状，能运行也不代表计算意图正确。

## 7. 一个神经网络层的前向计算

```text
output = relu(W @ x + b)
```

矩阵乘法后加偏置，再逐元素执行 ReLU：`relu(z) = max(0,z)`。负数变成 0，非负数保持原值。

```text
W = [[0,1,1], [1,1,0]]，形状 (2,3)
x = [[2], [4], [5]]，形状 (3,1)
b = [[-10], [1]]，形状 (2,1)

W @ x = [[9], [6]]
加偏置 = [[-1], [7]]
ReLU 输出 = [[0], [7]]，形状 (2,1)
```

如果分数为 `[9,6]`，偏置改成 `[-5,-10]`，加偏置后为 `[4,-4]`，ReLU 输出 `[4,0]`。导师实际运行过该例，预测正确。

这属于给定参数后的前向计算，不是训练。训练还需要根据误差更新权重与偏置。

## 8. 课堂组合运行的手写代码

以下简化实现假设输入非空、每行长度一致；矩阵乘法与加法会检查形状。此加法不支持广播。

```python
class Matrix:
    def __init__(self, data):
        self.data = [list(row) for row in data]
        self.rows = len(self.data)
        self.cols = len(self.data[0])
        self.shape = (self.rows, self.cols)

    def matmul(self, other):
        if self.cols != other.rows:
            raise ValueError("左矩阵列数必须等于右矩阵行数")
        result = []
        for i in range(self.rows):
            row = []
            for j in range(other.cols):
                total = 0
                for k in range(self.cols):
                    total += self.data[i][k] * other.data[k][j]
                row.append(total)
            result.append(row)
        return Matrix(result)

    def __add__(self, other):
        if self.shape != other.shape:
            raise ValueError("两个矩阵的形状必须相同")
        result = []
        for i in range(self.rows):
            row = []
            for j in range(self.cols):
                row.append(self.data[i][j] + other.data[i][j])
            result.append(row)
        return Matrix(result)


def relu_matrix(matrix):
    result = []
    for row in matrix.data:
        new_row = [max(0, value) for value in row]
        result.append(new_row)
    return Matrix(result)


W = Matrix([[0, 1, 1], [1, 1, 0]])
x = Matrix([[2], [4], [5]])
b = Matrix([[-10], [1]])
scores = W.matmul(x)
adjusted = scores + b
output = relu_matrix(adjusted)
print(scores.data)
print(adjusted.data)
print(output.data)
print(output.shape)
```

导师实际运行结果，与学习者预测一致：

```text
[[9], [6]]
[[-1], [7]]
[[0], [7]]
(2, 1)
```

`__init__` 初始化对象，`self` 表示当前对象，`__add__` 让 `A+B` 调用加法方法。这里使用 `W.matmul(x)`，尚未实现让手写类支持 `W @ x` 的 `__matmul__` 方法。

## 9. NumPy 对应写法

```python
import numpy as np

W = np.array([[0, 1, 1], [1, 1, 0]])
x = np.array([[2], [4], [5]])
b = np.array([[-10], [1]])
output = np.maximum(0, W @ x + b)
```

NumPy 接管矩阵相乘、偏置加法和逐元素 ReLU 的循环；使用者仍需确保形状正确。本课只阅读与推演此版本，未实际运行 NumPy。

## 10. 测验与复习重点

2026-09-28：课后测验 1/2。

1. 行列式为 0 能推断什么？回答 C（单位变换），错误；正确答案 A：奇异矩阵，不可逆。
2. 转置如何改变矩阵？回答 B（交换行与列），正确。

优先复习：单位矩阵保留所有信息，行列式是 1；行列式为 0 的方阵至少压掉一个维度，无法恢复所有输入信息。

建议对照 `[[1,0],[0,1]]` 与 `[[1,0],[0,0]]`，分别计算 `[3,4]` 和 `[3,8]` 的输出，再说明哪一个可逆。此建议练习尚未完成。

## 11. 完成范围与待实践

已完成：中文互动概念讲解、手算矩阵乘法与逆矩阵验证、逐段阅读矩阵类、预测并核对导师执行的前向计算。

待实践：独立编写完整 Matrix 类；为非空与规则形状添加检查；补上逐元素乘法、行列式和逆矩阵方法；运行转置与 NumPy 对照；尝试两层网络。本课没有独立完成上述实现，不将其记为已完成。

下一课：矩阵变换与特征值（Matrix Transformations & Eigenvalues）。
