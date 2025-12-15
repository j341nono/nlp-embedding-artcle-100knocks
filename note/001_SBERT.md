
### Sentence-BERT: Sentence Embeddings using Siamese BERT ([EMNLP-2019](https://aclanthology.org/D19-1410.pdf))
- SBERT の比較対象となっている従来のモデルには，GloVe embeddings や InferSent，Universal Sentence Encoder が採用されている．InferSent はシンプルな BiLSTM で構成される．SBERT は 12層の Transformer の層からなる．
- 訓練の損失関数は訓練データの形式に依存する．分類損失関数は，SNLI や Multi-NLI のようにデータに含意，矛盾，中率のようなラベルがある場合に，2つの文埋め込みとその差分を結合し，Softmax 分類器にかける．回帰損失関数は，STS のようにデータにスコアがある場合に，2つの文の余弦類似度を計算し，正解スコアとの間で平均二乗誤差を最小化する．Triplet 損失関数は，データがアンカー，正例，負例の3つ組で用意されている場合に使用する．(p3984, 3)
- [Reimers et al.,2016](https://aclanthology.org/C16-1009/) によると，STS の評価指標として ピアソン相関係数は適しておらず，代わりに スピアマン相関係数が良い (p3985, 4.1)
- 比較対象の2文対に対して，余弦類似度ではなく，負のマンハッタン距離や負のユークリッド距離を使用しても，STS における制度は変わらなかった．(p3985, 4)
- [Argument Facet Similarity (AFS) コーパス](https://aclanthology.org/W16-3636.pdf) (Misra et al.m,2016) は 銃規制，同性婚，死刑というトピックの文に対して 0~5 の関連性スコアが付与されたデータセットである．STS データセットは説明的な文章から構成されるが，AFS では会話からの議論的な抜粋から構成されるため，後者は類似性があるとみなされるためには，単に主張が似ているだけでなく，その reasoning についても考慮する必要があるため，難しいタスクとなっている．(p3986, 4.3)
- SentEval([Conneau and Kiela, 2018](https://arxiv.org/abs/1803.05449)) という文の感情極性を分類するタスクにおいて，BERT Avarage Pooling や BERT CLS-token による埋め込みを特徴量として線形回帰分類タスクとして訓練をした場合，GloVe embedding に匹敵する高い性能を発揮した．一方で，STS における NLI を用いて訓練し余弦類似度で類似度を算出する場合は GloVe embedding のスコアの方が圧倒的だった．これは，余弦類似度による測定ではすべての次元を同等に扱うが，線形回帰による分類器は特定の次元の影響力を高めることができることに由来する．(p3988, 5)
- SNLI などの訓練データを使用する場合の損失関数における文埋め込みの結合方式に関して，InferSent や Universal Sentence Encoder は (u, v, |u-v|, u*v) の形式が採用されているが，SBERT においては (u, v, |u-v|) が最も良い結果となった．この結合方式は Softmax 分類器の訓練にのみ関係する．(p3989, 6)
- SBERTにおいて，NLIのような分類データセットの訓練においては文埋め込みを結合して Softmax 分類器を訓練するが，推論時はこうして更新された重みを活用して2つの分埋め込みの余弦類似度を計算している．これは訓練によって，ネットワークは似ている文（含意）はベクトルの差 (|u-v|) が小さくなるように，逆に，矛盾する文は差が大きくなるように重みを更新せざるを得なくなるから成立する．(p3989, 6)
- [BiLSTM-layer](https://aclanthology.org/D17-1070/) においては Mean Pooling よりも Max Pooling の方が適していたが，
SBERT の回帰損失関数においては Max Pooling は他よりも最も悪い結果となった．(p3989, 6)

