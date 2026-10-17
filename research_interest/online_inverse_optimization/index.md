<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>

更新日：2026年10月17日

# オンライン逆最適化

**キーワード**: オンライン逆線形最適化，逆最適化，オンライン学習，リグレット解析，整数線形計画

**研究内容**：状態が逐次に到来するオンラインの設定で，エージェントの最適な行動だけを手がかりに，線形の目的関数の重みを学習する手法を研究した．とくに，順問題が整数線形計画（ILP）の場合に，誤りの回数とリグレットが総ラウンド数$T$に依存しない手法を提案した[[P22](#K22)]．

**背景**: 観測された意思決定から，意思決定者が行動を選ぶ際の基準（目的関数）を推定する問題は，逆最適化や逆強化学習として研究されてきた．推定された重みは意思決定の理由を表す目的関数として解釈でき，顧客の選好の推定や，電力市場・医療における非公開の目的関数の推定などに応用をもつ．多くの現実の状況では，意思決定のデータは一度にまとめて得られるのではなく，推薦への反応や日々の計画のように逐次的に観測される．このとき，データがそろうのを待たずに，観測のたびに推定を更新しながら良い提案を出し続けることが求められる．これを定式化したのがオンライン逆最適化（文脈付き推薦とも呼ばれる）であり，学習を続ける間に生じる損失の累積（リグレット）をどれだけ小さく抑えられるかが問題となる．(cf. [[P22](#K22), [Oki and Sakaue 2026](#oki2026finite)])

## オンライン逆最適化とは

逆最適化は，観測された最適解から目的関数を推定する問題である（cf. [混合整数計画に対する逆最適化]({{ '/research_interest/inverse_optimization' | relative_url }})）．オンライン逆最適化では，データが一度に与えられるのではなく，1つずつ逐次に到来する．

学習者とエージェントの2人のプレイヤーを考える．エージェントは未知の重み$\theta^* \in \mathbb{R}^d$をもち，状態$s$が与えられると，順問題

$$
x^*(\theta, s) \in \mathrm{argmax}_{x \in X(s)} \langle \theta, x \rangle
$$

の$\theta = \theta^*$での最適解を行動として選ぶ．ここで$X(s) \subset \mathbb{R}^d$は状態$s$での実行可能な行動の集合であり，凸でなくてもよい（整数線形計画の実行可能集合など）．各ラウンド$t = 1, 2, \ldots, T$で，次をくり返す．

1. 状態$s^t$（行動集合$X(s^t)$）が提示される．
2. 学習者は，過去の観測から重み$\hat{\theta}^t$を予測し，それに基づく行動$\hat{x}^t \in \mathrm{argmax}_{x \in X(s^t)} \langle \hat{\theta}^t, x \rangle$を提案する．
3. エージェントの最適な行動$x^t = x^*(\theta^*, s^t)$が観測される．目的関数の値そのものは観測されない．

例えば，推薦（文脈付き推薦）では，各回の候補の集合から推薦を出し，利用者が実際に選んだもの（その利用者にとっての最適な選択）だけを観測して，利用者の好みの重みを学習する．スケジューリングでは，毎日の状況$s^t$に対して熟練者が作った勤務表$x^t$を観測し，熟練者が重視している指標の重みを学習する．

学習者の性能は，主に次の量で測る．

- **累積決定リグレット**（単にリグレットと呼ぶことが多い）：真の重みで測った，提案の目的関数値の不足の累積
$$
R^{\mathrm{est}}_T := \sum_{t=1}^T \langle \theta^*, x^t - \hat{x}^t \rangle .
$$
- **累積準最適性リグレット**：予測した重み$\hat{\theta}^t$から見て，エージェントの行動がどれだけ説明できていないかの累積
$$
R^{\mathrm{sub}}_T := \sum_{t=1}^T \langle \hat{\theta}^t, \hat{x}^t - x^t \rangle .
$$
- **誤りの回数**$K$：提案がエージェントの行動を説明できなかった（準最適性損失が正となった）ラウンドの数．

リグレットが$T$とともにどう増えるか，そして$T$に依存しない上界があるならば次元$d$にどう依存するかが，この分野の中心的な問いである．

## <a id="S1">1</a>. 小勾配スキップによる，$T$に依存しないリグレットと有限回の誤り [[P22](#K22)]

### [[P22](#K22)]の概要

||詳細|
|---|---|
|背景|オンライン学習の手法（OGD，ONS，MetaGrad）を準最適性損失に適用する枠組みがあり，リグレットはOGDで$O(\sqrt{T})$，2階の手法で$O(d \log T)$であった．|
|課題|$T$に依存しない既存の上界には，次元について指数的（$\exp(O(d\log d))$），ギャップの逆数の2乗に比例，毎ラウンド重心の計算が必要，といった制約があった．整数計画を含む一般の順問題に対して，$T$に依存せず次元の多項式である保証を，軽い計算で得られるかは未解決であった．|
|仮定|**一様マージン**：真の重みの下で，正しい行動と他の候補行動との目的関数値の差が，すべての状態で一様にある$\gamma > 0$以上である．順問題が整数線形計画ならマージンは自動的に正である．|
|提案手法|誤りのないラウンド（劣勾配として$0$を選べるラウンド）では，反復点も内部状態も一切更新しない**小勾配スキップ**（small-gradient skipping, SGS）を，OGD，ONS，MetaGradに組み込む．|
|理論結果1|3手法とも，誤りの回数$K$，$R^{\mathrm{sub}}_T$，$R^{\mathrm{est}}_T$が，$T$に依存しない量で押さえられる（決定的な保証）．|
|理論結果2|マージン$\gamma$を，実行可能集合の組合せ的構造（一般のILP，Graver基底による線形不等式，完全単模，M凸，M$^\natural$凸）から明示的に下から押さえる．|
|理論結果3|それらを代入して，問題クラス別の明示的な上界を得る．とくにILPで重み空間が確率単体のとき，ONSは$R^{\mathrm{est}}_T = O(d^2 \lVert M\rVert_2 \log(2\lVert M\rVert_2))$を，1回の誤りラウンドあたり$O(d^2)$と1回の射影という軽い計算で達成する．|

ここで$M_i := \sup_{s} \left( \max_{x \in X(s)} x_i - \min_{x \in X(s)} x_i \right)$は座標ごとの行動のレンジ，$M = (M_1, \ldots, M_d)$である．

### 比較の見方

以下の表は，上から下へ年代順（arXivでの初出順）に並べている．太字が本研究[[P22](#K22)]である．「有限回の誤り」は，誤りの回数$K$（提案がエージェントの行動を説明できなかったラウンドの数）が$T$に依存しない量で押さえられていることを表す．「—」は，本記事ではその文献の主張を確認していないことを表す．

リグレットの値は正規化によって変わるので，(A) 効用ベクトルと行動のEuclidノルムが高々$1$の正規化，(B) 重み空間が確率単体$\Theta = \Delta^{d-1}$の場合，の2つに分けて比べる．さらに (C) で，整数計画などの実行可能集合の構造の下での結果を比べる．

### 表1 (A)：Euclidノルムが高々$1$の正規化でのリグレットの比較

効用ベクトル$\theta^*$と行動$x$がともに$d$次元単位球に含まれる場合（[[Sakaue 2026b](#sakaue2026tight)]の表1と同じ正規化）．

|手法|$R^{\mathrm{est}}_T$|追加の仮定|proper|有限回の誤り|1ラウンドあたりの計算|
|---|---|---|---|---|---|
|OGD [[Bärmann et al. 2018](#barmann2018online)]|$O(\sqrt{T})$|なし|Yes|—|射影1回|
|ProjectedCones [[Besbes et al. 2021, 2025](#besbes2025contextual)]|$O(d^4 \log T)$|なし|Yes|—|—|
|切除平面法 [[Gollapudi et al. 2021](#gollapudi2021contextual)]|$O(d \log T)$|なし|Yes|—|重心の計算|
|切除平面法 [[Gollapudi et al. 2021](#gollapudi2021contextual)]|$\exp(O(d \log d))$|なし|Yes|—|John楕円体の計算|
|ONS [[Sakaue et al. 2025b](#sakaue2025online)]|$O(d \log T)$|なし|Yes|—|$O(d^2)$＋射影1回|
|二次パーセプトロン（CoRectron）[[Sakaue 2026a](#sakaue2026simple)]|$O(d \log T)$|なし|Yes|—|線形最適化1回＋$O(d^2)$（射影なし）|
|多重スケール報酬ヘッジング [[Dewasurendra 2026](#dewasurendra2026)]|$O(d)$|なし|No|—|一般には$T^{\Theta(d)}$個のプロキシ（非凸な投票の最大化）|
|**SGS-ONS, SGS-MetaGrad [[P22](#K22)]**|$O\left(d \log \left(2 + \frac{1}{\gamma}\right)\right)$ ※1|一様マージン$\gamma>0$|Yes|**Yes**|誤りラウンドのみ$O(d^2)$＋射影1回|
|可変計量アルゴリズム [[Cai et al. 2026](#cai2026efficient)]|$O(d)$|なし|Yes|—|線形最適化1回＋$O(d^2)$|
|多重スケール行列重み [[Sakaue 2026b](#sakaue2026tight)]|$O(\sqrt{d})$（期待値）|なし|No|—|$(dT)^{O(d)}$回の線形最適化|
|[[Gupta et al. 2026](#gupta2026optimal)]|$O(\sqrt{d})$|なし|Yes|—|線形最適化1回＋$O(td^2+t^2d)$回の演算（ラウンド$t$，決定的）|
|下界 [[Sakaue et al. 2025b](#sakaue2025online), [Sakaue 2026b](#sakaue2026tight)]|$\Omega(\sqrt{d})$（$T \geq d$）|—|—|—|—|

※ properな学習者とは，現在の行動集合を観測する前に非零の効用の推定値を固定し，その推定値の最大化元を推薦する学習者を指す（[[Sakaue 2026b](#sakaue2026tight)]の用法）．そうでない学習者をimproperと呼ぶ．なお[[Sakaue 2026a](#sakaue2026simple)]は，推定値の大きさを制限しないという意味で自らの手法をimproperと呼んでいるが，この定義ではproperな学習者である．

※1 [[P22](#K22)]のONSの上界$R^{\mathrm{est}}_T \le LD\left(1 + 2d \log\left(2 + \frac{2LD}{\gamma}\right)\right)$（$D$は重み空間の直径，$L := \sup_{s} \sup_{x \in X(s)} \lVert x - x^*(\theta^*, s) \rVert$は行動の広がり）に，この正規化での$L, D \leq 2$を代入したもの．

### 表1 (B)：重み空間が確率単体$\Theta = \Delta^{d-1}$の場合のリグレットの比較

重み空間$\Theta$として確率単体を含む一般の有界閉凸集合を許す研究を比べる．$L, D$は※1と同じ定数である．

|手法|$R^{\mathrm{sub}}_T$|$R^{\mathrm{est}}_T$|追加の仮定|有限回の誤り|
|---|---|---|---|---|
|OGD [[Bärmann et al. 2018](#barmann2018online)]|$O(LD\sqrt{T})$|$O(LD\sqrt{T})$|なし|—|
|FTRL [[Sakaue et al. 2025a](#sakaue2025aistats)]|$O(1/\Delta^2)$|$O(1/\Delta^2)$|ギャップ条件$\Delta > 0$|—|
|ONS, MetaGrad [[Sakaue et al. 2025b](#sakaue2025online)]|$O\left(LD\, d \log \frac{T}{d}\right)$|$O\left(LD\, d \log \frac{T}{d}\right)$|なし|—|
|**SGS-OGD [[P22](#K22)]**|$\dfrac{L^2D^2}{2\gamma}$|$\dfrac{2L^2D^2}{\gamma}$|一様マージン$\gamma>0$|**Yes**|
|**SGS-ONS [[P22](#K22)]**|$LD\left(1 + d \log \max\left(\frac{LD}{\gamma}, 1\right)\right)$|$LD\left(1 + 2d \log\left(2 + \frac{2LD}{\gamma}\right)\right)$|一様マージン$\gamma>0$|**Yes**|

[[Sakaue et al. 2025a](#sakaue2025aistats)]の定数は，正則化項と$\Theta$，$X(s)$の大きさに依存する．

### 表1 (C)：整数計画などの構造の下でのリグレットと誤りの回数

順問題が整数線形計画(ILP)である場合や，行動集合がM凸である場合の結果を比べる．[[P22](#K22)]の行は$\Theta = \Delta^{d-1}$とし，ONS（成長格子SGS-MetaGradも同じオーダー）の上界を示す．ここで$M_i := \sup_{s} \left( \max_{x \in X(s)} x_i - \min_{x \in X(s)} x_i \right)$は座標ごとの行動のレンジ，$M = (M_1, \ldots, M_d)$，$C_g$はスラック拡大行列のGraver基底の$\ell_\infty$ノルム（完全単模なら$C_g = 1$）である．

|手法|実行可能集合|マージン$\gamma$の下界|$R^{\mathrm{est}}_T$|誤りの回数$K$|1ラウンドあたりの計算|
|---|---|---|---|---|---|
|重心法 [[Oki and Sakaue 2026](#oki2026finite)]|M凸|（マージンを用いない）|$O(d \log d)$ ※2|主張なし|重心の計算（厳密には\#P困難）|
|**[[P22](#K22)]**|一般のILP|$\dfrac{1}{2^{d-1}\max(d-1,\sqrt{2})\,\lVert M\rVert_2^{d-1}}$|$O\left(d^2 \lVert M\rVert_2 \log(2\lVert M\rVert_2)\right)$|$O\left(d^{3} 2^{d} \lVert M\rVert_2^{d} \log (2\lVert M\rVert_2)\right)$|誤りラウンドのみ$O(d^2)$＋射影1回|
|**[[P22](#K22)]**|線形不等式（$C_g$）|$\dfrac{1}{\max(d-1,\sqrt{2})\,(2C_g\sqrt{d})^{d-1}}$|$O\left(L d^2 \log(2C_g dL)\right)$|$O\left(L d^{3} (2C_g\sqrt{d})^{d} \log (2C_g dL)\right)$|同上|
|**[[P22](#K22)]**|M凸|$\dfrac{2}{d(d-1)}$|$O\left(L d \log(2dL)\right)$|$O\left(L d^{3} \log (2dL)\right)$|同上|
|**[[P22](#K22)]**|M$^\natural$凸|$\dfrac{2}{d(d+1)}$|$O\left(L d \log(2dL)\right)$|$O\left(L d^{3} \log (2dL)\right)$|同上|
|参考：表1 (A)の$T$に依存しない手法|任意のコンパクト集合（ILPを含む）|（マージンを用いない）|$O(d)$，$O(\sqrt{d})$など（表1 (A)の正規化）|—|表1 (A)を参照|

※2 [[Oki and Sakaue 2026](#oki2026finite)]の上界は，1ラウンドあたりのリグレットを$O(1)$とする正規化の下での値である．正規化を外すと$O(\lVert \theta^* \rVert L\, d \log d)$となり（この換算は[[P22](#K22)]による），M凸の場合の[[P22](#K22)]の上界$O(Ld\log(2dL))$との違いは対数の中身だけである．[[Oki and Sakaue 2026](#oki2026finite)]は近似的な重心による多項式時間の乱択実装も与えているが，その場合の上界は期待値の意味であり，1ラウンドあたり$O(d^6 \log d \log T)$程度の計算を要する．


表1 (C)のとおり，**一般のILPに対して，誤りの回数$K$を$T$に依存しない量で押さえているのは[[P22](#K22)]だけである**．[[Oki and Sakaue 2026](#oki2026finite)]では，誤りの回数の評価は定理4.2の証明の中に現れるのみで，しかも行動集合のM凸性を要する．

### その後の研究による改善

[[P22](#K22)]の後，$T$に依存しない$R^{\mathrm{est}}_T$は，マージンを仮定しない設定で$O(d)$ [[Cai et al. 2026](#cai2026efficient)]，さらに下界と定数倍まで一致する$O(\sqrt{d})$ [[Sakaue 2026b](#sakaue2026tight)]まで改善され，$O(\sqrt{d})$は多項式時間の決定的な手法でも達成された [[Gupta et al. 2026](#gupta2026optimal)]．したがって，表1 (A)の正規化での$R^{\mathrm{est}}_T$の次元依存については，[[P22](#K22)]の上界は後続研究によって改善されている．

一方，[[P22](#K22)]は$R^{\mathrm{est}}_T$に加えて，**誤りの回数$K$**と**累積準最適性リグレット$R^{\mathrm{sub}}_T$**も$T$に依存しない量で押さえており，一般のILPに対してはマージンの明示的な下界まで与えている（表1 (C)）．

### 表2：[[P22](#K22)]の主結果

一様マージン$\gamma$の下で，任意の（適応的でもよい）状態列に対して次が成り立つ．右辺はいずれも$T$に依存しない．

||SGS-OGD|ONS|成長格子SGS-MetaGrad|
|---|---|---|---|
|$K$|$\dfrac{2L^2D^2}{\gamma^2}$|$d + \dfrac{2LD}{\gamma}\left(1 + d \log \max\left(\dfrac{2LD}{\gamma}, 1\right)\right)$|$O\left(\dfrac{dLD}{\gamma} \log \max\left(\dfrac{2LD}{\gamma}, 2\right)\right)$|
|$R^{\mathrm{sub}}_T$|$\dfrac{L^2D^2}{2\gamma}$|$LD\left(1 + d \log \max\left(\dfrac{LD}{\gamma}, 1\right)\right)$|$O\left(LD\, d \log \max\left(\dfrac{LD}{\gamma}, 1\right)\right)$|
|$R^{\mathrm{sub}}_T + R^{\mathrm{est}}_T$|$\dfrac{2L^2D^2}{\gamma}$|$LD\left(1 + 2d \log\left(2 + \dfrac{2LD}{\gamma}\right)\right)$|$O\left(LD\, d \log \max\left(\dfrac{LD}{\gamma}, 2\right)\right)$|

$R^{\mathrm{sub}}_T, R^{\mathrm{est}}_T \geq 0$なので，$R^{\mathrm{est}}_T$も最後の行の上界をもつ．ONSとMetaGradは，マージンへの依存が対数のみである．

## 参考文献

[<a id="K22">P22</a>] Akira Kitaoka, Online Inverse Integer Linear Optimization via Small-Gradient Skipping: Constant Regret and Finite Mistakes, preprint.
[[arXiv:2609.09809](https://arxiv.org/abs/2609.09809)]

[<a id="barmann2018online">Bärmann et al. 2018</a>] A. Bärmann, A. Martin, S. Pokutta, O. Schneider, An online-learning approach to inverse optimization, 2018. [[arXiv:1810.12997](https://arxiv.org/abs/1810.12997)]

[<a id="besbes2025contextual">Besbes et al. 2025</a>] O. Besbes, Y. Fonseca, I. Lobel, Contextual inverse optimization: Offline and online learning, Operations Research 73(1) (2025), 424–443.

[<a id="cai2026efficient">Cai et al. 2026</a>] Y. Cai, A. Gupta, V. Gupta, G. Guruganesh, Y. Jiang, C. Liaw, A. Mehta, R. Paes Leme, G. Velegkas, D. Wang, Efficient Online Inverse Optimization with $O(d)$ Regret, preprint, 2026. [[arXiv:2609.13440](https://arxiv.org/abs/2609.13440)]

[<a id="dewasurendra2026">Dewasurendra 2026</a>] P. Dewasurendra, Multiscale Reward Hedging from Correct Demonstrations, preprint, 2026. [[arXiv:2608.06825v1](https://arxiv.org/abs/2608.06825v1)]（arXivにより取り下げ（2026年9月24日，v2）．本記事はv1に基づく．）

[<a id="gollapudi2021contextual">Gollapudi et al. 2021</a>] S. Gollapudi, G. Guruganesh, K. Kollias, P. Manurangsi, R. Paes Leme, J. Schneider, Contextual recommendations and low-regret cutting-plane algorithms, Advances in Neural Information Processing Systems 34 (2021), 22498–22508.

[<a id="gupta2026optimal">Gupta et al. 2026</a>] A. Gupta, G. Guruganesh, H. Lin, V. Mirrokni, R. Paes Leme, D. P. Woodruff, Optimal and Efficient Online Inverse Optimization, preprint, 2026. [[arXiv:2610.08735](https://arxiv.org/abs/2610.08735)]

[<a id="oki2026finite">Oki and Sakaue 2026</a>] T. Oki, S. Sakaue, Finite and Corruption-Robust Regret Bounds in Online Inverse Linear Optimization under M-Convex Action Sets, preprint, 2026. [[arXiv:2602.01682](https://arxiv.org/abs/2602.01682)]

[<a id="sakaue2025aistats">Sakaue et al. 2025a</a>] S. Sakaue, H. Bao, T. Tsuchiya, Revisiting Online Learning Approach to Inverse Linear Optimization: A Fenchel–Young Loss Perspective and Gap-Dependent Regret Analysis, Proceedings of the 28th International Conference on Artificial Intelligence and Statistics (AISTATS 2025), 46–54. [[arXiv:2501.13648](https://arxiv.org/abs/2501.13648)]

[<a id="sakaue2025online">Sakaue et al. 2025b</a>] S. Sakaue, T. Tsuchiya, H. Bao, T. Oki, Online Inverse Linear Optimization: Efficient Logarithmic-Regret Algorithm, Robustness to Suboptimality, and Lower Bound, Advances in Neural Information Processing Systems 38 (2025). [[arXiv:2501.14349](https://arxiv.org/abs/2501.14349)]

[<a id="sakaue2026simple">Sakaue 2026a</a>] S. Sakaue, Simple Projection-Free Algorithm for Contextual Recommendation with Logarithmic Regret and Robustness, preprint, 2026. [[arXiv:2603.20826](https://arxiv.org/abs/2603.20826)]

[<a id="sakaue2026tight">Sakaue 2026b</a>] S. Sakaue, Tight Regret Bound for Online Inverse Linear Optimization via Multiscale Matrix Weights, preprint, 2026. [[arXiv:2609.26978](https://arxiv.org/abs/2609.26978)]

<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>
