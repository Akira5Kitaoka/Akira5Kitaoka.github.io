# 研究紹介

北岡がどのようなことを研究してきたかを説明します．
(2026年10月25日現在)

## 今まで主に研究したテーマ

### 混合整数計画に対する逆最適化
- 与えられたデータが最適解になるような，(混合整数)線形計画の目的関数の重みと制約条件を学習する高速なアルゴリズムを開発しています．
- <a href="{{ '/research_interest/inverse_optimization' | relative_url }}">詳細はこちら</a>
- 参考文献 (解説)
    - [P20] 北岡 旦, 逆混合整数計画法: 制約条件学習からの目的関数学習 (Japanese), [JSAI2026](https://pub.confit.atlas.jp/ja/event/jsai2026/presentation/5H2-OS-18a-02), 5H2-OS-18a, 2026年6月12日. \[[論文](https://www.jstage.jst.go.jp/article/pjsai/JSAI2026/0/JSAI2026_5H2OS18a02/_article/-char/ja/)\]
    - [P9] 北岡　旦, 江藤　力, 混合整数線形計画の逆問題におけるSuboptimality損失と予測損失の等価性 (Japanese), [JSAI2024](https://confit.atlas.jp/guide/event/jsai2024/subject/1F3-GS-1-02/date?cryptoId=), (2024年5月28日), 1F3-GS-1-02. \[[論文](https://www.jstage.jst.go.jp/article/pjsai/JSAI2024/0/JSAI2024_1F3GS102/_article/-char/ja)\]
- 参考文献 (論文)
    - [P21] Akira Kitaoka, Explicit Iteration Complexity of Exact Data-Driven Inverse Optimization for Integer Linear Programs, preprint.
[[arXiv:2607.22263](https://arxiv.org/abs/2607.22263)]
    - [P13] Akira Kitaoka, Inverse Mixed-Integer Programming: Learning Constraints then Objective Functions, preprint.
[[arXiv:2510.04455](https://arxiv.org/abs/2510.04455)]
    - [P8] Akira Kitaoka, Exact Solution to Data-Driven Inverse Optimization of MILPs in Finite Time via Gradient-Based Methods, preprint.
[[arXiv:2405.14273](https://arxiv.org/abs/2405.14273)]
    - [P6] Akira Kitaoka, Riki Eto, A proof of imitation of Wasserstein inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.10089](https://arxiv.org/abs/2305.10089)]
    - [P5] Akira Kitaoka, Riki Eto, A proof of convergence of inverse reinforcement learning for multi-objective optimization, preprint.
[[arXiv:2305.06137](https://arxiv.org/abs/2305.06137)]


### アウトパフォーマンス逆最適化
- 観測されたエージェントの行動を再現するのではなく，その行動を全ての成分で上回る最適解を与えるような，目的関数の重みを学習する手法を研究しています．
- 順問題のソルバーのみで計算できる損失関数を設計し，混合整数計画に適用できる勾配ベースの手法とDC最適化による手法を提案しました．また，汎化誤差が観測数に反比例して減少することを示しました．
- 参考文献 (論文)
    - [P23] Akira Kitaoka, Outperformance Inverse Optimization: Learning Objective Functions that Outperform Agent Decisions, preprint.
[[arXiv:2610.09890](https://arxiv.org/abs/2610.09890)]

### オンライン逆最適化
- 逐次的に観測されるエージェントの最適な行動から，線形の目的関数の重みを学習する手法を研究しています．
- 順問題が整数線形計画（ILP）の場合に，誤りの回数とリグレットが総ラウンド数に依存しない手法を提案しました．
- <a href="{{ '/research_interest/online_inverse_optimization' | relative_url }}">詳細はこちら</a>
- 参考文献 (論文)
    - [P22] Akira Kitaoka, Online Inverse Integer Linear Optimization via Small-Gradient Skipping: Constant Regret and Finite Mistakes, preprint.
[[arXiv:2609.09809](https://arxiv.org/abs/2609.09809)]


### 反実仮想説明・アルゴリズミックリコース
- 反実仮想説明，つまり「現時点での入力から，どう修正したら望む出力が得られるか」を説明するAIに関して研究しています．
- 反実仮想説明の中でも，アクションを経路で提示する方法について，研究しています．
- <a href="{{ '/research_interest/counterfactual_explanation' | relative_url }}">詳細はこちら</a>
- 参考文献 (解説)
    - [P11] 北岡 旦, 岡嶋 穣，佐々木 耀一，高野 凜，曲線のエネルギー最小化による経路に基づく反実仮想説明 (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/1Win4-97/advanced), 1Win4-97, 2025年5月27日. \[[論文](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_1Win497/_article/-char/ja/)\] \[[正誤表](../papers/JSAI2025_energy_minimization_CE/JSAI_energy_minimization_CE_errata.pdf)\]

### 平均化データ学習
- データの平均化をはじめとするデータ集約技術は，データ要約，圧縮，アノテーションコスト削減，データ品質向上，プライバシー保護に有用です．
- 平均化されたデータから機械学習する方法について研究していました．
- <a href="{{ '/research_interest/averaged_data' | relative_url }}">詳細はこちら</a>
- 参考文献 (解説)
    - [P12] 松野 竜太，北岡 旦，佐久間 啓太，廣川 暢一，2サンプル平均化データに基づく非線形回帰モデルの学習 (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/2L4-GS-1-05/advanced), 2L4-GS-1-05, 2025年5月28日.  \[[論文](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_2L4GS105/_article/-char/ja/)\]
- 参考文献 (論文)
    - [P19] Ryuta Matsuno, Akira Kitaoka, Learning from Two-Sample Averaged Data, [ICDM 2025](https://www3.cs.stonybrook.edu/~icdm2025/index.html), 2025年11月12日. [[論文](https://ieeexplore.ieee.org/document/11391798)] [[講演動画](https://video.computer.org/ICDM25/10ME7GxDtJrOktMO8JeVyl-DM531-Matsuno.mp4.html)]

### 最短測地線近似アルゴリズム
- 有限差分法と数値積分を用いた実座標空間上の曲線のエネルギー最小化による長さ最小化の誤差評価の研究をしていました．
- <a href="{{ '/research_interest/geodesic' | relative_url }}">詳細はこちら</a>
- 参考文献 (論文)
    - [P10] Akira Kitaoka, Minimization of curve length through energy minimization using finite difference and numerical integration in real coordinate space, preprint.
[[arXiv:2504.15566](https://arxiv.org/abs/2504.15566)]

### BGG複体を用いた放物型幾何学の不変量
- De Rham複体で定義される不変量や概念などを，接触幾何や放物型幾何で見られるBernstein-Gelfand-Gelfand複体(BGG複体)でも同様に定義し，この二つの関係を明らかにすることで，幾何学的な制約条件を見つける研究をしていました．
- <a href="{{ '/research_interest/bgg_complex' | relative_url }}">詳細はこちら</a>
- 参考文献 (解説)
    - [P7] Akira Kitaoka, Ray-Singer torsion and the Rumin Laplacian on lens spaces, [幾何構造と微分方程式 — 対称性・特異性及び量子化の視点から —](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/2268.html), 京都大学数理解析研究所講究録, 2023年11月. [[発表原稿](https://www.kurims.kyoto-u.ac.jp/~kyodo/kokyuroku/contents/pdf/2268-10.pdf)]
- 参考文献 (論文)
    - [P4] Akira Kitaoka, Harmonic forms and the Rumin complex on Sasakian manifolds, [Osaka Journal of Mathematics](http://www.math.sci.osaka-u.ac.jp/ojm/), to appear.
[[arXiv:2204.03446](https://arxiv.org/abs/2204.03446)]
    - [P3] Akira Kitaoka, Ray-Singer Torsion and the Rumin Laplacian on lens spaces, Symmetry, Integrability and Geometry: Methods and Applications (SIGMA) 18 (2022), 091, 16 pages.
[[article link](https://doi.org/10.3842/SIGMA.2022.091)][[arXiv:2009.03276](https://arxiv.org/abs/2009.03276)]
    - [P1] Akira Kitaoka, Analytic torsions associated with the Rumin complex on contact spheres, International Journal of Mathematics 31 (2020), no. 13, 2050112.
[[article link](https://www.worldscientific.com/doi/10.1142/S0129167X20501128)][[arXiv:1911.03092](https://arxiv.org/abs/1911.03092)]


## 研究の小ネタ集
自分の研究で小ネタになりそうな話．

### 写像minとmaxとAffine写像の合成写像による区分線形写像の特徴付け
- 区分線形写像(piecewise linear map, PL写像)は，最適化問題，機械学習，統計学，制御理論，位相幾何学などで出てくる，基本的な関数である．今回は，区分線形写像の特徴付けとして，minとmaxとAffine写像の合成写像で与えられることを紹介する．
- [リンク](https://zenn.dev/akira5kitaoka/articles/216c252af23dce)

### 係数が解析的な線形常微分方程式の解の次元
- 線形常微分方程式の先頭項の係数がどの点でも0にならないとき，この微分方程式の解空間の次元は，その微分方程式の階数に一致するという定理が知られています．では，先頭項の係数が0になる点がある場合の次元はどうなるのでしょうか．今回は，係数が実解析的な場合，線形常微分方程式の解空間の次元と，その微分方程式の階数との誤差が，先頭係数の零点の位数で評価できることを紹介します．
- [リンク](https://zenn.dev/akira5kitaoka/articles/8f2a74a55047b7)
