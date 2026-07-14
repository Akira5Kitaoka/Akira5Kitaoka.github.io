<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>

# 曲線のエネルギー最小化による経路に基づく反実仮想説明

**キーワード**: 反実仮想説明，XAI(説明可能AI)，機械学習，数値解析，微分幾何

**背景**: 経路に基づく反実仮想説明は，訓練された分類器から望ましい予測結果を得るために，入力特徴量から経路を提案する説明手法である．この説明手法の需要は，ローン審査や健康診断といった領域で高まっている．

## 曲線のエネルギー最小化による経路に基づく反実仮想説明[[P11](#K11)] (with 岡嶋 穣，佐々木 耀一，高野 凜)

**従来課題**: 経路に基づく反実仮想説明において，提案される経路がジグザグになり解釈が難しいことや，スパース性の追加が困難であることが，課題として挙げられる．

**解決手法**: [[P10](#K10)]を参考に，曲線のエネルギー最小化問題の解として経路を定義し，その最適化問題を有限差分法で近似し，非線形計画問題へ帰着させる手法を提案する．

**効果**: 実験で，滑らかな経路を取得できることおよびスパース性を確認し，具体的には従来手法の必要な特徴量を73%に抑えられることを確認する．

<!-- 
<details><summary>詳細</summary>

工事中
</details>
-->

## 参考文献

[<a id="K11">P11</a>] 北岡 旦, 岡嶋 穣，佐々木 耀一，高野 凜，曲線のエネルギー最小化による経路に基づく反実仮想説明 (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/1Win4-97/advanced), 1Win4-97, 2025年5月27日. \[[論文](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_1Win497/_article/-char/ja/)\] \[[正誤表](../../papers/JSAI2025_energy_minimization_CE/JSAI_energy_minimization_CE_errata.pdf)\]

[<a id="K10">P10</a>] Akira Kitaoka, Minimization of curve length through energy minimization using finite difference and numerical integration in real coordinate space, preprint.
[[arXiv:2504.15566](https://arxiv.org/abs/2504.15566)]

<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る>></a>