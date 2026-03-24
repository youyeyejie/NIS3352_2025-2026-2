# 后量子密码
## 格理论以及基于格困难问题的密码学

### 格理论简介
#### 格的定义
- **格**（Lattice）：格 $\mathcal{L}$ 是 $\mathbb{R}^m$ 空间中离散的具有加法运算的子群。
    - $\mathbb{R}^m$ 空间：由 $m$ 维实数向量组成的空间。
        - 向量的范式：$\| x\| =\sqrt{\sum_{i=1}^{m} x_i^2}$
        - 向量的距离：$\mathrm{dist}(x,y)=\| x-y\|$
    - 离散（Discrete）：每一个格点 $x \in \mathcal{L}$，存在 $\mathbb{R}^{m}$ 中的一个领域仅包含 $x$ 唯一格点;
    - 加法：
        - $(0,\cdots, 0) \in \mathcal{L}$
        - $\forall x,y \in \mathcal{L}, x-y \in \mathcal{L}$
- 示例：$\mathbb{Z}^{m}$、$(q\mathbb{Z})^{m}$ 是格，但 $\mathbb{Q}^{m}$，$2\mathbb{Z}+ 1$ 以及 $\mathbb{Z}+\mathbb{Z}\sqrt{2}$ 都不是格。
- **格基**：设 $\mathcal{L}$ 是 $\mathbb{R}^{m}$ 中的格，则存在 $\mathbb{R}$-线性无关的向量 $b_{1}, b_{2}, ..., b_{n} \in \mathbb{R}^{m}$，使得
    $$
    \mathcal{L}=\left\{z_{1} b_{1}+z_{2} b_{2}+\cdots+z_{n} b_{n} \mid z_{i} \in \mathbb{Z}\right\}
    $$

    - **维数**：$m$
    - **秩**：$n$
    - **满秩格**：当 $m=n$ 时称 $\mathcal{L}$ 是满秩格
    - **格基**：$B=(b_{1}, b_{2}, ..., b_{m})$
    - **格点**：向量 $v=z_{1} b_{1}+z_{2} b_{2}+\cdots+z_{n} b_{n} \in \mathcal{L}$
- **性质**：两组基 $B=\{b_{1}, ..., b_{n}\}$ 与 $B'=\{b_{1}', ..., b_{n}'\}$ 生成同一个格当且仅当存在一个幺模矩阵 $U \in \mathbb{Z}^{n\times n}$ 使得 $B=B' U$。
    - 幺模矩阵：$U$ 是一个整数矩阵，且 $\det U=\pm 1$。
- **基本平行多面体**（Fundamental parallelepipeds）：设 $\mathcal{L}$ 为满秩格，则格 $\mathcal{L}$ 的基本平行多面体定义为
    $$
    \mathcal{F}(B):= \mathbb{R}^{m}/\mathcal{L} = \{ \sum _{i=1}^{m}x_{i}b_{i} \mid x_{i}\in [0,1)\}
    $$
    - $\mathcal{F}(B)$ 的体积为
        $$
        \mathrm{vol}(\mathcal {F}(B))=|\det M(B)|,\quad M(B)=\begin{pmatrix}b_{1} & b_{2} & \cdots & b_{m}\end{pmatrix}
        $$
- **定理**：
    1. 设 $b_{1}, b_{2}, ..., b_{m} \in \mathcal{L}$ 且整线性无关，则 $B=\{b_{1}, b_{2}, ..., b_{m}\}$ 为格基当且仅当
        $$
        \mathcal{F}(B) \cap \mathcal{L}=\{(0, ..., 0)\}
        $$
    2. 设 $B=\{b_{1}, b_{2}, ..., b_{m}\}$ 和 $B'=\{b_{1}', b_{2}', ..., b_{m}'\}$ 为两组格基，则
        $$
        \mathrm{vol}(\mathcal{F}(B))=\mathrm{vol}\left(\mathcal{F}\left(B'\right)\right)
        $$
    3. 设 $B=\{b_{1}, b_{2}, ..., b_{m}\}$ 为格基，定义 $\mathcal{L}$ 的行列式为
        $$
        \det(\mathcal{L}):=\mathrm{Vol}(\mathcal{F}(B))=|\det(B)|
        $$

#### 格的困难问题
- **格的最小距离**：设 $\mathcal{L}$ 是 $\mathbb{R}^{m}$ 中的格，则 $\mathcal{L}$ 的最小距离定义为
    $$
    \lambda_{1}(\mathcal{L})=min \{\| v\| : v \in \mathcal{L} \setminus\{0\}\}=min \{\| x-y\| : x \neq y \in \mathcal{L}\}
    $$
- **Minkowski’s first theorem**：设 $\mathcal{L}$ 为秩是 $m$ 的格，则
    $$
    \lambda_{1}(\mathcal{L}) \leq \sqrt{m}(\det \mathcal{L})^{1/m}
    $$
    - 示例：$b_{1}=(0,2^{-100})$，$b_{2}=(2^{100}, 0)$ 那么 $\lambda_{1}(\mathcal{L}(b_{1}, b_{2}))=2^{-100} \ll \sqrt{2}$。

- **最短向量问题** $(SVP)$：给定格 $\mathcal{L}$ 的任意格基 $B$，找到 $v \in \mathcal{L}\setminus\{0\}$ 使得
    $$
    \| v\| =\lambda_{1}(\mathcal{L})
    $$
- **最近向量问题** $(CVP)$：给定格 $\mathcal{L}$ 的任意格基 $B$，以及 $t \in \mathbb{R}^{m}$，找到 $v \in \mathcal{L}$ 使得
    $$
    \forall y \in \mathcal{L}, \| v-t\| \leq \| y-t\|
    $$
- **近似最短向量问题** $(SVP_{\gamma})$：给定格 $\mathcal{L}$ 的任意格基 $B$，找到 $v \in \mathcal{L}\setminus\{0\}$ 使得
    $$
    \| v\| ≤\gamma(m) \cdot \lambda_{1}(\mathcal{L})
    $$
- **近似最近向量问题** $(CVP_{\gamma})$：给定格 $\mathcal{L}$ 的任意格基 $B$，以及 $t \in \mathbb{R}^{m}$，找到 $v \in \mathcal{L}$ 使得
    $$
    \forall y \in \mathcal{L}, \| v-t\| \leq \gamma(m) \cdot\| y-t\|
    $$
- **有界距离解码问题** $(BDD_{\delta})$：给定格 $\mathcal{L}$ 的任意格基 $B$，以及 $t \in \mathbb{R}^{m}$，满足 $\mathrm{dist}(\mathcal{L}, t) \leq\delta<\frac{\lambda_{1}(\mathcal{L})}{2}$，找到唯一的格向量 $w \in \mathcal{L}$，使得
    $$
    \| w-t\| _{2} \leq \delta
    $$
    - $BDD_{\delta} \subset CVP_{\delta}$
    - $BDD_{\delta}$ 的计算复杂度随着维度 $n$ 和参数 $\delta$ 的增大而增加。
- **定理**：$SVP_{\gamma(m)} \leq_{P} CVP_{\gamma(m)}$。

### LWE 问题（Learning with Errors）
- **计算性 LWE 问题**（CLWE）：$n,m$ 为整数，$q$ 为正整数，$\chi$ 为区间 $[-B_{\chi}, B_{\chi}]$ 上的概率分布。均匀选取 $A \leftarrow \mathbb{Z}_{q}^{n ×m}$，$s \leftarrow \mathbb{Z}_{q}^{n}$，根据 $\chi$ 分布选取 $e \leftarrow[-B_{\chi}, B_{\chi}]^{m}$，计算 $z:=A^{\top} s+e \in \mathbb{Z}_{q}^{m}$。
    - **输入**：$(A, z)$
    - **输出**：$\mathrm{output}$
    - **计算性 LWE 问题困难**：任意 PPT 敌手的优势是可忽略的，即
        $$
        \left|\Pr(\mathrm{output}=s) - \frac{1}{q^{n}}\right| = \mathrm{negl}(\lambda)
        $$
    - 直觉：定义格 $\mathcal{L}(A)=\{x \in \mathbb{Z}^{m} \mid x=A^{T} s \pmod q, s \in \mathbb{Z}_{q}^{n}\}$，则 $z$ 是 $\mathcal{L}(A)$ 中某个格点附近的一个点，$e$ 是噪声。
- **判定性 LWE 问题**（DLWE）：$n,m$ 为整数，$q$ 为正整数，$\chi$ 为区间 $[-B_{\chi}, B_{\chi}]$ 上的概率分布。均匀选取 $A \leftarrow \mathbb{Z}_{q}^{n ×m}$，$s \leftarrow \mathbb{Z}_{q}^{n}$，根据 $\chi$ 分布选取 $e \leftarrow[-B_{\chi}, B_{\chi}]^{m}$，计算 $z_{0}:=A^{\top} s+e \in \mathbb{Z}_{q}^{m}$，$z_{1} \leftarrow \mathbb{Z}_{q}^{m}$，均匀选取 $\beta \leftarrow\{0,1\}$。
    - **输入**：$(A, z_{\beta})$
    - **输出**：$\mathrm{output}$
    - **判定性 LWE 问题困难**：任意 PPT 敌手的优势是可忽略的，即
        $$
        \left|\Pr(\mathrm{output}=\beta) - \frac{1}{2}\right| = \mathrm{negl}(\lambda)
        $$
- **定理**：**判定性 LWE 问题困难** $\iff$ **计算性 LWE 问题困难**

