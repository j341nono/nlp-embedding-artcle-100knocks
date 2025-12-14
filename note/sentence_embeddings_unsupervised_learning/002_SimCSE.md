
### SimCSE: Simple Contrastive Learning of Sentence Embeddings ([EMNLP-2020](https://aclanthology.org/2021.emnlp-main.552/))

- Unspervised SimCSE は，ドロップアウトのみ変更し同じ文から正例ペアを構築する．ドロップアウトはデータ拡張として機能するが，これを取り除くと representation collapse (表現の崩壊) を引き起こす．(p6894, 1)
- Supervisd SinCSE は，これまでが NLI の3つのラベルの分類タスクを実施していたが，本手法では含意ペアを正例に，矛盾ペアをハードネガティブに使用した．これにより，同じデータにもかかわらず性能が大きく向上した．(p6894, 1)
- Alignment と Uniformity の指標で分析すると，Unspervised SimCSE は，Alignment の劣化を回避しながら，Uniformity の向上に成功していた．Supervisd SinCSE では NLI の使用により，Alignment がさらに改善していた．(p6895, 1) また，この2つの指標を活用することで提案手法のアプローチの内部的な仕組みを正当化する．<- 自分の研究でも同じように使用できるのでは．(p6894, 2)
- 対照学習の引用 ([Hadsell et al., 2006-IEEE](https://ieeexplore.ieee.org/abstract/document/1640964))(p6895, 2)
- 対照損失関数 (InfoNCE) のわかりやすい説明．τ は分布の鋭さを操作し，値が小さいほど分布が尖り，モデルは正解以外を許さないとなり厳しい識別能力が求められるようになる．一般的に学習が難しくなるが，表現の分離能力は高まる．(p6895, 2)
- モーダル別の表現学習の名称に関して，visual representations と  language representations という表現を使用している．(p6895, 2)
- Unspervised SimCSE における Alignment と Uniformity の関係を実験した結果，慈善学習済みの Checkpoint から学習を開始することで Uniformity が大幅に改善されることがわかった．しかし，Dropout を0にしたり，正例ペアの Encoding で同一の Dropout mask を使用した場合，ペアの埋め込みが全く同じになり，劇的に性能が低下することがわかった．実際，これらの場合には Alignment の指標が悪化した．一方で，delete one word の場合には，Alighnemt が提案手法よりも良いが，Uniformity の改善幅が小さかった．また，この結果は，事前学習済み Checkpoint から開始することで良い初期 Alignment を維持できることもわかった．(p6897, 3)
- Supervised SimCSE の導入の理由として，Unsupervised SimCSE が良い Alignment を維持できたが，Supervised としてのデータセットを活用することで，Alignment を向上させることができるのか，と展開している．(p6897, 4)
- Supervised における正例ペアとするためのデータセットとして以下の4つの候補を実験した．(1)QQP4：Quora の質問ペア，(2)Flickr30k：画像に5つの人間によるキャプションがあり，同じ画像の任意の2つのキャプションを正例ペアとする．(3)ParaNMT：逆翻訳パラフレーズデータセット．(4)NLI：SNLIとMNLI．実験の結果，NLI が最も高く，他は Unspervised SimCSE と同等かそれ以下の結果となった．この理由として，クラウドソーシングを使用したという品質の高さや，2つの文の語彙の重複の少なさによる．(p6898, 4)
- [Wang et al. (2020)](https://aclanthology.org/D19-1006/) によると，異方性による現象として，SVD を行うと，少しの次元が非常に強い値を持ち，他はほぼ0になる．つまり，単語ベクトルは数百次元あっても、実質的にはごく一部の方向（数次元）しか使っておらず，ペチャンコに潰れたような分布になっている．異方性への対処法として，PCAにおける支配的な主成分を削除する ([Arora et al., 2017](https://openreview.net/forum?id=SyK00v5xx); [Mu and Viswanath, 2018](https://openreview.net/forum?id=HkuGJ3kCb)) か，等方性の分布へマッピングするという後処理がある． ([Li et al., 2020](https://aclanthology.org/2020.emnlp-main.733/); [Su et al., 2021](https://arxiv.org/pdf/2103.15316)).その他の対処法として，訓練時に正則化を追加することが挙げられる．ベクトルの分布が偏ったらペナルティを与えるというものである．([Gao et al., 2019](https://openreview.net/forum?id=SkEYojRqtm); [Wang et al., 2020](https://openreview.net/forum?id=ByxY8CNtvr))(p6898, 5)
- 異方性は埋め込み空間における分布という文脈で Uniformity と深い関わりがある．直感的にも対照目的関数による訓練では，Uniformity を改善し，異方性の問題も緩和している．(p6898, 5)
- 対照損失関数が最小化することは結果的に，固有値の合計が一定である中で最大固有値を低下させ，スペクトルが平坦化することになる．細かな証明は省く．後処理的な手法は等方性へ促すだけであり，正例同士の近さは保証されない．しかし，対照学習は損失関数（の第1項）により正例のアライメントを行いつつ，第2項によって空間の Uniformity 化を同時に最適化している．
- 文埋め込みの主な目的は意味的に類似した文をクラスタリングすることであるから，STSを主な結果として捉える．(p6899, 6.1)
- 評価設定において，(1)追加の回帰変数を使用するかどうか，(2)スピアマン相関係数かピアソン相関係数か，(3)結果の集計方法，について条件設定に違いが生じるが，(1)なし，(2)スピアマン相関係数，(3)全て集計する，という条件にした．(p6899, 6.1)
- 過去の研究では BERT の最終層の平均プーリングを使用する方が，CLS トークンをそのまま使用するよりも良い結果となることが報告されていた．これが SimCSE でも同様なのかを実験した．BERT の元の実装では，CLS トークンの後に 全結合層のMLP があるため，以下の3つの条件 (1)学習時も推論時もMLP層を通した出力を使用する，(2)MLP を取り払い，CLS トークンの出力をそのまま使用する，(3)学習時は MLP層を通して学習し，推論時は MLP を捨てて，手前の CLS トークンを使用する．実験の結果，Unsupervised SimCSE では (3)が最も高い性能を示し，Supervised SimCSE では，これらの手法の違いはあまりなかった．平均プーリングもほぼ同等だった．これを受けて，Unsupervised SimCSE では(3)を，Supervised SimCSE では，(1) を使用した．

- BERT における各種手法による文埋め込みの Alignment と Uniformity をプロットした図．

  <p align="center">
      <img src="./../../assets/002_SimCSE/figure3.png" alt="figure3" width="50%">
  </p>

  - 2つの指標が良いほど，良い性能を達成することがわかり，これは Alignment/Uniformity を提案した元論文での見解と一致する．
  - 事前学習済みモデルにおける埋め込みを通すことで，Alignment が良いが，Uniformity は低いことがわかる．
  - BERT-flow や BERT-whitening などの後処理的手法は Uniformity を非常に改善するが，Alignment が大幅に低下した．

- Singular value distributions の図．
  Singular value distributions は埋め込みベクトルが空間内でどのように広がっているかの分布の形状を表す指標である．つまり，モデルが表現空間の次元をどれだけ有効活用できているかを示している．

  <p align="center">
      <img src="./../../assets/002_SimCSE/figureF1.png" alt="figureF1" width="50%">
  </p>

  - BBERT や SBERT は特異値がすぐに低下しており，異方性の問題を抱えていることを示している．
  - SimCSE は BERT に比べてなだらかになっている．対照学習によって，似ていない文同士を遠ざけて Uniformity を向上させた結果，空間を広く使えるようになったため．
  - BERT-flow や BERT-whitening はさらにフラットで良いが，（これらは強制的に分布を球状にする手法なので，無理やり形だけ整えるよりも学習過程で自然に広がりを持たせた SimCSE の方が意味的な構造とのバランスが良い，と主張している．）
