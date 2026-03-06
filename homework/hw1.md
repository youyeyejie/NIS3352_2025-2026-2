# Assignment 1

## Problem 1 (25 marks)
证明 $p = \text{poly}(\lambda)$ 个压倒性（overwhelming）事件的交集仍然是压倒性的。

**证明**：令 $E_i = \overline{F_i}$，则 $E_i$ 是一个可忽略事件。所以
$$
\begin{aligned}
\Pr\left[\bigwedge_{i=1}^p F_i\right] &= 1 - \Pr\left[\bigvee_{i=1}^p E_i\right] \geq 1 - \sum_{i=1}^p \Pr[E_i] \\
&\geq 1 - p \cdot \max_{i} \Pr[E_i] \\
&\geq 1 - p \cdot \text{negl}(\lambda) \\
&= 1 - \text{negl}(\lambda)  \quad(\text{课堂上已证}) \\
&= \text{overwhelming}(\lambda)
\end{aligned}
$$

---

## Problem 2 (25 marks)
证明公钥加密方案中 IND-CPA 安全与 IND-mCPA 安全是等价的。

**证明**：
- **必要性易证**（$\Leftarrow$）：IND-CPA 是 IND-mCPA $Q=1$ 的特殊情况
- **充分性证明**（$\Rightarrow$）：
    1. 设敌手发起 $Q=\mathrm{poly}(\lambda)$ 次挑战，定义：
        - **Game 0** ($b=0$)：对所有 $j=1,\ldots,Q$，加密 $M_{0}^{(j)}$，即 $C^{*(j)} \leftarrow \mathrm{Enc}(PK, M_{0}^{(j)})$
        - **Game 1** ($b=1$)：对所有 $j=1,\ldots,Q$，加密 $M_{1}^{(j)}$，即 $C^{*(j)} \leftarrow \mathrm{Enc}(PK, M_{1}^{(j)})$
    2. 在 Game 0 和 Game 1 之间插入 $Q+1$ 个混合挑战 $H_{i}$，其中 $i=0,\ldots,Q$，$H_{i}$ 定义为对前 $i$ 个挑战加密 $M_{1}^{(j)}$，对后 $Q-i$ 个挑战加密 $M_{0}^{(j)}$，即
        $$
        H_{i} : C^{*(j)} \leftarrow \begin{cases} \mathrm{Enc}(PK, M_{1}^{(j)}) & j \leq i \\ \mathrm{Enc}(PK, M_{0}^{(j)}) & j > i \end{cases}
        $$
    3. 易知 Game 0 等价于 $H_{0}$，Game 1 等价于 $H_{Q}$，由**引理**：若算法满足 IND-CPA 安全，则对 $\forall i \in \{1, \dots, Q\}$，有
        $$
        \left| \Pr(\mathrm{output} = 1 \mid H_{i-1}) - \Pr(\mathrm{output} = 1 \mid H_{i}) \right| = \mathrm{negl}(\lambda)
        $$
    4. 由三角不等式，最终有
        $$
        \begin{aligned}
        \mathrm{Adv}=& \left| \Pr(\mathrm{output} = 1 \mid b = 0) - \Pr(\mathrm{output} = 1 \mid b = 1) \right| \\
        =&\left| \Pr(\mathrm{output} = 1 \mid H_{0}) - \Pr(\mathrm{output} = 1 \mid H_{Q}) \right| \\
        =&\left| \sum_{i=1}^{Q} \Pr(\mathrm{output} = 1 \mid H_{i-1}) - \Pr(\mathrm{output} = 1 \mid H_{i}) \right| \\
        \leq& \sum_{i=1}^{Q} \left| \Pr(\mathrm{output} = 1 \mid H_{i-1}) - \Pr(\mathrm{output} = 1 \mid H_{i}) \right| \\
        =& Q(\lambda) \cdot \mathrm{negl}(\lambda)  \\
        =& \mathrm{negl}(\lambda)
        \end{aligned}
        $$

        即 Game 0 与 Game 1 的差距是可忽略的，算法满足 IND-mCPA 安全性。
- **引理证明**：采用反证法，由区分 $H_{i-1}$ 与 $H_{i}$ 的敌手 $\mathcal{A}$ 来构造攻破 IND-CPA 安全性的敌手 $B$。
    - 假设存在一个概率多项式时间敌手 $\mathcal{A}$，以不可忽略的概率区分 $H_{i-1}$ 与 $H_{i}$，即
        $$
        \mathrm{Adv}_\mathcal{A} = \left| \Pr(\mathrm{output} = 1 \mid H_{i-1}) - \Pr(\mathrm{output} = 1 \mid H_{i}) \right| = \mathrm{non\text{-}negl}(\lambda)
        $$
    - 构造针对 IND-CPA 安全性的敌手 $\mathcal{B}$：
        - $\mathcal{B}$ 将 IND-CPA 实验中的 $PK$ 交给 $\mathcal{A}$, 并模拟 $\mathcal{A}$ 的查询：
            - $\mathcal{B}$ 接收 $\mathcal{A}$ 的前 $i-1$ 对明文 $(M_{0}^{(j)}, M_{1}^{(j)})$，修改为 $(M_{1}^{(j)}, M_{1}^{(j)})$ 进行 IND-CPA 实验，相当于固定加密 $M_{1}^{(j)}$，将密文交给 $\mathcal{A}$
            - $\mathcal{B}$ 接收 $\mathcal{A}$ 的第 $i$ 对明文 $(M_{0}^{(i)}, M_{1}^{(i)})$，将其作为 IND-CPA 实验的挑战明文，接收挑战密文 $C^{*(i)}$ 交给 $\mathcal{A}$
            - $\mathcal{B}$ 接收 $\mathcal{A}$ 的后 $Q-i$ 对明文 $(M_{0}^{(j)}, M_{1}^{(j)})$，同理，加密 $M_{0}^{(j)}$ 交给 $\mathcal{A}$
            - 则对 $\mathcal{A}$ 而言区分 $H_{i-1}$ 与 $H_{i}$ 的实验完全等价于 $\mathcal{B}$ 区分 IND-CPA 实验中的两个场景（$b=0$ 或 $b=1$）
        - $\mathcal{B}$ 的输出 $\mathrm{output}_{\mathcal{B}} = \begin{cases} 0 & \mathrm{output}_{\mathcal{A}}=0 \\ 1 & \mathrm{output}_{\mathcal{A}}=1 \end{cases}$
        - $\mathcal{B}$ 的优势：
            $$
            \begin{aligned}
            \mathrm{Adv}_{\mathcal{B}} = &\left| \Pr(\mathrm{output}_{\mathcal{B}} = b) - \frac{1}{2} \right| \\
            =& \frac{1}{2} \left| \Pr(\mathrm{output}_{\mathcal{B}} = 1 \mid b=0) - \Pr(\mathrm{output}_{\mathcal{B}} = 1 \mid b=1) \right| \\
            =& \frac{1}{2} \left| \Pr(\mathrm{output}_{\mathcal{A}} = 1 \mid H_{i-1}) - \Pr(\mathrm{output}_{\mathcal{A}} = 1 \mid H_{i}) \right| \\
            =& \frac{1}{2} \cdot \mathrm{non\text{-}negl}(\lambda) =\mathrm{non\text{-}negl}(\lambda)
            \end{aligned}
            $$
    - 则 $\mathcal{B}$ 能以不可忽略的优势打破 IND-CPA 安全性，与假设矛盾，因此引理成立。

---

## Problem 3 (25 marks)
证明 Plain RSA 不是 IND-CPA 安全的。

**证明**：构造一个PPT 敌手 $\mathcal{A}$：

- $\mathcal{A}$ 选取两个不同的等长明文 $M_0, M_1 \in \mathbb{Z}_N^*$ 发送给挑战者，挑战者随机选择 $b \in \{0, 1\}$，计算 $C = M_b^e \mod N$ 并返回给 $\mathcal{A}$。$\mathcal{A}$ 收到 $C$ 后，利用公开的公钥 $(N, e)$，自行计算 $C'_0 = M_0^e \mod N$ 和 $C'_1 = M_1^e \mod N$。
- 则 $\mathcal{A}$ 可以采取以下策略：
    $$
    \mathrm{output}_{\mathcal{A}} = \begin{cases} 0 & C = C'_0 \\ 1 & C = C'_1 \end{cases}
    $$
- 则 $\mathcal{A}$ 的优势为
    $$
    \begin{aligned}
    \mathrm{Adv}_{\mathcal{A}}=&\left| \Pr(\mathrm{output}_{\mathcal{A}} = b) - \frac{1}{2} \right| \\
    =&\left| \Pr(\mathrm{output}_{\mathcal{A}} = b \mid b=0) \cdot \Pr(b=0) + \Pr(\mathrm{output}_{\mathcal{A}} = b \mid b=1) \cdot \Pr(b=1) - \frac{1}{2} \right| \\
    =&\left| 1 \cdot \frac{1}{2} + 1 \cdot \frac{1}{2} - \frac{1}{2} \right| = \frac{1}{2} \\
    =& \mathrm{non\text{-}negl}(\lambda)
    \end{aligned}
    $$
- 因此，Plain RSA 不是 IND-CPA 安全的。
---

### Problem 4 (25 marks)
一个变体 ElGamal 加密，加密位 $b=0$ 时使用 $h^y$，加密位 $b=1$ 时使用随机的 $g^z$。

1. 如何利用私钥 $x$ 高效解密？

    已知私钥 $x$（满足 $h = g^x$）和密文 $(c_1, c_2)$：
    - **解密步骤：** 接收者计算 $V = c_1^x \pmod q$。
    - **判断逻辑：**
        - 检查是否 $V = c_2$。
        - 如果 $c_1^x = c_2$，说明密文形式是 $(g^y, (g^x)^y)$，即 $b=0$。
        - 如果 $c_1^x \neq c_2$，说明 $c_2$ 是一个独立的随机值 $g^z$（以极高概率不等于 $g^{xy}$），即 $b=1$。
    - **效率：** 仅需一次模幂运算，非常高效。

2. 证明若 DDH 困难则方案是 CPA 安全的

    **证明（归约法）：**
    1.  **假设：** 存在一个 PPT 敌手 $\mathcal{A}$ 可以以不可忽略的优势打破该方案的 CPA 安全性。
    2.  **构造 DDH 算法 $\mathcal{B}$：** $\mathcal{B}$ 收到一个 DDH 三元组 $(g^a, g^b, T)$，目标是判断 $T = g^{ab}$ 还是 $T = g^z$（随机值）。
        *   $\mathcal{B}$ 将公钥设为 $g, h=g^a, q$（这里 $a$ 相当于私钥 $x$）。
        *   $\mathcal{B}$ 将挑战密文设为 $c_1 = g^b, c_2 = T$。
        *   $\mathcal{B}$ 将 $(pk, (c_1, c_2))$ 发给 $\mathcal{A}$。
        *   如果 $T = g^{ab}$，则这完全符合 $b=0$ 时的加密分布（其中 $y=b$）。
        *   如果 $T = g^z$，则这完全符合 $b=1$ 时的加密分布。
    3.  **结论：** 
        *   $\mathcal{A}$ 区分 $b=0$ 和 $b=1$ 的优势，直接等于 $\mathcal{B}$ 区分 DDH 元组和随机元组的优势。
        *   如果 DDH 问题是困难的（即区分优势可忽略），那么该加密方案也一定是 CPA 安全的。