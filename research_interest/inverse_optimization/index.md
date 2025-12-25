<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>

更新日：2025年12月25日

# 混合整数計画問題の逆最適化問題

**キーワード**: 逆最適化，数理最適化，組合せ最適化

**研究内容**：与えられたデータが最適解になるような，(混合整数)線形計画法の目的関数の重みと制約条件の閾値を推定する高速なアルゴリズムを開発した．

**背景**:　最適化問題は，人間の意思決定から自然現象に至るまで，さまざまなプロセスやシステムの順方向モデルとして定義することが多い．しかし，そのようなモデルにおける真の目的関数や制約条件が，事前にはほとんど分かっていないのが実情である．したがって，観測された最適解から目的関数や制約条件を推定する，すなわち**逆最適化**の問題は，実践的に非常に重要である．逆最適化は，地球物理学，交通，電力システム，スケジューリング，医療への応用があり，また，逆強化学習，コントラスト学習などをはじめとするさまざまな機械学習の基礎としても発展している．(cf. [[Sakaue et. al. 2025](#sakaue2025online), [13](#K13)])
<!-- 
この分野における初期の研究は地球物理学から登場し、地震波データから地下構造を推定することを目的としていた \citep{Tarantola1988-tq,Burton1992-dc}。  
その後、逆最適化は広く研究されるようになり \citep{Ahuja2001-cv,Heuberger2004-zv,Chan2019-zg,Chan2023-qk}、交通 \citep{Bertsimas2015-kw}、電力システム \citep{Birge2017-il}、医療 \citep{Chan2022-uq} などのさまざまな分野に応用されてきた。  
さらに、逆強化学習 \citep{Ng2000-sf} やコントラスト学習 \citep{Shi2023-nd} をはじめとするさまざまな機械学習手法の基礎としても発展している。
-->

そこで，北岡がかかわった研究は以下である．(独立に読めます．)
- [1](#S1). 混合整数線形計画における目的関数の重みを高速に解くアルゴリズムを提案したこと [[5](#K5), [8](#K8)]

- [2](#S2). 混合整数線形計画における目的関数の重みと制約条件の閾値を高速に解くアルゴリズムを提案したこと [[13](#K13)]

## <a id="S1">1</a>. 混合整数線形計画における目的関数の重みを高速に解くアルゴリズムを提案したこと[[5](#K5), [8](#K8)]

**問題設定**: 順問題の目的関数が区分線形関数の線形和で書けている，混合整数線形計画を含む，基本的かつ重要な問題を取り上げる．
$i = 1 , \ldots ,d $に対し，
$h_i \colon \mathbb{R}^{\bullet} \to \mathbb{R}$は区分線形であるとする．
集合$\mathcal{S}$を

$$
    \mathcal{S} = \{ s = (A,b) \in  \mathbb{R}^{\bullet \times \bullet} \times \mathbb{R}^{\bullet} \}
$$

とし，$s \in \mathcal{S}$に対して集合

$$
    X(s) := \{ x \in \mathbb{R}^{\bullet} \times \mathbb{Z}^{\bullet} | Ax \leq b\} 
$$

とする．
確率単体$$\Delta^{d-1} := \{ \phi \in \mathbb{R}_{\geq 0 }^d | \sum_i \phi_i =1 \} $$
とする．
順問題を以下で定義する．

$$
\begin{equation*}
    \begin{split}
        x^* (\phi , s ) & \in \mathrm{argmax}_{x \in X (s)} \phi^{\intercal} h ( x ) \left( 
        =
        \sum_{i=1}^d \phi_i h_i (x)
        \right)
        , 
        \\
        \quad a(\phi , s ) & := h (x^*(\phi , s ))
        .
    \end{split}
    %\label{eq:forward_optimization_problem}
\end{equation*}
$$

順問題の逆最適化問題を定義する．
データ$\mathcal{D} = \\{ (s^{(n)} , a^{(n)} ) \\}_{n=1}^N$は未知の真の重み$\phi^* $が存在し，

$$
   s^{(n)} \in \mathcal{S} , 
   \quad a^{(n)} = a ( \phi^* , s^{(n)} )
   %\label{assu:data-follows-solver}  
$$

を満たすとする．
この未知の重み$\phi^* $をデータ$\mathcal{D}$から推定する問題，つまり，解の予測誤差

$$
\begin{equation*}%\label{eq:def_minimization_PLS} 
    \ell_{\mathrm{pres}} (\phi) := \frac{1}{N}\sum_{n=1}^N \| a(\phi , s^{(n)} ) - a^{(n)} \|_2^2.
\end{equation*}
$$

を最小化する問題を考える．

**課題**: 既存の手法では解の予測誤差を最小化するのに効率が悪く，特に高次元の場合次元の影響を受ける．素朴な方法として，
確率単体$\Delta^{d-1}$
から均一に点を取る方法(UPA)やランダムに点を取る方法(RPA)が挙げられる．UPAとRPAは次元$d$の影響を大きく受ける．


| 手法 | $\left\|\left\| \phi_k - \phi^* \right\|\right\| $ ※解の予測誤差が$0$でない場合 |
|------|-------------------------|
| UPA | $O(k^{-1/(d-1)})$ |
| RPA | $O_{\mathbb{P}}\left(\left( \frac{\log k}{k} \right)^{1/(d-1)}\right)$ |
| PSGD2 (提案手法) [[8](#K8)] | $O\left(k^{1/(\gamma+1)} \exp\left(-\frac{\gamma}{\gamma+2}k^{1/2}\right)\right)$ |

**効果**: 提案手法[[5](#K5), [8](#K8)]であるPSGD2は，推定困難な正定数$\gamma$が存在するものの，既存手法RPA, UPAに比べて，解の予測誤差の最小値を高速に達成できる．

**提案手法**: 

与えられた解を模倣するために，
suboptimality損失

$$
\begin{equation*}
    \ell_{\mathrm{sub}} (\phi) := \frac{1}{N} \sum_{n=1}^N \left( \phi^{ \intercal} a(\phi , s^{(n)} ) - \phi^{ \intercal} a^{(n)} \right) 
\end{equation*}
$$

を最小化すればよいことを説明する．
Suboptimality損失は区分線形(Lipschitz)で凸であり,
劣勾配が

$$
\begin{equation*}
    g (\phi) = \frac{1}{N}
    \sum_{n=1}^N 
    \left(
        a ( \phi , s^{(n)} )
        -
        a^{(n)}
    \right)    
\end{equation*} 
$$

という性質を持つ．
劣勾配$g$は解の予測誤差に現れるノルムの中身の平均を取ったものである．解の予測誤差が最小値である$0$になることと劣勾配$g$が$0$になることは同値である[補題 5.11, [8](#K8)]．
劣勾配$g$が$0$を達成することと，suboptimality損失が最小値を達成することは同値だとする．
このとき，解の予測誤差が最小値$0$を達成することと，suboptimality損失が最小値を達成することは同値だとしてよい．

Suboptimality損失は，凸でLipschitzで，劣勾配が$g$で与えられている．従って，Suboptimality損失を$\Delta^{d-1}$の上で最小化するためには，$\Delta^{d-1}$における射影劣勾配法をsuboptimality損失に適用すれば良い．学習率$$\left\{ \alpha_k \right\}_{k} \subset \mathbb{R}_ {>0}$$としたときの，射影劣勾配法をsuboptimality損失に適用したものを[アルゴリズム1](#alg:1)で与える．

[アルゴリズム1](#alg:1)に
学習率を$
   \alpha_k =
    \ell_{\mathrm{sub}} (\phi_k ) / \|\| g (\phi_k ) \| \|^2  ,
$
としたものをPSGDP,
学習率を
$
    \alpha_k = k^{-1/2}
    /
        \left\|\left\|
         g (\phi_k)
        \right\|\right\|
$
としたものをPSGD2とする．


> **<a id="alg:1">アルゴリズム1</a>**: suboptimality損失最小化 [アルゴリズム1, [5](#K5)]
> 1. $\phi_1 = \left(\frac{1}{d}, \ldots, \frac{1}{d}\right) \in \Delta^{d-1}$ で初期化
> 2. For $k = 1, \ldots, K-1$:
> 3. &nbsp;&nbsp;&nbsp;&nbsp; $\phi_{k+1} \leftarrow \phi_k - \alpha_k g(\phi_k)$ を計算
> 4. &nbsp;&nbsp;&nbsp;&nbsp; $\phi_{k+1}$ を $\Delta^{d-1}$ へ射影する
> 5. End For
> 6. $$\phi^{\mathrm{best}}_K \in \mathrm{argmin}_{\phi \in \left\{\phi_k \right\}^K_{k=1}} \ell_{\mathrm{sub}}(\phi)$$ を出力

## <a id="S2">2</a>. 混合整数線形計画における目的関数の重みと制約条件の閾値を高速に解くアルゴリズムを提案したこと [[13](#K13)]

### [[13](#K13)]の概要

||詳細|
|---|---|
|背景|混合整数線形計画において，観測データから目的関数や制約条件を学習するデータ駆動型逆最適化は，電力システムやスケジューリングなど様々な分野で適切な数理モデルを構築する上で，重要な役割を果たしている．|
|課題|先行研究においてその両方を学習する方法は報告されていない．|
|提案手法|目的関数が関数の線形和，制約条件が関数と閾値で表される問題のクラスに対して，まず制約条件を学習し，そして目的関数を学習する二段階アプローチを提案する．|4
|理論結果1|理論面では，有限データにおいて提案手法によって逆最適化問題を解けることの理論保証|
|理論結果2|擬距離空間と劣Gaussにおける統計的学習理論の展開|
|理論結果3|逆最適化の統計的学習理論の構築を行う．| 
|実験結果|実験面では，決定変数が$100$ 個の整数線形計画であるスケジューリングにおいてでも学習できることを実証する．|

このうち，逆最適化問題の定式化と，提案手法，理論結果1に関して解説する．

### (準備) 逆最適化
順問題の目的関数が区分線形関数の線形和で書けている，混合整数線形計画を含む，基本的かつ重要な問題を取り上げる．

- **状態空間** $\mathcal{S}$: 空でない集合

- **決定変数の集合** $\mathcal{X}$: 空でない $\mathbb{R}^{k}$ の部分集合

- **確率単体**  
  $$\Delta^{d-1} := \left\{ \phi = (\phi_i)_i \in \mathbb{R}_{\geq 0}^d \,\middle|\, \sum_i \phi_i = 1 \right\}$$

- **(特徴量)関数** $f_i \colon \mathcal{X} \times \mathcal{S} \to \mathbb{R}$ ($i = 1, \ldots, d$): 区分線形関数

- **制約条件のラベルの数** $J$: 整数

- **制約条件の閾値集合** $\Phi$: 空でない $\mathbb{R}^J$ の部分集合

- **制約条件の関数** $h_j \colon \mathcal{X} \times \mathcal{S} \to \mathbb{R}$ ($j = 1, \ldots, J$)

- **制約条件の写像**  
  $$h := (h_1, \ldots, h_J)$$

- **実行可能集合** $\mathcal{X}(\phi, s)$ (制約条件の閾値 $\phi \in \Phi$, 状態 $s \in \mathcal{S}$ に対して): 
  $$\mathcal{X}(\phi, s) := \left\{ x \in \mathcal{X} \,\middle|\, h(x, s) \leq \phi \right\}$$

$\theta \in \Delta^{d-1}$, $\phi \in \Phi$, $s \in \mathcal{S}$に対し，
順問題を

$$
\begin{equation}
    x^* (\theta, \phi, s)
    \in
    \mathbf{FOP} (\theta, \phi, s)
    :=
    \mathrm{arg max}_{x \in \mathcal{X} (\phi , s)} 
        \theta^{\top} f (x,s)
    \tag{2.1}
%    \label{eq:FOP_linear}
\end{equation}
$$

となる$x^*$を求めることである．最適解のデータ$\hat{x } \colon \mathcal{S} \to \mathcal{X}$が与えられたとき，データ駆動型逆最適化問題(data driven inverse optimization problem, DDIOP)を，以下を満たす$\theta \in \Delta^{d-1}$，$\phi \in \Phi$を求めることである：
任意の状態$s \in \mathcal{S} $に対し，

$$
\begin{equation}
    \hat{x} (s) \in \mathbf{FOP} (\theta, \phi , s)
    .
    \tag{2.2}
%    \label{eq:IOP_linear}
\end{equation}
$$

順問題と逆最適化問題の違い,
- $$\mathcal{X}(\phi, s) := \left\{ x \in \mathcal{X} \,\middle|\, h(x, s) \leq \phi \right\}$$
- $\mathbf{FOP} (\theta, \phi, s)
    :=
    \mathrm{arg max}_{x \in \mathcal{X} (\phi , s)} 
        \theta^{\top} f (x,s)$
- $$\hat{x} (s) \in \mathbf{FOP} (\theta, \phi , s)$$

に関して，以下の表を得る．

|順問題(最適化)||逆問題(逆最適化問題)|
|---|---|---|
|$\theta, \phi,s,f,h$|既知|$\hat{x} (s),s,f,h$|
|$\hat{x} (s)$|未知|$\theta, \phi$|


### (準備) 逆最適化の指標

逆最適化問題(式(2.2))が解けたことを判定する指標，suboptimality損失関数を導入する．ReLU関数を$u \in \mathbb{R}$に対して$$\mathrm{ReLU} (u) := \max (u , 0)$$とする．
定数$\lambda \in \mathbb{R}_{\geq 0 }$とする．
Suboptimality損失$$\ell^{\mathrm{sub}, \lambda} \colon \mathcal{X} \times \widetilde{\Theta} \times \mathcal{S} \to \mathbb{R}_{\geq 0 } $$ ([[Ren et. al. 2025](ren2025inverse), [13](#K13)])を

$$
\begin{equation*}
    \begin{split}
        \ell^{\mathrm{sub}, \lambda} \left( x, \theta ,\phi ,s \right) 
        & := 
        \mathrm{ReLU} \left(
                \max_{x^{\star} \in \mathcal{X} ( \phi , s )}
                \theta^{\top} f (x^{\star},s) 
            - \theta^{\top} f (x,s)
        \right)
        \\
        & \quad \quad + \lambda \sum_{j=1}^J
            \mathrm{ReLU} \left( 
                h_j ( x , s) - \phi_j
            \right)
    \end{split}
\end{equation*}
$$

で定義する．

Suboptimality損失は以下の性質を持つ：
> **命題**
> 定数$\lambda > 0 $とする．$x \in \mathcal{X}$とする，このとき，$x \in \mathrm{FOP}( \theta , \phi , s)$であることと$\ell^{\mathrm{sub}, \lambda} ( x, \theta , \phi ,s ) = 0$は同値である．

### 提案手法

逆最適化問題を解くアルゴリズムとして，以下を提案する．

> **<a id="alg:2.1">アルゴリズム2.1</a>**: Maximizing feasible set then minimizing suboptimality loss [アルゴリズム2, [13](#K13)]
> 1. $\varepsilon \geq 0 $をとる．
> 2. $$\phi^{\sup}:= \min_{\phi \in \Phi} \{ \phi | h ( x^{*} (s) , s) \leq \phi \text{ for } s \in \mathcal{S}^\prime \} $$
> 3. 以下を満たす$\theta^{\sup}\in \Delta^{d-1}$を計算する： $$
    \mathbb{E}_{S} \ell^{\mathrm{sub}, 0} ( \hat{x}^{*} (S), \theta^{\sup} , \phi^{\sup} ,S ) \leq \varepsilon$$
> 4. $\theta^{\sup} \in \Delta^{d-1}, \phi^{\sup} \in \Phi$を出力

**注釈 2.2**: [アルゴリズム 2.1](#alg:2.1)の3行目を実装する方法として，[[8](#K8),アルゴリズム 1]が挙げられる．

### (理論結果1) 模倣性に関する理論

[アルゴリズム 2.1](#alg:2.1)によって，逆最適化(式(2.2))が解ける，つまり，以下の定理が成り立つ．

> **定理**
> 写像$\hat{x} \colon \mathcal{S} \to \mathcal{X}$が最適解写像であるとは，ある$\theta^{\mathrm{true}} \in \Delta^{d-1}$, $\phi^{\mathrm{true}} \in \Phi $が存在して，任意の$s \in \mathcal{S}$に対して$\hat{x} (s) = x^* (\theta^{\mathrm{true}} , \phi^{\mathrm{true}} , s)$となるものとする．$\varepsilon = 0 $とする．
> このとき，ほとんど至る$\theta^{\mathrm{true}} \in \Delta^{d-1}$に対して，[アルゴリズム 2.1](#alg:2.1)の3行目に[[8](#K8),アルゴリズム 1]を組みこんだ[アルゴリズム 2.1](#alg:2.1)で出力された，$\theta^{\sup}, \phi^{\sup}$は$\hat{x}^{*} (s ) \in \mathbf{FOP} (\theta^{\sup}, \phi^{\sup}, s)$，つまり，式(2.2)を満たす．

# 参考文献

[<a id="K5">5</a>] Akira Kitaoka, Riki Eto, A proof of convergence of inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.06137](https://arxiv.org/abs/2305.06137)]

[<a id="K8">8</a>] Akira Kitaoka, A fast algorithm to minimize prediction loss of the optimal solution in inverse optimization problem of MILP, preprint.
[[arXiv:2405.14273](https://arxiv.org/abs/2405.14273)]


[<a id="K13">13</a>]  Akira Kitaoka, Inverse Mixed-Integer Programming: Learning Constraints then Objective Functions, preprint.
[[arXiv:2510.04455](https://arxiv.org/abs/2510.04455)]

[<a id="ren2025inverse">Ren et. al. 2025</a>] Ren, K., Esfahani, P. M., and Georghiou, A. (2025). Inverse optimization via learning feasible regions. In The 42nd International Conference on Machine Learning. to be appeared. [[arXiv:2505.15025](https://arxiv.org/abs/2505.15025)]

[<a id="sakaue2025online">Sakaue et. al. 2025</a>] S Sakaue, T Tsuchiya, H Bao, T Oki, Online Inverse Linear Optimization: Improved Regret Bound, Robustness to Suboptimality, and Toward Tight Regret Analysis, NeurIPS 2025, to be appeared.
[[arXiv:2501.14349](https://arxiv.org/abs/2501.14349)]




<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>