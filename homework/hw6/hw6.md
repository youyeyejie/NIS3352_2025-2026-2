# Assignment 6

## Problem 1 (25 marks)
$C \subseteq F_2^5$，的校验矩阵 $H=\begin{pmatrix}1 & 0 & 1 & 1 & 0 \\ 0 & 1 & 1 & 1 & 0 \\ 1 & 1 & 0 & 1 & 1 \end{pmatrix}$

### Question 1
计算单比特错误向量 $e_1$ 到 $e_5$ 的伴随式。

**解**：

- $S(e_1) = H e_1^T = (1, 0, 1)^T$
- $S(e_2) = H e_2^T = (0, 1, 1)^T$
- $S(e_3) = H e_3^T = (1, 1, 0)^T$
- $S(e_4) = H e_4^T = (1, 1, 1)^T$
- $S(e_5) = H e_5^T = (0, 0, 1)^T$

### Question 2
计算 $r = (1, 0, 1, 1, 1)$ 的伴随式。假设最多发生了一位错误，对 $r$ 进行解码。

**解**：
$$
S(r) = H r^T
= \begin{pmatrix} 1 & 0 & 1 & 1 & 0 \\ 0 & 1 & 1 & 1 & 0 \\ 1 & 1 & 0 & 1 & 1 \end{pmatrix} \begin{pmatrix} 1 \\ 0 \\ 1 \\ 1 \\ 1 \end{pmatrix}
= \begin{pmatrix} 3 \\ 2 \\ 3 \end{pmatrix}
\equiv \begin{pmatrix} 1 \\ 0 \\ 1 \end{pmatrix} \pmod 2
$$

因此，$S(r) = S(e_1)$，则有 $c = r - e_1 = (0, 0, 1, 1, 1)$

---

## Problem 2 (25 marks)
$C$ 的生成矩阵 $G = \begin{pmatrix} 1 & 0 & 1 & 1 & 0 \\ 0 & 1 & 1 & 0 & 1 \end{pmatrix}$

### Question 1
列出 $C$ 的所有码字。

**解**：

- $m_0 = (0, 0) \implies c_0 = (0, 0, 0, 0, 0)$
- $m_1 = (0, 1) \implies c_1 = (0, 1, 1, 0, 1)$
- $m_2 = (1, 0) \implies c_2 = (1, 0, 1, 1, 0)$
- $m_3 = (1, 1) \implies c_3 = (1, 1, 0, 1, 1)$


### Question 2
确定参数 $[n, k, d]$。

**解**：最小距离 $d=\min\{\mathrm{wt}_H(c) : c \in C, c \neq 0\} = \min\{3, 3, 4\} = 3$，因此该码的参数为 $[5, 2, 3]$。

### Question 3
求校验矩阵 $H$。

**解**：生成矩阵 $G$ 已是标准形式 $G = \begin{pmatrix} I_k & P \end{pmatrix}$，$P = \begin{pmatrix} 1 & 1 & 0 \\ 1 & 0 & 1 \end{pmatrix}$，因此校验矩阵为
$$
H = \begin{pmatrix} -P^\top & I_{n-k} \end{pmatrix} = \begin{pmatrix} P^\top & I_{3} \end{pmatrix} = \begin{pmatrix} 1 & 1 & 1 & 0 & 0 \\ 1 & 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 & 1 \end{pmatrix}
$$

### Question 4
计算 $r = (1, 1, 0, 0, 1)$ 的伴随式，并使用最小重量的陪集首部进行解码。

**解**：

$$
S(r) = H r^T = \begin{pmatrix} 1 & 1 & 1 & 0 & 0 \\ 1 & 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} 1 \\ 1 \\ 0 \\ 0 \\ 1 \end{pmatrix} = \begin{pmatrix} 2 \\ 1 \\ 2 \end{pmatrix} \equiv \begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix} \pmod 2
$$

伴随式对应 $H$ 的第 4 列，因此最小重量的陪集首部为 $e = (0, 0, 0, 1, 0)$。解码得：$c = r - e = (1, 1, 0, 1, 1)$。

---

## Problem 3 (25 marks)
在 $\mathbb{F}_7$ 上的 $[6, 3]$ Reed-Solomon 码，求值点为 $0, 1, 2, 3, 4, 5$。发送由多项式 $f(x)$ 生成的码字，$\deg(f) < 3$，接收字为 $r = (2, 6, 5, 6, 2, 6)$。

### Question 1
假设最多发生一个符号错误，恢复发送的多项式 $f(x)$。

**解**：设 $f(x) = cx^2 + bx + a \pmod 7$。任选前 3 个点尝试插值：

$$
\begin{cases}
f(0) = a \equiv 2 \implies a = 2 \\
f(1) = c + b + 2 \equiv 6 \implies c + b \equiv 4 \\
f(2) = 4c + 2b + 2 \equiv 5 \implies 4c + 2b \equiv 3
\end{cases}
$$

解得 $\begin{cases} a = 2 \\ b = 3 \\ c = 1 \end{cases}$，得多项式 $f(x) = x^2 + 3x + 2$。使用其余点进行验证无误得：

$$
\begin{cases}
f(3) = 9 + 9 + 2 = 20 \equiv 6 \pmod 7 = y_3 \\
f(4) = 16 + 12 + 2 = 30 \equiv 2 \pmod 7 = y_4 \\
f(5) = 25 + 15 + 2 = 42 \equiv 0 \pmod 7 \neq y_5 = 6
\end{cases}
$$

则 $f(x) = x^2 + 3x + 2$，且错误发生在 $x=5$ 处。

### Question 2
确定错误的位置和错误值。

**解**：由上一问，错误发生在 $x=5$ 处，错误值为 $e = r_5 - f(5) = 6 - 0 = 6$。

### Question 3
验证接收字和解码出的码字之间的汉明距离未超过该码的纠错能力。

**解**：$c = (2, 6, 5, 6, 2, 0)$，$r = (2, 6, 5, 6, 2, 6)$，则 $d_H(r, c) = \mathrm{wt}(r - c) = 1$。RS 码最小汉明距离 $d_{\min} = n - k + 1 = 4$，纠错能力 $t = \lfloor (d_{\min}-1)/2 \rfloor = 1 \geq d_H(r, c)$，因此满足纠错能力要求。

---

## Problem 4 (25 marks)
在 $\mathbb{F}_7$ 上，Reed-Solomon 码的求值点为 $\alpha_1=0, \alpha_2=1, \alpha_3=2, \alpha_4=3, \alpha_5=4$。消息多项式 $\deg(f) < 2$。假设最多发生一个符号错误，接收字 $y = (1, 3, 0, 0, 2)$。令 $E(x) = e_1x + e_0, N(x) = a_0 + a_1x + a_2x^2$。

### Question 1
使用 Berlekamp-Welch 条件 $E(\alpha_i)y_i = N(\alpha_i)$ 列出关于未知数 $e_1, e_0, a_0, a_1, a_2$ 的线性方程组。

**解**：将 $\alpha_i, y_i$ 依次代入 $y_i (e_1 \alpha_i + e_0) \equiv a_0 + a_1 \alpha_i + a_2 \alpha_i^2 \pmod 7$：

$$
\begin{cases}
e_0 = a_0 \\
3e_1 + 3e_0 = a_0 + a_1 + a_2 \\
0 = a_0 + 2a_1 + 4a_2 \\
0 = a_0 + 3a_1 + 2a_2 \\
e_1 + 2e_0 = a_0 + 4a_1 + 2a_2
\end{cases}
$$

### Question 2
求出所有的非零解 $(E(x), N(x))$。

**解**：解上述线性方程组，得

$$
\begin{cases}
e_0 = a_0 = 6a_2 \\
e_1 = 2a_1 = 4a_2
\end{cases} \implies \begin{cases}
E(x) = 4a_2 x + 6a_2 \\
N(x) = a_2 x^2 + 2a_2 x + 6a_2
\end{cases},\quad a_2 \in \mathbb{F}_7^*
$$

### Question 3
恢复传输的多项式 $f(x)$。

**解**：因为 $E(2) = a_2 \cdot (8 + 6) = 0$，$N(2) = a_2 \cdot (4 + 4 + 6) = 0$，则

$$
\begin{aligned}
f(x) &= \frac{N(x)}{E(x)} = \frac{a_2(x^2 + 2x + 6)}{a_2(4x + 6)} \\
&= \frac{x^2 + 2x + 6}{4x + 6} \\
&\equiv \frac{(x-2)(x-3)}{4(x-2)} \pmod 7 \\
&\equiv 4^{-1}(x-3) \pmod 7 \\
&\equiv 2x + 1 \pmod 7
\end{aligned}
$$

### Question 4
确定错误的位置和错误值。

**解**：由 $f(x) = 2x + 1$ 可得 $f(0) = 1, f(1) = 3, f(2) = 5 \neq y_2 = 0, f(3) = 0, f(4) = 2$，因此错误发生在 $\alpha_3 = 2$ 处，错误值为 $e = y_2 - f(2) = 0 - 5 = 2$。