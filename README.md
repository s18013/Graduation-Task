# Graduation Task
## Notice
  githuでjupyterが上手く動かないかもしれません
  お手数ですが clone して下さい(python file にvscode上でexportする機能がバグっているため用意出来ません)
  google drive 上にも置いておきますので、colaboratoryで開けるかと思います
## Team
  宮國龍之介
## Product
  kaggle Competition
    [Riiid! Answer Correctness Prediction](https://www.kaggle.com/c/riiid-test-answer-prediction)
  に参加して予測モデルを作る

## Schedule
### 2020 - 2021
- 10/08 ~ 10/11 **complet**
  - 環境構築(kaggleとcolaboratoryの連携, 自宅のデスクトップにデータセットののダウンロード)
- 10/12 ~ 10/18 **complet**
  - データセットの全体像把握(pythonで色々なグラフにしてみる、グラフ化のコードはgitに保存する)
  - 使えそうなモデルの選定(kaggle discussion, notebookからの情報収集 <-これは卒研中毎日)
- 10/19 ~ 10/25 **complet**
  - 予測モデルの構成の大まかな決定
  - ->transformersかnnかGBM
  - dataをモデルに流す
- 10/26 ~ 10/31 **complet**
  - 予備日
  - 上記の予定でモデルの選定に時間を取られる可能性が高いので、行き詰まり次第dataをどうゆう形でモデルに流すかを先に考える
- 11/1 ~ 11/7 **complet**
  - 10月時点の予定が滞りなかった場合は、パラメーターの調整・モデルのグラフ化(TensorBoardなどで)・モデルの見直し
  - この時点のkaggle Leaderboardで上位の予測率を目標に調整
- 11/8 ~ 01/7
  - ここから先は 11/1 ~ 11/7 の繰り返し、締め切りの 01/7 まで予測率を高められるまで高める
  - -> nnとGBMの複合モデルで予測していく
  - -- ここに研究内容を随時追記 --
- 01/8 ~ 01/14
  - プレゼンの作成(予測率を上げるためにしたことを生成してきたグラフを交えて)
- 01/15 ~ 発表日
  - プレゼンできてたら練習

## Research Record
### 2020 - 2021
- 12/5 ~ 12/6
  - Competitions 提出テスト(できるかどうかの確認のため、軽量版のmodelを使用　accuracy 0.628)
- 12/7 ~ 12/8
  - 結果(とりあえず作ったLGBMより)
    - ![LGBM(accuracy 0.720)より重要度ランキング](https://github.com/s18013/Graduation-Task/blob/master/images/feature_importance.pdf)
    - 上記の結果より
      - prior_question_elapsed_time(前の問題からの経過時間)の重要度が高い
      - question_asked(コンテンツごとの質問数)も高い
      - コンテンツごとの正解の平均、標準偏差、歪度の順で高い
  - 考察
    - 問題を解くまでの時間が予測の関係性をが高いのはなんとなく分かる、次点でのコンテンツごとの質問数重要度が高い理由がいまいち不明(下記よりコンテンツごとに高い相関関係があることと関係があるやも)
    - コンテンツごとの平均、標準偏差、歪度の重要度が僅差で高いことから何らかの相関関係があると思われる
    - 中央値の価値が低いのはそのほとんどが1.0であるためと思われる
    - ユーザーごとの平均、標準偏差、歪度も予測に使用しているがコンテンツごとのそれよりも高い成果を得られていない
    - prior_question_had_explanation(質問の後の講義を見たかどうか)はすごく高い訳ではないが数値が出ているので今後のデータのバリエーションを増やした時どうなるかで判断していきたい

  - 仮説
    - コンテンツごとの質問の数であったり平均、標準偏差、歪度が高いためユーザーごとよりも重要
    - 平均、標準偏差、歪度より正規分布のありようが重要
    - 問題を解くまでの時間はコンテンツごとユーザーごと関係なく重要

  - 次の実験内容
    1. LGBMに入れるデータのバリエーションを増やしてそのほかの重要な要素を探る(仮説よりコンテンツごとをメインにと、尖度なども探る)
    2. 重要度の高かった上位5件をNNに正解率との差分とともに食わせる
- 12/9 ~ 12/11
  - 結果(前日の実験内容の番号が結果の前についている)
    - <1>![LGBM(accuracy 0.720)より重要度ランキング](https://github.com/s18013/Graduation-Task/blob/master/images/feature_importance2.png)
    - 上記<1>より、
      - kurtosis(尖度)を追加しても、accuracy, feature_importanceともに変化なし
    - <2>array([[0.6271183],
       [0.6271183],
       [0.6271183],
       ...,
       [0.6271183],
       [0.6271183],
       [0.6271183]], dtype=float32)
    - <2>array([[0.6263924],
       [0.6263924],
       [0.6263924],
       ...,
       [0.6263924],
       [0.6263924],
       [0.6263924]], dtype=float32)
    - 上記<2>より
      - この結果は、nnを学習させ予測させたものです
      - 2パターン(5 epochs, 2 epochs 向上がまるで見られなかったため少な目のepochs)
      - いずれも予測値がすべて同じことからおそらく学習不足(dataバリエーション不足)
  - 考察
    - <1>よりdataのバリエーションをでたらめに増やしても成果が向上しない、もしくは増やした量がすくなすぎた
    - <1>より尖度は効果が薄い可能性がある
    - <2>よりもっとデータのバリエーションを増やしてから実験する必要がある
    - <2>より可能性は学習中上昇画見られないことから低いが、epochs不足も視野に入れる必要がある

  - 仮説
    - LGBMはとにかくバリエーション不足
    - NNも少ないバリエーションでは成果がでない

  - 次の実験内容
    - いっぱいバリエーション集めたいのでLGBMに絞って、ほかの参加者のnotebookも参考に集める

- 12/12 ~ 12/24
  - kaggle の他参加者の方法まとめ(LGBMの結果から重要度のたかいものを抜粋)
    - <1>![1](https://github.com/s18013/Graduation-Task/blob/master/images/FI_another_person.png)
    - <2>![2](https://github.com/s18013/Graduation-Task/blob/master/images/FI_another_person2.png)
    - <3>![3](https://github.com/s18013/Graduation-Task/blob/master/images/FI_another_person3.png)
    - <4>![4](https://github.com/s18013/Graduation-Task/blob/master/images/FI_another_person4.png)
    - <5>![5](https://github.com/s18013/Graduation-Task/blob/master/images/FI_another_person5.png)

  - 上記より
    - とりあえず各1位について
    - 1 content_correctness
      - contentごとにグループ化された、正解率
    - 2 answered_correctly_avg_c
      - contentごとにグループ化された、正解率の平均
    - 3 timestamp_u_recency
      - timestamp は、それまでのユーザーの総操作時間、uはユーザー
    - 4 content_correctness
      - 1と一緒
    - 5 tags1
      - question.csv の分類分けてのタグ

- 12/25 ~ 1/7
  - SAKT + LGBM が高いスコアを出してきたため、勉強します

- 1/8 ~ 1/30
  - 結果として、SAKTに乗り換える前にコンペの終了
  - 研究のまとめとプレゼン準備