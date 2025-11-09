<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>>></a>

# BGG複体を用いた放物型幾何学の不変量に関する研究
**キーワード**:  Rumin 複体，解析的捩率，接触幾何，放物型幾何，幾何解析

**忙しい人向けモチベーションの説明**: De Rham複体で定義される不変量や概念などを，接触幾何や放物型幾何で見られるBernstein-Gelfand-Gelfand複体(BGG複体)でも同様に定義し，この二つの関係を明らかにすることで，幾何学的な制約条件を見つけることである．

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

[<a id="K3">3</a>] Akira Kitaoka, Ray-Singer Torsion and the Rumin Laplacian on lens spaces, 	
Symmetry, Integrability and Geometry: Methods and Applications (SIGMA) 18 (2022), 091, 16 pages.
[[article link](https://doi.org/10.3842/SIGMA.2022.091)][[arXiv:2009.03276](https://arxiv.org/abs/2009.03276)]

<details><summary>裏話([2]の論文ができるまで)</summary>

Littlewood-Richardson則を使って，Lens空間上のRuminラプラシアンの固有値を表現論の指標の計算に帰着させたのがポイント．これが博論になった．
</details>

[<a id="K4">4</a>] Akira Kitaoka, Harmonic forms and the Rumin complex on Sasakian manifolds, Osaka Journal of Mathematics, to appear.
[[arXiv:2204.03446](https://arxiv.org/abs/2204.03446)]

<details><summary>裏話([4]の論文ができるまで)</summary>

博士課程の頃にはできていた．とりあえず，論文にしたほうがよいということで書いた．
</details>

[<a id="K7">7</a>] Akira Kitaoka, Ray-Singer torsion and the Rumin Laplacian on lens spaces, [幾何構造と微分方程式 — 対称性・特異性及び量子化の視点から —](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/2268.html), 京都大学数理解析研究所講究録, 2023年11月. [発表原稿](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/pdf/2268-10.pdf)

<details><summary>BGG複体を定義するRTAならこれが速いかもしれない</summary>

と思っている．
</details>

[<a id="AQ22">AQ22</a>] P. Albin, H. Quan, Sub-Riemannian limit of the differential form heat kernels of contact manifolds, Int. Math. Res. Not. 2022 (2022), 5818–5881.

[<a id="Fuj66">Fuj66</a>] T. Fujitani, Complex-valued differential forms on normal contact Riemannian manifolds, Tohoku Math. J. 81 (1966), 349–361.

[<a id="IT78">IT78</a>] A. Ikeda and Y. Taniguchi, Spectra and eigenforms of the Laplacian on $S^n$ and $P^n(C)$, Osaka J. Math. 15 (1978), no. 3, 515–546.

[<a id="Rum94">Rum94</a>] M. Rumin, Formes diff ́erentielles sur les vari ́et ́es de contact (French), J. Differential Geom. 39 (1994), no. 2, 281–330.

[<a id="Rum94">Rum00</a>] M. Rumin, Sub-Riemannian limit of the differential form spectrum of contact manifolds, Geom. Funct. Anal.
10 (2000), 407–452.

[<a id="RS12">RS12</a>] M. Rumin and N. Seshadri, Analytic torsions on contact manifolds, Ann. Inst. Fourier
(Grenoble) 62 (2012), 727–782.

[<a id="Tac65">Tac65</a>] S. Tachibana, On harmonic tensors in compact Sasakian spaces, Tohoku Math. J. (2) 17 (1965), 271–284.

<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>>></a>