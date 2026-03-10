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
    2. **签名算法**：$\sigma \leftarrow \mathrm{Sign}(SK, M)$：$M \in \mathbb{M}$，其中 $\mathbb{M}$ 是消息空间
    3. **验证算法**：$0/1 \leftarrow \mathrm{Verify}(PK, M, \sigma)$：一般为确定性算法；输出 $1$ 表示验证通过
- **正确性要求**（Correctness）：对任意 $(PK, SK) \leftarrow \mathrm{Gen}(1^\lambda)$、任意 $M \in \mathbb{M}$、任意 $\sigma \leftarrow \mathrm{Sign}(SK, M)$，均有
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
    - **唯一性要求**（uniqueness）：$\forall (PK,SK) \leftarrow \mathrm{Gen}(1^\lambda), \forall M \in \mathbb{M}$，存在唯一的 $\sigma$ 使得 $\mathrm{Verify}(PK, M, \sigma) = 1$。

### 身份证明协议的安全性
#### 身份证明协议简介
- 核心：证明者向验证者证明自己拥有公钥 $PK$ 所对应的私钥 $SK$
- 交互流程（3 轮的 Identification）：
    ![](image/image-6.png)

#### 身份证明协议的 UI-PA 安全性
- **被动攻击**（Passive Attacks, PA）安全模型：
    ![PA-security](image/image-7.png)
- **PA 敌手攻击能力**：
    - 输入：公开信道中的 $PK$ 及多次运行协议生成的 $(R_i, e_i, z_i)$
    - 运行时间：概率多项式时间 PPT
- **UI 安全目标**：**不可假冒性**（Unimpersonation, UI）── 敌手无法冒充 Prover 通过协议的验证算法（即如果 $\mathrm{output} = (R^*, e^*, z^*)$ 满足 $V(PK, e^*, R^*) = z^*$，则攻破该目标）
- **身份证明协议的 UI-PA 安全性定义**：任意概率多项式时间敌手在 P A安全模型中攻破 UI 安全目标的**优势**是可忽略的，即
    $$
    \mathrm{Adv}_{\mathrm{UI-PA}} = \Pr\left(
    \mathrm{output} = z^* \left|
    V(PK, e^*, R^*) = z^*
    \right.\right) = \mathrm{negl}(\lambda)
    $$

#### Fiat-Shamir 变换
- 核心：为了使用身份证明协议进行签名，证明者（签名者）可将挑战 $e$ 用哈希函数 $H()$ 计算，自己独立地执行协议，无需与验证者交互。
- 交互流程：
    ![](image/image-8.png)
- Fiat-Shamir 变换的安全性：**身份证明协议是 UI-PA 安全的** + **$H$ 为 RO** $\implies$ **通过 Fiat-Shamir 变换得到的签名算法是 EUF-CMA 安全的**

### Random Oracle 模型（随机预言机）
- Random Oracle 模型（随机预言机）是对哈希函数 $H: G \to \{0,1\}^{m}$ 的假设，用于安全性证明中，包含三个核心假设：
    1. **Oracle-预言机假设**：敌手无法自己计算 $H$ 的值，只能通过查询“预言机” $H(·)$ 的方式获得 $H$ 的值：每一次查询，敌手向预言机 $H(·)$ 提交一个 $X$，$H(·)$ 返回输出 $H(X)$。（**黑盒**）
    2. **Random-随机性假设**：随机预言机 $H(·)$ 在每一个 $X$ 上的输出值 $H(X)$ 都是服从值域 $\{0,1\}^m$ 上均匀分布的，其随机性来源于预言机 $H(·)$ 内部。
    3. **Programmable-可编程性假设**：在安全性证明中，“随机预言机” $H(·)$ 由环境/挑战者向敌手提供。


### Schnorr身份证明协议与签名算法
#### Schnorr 身份证明协议
- 目的：Prover 向 Verifier 证明自己拥有 $PK$ 所对应的私钥 $SK$。
- 交互流程：基于椭圆曲线上的离散对数问题
    ![](image/image-9.png)

#### Schnorr 签名算法
- **密钥生成算法** $(PK, SK) \leftarrow \mathrm{Gen}(1^\lambda)$：
    1. 选择循环群 $G$，其阶为素数 $p$、生成元为 $g$
    2. 均匀选取 $s \leftarrow \mathbb{Z}_p$，计算 $h := g^s \in G$
    3. 输出 $PK = (G, p, g, h)$，$SK = s$
- **签名算法** $\sigma \leftarrow \mathrm{Sign}(SK, M)$，消息空间为 $\mathbb{M}=\{0,1\}^{*}$
    1. 均匀选取 $r \leftarrow \mathbb{Z}_{p}$，计算 $R:=g^{r} \in G$
    2. 计算 $e:=H(R, M) \in \mathbb{Z}_{p}$
    3. 计算 $z:=e \cdot s+r \in \mathbb{Z}_{p}$
    4. 输出 $\sigma:=(R, z)$
- **验证算法** $0/1 \leftarrow \mathrm{Verify}(PK, M, \sigma=(R, z))$
    1. 计算 $e:=H(R, M) \in \mathbb{Z}_{p}$
    2. 验证 $g^{z} \stackrel{?}{=} h^{e} \cdot R$，相等输出 $1$，否则输出 $0$

## Schnorr签名算法的安全性引理
**引理1 (Fiat-Shamir转换)**:
$Schnorr身份证明协议是UI\text{-}PA安全的 + H为RO \Rightarrow Schnorr签名算法是EUF\text{-}CMA安全的$。

### 安全性归约
- UI-PA安全模型的敌手$E_{id}$：可进行多次协议运行查询，输出$(R^*, z^*)$尝试假冒，优势为$Adv=Pr\left[g^{z^{*}}=h^{e^{*}} \cdot R^{*}\right]$
- EUF-CMA安全模型的敌手$E_{Sign}$：可进行多次签名查询，输出$(M^*, \sigma ^*=(R^*,z^*))$尝试伪造，优势为$Adv=Pr\left[\begin{array}{l}(1) M^{*} \notin\left\{M_{i}\right\} \\ (2) Verify\left(PK, M^{*}, \sigma^{*}\right)=1\end{array}\right]$
- 签名查询与协议查询的对应：$R_{i}= g^{r_i}$（$r_i \leftarrow \mathbb{Z}_p$），$e_{i}=H\left(R_{i}, M_{i}\right)$，$z_{i}=e_{i} \cdot s+r_{i}$，$\sigma_i= (R_i, z_i)$

## 回顾：Random Oracle模型
随机预言机是对Hash Function $H: G \to \{0,1\}^{m}$的假设，用于安全性证明中，核心假设：
1. **预言机(Oracle)假设**：
敌手无法自己计算$H$的值，只能通过查询“预言机”$H(·)$的方式获得$H$的值：每一次查询，敌手向预言机$H(·)$提交一个$X$，$H(·)$返回输出$H(X)$。

2. **随机性(Random)假设**：
随机预言机$H(·)$在每一个$X$上的输出值$H(X)$都是服从值域$\{0,1\}^m$上均匀分布的，其随机性来源于预言机$H(·)$内部。

3. **可编程性(Programmable)假设**：
在安全性证明中，“随机预言机”$H(·)$由环境/挑战者向敌手提供。

### 进阶的安全性规约技术：Random Oracle模型推论
$Oracle假设 + Random假设$：
如果敌手没有向预言机$H(·)$查询过某个输入$X$，那么$H(X)$的值对于敌手而言是完全均匀的。

$Oracle假设 + Programmable假设$：
挑战者可控制预言机$H(·)$的输出，为敌手的查询返回指定值。

$Random假设+ Programmable假设$：
环境/挑战者针对敌手的每一次预言机$H(·)$查询$X$，返回值域$\{0,1\}^m$上均匀分布的值作为$H(X)$的值。

**注**:
- 敌手无法仅阅读Hash Function的代码而不调用Hash Function推断出$H(X)$的值。
- 参考第5.5节:Katz J, Lindell Y. Introduction to modern cryptography, 2rd

## 引理1证明(分析)
### 断言1
如果在数字签名的EUF-CMA安全模型下，存在攻击者$A$以不可忽略的概率攻破Schnorr签名。设$A$的输出为$(M^{*}, \sigma^{*}=(R^{*}, z^{*}))$，即
$$Adv_{A}=Pr\left[output _{A}=(M^{*}, \sigma^{*}): h^{H(R^{*}, M^{*})} \cdot R^{*}=g^{z^{*}}\right]=non-negl(\lambda)$$
则$A$以不可忽略的概率查询过$(R^{*}, M^{*})$的Hash值。

### 分析
如果$A$没有查询过$(R^{*}, M^{*})$的Hash值，则在$H$为RO假设下，$H(R^{*}, M^{*})$对于$A$是$\mathbb{Z}_{p}$中均匀随机的元素，因此$h^{H(R^{*}, M^{*})}$对于$A$是$G$中均匀随机的元素。

则$A$猜对$z^{*} \in \mathbb{Z}_{p}$满足
$$h^{H\left(R^{*}, M^{*}\right)}=g^{z^{*}} \cdot\left(R^{*}\right)^{-1}$$
的概率为$\frac{1}{|G|}=\frac{1}{p}=negl(\lambda)$。

### B的策略
假设在EUF-CMA安全模型下，存在攻破Schnorr签名算法的$A$，构造攻击Schnorr身份证明协议的$B$攻破其UI-PA安全性。

**思路**:将$A$作为$B$的子算法，为$A$提供合法的输入，以及返回合法的签名查询，利用$A$的输出结果帮助$B$解决UI-PA中的挑战输出。

#### B的策略(1/2)
1. $B$从UI-PA挑战者处获得$PK$，并将$PK$发送给$A$
2. $A$向$B$发起签名查询$M_i$，$B$模拟EUF-CMA挑战者，生成$R_{i}= g^{r_i}$（$r_i \leftarrow \mathbb{Z}_p$），随机选取$e_i \leftarrow \mathbb{Z}_p$，计算$z_i= e_i \cdot s+ r_i$，将$\sigma_i= (R_i, z_i)$返回给$A$
3. $B$同时将$(R_i, e_i, z_i)$作为UI-PA的协议运行查询结果记录
4. $A$输出伪造签名$(M^{*}, \sigma ^*=(R^{*},z^{*}))$，满足$h^{H\left(R^{*}, M^{*}\right)} \cdot R^{*}=g^{z^{*}}$，$B$将$R^*$作为UI-PA挑战的输入，尝试输出$z^*$完成假冒。

#### B的策略(2/2)
$H$为RO：$B$为$A$的$(R, M)$查询提供Hash值，设$A$向$B$查询的次数为$Q(\lambda)$次：
1. 对$A$查询过的消息$\{M_{i}\}_{i \in I}$，以及返回的签名$\sigma_{i}=(R_{i}, z_{i})$，记录$H ( R _ { i } , M _ { i } ) = e _ { i }$；
2. $B$随机选择$j \in[1, Q(\lambda)]$，如果$A$的第$j$次查询$(R_{j}, M_{j})$中$M_{j} \notin \{M_i\}$，则$B$将$A$的第$j$次查询$(R_{j}, M_{j})$中的$R_{j}$作为其向挑战者$Eid$的输入；并将$Eid$返回的$e_{j}$作为$(R_{j}, M_{j})$的Hash值返回给$A$，即$H(R_{j}, M_{j})=e_{j}$；否则$M_{j} \in \{M_i\}$，重新选择$j \in[1, Q(\lambda)]$；
3. 对于$k \in[1, Q(\lambda)] \setminus \{j\}$，$B$查询记录，如果有$H(R_{k}, M_{k})$的记录，返回$H(R_{k}, M_{k})$；如果没有$H(R_{k}, M_{k})$的记录，则随机选择$e \leftarrow \mathbb{Z}_{p}$，作为$(R_{k}, M_{k})$的Hash值返回，并记录。

### B的优势分析
设$A$的输出为$(M^{*}, \sigma^{*}=(R^{*}, z^{*}))$，记事件$P_{1}$为 "$A$向$B$查询过$(R^{*}, M^{*})$的Hash值"。

由前面的断言可知$Pr[P_{1}]= non-negl(\lambda)$，则
$$\begin{array}{rl}& Adv_{B}\geq Pr[P_{1}]\cdot Pr\left[ \left( R_{j},M_{j}\right) =\left( R^{*},M^{*}\right) \mid P_{1}\right] \geq Pr[P_{1}]\cdot 1/Q(\lambda )\\ & =non-negl(\lambda )\end{array}$$

**引理1得证**。

## Schnorr 身份证明协议、签名算法的安全性
**引理1**:
$Schnorr身份证明协议是UI\text{-}PA安全的 + H为RO \Rightarrow Schnorr签名算法是EUF\text{-}CMA安全的$。

**引理2**:
$DL问题困难 \Rightarrow Schnorr身份证明协议是UI\text{-}PA安全的$。

### DL问题(离散对数问题)
$G$为循环群，其阶为素数$p$、生成元为$g$，并均匀选取$h \leftarrow G$：
- 输入:$(G, p, g, h)$
- 输出:$s = DLOG_g h$（即找到$s$使得$h=g^s$）

### Schnorr 身份证明协议：UI-PA安全性证明
**证明**:安全性归约（$\Rightarrow$）：由攻破UI-PA安全性的敌手$A$来构造解决DL问题的敌手$B$。

若$V(R^{*}, e^{*}, z^{*})=1$，即$R^{*} \cdot h^{e^{*}}=g^{z^{*}}$，若敌手$A$能输出两组不同的有效三元组：
$$\left(R^{*}, e_{1}^{*}, z_{1}^{*}\right) \Rightarrow R^{*} \cdot h^{e_{1}^{*}}=g^{z_{1}^{*}}$$
$$\left(R^{*}, e_{2}^{*}, z_{2}^{*}\right) \Rightarrow R^{*} \cdot h^{e_{2}^{*}}=g^{z_{2}^{*}}$$

两式相减得：
$$g^{s\left(e_{1}^{*}-e_{2}^{*}\right)}=g^{z_{1}^{*}-z_{2}^{*}}$$

由于$G$的阶为素数$p$，则：
$$s\left(e_{1}^{*}-e_{2}^{*}\right) \equiv z_{1}^{*}-z_{2}^{*} \pmod p$$

若$e_{1}^{*} \neq e_{2}^{*}$，则可计算出离散对数：
$$s \equiv (e_{1}^{*}-e_{2}^{*})^{-1}(z_{1}^{*}-z_{2}^{*}) \pmod p$$

### Rewind 技术
设$A$为攻击Schnorr证明协议的PPT算法，构造解决DL问题的算法$B$，算法的输入为$(G, p, g, h)$：
1. 令$PK=(G, p, g, h)$，调用$A(PK)$，并且为$A$的每次查询提供正确的三元组$(R,e,z)$；
2. 当$A$输出$R^{*}$，均匀地选择$e_{1}^{*} \leftarrow \mathbb{Z}_{p}$，将$e_{1}^{*}$作为挑战发送给$A$并收到$A$的输出$z_{1}^{*}$；
3. 再次调用$A(PK)$，除了返回挑战$e^{*}$时的随机数，其余使用和第一次调用$A$相同的随机数（包括$A$使用的随机数，以及$B$返回查询结果使用的随机数）。当$A$输出$R^*$，使用不同的随机数均匀地选择$e_{2}^{*} \leftarrow \mathbb{Z}_{p}$，将$e_{2}^{*}$作为挑战发送给$A$并收到$A$的输出$z_{2}^{*}$；
4. 如果$R^{*} \cdot h^{e_{1}^{*}}=g^{z_{1}^{*}}$，$R^{*} \cdot h^{e_{2}^{*}}=g^{z_{2}^{*}}$并且$e_{1}^{*} ≠e_{2}^{*}$，输出$s \equiv(e_{1}^{*}-e_{2}^{*})^{-1}(z_{1}^{*}-z_{2}^{*}) \pmod p$。

### B的优势分析
用变量$\omega$代表$B$除了返回挑战$e^{*}$之外所使用的随机数（包括$A$使用的随机数，以及$B$返回查询结果使用的随机数）；

定义$V(\omega, e^{*})=1$当且仅当$A$在$B$使用随机数$\omega$以及挑战$e^{*}$时，$A$成功返回正确的$z^{*}$，即$R^{*} \cdot h^{e^{*}}=g^{z^{*}}$。

$$\begin{aligned} Adv_{B} & =\underset{\omega, e_{1}^{*}, e_{2}^{*}}{Pr}\left[V\left(\omega, e_{1}^{*}\right)=1 \bigwedge V\left(\omega, e_{2}^{*}\right)=1 \bigwedge e_{1}^{*} \neq e_{2}^{*}\right] \\ & \geq \underset{\omega, e_{1}^{*}, e_{2}^{*}}{Pr}\left[V\left(\omega, e_{1}^{*}\right)=1 \bigwedge V\left(\omega, e_{2}^{*}\right)=1\right]-1 / p \\ & =\sum Pr[\omega=R] \cdot Pr[\omega=R] \cdot \underset{e_{1}^{*}, e_{2}^{*}}{Pr}\left[V\left(R, e_{1}^{*}\right)=1 \bigwedge V\left(R, e_{2}^{*}\right)=1\right]-1 / p \\ & =\sum Pr[\omega=R] \cdot Pr[\omega=R] \cdot \underset{e^{*}}{Pr}\left[V\left(R, e^{*}\right)=1\right]^{2}-1 / p \\ & \geq\left(\sum Pr[\omega=R] \cdot Pr\left[V\left(R, e^{*}\right)=1\right]\right)^{2}-1 / p \\ & =Pr_{\omega, e^{*}}\left[V\left(\omega, e^{*}\right)=1\right]^{2}-\frac{1}{p} \\ & =Adv_{A}^{2}-\frac{1}{p}=non-negl(\lambda) . \end{aligned}$$

## Schnorr签名算法的安全性定理
**定理**:
$DL问题困难 + H为RO \Rightarrow Schnorr签名算法是EUF\text{-}CMA安全的$。

反之亦有：$Schnorr签名算法是EUF\text{-}CMA安全的 \Rightarrow DL问题困难$。

## 总结：数字签名算法的安全性及其证明
1. 数字签名算法简介(语义、正确性、安全性)
2. 身份认证协议及Fiat-Shamir 变换
3. 基于离散对数相关问题的签名算法:
   ➢Schnorr身份证明的UI-PA安全
   ➢Schnorr签名算法的EUF-CMA安全

谢谢!