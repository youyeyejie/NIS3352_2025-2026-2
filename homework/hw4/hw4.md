# Assignment 4

## Problem 1 (25 marks)
设 $\{u, v\}$ 为 $\mathbb{R}^2$ 中一个格 $\mathcal{L}$ 的基底，其中 $u = (3, 7)$ 且 $v = (5, 10)$。

1. 求出 $u$ 的范数和 $v$ 的范数。
2. 确定 $\mathcal{L}$ 中最短的向量。

**解**：
1. $\|u\| = \sqrt{3^2 + 7^2} = \sqrt{58}$，$\|v\| = \sqrt{5^2 + 10^2} = \sqrt{125}$
2. 使用高斯基约化算法：
    - $m_0 = \lfloor \frac{u_0 \cdot v_0}{\|u_0\|^2} \rceil = \lfloor \frac{3\times 5 + 7\times 10}{58} \rceil = \lfloor \frac{85}{58} \rceil = 1$，则 $u_1 = v_0 - m_0 u_0 = (5, 10) - 1 \cdot (3, 7) = (2, 3)$，$v_1 = u_0 = (3, 7)$。
    - $m_1 = \lfloor \frac{u_1 \cdot v_1}{\|u_1\|^2} \rceil = \lfloor \frac{3\times 2 + 7\times 3}{2^2 + 3^2} \rceil = \lfloor \frac{27}{13} \rceil = 2$，则 $u_2 = v_1 - m_1 u_1 = (3, 7) - 2 \cdot (2, 3) = (-1, 1)$，$v_2 = u_1 = (2, 3)$。
    - $m_2 = \lfloor \frac{u_2 \cdot v_2}{\|u_2\|^2} \rceil = \lfloor \frac{-1\times 2 + 1\times 3}{(-1)^2 + 1^2} \rceil = \lfloor \frac{1}{2} \rceil = 0$，因此 $\lambda_1(\mathcal{L}) = \|u_2\| = \sqrt{(-1)^2 + 1^2} = \sqrt{2}$。

---
## Problem 2 (25 marks)
给定 $\mathbb{R}^3$ 中格 $\mathcal{L}$ 的基向量：$\mathbf{v}_1 = (1, 1, 1), \mathbf{v}_2 = (1, 2, 3), \mathbf{v}_3 = (1, 0, 1)$，对其进行施密特正交化。

**解**：
$$
\begin{aligned}
&\mathbf{u}_1 = \mathbf{v}_1 = (1, 1, 1) \\
&\mathbf{u}_2 = \mathbf{v}_2 - \frac{\mathbf{v}_2 \cdot \mathbf{u}_1}{\|\mathbf{u}_1\|^2} \mathbf{u}_1 = (1, 2, 3) - \frac{6}{3} (1, 1, 1) = (-1, 0, 1) \\
&\mathbf{u}_3 = \mathbf{v}_3 - \frac{\mathbf{v}_3 \cdot \mathbf{u}_1}{\|\mathbf{u}_1\|^2} \mathbf{u}_1 - \frac{\mathbf{v}_3 \cdot \mathbf{u}_2}{\|\mathbf{u}_2\|^2} \mathbf{u}_2 = (1, 0, 1) - \frac{2}{3} (1, 1, 1) - \frac{0}{2} (-1, 0, 1) = \left(\frac{1}{3}, -\frac{2}{3}, \frac{1}{3}\right)
\end{aligned}
$$

---
## Problem 3 (25 marks)
证明 multi-secret 形式的 DLWE 问题是困难的，即证明对于任何多项式界定的 $t$，$(\mathbf{a}, b_1, \cdots, b_t) \in \mathbb{Z}_q^n \times \mathbb{Z}_q^t$ 形式的元组与均匀随机分布不可区分，其中 $b_j = \mathbf{a}^\top \cdot \mathbf{s}_j + e_j$， 所有的 $\mathbf{s}_j \leftarrow \mathbb{Z}_q^n$ 选定后固定不变，且 $\mathbf{a} \leftarrow \mathbb{Z}_q^n$ 与 $e_j \leftarrow \chi$ 是相互独立的。

**证明**：

- 定义混合挑战 $H_i, i \in \{0, 1, \cdots, t\}$：返回的元组格式为：$(\mathbf{a}, u_1, \cdots, u_i, b_{i+1}, \cdots, b_t)$，其中 $u_j \leftarrow \mathbb{Z}_q$ 是均匀随机的。
    - 则 $H_0 =$ DLWE 问题，$H_t =$ 均匀随机分布。
    - 只需证明对于每个 $i \in \{0, 1, \cdots, t-1\}$，$H_i$ 与 $H_{i+1}$ 不可区分。
- 假设存在一个多项式时间算法 $\mathcal{A}$ 能以不可忽略的优势区分 $H_i$ 与 $H_{i+1}$，则可以构造一个算法 $\mathcal{B}$ 来解决 DLWE 困难问题。
    - $\mathcal{B}$ 的输入：$(\mathbf{a}, z_{\beta})\in \mathbb{Z}_q^n \times \mathbb{Z}_q$，其中 $z_0 = \mathbf{a}^\top \cdot \mathbf{s}_i + e_i$ 是 DLWE 样本，$z_1 \leftarrow \mathbb{Z}_q$ 是均匀随机的。
    - $\mathcal{B}$ 的策略：
        - 当 $j < i$ 时，均匀选取 $b_j \leftarrow \mathbb{Z}_q$
        - 当 $j = i$ 时，设置 $b_j = z_{\beta}$
        - 当 $j > i$ 时，设置 $b_j = \mathbf{a}^\top \cdot \mathbf{s}_j + e_j$，其中 $e_j \leftarrow \chi$ 是独立的。
    - $\mathcal{B}$ 将 $(\mathbf{a}, b_1, \cdots, b_t)$ 输入 $\mathcal{A}$，并输出 $\mathcal{A}$ 的结果。
        - 如果 $\beta = 0$，则 $\mathcal{B}$ 的输入是 DLWE 样本，$\mathcal{A}$ 的输入是 $H_i$ 的分布
        - 如果 $\beta = 1$，则 $\mathcal{B}$ 的输入是均匀随机的，$\mathcal{A}$ 的输入是 $H_{i+1}$ 的分布
    - 因此 $\mathcal{B}$ 能以 $\mathcal{A}$ 的优势区分 DLWE 样本与均匀随机分布，与 DLWE 的困难性假设矛盾。因此对于每个 $i \in \{0, 1, \cdots, t-1\}$，$H_i$ 与 $H_{i+1}$ 不可区分。
- 综上所述，$H_0$ 与 $H_t$ 不可区分，即 multi-secret 形式的 DLWE 问题是困难的。

---
## Problem 4 (25 marks)
定义 Dual-Regev 公钥加密方案（在加密时 $c_1, c_2$ 分别引入误差项 $e_1, e_2$），证明 DLWE 困难 $\implies$ Dual-Regev 是 IND-CPA 安全的。

**证明**：

- **定义混合游戏**：
    - **Game 0**：IND-CPA 安全模型
        1. $\mathrm{Gen}: \mathbf{A}\leftarrow\mathbb{Z}_{q}^{n\times m}$，$\mathbf{s}\leftarrow\mathbb{Z}_{q}^{m}$，$\mathbf{h}:=\mathbf{A}\mathbf{s}$，输出 $PK=(\mathbf{A},\mathbf{h})$
        2. $\mathrm{Enc}(PK,M_b): \mathbf{r}\leftarrow\mathbb{Z}_{q}^{n}$，$\mathbf{c}_1:=\mathbf{A}^\top\mathbf{r}+\mathbf{e}_1$，$c_2:=\mathbf{r}^\top\mathbf{h}+M_b\cdot\lfloor q/2\rceil+e_2$，其中 $\mathbf{e}_1\leftarrow\chi^{m}$，$e_2\leftarrow\chi$，输出 $C^*=(\mathbf{c}_1,c_2)$
    - **Game 1**：$PK$ 中的 $h\leftarrow\mathbb{Z}_{q}^{n}$
        1. $\mathrm{Gen}$：均匀选取 $\mathbf{A}\leftarrow\mathbb{Z}_{q}^{n\times m}$，$\mathbf{h}\leftarrow\mathbb{Z}_{q}^{n}$，输出 $PK=(\mathbf{A},\mathbf{h})\leftarrow\mathbb{Z}_{q}^{n\times m}\times\mathbb{Z}_{q}^{n}$
        2. 其余步骤与 Game 0 一致
    - **Game 2**：$C^*\leftarrow\mathbb{Z}_{q}^{n}\times\mathbb{Z}_{q}$
        1. $\mathrm{Enc}(PK,M_b)$：均匀选取 $\mathbf{c}_1\leftarrow\mathbb{Z}_{q}^{m}$，$c_2\leftarrow\mathbb{Z}_{q}$，$C^*=(\mathbf{c}_1,c_2)\leftarrow\mathbb{Z}_{q}^{m}\times\mathbb{Z}_{q}$
        2. 其余步骤与 Game 1 一致
- **混合游戏性质**：
    1. Game 0 与 Game 1 的区别只在于 $PK$ 中的 $\mathbf{h}$ 的生成：
        - Game 0 中 $\mathbf{h}=\mathbf{A}\mathbf{s}$
        - Game 1 中 $\mathbf{h}\leftarrow\mathbb{Z}_{q}^{n}$
    2. 由剩余哈希引理：$(\mathbf{A},\mathbf{h}=\mathbf{A}\mathbf{s})$ 与 $(\mathbf{A},\mathbf{u}\leftarrow\mathbb{Z}_{q}^{n})$ 不可区分，即 Game 0 与 Game 1 不可区分，即
        $$
        |\Pr(\mathrm{output}=b\mid \text{Game 0}) - \Pr(\mathrm{output}=b\mid \text{Game 1})| = \mathrm{negl}(\lambda)
        $$
    3. Game 1 与 Game 2 的区别只在于密文 $C^{*}=(\mathbf{c}_{1},c_{2})$ 的生成：
        - Game 1 中 $(\mathbf{c}_{1},c_{2})=(\mathbf{A}^\top\mathbf{r}+\mathbf{e}_1,\mathbf{r}^{\top}\mathbf{h}+M_b\cdot\lfloor q/2\rceil+e_2)=(\mathbf{A}^\top\mathbf{r}+\mathbf{e}_1,\mathbf{h}^{\top}\mathbf{r}+M_b\cdot\lfloor q/2\rceil+e_2)$
        - Game 2 中 $(\mathbf{c}_1,c_2)\leftarrow\mathbb{Z}_{q}^{m}\times\mathbb{Z}_{q}$
    4. 由判定性 LWE 问题困难，$(\mathbf{A}', \mathbf{A}'^\top \mathbf{r}+\mathbf{e}')$ 与 $(\mathbf{A}', \mathbf{u}'\leftarrow\mathbb{Z}_{q}^{m+1})$ 不可区分，其中 $\mathbf{A}'=(\mathbf{A} \mid \mathbf{h})$，$\mathbf{e}'=\begin{pmatrix} \mathbf{e}_1 \\ e_2 \end{pmatrix}$，$\mathbf{u}'=\begin{pmatrix} \mathbf{u}_1 \\ u_2 \end{pmatrix}$  。因此 Game 1 与 Game 2 不可区分，即
        $$
        |\Pr(\mathrm{output}=b\mid \text{Game 1}) - \Pr(\mathrm{output}=b\mid \text{Game 2})| = \mathrm{negl}(\lambda)
        $$
    5. Game 2 中，$C^{*}=(\mathbf{c}_{1},c_{2})$ 中已经不含 $b$ 的信息，因此
        $$
        \Pr(\mathrm{output}=b\mid \text{Game 2})=\frac{1}{2}
        $$
- **由三角不等式**：
    $$
    \begin{aligned}
    \mathrm{Adv}_{A} =& \left| \Pr(\mathrm{output}=b\mid \text{Game 0}) - \frac{1}{2} \right| \\
    \leq& \left| \Pr(\mathrm{output}=b\mid \text{Game 0}) - \Pr(\mathrm{output}=b\mid \text{Game 1}) \right|+ \\
    & \left| \Pr(\mathrm{output}=b\mid \text{Game 1}) - \Pr(\mathrm{output}=b\mid \text{Game 2}) \right|+ \\
    & \left| \Pr(\mathrm{output}=b\mid \text{Game 2}) - \frac{1}{2} \right| \\
    =& \mathrm{negl}(\lambda) + \mathrm{negl}(\lambda) + 0 \\
    =& \mathrm{negl}(\lambda)
    \end{aligned}
    $$
- 因此 $\mathrm{Adv}_{A}=\mathrm{negl}(\lambda)$，得证 IND-CPA 安全性。
