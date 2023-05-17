# 第3课 课后作业

给定整数 $x, m$，如果 $x$ 在模 $m$ 下是二次剩余，即存在整数 $s$，使得 $s^{2} \equiv x(\bmod m)$，则记作 $QR(m, x)=1$ ; 如果 $x$ 在模 $m$ 下不是二次剩余，则记作 $QR(m, x)=0$。

## 二次非剩余 Quadratic nonresidue

_假定_：证明者可以为所有 $m, x$ 计算 $QR(m, x)$

_公共输入_：正整数 $m, x$

_目标_：证明者想要说服验证者 $QR(m, x)=0$

::: tip 协议

1. Verifier 在与 $m$ 互质的元素中均匀地随机选取一个 $s \in \mathbb{Z}_{m}$，同时抛硬币 $b \gets_R \{0,1\}$。 设
    $$
    y \leftarrow \begin{cases}
        s^{2} x & \text { if } b=0, \\
        s^{2} & \text { if } b=1.
    \end{cases}
    $$
    Verifier 将 $y$ 发送给 Prover 并挑战 Prover 以确定 $b$。

2. Prover 计算 $QR(m, y)$ 并将其值发送回 Verifier

3. 如果 Verifier 从 Prover 收到的值确实等于 $b$，则 Verifier 接受； 否则它拒绝。

::::

您需要检查：

- (a) **完备性**：如果 $QR(m, x)=0$ 并且双方都按照协议行事，那么验证者总是接受。

- (b) **可靠性**：如果 $QR(m, x)=1$，那么无论 Prover 做什么（Prover 不必遵循协议），Verifer 都会以 $\geq 1 / 2$ 的概率拒绝


答： 
（1） **完备性** : 如果$QR(m, x)=0$, 则存在如下两种可能
(a) 如果$b=0$，$y=s^2x$，Prover是诚实的，计算$QR(m,y)=QR(m,s^2x)$，由于$QR(m,x)=0$，则一定有$QR(m,s^2x)=0$，因此Verifier接收到的就是0，验证$QR(m,y)=0=b$，验证者accept。
(b) 如果$b=1$，$y = s^2$，Prover计算$QR(m,y)=QR(m,s^2)$。$s^2$一定是模$m$的二次剩余，因为根据定义，存在一个数$s$，使得$s^2 = s^2 \mod m$。因此$QR(m,s^2)=1$，Verifier就会接收到1，验证$QR(m,y)=1=b$，验证者accept。

综上，验证者总是接受的。

(2) **可靠性** : 如果 $QR(m, x)=1$, 则对 Prover 进行如下分类讨论
(a) 当 Prover 是诚实的时候，如果$b=0$，$y=s^2x$，Prover 返回 1 给 Verifier, 验证$QR(m,y)=1 \neq b$, Veifier 拒绝; 如果$b=1$，$y=s^2$，Prover 返回 1 给 Verifier, 验证$QR(m,y)=1=b$, Veifier 接受; 所以这个场景下，Verifer 会以 $\geq 1 / 2$ 的概率拒绝
(b) 当 Prover 是非诚实的时候，如果$b=0$，$y=s^2x$，Prover 返回 0 给 Verifier, 验证$QR(m,y)=0=b$, Veifier 接受; 如果$b=1$，$y=s^2$，Prover 返回 0 给 Verifier, 验证$QR(m,y)=0 \neq b$, Veifier 拒绝;
所以宗上所述，Verifer 都会以 $\geq 1 / 2$ 的概率拒绝


## 二次剩余 Quadratic residue
_公共输入_：正整数 $m, x$ 且满足 $gcd(m, x)=1$

_私有输入_：证明者知道一个秘密整数 $s$ 使得 $s^{2} \equiv x \bmod m$

_目标_：证明者想要说服验证者 $QR(m, x)=1$ 而不透露有关 $s$ 的任何信息

（请注意，证明者可以通过向验证者发送 $s$ 轻松说服验证者 $QR(m, x)=1$，但这会完全暴露 $s$。）

::: tip 协议

1. Prover 在与 $m$ 互质的剩余中随机选择一个均匀的 $t \in \mathbb{Z}_{m}$。 Prover 将 $y \leftarrow x t^{2} \bmod m$ 发送给 Verifier。

1. Verifier 抛硬币 $b \gets_R \{0,1\}$ 并将 $b$ 发送给 Prover。

1. Prover 收到 $b$ 并将$u$值发送给 Verifier
    $$
    u \leftarrow \begin{cases}
        t & \text { if } b=0, \\
        s t & \text { if } b=1.
    \end{cases}
    $$

1. 验证者接受证明如果
    $$
    y \equiv \begin{cases}
        u^2 x \bmod m & \text { if } b=0, \\
        u^2 \bmod m & \text { if } b=1.
    \end{cases}
    $$
    否则拒绝。

::::

您需要检查：

- (a) **完备性**：如果双方都按照协议行事，那么 Verifier 总是接受。

- (b) **可靠性**：如果 $QR(m, x)=0$（特别是，Prover 不知道任何有效的 $s$ ），那么无论 Prover 做什么，Verifier 都以 $\geq 1 / 2$ 的几率拒绝。

- (b\*) **知识可靠性**：对于任何使得Verifier接受的证明算法，如果允许Verifier同时查询$b\in\{0,1\}$的两个值（对于 相同的 $t$ )，则 Verifier 可以从 Prover 中提取秘密 $s$。 （这里的重点是除非证明者“知道”$s$，否则证明者无法说服验证者。）

- (c) **零知识**：无论验证者做什么（验证者的行为可能与协议不同），验证者都可以在不与证明者交互的情况下自行模拟整个交互，这样如果验证者接受，那么记录下来的信息与实际交互几乎不可区分。

     提示：想象模拟器自己生成一个随机的 $u$ 而不是从 Prover 获取 $u$。

     （这部分有点棘手，因为允许对抗性验证器的行为与协议不同 - 请记住我们试图展示的是，无论验证器做什么，它都不会从证明者那里学到关于 $s$ 的信息 验证者不可能简单地自行生成。如果您想查看解决方案，请参阅 [Barak 笔记中的定理 13.6](https://files.boazbarak.org/crypto/lec_14_zero_knowledge.pdf) 的证明。）


答：
(a)**完备性**：也就是$QR(m,x)=1$。Verifier选择$b$有两种情况：
(1)$b=0$，此时Prover计算的$u=t$，将$u$发送给Verifier进行验证，等式左边$y = xt^2 \mod m$，等式右边$u^2 x \mod m = t^2x \mod m$，等式两边相等，验证通过。

(2)$b=1$，此时Prover计算$u = st$，将$u$发送给Verifier进行验证，等式左边$y = xt^2 \mod m$，等式右边$u^2 \mod m = s^2t^2 \mod m$，由于$QR(m,x)=1$，因此$s^2 = x \mod m$，从而等式左右两边相等，$y = u^2 \mod m$，验证通过。

(b)**可靠性**：如果$QR(m,x)=0$
(1)$b=0$，Prover选择$u=t$会通过验证，因为$y=xt^2 = u^2x \mod m$。

(2)$b=1$，Verifier会验证$y = xt^2 = u^2 \mod m$。由于$QR(m,x)=0$，不会存在一个数$s$，使得$s^2 = x \mod m$，将Verifier验证的等式变形为$x = (\frac{u}{t})^2 \mod m$，由于不存在这样的数，因此这里Verifier会验证失败，当然Prover不知道有效的$s$使得$s = \frac{u}{t}$，Verifier会验证失败。

综上，Verifier会以超过$1/2$的概率拒绝。

(b*)**知识可靠性**：
(1) Verifier首先选择随机数$b=0$，接收到Prover发送的值$u = t$
(2) Verifier接着选择随机数$b=1$，接收到Prover发送的另一个值$u = st$
(3) Verifier 将 step2 接受到的值除以 step1 接受到的值，就可以得到 s

(c) **零知识**：
(c)**零知识**：现在我们设计一个模拟器，模拟器的视角和真实交互视角一样，但是模拟器中不知道秘密$s$。

Step1: 随机选择一个随机数$b$。

Step2: 选择一个随机数$u \in \mathbb{Z}_{m}$.

Step3: 根据$b$的值，计算$y$，如果$b=0$，$y=u^2x \mod m$，如果$b=1$，$y = u^2 \mod m$。

Step4: 接着和Verifier进行交互，发送$y$，如果Verifier恰好也是我们在Step1中选择的随机数$b$，那么就继续下面的交互，最终会通过验证。如果Verifier选择的随机数和我们在Step1中选择的随机数不同，我们转到Step1，重新随机选择一个数。这一过程中由于$b$就两个数，因此期望2轮会和Verifier选择的随机数相同，通过验证。

这样的一个模拟器和知道秘密的Prover与Verifier交互，其视角在Verifier看来都是一样的，因此也就证明了零知识。

## 双线性自映射意味着DDH的失效 Self-pairing implies failure of DDH
设 $\mathbb{G}$ 和 $\mathbb{G}_{T}$ 是相同素数阶 $q$ 的阿贝尔群（以加法写出）。 设 $g \in \mathbb{G}$ 为生成元。 假设我们有一个可有效计算的非退化双线性对

$$
e: \mathbb{G} \times \mathbb{G} \rightarrow \mathbb{G}_{T}
$$

给出一个有效的决策算法，给定 $\alpha g, \beta g, y \in \mathbb{G}$（其中 $\alpha, \beta \in \mathbb{Z}_{q}$为未知)，判断是否$\alpha \beta g=y$。

提示：计算与 $g$ 的映射。

此练习表明确定性 Diffie-Hellman (DDH) 假设对于双线性自映射是错误的。

答：判断等式$\alpha \beta g = y$是否成立，只需要计算$e(\alpha g, \beta g) = e(y, g)$是否成立。在等式$\alpha \beta g = y$两端同时计算与$g$的映射，得到$e(\alpha \beta g, g) = e(\alpha g, \beta g) = e(y, g)$。

## BLS 签名聚合 BLS signature aggregation
::: tip 设置

1. 映射 Pairing $e: \mathbb{G}_{0} \times \mathbb{G}_{1} \rightarrow \mathbb{G}_{T}$，其中$\mathbb{G}_{0} , \mathbb{G}_{1}, \mathbb{G}_{T}$ 是素数阶 $p$ 的阿贝尔群（以加法写出），生成元分别是 $g_{0}, g_{1}, g_{ T}$

1. 哈希函数 $H$ 在 $\mathbb{G}_{1}$ 中取值
::::

BLS签名方案定义为：

- $\operatorname{KeyGen()}$: 选择随机（私钥）$sk=\alpha \leftarrow{ }_{R} \mathbb{Z}_{q}$ 并设置（公钥）$pk \leftarrow \alpha g_{0} \in \mathbb{G}_{0}$
- $\operatorname{Sign}(sk, m)$: 输出 $\sigma \leftarrow \alpha H(m) \in \mathbb{G}_{1}$ 作为消息 $m$ 上的签名
- $\operatorname{Verify}(pk, m, \sigma)$ ：如果 $e\left(g_{0}, \sigma\right)=e(pk, H(m))$，则接受。

检查验证算法是否始终能接受正确签名。 还争辩说，如果给伪造者的是 $pk$ 而不是 $sk$，那么为任何消息 $m$（伪造者可以自由选择 $m$ ）想出一个伪造的签名 $\sigma$ 是计算上不可行的。 这在选择的消息攻击下被称为存在不可伪造。 你能指定一些计算难度假设吗？

请检查验证算法是否总是接受正确的签名。并论证：在给定$pk$但没有$sk$的情况下，（伪造者可以选择消息）对于任何消息$m$，构造出一个伪造的签名$\sigma$是计算上不可行的 。这被称为 _在选择消息攻击下的存在性不可伪造性_ 。您能找到一些计算难度假设吗？

**签名聚合**。 给定三元组 $\left(pk_{i}, m_{i}, \sigma_{i}\right)$ 其中 $i=1, \ldots, n$（即来自 $n$ 个用户），我们可以聚合这些 $n$ 个签名 $\sigma_{1}, \ldots, \sigma_{n}$ 通过简单地在 $\mathbb{G}_{1}$ 中求和，将其合并为一个签名：

$$
\sigma \leftarrow \sigma_{1}+\cdots \sigma_{n} \in \mathbb{G}_{1}
$$

给定 $\left(pk_{1}, m_{1}\right), \ldots,\left(pk_{n}, m_{n}\right)$ 和聚合签名 $\sigma$，表明我们可以 通过检查来验证确实所有 $n$ 用户都签署了他们的消息

$$
e\left(g_{0}, \sigma\right)=e\left(pk_{1}, H\left(m_{1}\right)\right)+\cdots+e\left(pk_{n}, H\left(m_{n}\right)\right)
$$

注意：由于 _恶意公钥攻击_ ，上述签名聚合方案不安全。 使方案安全的一种方法是确保所有消息 $m_{i}$ 是不同的，例如，要求每个 $m_{i}$ 以用户的公钥 $pk_{i}$ 开头。

有关 BLS 签名聚合的更多信息，请参阅 <https://crypto.stanford.edu/~dabo/pubs/papers/BLSmultisig.html>


答：验证算法始终能接受正确的签名。因为在$Verifiy(pk,m,\sigma)$这一步，等式左端$e(g_0,\sigma) = e(g_0, \alpha H(m))$，等式右端$e(pk,H(m)) = e(\alpha g_0, H(m))= e(g_0,\alpha H(m))$，等式两端相等，会始终通过算法验证。

选择消息攻击下的存在性不可伪造性：攻击者想要伪造签名$\sigma \leftarrow \alpha H(m)$，首先在多项式时间计算出$\alpha$是困难的，因为知道公钥和生成员$g_0$在多项式时间内计算出私钥是离散对数困难问题。其次，攻击者想要任意选择消息$m$使得$H(m)$等于一个特定的值，这在多项式时间内也无法做到，因为哈希函数具有抗碰撞性，无法在多项式时间内找到一个$m$，使得$H(m)$等于一个事先给定的值。