# Ch1 密码学中的可证明安全理论
## 公钥加密算法的安全性定义以及 ElGamal 加密算法

### 可证明安全理论
#### 基本概念
- **安全参数**（Security Parameter）$\lambda$：刻画攻击者攻破密码方案的难度
    - **计算安全参数**（底层困难问题的计算复杂度，依赖于算力和算法）：攻破该问题至少需要 $2^{\lambda}$ 步运算。
        - 例如：分解大整数 $n \approx 2^{2\lambda}$ 的计算复杂度为 $2^{\lambda}$，则计算安全参数为 $\lambda$
    - **统计安全参数**（依赖于不同分布之间的统计距离，信息论安全）：攻破密码算法的概率：区分猜测值和目标值之间的概率是 $2^{-\lambda}$，则统计安全参数为 $\lambda$。
- **$\lambda$ 的多项式大小**：$\mathrm{poly}(\lambda) = O(\lambda^{c})$，即 $\exists c \in \mathbb{N},\lambda' \in \mathbb{N}$，使得对 $\forall \lambda \geq \lambda'$，有 $$\mathrm{poly}(\lambda) \leq \lambda^{c}$$
	- 例如：$\mathrm{poly}(\lambda) = 4\lambda^{10} + \lambda^{3} + 1$，$\lambda$，$\lambda \log \lambda$，$\lambda^{1.5}$，$\lambda^{2}$ 等。
- **多项式时间算法**：算法 $\mathrm{ALG}(x)$ 的运行时间与输出比特长度是输入比特长度 $\log |x|$ 的多项式大小。
	- 确定性多项式时间（Deterministic Polynomial Time, DPT）算法
	- 概率性多项式时间（Probabilistic Polynomial Time, PPT）算法
- **可忽略概率**（Negligible probability）：**小于任意多项式倒数的概率，在密码学中认为几乎不可能发生的概率**，通常记为 $\mathrm{negl}(\lambda)$，即对任意多项式 $\mathrm{poly}$，$\exists \lambda' \in \mathbb{N}$，对 $\forall \lambda \geq \lambda'$，有
	$$
	\mathrm{negl}(\lambda) \leq \frac{1}{\mathrm{poly}(\lambda)}
	$$
	- 性质：$\forall c \in \mathbb{N}$，$\exists \lambda' \in \mathbb{N}$，使得对 $\forall \lambda \geq \lambda'$，有 $\mathrm{negl}(\lambda) \leq \lambda^{-c}$
	- 例：$\mathrm{negl}(\lambda) = 2^{-\lambda}$，$2^{-\sqrt{\lambda}}$，$\lambda^{-\lambda} = 2^{-\lambda \log \lambda}$
- **不可忽略概率**（Non-negligible probability）：$\text{non-negl}(\lambda)$，即存在多项式 $\mathrm{poly}$，$\forall \lambda' \in \mathbb{N}$，$\exists \lambda \geq \lambda'$，有
	$$
	\text{non-negl}(\lambda) > 1/\mathrm{poly}(\lambda)
	$$
- **压倒性概率**（Overwhelming probability）：**在密码学中认为几乎一定发生的概率**
	$$
	\mathrm{overwhelm}(\lambda) = 1 - \mathrm{negl}(\lambda)
	$$
	- 例：$\mathrm{overwhelm}(\lambda) = 1 - 2^{-\lambda}$

#### 基本工具
- **布尔不等式**：设 $E_{1}, \dots, E_{p}$ 为 $p$ 个事件（它们之间可以任意相关），则
	$$
	\Pr(E_{1} \vee \dots \vee E_{p}) \leq \Pr(E_{1}) + \dots + \Pr(E_{p})
	$$
	- 设 $E_{1}, \dots, E_{p}$ 为 $p$ 个事件。如果 $p = \mathrm{poly}_{0}(\lambda)$ 且 $\Pr(E_{i}) = \mathrm{negl}_{i}(\lambda)$，则
		$$
		\Pr(E_{1} \vee \cdots \vee E_{p}) = \mathrm{negl}(\lambda)
		$$

		即一个可忽略概率发生的事件重复做多项式次，至少发生一次的概率是可忽略的。
	- 设 $F_{1}, \dots, F_{p}$ 为 $p$ 个事件。如果 $p = \mathrm{poly}_{0}(\lambda)$ 且 $\Pr(F_{i}) = \mathrm{overwhelm}_{i}(\lambda)$，则
		$$
		\Pr(F_{1} \wedge \dots \wedge F_{p}) = \mathrm{overwhelm}(\lambda)
		$$

		即一个压倒性概率发生的事件重复做多项式次，每次都发生的概率是压倒性的。
- **全概率公式**：若事件 $A_{1}, A_{2}, \dots, A_{n}$ 构成一个完备事件组，即它们两两互不相容其和为全集，且都有正概率，则对任意一个事件 $B$，有如下公式成立，称为全概率公式：
	$$
	\Pr(B) = \Pr(B \wedge A_{1}) + \Pr(B \wedge A_{2}) + \cdots + \Pr(B \wedge A_{n}) = \Pr(B\mid A_{1}) \Pr(A_{1}) + \Pr(B\mid A_{2}) \Pr(A_{2}) + \cdots + \Pr(B\mid A_{n}) \Pr(A_{n})
	$$
	- 特别地，对于任意两随机事件 $A$ 和 $B$，有如下成立：
		$$
		\Pr(B) = \Pr(B\mid A) \Pr(A) + \Pr(B\mid \neg A) \Pr(\neg A)
		$$
- **贝叶斯公式**：设 $A$、$B$ 为两个事件，则
	$$
	\Pr(A \wedge B) = \Pr(A\mid B) \Pr(B)
	$$

#### 可证明安全性的定义
- **刻画攻击者/敌手 (attacker/adversary) 的攻击能力**
    - 敌手的输入（例如：公开信息）
    - 敌手的攻击能力（一般运行时间为多项式时间 PT，不考虑拥有无条件计算能力的敌手）
    - 敌手的攻击方式（影响方案执行的行为，主动攻击、被动攻击，通过安全模型的方式刻画）
- **刻画安全目标**
    - 希望达到的目标，一般通过刻画不希望被攻破的目标定义
- **密码算法/协议 $\Pi$ 的 X-Y 安全定义**（template）：任意概率多项式时间敌手在 Y 安全模型中攻破密码算法/协议 $\Pi$ 的 X 安全目标的概率是可忽略的。

### 公钥加密算法的安全性
#### 公钥加密算法的语义及正确性要求
- **语义**：一个公钥加密算法（public-key encryption, PKE）包含三个概率多项式时间（PPT）算法 $(\mathrm{Gen}, \mathrm{Enc}, \mathrm{Dec})$：
	1. **密钥生成算法** $(PK, SK) \leftarrow \mathrm{Gen}(1^{\lambda})$：一般为概率性算法
	2. **加密算法** $C \leftarrow \mathrm{Enc}(PK, M)$：$M \in \mathcal{M}$，其中 $\mathcal{M}$ 为消息空间
	3. **解密算法** $M' \leftarrow \mathrm{Dec}(SK, C)$：一般为确定性算法，$M' \in \mathcal{M} \cup \{\bot\}$，其中 $\bot$ 代表解密失败
- **正确性要求**：对于 $\forall (PK, SK) \leftarrow \mathrm{Gen}(1^{\lambda})$，$\forall M \in \mathcal{M}$，$\forall C \leftarrow \mathrm{Enc}(PK, M)$，一定有
	$$
	\mathrm{Dec}(SK, C) = M
	$$

#### 公钥加密算法的 SKH/PH/PFbH-PA 安全性
- **被动攻击**（Passive Attacks, PA）安全模型：
    ```mermaid
    sequenceDiagram
    participant 挑战者
    participant 敌手

    note left of 挑战者: (PK, SK) ← Gen
    note left of 挑战者: C ← Enc(PK, M)
    挑战者->>敌手: (PK, C)
    敌手->>挑战者: output
    ```
- **敌手攻击能力**
    - 输入：公开信道中的 $PK, C$
    - 运行时间：概率多项式时间 PPT
    - 攻击方式：被动攻击 PA —— 仅仅从公开信道中获取信息，不进行其他攻击
- **安全目标（刻画不希望该密码算法被攻破目标）**
    1. **隐藏私钥**（SK-hiding, SK）：$\mathrm{output} = SK$ 则攻破该目标
    2. **隐藏明文**（Plaintext-hiding, PH）：$\mathrm{output} = M$ 则攻破该目标
    3. **隐藏明文首比特**（Plaintext-first-bit-hiding, PFbH）：$\mathrm{output} = M[1]$ 则攻破该目标
- **公钥加密算法的 SKH/PH/PFbH-PA 安全性定义**：任意概率多项式时间敌手在 PA 安全模型中攻破 SKH/PH/PFbH 安全目标的概率是可忽略的，即
    $$
    \Pr(\mathrm{output} = SK/M/M[1]) = \mathrm{negl}(\lambda)
    $$
    - 说明：SKH/PH/PFbH-PA 安全性定义**不能**保证加密算法的安全性。

#### 公钥加密算法的 IND-CPA 安全性
- **选择明文攻击**（Chosen-Plaintext Attacks, CPA）安全模型：
    ```mermaid
    sequenceDiagram
        participant 挑战者
        participant 敌手

        note left of 挑战者: (PK, SK) ← Gen
        挑战者->>敌手: PK

        敌手->>挑战者: (M₀, M₁)
        note left of 挑战者: b ← {0, 1}<br/>C* ← Enc(PK, M_b)
        挑战者->>敌手: C*

        敌手->>挑战者: output
    ```
- **敌手攻击能力**：
    - 输入：公开信道中的 $PK$ 及挑战密文 $C^{*}$
	- 运行时间：概率多项式时间 PPT
	- 攻击方式：选择明文攻击 CPA ——敌手可以提供/决定/影响加密使用的明文
- **IND 安全目标**：**不可区分性**（Indistinguishability）——敌手无法区分密文 $C^{*}$ 加密的是 $M_{0}$ 还是 $M_{1}$（即如果 $\mathrm{output} = b$，则攻破该目标）
- **公钥加密算法的 IND-CPA 安全性定义**：任意概率多项式时间敌手在 CPA 安全模型中攻破 IND 安全目标的**优势**（advantage）是可忽略的，也即
    $$
    \mathrm{Adv} = \left| \Pr(\mathrm{output} = b) - \frac{1}{2} \right| = \mathrm{negl}(\lambda)
    $$
    - **等价定义**：
        $$
        \begin{aligned}
        \mathrm{Adv} =
        &\left| \Pr(\mathrm{output} = b) - \frac{1}{2} \right| \\
        =& \left| \Pr(\mathrm{output} = 0 \mid b = 0) \cdot \Pr(b=0) + \Pr(\mathrm{output} = 1 \mid b = 1) \cdot \Pr(b=1) - \frac{1}{2} \right| \\
        =& \frac{1}{2} \left| \Pr(\mathrm{output} = 0 \mid b = 0) + \Pr(\mathrm{output} = 1 \mid b = 1) - 1 \right| \\
        =& \frac{1}{2} \left| \Pr(\mathrm{output} = 0 \mid b = 0) - \Pr(\mathrm{output} = 0 \mid b = 1) \right| \\
        =& \frac{1}{2} \left| \Pr(\mathrm{output} = 1 \mid b = 0) - \Pr(\mathrm{output} = 1 \mid b = 1) \right|
        \end{aligned}
        $$
- **IND-CPA 安全性的合理性**：公钥加密算法的 **IND-CPA 安全性** $\Rightarrow$ **SKH-PA/PH-PA/PFbH-PA 安全性**
    - **证明**：以 SKH-PA 安全性为例，采用反证法（**安全性归约**）
        - **假设结论错误**：算法不是 SKH-PA 安全的，即存在一个概率多项式敌手 $\mathcal{A}$，以不可忽略的概率在 PA 安全模型中攻破 SKH 安全目标，即
            $$
            \mathrm{Adv}_\mathcal{A} = \Pr[\mathcal{A}(PK, C) = SK] = \text{non-negl}(\lambda)
            $$
        - **证明前提错误**：构造一个概率多项式时间敌手 $\mathcal{B}$，在 CPA 安全模型中攻破 IND 安全目标。
            - $\mathcal{B}$ 的输入：公开信道中的 $PK$ 及挑战密文 $C^{*}$
            - 调用子敌手：$\mathcal{B}$ 将 $(PK, C^{*})$ 交给 $\mathcal{A}$，模拟 SKH-PA 实验，$\mathcal{A}$ 输出一个候选私钥 $SK'$。$\mathcal{B}$ 尝试使用 $SK'$ 解密 $C^{*}$，得到 $M' = \mathrm{Dec}(SK', C^{*})$
                ```mermaid
                sequenceDiagram
                    participant 挑战者
                    participant 敌手B
                    participant 敌手A

                    note left of 挑战者: (PK, SK) ← Gen
                    挑战者->>敌手B: PK
                    敌手B->>挑战者: (M₀, M₁)
                    note left of 挑战者: b ← {0, 1}<br/>C* ← Enc(PK, M_b)
                    挑战者->>敌手B: C*
                    敌手B->>敌手A: (PK, C*)
                    note right of 敌手A: SK' ← A(PK, C*)
                    敌手A->>敌手B: SK'
                    note right of 敌手B: M' ← Dec(SK', C*)
                    敌手B->>挑战者: output
                ```
            - $\mathcal{B}$ 的输出：$\mathrm{output} = \begin{cases} 0 & M' = M_0 \\ 1 & M' = M_1 \\ \mathrm{random}\{0, 1\} & \text{otherwise} \end{cases}$
            - $\mathcal{B}$ 的优势：
                $$
                \begin{aligned}
                \mathrm{Adv}_\mathcal{B} = &\left| \Pr(\mathrm{output} = b) - \frac{1}{2} \right| \\
                =& \left| \Pr(\mathrm{output} = b \mid SK' = SK) \cdot \Pr(SK' = SK) + \Pr(\mathrm{output} = b \mid SK' \neq SK) \cdot \Pr(SK' \neq SK) - \frac{1}{2} \right| \\
                =& \left| 1 \cdot \text{non-negl}(\lambda) + \frac{1}{2} \cdot (1 - \text{non-negl}(\lambda)) - \frac{1}{2} \right| \\
                =& \frac{1}{2} \cdot \text{non-negl}(\lambda) = \text{non-negl}(\lambda)
                \end{aligned}
                $$

                即 $\mathcal{B}$ 在 CPA 安全模型中攻破 IND 安全目标的优势是不可忽略的，算法不是 IND-CPA 安全的，与前提矛盾。
    - **结论**：IND-CPA 是公钥加密算法的基本安全性要求。

#### 公钥加密算法的 IND-mCPA 安全性
- **多挑战-选择明文攻击**（multiple-challenge Chosen-Plaintext Attacks, mCPA）
    ```mermaid
    sequenceDiagram
    participant 挑战者
    participant 敌手

    note left of 挑战者: (PK, SK) ← Gen
    note left of 挑战者: b ← {0, 1}
    挑战者->>敌手: PK

    note right of 敌手: 可进行多项式 Q 次挑战，j ∈ {1, ..., Q}
    loop 第 j 次明文攻击
        敌手->>挑战者: (M₀⁽ʲ⁾, M₁⁽ʲ⁾)
        note left of 挑战者: C⁽ʲ⁾* ← Enc(PK, M_b⁽ʲ⁾)
        挑战者->>敌手: C⁽ʲ⁾*
    end

    %% 最终输出
    敌手->>挑战者: output
    ```
- **敌手的攻击能力**
	- 输入：公开信道中的 $PK$ 及**多项式**个挑战密文 $C^{*(1)}, \dots, C^{*(Q)}$
	- 运行时间：概率多项式时间 PPT
	- 攻击方式：多挑战-选择明文攻击 mCPA ——敌手可以提供/决定/影响加密使用的多个明文
- **IND 安全目标**：敌手无法区分密文 $C^{*(j)}$ 加密的是 $M_{0}^{(j)}$ 还是 $M_{1}^{(j)}$（即如果 $\mathrm{output} = b$，则攻破该目标）
- **公钥加密算法的 IND-mCPA 安全性定义**：任意概率多项式时间敌手在 mCPA 安全模型中攻破 IND 安全目标的优势是可忽略的，即
    $$
    \mathrm{Adv} = \left| \Pr(\mathrm{output} = b) - \frac{1}{2} \right| = \mathrm{negl}(\lambda)
    $$
    - **等价定义**：
    $$
    \begin{aligned}
    \mathrm{Adv} =
    &\frac{1}{2} \left| \Pr(\mathrm{output} = 1 \mid b = 0) - \Pr(\mathrm{output} = 1 \mid b = 1) \right| \\
    =& \frac{1}{2} \left| \Pr(\mathrm{output} = 0 \mid b = 0) - \Pr(\mathrm{output} = 0 \mid b = 1) \right|
    \end{aligned}
    $$
- **IND-CPA 与 IND-mCPA 的等价性**：公钥加密算法的 **IND-CPA 安全性** $\Leftrightarrow$ **IND-mCPA 安全性**
    - **必要性易证**（$\Leftarrow$）：IND-CPA 是 IND-mCPA $Q=1$ 的特殊情况
    - **充分性证明**（$\Rightarrow$）：采用混合论证（Hybrid Arguments）与三角不等式，核心思路是在全加密 $M_0^{(j)}$ 和全加密 $M_1^{(j)}$ 两个极端场景之间，插入一系列混合场景（Hybrid），证明相邻混合场景的不可区分性，最终推导出两个极端场景的不可区分性。
        1. **定义两个极端游戏**（Game）：设敌手发起 $Q=\mathrm{poly}(\lambda)$ 次挑战，定义两个基础游戏：
            - **Game 0** ($b=0$)：对所有 $j=1,\ldots,Q$，加密 $M_{0}^{(j)}$，即 $C^{*(j)} \leftarrow \mathrm{Enc}(PK, M_{0}^{(j)})$
            - **Game 1** ($b=1$)：对所有 $j=1,\ldots,Q$，加密 $M_{1}^{(j)}$，即 $C^{*(j)} \leftarrow \mathrm{Enc}(PK, M_{1}^{(j)})$
        2. **定义混合游戏**（Hybrid Game）：在 Game 0 和 Game 1 之间插入 $Q+1$ 个混合游戏 $\mathrm{Hybrid}_{i}$，其中 $i=0,\ldots,Q$，$\mathrm{Hybrid}_{i}$ 定义为对前 $i$ 个挑战加密 $M_{1}^{(j)}$，对后 $Q-i$ 个挑战加密 $M_{0}^{(j)}$，即
            $$
            \mathrm{Hybrid}_{i} : C^{*(j)} \leftarrow \begin{cases} \mathrm{Enc}(PK, M_{1}^{(j)}) & j \leq i \\ \mathrm{Enc}(PK, M_{0}^{(j)}) & j > i \end{cases}
            $$
        3. **分析混合游戏**：易知 Game 0 等价于 $\mathrm{Hybrid}_{0}$，Game 1 等价于 $\mathrm{Hybrid}_{Q}$，由**引理**：若算法满足 IND-CPA 安全，则对 $\forall i \in \{1, \dots, Q\}$，有
            $$
            \left| \Pr(\mathrm{output} = 1 \mid \mathrm{Hybrid}_{i-1}) - \Pr(\mathrm{output} = 1 \mid \mathrm{Hybrid}_{i}) \right| = \mathrm{negl}(\lambda)
            $$
        4. 累加相邻场景的差距：由三角不等式，最终有
            $$
            \begin{aligned}
            \mathrm{Adv} =& \left| \Pr(\mathrm{output} = 1 \mid b = 0) - \Pr(\mathrm{output} = 1 \mid b = 1) \right| \\
            =&\left| \Pr(\mathrm{output} = 1 \mid \mathrm{Hybrid}_{0}) - \Pr(\mathrm{output} = 1 \mid \mathrm{Hybrid}_{Q}) \right| \\
            =&\left| \sum_{i=1}^{Q} \Pr(\mathrm{output} = 1 \mid \mathrm{Hybrid}_{i-1}) - \Pr(\mathrm{output} = 1 \mid \mathrm{Hybrid}_{i}) \right| \\
            \leq& \sum_{i=1}^{Q} \left| \Pr(\mathrm{output} = 1 \mid \mathrm{Hybrid}_{i-1}) - \Pr(\mathrm{output} = 1 \mid \mathrm{Hybrid}_{i}) \right| \\
            =& Q(\lambda) \cdot \mathrm{negl}(\lambda)  \\
            =& \mathrm{negl}(\lambda)
            \end{aligned}
            $$

            即 Game 0 与 Game 1 的差距是可忽略的，算法满足 IND-mCPA 安全性。
    - **引理证明**：采用反证法，由区分 $\mathrm{Hybrid}_{i-1}$ 与 $\mathrm{Hybrid}_{i}$ 的敌手 $\mathcal{A}$ 来构造攻破 IND-CPA 安全性的敌手 $\mathcal{B}$。
        - $\mathrm{Hybrid}_{i-1}$ 与 $\mathrm{Hybrid}_{i}$ 的差别只在于第 $i$ 个挑战密文，与 IND-CPA 刻画的安全性十分接近。
        - 假设存在一个概率多项式时间敌手 $\mathcal{A}$，以不可忽略的概率区分 $\mathrm{Hybrid}_{i-1}$ 与 $\mathrm{Hybrid}_{i}$，即
            $$
            \mathrm{Adv}_\mathcal{A} = \left| \Pr(\mathrm{output}_\mathcal{A} = 1 \mid \mathrm{Hybrid}_{i-1}) - \Pr(\mathrm{output}_\mathcal{A} = 1 \mid \mathrm{Hybrid}_{i}) \right| = \text{non-negl}(\lambda)
            $$
        - 构造针对 IND-CPA 安全性的敌手 $\mathcal{B}$：
            - $\mathcal{B}$ 将 IND-CPA 实验中的 $PK$ 交给 $\mathcal{A}$
                - $\mathcal{B}$ 接收 $\mathcal{A}$ 的前 $i-1$ 对明文 $(M_{0}^{(j)}, M_{1}^{(j)})$，修改为 $(M_{1}^{(j)}, M_{1}^{(j)})$ 进行 IND-CPA 实验，相当于固定加密 $M_{1}^{(j)}$，将密文交给 $\mathcal{A}$
                - $\mathcal{B}$ 接收 $\mathcal{A}$ 的第 $i$ 对明文 $(M_{0}^{(i)}, M_{1}^{(i)})$，将其作为 IND-CPA 实验的挑战明文，接收挑战密文 $C^{*(i)}$ 交给 $\mathcal{A}$
                - $\mathcal{B}$ 接收 $\mathcal{A}$ 的后 $Q-i$ 对明文 $(M_{0}^{(j)}, M_{1}^{(j)})$，同理，加密 $M_{0}^{(j)}$ 交给 $\mathcal{A}$
            - 则 $\mathcal{B}$ 的输出 $\mathrm{output}_{\mathcal{B}} = \begin{cases} 0 & \mathrm{output}_{\mathcal{A}}=0 \\ 1 & \mathrm{output}_{\mathcal{A}}=1 \end{cases}$
            - $\mathcal{B}$ 的优势：
                $$
                \begin{aligned}
                \mathrm{Adv}_\mathcal{B} = &\left| \Pr(\mathrm{output}_{\mathcal{B}} = b) - \frac{1}{2} \right| \\
                =& \frac{1}{2} \left| \Pr(\mathrm{output}_{\mathcal{B}} = 1 \mid b=0) - \Pr(\mathrm{output}_{\mathcal{B}} = 1 \mid b=1) \right| \\
                =& \frac{1}{2} \left| \Pr(\mathrm{output}_{\mathcal{A}} = 1 \mid \mathrm{Hybrid}_{i-1}) - \Pr(\mathrm{output}_{\mathcal{A}} = 1 \mid \mathrm{Hybrid}_{i}) \right| \\
                =& \frac{1}{2} \cdot \text{non-negl}(\lambda) =\text{non-negl}(\lambda)
                \end{aligned}
                $$
        - 则 $\mathcal{B}$ 能以不可忽略的优势打破 IND-CPA 安全性，与假设矛盾，因此引理成立。

### ElGamal 加密算法
#### 群的基本知识
- 设 $(G,\cdot,1)$ 为**有限交换群**，其中 $1$ 为 $G$ 中关于运算 $\cdot$ 的单位元，则群 $G$ 满足以下性质：
    - 有限性：$|G| < \infty$
    - 封闭性：$\forall a, b \in G$，$a \cdot b \in G$
    - 交换性：$\forall a, b \in G$，$a \cdot b = b \cdot a$
    - 单位元：$\forall a \in G$，$a \cdot 1 = 1 \cdot a = a$
    - 逆元：$\forall a \in G$，存在 $a^{-1} \in G$，使得 $a \cdot a^{-1} = a^{-1} \cdot a = 1$
- **生成元**（Generator）：$a \in G$，集合 $\langle a \rangle = \{a^{i} : i \in \mathbb{Z}\}$ 是 $G$ 的子群，称为循环子群（Cyclic Subgroup），称 $a$ 为循环群 $\langle a \rangle$ 的生成元。
    - 特别地，如果 $\langle g \rangle = G$，则称 $g$ 是 $G$ 的生成元，$G$ 是由 $g$ 生成的循环群。
    - **群的阶**：$|\langle a \rangle| = \mathrm{ord}(a) = \min\{n \in \mathbb{N} : a^{n} = 1\}$，即 $a$ 的阶。
        - 拉格朗日定理：$\mathrm{ord}(a) \mid |G|$
    - **素数阶群**：如果 $G$ 的阶 $|G|$ 是素数，则 $G$ 中的任意非单位元都是生成元，且 $G$ 为循环群。 
    - 如果群 $G=\langle g \rangle$ 为循环群，则 $G$ 中元素的运算为 $\log|G|$ 的多项式时间。
- **离散对数**（Discrete Logarithm, DLOG）：对于 $a \in G$ 和 $b = a^{x} \in \langle a \rangle$，则称 $x\in\{0,1,\dots,\mathrm{ord}(a)-1\}$ 为 $b$ 相对于 $a$ 的离散对数，记为 $\mathrm{DLOG}_{G,a}(b)$。
    - DLOG 问题的困难性与群的运算有关。
    - 公认 DLOG 计算困难的群：
        - **有限域的子群**：$G$ 为模 $n$ 乘法群 $(\mathbb{Z}_n^*, \cdot)$ 的 $p$ 阶循环子群，其中 $n = 2p + 1$，且 $n$ 和 $p$ 均为素数。
            - 有限域上的离散对数问题（FFDLP）在数域筛法 NFS 下的求解复杂度为 $O\left(2^{\log^{\frac{1}{3}} p \cdot \log \log^{\frac{2}{3}} p}\right)$，为亚指数级别。
        - **椭圆曲线群**：$G=(E(\mathbb{Z}_p), +)$，其中 $E(\mathbb{Z}_p)$ 是定义在有限域 $\mathbb{Z}_p$ 上的椭圆曲线 $E$ 的点集，运算为点加法。
            - 椭圆曲线上的离散对数问题（ECDLP）在 Pollard's rho 算法下的求解复杂度为 $O(\sqrt{p})=O(2^{\frac{1}{2} \log p})$，为指数级别。

#### 底层困难问题
- **离散对数问题**（Discrete Logarithm, DLOG）：$G$ 为循环群，其阶为素数 $p$、生成元为 $g$，并均匀选取 $h \leftarrow G$
    - 输入：$(G, p, g, h)$
    - 输出：$d = \mathrm{DLOG}_g\, h$
- **计算性 Diffie-Hellman 问题**（Computational Diffie-Hellman, CDH）：$G$ 为循环群，其阶为素数 $p$、生成元为 $g$，均匀选取 $x, y \leftarrow \mathbb{Z}_p$
    - 输入：$(G, p, g, g^x, g^y)$
    - 输出：$g^{xy}$，也称为 $(g^x, g^y)$ 的 CDH 值
- **判定性 Diffie-Hellman 问题**（Decisional Diffie-Hellman, DDH）：$G$ 为循环群，其阶为素数 $p$、生成元为 $g$，均匀选取 $x, y, z \leftarrow \mathbb{Z}_p, \beta \leftarrow \{0, 1\}$，并定义 $z_\beta = \begin{cases} g^{xy} & \beta=0 \\ g^{z} & \beta=1 \end{cases}$
    - 输入：$(G, p, g, g^x, g^y, z_\beta)$
    - 输出：$\beta$
    - DDH 问题的困难性：任意 PPT 敌手在 DDH 安全模型中攻破 DDH 安全目标的优势是可忽略的，即
        $$
        \mathrm{Adv} = \left| \Pr(\mathrm{output} = \beta) - \frac{1}{2} \right| = \mathrm{negl}(\lambda)
        $$
- **定理**：**DDH 问题困难** $\Rightarrow$ **CDH 问题困难** $\Rightarrow$ **DLOG 问题困难**

#### ElGamal 算法简介
- **密钥生成算法** $(PK, SK) \leftarrow \mathrm{Gen}(1^\lambda)$：
    1. 选择循环群 $G$，其阶为素数 $p$、生成元为 $g$
    2. 均匀选取 $s \leftarrow \mathbb{Z}_p$，计算 $h := g^s$
    3. 输出 $PK = (G, p, g, h)$，$SK = s$
- **加密算法** $C \leftarrow \mathrm{Enc}(PK, M)$：消息空间为 $\mathcal{M} = G$
    1. 均匀选取 $r \leftarrow \mathbb{Z}_p$
    2. 计算 $C_1 := g^r$
    3. 计算 $C_2 := h^r \cdot M$
    4. 输出 $C := (C_1, C_2)$
- **解密算法** $M' \leftarrow \mathrm{Dec}(SK, C = (C_1, C_2))$：
    1. 计算并输出 $M' := C_2 \cdot (C_1^s)^{-1}$

#### ElGamal 加密算法的 IND-CPA 安全性
- **定理**：**DDH 问题困难** $\Leftrightarrow$ **ElGamal 算法是 IND-CPA 安全的**
- **证明** $\Rightarrow$：反证法（安全性归约）
    - 假设结论错误：ElGamal 算法不是 IND-CPA 安全的，即存在一个概率多项式时间敌手 $\mathcal{A}$，以不可忽略的概率在 ElGamal 算法 CPA 安全模型中攻破 IND 安全目标，即
        $$
        \mathrm{Adv}_\mathcal{A} = \left| \Pr(\mathrm{output}_\mathcal{A} = b) - \frac{1}{2} \right| = \text{non-negl}(\lambda)
        $$
    - 证明前提错误：构造一个概率多项式时间敌手 $\mathcal{B}$，在 DDH 安全模型中攻破 DDH 安全目标。
        - $\mathcal{B}$ 的输入：$(G, p, g, g^x, g^y, z_\beta)$
        - 调用子敌手：$\mathcal{B}$ 将 $PK = (G, p, g, h=g^x)$ 交给 $\mathcal{A}$，$\mathcal{A}$ 输入两个消息 $M_0, M_1 \in G$，$\mathcal{B}$ 将挑战密文设置为 $C^{*} = (g^y, z_\beta \cdot M_b)$ 交给 $\mathcal{A}$，则
            - $\beta = 0$ 时：$C^{*} = (g^y, M_b \cdot g^{xy})$，与 ElGamal 加密 $M_b$ 的密文分布相同，则
                $$
                \left|\Pr(\mathrm{output}_\mathcal{A}=b\mid \beta=0) - \frac{1}{2}\right| = \left| \Pr(\mathrm{output}_\mathcal{A} = b) - \frac{1}{2} \right| = \text{non-negl}(\lambda)
                $$

                也即 $\Pr(\mathrm{output}_\mathcal{A}=b \mid \beta=0) = \frac{1}{2} \pm \text{non-negl}(\lambda)$
            - $\beta = 1$ 时：$C^{*} = (g^y, M_b \cdot g^{z})$，由于 $z \leftarrow \mathbb{Z}_p$ 均匀分布，$M_b \cdot g^{z}$ 也均匀分布在 $G$ 中，则密文不包含任何关于 $b$ 的信息，$\Pr(\mathrm{output}_\mathcal{A}=b \mid \beta=1) = \frac{1}{2}$
        - $\mathcal{B}$ 的输出：$\mathrm{output}_\mathcal{B} = \begin{cases} 0 & \mathrm{output}_\mathcal{A} = b \\ 1 & \mathrm{output}_\mathcal{A} \neq b \end{cases}$
        - $\mathcal{B}$ 的优势：
            $$
            \begin{aligned}
            \mathrm{Adv}_\mathcal{B} =& \left| \Pr(\mathrm{output}_\mathcal{B} = \beta) - \frac{1}{2} \right| \\
            =& \frac{1}{2} \left| \Pr(\mathrm{output}_\mathcal{B} = 0 \mid \beta = 0) - \Pr(\mathrm{output}_\mathcal{B} = 0 \mid \beta = 1) \right| \\
            =& \frac{1}{2} \left| \Pr(\mathrm{output}_\mathcal{A} = b \mid \beta = 0) - \Pr(\mathrm{output}_\mathcal{A} = b \mid \beta = 1) \right| \\
            =& \frac{1}{2} \left| \left( \frac{1}{2} \pm \text{non-negl}(\lambda) \right) - \frac{1}{2} \right| \\
            =& \frac{1}{2} \cdot \text{non-negl}(\lambda) = \text{non-negl}(\lambda)
            \end{aligned}
            $$
- **证明** $\Leftarrow$：反证法（安全性归约）
    - 假设结论错误：DDH 问题不困难，即存在一个概率多项式时间敌手 $\mathcal{B}$，以不可忽略的概率在 DDH 安全模型中攻破 DDH 安全目标，即
        $$
        \mathrm{Adv}_\mathcal{B} = \left| \Pr(\mathrm{output}_\mathcal{B} = \beta) - \frac{1}{2} \right| = \text{non-negl}(\lambda)
        $$
    - 证明前提错误：构造一个概率多项式时间敌手 $\mathcal{A}$，在 ElGamal 算法 CPA 安全模型中攻破 IND 安全目标。
        - $\mathcal{A}$ 的输入：$PK = (G, p, g, h=g^s)$ 和 $C^* = (C_1, C_2) = (g^r, M_b\cdot h^r)$
        - 调用子敌手：$\mathcal{A}$ 将 $(G, p, g, g^s, g^r, C_2/M_0)$ 交给 $\mathcal{B}$，
            - $b = 0$ 时：$C_2 = M_0 \cdot g^{sr}$，则 $C_2/M_0 = g^{sr}$，与 DDH 问题中取 $\beta = 0$ 等价
            - $b = 1$ 时：$C_2 = M_1 \cdot g^{sr}$，则 $C_2/M_0 = M_1/M_0 \cdot g^{sr}$，由于 $M_0, M_1$ 均匀分布在 $G$ 中，则 $C_2/M_0$ 也均匀分布在 $G$ 中，与 DDH 问题中取 $\beta = 1$ 等价
        - $\mathcal{A}$ 的输出：$\mathrm{output}_\mathcal{A} = \begin{cases} 0 & \mathrm{output}_\mathcal{B} = 0 \\ 1 & \mathrm{output}_\mathcal{B} = 1 \end{cases}$
        - $\mathcal{A}$ 的优势：
            $$
            \begin{aligned}
            \mathrm{Adv}_\mathcal{A} =& \left| \Pr(\mathrm{output}_\mathcal{A} = b) - \frac{1}{2} \right| \\
            =& \frac{1}{2} \left| \Pr(\mathrm{output}_\mathcal{A} = 0 \mid b = 0) - \Pr(\mathrm{output}_\mathcal{A} = 0 \mid b = 1) \right| \\
            =& \frac{1}{2} \left| \Pr(\mathrm{output}_\mathcal{B} = 0 \mid b = 0) - \Pr(\mathrm{output}_\mathcal{B} = 0 \mid b = 1) \right| \\
            =& \frac{1}{2} \left| \Pr(\mathrm{output}_\mathcal{B} = 0 \mid \beta = 0) - \Pr(\mathrm{output}_\mathcal{B} = 0 \mid \beta = 1) \right| \\
            =& \mathrm{Adv}_\mathcal{B} = \text{non-negl}(\lambda)
            \end{aligned}
            $$