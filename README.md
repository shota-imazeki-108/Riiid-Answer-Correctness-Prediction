# [Riiid-Answer-Correctness-Prediction](https://www.kaggle.com/c/riiid-test-answer-prediction/overview)
## Track knowledge states of 1M+ students in the wild
------
## これは何か
- Riiidにて使用した
- 自分のソリューションや反省点については、 `solution.md`に記載しております。
- ソロで挑んだり今回しか使用しないコードの都合上、オブジェクト指向とか全く意識していません(いずれ使いやすいようまとめる予定)。

## コンペ概要
- ユーザーの行動履歴(講義の受講履歴、問題への回答など)をもとに次に取り組む問題に正解できるか否かの二値分類問題
- ユーザーが解く問題はTOEIC形式
- クラス数1
- 特徴はとにかくデータが大きい(1億件、5.45GB)

## 使用したノートブック
- ./notebooks/train_model_with_part_lecture: LightGBM訓練用
- ./notebooks/riiid-self-attention-transformer.ipynb: SAKTモデル訓練用
- ./notebooks/inference-riiid-lgbm-bagging2-1-with-part.ipynb: 推論用
    - こちらのみkaggle上ではメモリ不足のためEC2でメモリ256GBのインスタンスを立てて実行しました。

## データ
- [こちらから](https://www.kaggle.com/c/riiid-test-answer-prediction/data)
- 規約に同意する必要あり

## 結果
- 182位
- 銅
- 170位が銀圏でスコア的に0.001も差がなかったので非常に悔しいがexpert昇格
