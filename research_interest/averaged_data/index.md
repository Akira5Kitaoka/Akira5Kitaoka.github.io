<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る</a>

# 平均化データ学習

**背景**：データの平均化をはじめとするデータ集約技術は，データ要約，圧縮，アノテーションコスト削減，データ品質向上，プライバシー保護に有用です[[Zhang+ 2020][#Zhang2020]]．特に，フック数サンプルの平均化は，平均化前のオリジナルデータの完全な復元を困難にすることからプライバシー保護に効果的である．

**課題**：複数サンプル間で平均化された入出力に対する機械学習手法
として Ecological Regression が知られている [[Robinson 50][#Robinson1950], [Gelman+ 01][#Gelman2001]]. Ecological Regressionは, 平均化されたデータに対する予測誤差を用いて, 線形回帰モデルを学習する[[Bhowmik+ 16][#Bhomik2016]]. しかしながら, この訓練手法は非線形回帰モデルに対して不適当である. 我々の知る限り, 複数サンプル間で平均化された入出力データから一般的な非線形モデルを学習する手法は知られていない.

**手法**: [[12][#K12]]は, 加法的等分散ガウスノイズと正規分布入力を仮定した上で平均化データに対する予測モデルの尤度を定式化し, Taylor 近似を用いて導出した近似尤度を最大化することで予測モデルを学習する. また, [[12][#K12]]はこの近似尤度の効率的な数値計算方法を理論解析に基づいて導出する.

**効果**: [[12][#K12]]は, 平均化されたデータから非線形モデルを学習する最初の手法として, 特に2サンプル平均化データを対象とした学習手法である．





## 参考文献

[<a id="K12">12</a>] 松野 竜太，北岡 旦，佐久間 啓太，廣川 暢一，2サンプル平均化データに基づく非線形回帰モデルの学習 (Japanese), [JSAI2025](https://confit.atlas.jp/guide/event/jsai2025/subject/2L4-GS-1-05/advanced), 2L4-GS-1-05, 2025年5月28日.  \[[論文](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_2L4GS105/_article/-char/ja/)\]

[<a id="Bhomik2016">Bhowmik+ 16</a>] Bhowmik, A., Ghosh, J., and Koyejo, O.: Sparse Parameter Recovery from Aggregated Data, inInternational Conference on Machine Learning (2016)

[<a id="Gelman2001">Gelman+ 01</a>] Gelman, A., Park, D., Ansolabehere, S.,　Price, P., and Minnite, L.: Models, assumptions and　model checking in ecological regressions, Journal of the　Royal Statistical Society. Series A: Statistics in Society,　Vol. 164, No. 1 (2001)

[<a id="Robinson1950">Robinson 50</a>] Robinson, W. S.: Ecological Correlations and the Behavior of Individuals, American Sociological　Review, Vol. 15, No. 3 (1950)

[<a id="Zhang2020">Zhang+ 20</a>] Zhang, Y., Charoenphakdee, N., Wu, Z., and Sugiyama, M.: Learning from Aggregate Observations, in Advances in Neural Information Processing Systems, Vol. 33 (2020)

<a href="{{ '/research_interest' | relative_url }}">研究紹介に戻る</a>