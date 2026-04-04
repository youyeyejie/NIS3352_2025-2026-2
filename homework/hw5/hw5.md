# Assignment 5

## Problem 1 (25 marks)
定义 $A\in \mathbb{Z}_q^{n\times m}, H_A(x) := Ax \pmod q, x \in \{0, 1\}^m$。证明如果能找到 $H_A$ 的碰撞，就能构造出 SIS 问题的解。

**证明**：假设我们找到了 $H_A$ 的一个碰撞 $x_1\neq x_2 \in \{0, 1\}^m$，使得 $H_A(x_1) = H_A(x_2)$，则有
$$
\begin{aligned}
& Ax_1 \equiv Ax_2 \pmod q
\implies A(x_1 - x_2) \equiv 0 \pmod q
\end{aligned}
$$

则 $z = x_1 - x_2 \neq 0$ 且 $z_i \in \{-1, 0, 1\}$，则 $z$ 正是一个 SIS 问题的有效解。

---

## Problem 2 (25 marks)
证明 ISIS 问题困难 + H 为 RO $\implies$ ISIS-based 签名算法 EUF-CMA 安全。

**证明**：假设存在一个多项式时间敌手 $\mathcal{A}$ 能以不可忽略的概率攻破 EUF-CMA 安全，构造一个敌手 $\mathcal{B}$，利用 $\mathcal{A}$ 来求解 ISIS 问题。

- **$\mathcal{B}$ 的输入**：$\mathbf{A}\in\mathbb{Z}_{q}^{n\times m}$
- **$\mathcal{B}$ 的策略**：
    - $\mathcal{B}$ 将公钥 $PK=\mathbf{A}$ 作为输入提供给 $\mathcal{A}$，并维护一个哈希查询表 $\mathcal{T}$ 记录 $\mathrm{H}(M)$ 与 $\sigma$ 的对应关系。假设 $\mathcal{A}$ 最多发起 $Q(\lambda)$ 次哈希查询，则在挑战开始前，$\mathcal{B}$ 猜测 $\mathcal{A}$ 最终会伪造的消息为 $M^* = M_k, k \in [1, Q(\lambda)]$。
    - 当 $\mathcal{A}$ 使用 $M_j$ 进行**哈希查询**时：
        - 若 $j = k$，$\mathcal{B}$ 将目标向量 $\mathbf{u}$ 返回给 $\mathcal{A}$ 作为 $\mathrm{H}(M_j)$ 的响应，并记录 $\mathbf{u} = \mathrm{H}(M_j)$。
        - 否则，$\mathcal{B}$ 首先检查 $\mathcal{T}$ 中是否存在 $M_j$ 的记录
            - 若存在，$\mathcal{B}$ 将记录的 $\mathrm{H}(M_j)$ 返回给 $\mathcal{A}$
            - 否则，$\mathcal{B}$ 随机生成一个签名 $\sigma_j$，计算 $\mathrm{H}(M_j) = \mathbf{A}\sigma_j$ 并将 $\mathrm{H}(M_j)$ 返回给 $\mathcal{A}$，同时记录 $\mathrm{H}(M_j) = \mathbf{A}\sigma_j$ 到 $\mathcal{T}$ 中
    - 当 $\mathcal{A}$ 使用 $M_i$ 进行**签名查询**时：
        - 若 $i = k$，$\mathcal{B}$ 无法提供合法签名，中止运行（失败）
        - 否则，$\mathcal{B}$ 首先检查 $\mathcal{T}$ 中是否存在 $M_i$ 的记录
            - 若存在，$\mathcal{B}$ 将记录的 $\sigma_i$ 返回给 $\mathcal{A}$
            - 否则，$\mathcal{B}$ 随机生成一个签名 $\sigma_i$ 返回给 $\mathcal{A}$，同时记录 $\mathrm{H}(M_i) = \mathbf{A}\sigma_i$ 到 $\mathcal{T}$ 中
    - $\mathcal{A}$ 最终以不可忽略优势输出一对有效的消息签名对 $(M^*,\sigma^*)$，则有 $\mathbf{A}\sigma^* \equiv \mathrm{H}(M^*) \pmod q$ 且 $\|\sigma^*\| \le \beta$。
        - 若 $M^* \neq M_k$，则 $\mathcal{B}$ 失败
        - 若 $M^* = M_k$，则有 $\mathbf{A}\sigma^* \equiv \mathrm{H}(M^*) = \mathbf{u} \pmod q$，$\sigma^*$ 是 ISIS 实例 $(\mathbf{A}, \mathbf{u})$ 的一个有效解。
- **$\mathcal{B}$ 的输出**：$\sigma^*$
- **$\mathcal{B}$ 的优势**：
    $$
    \begin{aligned}
    \mathrm{Adv}_{\mathcal{B}} &\geq \Pr\left[
    \begin{array}{l}
    (1)\ \mathcal{A} \text{ 成功攻破 GPV 签名算法的 EUF-CMA 安全性} \\
    (2)\ \mathcal{A} \text{ 查询过 } M^{*} \text{ 的 Hash 值} \\
    (3)\ \mathcal{B} \text{ 猜测的 } M_k \text{ 与 } M^* \text{ 相同}
    \end{array}
    \right] \\
    &= \text{non-negl}(\lambda) \cdot \text{non-negl}(\lambda) \cdot \frac{1}{Q(\lambda)} \\
    &= \text{non-negl}(\lambda)
    \end{aligned}
    $$
- 因此，$\mathcal{B}$ 以不可忽略的概率成功求解 ISIS 问题，与 ISIS 问题的困难性假设矛盾。得证。

---

## Problem 3 (25 marks)
### Question 1

$f(x) = x^2 + 1 \in F_3[x]$

1. 证明 $f$ 不可约。
    - **证明**：$f$ 在 $F_3$ 上的根为 $0, 1, 2$，则 
        $$
        \begin{cases}
        f(0) = 1 \neq 0 \\
        f(1) = 2 \neq 0 \\
        f(2) = 5 \equiv 2 \neq 0
        \end{cases} \pmod 3
        $$ 因此 $f$ 没有线性因子，也就不可约。
2. $F_9 \cong F_3[x]/(x^2 + 1)$，令 $\alpha = [x]$，以 $a+b\alpha$ 的形式表示 $F_9$ 中的元素，其中 $a,b \in F_3$。
    - **解**：
        $$
        \begin{aligned}
        F_9 &\cong F_3[x]/(x^2 + 1) \\
        &= \{[g(x)]: g(x) \in F_{3}[x], \deg(g)< 2 \} \\
        &= \{a + b\alpha : a,b \in F_3\} \\
        &= \{0, 1, 2, \alpha, 1+\alpha, 2+\alpha, 2\alpha, 1+2\alpha, 2+2\alpha\}
        \end{aligned}
        $$
3. 计算 $F_9$ 中每个非零元素的乘法逆元。
    - **解**：
        - $1^{-1} = 1$
        - $2^{-1} = 2$
        - 因为 $\alpha \cdot 2\alpha = 2\alpha^2 \equiv 2x^2 \equiv -2 \equiv 1 \pmod{x^2+1}$，所以 $\alpha^{-1} = 2\alpha$，同理 $(2\alpha)^{-1} = \alpha$。
        - 剩余四个元素 $1+\alpha, 2+\alpha, 1+2\alpha, 2+2\alpha$ 中存在两对互为逆元的元素：
            - $(1+\alpha)(2+\alpha) = 2 + 3\alpha + \alpha^2 \equiv 2 + 0\alpha - 1 \equiv 1$，所以 $(1+\alpha)^{-1} = 2+\alpha$，同理 $(2+\alpha)^{-1} = 1+\alpha$。
            - $(1+2\alpha)(2+2\alpha) = 2 + 6\alpha + 4\alpha^2 \equiv 2 + 0\alpha - 4 \equiv 1$，所以 $(1+2\alpha)^{-1} = 2+2\alpha$，同理 $(2+2\alpha)^{-1} = 1+2\alpha$。
4. 寻找 $F_9^*$ 的生成元。
    - **解**：$\mathrm{ord}(F_9^*) = 8$，因此生成元的阶必须为 8，共 $\varphi(8) = 4$ 个生成元
        - $1$ 显然非生成元
        - $2^2 = 4, 2^3 = 8, 2^4 = 16 \equiv 7, 2^5 = 14 \equiv 5, 2^6 = 10 \equiv 1$，$\mathrm{ord}(2) = 6$，非生成元
        - $\alpha^2 = 2, \alpha^3 = 2\alpha, \alpha^4 = 2\alpha^2 \equiv 1， \mathrm{ord}(\alpha) = 4$，$\alpha$ 非生成元
        - $2\alpha = \alpha^{-1}, \mathrm{ord}(2\alpha) = \mathrm{ord}(\alpha) = 4$，非生成元
        - 因此剩余四个元素 $1+\alpha, 2+\alpha, 1+2\alpha, 2+2\alpha$ 都是生成元。

### Question 2
令 $F_8 \cong F_2[x]/(x^3+x+1)$，$\alpha = [x]$

1. 计算 $\alpha$ 的 $3,4,5,6,7$ 次幂：
    - **解**：
        - $\alpha^2 = [x^2]$
        - $x^3 \equiv x + 1 \pmod{x^3+x+1}$，因此 $\alpha^3 = \alpha + 1$。
        - $\alpha^4 = \alpha \cdot \alpha^3 = \alpha(\alpha + 1) = \alpha^2 + \alpha$
        - $\alpha^5 = \alpha^2 \cdot \alpha^3 = \alpha^2(\alpha + 1) = \alpha^3 + \alpha^2 = \alpha^2 + \alpha + 1$
        - $\alpha^6 = (\alpha^3)^2 = (\alpha + 1)^2 = \alpha^2 + 1$
        - $\alpha^7 = \alpha^6 \cdot \alpha = (\alpha^2 + 1)\alpha = \alpha^3 + \alpha = 1$
2. 求 $\mathrm{ord}(\alpha)$。
     - **解**：$\alpha^7 = 1$，且 $1 \le k < 7$ 时 $\alpha^k \neq 1$，因此 $\mathrm{ord}(\alpha) = 7$。

### Question 3

证明有限域乘法群 $F_q^*$ 是循环群。

**证明**：

- $F_q^*$ 是一个有限阿贝尔群，包含 $q-1$ 个元素。令 $m$ 为该群中元素的最大乘法阶，则根据有限阿贝尔群的性质，群中所有元素的阶都整除 $m$。
- 对于任意 $x \in F_q^*$，都满足方程 $x^m - 1 = 0$，因为在域上一个 $m$ 次多项式最多有 $m$ 个根，所以必然有 $q-1 \le m$。
- 同时，元素的阶不能超过群的阶，即 $m \le q-1$。
- 因此 $m = q-1$。这意味着群中存在一个阶为 $q-1$ 的元素，它能生成整个群，故 $F_q^*$ 是循环群。

### Question 4

证明有限域 $F_{p^n}$ 包含大小为 $p^d$ 的子域 $\iff d \mid n$。

**证明**：

- $(\implies)$：假设 $F_{p^n}$ 包含 $F_{p^d}$，则可将 $F_{p^n}$ 视为 $F_{p^d}$ 上的向量空间。设其维度为 $k$，则 $|F_{p^n}| = (p^d)^k = p^{dk} = p^n$。因此 $n=dk$，即 $d \mid n$。
- $(\impliedby)$：$F_{p^n}$ 中的元素是 $A(x) = x^{p^n} - x$ 的根，而 $F_{p^d}$ 中的元素是 $B(x) = x^{p^d} - x$ 的根。因为 $d \mid n$，所以 $p^n = p^{kd}$，所以 $p^d - 1 \mid p^n - 1$，所以 $x^{p^d-1} - 1 \mid x^{p^n-1} - 1$，因此 $B(x) \mid A(x)$，则 $B(x)$ 的根也是 $A(x)$ 的根，因此 $F_{p^d} \subseteq F_{p^n}$。


### Question 5

列举 $F_{2^{16}}$ 所有可能子域的大小。

**解**：由上一题可知，$d \mid 16$ 的因子有 $1, 2, 4, 8, 16$。因此可能的子域大小为
$$
\begin{cases}
2^1 = 2, \\
2^2 = 4, \\
2^4 = 16, \\
2^8 = 256, \\
2^{16} = 65536
\end{cases}
$$

---

## Problem 4 (25 marks)
### Question 1

二元线性码 $C$，生成矩阵 $G = \begin{pmatrix} 1 & 0 & 1 & 0 & 1 \\ 0 & 1 & 1 & 1 & 0 \end{pmatrix}$

1. 列出所有码字
    - **解**：
        - $m_1=(0,0) \to c_1 = m_1^\top G = 00000$
        - $m_2=(0,1) \to c_2 = m_2^\top G = 01110$
        - $m_3=(1,0) \to c_3 = m_3^\top G = 10101$
        - $m_4=(1,1) \to c_4 = m_4^\top G = 11011$
2. 参数 $[n, k, d]$
    - **解**：
        - $n=5$
        - $k=2$
        - $d=\min\{\mathrm{wt}(c_2), \mathrm{wt}(c_3), \mathrm{wt}(c_4)\} = \min\{4, 3, 4\} = 3$
3. 求校验矩阵 $H$
    - **解**：
        - $G = \begin{pmatrix} I_2 & P \end{pmatrix}$，其中 $P = \begin{pmatrix} 1&0&1 \\ 1&1&0 \end{pmatrix}$。
        - 则 $H = \begin{pmatrix} -P^T & I_{n-k} \end{pmatrix}$，在二元域中 $-P^T = P^T = \begin{pmatrix} 1&1 \\ 0&1 \\ 1&0 \end{pmatrix}$。故 $H = \begin{pmatrix} 1&1&1&0&0 \\ 0&1&0&1&0 \\ 1&0&0&0&1 \end{pmatrix}$。
4. $r = (1, 1, 0, 1, 1)$，计算伴随式 $Hr^\top$
    - **解**：
        $$
        Hr^\top = \begin{pmatrix} 1&1&1&0&0 \\ 0&1&0&1&0 \\ 1&0&0&0&1 \end{pmatrix} \begin{pmatrix} 1 \\ 1 \\ 0 \\ 1 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 0 \end{pmatrix}
        $$

### Question 2
线性码 $C\subseteq F_3^4$，生成矩阵 $G = \begin{pmatrix} 1 & 0 & 1 & 2 \\ 0 & 1 & 1 & 1 \end{pmatrix}$

1. 计算码字数量 $|C|$
    - **解**：域大小为 $3$，秩 $k=2$， $|C| = 3^2 = 9$。
2. 列出所有码字
    - **解**：
        - $m_1=(0,0) \to c_1 = 0000$
        - $m_2=(0,1) \to c_2 = 0111$
        - $m_3=(0,2) \to c_3 = 0222$
        - $m_4=(1,0) \to c_4 = 1012$
        - $m_5=(1,1) \to c_5 = 1120$
        - $m_6=(1,2) \to c_6 = 1201$
        - $m_7=(2,0) \to c_7 = 2021$
        - $m_8=(2,1) \to c_8 = 2102$
        - $m_9=(2,2) \to c_9 = 2210$
3. 计算最小距离 $d$
    - **解**：计算所有非零码字的重量，发现它们全都为 3，故 $d=3$。
4. 判断 $C$ 是否能纠正 1 个错误。
    - **解**：纠错数目 $t = \lfloor \frac{d-1}{2} \rfloor = \lfloor \frac{3-1}{2} \rfloor = 1$，可以纠正 1 个错误。

### Question 3
$F_q$ 上的线性码 $C$，参数 $[n, k, d]$，证明 Singleton 界 $d \le n - k + 1$。

**证明**：将所有码字的最后 $d-1$ 个坐标删除得到长度为 $n - (d - 1)$ 的向量。因为原码的最小距离为 $d$，这意味着任意两个不同的码字在原来的 $n$ 位中至少有 $d$ 个位置不同；删除 $d-1$ 个位置后，它们至少还有 1 个位置不同。

因此，删余操作后，码字之间仍然是互不相同的，总数依然保持 $q^k$ 个不变。又因为长度为 $n-d+1$ 的向量空间总容量为 $q^{n-d+1}$。所以码字数量必须小于等于空间容量：$q^k \le q^{n-d+1}$，即 $d \le n - k + 1$。

### Question 4

二元线性码，生成矩阵 $G = \begin{pmatrix} 1 & 1 & 0 & 1 & 0 \\ 1 & 0 & 1 & 1 & 1 \\ 0 & 1 & 1 & 0 & 1 \end{pmatrix}$

1. 使用基本行操作将 $G$ 转换为系统矩阵 $G' = \begin{pmatrix} I_k & P \end{pmatrix}$
    - **解**：
        - $R_2 \leftarrow R_2 + R_1$：$\begin{pmatrix} 1 & 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 0 & 1 \\ 0 & 1 & 1 & 0 & 1 \end{pmatrix}$
        - $R_3 \leftarrow R_3 + R_2$：$\begin{pmatrix} 1 & 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 0 & 1 \\ 0 & 0 & 0 & 0 & 0 \end{pmatrix}$
        - $R_1 \leftarrow R_1 + R_2$：$\begin{pmatrix} 1 & 0 & 1 & 1 & 1 \\ 0 & 1 & 1 & 0 & 1 \\ 0 & 0 & 0 & 0 & 0 \end{pmatrix}$
        - 因此 $G' = \begin{pmatrix} 1 & 0 & 1 & 1 & 1 \\ 0 & 1 & 1 & 0 & 1 \end{pmatrix} = \begin{pmatrix} I_2 & P \end{pmatrix}$，其中 $P = \begin{pmatrix} 1&1&1 \\ 1&0&1 \end{pmatrix}$
2. 写出校验矩阵 $H = (-P^\top \mid I_{n-k})$
    - **解**：二元域上有 $-P^T=P^T$，因此 $-P^\top = P^\top = \begin{pmatrix} 1&1 \\ 1&0 \\ 1&1 \end{pmatrix}$，故 $H = \begin{pmatrix} 1&1&1&0&0 \\ 1&0&0&1&0 \\ 1&1&0&0&1 \end{pmatrix}$
3. 验证 $G'H^\top = 0$
    - **解**：
        $$
        \begin{aligned}
        G'H^\top &= \begin{pmatrix} I_2 & P_{2 \times 3} \end{pmatrix} \begin{pmatrix} P_{2 \times 3} \\ I_3 \end{pmatrix} \\
        &= I_2 P_{2 \times 3} + P_{2 \times 3} I_3 \\
        &= 2 P_{2 \times 3} \\
        &= 0
        \end{aligned}
        $$