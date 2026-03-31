# 后量子密码
## 格理论以及基于格困难问题的密码学

### 格理论简介
#### 格的定义
- **格**（Lattice）：格 $\mathcal{L}$ 是 $\mathbb{R}^m$ 空间中离散的具有加法运算的子群。
    - $\mathbb{R}^m$ 空间：由 $m$ 维实数向量组成的空间。
        - 向量的范式：$\| \mathbf{x}\| =\sqrt{\sum_{i=1}^{m} x_i^2}$
        - 向量的距离：$\mathrm{dist}(\mathbf{x},\mathbf{y})=\| \mathbf{x}-\mathbf{y}\|$
    - 离散（Discrete）：每一个格点 $\mathbf{x} \in \mathcal{L}$，存在 $\mathbb{R}^{m}$ 中的一个领域仅包含 $\mathbf{x}$ 唯一格点;
    - 加法：
        - $(0,\cdots, 0) \in \mathcal{L}$
        - $\forall \mathbf{x},\mathbf{y} \in \mathcal{L}, \mathbf{x}-\mathbf{y} \in \mathcal{L}$
- 示例：$\mathbb{Z}^{m}$、$(q\mathbb{Z})^{m}$ 是格，但 $\mathbb{Q}^{m}$，$2\mathbb{Z}+ 1$ 以及 $\mathbb{Z}+\mathbb{Z}\sqrt{2}$ 都不是格。
- **格基**：设 $\mathcal{L}$ 是 $\mathbb{R}^{m}$ 中的格，则存在 $\mathbb{R}$-线性无关的向量 $\mathbf{b}_{1}, \mathbf{b}_{2}, \cdots, \mathbf{b}_{n} \in \mathbb{R}^{m}$，使得
    $$
    \mathcal{L}=\left\{z_{1} \mathbf{b}_{1}+z_{2} \mathbf{b}_{2}+\cdots+z_{n} \mathbf{b}_{n} \mid z_{i} \in \mathbb{Z}\right\}
    $$

    - **维数**：$m$
    - **秩**：$n$
    - **满秩格**：当 $m=n$ 时称 $\mathcal{L}$ 是满秩格
    - **格基**：$B=(\mathbf{b}_{1}, \mathbf{b}_{2}, \cdots, \mathbf{b}_{m})$
    - **格点**：向量 $\mathbf{v}=z_{1} \mathbf{b}_{1}+z_{2} \mathbf{b}_{2}+\cdots+z_{n} \mathbf{b}_{n} \in \mathcal{L}$
- **性质**：两组基 $B=\{\mathbf{b}_{1}, \cdots, \mathbf{b}_{n}\}$ 与 $B'=\{\mathbf{b}_{1}', \cdots, \mathbf{b}_{n}'\}$ 生成同一个格当且仅当存在一个幺模矩阵 $U \in \mathbb{Z}^{n\times n}$ 使得 $B=B' U$。
    - 幺模矩阵：$U$ 是一个整数矩阵，且 $\det U=\pm 1$。
- **基本平行多面体**（Fundamental parallelepipeds）：设 $\mathcal{L}$ 为满秩格，则格 $\mathcal{L}$ 的基本平行多面体定义为
    $$
    \mathcal{F}(B):= \mathbb{R}^{m}/\mathcal{L} = \{ \sum _{i=1}^{m}x_{i}\mathbf{b}_{i} \mid x_{i}\in [0,1)\}
    $$
    - $\mathcal{F}(B)$ 的体积为
        $$
        \mathrm{vol}(\mathcal {F}(B))=|\det M(B)|,\quad M(B)=\begin{pmatrix}\mathbf{b}_{1} & \mathbf{b}_{2} & \cdots & \mathbf{b}_{m}\end{pmatrix}
        $$
- **定理**：
    1. 设 $\mathbf{b}_{1}, \mathbf{b}_{2}, \cdots, \mathbf{b}_{m} \in \mathcal{L}$ 且整线性无关，则 $B=\{\mathbf{b}_{1}, \mathbf{b}_{2}, \cdots, \mathbf{b}_{m}\}$ 为格基当且仅当
        $$
        \mathcal{F}(B) \cap \mathcal{L}=\{(0, \cdots, 0)\}
        $$
    2. 设 $B=\{\mathbf{b}_{1}, \mathbf{b}_{2}, \cdots, \mathbf{b}_{m}\}$ 和 $B'=\{\mathbf{b}_{1}', \mathbf{b}_{2}', \cdots, \mathbf{b}_{m}'\}$ 为两组格基，则
        $$
        \mathrm{vol}(\mathcal{F}(B))=\mathrm{vol}\left(\mathcal{F}\left(B'\right)\right)
        $$
    3. 设 $B=\{\mathbf{b}_{1}, \mathbf{b}_{2}, \cdots, \mathbf{b}_{m}\}$ 为格基，定义 $\mathcal{L}$ 的行列式为
        $$
        \det(\mathcal{L}):=\mathrm{Vol}(\mathcal{F}(B))=|\det(B)|
        $$

#### 格上定义的问题
- **格的最小距离**：设 $\mathcal{L}$ 是 $\mathbb{R}^{m}$ 中的格，则 $\mathcal{L}$ 的最小距离定义为
    $$
    \lambda_{1}(\mathcal{L})=\min\{\| \mathbf{v}\| : \mathbf{v} \in \mathcal{L} \setminus\{0\}\}=\min\{\| \mathbf{x}-\mathbf{y}\| : \mathbf{x} \neq \mathbf{y} \in \mathcal{L}\}
    $$
- **Minkowski’s first theorem**：设 $\mathcal{L}$ 为秩是 $m$ 的格，则
    $$
    \lambda_{1}(\mathcal{L}) \leq \sqrt{m}(\det \mathcal{L})^{1/m}
    $$
    - 示例：$\mathbf{b}_{1}=(0,2^{-100})$，$\mathbf{b}_{2}=(2^{100}, 0)$ 那么 $\lambda_{1}(\mathcal{L}(\mathbf{b}_{1}, \mathbf{b}_{2}))=2^{-100} \ll \sqrt{2}$。

- **最短向量问题** $(SVP)$：给定格 $\mathcal{L}$ 的任意格基 $B$，找到 $\mathbf{v} \in \mathcal{L}\setminus\{0\}$ 使得
    $$
    \| \mathbf{v}\| =\lambda_{1}(\mathcal{L})
    $$
- **最近向量问题** $(CVP)$：给定格 $\mathcal{L}$ 的任意格基 $B$，以及 $t \in \mathbb{R}^{m}$，找到 $\mathbf{v} \in \mathcal{L}$ 使得
    $$
    \forall \mathbf{y} \in \mathcal{L}, \| \mathbf{v}-\mathbf{t}\| \leq \| \mathbf{y}-\mathbf{t}\|
    $$
- **近似最短向量问题** $(SVP_{\gamma})$：给定格 $\mathcal{L}$ 的任意格基 $B$，找到 $\mathbf{v} \in \mathcal{L}\setminus\{0\}$ 使得
    $$
    \| \mathbf{v}\| ≤\gamma(m) \cdot \lambda_{1}(\mathcal{L})
    $$
- **近似最近向量问题** $(CVP_{\gamma})$：给定格 $\mathcal{L}$ 的任意格基 $B$，以及 $t \in \mathbb{R}^{m}$，找到 $\mathbf{v} \in \mathcal{L}$ 使得
    $$
    \forall \mathbf{y} \in \mathcal{L}, \| \mathbf{v}-\mathbf{t}\| \leq \gamma(m) \cdot\| \mathbf{y}-\mathbf{t}\|
    $$
- **判定性近似最短向量问题** $(GapSVP_{\gamma})$：假设 $\mathcal{L}$ 是满足 $\lambda_{1}(\mathcal{L})\leq1$ 或 $\lambda_{1}(\mathcal{L})>\gamma(n)$ 的格，给定格 $\mathcal{L}$ 的任意格基 $B$，判断 $\mathcal{L}$ 属于哪种情况。
- **近似最短独立向量问题** $(SIVP_{\gamma})$：给定格 $\mathcal{L}$ 的任意格基 $B$，找到 $n$ 个线性无关向量 $s_{i}\in \mathcal{L}$，使得
    $$
    \|s_{i}\|\leq\gamma(n)\cdot\lambda_{n}(\mathcal{L})
    $$ 其中 $\lambda_{n}(\mathcal{L})$ 表示 $\mathcal{L}$ 的第 $n$ 小距离。
- **有界距离解码问题** $(BDD_{\delta})$：给定格 $\mathcal{L}$ 的任意格基 $B$，以及 $\mathbf{t} \in \mathbb{R}^{m}$，满足 $\mathrm{dist}(\mathcal{L}, \mathbf{t}) \leq\delta<\frac{\lambda_{1}(\mathcal{L})}{2}$，找到唯一的格向量 $\mathbf{w} \in \mathcal{L}$，使得
    $$
    \| \mathbf{w}-\mathbf{t}\| _{2} \leq \delta
    $$
    - $BDD_{\delta} \subset CVP_{\delta}$
    - $BDD_{\delta}$ 的计算复杂度随着维度 $n$ 和参数 $\delta$ 的增大而增加。
- **定理**：$SVP_{\gamma(m)} \leq_{P} CVP_{\gamma(m)}$。

### LWE 问题（Learning with Errors）
- **参数**：LWE 参数 $(n,m,q,B_{\chi},\chi)$ 满足：
    1. $m \cdot B_{\chi} < q/4$
    2. $m \geq 2n \cdot \log q$
- **计算性 LWE 问题**（CLWE）：$n,m$ 为整数，$q$ 为素数，$\chi$ 为区间 $[-B_{\chi}, B_{\chi}]$ 上的概率分布。均匀选取 $\mathbf{A} \leftarrow \mathbb{Z}_{q}^{n\times m}$，$\mathbf{s} \leftarrow \mathbb{Z}_{q}^{n}$，根据 $\chi$ 分布选取 $\mathbf{e} \leftarrow[-B_{\chi}, B_{\chi}]^{m}$，计算 $\mathbf{z}:=\mathbf{A}^{\top} \mathbf{s}+\mathbf{e} \in \mathbb{Z}_{q}^{m}$。
    - **输入**：$(\mathbf{A}, \mathbf{z})$
    - **输出**：$\mathbf{s}$
    - **计算性 LWE 问题困难**：任意 PPT 敌手的优势是可忽略的，即
        $$
        \mathrm{Adv} = \left|\Pr(\mathrm{output}=\mathbf{s}) - \frac{1}{q^{n}}\right| = \mathrm{negl}(\lambda)
        $$
    - 直觉：定义格 $\mathcal{L}(\mathbf{A})=\{\mathbf{x} \in \mathbb{Z}^{m} \mid \mathbf{x}=\mathbf{A}^{T} \mathbf{s} \pmod q, \mathbf{s} \in \mathbb{Z}_{q}^{n}\}$，则 $\mathbf{z}$ 是 $\mathcal{L}(\mathbf{A})$ 中某个格点附近的一个点，$\mathbf{e}$ 是噪声。
        ![](image/image-18.png)
- **判定性 LWE 问题**（DLWE）：$n,m$ 为整数，$q$ 为素数，$\chi$ 为区间 $[-B_{\chi}, B_{\chi}]$ 上的概率分布。均匀选取 $\mathbf{A} \leftarrow \mathbb{Z}_{q}^{n\times m}$，$\mathbf{s} \leftarrow \mathbb{Z}_{q}^{n}$，根据 $\chi$ 分布选取 $\mathbf{e} \leftarrow[-B_{\chi}, B_{\chi}]^{m}$，计算 $\mathbf{z}_{0}:=\mathbf{A}^{\top} \mathbf{s}+\mathbf{e} \in \mathbb{Z}_{q}^{m}$，$\mathbf{z}_{1} \leftarrow \mathbb{Z}_{q}^{m}$，均匀选取 $\beta \leftarrow\{0,1\}$。
    - **输入**：$(\mathbf{A}, \mathbf{z}_{\beta})$
    - **输出**：$\beta$
    - **判定性 LWE 问题困难**：任意 PPT 敌手的优势是可忽略的，即
        $$
        \mathrm{Adv} = \left|\Pr(\mathrm{output}=\beta) - \frac{1}{2}\right| = \mathrm{negl}(\lambda)
        $$
    - ![](image/image-17.png)
- **定理（Regev05）**：**DLWE 问题困难** $\iff$ **CLWE 问题困难**

!!! fold info @Proof
    - **充分性证明** $\implies$：假设存在 PPT 敌手 $\mathcal{A}$ 能够以不可忽略的优势攻破 CLWE 问题，构造 PPT 敌手 $\mathcal{B}$ 来攻破判定性 LWE 问题：
        - $\mathcal{B}$ 的输入：$(\mathbf{A}, \mathbf{z}_{\beta})$
        - 调用子敌手：将 $(\mathbf{A}, \mathbf{z}_{\beta})$ 作为输入传递给 $\mathcal{A}$
            - 当 $\beta=0$ 时，$\mathbf{z}_{0}=\mathbf{A}^{\top} \mathbf{s}+\mathbf{e}$ 满足 LWE 问题形式，$\mathcal{A}$ 以不可忽略的优势输出 $\mathbf{s}$。
            - 当 $\beta=1$ 时，$\mathbf{z}_{1}\leftarrow \mathbb{Z}_{q}^{m}$ 是均匀分布的。考虑若 $\mathcal{A}$ 仍可以输出有效的 $\mathbf{s}$，则 $\exists \mathbf{s}' \in \mathbb{Z}_{q}^{n}$ 使得 $\mathbf{z}_{1} - \mathbf{A}^{\top} \mathbf{s}' \in [-B_{\chi}, B_{\chi}]^{m}$，计算这个概率：
                $$
                \begin{aligned}
                P &= \frac{\#\{\mathbf{z}_{1} \in \mathbb{Z}_{q}^{m} \mid \mathbf{z}_{1} - \mathbf{A}^{\top} \mathbf{s}' \in [-B_{\chi}, B_{\chi}]^{m}\}}{\mathbb{Z}_{q}^{m}} \\
                &\leq \frac{q^n\cdot (2B_{\chi}+1)^{m}}{q^{m}} < \frac{q^n\cdot (q/m)^{m}}{q^{m}} \\
                &= \frac{q^n}{m^m} \leq \frac{2^{n \cdot \log q}}{2^{m\cdot \log m}} \\
                &\leq \frac{2^{\frac{m}{2}}}{2^{m\cdot \log m}} = 2^{-\frac{m}{2}\cdot \log m} = \mathrm{negl}(\lambda)
                \end{aligned}
                $$ 因此 $\mathcal{A}$ 输出有效的 $\mathbf{s}$ 的概率是可忽略的。
        - $\mathcal{B}$ 的输出：假设 $\mathrm{output}_\mathcal{B} = \mathbf{s}'$
            $$
            \mathrm{output}_\mathcal{B} = \begin{cases}
            0 & \mathcal{A} \text{ 输出有效的 } \mathbf{s} \\
            1 & \text{otherwise}
            \end{cases}
            $$
        - $\mathcal{B}$ 的优势
            $$
            \begin{aligned}
            \mathrm{Adv}_\mathcal{B} &= \left|\Pr(\mathrm{output}_\mathcal{B}=\beta) - \frac{1}{2}\right| \\
            &= \frac{1}{2} \cdot \left|\Pr(\mathrm{output}_\mathcal{B}=0 \mid \beta=0) - \Pr(\mathrm{output}_\mathcal{B}=0 \mid \beta=1)\right| \\
            &= \frac{1}{2} \cdot \left|\text{non-negl}(\lambda)-\mathrm{negl}(\lambda) \right| \\
            &= \text{non-negl}(\lambda)
            \end{aligned}
            $$
        - 因此 $\mathcal{B}$ 以不可忽略的优势攻破判定性 LWE 问题，与假设矛盾。
    - **必要性证明** $\impliedby$：假设存在 PPT 敌手 $\mathcal{B}$ 能够以不可忽略的优势攻破 DLWE 问题，构造 PPT 敌手 $\mathcal{A}$ 来攻破计算性 LWE 问题。
        - $\mathcal{A}$ 的输入：$(\mathbf{A}, \mathbf{z})$，其中 $\mathbf{A} \leftarrow \mathbb{Z}_q^{n \times m}$，$\mathbf{z} =\mathbf{A}^\top \mathbf{s} + \mathbf{e} \in \mathbb{Z}_q^m$。
        - 考虑要求解的 $\mathbf{s} = (s_1, s_2, \ldots, s_n)^\top$，$\mathcal{A}$ 逐一求解每个分量 $s_i$：
            - 猜测分量 $s_i=k\in\{0,1,\ldots,q-1\}$，构造新的矩阵 $\mathbf{A}'$ 和向量 $\mathbf{z}'$：
                1. 均匀随机选取一个向量 $\mathbf{v} \leftarrow \mathbb{Z}_q^m$。
                2. 构造 $\mathbf{A}' = \mathbf{A} + \mathbf{e}_i \mathbf{v}^\top$，其中 $\mathbf{e}_i = \{0, \ldots, 0, 1, 0, \ldots, 0\}$ 是第 $i$ 个标准基向量。
                3. 构造 $\mathbf{z}' = \mathbf{z} + k \cdot \mathbf{v}$。
                4. 则有
                    $$
                    \begin{aligned}
                    \mathbf{z}' - \mathbf{A}'^\top \mathbf{s} &= (\mathbf{z} + k \cdot \mathbf{v}) - (\mathbf{A} + \mathbf{e}_i \mathbf{v}^\top)^\top \mathbf{s} \\
                    &= \mathbf{z} + k \cdot \mathbf{v} - \mathbf{A}^\top \mathbf{s} - s_i\cdot \mathbf{v}  \\
                    &= \mathbf{e} + (k - s_i) \cdot \mathbf{v}
                    \end{aligned}
                    $$
            - 将 $(\mathbf{A}', \mathbf{z}')$ 作为输入传递给 $\mathcal{B}$：
                - 若猜测正确，即 $k = s_i$，则 $\mathbf{z}' - \mathbf{A}'^\top \mathbf{s} = \mathbf{e}$ 满足 LWE 问题形式，$\mathcal{B}$ 以不可忽略的优势输出 $0$。
                - 若猜测错误，则 $\mathbf{z}'' = \mathbf{z}' - (k - s_i) \cdot \mathbf{v}$ 是 $\mathbb{Z}_q^m$ 上均匀分布的，$\mathcal{B}$ 以不可忽略的优势输出 $1$。
        - 因此，只要遍历 $s_i=k\in\{0,1,\ldots,q-1\}$，就可以以不可忽略的优势正确求解 $s_i$；再对所有 $i \in \{1,2,\ldots,n\}$ 重复上述过程，即可以不可忽略的优势求解 $\mathbf{s}$。
        - $\mathcal{A}$ 的复杂度：上述过程需要 $n \cdot q$ 次调用 $\mathcal{B}$，由 $q=\mathrm{poly}(n)$ 以及 $\mathcal{B}$ 的 PPT 性质可知 $\mathcal{A}$ 也是 PPT 算法。因此 $\mathcal{A}$ 以不可忽略的优势攻破计算性 LWE 问题，与假设矛盾。
- **LWE 问题的困难性**：LWE 问题可以归约到格中的困难问题，因此 LWE 被公认为是抗量子的。
    - 对于任意 $m=\mathrm{poly}(n)$，任意模数 $q \leq 2^{\mathrm{poly}(n)}$，以及任何（离散化的）参数为 $\alpha q \geq 2\sqrt{n}$ 的高斯误差分布 $\chi$，解决判定性 LWE 问题至少和量子地解决任意 $n$ 维格上的 $GapSVP_{\gamma}$ 和 $SIVP_{\gamma}$ 一样困难，其中 $\gamma = \tilde{O}(n/\alpha)$。

### Regev 公钥加密算法
#### Regev 公钥加密算法（加密 1 比特）
- **密钥生成算法** $(PK,SK)\leftarrow \mathrm{Gen}(1^{\lambda})$：
    1. 均匀选取 $\mathbf{A}\leftarrow\mathbb{Z}_{q}^{n\times m}$
    2. 均匀选取 $\mathbf{s}\leftarrow\mathbb{Z}_{q}^{n}$，$\mathbf{e}\leftarrow[-B_{\chi},B_{\chi}]^{m}$，计算 $\mathbf{h}:=\mathbf{A}^{\top}\mathbf{s}+\mathbf{e}\in\mathbb{Z}_{q}^{m}$
    3. 输出 $PK=(\mathbf{A},\mathbf{h})$，$SK=\mathbf{s}$
- **加密算法** $C\leftarrow \mathrm{Enc}(PK,M)$：消息空间为 $\mathbb{M}=\{0,1\}$
    1. 均匀选取 $\mathbf{r}\leftarrow\{0,1\}^{m}$
    2. 计算 $\mathbf{c}_{1}:=\mathbf{A}\cdot \mathbf{r}\in\mathbb{Z}_{q}^{n}$
    3. 计算 $c_{2}:=\mathbf{r}^{\top}\mathbf{h}+M\cdot\lfloor q/2\rceil\in\mathbb{Z}_{q}$
    4. 输出 $C:=(\mathbf{c}_{1},c_{2})$
- **解密算法** $M'\leftarrow \mathrm{Dec}(SK,C=(\mathbf{c}_{1},c_{2}))$:
    1. 计算 $d:=c_{2}-\mathbf{c}_{1}^{\top}\mathbf{s}\in\mathbb{Z}_{q}$
    2. 如果 $d\approx0$，输出 $M':=0$；如果 $d\approx\lfloor q/2\rceil$，输出 $M':=1$
- **正确性分析**：对于 $\forall PK=(\mathbf{A},\mathbf{h})=(\mathbf{A},\mathbf{A}^{\top}\mathbf{s}+\mathbf{e})$，其中 $\mathbf{e}\leftarrow[-B_{\chi},B_{\chi}]^{m}$；$\forall C=(\mathbf{c}_{1},c_{2})=(\mathbf{A}\mathbf{r},\mathbf{r}^{\top}\mathbf{h}+M\cdot\lfloor q/2\rceil)$，其中 $\mathbf{r}\leftarrow\{0,1\}^{m}$：
    - 解密时，有
        $$
        \begin{aligned}
        d &=\mathbf{c}_{2}-\mathbf{c}_{1}^{\top}\mathbf{s} \\
        &=\mathbf{r}^{\top}\mathbf{h}+M\cdot\lfloor q/2\rceil-(\mathbf{A}\mathbf{r})^{\top}\mathbf{s} \\&=\mathbf{r}^{\top}(\mathbf{A}^{\top}\mathbf{s}+\mathbf{e})+M\cdot\lfloor q/2\rceil-(\mathbf{A}\mathbf{r})^{\top}\mathbf{s} \\
        &=\mathbf{r}^{\top}\mathbf{e}+M\cdot\lfloor q/2\rceil
        \end{aligned}
        $$
    - 由于 $\mathbf{e}\leftarrow[-B_{\chi},B_{\chi}]^{m}$，$\mathbf{r}\leftarrow\{0,1\}^{m}$，因此 $\mathbf{r}^{\top}\mathbf{e}\in[-mB_{\chi},mB_{\chi}]\subseteq(-q/4,q/4)$
        - 如果 $M=0$，则 $d=\mathbf{r}^{\top}\mathbf{e}\in(-q/4,q/4)=[0,q/4)\cup(3q/4,q-1]$
        - 如果 $M=1$，则 $d=\mathbf{r}^{\top}\mathbf{e}+\lfloor q/2\rceil\in(-q/4,q/4)+\lfloor q/2\rceil=(q/4,3q/4)$
    - 综上：
        - 当 $M=0$ 时，$d\approx0$，则 $M'=0$
        - 当 $M=1$ 时，$d\approx\lfloor q/2\rceil$，则 $M'=1$

#### Regev 公钥加密算法（加密 $\ell$ 比特）
- 设计思想：
    1. Regev 算法（加密 1 比特）
    2. 并行 $\ell$ 次（共用 $A$ 和 $r$）
- **密钥生成算法** $(PK,SK)\leftarrow \mathrm{Gen}(1^{\lambda})$：
    1. 均匀选取 $\mathbf{A}\leftarrow\mathbb{Z}_{q}^{n\times m}$
    2. 均匀选取 $\mathbf{S}\leftarrow\mathbb{Z}_{q}^{n\times\ell}$，$\mathbf{E}\leftarrow[-B_{\chi},B_{\chi}]^{m\times\ell}$，计算 $\mathbf{H}:=\mathbf{A}^{T}\mathbf{S}+\mathbf{E}\in\mathbb{Z}_{q}^{m\times\ell}$
    3. 输出 $PK=(\mathbf{A},\mathbf{H})$，$SK=\mathbf{S}$
- **加密算法** $C\leftarrow \mathrm{Enc}(PK, \mathbf{M}=(M_{1},\cdots,M_{\ell}))$：消息空间为 $\mathbb{M}=\{0,1\}^{\ell}$
    1. 均匀选取 $\mathbf{r}\leftarrow\{0,1\}^{m}$
    2. 计算 $\mathbf{c}_{1}:=\mathbf{A}\cdot \mathbf{r}\in\mathbb{Z}_{q}^{n}$
    3. 计算 $\mathbf{c}_{2}:=\mathbf{r}^{\top}\mathbf{H}+(M_{1}\cdot\lfloor q/2\rceil,\cdots,M_{\ell}\cdot\lfloor q/2\rceil)\in\mathbb{Z}_{q}^{1\times\ell}$（简记为$M\cdot\lfloor q/2\rceil$）
    4. 输出 $C:=(\mathbf{c}_{1},\mathbf{c}_{2})$
- **解密算法** $M'\leftarrow \mathrm{Dec}(SK,C=(\mathbf{c}_{1},\mathbf{c}_{2}))$：
    1. 计算 $\mathbf{d}:=\mathbf{c}_{2}-\mathbf{c}_{1}^{\top}\mathbf{S}\in\mathbb{Z}_{q}^{1\times\ell}$；将 $\mathbf{d}$ 按分量展开为 $(d_{1},\cdots,d_{\ell})$
    2. $\forall i\in\{1,\cdots,\ell\}$：如果 $d_{i}\approx0$，$M_{i}':=0$；如果 $d_{i}\approx\left\lfloor\frac{q}{2}\right\rceil$，$M_{i}':=1$
    3. 输出 $\mathbf{M}':=(M_{1}',\cdots,M_{\ell}')$

#### 1 比特 Regev 算法的 IND-CPA 安全性
- **定理**：**判定性 LWE 问题困难** $\implies$ **1 比特 Regev 算法是 IND-CPA 安全的**
    - **思路**：正向证明，给定任意 PPT 敌手 $\mathcal{A}$，证明其攻破 IND-CPA 安全性的优势是可忽略的
        $$
        \mathrm{Adv}_{\mathcal{A}}=\left|\Pr(\mathrm{output}=b)-\frac{1}{2}\right|=\mathrm{negl}(\lambda)
        $$ 

!!! fold info @Proof
    - **证明**：采用混合论证（Hybrid Arguments）与三角不等式。
        ![](image/image-20.png)
        - **定义混合游戏**：
            - **Game 0**：IND-CPA 安全模型
                1. $\mathrm{Gen}: \mathbf{A}\leftarrow\mathbb{Z}_{q}^{n\times m}$，$\mathbf{s}\leftarrow\mathbb{Z}_{q}^{n}$，$\mathbf{e}\leftarrow[-B_{\chi},B_{\chi}]^{m}$，$\mathbf{h}:=\mathbf{A}^\top \mathbf{s}+\mathbf{e}$，输出 $PK=(\mathbf{A},\mathbf{h})$
                3. $\mathrm{Enc}(PK,M_b): \mathbf{r}\leftarrow\{0,1\}^m$，$\mathbf{c}_1:=\mathbf{A}\mathbf{r}$，$c_2:=\mathbf{r}^\top\mathbf{h}+M_b\cdot\lfloor q/2\rceil$，输出 $C^*=(\mathbf{c}_1,c_2)$
            - **Game 1**：$PK$ 中的 $h\leftarrow\mathbb{Z}_{q}^{m}$
                1. $\mathrm{Gen}': \mathbf{A}\leftarrow\mathbb{Z}_{q}^{n\times m}$，$\mathbf{h}\leftarrow\mathbb{Z}_{q}^{m}$，输出 $PK=(\mathbf{A},\mathbf{h})\leftarrow\mathbb{Z}_{q}^{n\times m}\times\mathbb{Z}_{q}^{m}$
                2. 其余算法与 Game 0 一致
            - **Game 2**：$C^*\leftarrow\mathbb{Z}_{q}^{n}\times\mathbb{Z}_{q}$
                1. $\mathrm{Enc}'(PK,M_b): \mathbf{c}_1\leftarrow\mathbb{Z}_{q}^{n}$，$c_2\leftarrow\mathbb{Z}_{q}$，$C^*=(\mathbf{c}_1,c_2)\leftarrow\mathbb{Z}_{q}^{n}\times\mathbb{Z}_{q}$
                2. 其余算法与 Game 1 一致
        - **混合游戏性质**：
            1. Game 0 与 Game 1 的区别只在于 $PK$ 中的 $\mathbf{h}$ 的生成：
                - Game 0 中 $\mathbf{h}=\mathbf{A}^{\top}\mathbf{s}+\mathbf{e}$
                - Game 1 中 $\mathbf{h}\leftarrow\mathbb{Z}_{q}^{m}$
            2. 由**引理1**：$(\mathbf{A},\mathbf{A}^{\top}\mathbf{s}+\mathbf{e})$ 与 $(\mathbf{A},\mathbf{u}\leftarrow\mathbb{Z}_{q}^{m})$ 不可区分，即 Game 0 与 Game 1 不可区分，即
                $$
                |\Pr(\mathrm{output}=b\mid \text{Game 0}) - \Pr(\mathrm{output}=b\mid \text{Game 1})| = \mathrm{negl}(\lambda)
                $$
            2. Game 1 与 Game 2 的区别只在于密文 $C^{*}=(\mathbf{c}_{1},c_{2})$ 的生成：
                - Game 1 中 $(\mathbf{c}_{1},c_{2})=(\mathbf{A}\mathbf{r},\mathbf{r}^{\top}\mathbf{h}+M_b\cdot\lfloor q/2\rceil)$
                - Game 2 中 $(\mathbf{c}_1,c_2)\leftarrow\mathbb{Z}_{q}^{n}\times\mathbb{Z}_{q}$
            3. 由**引理2（剩余哈希引理）推论**：$\begin{pmatrix}\begin{pmatrix}\mathbf{A} \\ \mathbf{h}^\top \end{pmatrix},\begin{pmatrix}\mathbf{Ar} \\ \mathbf{h}^\top\mathbf{r} \end{pmatrix} \end{pmatrix}$ 与 $\begin{pmatrix}\begin{pmatrix}\mathbf{A} \\ \mathbf{h}^\top \end{pmatrix},\begin{pmatrix}\mathbf{u}_1 \\ \mathbf{u}_2 \end{pmatrix} \end{pmatrix}$ 不可区分，即 Game 1 与 Game 2 不可区分，即
                $$
                |\Pr(\mathrm{output}=b\mid \text{Game 1}) - \Pr(\mathrm{output}=b\mid \text{Game 2})| = \mathrm{negl}(\lambda)
                $$
            4. Game 2 中，$C^{*}=(\mathbf{c}_{1},c_{2})$ 中已经不含 $b$ 的信息，因此
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

- **引理 1**：**判定性 LWE 问题困难** $\implies$ Game 0 与 Game 1 不可区分，即
    $$
    |\Pr(\mathrm{output}=b\mid \text{Game 0}) - \Pr(\mathrm{output}=b\mid \text{Game 1})| = \mathrm{negl}(\lambda)
    $$
    - 证明思路：采用反证法（安全性归约），由区分 Game 0 与 Game 1 的敌手 $\mathcal{A}$ 来构造解决判定性 LWE 问题的敌手 $\mathcal{B}$。
- **引理 2**：
    - **剩余哈希引理**（Leftover Hash Lemma）: 当 $q$ 为素数且 $m\geq n\log q+2\log(1/\epsilon)$ 时，任意敌手区分下面两个分布的优势为可忽略的
        $$
        (\mathbf{H},\mathbf{Hr})\approx_s(\mathbf{H},\mathbf{u})
        $$

        其中 $\mathbf{H}\leftarrow\mathbb{Z}_{q}^{n\times m}$，$\mathbf{r}\leftarrow\{0,1\}^{m}$，$\mathbf{u}\leftarrow\mathbb{Z}_{q}^{n}$。
    - **推论**：当 $q$ 为素数且 $m\geq2n\cdot\log q$ 时，任意敌手区分下面两个分布的优势为可忽略的
        $$
        \begin{pmatrix}\begin{pmatrix}\mathbf{A} \\ \mathbf{h}^\top \end{pmatrix},\begin{pmatrix}\mathbf{Ar} \\ \mathbf{h}^\top\mathbf{r} \end{pmatrix} \end{pmatrix}
        \approx_s
        \begin{pmatrix}\begin{pmatrix}\mathbf{A} \\ \mathbf{h}^\top \end{pmatrix},\begin{pmatrix}\mathbf{u}_1 \\ \mathbf{u}_2 \end{pmatrix} \end{pmatrix}
        $$

        其中 $\mathbf{A}\leftarrow\mathbb{Z}_{q}^{n\times m}$，$\mathbf{h}\leftarrow\mathbb{Z}_{q}^{m}$，$\mathbf{r}\leftarrow\{0,1\}^{m}$，$\mathbf{u}_{1}\leftarrow\mathbb{Z}_{q}^{n}$，$\mathbf{u}_{2}\leftarrow\mathbb{Z}_{q}$。

#### $\ell$ 比特 Regev 算法的 IND-CPA 安全性
- **定理**：**判定性 LWE 问题困难** $\implies$ **$\ell$ 比特 Regev 算法是 IND-CPA 安全的**
    - 思路：采用混合论证，结合剩余哈希引理与 LWE 假设。
        - 混合论证：
            - **Game 0**（IND-CPA 安全模型）中：$\mathbf{H}=\mathbf{A}^{\top}\mathbf{S}+\mathbf{E}$，$\mathbf{c}_1=\mathbf{A}\mathbf{r}$，$\mathbf{c}_2=\mathbf{r}^{\top}\mathbf{H}+\mathbf{M}\cdot\lfloor q/2\rceil$
            - **Game 1** 中：$\mathbf{H}\leftarrow\mathbb{Z}_{q}^{m\times\ell}$
            - **Game 2** 中：$\mathbf{c}_{1}\leftarrow\mathbb{Z}_{q}^{n}$，$\mathbf{c}_{2}\leftarrow\mathbb{Z}_{q}^{1\times\ell}$
        - 通过 LWE 假设（每次换 $H$ 的第 $i$ 列，共混合论证 $\ell$ 次）证明 Game 0 与 Game 1不可区分。
        - 通过剩余哈希引理证明 Game 1与 Game 2不可区分，最终得证 IND-CPA 安全性。

### SIS 困难问题与 GPV 签名算法
#### SIS 问题（Short Integer Solution）
- **SIS 问题**：$n,m$ 为整数，$q$ 为素数，$B_{s}$ 为整数。均匀选取 $\mathbf{A} \leftarrow\mathbb{Z}_{q}^{n\times m}$。
    - **输入**：$\mathbf{A}$
    - **输出**：$\mathbf{x}\in\mathbb{Z}_{q}^{m}$ 满足
        1. $\mathbf{Ax}=\mathbf{0}\in\mathbb{Z}_{q}^{n}$
        2. $\mathbf{x}\neq\mathbf{0}$
        3. $\mathbf{x}\in[-B_{s},B_{s}]^{m}$（Short）
    - **SIS 问题困难**：任意 PPT 敌手的优势是可忽略的，即
        $$
        \Pr(\text{find such } \mathbf{x}) = \mathrm{negl}(\lambda)
        $$
    - 观察:
        1. 如果没有对于 $\mathbf{x}$ 的限制，使用高斯消元法很容易求解 $\mathbf{x}$。
        2. 对于 SIS 问题，$m$ 越大越容易，$n$ 越大越困难。
    - 直觉：定义正交格 $\Lambda^\bot(\mathbf{A}) =\{\mathbf{x}\in\mathbb{Z}^{m} \mid \mathbf{Ax}\equiv\mathbf{0}\pmod q\}$，SIS 问题要求找到 $\mathcal{L}(\mathbf{A})$ 中一个非零的短向量。
        ![](image/image-19.png)
- **定理**：如果 $m\cdot B_{\chi}\cdot B_{s}<q/4$，则 **判定性 LWE 问题 $(n,m,q,\chi)$ 困难** $\implies$ **SIS 问题 $(n,m,q,B_{s})$ 困难**

!!! fold info @Proof
    - **证明**：由解决 SIS 问题的敌手 $\mathcal{A}$ 来构造解决判定性 LWE 问题的敌手 $\mathcal{B}$。
        - 若存在 PPT 敌手 $\mathcal{A}$ 能够以不可忽略的优势解决 SIS 问题 $(n,m,q,B_{s})$，则输入 $\mathbf{A}$，$\mathcal{A}$ 以不可忽略的优势输出 $\mathbf{x}\in[-B_{s},B_{s}]^{m}$ 满足 $\mathbf{Ax}=\mathbf{0}\in\mathbb{Z}_{q}^{n}$。
        - 此时考虑 $\mathbf{z}_\beta^\top \cdot \mathbf{x}$：
            - 当 $\beta=0$ 时，$\mathbf{z}_{0}=\mathbf{A}^{\top}\mathbf{s}+\mathbf{e}$，因此
                $$
                \begin{aligned}
                \mathbf{z}_{0}^{\top}\cdot \mathbf{x}&=\mathbf{s}^{\top}\mathbf{A}\mathbf{x}+\mathbf{e}^{\top}\mathbf{x}=\mathbf{e}^{\top}\mathbf{x}=\sum_{i=1}^{m} e_i x_i \\
                &\in[-mB_{\chi}B_{s},mB_{\chi}B_{s}] \subseteq(-q/4,q/4)
                \end{aligned}
                $$
            - 当 $\beta=1$ 时，$\mathbf{z}_{1}\leftarrow\mathbb{Z}_{q}^{m}$，因此 $\mathbf{z}_{1}^{\top}\cdot \mathbf{x}$ 在 $\mathbb{Z}_{q}$ 上均匀分布，则 $\mathbf{z}_{1}^{\top}\cdot \mathbf{x} \in(-q/4,q/4)$ 的概率为 $1/2$。
        - $\mathcal{B}$ 的输出：
            $$
            \mathrm{output}_\mathcal{B} = \begin{cases}
            0 & \mathbf{z}^\top\cdot \mathbf{x} \in(0,\frac{q}{4})\cup(\frac{3q}{4},q) \\
            1 & \text{otherwise}
            \end{cases}
            $$
        - $\mathcal{B}$ 的优势
            $$
            \begin{aligned}
            \mathrm{Adv}_\mathcal{B} &= \left|\Pr(\mathrm{output}_\mathcal{B}=\beta) - \frac{1}{2}\right| \\
            &= \frac{1}{2} \cdot \left|\Pr(\mathrm{output}_\mathcal{B}=0 \mid \beta=0) - \Pr(\mathrm{output}_\mathcal{B}=0 \mid \beta=1)\right| \\
            &= \frac{1}{2} \cdot \left|\left(\mathrm{Adv}_\mathcal{A} + \frac{1}{2}(1 - \mathrm{Adv}_\mathcal{A}) \right) - \frac{1}{2} \right| \\
            &= \frac{1}{4} \cdot \mathrm{Adv}_\mathcal{A} = \text{non-negl}(\lambda)
            \end{aligned}
            $$
        - 因此 $\mathcal{B}$ 以不可忽略的优势攻破判定性 LWE 问题，与假设矛盾。

#### 原像可采样函数 PSF（Preimage Sampleable Function）
- PSF 参数：$(n,m,q,B)$
- **矩阵 A 的带陷门采样算法**：$(\mathbf{A},td)\leftarrow \mathrm{SampleA}(1^{\lambda})$
    - 矩阵 $\mathbf{A}$ 在 $\mathbb{Z}_{q}^{n\times m}$ 上均匀分布，$td$ 为陷门信息（trapdoor）
    - 定义函数
        $$
        \begin{aligned}
        f_{A}:\mathbb{Z}_{q}^{m}&\to\mathbb{Z}_{q}^{n} \\
        \mathbf{x} &\mapsto \mathbf{Ax}
        \end{aligned}
        $$ 则给出 $x$ 计算 $f_{A}(x)$ 是高效的；给出 $y$ 和陷门信息 $td$ 计算 $x$ 满足 $f_{A}(x)=y$ 是高效的；但给出 $y$ 没有陷门信息 $td$ 计算 $x$ 满足 $f_{A}(x)=y$ 是困难的。
- **正向采样算法**：$(\mathbf{x}\in\mathbb{Z}_{q}^{m},\mathbf{y}\in\mathbb{Z}_{q}^{n})\leftarrow \mathrm{SampleTuple}(\mathbf{A})$
    - 向量 $\mathbf{x}$ 满足 $\mathbf{x}\in[-B,B]^{m}$
    - 向量 $\mathbf{y}$ 满足 $\mathbf{y}=\mathbf{A}\mathbf{x}$ 且在 $\mathbb{Z}_{q}^{n}$ 上均匀分布
- **原像采样算法**：$\mathbf{x}\in\mathbb{Z}_{q}^{m}\leftarrow \mathrm{SamplePre}(td,\mathbf{A},\mathbf{y}\in\mathbb{Z}_{q}^{n})$:
    - 向量 $\mathbf{x}$ 满足 $\mathbf{x}\in[-B,B]^{m}$ 且 $\mathbf{Ax}=\mathbf{y}$
- **关键性质**（GPV08）：下面两种 $(\mathbf{x}\in\mathbb{Z}_{q}^{m},\mathbf{y}\in\mathbb{Z}_{q}^{n})$ 的分布一样:
    - $(\mathbf{x}\in\mathbb{Z}_{q}^{m},\mathbf{y}\in\mathbb{Z}_{q}^{n})\leftarrow \mathrm{SampleTuple}(\mathbf{A})$
    - 先均匀选取 $\mathbf{y}\leftarrow\mathbb{Z}_{q}^{n}$，再调用 $\mathbf{x}\leftarrow \mathrm{SamplePre}(td,\mathbf{A},\mathbf{y})$

#### MP12 陷门生成算法
##### 目标
- 给定 $\mathbf{A} \leftarrow \mathbb{Z}_q^{n \times m}$，求解 $\mathbf{T}_A \in \mathbb{Z}^{m \times m}$ 满足：
    1. $\mathbf{A} \cdot \mathbf{T}_A \equiv \mathbf{0} \pmod q$
    2. $\mathbf{T}_A \in [-B_s, B_s]^{m \times m}$
    3. $\mathbf{T}_A$ 满秩
- 寻找陷门 $\mathbf{T}_A$ 本质上就是为正交格 $\Lambda^\bot(\mathbf{A}) =\{\mathbf{x}\in\mathbb{Z}^{m} \mid \mathbf{Ax}\equiv\mathbf{0}\pmod q\}$ 寻找一组短基（Short Basis）
- 如果得到了陷门，那么 SIS 问题、LWE 问题都不再困难！

##### 构造 Gadget 矩阵 $G$ 及其陷门 $T_G$
- **思路**：由于直接为一个随机矩阵 $A$ 寻找陷门困难的，因此人为构造一个结构极度规律的特殊矩阵 $G$，它的陷门可以直接写出。
- **构造方法**：
    1. 二进制拆分
        - 定义二进制位宽 $k = \lceil \log q \rceil$，则 $2^{k-1} < q \leq 2^k$
        - 对于任意整数 $x \in \mathbb{Z}_q$，它可以被唯一拆分为二进制表示 $x = x_0 + 2\cdot x_1 + \dots + 2^{k-1}\cdot x_{k-1} \quad (x_i \in \{0, 1\})$，定义提取函数：$x \mapsto \langle x \rangle _{BE} = (x_0, x_1, \dots, x_{k-1})^\top$
    2. 定义 $\mathbf{g} = (1, 2, 2^2, \dots, 2^{k-1})_{1 \times k}$，则有 $x = \mathbf{g} \cdot \langle x \rangle _{BE}$
    3. 通过张量积将 $\mathbf{g}$ 扩展为 $n \times nk$ 维的对角块矩阵：
        $$
        \mathbf{G} := \mathbf{I}_n \otimes \mathbf{g} = \begin{pmatrix} \mathbf{g} & \mathbf{0} & \dots & \mathbf{0} \\
        \mathbf{0} & \mathbf{g} & \dots & \mathbf{0} \\
        \vdots & \vdots & \ddots & \vdots \\
        \mathbf{0} & \mathbf{0} & \dots & \mathbf{g}
        \end{pmatrix}_{n \times nk}
        $$
    4. 构造针对 $\mathbf{g}$ 的一维陷门 $\mathbf{T}_g$：
        $$
        \mathbf{T}_g = \begin{pmatrix}
        2 & 0 & 0 & \dots & 0 \\
        -1 & 2 & 0 & \dots & 0 \\
        0 & -1 & 2 & \dots & 0 \\
        \vdots & \vdots & \vdots & \ddots & \vdots \\
        0 & 0 & 0 & \dots & 2 \\
        \end{pmatrix}_{k \times k}
        $$

        易证 $\mathbf{g} \cdot \mathbf{T}_g = \mathbf{0} \pmod q$
    5. 扩展到全维，得到针对 $\mathbf{G}$ 的陷门 $\mathbf{T}_G$：
        $$
        \mathbf{T}_G := \mathbf{I}_n \otimes \mathbf{T}_g = \begin{pmatrix}
        \mathbf{T}_g & & \\
        & \ddots & \\
        & & \mathbf{T}_g
        \end{pmatrix}_{nk \times nk}
        $$
        - $\mathbf{G} \cdot \mathbf{T}_G = \mathbf{0} \pmod q$ 显然成立
        - $\mathbf{T}_G$ 内部元素仅包含 $2, -1, 0$，满足短矩阵要求
        - $\mathbf{T}_G$ 是下三角矩阵，主对角线不为 0，满秩。

##### 定义逆向操作函数 $\mathbf{G}^{-1}(\cdot)$
- 定义：给定任意目标向量 $\mathbf{y} \in \mathbb{Z}_q^n$，求解 $\mathbf{x} \in \{0, 1\}^{nk}$ 使得 $\mathbf{G} \cdot \mathbf{x} = \mathbf{y} \pmod q$，即
    $$
    \mathbf{x} = \mathbf{G}^{-1}(\mathbf{y})
    $$
    - 实际上解决了一个特定于 $\mathbf{G}$ 的 SIS 问题。
- **计算方法**：将向量 $\mathbf{y} = (y_1, y_2, \dots, y_n)^\top$ 的每一个元素分别进行二进制拆分，得到 $\langle y_i \rangle _{BE~n\times 1}$，然后将它们竖着拼接起来：
    $$
    \mathbf{x} := \mathbf{G}^{-1}(\mathbf{y}) = \begin{pmatrix}
    \langle y_1 \rangle _{BE} \\
    \langle y_2 \rangle _{BE} \\
    \vdots \\
    \langle y_n \rangle _{BE}
    \end{pmatrix}_{nk \times 1} \in \{0, 1\}^{nk}
    $$
- **正确性验证**：因为 $\mathbf{G} = I_n \otimes \mathbf{g}$，并且根据前面的定义有 $\mathbf{g} \cdot \langle y_i \rangle _{BE} = y_i$，所以：
    $$
    \mathbf{G} \cdot \mathbf{x} = \begin{pmatrix}
    \mathbf{g} & \mathbf{0} & \dots & \mathbf{0} \\
    \mathbf{0} & \mathbf{g} & \dots & \mathbf{0} \\
    \vdots & \vdots & \ddots & \vdots \\
    \mathbf{0} & \mathbf{0} & \dots & \mathbf{g}
    \end{pmatrix} \cdot \begin{pmatrix}
    \langle y_1 \rangle _{BE} \\
    \langle y_2 \rangle _{BE} \\
    \vdots \\
    \langle y_n \rangle _{BE}
    \end{pmatrix} = \begin{pmatrix}
    \mathbf{g} \cdot \langle y_1 \rangle _{BE} \\ \mathbf{g} \cdot \langle y_2 \rangle _{BE} \\
    \vdots \\
    \mathbf{g} \cdot \langle y_n \rangle _{BE}
    \end{pmatrix} = \begin{pmatrix}
    y_1 \\ y_2 \\ \vdots \\ y_n
    \end{pmatrix} = \mathbf{y}
    $$

##### 生成随机公钥及其陷门
1. **密钥生成**：
    - 掩码矩阵：$\mathbf{B} \leftarrow \mathbb{Z}_q^{n \times m^*}$
    - **生成私钥**：$\mathbf{R} \leftarrow \{0, 1\}^{m^* \times nk}$
    - **构造公钥**：$\mathbf{A} := \begin{pmatrix} \mathbf{B} & \mathbf{BR} + \mathbf{G} \end{pmatrix}_{n \times (m^* + nk)}$
        - 基于剩余哈希引理：由于 $\mathbf{R}$ 是未知的短矩阵，$\mathbf{B}$ 是随机的，因此 $\mathbf{BR}$ 看起来也是完全随机的；$\mathbf{BR}+\mathbf{G}$ 完美地掩盖了 $\mathbf{G}$ 的痕迹。在外界看来，$\mathbf{A}$ 只是一个普通的随机矩阵。
2. **利用私钥提取 $G$**：拥有私钥 $\mathbf{R}$ 的人，可以通过右乘一个特殊矩阵提取出 $\mathbf{G}$：
    $$
    \mathbf{A} \cdot \begin{pmatrix} -\mathbf{R} \\ \mathbf{I} \end{pmatrix}
    = \begin{pmatrix} \mathbf{B} \quad \mathbf{BR}+\mathbf{G} \end{pmatrix} \cdot \begin{pmatrix} -\mathbf{R} \\ \mathbf{I} \end{pmatrix}
    = -\mathbf{BR} + (\mathbf{BR}+\mathbf{G})
    = \mathbf{G}
    $$
3. **构造随机公钥陷门 $\mathbf{T}_A$**：利用私钥 $\mathbf{R}$、公开的工具陷门 $\mathbf{T}_G$、以及操作函数 $\mathbf{G}^{-1}$，构造如下的分块矩阵 $\mathbf{T}_A$：
    $$
    \mathbf{T}_A := \begin{pmatrix} \mathbf{I} & -\mathbf{R} \\ \mathbf{0} & \mathbf{I} \end{pmatrix}\cdot \begin{pmatrix} \mathbf{I} & \mathbf{0} \\ -\mathbf{G}^{-1}(\mathbf{B}) & \mathbf{T}_G \end{pmatrix}
    = \begin{pmatrix} \mathbf{I} + \mathbf{R} \cdot \mathbf{G}^{-1}(\mathbf{B}) & -\mathbf{R} \cdot \mathbf{T}_G \\ -\mathbf{G}^{-1}(\mathbf{B}) & \mathbf{T}_G \end{pmatrix}
    $$

    - 满足 $\mathbf{A} \cdot \mathbf{T}_A = \mathbf{0}$：
        $$
        \begin{aligned}
        \mathbf{A} \cdot \mathbf{T}_A
        &= \begin{pmatrix} \mathbf{B} & \mathbf{BR}+\mathbf{G} \end{pmatrix} \cdot \begin{pmatrix} \mathbf{I} + \mathbf{R} \mathbf{G}^{-1}(\mathbf{B}) & -\mathbf{R} \mathbf{T}_G \\ -\mathbf{G}^{-1}(\mathbf{B}) & \mathbf{T}_G  \end{pmatrix} \\
        &= \begin{pmatrix} \mathbf{B}-\mathbf{G}\mathbf{G}^{-1}(\mathbf{B}) & \mathbf{G}\mathbf{T}_G \end{pmatrix} \\
        &= \begin{pmatrix} \mathbf{0} & \mathbf{0} \end{pmatrix} \\
        &= \mathbf{0}
        \end{aligned}
        $$
    - $\mathbf{T}_A$ 的四个分块：单位阵 $\mathbf{I}$、私钥 $\mathbf{R} \in \{0,1\}^{m^* \times nk}$、拆分矩阵 $\mathbf{G}^{-1}(\mathbf{B}) \in \{0,1\}^{nk \times n}$、工具陷门 $\mathbf{T}_G \in \{-1,0,2\}^{nk \times nk}$。由于所有基础构件都是极小的数字，它们相乘相加后依然是多项式级别的小数字。
    - 由于 $\mathbf{T}_A$ 可以分解为两个分块满秩三角阵的乘积，因此 $\mathbf{T}_A$ 本身也是满秩的。

#### GPV 数字签名算法（Gentry-Peikert-Vaikuntanathan）
- 组件：
    1. PSF：参数 $(n,m,q,B)$
    2. Hash Function $\mathrm{H}:\{0,1\}^{*}\to\mathbb{Z}_{q}^{n}$
- **密钥生成算法** $(PK,SK)\leftarrow \mathrm{Gen}(1^{\lambda})$：
    1. 调用 $(\mathbf{A},td)\leftarrow \mathrm{SampleA}(1^{\lambda})$，其中 $\mathbf{A}$ 在 $\mathbb{Z}_{q}^{n\times m}$ 上均匀分布
    2. 输出 $PK=\mathbf{A}$，$SK=td$
- **签名算法** $\sigma\leftarrow \mathrm{Sign}(SK,M)$：消息空间为 $\mathbb{M}=\{0,1\}^{*}$
    1. 计算 $\mathrm{H}(M)\in\mathbb{Z}_{q}^{n}$
    2. 调用并输出 $\sigma\in\mathbb{Z}_{q}^{m}\leftarrow \mathrm{SamplePre}(td,\mathbf{A},\mathrm{H}(M))$
- **验证算法** $0/1\leftarrow \mathrm{Verify}(PK,M,\sigma)$：
    1. 验证  $\sigma\in[-B,B]^{m} \land \mathbf{A}\sigma = \mathrm{H}(M)$
    2. 如果验证通过，输出 $1$；否则输出 $0$

#### GPV 数字签名算法的 EUF-CMA 安全性
- **定理**：**SIS 问题 $(n,m,q,B_{s}=2B)$ 困难** + **$\mathrm{H}$ 为 RO** $\implies$ **GPV 签名算法 EUF-CMA 安全**

!!! fold info @Proof
    - **证明**：安全性规约，由攻破 EUF-CMA 安全性的敌手 $\mathcal{A}$ 来构造解决 SIS 问题的敌手 $\mathcal{B}$。
        ![](image/image-21.png)
        - **$\mathcal{B}$ 的输入**：$\mathbf{A}\in\mathbb{Z}_{q}^{n\times m}$
        - **$\mathcal{B}$ 的策略**：
            - $\mathcal{B}$ 将公钥 $PK=\mathbf{A}$ 作为输入提供给 $\mathcal{A}$
            - $\mathcal{B}$ 维护一个哈希查询表 $\mathcal{T}$ 记录 $\mathrm{H}(M)$ 与 $\sigma$ 的对应关系
            - 当 $\mathcal{A}$ 使用 $M_i$ 进行**签名查询**时：$\mathcal{B}$ 首先检查 $\mathcal{T}$ 中是否存在 $M_i$ 的记录
                - 若存在，$\mathcal{B}$ 将记录的 $\sigma_i$ 返回给 $\mathcal{A}$
                - 否则，$\mathcal{B}$ 随机生成一个签名 $\sigma_i$ 返回给 $\mathcal{A}$，同时记录 $\mathrm{H}(M_i) = \mathbf{A}\sigma_i$ 到 $\mathcal{T}$ 中
            - 当 $\mathcal{A}$ 使用 $M_j$ 进行**哈希查询**时：$\mathcal{B}$ 首先检查 $\mathcal{T}$ 中是否存在 $M_j$ 的记录
                - 若存在，$\mathcal{B}$ 将记录的 $\mathrm{H}(M_j)$ 返回给 $\mathcal{A}$
                - 否则，$\mathcal{B}$ 随机生成一个签名 $\sigma_j$，计算 $\mathrm{H}(M_j) = \mathbf{A}\sigma_j$ 并将 $\mathrm{H}(M_j)$ 返回给 $\mathcal{A}$，同时记录 $\mathrm{H}(M_j) = \mathbf{A}\sigma_j$ 到 $\mathcal{T}$ 中
            - $\mathcal{A}$ 最终以不可忽略优势输出一对有效的消息签名对 $(M^*,\sigma^*)$，则 $\mathcal{A}$ 以不可忽略概率查询过 $\mathrm{H}(M^*)$，因此 $\mathcal{B}$ 已经记录了 $\mathrm{H}(M^*)$ 与某个签名 $\sigma$ 的对应关系
                - 若 $\sigma^* = \sigma$，则失败
                - 若 $\sigma^* \neq \sigma$，则有
                    $$
                    \begin{cases}
                    \mathbf{A}(\sigma^* - \sigma) = \mathrm{H}(M^*) - \mathrm{H}(M^*) = \mathbf{0} \\
                    \|\sigma^* - \sigma\| \leq \|\sigma^*\| + \|\sigma\| \leq 2B = B_s \implies
                    \sigma^* - \sigma \in [-B_s,B_s]^{m}
                    \end{cases}
                    $$ 因此 $\sigma^* - \sigma$ 是 $\mathbf{A}$ 的一个非零的短整数解。可以证明当 $m \gg n \log q$ 时，$\sigma^* \neq \sigma$ 的概率为不可忽略的。
        - **$\mathcal{B}$ 的输出**：$\mathbf{x} = \sigma^* - \sigma$
        - **$\mathcal{B}$ 的优势**：
            $$
            \begin{aligned}
            \mathrm{Adv}_{\mathcal{B}} &\geq \Pr\left[
            \begin{array}{l}
            (1)\ \mathcal{A} \text{ 成功攻破 GPV 签名算法的 EUF-CMA 安全性} \\
            (2)\ \mathcal{A} \text{ 查询过 } M^{*} \text{ 的 Hash 值} \\
            (3)\ \sigma^{*} \neq \sigma
            \end{array}
            \right] \\
            &= \text{non-negl}(\lambda)
            \end{aligned}
            $$

#### 基于 GPV 的身份基加密算法
- 组件：
    1. PSF：参数 $(n,m,q,B)$
    2. Hash Function $\mathrm{H}:\{0,1\}^{*}\to\mathbb{Z}_{q}^{n}$
- **密钥生成算法** $(PK,SK) \leftarrow \mathrm{Gen}(1^{\lambda})$：类似于GPV
    1. 调用 $(\mathbf{A},td) \leftarrow \mathrm{SampleA}(1^{\lambda})$
    2. 输出 $PK = \mathbf{A}$, $SK = td$
- **私钥派生算法** $SK_{id} \leftarrow \mathrm{Derive}(SK,id)$：身份空间为 $\{0,1\}^*$
    1. 计算并输出 $SK_{id} := \mathrm{SamplePre}(td,\mathbf{A},H(id)) \in\mathbb{Z}_{q}^{m}$
- **加密算法** $C \leftarrow \mathrm{Enc}(PK,id,M)$：消息空间为 $\mathbb{M}=\{0,1\}$
    1. 均匀选取 $\mathbf{r} \leftarrow \mathbb{Z}_{q}^{n}$，$\mathbf{e}_{1} \leftarrow_{\chi}[-B_{x},B_{x}]^{m}$，$\mathbf{e}_2 \leftarrow_{\chi}[-B_x,B_x]$
    2. 计算 $\mathbf{c}_{1}:=\mathbf{A}^{\top}\mathbf{r}+\mathbf{e}_{1} \in \mathbb{Z}_{q}^{m}$
    3. 计算 $c_{2}:=\mathbf{r}^{\top}H(id)+\mathbf{e}_{2}+M\cdot\lfloor q/2\rceil \in \mathbb{Z}_{q}$
    4. 输出 $C:=(\mathbf{c}_{1},c_{2})$
- **解密算法** $M' \leftarrow \mathrm{Dec}(SK_{id},C=(\mathbf{c}_{1},c_{2}))$：
    1. 计算 $d:=c_{2}-\mathbf{c}_{1}^{\top}\cdot SK_{id} \in \mathbb{Z}_{q}$
    2. 如果 $q/4<d<3q/4$，输出 $M':=1$；否则，输出 $M':=0$
