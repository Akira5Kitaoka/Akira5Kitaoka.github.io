北岡がどのようなことを主に研究してきたかを説明します．

# 最短測地線近似アルゴリズム


**キーワード**: 数値解析，測地線，数理最適化，最適制御

**背景**: 最短の長さを実現する最短測地線を決定する問題は，コンピュータビジョン，ロボティクス，機械学習など，様々な分野で注目を集めている．(cf. [[10](#K10)])


## 有限差分法と数値積分を用いて長さ最小化を近似して解いたものが真の長さ最小化に漸近しない例[[10](#K10), $\S 5$]

**効果**: これは，汎関数最小化を実装するために．有限差分法と数値積分を用いて近似して解いても，真の汎関数最小化を実現できないことを意味する．

##  有限差分法と数値積分を用いた実座標空間上の曲線のエネルギー最小化による長さ最小化の誤差評価[[10](#K10)]

**背景**: 最短測地線を実装する方法として，曲線のエネルギー最小化問題を，有限差分と数値積分で近似した非線形最適化問題の解を線形補完する方法が挙げられる．この方法は，他の方法に比べて，実装が容易な傾向にある．


**従来課題**: 実座標空間のRiemann計量において，有限差分と数値積分でエネルギー最小化を近似しして解く方法が，曲線の長さを最小化できているのかは，知られていたなかった．

**効果**: 曲線のエネルギーを有限差分と台形公式で近似した最適化問題の解を線形補間した曲線の長さが，最小の曲線の長さに，数値積分で用いた点群の個数の次数$1/2$で収束することを証明した．また，左点公式を使った場合でも同様に，近似して得た曲線の長さが，最小の曲線の長さに数値積分で用いた点群の個数の次数$1/2$で収束することを証明した．

**鍵となるアイデア**: アプリオリ評価，変分原理，Sobolev空間論

## 参考文献

[<a id="K10">10</a>] Akira Kitaoka, Minimization of curve length through energy minimization using finite difference and numerical integration in real coordinate space, preprint.
[[arXiv:2504.15566](https://arxiv.org/abs/2504.15566)]

<!--
<details><summary>裏話([10]の論文ができるまで)</summary>

秘密
</details>
<br>
-->

# 曲線のエネルギー最小化による経路に基づく反実仮想説明

**キーワード**: 反実仮想説明，XAI(説明可能性AI)，機械学習，数値解析，微分幾何

**背景**: 経路に基づく反実仮想説明は，訓練された分類器から望ましい予測結果を得るために，入力特徴量から経路を提案する説明手法である．この説明手法の需要は、ローン審査や健康診断といった領域で高まっている．

## 曲線のエネルギー最小化による経路に基づく反実仮想説明[[11](#K11)](with 岡嶋 穣，佐々木 耀一，高野 凜)

**従来課題**: 経路に基づく反実仮想説明において，提案される経路がジグザグになり解釈が難しいことや，スパース性の追加が困難であることが，課題として挙げられる．

**解決手法**: [[10](#K10)]を参考に，曲線のエネルギー最小化問題の解として経路を定義し，その最適化問題を有限差分法で近似し，非線形計画問題へ帰着させる手法を提案する．

**効果**: 実験で，滑らかな経路の取得できることと，およびスパース性を確認し，具体的には従来手法の必要な特徴量を73%に抑えられることを確認する．

<!-- 
<details><summary>詳細</summary>

工事中
</details>
-->

## 参考文献

[<a id="K11">11</a>] 北岡 旦, 岡嶋 穣，佐々木 耀一，高野 凜，曲線のエネルギー最小化による経路に基づく反実仮想説明 (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/1Win4-97/advanced), (2025年5月27日), 1Win4-97, to appear.



# 混合整数計画問題の逆最適化問題

**キーワード**: 逆最適化，数理最適化，組合せ最適化

研究内容：与えられたデータが最適解になるような，(混合整数)線形計画法の目的関数の重みを推定する高速なアルゴリズムを開発しています．

**背景(逆最適化)**:　最適化問題は，人間の意思決定から自然現象に至るまで，さまざまなプロセスやシステムの順方向モデルとして定義することが多い．しかし，そのようなモデルにおける真の目的関数は，事前にはほとんど分かっていないのが実情である．したがって，観測された最適解から目的関数を推定する、すなわち **逆最適化**の問題は，実践的に非常に重要である．逆最適化は，地球物理学，交通，電力システム，医療への応用があり，また，逆強化学習，コントラスト学習などをはじめとするさまざまな機械学習の基礎としても発展している．(cf. [[Sakaue et. al. 2025](#sakaue2025online)])
<!-- 
この分野における初期の研究は地球物理学から登場し、地震波データから地下構造を推定することを目的としていた \citep{Tarantola1988-tq,Burton1992-dc}。  
その後、逆最適化は広く研究されるようになり \citep{Ahuja2001-cv,Heuberger2004-zv,Chan2019-zg,Chan2023-qk}、交通 \citep{Bertsimas2015-kw}、電力システム \citep{Birge2017-il}、医療 \citep{Chan2022-uq} などのさまざまな分野に応用されてきた。  
さらに、逆強化学習 \citep{Ng2000-sf} やコントラスト学習 \citep{Shi2023-nd} をはじめとするさまざまな機械学習手法の基礎としても発展している。
-->
 

## 混合整数線形計画の逆最適化問題を高速に解くアルゴリズムを提案したこと[[8](#K8)]


## 参考文献

[<a id="K8">8</a>] Akira Kitaoka, A fast algorithm to minimize prediction loss of the optimal solution in inverse optimization problem of MILP, preprint.
[[arXiv:2405.14273](https://arxiv.org/abs/2405.14273)]

[<a id="sakaue2025online">Sakaue et. al. 2025</a>] S Sakaue, T Tsuchiya, H Bao, T Oki, Online Inverse Linear Optimization: Improved Regret Bound, Robustness to Suboptimality, and Toward Tight Regret Analysis, preprint.
[[arXiv:2501.14349](https://arxiv.org/abs/2501.14349)]


<!-- 
<details><summary>詳細</summary>

工事中
</details>
-->


# BGG複体を用いた放物型幾何学の不変量に関する研究
**キーワード**:  Rumin 複体，解析的捩率，接触幾何，放物型幾何，幾何解析

**忙しい人向けモチベーションの説明**: 現在の中心的な研究課題は，de Rham複体で定義される不変量や概念などを，接触幾何や放物型幾何で見られるBernstein-Gelfand-Gelfand複体(BGG複体)でも同様に定義し，この二つの関係を明らかにすることで，幾何学的な制約条件を見つけることである．

**用語説明**
BGG複体: 放物型幾何やフィルター付き多様体に対して構成される複体であり，BGG複体のコホモロジーはde Rhamコホモロジーに一致するという事が挙げられる．
Rumin複体: 接触多様体に関するBernstein-Gelfand-Gelfand複体(BGG複体)である(cf. [[7](#K7)])．また，sub-Riemmann極限を考えた際に自然に現れるという性質を持つ[[Rum00](#Rum00),[AQ22](#AQ22)]．



## Rumin Laplacianの球面上の上の固有値分解[[1](#K1)]

**モチベーション**: Rumin複体から定まる偏微分作用素，Rumin Laplacianの固有値を計算することで，幾何学的不変量と予想されているものを球面上で計算できるようにするため．De Rham複体を使って定義される偏微分作用素(Hodge-de Rham Laplacian)を使って，幾何学的な不変量を定義することができる．例として，コホモロジーの次元，Euler数，解析的捩率(Euler数は熱核による等式で書けるが，この公式に変分を取ったもの)，$\eta$不変量が挙げられる．今，挙げたものは，Hodge-de Rham Laplacianの固有値を使って定義することができる．そこで，De Rham複体の理論をBGG複体に対応する理論を構築する際に，Rumin Laplacianの固有値がわかれば，対応する不変量と思われるものを計算できると期待できる．

**主定理について**: 対称空間上のHode-de Rhamラプラシアンの固有値はCasimir元で書ける[[IT78](#IT78)]．Rumin Laplacianでも，Casimir元みたいなもので表されるかというのは面白い問である．

## Lens空間におけるRumin複体から定まる接触捩率(接触捩率)と解析的捩率の一致[[2](#2)]

**モチベーション**: Rumin-Sesahdriが$3$次元$S^1$作用付き佐々木多様体上でRumin-Seshadri Laplacianの解析的捩率と(Hodge-de Rham Laplacianの)解析的捩率が一致する[[RS12](#RS12)]. これを高次元の佐々木多様体でも成り立つのかというのが自然な問として生じる．



## Rumin Laplacianの$0$固有値の空間とHodge-de Rham Laplacianの$0$固有値の空間の一致[[4](#K4)]

**モチベーション**: Rumin複体のコホモロジーとde Rhamコホモロジーは同型であることが知られている[[Rum94](#Rum94)]．一方で，Laplacianの$0$固有空間では，どれくらいずれるのかという自然な問が生じる．

**主定理について**: 佐々木多様体上だと，Rumin Laplacianの$0$固有値の空間とHodge-de Rham Laplacianの$0$固有値の空間は一致することが言える[[4](#K4)]．佐々木多様体では，Hodge-de Rham Laplacianの$0$固有ベクトルが，primitiveであることが知られていた[[Tac65](#Tac65), 定理 7.1, 8.1] [[Fuj66](#Fuj66), 系 4.1]．これらの命題をRumin複体の言葉で書き換えたことを意味する．

## 参考文献

[<a id="K1">1</a>] Akira Kitaoka, Analytic torsions associated with the Rumin complex on contact spheres, International Journal of Mathematics 31 (2020), no. 13, 2050112.
[[article link](https://www.worldscientific.com/doi/10.1142/S0129167X20501128)]
[[arXiv:1911.03092](https://arxiv.org/abs/1911.03092)]

<details><summary>裏話([1]の論文ができるまで)</summary>

修士の頃に始めた研究である．Rumin複体を使って定義される偏微分作用素として，Rumin Laplacianの他に，Rumin-Seshadri Laplacianがある．目標として，Rumin-Sesahdriが$3$次元$S^1$作用付き佐々木多様体上でRumin-Seshadri Laplacianの解析的捩率と(Hodge-de Rham Laplacianの)解析的捩率が一致するから，高次元の佐々木多様体でも証明できると思って研究を始めた．修士2年の8月にRumin-Seshadri Laplacianの固有値を球面上で計算できた．そこで，解析的捩率に対応するものを計算したら，解析的捩率と一致せず，なぜずれるのか，わからなかった．指導教員である平地先生に相談したところ**「定義を見返すのです」**というアドバイスが印象的で，定義をみなおしたら，博士1年の8月に，Rumin Laplacianの球面上の固有値がシンプルな式で書ける事に気づき，Rumin Laplacianの解析的捩率と(Hodge-de Rham Laplacianの)解析的捩率のズレを計算することができた．
</details>
<br>

[<a id="K3">3</a>] Akira Kitaoka, Ray-Singer Torsion and the Rumin Laplacian on lens spaces, 	
Symmetry, Integrability and Geometry: Methods and Applications (SIGMA) 18 (2022), 091, 16 pages.
[[article link](https://doi.org/10.3842/SIGMA.2022.091)][[arXiv:2009.03276](https://arxiv.org/abs/2009.03276)]

<details><summary>裏話([2]の論文ができるまで)</summary>

Littlewood-Richardson則を使って，Lens空間上のRuminラプラシアンの固有値を表現論の指標の計算に帰着させたのがポイント．これが博論になった．
</details>
<br>

[<a id="K4">4</a>] Akira Kitaoka, Harmonic forms and the Rumin complex on Sasakian manifolds, Osaka Journal of Mathematics, to appear.
[[arXiv:2204.03446](https://arxiv.org/abs/2204.03446)]

<details><summary>裏話([4]の論文ができるまで)</summary>

博士課程の頃にはできていた．とりあえず，論文にしたほうがよいということで書いた．
</details>
<br>

[<a id="K7">7</a>] Akira Kitaoka, Ray-Singer torsion and the Rumin Laplacian on lens spaces, [幾何構造と微分方程式 — 対称性・特異性及び量子化の視点から —](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/2268.html), 京都大学数理解析研究所講究録, 2023年11月. [発表原稿](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/pdf/2268-10.pdf)

<details><summary>BGG複体を定義するRTAならこれが速いかもしれない</summary>

と思っている．
</details>
<br>

[<a id="AQ22">AQ22</a>] P. Albin, H. Quan, Sub-Riemannian limit of the differential form heat kernels of contact manifolds, Int. Math. Res. Not. 2022 (2022), 5818–5881.

[<a id="Fuj66">Fuj66</a>] T. Fujitani, Complex-valued differential forms on normal contact Riemannian manifolds, Tohoku Math. J. 81 (1966), 349–361.

[<a id="IT78">IT78</a>] A. Ikeda and Y. Taniguchi, Spectra and eigenforms of the Laplacian on $S^n$ and $P^n(C)$, Osaka J. Math. 15 (1978), no. 3, 515–546.

[<a id="Rum94">Rum94</a>] M. Rumin, Formes diff ́erentielles sur les vari ́et ́es de contact (French), J. Differential Geom. 39 (1994), no. 2, 281–330.

[<a id="Rum94">Rum00</a>] M. Rumin, Sub-Riemannian limit of the differential form spectrum of contact manifolds, Geom. Funct. Anal.
10 (2000), 407–452.

[<a id="RS12">RS12</a>] M. Rumin and N. Seshadri, Analytic torsions on contact manifolds, Ann. Inst. Fourier
(Grenoble) 62 (2012), 727–782.

[<a id="Tac65">Tac65</a>] S. Tachibana, On harmonic tensors in compact Sasakian spaces, Tohoku Math. J. (2) 17 (1965), 271–284.

