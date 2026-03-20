# Assignment 3

## Problem 1 (10 marks)
在 $\mathbb{F}_{11}$ 上的椭圆曲线 $\varepsilon: y^2 = x^3 + x + 6$ 上，给定 $P = (2, 7)$ 与 $Q = (5, 2)$，计算 $P + Q$。

**解**：

$\lambda = \frac{y_Q - y_P}{x_Q - x_P} \pmod{11} = \frac{-5}{3} \equiv 6\cdot 3^{-1} \equiv 24 \equiv 2 \pmod{11}$
$x_3 = \lambda^2 - x_P - x_Q \pmod{11} = 2^2 - 2 - 5 \pmod{11} = 8\pmod{11}$
$y_3 = \lambda(x_P - x_3) - y_P \pmod{11} = 2(2 - 8) - 7 \pmod{11} = 3\pmod{11}$

因此 $P + Q = (8, 3)$。

---

## Problem 2 (10 marks)
在 $\mathbb{F}_{5}$ 上的椭圆曲线 $\varepsilon: y^2 = x^3 + x + 4$ 上，证明 $P = (3, 2)$ 是一个三阶点。

**证明**：$P$ 是三阶点 $\iff 3P = O \wedge 2P \neq O \wedge P \neq O \iff 2P = -P \neq O$

- 计算 $2P$：
    - $\lambda = \frac{3x_P^2 + a}{2y_P} \pmod{5} = \frac{3 \times 3^2 + 1}{2 \times 2} \equiv \frac{28}{4} \equiv 3\cdot 4^{-1} \equiv 12 \equiv 2 \pmod{5}$
    - $x_{2P} = \lambda^2 - 2x_P \pmod{5} = 2^2 - 2\cdot 3 \equiv 3 \pmod{5}$
    - $y_{2P} = \lambda(x_P - x_{2P}) - y_P \pmod{5} = 2(3 - 3) - 2 \equiv 3 \pmod{5}$
    - 因此 $2P = (3, 3)$
- 计算 $-P = (x_P, -y_P-a_1x_P - a_3) $
    - 由于 $a_1=a_3=0$，因此 $y_{-P} = -y_P \equiv 3 \pmod{5}$
    - 因此 $-P = (3, 3)$

QED

---
## Problem 3 (10 marks)
定义对称配对 $e: G_1 \times G_2 \rightarrow G_T$ 满足 $\forall g \in G_1, e(g, g) = 1$，证明 $e$ 是对称配对 $\implies e(g, h) = e(h, g)^{-1}$。

**证明**：
$$
\begin{aligned}
1 &= e(g+h,g+h) \\
&= e(g,g) \cdot e(g,h) \cdot e(h,g) \cdot e(h,h) \\
&= e(g,h) \cdot e(h,g) \\
\therefore &\quad e(g,h) = e(h,g)^{-1}
\end{aligned}
$$

---
## Problem 4 (10 marks)
定义配对反演：设 $e: G_1 \times G_2 \rightarrow G_T$ 是一个双线性配对。给定 $g_t \in G_T$ 和 $h \in G_2$，找到 $g \in G_1$ 使得 $e(g, h) = g_t$。证明配对反演问题不比 $G_T$ 中的离散对数问题更难。

**证明**：即证明若 DL 问题不困难则配对反演问题也不困难。设 $G_1=\langle g_1\rangle$，则 $\exists k \in \mathbb{Z}_N, g = kg_1$，则 $e(g, h) = e(kg_1, h) = e(g_1, h)^k$，因此 $k=\mathrm{DLOG}_{e(g_1, h)}(g_t)$，其中 $e(g_1, h)$ 可以 PPT 计算，QED

---
## Problem 5 (20 marks)
记 $E[N]$ 为 $E$ 上的 $N$ 阶子群，$(P, Q)$ 是 $E[N]$ 的一组基，即 $E[N]$ 中的每个点都可以唯一表示为 $aP + bQ$ 的形式。设 $e: E[N] \times E[N] \rightarrow \langle \xi \rangle _N$ 是一个对称配对，证明 $e(aP + bQ, cP + dQ) = e(P, Q)^{ad - bc} \pmod{N}$。

**证明**：
$$
\begin{aligned}
e(aP + bQ, cP + dQ) &= e(aP, cP) \cdot e(aP, dQ) \cdot e(bQ, cP) \cdot e(bQ, dQ) \\
&= e(P, P)^{ac} \cdot e(P, Q)^{ad} \cdot e(Q, P)^{bc} \cdot e(Q, Q)^{bd} \\
&= 1 \cdot e(P, Q)^{ad} \cdot e(P, Q)^{-bc} \cdot 1 \\
&= e(P, Q)^{ad - bc} \pmod{N}
\end{aligned}
$$

---
## Problem 6 (20 marks)
设方程 $x^3 + ax + b \equiv 0 \pmod{p}$ 在 $\mathbb{Z}_p$ 中有三个不同的根，其中 $p$ 为奇素数，$a, b \in \mathbb{Z}_p$。证明对应的椭圆曲线群 $(\varepsilon, +)$ 不是循环群。

**证明**：

- 考虑椭圆曲线 $\varepsilon: y^2 = x^3 + ax + b$ 的二阶点 $P$，即满足 $2P = O \wedge P \neq O$ 的点，则 $P=-P$，因为 $y_{-P} = -y_P$，则 $y_P\equiv y_{-P} \pmod{p} \implies 2y_P \equiv 0 \pmod{p} \implies y_P \equiv 0 \pmod{p}$，因此 $P$ 若存在，则 $P=(x, 0)$
- 代入椭圆曲线方程可得 $x^3 + ax + b \equiv 0 \pmod{p}$，由于该方程在 $\mathbb{Z}_p$ 中有三个不同的根（记作 $r_1, r_2, r_3$），因此 $\varepsilon$ 上至少存在三个二阶点 $P_i=(r_i, 0), i=1,2,3$
- 考虑 $S=\{O, P_1, P_2, P_3\}$，由于 $P_i$ 是二阶点，因此 $P_i \neq O$，且 $P_i \neq P_j$（$i \neq j$），因此 $S$ 中有四个不同的元素。又由于 $P_i + P_j = P_k, i\neq j \neq k$，因此 $S$ 在加法下封闭，即 $S \leq \varepsilon$ 是 $\varepsilon$ 的一个子群且 $S \cong \mathbb{Z}_2 \times \mathbb{Z}_2$，因此 $\varepsilon$ 不是循环群。QED

---
## Problem 7 (20 marks)
给定 $\mathbb{Z}_p$ 上的椭圆曲线 $\varepsilon: y^2 = x^3 + ax + b$，其中 $p$ 是奇素数，$4a^3 + 27b^2 \not\equiv 0 \pmod{p}$。

### Subproblem 1
证明如果 $P=(x_1, y_1)\in \varepsilon$ 是一个三阶点，则 $3x_1^4 + 6ax_1^2 + 12bx_1 - a^2 \equiv 0 \pmod{p}$。

**证明**：由于 $P$ 是三阶点，因此 $2P = -P$，因此 $x_{2P} \equiv x_{-P} \equiv x_1 \pmod p$，则
$$
\begin{cases}
\lambda \equiv \frac{3x_1^2 + a}{2y_1} \pmod{p} \\
\lambda^2 - 2x_1 \equiv x_1 \pmod p \\ y_1^2 \equiv x_1^3 + a x_1 + b \pmod{p}
\end{cases}
$$

解得 $3x_1^4 + 6ax_1^2 + 12bx_1 - a^2 \equiv 0 \pmod{p}$，QED

### Subproblem 2
证明 $\varepsilon$ 上最多只有 8 个三阶点。

**证明**：由上问，若 $P=(x_1, y_1)$ 是 $\varepsilon$ 上的三阶点，则 $x_1$ 是方程 $3x^4 + 6ax^2 + 12bx - a^2 \equiv 0 \pmod{p}$ 的根。由于该方程的次数为 4，因此在 $\mathbb{Z}_p$ 中最多有 4 个不同的根。又由于 $y_1^2 \equiv x_1^3 + ax_1 + b \pmod{p}$，因此每个 $x_1$ 对应至多两个不同的 $y_1$，因此 $\varepsilon$ 上最多有 $4 \times 2 = 8$ 个三阶点。QED
