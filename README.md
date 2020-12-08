# Graduation Task
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
  - 仮説
    - ![LGBMより重要度ランキング](https://github.com/s18013/Graduation-Task/blob/master/images/feature_importance.pdf)
    

