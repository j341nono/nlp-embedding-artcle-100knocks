### Simple Techniques for Enhancing Sentence Embeddings in Generative Language Models
([ICIC-2024](https://arxiv.org/abs/2404.03921))

- 異方性解消のための対照学習は，文ベクトルの計算に際して，モデルの崩壊を防ぎつつ十分な参照国モクの確保のために．大きなバッチサイズを必要とし，膨大な計算リソースを必要とする．(p2,1)
- EOLテンプレートを対照学習に取り組むここみがあるが，QLoRA による学習でも110M-scale BERT のフルファインチューニングよりも大きくの計算リソースを必要とする．(p2,1)
- PretendedCoT は Zero-shot CoT であり，モデルが中間推論ステップを出力する必要がないため，主にインスピレーションの役割を果たす．(p2,1)
- 教師ありFT としては，SBERT により重みを共有する2つのネットワーク構造に，2つの入力を通す構造の有効性を示し，SimCSE によって Anchor，Positive，Negative を1セットとして扱い，Anchor と Positive を近づけ，Anchor と Negative を遠ざける，という Triplet な学習構造の有効性を示した．その後，PromptEOL や AnglE，DeeLM がこれらの手法を生成PLMに応用したが，ラベル付されたデータセットが必要という点で，応用可能性に制限がかかる．
- Zero-shot CoT は中間的な推論ステップを引き出すのではなく，分の表現に細心の注意を払うことを目的に導入する．-> アテンションの可視化で違いを見たい．(p8,3.3)
- KE では，要約における人間の知見を直接生かしたもの．これも，アテンションの可視化で違いを見て，CoT Ver. と違いを分析したい．(p8,3.3)
- PCoTEOL はモデルサイズに関わらず安定した性能を発揮していた一方で，KE はサイズの小さい場合，PromptEOL よりも低い性能となった．これは，プロンプトが長く，難しい語彙を含んでいるため，モデルの理解能力が重要になるからである．(p12,4.3)
- sentence representation の分野では，alignment と uniformity の指標は埋め込みの品質を評価する重要な指標である．(p13,5.1)
- alignment と uniformity の指標は，STS-B のテストセットを使用した．uniformity は全てのデータを使用し，alignment は類似度が 4.5 の閾値を超えたサンプルを使用した． (p13,5.1)
- alignment と uniformity のいずれの指標においても，これらがよくなると，タスクのスコアも向上していた．
- "Distribution of each token’s contribution to sentence embeddings derived from PromptEOL and Knowledge Enhancement." として，各トークンの寄与率を見ている．文全体で100%となっていて，例文は "a man is driving a car" と短い．手法はアテンションの可視化ではなく，最終的な文埋め込みに対する各トークンの意味的寄与を余弦類似度を用いて分析した．MetaEOL にも似たような図があったので，それはどうやって算出したものなのかを見たい． (p15,5.2)


