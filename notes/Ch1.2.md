## 数字签名算法的安全性定义以及 Schnorr 签名算法
### 数字签名算法的安全性
#### 数字签名简介
- **数字签名的要求**：
    - **可认证性**：能够验证消息源，以及消息的真实性/完整性
    - **不可否认性**：签名能够被公开验证
- **数字签名实现安全认证的条件**：
    - 生成签名相对容易
    - 识别和验证签名相对容易
    - 在计算上不可伪造：
        - 对新消息伪造签名不可行
        - 对已有签名伪造新签名并通过认证不可行

#### 数字签名算法的语义及正确性要求
- **语义**：一个数字签名方案 $\mathrm{DS}$ 包含三个概率多项式时间（PPT）算法 $(\mathrm{Gen}, \mathrm{Sign}, \mathrm{Verify})$：
    1. **密钥生成算法**：$(PK, SK) \leftarrow \mathrm{Gen}(1^\lambda)$：⼀般为概率性算法
    2. **签名算法**：$\sigma \leftarrow \mathrm{Sign}(SK, M)$：$M \in \mathcal{M}$，其中 $\mathcal{M}$ 是消息空间
    3. **验证算法**：$0/1 \leftarrow \mathrm{Verify}(PK, M, \sigma)$：一般为确定性算法；输出 $1$ 表示验证通过
- **正确性要求**（Correctness）：对任意 $(PK, SK) \leftarrow \mathrm{Gen}(1^\lambda)$、任意 $M \in \mathcal{M}$、任意 $\sigma \leftarrow \mathrm{Sign}(SK, M)$，均有
    $$
    \mathrm{Verify}(PK, M, \sigma) = 1
    $$

#### 数字签名算法的 EUF-CMA 安全性
- **选择消息攻击**（Chosen Message Attacks, CMA）安全模型：
    ![CMA-security](image/image-5.png)
- **CMA 敌手攻击能力**：
    - 输入：公开信道中的 $PK, M, \sigma$
    - 运行时间：概率多项式时间 PPT
    - 攻击方式：选择消息攻击 CMA ── 敌手可以提供/决定/影响签名使用的消息
- **EUF 安全目标**：**存在不可伪造性**（Existential Unforgeability, EUF）── 敌手无法为一个**未查询过的新消息** $M^*$ 伪造有效签名 $\sigma^*$（即如果 $\mathrm{output} = (M^*, \sigma^*)$，满足 $M^* \notin \{M_i\}$ 和 $\mathrm{Verify}(PK, M^*, \sigma^*) = 1$，则攻破该目标）
- **数字签名算法的 EUF-CMA 安全性定义**: 任意概率多项式时间敌手在 CMA 安全模型中攻破 EUF 安全目标的**优势**是可忽略的，即
    $$
    \mathrm{Adv}_{\mathrm{EUF-CMA}} = \Pr\left[
    \mathrm{output} = (M^*, \sigma^*) \left|
    \begin{array}{l}
    (1)\ M^* \notin \{ M_i \} \\
    (2)\ \mathrm{Verify}(\mathrm{PK}, M^*, \sigma^*) = 1
    \end{array}
    \right.\right] = \mathrm{negl}(\lambda)
    $$

#### 数字签名算法的 sEUF-CMA 安全性
- **sEUF 安全目标**：**强存在不可伪造性**（Strong Existential Unforgeability, sEUF）── 敌手不仅无法为一个**未查询过的新消息** $M^*$ 伪造有效签名 $\sigma^*$，也无法为一个已签名消息 $M_i$ 伪造**新的有效签名** $\sigma^* \neq \sigma_i$（即如果 $\mathrm{output} = (M^*, \sigma^*)$，满足 $(M^*, \sigma^*) \notin \{(M_i, \sigma_i)\}$ 和 $\mathrm{Verify}(PK, M^*, \sigma^*) = 1$，则攻破该目标）
- **数字签名算法的 sEUF-CMA 安全性定义**: 任意概率多项式时间敌手在 CMA 安全模型中攻破 sEUF 安全目标的**优势**是可忽略的，即
    $$
    \mathrm{Adv}_{\mathrm{sEUF-CMA}} = \Pr\left[
    \mathrm{output} = (M^*, \sigma^*) \left|
    \begin{array}{l}
    (1)\ (M^*, \sigma^*) \notin \{ (M_i, \sigma_i) \} \\
    (2)\ \mathrm{Verify}(\mathrm{PK}, M^*, \sigma^*) = 1
    \end{array}
    \right.\right] = \mathrm{negl}(\lambda)
    $$

#### EUF-CMA 与 sEUF-CMA 安全性之间的关系
- **一般情况**：sEUF-CMA 安全 $\impliedby$ EUF-CMA 安全
    $$
    \mathrm{Adv}_{\mathrm{sEUF-CMA}} \leq \mathrm{Adv}_{\mathrm{EUF-CMA}}
    $$
- **若满足唯一性要求**：sEUF-CMA 安全 $\iff$ EUF-CMA 安全
    $$
    \mathrm{Adv}_{\mathrm{sEUF-CMA}} = \mathrm{Adv}_{\mathrm{EUF-CMA}}
    $$
    - **唯一性要求**（uniqueness）：$\forall (PK,SK) \leftarrow \mathrm{Gen}(1^\lambda), \forall M \in \mathcal{M}$，存在唯一的 $\sigma$ 使得 $\mathrm{Verify}(PK, M, \sigma) = 1$。


### 身份证明协议的安全性
#### 身份证明协议简介
- 核心：证明者向验证者证明他拥有私钥 $SK$
- 交互流程（3轮的Identification）：
    ![](image/image-6.png)
