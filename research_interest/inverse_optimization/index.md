<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>

更新日：2025年12月22日

# 混合整数計画問題の逆最適化問題

**キーワード**: 逆最適化，数理最適化，組合せ最適化

**研究内容**：与えられたデータが最適解になるような，(混合整数)線形計画法の目的関数の重みを推定する高速なアルゴリズムを開発しています．

**背景**:　最適化問題は，人間の意思決定から自然現象に至るまで，さまざまなプロセスやシステムの順方向モデルとして定義することが多いです．しかし，そのようなモデルにおける真の目的関数や制約条件が，事前にはほとんど分かっていないのが実情である．したがって，観測された最適解から目的関数や制約条件を推定する，すなわち**逆最適化**の問題は，実践的に非常に重要である．逆最適化は，地球物理学，交通，電力システム，スケジューリング，医療への応用があり，また，逆強化学習，コントラスト学習などをはじめとするさまざまな機械学習の基礎としても発展している．(cf. [[Sakaue et. al. 2025](#sakaue2025online), [13](#K13)])
<!-- 
この分野における初期の研究は地球物理学から登場し、地震波データから地下構造を推定することを目的としていた \citep{Tarantola1988-tq,Burton1992-dc}。  
その後、逆最適化は広く研究されるようになり \citep{Ahuja2001-cv,Heuberger2004-zv,Chan2019-zg,Chan2023-qk}、交通 \citep{Bertsimas2015-kw}、電力システム \citep{Birge2017-il}、医療 \citep{Chan2022-uq} などのさまざまな分野に応用されてきた。  
さらに、逆強化学習 \citep{Ng2000-sf} やコントラスト学習 \citep{Shi2023-nd} をはじめとするさまざまな機械学習手法の基礎としても発展している。
-->

そこで，北岡がかかわった研究は以下である．
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
確率単体$\Delta^{d-1} := \\{ \phi \in \mathbb{R}_{\geq 0 }^d | \sum_i \phi_i =1 \\} $
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


| 手法 | $\| \phi_k - \phi^* \|$ ※学習未完了の場合 |
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
    \ell_{\mathrm{sub}} (\phi_k ) / \| g (\phi_k ) \|^2  ,
$
としたものをPSGDP,
学習率を
$
    \alpha_k = k^{-1/2}
    /
        \left\|
         g (\phi_k)
        \right\|
$
としたものをPSGD2とする．


<a id="alg:1">アルゴリズム1</a>: suboptimality損失最小化 [アルゴリズム1, [5](#K5)]
1. $\phi_1 = \left(\frac{1}{d}, \ldots, \frac{1}{d}\right) \in \Delta^{d-1}$ で初期化
2. For $k = 1, \ldots, K-1$：
    1. $\phi_{k+1} \leftarrow \phi_k - \alpha_k g(\phi_k)$ を計算
    2. $\phi_{k+1}$ を $\Delta^{d-1}$ へ射影する
3. $\phi^{\mathrm{best}}\_K \in$ $\arg\min\_{\phi \in \left\{\phi\_k \right\}^K\_{k=1}}$ $\ell_{\mathrm{sub}}(\phi)$ を出力

## <a id="S2">2</a>. 混合整数線形計画における目的関数の重みと制約条件の閾値を高速に解くアルゴリズムを提案したこと [[13](#K13)]

工事中

<details><summary>詳細</summary>

工事中
</details>


## 参考文献

[<a id="K5">5</a>] Akira Kitaoka, Riki Eto, A proof of convergence of inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.06137](https://arxiv.org/abs/2305.06137)]

[<a id="K8">8</a>] Akira Kitaoka, A fast algorithm to minimize prediction loss of the optimal solution in inverse optimization problem of MILP, preprint.
[[arXiv:2405.14273](https://arxiv.org/abs/2405.14273)]


[<a id="K13">13</a>]  Akira Kitaoka, Inverse Mixed-Integer Programming: Learning Constraints then Objective Functions, preprint.
[[arXiv:2510.04455](https://arxiv.org/abs/2510.04455)]


[<a id="sakaue2025online">Sakaue et. al. 2025</a>] S Sakaue, T Tsuchiya, H Bao, T Oki, Online Inverse Linear Optimization: Improved Regret Bound, Robustness to Suboptimality, and Toward Tight Regret Analysis, preprint.
[[arXiv:2501.14349](https://arxiv.org/abs/2501.14349)]




<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>