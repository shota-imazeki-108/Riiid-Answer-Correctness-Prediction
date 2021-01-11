# 1. My Solution
## Model
- LightGBM
    - parameterなどはLightGBM訓練用ノートブックを参照
    - [baseにしたノートブック](https://www.kaggle.com/zephyrwang666/riiid-lgbm-bagging2-1)
- Feature Engineering
    - 上記ノートブックをもとにcumsumなどで特徴量を作成
    - 講義の時間も考慮したlagtime: lagtime_in_lecture
    - 各ユーザーのその時点でのattempt_noの平均: attempt_no_mean
    - 各ユーザーの問題ごと(content_id)のlagtime: content_lagtime
    - 各ユーザーのpartごとのlagtime: part_lagtime
    - 各ユーザーのpartごとの正答率: user_part_correctness
    - 各ユーザーがパート別に講義を受けた回数: user_lecture_sum
    - 各ユーザーがパート別に講義を受けた回数をユーザーの全行countで割った値: user_lecture_lv
- AWS
    - LGBの場合、kaggle上ではメモリ不足(16GB)のため、EC2上にてtrain
    - [AWS Deep Learning AMI](https://docs.aws.amazon.com/ja_jp/dlami/latest/devguide/what-is-dlami.html)を使用
        - SAKTモデル(pytorchベース)もここで訓練させようと思っていた(kaggle上で全件で学習可能)
        - 簡単にjupyter notebookサーバーを立てられる
    - インスタンスタイプ: r5a.8xlarge
- SAKT
    - [こちらのノートブックがベース](https://www.kaggle.com/manikanthr5/riiid-sakt-model-training-public)
    - Attention layerを使用したNNモデル
    - 多くの時間を確保できず、こちらのモデルはほとんど改良を加えられていない
    - MAX_SEQの値を大きくした程度
    - LGBとのアンサンブルでスコアがo.oo5~7近く変わるため使用

# 2. Reflection
- Data Engineering
    - pandasではなくnumpyで特徴量作成(partition by user_id)
    - 
- Feature Engineering
    - 直近n個の正解率など
    - user_answerで同じ値が何回連続しているか
    - 他の回答者が選ばない(やばい)回答を選んだかどうか(パーセンタイル)
    - 直前の問題が正解しているかどうか
    - Question-Question matrix: 設問XにAと回答した人が設問YにてA, B, C, Dのいずれを回答する確率
    - qiestionごとの正答率などはリークの恐れあり(未来の情報を含んでいる)
    - lagは1, 2とかだけでなく9, 10でも効果があった
    - Order of task containers
    - word2vecによるコサイン類似度やクラスタリング
        - 各ユーザーごとにcontent_idを時系列順に並べる
        - その際、正解不正解を分ける(word2vecそのものを分ける or content_id_correct, content_id_incorrectのように1つのcontent_idを2パターンに分ける)
        - ユーザーの直近N問のvectorを取得し、平均化
        - その平均化したベクトルと次に解くcontent_idのcorrect, incorrectそれぞれのベクトルとコサイン類似度を算出し、それを特徴量にしている
        - 試しに実装してみた
            - [Riiid - word2vec with content_id](https://www.kaggle.com/imazekishota/riiid-word2vec-with-content-id)
            - [Riiid - LGBM with word2vec](https://www.kaggle.com/imazekishota/riiid-lgbm-with-word2vec)

# 3. Reference
- read csv
    - [大規模データセットの読み込み](https://www.kaggle.com/rohanrao/tutorial-on-reading-large-datasets)
- Feature Engineering
    - [Elo Rating](https://www.kaggle.com/stevemju/riiid-simple-elo-rating)
    - [TrueSkill](https://www.kaggle.com/neinun/understanding-microsoft-trueskill-with-riiid)



