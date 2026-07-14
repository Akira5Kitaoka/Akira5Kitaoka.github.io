<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>

更新日：2026年05月31日

# 混合整数計画の逆最適化問題

**キーワード**: 逆最適化，数理最適化，組合せ最適化

**研究内容**：与えられたデータが最適解になるような，(混合整数)線形計画の目的関数の重みと制約条件の閾値を推定する高速なアルゴリズムを開発した．

**背景**: 最適化問題は，人間の意思決定から自然現象に至るまで，さまざまなプロセスやシステムの順方向モデルとして定義することが多い．しかし，そのようなモデルにおける真の目的関数や制約条件が，事前にはほとんど分かっていないのが実情である．したがって，観測された最適解から目的関数や制約条件を推定する，すなわち**逆最適化**の問題は，実践的に非常に重要である．逆最適化は，地球物理学，交通，電力システム，スケジューリング，医療への応用があり，また，逆強化学習，コントラスト学習などをはじめとするさまざまな機械学習の基礎としても発展している．(cf. [[Sakaue et al. 2025](#sakaue2025online), [P13](#K13)])
<!-- 
この分野における初期の研究は地球物理学から登場し、地震波データから地下構造を推定することを目的としていた \citep{Tarantola1988-tq,Burton1992-dc}。  
その後、逆最適化は広く研究されるようになり \citep{Ahuja2001-cv,Heuberger2004-zv,Chan2019-zg,Chan2023-qk}、交通 \citep{Bertsimas2015-kw}、電力システム \citep{Birge2017-il}、医療 \citep{Chan2022-uq} などのさまざまな分野に応用されてきた。  
さらに、逆強化学習 \citep{Ng2000-sf} やコントラスト学習 \citep{Shi2023-nd} をはじめとするさまざまな機械学習手法の基礎としても発展している。
-->

そこで，北岡がかかわった研究は以下である．(独立に読めます．)
- [1](#S1). 混合整数線形計画における目的関数の重みを高速に解くアルゴリズムを提案したこと [[P5](#K5), [P8](#K8)]

- [2](#S2). 混合整数線形計画における目的関数の重みと制約条件の閾値を高速に解くアルゴリズムを提案したこと [[P13](#K13)]

## <a id="S1">1</a>. 混合整数線形計画における目的関数の重みを高速に解くアルゴリズムを提案したこと[[P5](#K5), [P8](#K8)]

### [[P8](#K8)]の概要

||詳細|
|---|---|
|背景|混合整数線形計画(MILP)において，観測された最適解データと整合する目的関数(重み)を推定するデータ駆動型逆最適化問題(DDIOP)は，電力システムやスケジューリングなど様々な分野で適切な数理モデルを構築する上で，重要な役割を果たしている．|
|課題|評価指標である特徴量の予測誤差(PLF)は重みに関して不連続であり，勾配ベース最適化を適用できない．代替指標であるsuboptimality損失はLipschitz連続かつ凸であるが，既存研究の多くは反復回数$T\to\infty$での漸近的な収束保証にとどまり，有限回の反復で厳密に($\ell_{\mathrm{sub}}=0$)解けるかは整理されていなかった．素朴な点群探索(UPA/RPA)は次元$d$の影響を強く受ける．|
|提案手法|suboptimality損失に対し，射影劣勾配法(PSGD)を含む広いクラスの勾配ベース最適化手法を適用する．|
|理論結果1 (有限回での厳密可解性)|(A)過去の反復点列と劣勾配のみで更新され，(B)Lipschitz凸関数に対して標準的な収束保証をもつ勾配ベース最適化手法は，有限回の反復で$\ell_{\mathrm{sub}}=0$に到達し，MILPに関するDDIOPを厳密に解けることを示す．|
|理論結果2 (反復回数の具体化)|PSGDの代表的なステップサイズ則に対し，定数$\gamma(\ell_{\mathrm{sub}})$を介して有限回到達までに要する反復回数の上界を与える．|
|理論結果3 (派生結果)|PSGDが有限回の反復でPLFの最小値$0$に到達することを示す．|
|実験結果|人工データ(LP)とスケジューリング問題において，既存手法の$1/7$(LP)・$1/10$(スケジューリング)未満の反復回数でPLFを最小化し，$100\%$の確率でPLF$=0$を達成することを実証する．|

### (準備) 問題設定
順問題の目的関数が区分線形関数の線形和で書けている，混合整数線形計画(MILP)を含む，基本的かつ重要な問題を取り上げる．
決定変数の集合$\mathcal{X} \subset \mathbb{R}^{d_{\mathcal{X}}}$，状態の集合$\mathcal{S}$とする．各状態$s \in \mathcal{S}$に対し，実行可能領域$X(s) \subset \mathcal{X}$を有界閉凸多面体の有限合併集合とする．
各$i = 1 , \ldots ,d $に対し，$f_i \colon \mathcal{X} \to \mathbb{R}$は区分線形であるとし，$f = (f_1 , \ldots , f_d )$とする．
重みの空間$\Theta \subset \mathbb{R}^d$を有界閉凸集合とし，特に確率単体
$$\Delta^{d-1} := \{ \theta \in \mathbb{R}_{\geq 0 }^d \mid \sum_i \theta_i =1 \} $$
を考える．
順問題(forward optimization problem, FOP)とその最適解の特徴量を以下で定義する．

$$
    x^* (\theta , s ) \in \mathrm{argmax}_{x \in X (s)} \theta^{\top} f ( x ) \left( 
        = \sum_{i=1}^d \theta_i f_i (x) \right)
        , 
    \quad a^* (\theta , s ) := f (x^*(\theta , s ))
        .
$$

順問題の逆最適化問題を定義する．
データ$\mathcal{D} = \\{ (s^{(n)} , x^{(n)} ) \\}_{n=1}^N$には未知の真の重み$\theta^* \in \Theta$が存在し，

$$
   s^{(n)} \in \mathcal{S} , 
   \quad x^{(n)} = x^* ( \theta^* , s^{(n)} )
   %\label{assu:data-follows-solver}  
$$

を満たすとする．$a^{(n)} := f (x^{(n)})$とおく．
このデータ駆動型逆最適化問題(DDIOP)とは，観測された最適解と整合する重み$\theta \in \Theta$，すなわち任意の$n = 1 , \ldots , N$に対して

$$
    x^{(n)} \in \mathrm{argmax}_{x \in X (s^{(n)})} \theta^{\top} f ( x )
$$

を満たす$\theta$を求める問題である．
DDIOPが解けたかを測る指標として，特徴量の予測誤差(prediction loss of features, PLF)

$$
    \ell_{\mathrm{plf}} (\theta) := \frac{1}{N}\sum_{n=1}^N \| a^* (\theta , s^{(n)} ) - a^{(n)} \|_2^2
$$

がある．$\ell_{\mathrm{plf}} (\theta) = 0$ならばDDIOPが解けたことを意味する．

### (準備) スケジューリング問題から見る順問題と逆問題

スケジューリングを例に，順問題を定式化するための課題と，その課題を解く方法として，逆最適化問題によって，何を解決しているのかを解説する．

スケジューリングを数理計画で定式化することは，目的関数と制約条件をモデリングすることである．しかし，目的関数をどう設計するか，という問題が生じる．

(1) 目的関数を決定する際に，目的関数の候補が一つだけというのは稀で，実際は複数の候補がある．たとえば，スケジューリングにおける目的関数の候補の例として，
- 休暇希望の反映度合い，
- 業務負荷の平準化度，
- 人件費

などが挙げられる[[P5](#K5)]．そこで，目的関数を決定する方法として，複数の目的関数の候補を割合$\theta \in \Theta$を決めて足し合わせることが考えられる．

さて，逆最適化によって，目的関数の設計に役立つことをスケジューリングを通して解説する．スケジューリングのデータ$\{ (s^{(n)}, a^{(n)}) \}$を用いて，スケジューリングにおける逆最適化問題を解くとは，数理モデル$\mathrm{FOP}$があたえられたとき，データ$\{ (s^{(n)}, a^{(n)}) \}$を復元するような，目的関数の割合$\theta \in \Theta$を決定することを意味する．具体例を通して解説すると，逆最適化は，データ$\{ (s^{(n)}, a^{(n)}) \}$を再現するような，(1)で出た目的関数の候補の例に関して割合$\theta \in \Theta$を決定することである．

### 課題
PLFは重み$\theta$に関して不連続であるため，勾配ベース最適化手法を直接適用できない．DDIOPを解く素朴な方法として，確率単体$\Delta^{d-1}$から均一に点群を取る方法(UPA)やランダムに点群を取る方法(RPA)が挙げられる．しかし真の重み$\theta^*$と点群との距離は，点群数を$T$とすると，それぞれ$O(T^{-1/(d-1)})$，$O\left(\left( \frac{\log T}{T} \right)^{1/(d-1)}\right)$で評価され，次元$d$が大きくなるにつれ急速に悪化する．

一方，DDIOPが解けたかを判定できるLipschitz連続かつ凸な損失として後述のsuboptimality損失$\ell_{\mathrm{sub}}$があり，これにオンライン最適化などの一次法を適用できる．しかし既存研究の多くは「$T\to\infty$で$\ell_{\mathrm{sub}}$が$0$に近づく」漸近的な収束保証にとどまり，**有限回の反復で厳密に$\ell_{\mathrm{sub}}=0$を達成できるか**は整理されていなかった．

| 手法 | 典型的な保証 | 有限回で$\ell_{\mathrm{sub}}=0$ |
|------|------|------|
| 点群探索 UPA/RPA | カバリング誤差に基づく近似（次元依存が強い） | No |
| オンライン最適化 (MWU/ONS/MetaGrad 等) | $\ell_{\mathrm{sub}}$の漸近的減少（例 $O(1/\sqrt{T})$, $O(\log T/T)$） | No |
| 本研究（勾配ベース）[[P8](#K8)] | 凸・区分線形＋内点性により有限回で$0$を保証 | **Yes** |

### 提案手法  

PLFが不連続である一方，与えられた解を模倣するための代替指標である
suboptimality損失

$$
    \ell_{\mathrm{sub}} (\theta) := \frac{1}{N} \sum_{n=1}^N \left( \theta^{\top} a^* (\theta , s^{(n)} ) - \theta^{\top} a^{(n)} \right) 
$$

はLipschitz連続かつ凸であり，劣勾配が

$$
    g (\theta) = \frac{1}{N}
    \sum_{n=1}^N 
    \left(
        a^* ( \theta , s^{(n)} )
        -
        a^{(n)}
    \right)    
$$

で与えられる[命題3.1, [P8](#K8)]．DDIOPが解けることと$\ell_{\mathrm{sub}}(\theta) = 0$となることは同値である．したがって，$\Delta^{d-1}$の上で suboptimality損失を最小化すればよい．

提案手法は，suboptimality損失に対して，劣勾配情報$g$を用いて$\theta$を反復更新する勾配ベース最適化手法を適用するものである([アルゴリズム1](#alg:1))．更新則$\mathrm{update}_t$には，射影劣勾配法(projected subgradient descent, PSGD)
$$\mathrm{update}_t = \mathrm{Proj}_{\Theta} \left( \theta^t - \alpha_t g(\theta^t) \right)$$
などを用いる．学習率$\alpha_t$には，非加算的ステップサイズ(NSS)・ステップ長(NSL)，特に$\beta t^{-1/2}$型のSRSS/SRSL，最小値が既知の場合のPolyakなどがある．


> **<a id="alg:1">アルゴリズム1</a>**: suboptimality損失最小化 [アルゴリズム1, [P8](#K8)]
> 1. $\theta^1 \in \Theta$ で初期化
> 2. For $t = 1, \ldots, T-1$:
> 3. &nbsp;&nbsp;&nbsp;&nbsp; 各$n$に対し $x^*(\theta^t, s^{(n)})$ を解く
> 4. &nbsp;&nbsp;&nbsp;&nbsp; $$\theta^{t+1} \leftarrow \mathrm{update}_t \left( \{ \theta^{t^\prime} \}_{t^\prime=1}^t \mid \ell_{\mathrm{sub}}, g \right)$$
> 5. End For
> 6. $$\theta^{\mathrm{best}}_T \in \mathrm{argmin}_{\theta \in \left\{\theta^t \right\}^T_{t=1}} \ell_{\mathrm{sub}}(\theta)$$ を出力

### 主結果（有限回での厳密可解性）

勾配ベース最適化手法が，(A)過去の反復点列と劣勾配のみで更新され，(B)任意のLipschitz凸関数に対して最良反復に標準的な収束保証$Q$をもつとする．このとき正定数$\gamma(\ell_{\mathrm{sub}})$が存在し，$T \geq Q(\gamma(\ell_{\mathrm{sub}}))$ ならば $\min_{t = 1, \ldots, T}\ell_{\mathrm{sub}}(\theta^t)=0$，すなわちMILPに関するDDIOPを厳密に解ける[主定理, [P8](#K8)]．

特にPSGD(SRSS/SRSL)では$Q$を具体的に構成でき，有限回到達までに要する反復回数の上界（概ね$O(\gamma(\ell_{\mathrm{sub}})^{-2})$オーダー）を与えられる．また派生結果として，PSGD(SRSS/SRSL)は有限回の反復でPLFの最小値$0$にも到達する．

### 証明のアイデア（凸＋区分線形＋内点性）

suboptimality損失は，(i)凸かつ区分線形であり，(ii)最小値が$0$であり，(iii)（ほとんど至る真の重み$\theta^*$で）最小値集合が内点をもつ．区分線形凸関数で最小値集合が内点をもつとき，最小値集合の内部だけ値を押し下げた凸区分線形関数$\widetilde{\ell}$を構成でき，最小値集合の外では$\ell_{\mathrm{sub}}=\widetilde{\ell}$となる．よって最小値集合に入るまで両者に対する一次法の反復は一致する．$\gamma:=\min \ell_{\mathrm{sub}} - \min\widetilde{\ell}>0$とおくと，$\widetilde{\ell}$への標準的な収束保証から有限回で$\widetilde{\ell}(\theta^T)\le \min\widetilde{\ell}+\gamma=\min\ell_{\mathrm{sub}}$となり，反復の一致から同じ$T$で$\ell_{\mathrm{sub}}(\theta^T)=0$に到達する．

### 効果  

提案手法[[P5](#K5), [P8](#K8)]は，推定困難な正定数$\gamma(\ell_{\mathrm{sub}})$に反復回数が依存するものの，UPA・RPA・オンライン最適化と異なり，**有限回の反復でDDIOPを厳密に解ける**点が新しい．数値実験では，PSGD(SRSL)が既知手法(UPA/RPA/CHAN)の$1/7$(LP)・$1/10$(スケジューリング)未満の反復でPLFを最小化し，$100\%$の確率でPLF$=0$を達成することを確認した．

## <a id="S2">2</a>. 混合整数線形計画における目的関数の重みと制約条件の閾値を高速に解くアルゴリズムを提案したこと [[P13](#K13)]

### [[P13](#K13)]の概要

||詳細|
|---|---|
|背景|混合整数線形計画(MILP)において，観測データから目的関数や制約条件を学習するデータ駆動型逆最適化問題(DDIOP)は，電力システムやスケジューリングなど様々な分野で適切な数理モデルを構築する上で，重要な役割を果たしている．|
|課題|線形計画では両方を学習する研究があるが，MILPに関するDDIOPで目的関数と制約条件の両方を学習する方法は先行研究で報告されていない．|
|提案手法|目的関数が特徴量の線形和，制約条件が関数と閾値で表される問題のクラスに対して，まず制約条件を学習し，次に目的関数を学習する二段階アプローチを提案する．|
|理論結果1 (模倣性)|有限分布の下で，提案手法が観測された最適解を厳密に再現できること（模倣性）を保証する．|
|理論結果2 (擬距離への拡張)|逆最適化で自然に現れるパラメータ間の距離が擬距離となることに着目し，劣Gauss仮定下の統計的学習理論（被覆数解析）を距離空間から擬距離空間へ拡張する．|
|理論結果3 (汎化誤差)|上記を用いて，逆最適化（suboptimality損失最小化）の汎化誤差境界を導出する．|
|実験結果|決定変数が$100$個（$D=10$）の整数線形計画であるスケジューリングでも，観測解を学習後の最適解として再現できる（DDIOPを解ける）ことを実証する．|

このうち，逆最適化問題の定式化と，提案手法，理論結果1に関して解説する．

### (準備) 逆最適化
順問題の目的関数が区分線形関数の線形和で書けている，混合整数線形計画を含む，基本的かつ重要な問題を取り上げる．

- **状態空間** $\mathcal{S}$: 空でない集合

- **決定変数の集合** $\mathcal{X}$: 空でない $\mathbb{R}^{k}$ の部分集合

- **目的関数のパラメータ集合** $\Theta$: 空でない $\mathbb{R}^{D}$ の有界閉凸部分集合

- **確率単体**  
  $$\Delta^{D-1} := \left\{ \theta = (\theta_i)_i \in \mathbb{R}_{\geq 0}^D \,\middle|\, \sum_i \theta_i = 1 \right\}$$

- **(特徴量)関数** $f_i \colon \mathcal{X} \times \mathcal{S} \to \mathbb{R}$ ($i = 1, \ldots, D$): 区分線形関数

- **制約条件のラベルの数** $J$: 整数

- **制約条件の閾値集合** $\Phi$: 空でない $\mathbb{R}^J$ の部分集合

- **制約条件の関数** $h_j \colon \mathcal{X} \times \mathcal{S} \to \mathbb{R}$ ($j = 1, \ldots, J$)

- **制約条件の写像**  
  $$h := (h_1, \ldots, h_J)$$

- **実行可能集合** $\mathcal{X}(\phi, s)$ (制約条件の閾値 $\phi \in \Phi$, 状態 $s \in \mathcal{S}$ に対して): 
  $$\mathcal{X}(\phi, s) := \left\{ x \in \mathcal{X} \,\middle|\, h(x, s) \leq \phi \right\}$$

$\theta \in \Theta $, $\phi \in \Phi$, $s \in \mathcal{S}$に対し，
順問題を

$$
    x^* (\theta, \phi, s)
    \in
    \mathbf{FOP} (\theta, \phi, s)
    :=
    \mathrm{arg max}_{x \in \mathcal{X} (\phi , s)} 
        \theta^{\top} f (x,s)
    \tag{2.1}
$$

となる$x^*$を求めることである．最適解のデータ$\hat{x } \colon \mathcal{S} \to \mathcal{X}$が与えられたとき，データ駆動型逆最適化問題(data driven inverse optimization problem, DDIOP)を，以下を満たす$\theta \in \Theta$，$\phi \in \Phi$を求めることである：
任意の状態$s \in \mathcal{S} $に対し，

$$
    \hat{x} (s) \in \mathbf{FOP} (\theta, \phi , s)
    .
    \tag{2.2}
$$

順問題と逆最適化問題の違いについて，
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
|最適解$\hat{x} (s)$|未知|目的関数の割合$\theta$，制約条件の閾値$\phi$|


### (準備) スケジューリング問題から見る順問題と逆問題

スケジューリングを例に，順問題を定式化するための課題と，その課題を解く方法として，逆最適化問題によって，何を解決しているのかを解説する．

スケジューリングを数理計画で定式化することは，目的関数と制約条件をモデリングすることである．しかし，(1) 目的関数をどう設計するか，(2) 制約条件をどう設計するか，という問題が生じる．

(1) 目的関数を決定する際に，目的関数の候補が一つだけというのは稀で，実際は複数の候補がある．たとえば，スケジューリングにおける目的関数の候補の例として，
- 休暇希望の反映度合い，
- 業務負荷の平準化度，
- 人件費

などが挙げられる[[P5](#K5)]．そこで，目的関数を決定する方法として，複数の目的関数の候補を割合$\theta \in \Theta$を決めて足し合わせることが考えられる．


(2) 制約条件を決定する際に，制約条件の適切な閾値は何か，もしくは，真に必要な制約条件は何かを考える必要がある．これらの課題を解決する方法として，制約テンプレートを用いる方法がある[[Suenaga et al. 2024](#suenaga2024kaigoshi)]．制約テンプレートの例として，
- 連続勤務日数の数，割合$\phi_1$，
- 夜勤入りと夜勤明けを連続する$\phi_2$日数以下で週に割り当てる
- 特定の勤務パターン(夜勤->休->夜勤)を禁止しているか？しているなら，$\phi_3 = 0$, していないなら$\phi_3 = 1$,
- 各シフト$\tau$に対して，必要な最低人数$\phi_{4,\tau}$，最高人数$\phi_{5,\tau}$

などが挙げられる．制約条件の閾値$\phi \in \Phi$を決定することで，制約条件を決定することができる．

ちなみに，制約条件においては，法律や物理的制約などが理由で，閾値を決める必要がないものがある．たとえば，
- 労働基準法
- 一日24時間
- マルチタスク不可

などである．

さて，逆最適化によって，目的関数と制約条件の設計に役立つことをスケジューリングを通して解説する．スケジューリングのデータ$\hat{x} \colon \mathcal{S} \to \mathcal{X}$を用いて，スケジューリングにおける逆最適化問題を解くとは，数理モデル$\mathrm{FOP}$があたえられたとき，データ$\hat{x}$を復元するような，目的関数の割合$\theta \in \Theta$, 制約条件の閾値$\phi \in \Phi$を決定することを意味する．具体例を通して解説すると，逆最適化は，データ$\hat{x}$を再現するような，(1)で出た目的関数の候補の例に関して割合$\theta \in \Theta$や，(2)で出た制約テンプレートの閾値$\phi \in \Phi$を決定することである．



### (準備) 逆最適化の指標

逆最適化問題(式(2.2))が解けたことを判定する指標，suboptimality損失関数を導入する．ReLU関数を$u \in \mathbb{R}$に対して$$\mathrm{ReLU} (u) := \max (u , 0)$$とする．
定数$\lambda \in \mathbb{R}_{\geq 0 }$とする．
Suboptimality損失$$\ell^{\mathrm{sub}, \lambda} \colon \mathcal{X} \times \Theta \times \Phi \times \mathcal{S} \to \mathbb{R}_{\geq 0 } $$ ([[Ren et al. 2025](#ren2025inverse), [P13](#K13)])を

$$
\begin{aligned}
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
\end{aligned}
$$

で定義する．

Suboptimality損失は以下の性質を持つ：
> **命題 2.1**
> 定数$\lambda > 0 $とする．$x \in \mathcal{X}$とする．このとき，$x \in \mathrm{FOP}( \theta , \phi , s)$であることと$\ell^{\mathrm{sub}, \lambda} ( x, \theta , \phi ,s ) = 0$は同値である．

### 提案手法

$\Theta = \Delta^{D-1}$とする．
逆最適化問題を解くアルゴリズムとして，以下を提案する．

> **<a id="alg:2.1">アルゴリズム2.1</a>**: Maximizing feasible set then minimizing suboptimality loss [アルゴリズム2, [P13](#K13)]
> 1. $\varepsilon \geq 0 $をとる．
> 2. $$\phi^{\sup}:= \min_{\phi \in \Phi} \{ \phi \mid h ( \hat{x} (s) , s) \leq \phi \text{ for } s \in \mathcal{S} \} $$
> 3. 以下を満たす$\theta^{\sup}\in \Delta^{D-1}$を計算する： $$
    \mathbb{E}_{S} \ell^{\mathrm{sub}, 0} ( \hat{x} (S), \theta^{\sup} , \phi^{\sup} ,S ) \leq \varepsilon$$
> 4. $\theta^{\sup} \in \Delta^{D-1}, \phi^{\sup} \in \Phi$を出力

**注釈 2.2**: [アルゴリズム 2.1](#alg:2.1)の3行目を実装する方法として，[[P8](#K8),アルゴリズム 1]が挙げられる．

### (理論結果1) 模倣性に関する理論

[アルゴリズム 2.1](#alg:2.1)によって，逆最適化(式(2.2))が解ける，つまり，以下の定理が成り立つ．

> **定理 2.3**
> 写像$\hat{x} \colon \mathcal{S} \to \mathcal{X}$が最適解写像であるとは，ある$\theta^{\mathrm{true}} \in \Theta = \Delta^{D-1}$, $\phi^{\mathrm{true}} \in \Phi $が存在して，任意の$s \in \mathcal{S}$に対して$\hat{x} (s) = x^* (\theta^{\mathrm{true}} , \phi^{\mathrm{true}} , s)$となるものとする．$\varepsilon = 0 $とする．
> このとき，ほとんど至る$\theta^{\mathrm{true}} \in \Delta^{D-1}$に対して，[アルゴリズム 2.1](#alg:2.1)の3行目に[[P8](#K8),アルゴリズム 1]を組みこんだ[アルゴリズム 2.1](#alg:2.1)で出力された，$\theta^{\sup}, \phi^{\sup}$は$\hat{x} (s) \in \mathbf{FOP} (\theta^{\sup}, \phi^{\sup}, s)$，つまり，式(2.2)を満たす．

# 参考文献

[<a id="K5">P5</a>] Akira Kitaoka, Riki Eto, A proof of convergence of inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.06137](https://arxiv.org/abs/2305.06137)]

[<a id="K8">P8</a>] Akira Kitaoka, A fast algorithm to minimize prediction loss of the optimal solution in inverse optimization problem of MILP, preprint.
[[arXiv:2405.14273](https://arxiv.org/abs/2405.14273)]


[<a id="K13">P13</a>]  Akira Kitaoka, Inverse Mixed-Integer Programming: Learning Constraints then Objective Functions, preprint.
[[arXiv:2510.04455](https://arxiv.org/abs/2510.04455)]

[<a id="ren2025inverse">Ren et al. 2025</a>] Ren, K., Esfahani, P. M., and Georghiou, A. (2025). Inverse optimization via learning feasible regions. In The 42nd International Conference on Machine Learning. to appear. [[arXiv:2505.15025](https://arxiv.org/abs/2505.15025)]

[<a id="sakaue2025online">Sakaue et al. 2025</a>] S Sakaue, T Tsuchiya, H Bao, T Oki, Online Inverse Linear Optimization: Improved Regret Bound, Robustness to Suboptimality, and Toward Tight Regret Analysis, NeurIPS 2025, to appear.
[[arXiv:2501.14349](https://arxiv.org/abs/2501.14349)]

[<a id="suenaga2024kaigoshi">Suenaga et al. 2024</a>] 末永 康貴, 永井 裕也, 柏木 一杜, 小野 智司, 介護士スケジューリングにおける制約条件の自動抽出の試み, 人工知能学会全国大会論文集, 2024, JSAI2024 巻, 第38回 (2024), セッションID 2M1-OS-11a-04, p. 2M1OS11a04.

<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>